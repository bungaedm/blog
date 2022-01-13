---
collapsible: false
date: "2022-01-05T10:09:56+09:00"
description: Javascript 기초
draft: false
title: Javascript 기초
weight: 1
---

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
const title = document.querySelector('div.hello:first-child h1');

function handleTitleClick() {
    // console.log('title was clicked!');
    title.style.color = 'blue';
}

function handleMouseEnter() {
    // console.log('mouse is here!');
    title.innerText = 'Mouse is here!';
}

function handleMouseLeave() {
    // console.log('mouse is leaving');
    title.innerText = 'Mouse is gone!';
}

title.addEventListener('click', handleTitleClick);
title.addEventListener('mouseenter', handleMouseEnter);
title.addEventListener('mouseleave', handleMouseLeave);
```

- 버튼이 아니어도, h1여도 클릭이 가능하다!