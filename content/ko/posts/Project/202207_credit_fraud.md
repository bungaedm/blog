---
date: "2022-08-12T10:08:56+09:00"
description: null
draft: false
title: 신용카드 사기거래 탐지
weight: 1
---

# 신용카드 사기거래 탐지 AI 경진대회
- 분석기간: 2022 July ~ 2022 October
- 본 대회는 `DACON`에서 매월 주최하는 경진대회 중 하나이다. 
- 본 대회의 핵심은 `Unsupervised Anomaly Detection`이었다. Validation Data에는 Class가 주어지기는 했지만, 단순통계정보 외에는 정보를 사용할 수는 없는 것이 규정이었다. Train Data에서 어떤 것이 사기거래 데이터인지 모르는 상태에서 찾아내는 것이 어려운 부분이었다.
- [대회 링크](https://dacon.io/competitions/official/235930/overview/description)

# ---

## 1. What I Have Learned
**1. 다양한 Anomaly Detection 방법**
[사이트](https://ffighting.tistory.com/entry/%EB%94%A5%EB%9F%AC%EB%8B%9D-Anomaly-Detection)에서 많은 부분을 참고하여 배울 수 있었다.

1. Reconstruction 방식
- AnoGAN
- GANomaly

2. One-Class Classification 
- OC-Deep-SVDD

3. Feature Matching

4. Probablistic (Normalizing Flow)

**2. 다양한 Unsupervised 방법**
unsupervised에서는 어떤 논리로 접근해야 하는지 공부를 할 수 있었다.

**3. AutoEncoder**
AutoEncoder를 처음부터 끝까지 구현하고, 때로는 Encoder 하나에 Decoder 두 개를 붙이는 등의 방식으로 응용할 수 있었다.

**4. Adversarial Training**
GAN의 아이디어를 AutoEncoder에 적용하여 시도해보았다.

## 2. Difficulty
**1. 정상데이터와 비슷한 이상치**
이상치인데도 불구하고 정상데이터와 특징이 비슷한 경우에는 AutoEncoder로 탐지하기가 어려웠다. (당연히 분포를 가정한 모수적 방법도 이를 구분해내기 어려웠다.) 그래서 GAN에서 Generator와 Discriminator가 적대적으로 학습하는 것에서 착안하여 Adversarial Training을 적용해보았다. 구체적인 구현은 아래 `Code 2`에 적어두었다.

**2. Hyperparameter에 민감하다**
Unsupervised Learning의 특성상, hyperparameter에 민감했다. 소수점 셋째자리의 변화에도 민감했다. 가령, AutoEncoder with Adversarial Training에서 코사인유사도를 통해 비교할 때, threshold을 0.995로 정했을 때와 0.996으로 정했을 때의 결과가 상이했다. 그리고 번외로 하나 더 이야기하자면, 이번 대회를 준비하면서 정말 다양한 모델들을 구현해보았는데 SEED에도 민감했다...

## 3. Models I Have Tried
- MCD
- AutoEncoder (reconstruction)
- AutoEncoder with Adversarial Training

# ---

## Code 1. MCD

Detecting outliers in a Gaussian distributed dataset using Minimum Covariance Determinant (MCD): robust estimator of covariance.

```python
from pyod.models.mcd import MCD 

clf = MCD(contamination = contamination_rate*1.2, store_precision=True, 
          assume_centered = False, support_fraction=0.999, random_state=1203)
clf.fit(train_df)
y_val_pred_mcd = clf.predict(val_df.drop(columns=['Class']).values)

print(classification_report(y_val_pred_mcd, val_df.Class))
```

**Macro F1-Score**
Validation Data: 0.916
Public Data: 0.93052 (공동 11위)

# ---

## Code 2. AutoEncoder with Adversarial Training

[USAD](/blog/220804_usad_multivariatetimeseries/) 논문의 아이디어를 참고하였다.

STEP1. Encoder 1개에 Decoder 2개가 붙어있는 구조를 만든다.
STEP2. Decoder 중 하나는 Generator, 나머지는 Discriminator의 역할을 하며 서로 적대적으로 학습한다.
STEP3. 재생성된 데이터와 원본데이터의 코사인유사도를 통해 1차적으로 이상데이터를 탐색한다.
- 기준: 0.95 (코사인유사도)
STEP4. Anomaly Score를 계산하여 한 번 더 이상치를 탐색한다.
- 기준1. 사기로 예측한 데이터의 Anomaly Score가 1 미만이면, 정상으로 바꾼다.
- 기준2. 정상으로 예측한 데이터의 Anomaly Score가 30 초과이면, 사기로 바꾼다.

### 1D AutoEncoder
```python 
class AutoEncoder(nn.Module):
    def __init__(self):
        super(AutoEncoder, self).__init__()
        self.Encoder = nn.Sequential(
            nn.Linear(30,64),
            nn.BatchNorm1d(64),
            nn.LeakyReLU(),
            nn.Linear(64,128),
            nn.BatchNorm1d(128),
            nn.LeakyReLU(),
        )

        self.Decoder1 = nn.Sequential(
            nn.Linear(128,64),
            nn.BatchNorm1d(64),
            nn.LeakyReLU(),
            nn.Linear(64,30),
        )
        
        self.Decoder2 = nn.Sequential(
            nn.Linear(128,64),
            nn.BatchNorm1d(64),
            nn.LeakyReLU(),
            nn.Linear(64,30),
        )
        
    def forward(self, x):
        x_enc = self.Encoder(x)
        x_dec1 = self.Decoder1(x_enc)
        x_dec2 = self.Decoder2(x_enc)
        return x_dec1, x_dec2
```

{{< expand "Training" >}}
### Training
```python
class Trainer():
    def __init__(self, model, optimizer, train_loader, val_loader, scheduler, device):
        self.encoder = model.module.Encoder
        self.decoder1 = model.module.Decoder1
        self.decoder2 = model.module.Decoder2
        self.optimizer_AE1 = optimizer
        self.optimizer_AE2 = optimizer
        self.train_loader = train_loader
        self.val_loader = val_loader
        self.scheduler = scheduler
        self.device = device
        # Loss Function
        self.criterion = nn.L1Loss().to(self.device)
        
    def fit(self, ):
        self.encoder.to(self.device)
        self.decoder1.to(self.device)
        self.decoder2.to(self.device)
        
        best_score = 0
        for epoch in range(EPOCHS):
            loss_AE1_list = []
            loss_AE2_list = []
            recon_loss_AE1_list = []
            recon_loss_AE2_list = []
            adv_loss_AE1_list = []
            adv_loss_AE2_list = []
            
            # Loss 가중치
            if epoch <= 100:
                w1=1 # Reconstruction Loss Weight
                w2=0 # Adversarial Loss Weight
            else:
                w2 = (epoch-100)/EPOCHS
                w1 = 1 -w2
    
            # Decoder1 학습 (Generator)
            for param in self.encoder.parameters():
                param.requires_grad = True
            for param in self.decoder1.parameters():
                param.requires_grad = True            
            for param in self.decoder2.parameters(): # Decoder2 파라미터 고정
                param.requires_grad = False
            
            for x in iter(self.train_loader):
                x = x.float().to(self.device)
                self.optimizer_AE1.zero_grad()
                
                # 1) Reconstruction Loss
                x_enc = self.encoder(x)
                x_dec1 = self.decoder1(x_enc)
                reconstruction_loss_AE1 = self.criterion(x, x_dec1)
                
                # 2) Adversarial Loss
                x_dec1_enc = self.encoder(x_dec1)
                x_dec1_dec2 = self.decoder2(x_dec1_enc)
                adversarial_loss_AE1 = self.criterion(x, x_dec1_dec2)                
                
                loss_AE1 = w1*reconstruction_loss_AE1 + w2*adversarial_loss_AE1
                loss_AE1.backward()

                self.optimizer_AE1.step()
                loss_AE1_list.append(loss_AE1.item())
                recon_loss_AE1_list.append(reconstruction_loss_AE1.item())
                adv_loss_AE1_list.append(adversarial_loss_AE1.item())
              
            # Decoder 2 학습 (Discriminator)
            for param in self.decoder1.parameters(): # Decoder1 파라미터 고정
                param.requires_grad = False
            for param in self.decoder2.parameters():
                param.requires_grad = True

            for x in iter(self.train_loader):
                x = x.float().to(self.device)
                self.optimizer_AE2.zero_grad()
                                
                # 1) Reconstruction Loss
                x_enc = self.encoder(x)
                x_dec2 = self.decoder2(x_enc)
                reconstruction_loss_AE2 = self.criterion(x, x_dec2)
                
                # 2) Adversarial Loss
                x_dec1 = self.decoder1(x_enc)
                x_dec1_enc = self.encoder(x_dec1)
                x_dec1_dec2 = self.decoder2(x_dec1_enc)
                adversarial_loss_AE2 = self.criterion(x, x_dec1_dec2)
                
                loss_AE2 = w1*reconstruction_loss_AE2 - w2*adversarial_loss_AE2
                loss_AE2.backward()

                self.optimizer_AE2.step()
                loss_AE2_list.append(loss_AE2.item())            
                recon_loss_AE2_list.append(reconstruction_loss_AE2.item())
                adv_loss_AE2_list.append(adversarial_loss_AE2.item())
                
            score = self.validation(self.decoder2, 0.95, 1)
            recon_loss_AE1 = np.sum(recon_loss_AE1_list).round(4)
            recon_loss_AE2 = np.sum(recon_loss_AE2_list).round(4)
            adv_loss_AE1 = np.sum(adv_loss_AE1_list).round(4)
            adv_loss_AE2 = np.sum(adv_loss_AE2_list).round(4)

            print(f'Epoch : [{epoch}] Val Score : [{score}] Loss1 : [{np.mean(loss_AE1_list).round(10)}] Loss2 : [{np.mean(loss_AE2_list).round(10)}] reconAE1:[{recon_loss_AE1}] reconAE2:[{recon_loss_AE2}] advAE1:[{adv_loss_AE1}] advAE2:[{adv_loss_AE2}]')

            if self.scheduler is not None:
                self.scheduler.step(score)

            if best_score <= score:
                best_score = score
                torch.save(model.module.state_dict(), '../model/best_model_autoencoder_linear_GAN5.pth', _use_new_zipfile_serialization=False)
#                 torch.save(model.module.state_dict(), '../content/gdrive/MyDrive/_credit/model/best_model_autoencoder_linear_GAN5.pth', _use_new_zipfile_serialization=False)
                
                 
    
    def validation(self, eval_model, thr1, thr2):
        cos = nn.CosineSimilarity(dim=1, eps=1e-6)
        eval_model.eval()
        pred = []
        true = []
        with torch.no_grad():
            for x, y in iter(self.val_loader):
                x = x.float().to(self.device)

                x_enc = self.encoder(x)
                x_dec = self.decoder1(x_enc)
                
                x_dec_enc = self.encoder(x_dec)
                x_dec_dec = self.decoder2(x_dec_enc)
                
                diff1 = cos(x, x_dec).cpu()
                diff2 = cos(x_enc, x_dec_enc).cpu()
                diff_mask = np.logical_and(diff1<thr1, diff2<thr2)
                
                batch_pred = np.where(diff_mask, 1,0).tolist()
                pred += batch_pred
                true += y.tolist()

        return f1_score(true, pred, average='macro')
        
model = nn.DataParallel(AutoEncoder())
model.eval()
optimizer = torch.optim.Adam(params = model.parameters(), lr = LR)
scheduler = torch.optim.lr_scheduler.ReduceLROnPlateau(optimizer, mode='max', factor=0.5, patience=30, threshold_mode='abs', min_lr=1e-8, verbose=True)        

trainer = Trainer(model, optimizer, train_loader, val_loader, scheduler, device)
trainer.fit()
```
{{< /expand >}}

{{< expand "Prediction" >}}
### Prediction
```python
model = AutoEncoder()
model.load_state_dict(torch.load('../content/gdrive/MyDrive/_credit/model/best_model_autoencoder_linear_GAN5.pth'))
# model.load_state_dict(torch.load('../model/best_model_autoencoder_linear_GAN5.pth'))
model = nn.DataParallel(model)
model.eval()

def prediction(model, thr1, thr2, test_loader, device):
    model.to(device)
    model.eval()
    cos = nn.CosineSimilarity(dim=1, eps=1e-6)
    pred = []
    with torch.no_grad():
        for x in iter(test_loader):
            x = x.float().to(device)
            
            x_enc, x_dec = model(x)
            x_dec_enc, x_dec_dec = model(x_dec)

            diff1 = cos(x, x_dec).cpu()
            diff2 = cos(x_enc, x_dec_enc).cpu()
            diff_mask = np.logical_and(diff1<thr1, diff2<thr2)

            batch_pred = np.where(diff_mask, 1,0).tolist()
            pred += batch_pred
    return pred
    
preds = prediction(model, 0.95, 1, test_loader, device)    
```
{{< /expand >}}

### Anomaly Score
```python
def calculate_anomaly_score(x, alpha=0.1, model=model, validation=False, device=device):
    anomaly_score = []
    loss_func = nn.MSELoss().to(device) ## MSE Loss
    beta = 1-alpha
    
    if validation:
        xx = x[0].to(device)
    else:
        xx = x.to(device)
    enc = model.module.Encoder(xx)
    dec1 = model.module.Decoder1(enc)

    dec1_enc = model.module.Encoder(dec1)
    dec1_dec2 = model.module.Decoder2(dec1_enc)

    for i in range(len(xx)):
        score = alpha * loss_func(xx[i], dec1[i]) + beta *loss_func(xx[i], dec1_dec2[i])
        anomaly_score.append(score.item())
    return anomaly_score

anomaly_score_val = []
for x in iter(val_loader):
    anomaly_score = calculate_anomaly_score(x, validation=True)
    anomaly_score_val += anomaly_score    
    
    
## Test Data
anomaly_score_test = []
for x in iter(test_loader):
    anomaly_score = calculate_anomaly_score(x)
    anomaly_score_test += anomaly_score
    
test_df_anom = test_df.copy()
test_df_anom['pred'] = preds # 예측값
test_df_anom['score'] = pd.Series(anomaly_score_test) # anomaly score

# # Anomaly Score 
# 기준1. 사기 예측 => 정상 예측 / below 1
# anomaly score가 1보다 작은 사람은 정상으로 분류하기
test_df_anom.loc[test_df_anom['score']<1, 'pred'] = 0

# 기준2. 정상 예측 => 사기 예측 / over 30
# # anomaly score가 30보다 큰 사람은 사기로 분류하기
test_df_anom.loc[test_df_anom['score']>=30, 'pred'] = 1
```

**Macro F1-Score**
Validation Data: 0.923
Public Data: 0.93052 (공동 11위)

# ---

## Reference
[1] [딥러닝 Anomaly Detection의 모든 것](https://ffighting.tistory.com/entry/%EB%94%A5%EB%9F%AC%EB%8B%9D-Anomaly-Detection)


<br>
<br>