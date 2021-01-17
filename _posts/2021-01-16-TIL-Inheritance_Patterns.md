---
title: "Inheritance에 대해 자세히 알아보자"
date: 2021-01-16 14:12:33 -0400
categories: JavaScript codestates github Prototype Inheritance ES6 
---
Inheritance(상속)란 상위 객체(parent)의 특징을 하위 객체(child)한테 넘겨주는 것이다. 그리고 하위 객체는 상위 객체로부터 물려받은 특징을 베이스로 해서 새로운 특징을 추가할 수 있다.

**1.prototype**
상속에 대해서 정확히 이해하기 위해선 prototype을 빼놓을 수 없다.   
prototype이란 어떤 object를 만들 때 쓰는 원형 객체(original form)을 말한다. (ex. Array.prototype : Array의 원형 객체, Object.prototype : Object의 원형 객체)   
이 prototype은 모든 함수(object)에 property로 들어가있는데 

/* prototype, constructor, ES6 Syntex, __proto__, Object.create 에 대해서 스스로 학습 후 이들 중 하나를 골라 TIL 작성 */