---
collapsible: false
date: "2022-07-28T10:08:56+09:00"
description: RSR AutoEncoder
draft: false
title: RSR AutoEncoder
weight: 1
---

# RSR AutoEncoder
**Robust Space Recovery AutoEncoder**
Lai, C. H., Zou, D., & Lerman, G. (2019). `Robust subspace recovery layer for unsupervised anomaly detection`. arXiv preprint arXiv:1904.00152.

## In Short
1. AutoEncoder를 unsupervised anomaly detection에 사용한 모델
1. Encoder와 Decoder 사이에 RSR layer를 추가한 모델
1. Reconstruction loss 외에 RSR loss를 추가한 모델

## 1. Introduction
{{< notice info "Main Idea" >}}
It maps one representation (embedding obtained from the encoder) into another **low-dimensional representation that is outlier-robust.**

Let’s say that D (upper case D) is the dimension of the embedding from the encoder. The assumption in the paper is that the “normal” data lies within d-dimensional (lower case d) manifold (“subspace”) of the original embedding, which means that d < D.
{{< /notice >}}

## 2. RSR Layer
```python
class RSRLayer(nn.Module):
    def __init__(self, d:int, D: int):
        super().__init__()
        self.d = d
        self.D = D
        self.A = nn.Parameter(torch.nn.init.orthogonal_(torch.empty(d, D)))

    def forward(self, z):
        # z is the output from the encoder
        z_hat = self.A @ z.view(z.size(0), self.D, 1)
        return z_hat.squeeze(2)
```

{{< expand "RSR Auto Encoder in PyTorch" >}}
```python
class RSRAutoEncoder(nn.Module):
    def __init__(self, input_dim, d, D):
        super().__init__()
        # Put your encoder network here, remember about the output D-dimension
        self.encoder = nn.Sequential(
          nn.Linear(input_dim, input_dim // 2),
          nn.LeakyReLU(),
          nn.Linear(input_dim // 2, input_dim // 4),
          nn.LeakyReLU(),
          nn.Linear(input_dim // 4, D)
        )

        self.rsr = RSRLayer(d, D)

        # Put your decoder network here, rembember about the input d-dimension
        self.decoder = nn.Sequential(
          nn.Linear(d, D),
          nn.LeakyReLU(),
          nn.Linear(D, input_dim // 2),
          nn.LeakyReLU(),
          nn.Linear(input_dim // 2, input_dim)
        )
    
    def forward(self, x):
        enc = self.encoder(x) # obtain the embedding from the encoder
        latent = self.rsr(enc) # RSR manifold
        dec = self.decoder(latent) # obtain the representation in the input space
        return enc, dec, latent, self.rsr.A
```
{{< /expand >}}

위 코드에서 확인할 수 있는 것처럼, A라는 행렬을 곱해주는게 포인트이다.

참고로 여기서 RSR Layer에서 D가 input dimension이고, d가 output dimension이다.

## 3. Loss

$$
L_{RSRAE}(Enc,A,Dec) = L^p_{AE}(Enc, A, Dec) + L^q_{RSR}(A)
$$

### 3-1. Reconstruction Loss

$$
L^p_{AE}(Enc, A, Dec) = \sum^N_{t=1} \bigg|\bigg| \bf{x}^{(t)} - \tilde{\bf{x}}^{(t)} \bigg|\bigg|^p_2
$$

### 3-2. RSR Loss
$$
`\begin{align}
L^q_{RSR}(A) &= \lambda_1 L_{RSR_1}(A) + \lambda_2 L_{RSR_2}(A) \\
:&= \lambda_1 \sum^N_{t=1} \Bigg|\Bigg|z^{(t)} - A^T \underbrace{Az^{(t)}}_{\tilde{z}^{(t)}}\Bigg|\Bigg|^q_2 + \lambda_2\bigg|\bigg|AA^T-I_d\bigg|\bigg|^2_F
\end{align}`
$$

크게 두 개의 Loss로 이루어져있다. 하나는 Reconstruction loss이고, 나머지 하나는 RSR loss이다. 각각은 위와 같이 정의된다.

RSR loss 중에서 첫번째는 얼마나 RSR layer를 실제 latent vector와 최대한 비슷하게 하기 위한 것이다. 두 번째는 프로젝션을 최대한 orthogonal하게 만들기 위함이다.

{{< expand "영어 원문" >}}
The first term enforces the RSR Layer projection to be robust and the second term enforces the projection to be orthogonal.
{{< /expand >}}

{{< expand "RSRAutoencoder module in PyTorch Lightning" >}}
```python
class RSRAE(pl.LightningModule):
    def __init__(self, hparams):
        super().__init__()
        self.hparams = hparams
        self.ae = RSRAutoEncoder(
            self.hparams.input_dim, 
            self.hparams.d, 
            self.hparams.D)
        self.reconstruction_loss = L2p_Loss(p=1.0)
        self.rsr_loss = RSRLoss(self.hparams.lambda1, self.hparams.lambda2, self.hparams.d, self.hparams.D)
  
    def forward(self, x):
        return self.ae(x)
  
    def training_step(self, batch, batch_idx):
        X, _ = batch
        x = X.view(X.size(0), -1)
        enc, dec, latent, A = self.ae(x)

        rec_loss = self.reconstruction_loss(torch.sigmoid(dec), x)
        rsr_loss = self.rsr_loss(enc, A)
        loss = rec_loss + rsr_loss
        
        # log some usefull stuff
        self.log("reconstruction_loss", rec_loss.item(), on_step=True, on_epoch=False, prog_bar=True)
        self.log("rsr_loss", rsr_loss.item(), on_step=True, on_epoch=False, prog_bar=True)
        return {"loss": loss}

    def configure_optimizers(self):
        opt = torch.optim.AdamW(self.parameters(), lr=self.hparams.lr)
        # Fast.AI's best practices :)
        scheduler = torch.optim.lr_scheduler.OneCycleLR(opt, max_lr=self.hparams.lr, 
                                                        epochs=self.hparams.epochs, 
                                                        steps_per_epoch=self.hparams.steps_per_epoch)
        return [opt], [{
            "scheduler": scheduler,
            "interval": "step"
        }]
        
dl = DataLoader(ds, batch_size=64, shuffle=True, drop_last=True)

hparams = dict(
    d=16,
    D=128,
    input_dim=28*28,
    # Peak learning rate
    lr=0.01,
    # Configuration for the OneCycleLR scheduler
    epochs=150,
    steps_per_epoch=len(dl),
    # lambda coefficients from RSR Loss
    lambda1=1.0,
    lambda2=1.0,
)

model = RSRAE(hparams)
model
```
{{< /expand >}}

## 4. Experiment Result

### 4-1. Dataset
1. Caltech 101
1. Fashion-Mnist
1. Tiny Imagenet
1. Reuters-21578
1. 20 Newsgroups

### 4-2. Measure
AUC (area under curve) and AP (average precision) scores

# ---

## Critical Point (MY OWN OPINION)
1. semi-supervised이면서 unsupervised라고 속이는 다른 모델에 비해, 이 모델은 진짜로 unsupervised이다.
1. RSR loss와 Reconstruction loss의 가중치도 조절해야하지 않을까?
1. semi-supervised나 supervised에 비해서는 (당연히) 성능이 떨어지지만, 이것에 대해서 솔직하게 밝히고 있다는 점도 좋았다.
1. PyTorch로 간단하게 구현된 코드도 공개되어 있어서 좋았다.

# ---

## Reference
[1] https://zablo.net/blog/post/robust-space-recovery-rsrlayer-pytorch/
