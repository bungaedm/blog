---
collapsible: false
date: "2022-01-05T10:09:56+09:00"
description: Javascript 기초
draft: false
title: Javascript 기초
weight: 1
---

본 포스팅은 [노마드 코더](https://nomadcoders.co/javascript-for-beginners/lectures)를 참고하여 공부하고 정리하였다.

## 0. 기본 설정
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <link rel="stylesheet" href="style.css">
    <title>Momentum</title>
</head>
<body>
    <div>
        <h1 class='hello'>Grab me!</h1>
    </div>
    <script src="app.js"></script>
</body>
</html>
```

## 1. list 만들기 & 원소 추가
```javascript
const daysOfWeek = ['mon', 'tue', 'wed', 'thu', 'fri', 'sat'];

// Get Item from Array
console.log(daysOfWeek);

// Add one more day to the array
daysOfWeek.push('sun');
console.log(daysOfWeek);
```

## 2. 원소 찾기
```javascript
const hellos = document.getElementsByClassName('hello');
console.log(hellos);

const title = document.querySelector('#hello h1'); // ID
const title = document.querySelector('.hello h1'); // class
console.log(title); 

const title = document.querySelector('div.hello:first-child h1');
console.dir(title);
title.style.color = 'blue';
```

- getElementsByClassName() : 많은 element를 가져올때 씀(array를 반환)
- getElementsByTagName() : name을 할당할 수 있음(array를 반환)
- querySelector : element를 CSS selector방식으로 검색할 수 있음 (ex. h1:first-child)
단 하나의 element를 return해줌
⇒ hello란 class 내부에 있는 h1을 가지고 올 수 있다(id도 가능함)
- 첫번째 element만 가져옴
- 조건에 맞는 세개 다 가져오고 싶으면 querySelectorAll 
⇒ 세개의 h1이 들어있는 array를 가져다 줌
- querySelector("#hello); 와 getElementById("hello"); 는 같은 일을 하는 것임
하지만 후자는 하위요소 가져오는 것을 못하므로 전자만 쓸거다
(출처: https://nomadcoders.co/javascript-for-beginners/lectures/2892)


## 3. 이벤트
```javascript
const h1 = document.querySelector('div.hello:first-child h1');

function handleTitleClick() {
    // console.log('title was clicked!');
    h1.style.color = 'blue';
}

function handleMouseEnter() {
    // console.log('mouse is here!');
    h1.innerText = 'Mouse is here!';
}

function handleMouseLeave() {
    // console.log('mouse is leaving');
    h1.innerText = 'Mouse is gone!';
}

function handleWindowResize() {
    document.body.style.backgroundColor = 'tomato';
}

function handleWindowCopy() {
    alert('copier!');
}

function handleWindowOffline() {
    alert('SOS no WIFI');
}

function handleWindowOnline() {
    alert('ALL GOOD');
}

h1.addEventListener('click', handleTitleClick);
h1.addEventListener('mouseenter', handleMouseEnter);
h1.addEventListener('mouseleave', handleMouseLeave);
// title.onclick = handleTitleClick;
// title.onmouseenter = handleMouseEnter;

window.addEventListener('resize', handleWindowResize);
window.addEventListener('copy', handleWindowCopy);
window.addEventListener('offline', handleWindowOffline);
window.addEventListener('Online', handleWindowOnline);
```

- 버튼이 아니어도, h1여도 클릭이 가능하다!

## 4. CSS in Javascript

```javascript
const h1 = document.querySelector('div.hello:first-child h1');

function handleTitleClick() {
    const currentColor = h1.style.color;
    let newColor;
    if(currentColor === 'blue'){
        newColor = 'tomato';
    } else {
        newColor = 'blue';
    }
    h1.style.color = newColor;
}

h1.addEventListener('click', handleTitleClick);
```

{{< codes javascript css >}}
  {{< code >}}
  ```javascript
    const h1 = document.querySelector('div.hello:first-child h1');
    
    function handleTitleClick() {
        if (h1.className === 'active') {
            h1.className = '';
        } else {
            h1.className = 'active';
        }
    
    }
    
    h1.addEventListener('click', handleTitleClick);
  ```
  {{< /code >}}
  {{< code >}}
  ```css
    body {
        background-color: beige;
    }
    
    h1 {
        color: cornflowerblue;
        transition: color .5s ease-in-out;
    }

    .active {
        color: tomato;
    }
  ```
  {{< /code >}}
{{< /codes >}}

```javascript
const h1 = document.querySelector('div.hello:first-child h1');

function handleTitleClick() {
    h1.classList.toggle('clicked') // 아래 코드와 같다.
    // const clickedClass = 'clicked';
    // if (h1.classList.contains(clickedClass)) {
    //     h1.classList.remove(clickedClass);
    // } else {
    //     h1.classList.add(clickedClass);
    // }
}

h1.addEventListener('click', handleTitleClick);
```

## 4-0. Input Values
{{< codes javascript html >}}
  {{< code >}}
  ```javascript
    const loginInput = document.querySelector('#login-form input');
    const loginButton = document.querySelector('#login-form button');
    
    function onLoginBtnClick() {
        console.log(loginInput.value);
    }
    
    loginButton.addEventListener('click', onLoginBtnClick);
  ```
  {{< /code >}}
  {{< code >}}
  ```html
    <body>
    <div id='login-form'>
        <input type='text' placeholder='What is your name?' />
        <button>Log In</button>
    </div>
    <script src="app.js"></script>
</body>
  ```
  {{< /code >}}
{{< /codes >}}

## 4-1. Form Submission

```html
<body>
    <form id='login-form'>
        <input required maxlength="15" type='text' placeholder='What is your name?' />
        <!-- <button>Log In</button> -->
        <input type='submit' value='Log In' />
    </form>
    <script src="app.js"></script>
</body>
```

- input의 유횽성을 검사하기 위해서는 input이 form 안에 있어야 한다.

## 4-2. Events

```javascript
const loginForm = document.querySelector('#login-form');
const loginInput = document.querySelector('#login-form input');

function onLoginSubmit(event) {
  event.preventDefault();
  console.log(loginInput.value);
}

loginForm.addEventListener('submit', onLoginSubmit);
```

```javascript
function onLoginSubmit(event) {
  event.preventDefault();
  const username = loginInput.value;
  loginForm.classList.add('hidden');
  console.log(username);
}
```

## 4-3. 사라지는 로그인 Form 만들기

```css
.hidden {
  display: none;
}
```

```javascript
const loginForm = document.querySelector('#login-form');
const loginInput = document.querySelector('#login-form input');

function onLoginSubmit(event) {
  event.preventDefault();
  const username = loginInput.value;
  loginForm.classList.add('hidden');
  console.log(username);
}

loginForm.addEventListener('submit', onLoginSubmit);
```