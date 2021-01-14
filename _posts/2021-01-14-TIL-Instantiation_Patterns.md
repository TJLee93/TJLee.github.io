---
title: "This is Instantiation Patterns"
date: 2021-01-14 23:52:33 -0400
categories: JavaScript codestates github Instantiation class
---
![instantiation patterns](https://user-images.githubusercontent.com/70124288/104607554-49ff9f00-56c4-11eb-800d-404096c40835.jpeg)

**1.Instantiation Patterns**
> 객체 지향 프로그래밍을 하는데 있어서 class는 필수적인 개념이다. 현재는 이 class를 만드는 법이 간편화되어 있지만 과거에는 class를 만들려면 직접 객체를 만들고 함수를 만들어서 조합하여 class를 생성해야했다. 오늘은 과거 class를 만드는 방법을 코드스테이츠 강의에서 사용한 코드를 가지고 정리해보려고 한다.  

1) Functional
> 함수를 이용하여 '찍어내는' 방식이다.
```js
let Car = function() {
  let someInstance = {};
  someInstance.position = 0; // position의 초기값을 0으로 고정
  someInstance.move = function() {
      this.position += 1;
  }
  return someInstance;
}; 
let car1 = Car(5); // car1은 Car(class)의 instance
```
or
```js
let Car = function(position) {
  let someInstance = {};
  someInstance.position = position;
  someInstance.move = function() {
      this.position += 1;
  }
  return someInstance;
} // 인자를 통해 position의 초기값을 설정
```

이렇게 변수에 함수를 할당하고 함수에 객체와 함수를 넣어줘서 객체를 리턴하는 함수를 생성하면 우리가 알고있던 class가 만들어진다. 하지만 이러한 방식은 instance를 생성할 때마다 모든 method를 변수(여기선 someInstace)에게 할당하므로, 각각의 instance들이 method의 수 만큼의 메모리를 더 차지한다. 즉, 용량이 커진다.
   
2) Functional Shared
> 각각의 역할이 정해진 함수들을 만들어놓고 그 함수들을 이용하여 instance를 생성하는 방식이다.
```
let extend = function(to, from) {
  for(let key in from) {
      to[key] = from[key];
  }
}; // 2개의 객체를 인자로 받고, 하나의 객체의 다른 객체의 key, value를 넣는다.

let someMethods = {};
someMethods.move = function () {
  this.position += 1;
}; // move라는 기능의 함수가 있는 객체

let Car = function(position) {
  let someInstance = {
      position: position, // value(position) === parameter(position);
  };
  extend(someInstance, someMethods); // extend 함수를 통해 someInstance에 someMethods의 기능을 넣어줌.
  return someInstance;
}; 
```
역할을 분리했기 때문에 변수가 많아지긴 했지만, 이 방식은 someMethods라는 객체에 있는 method들의 메모리 주소만을 참조하기 때문에 메모리 효율이 좋다. 또한, 사용하고 싶은 method가 많아져도 someMethods 변수 안에서 한번 다 만들어 놓으면 method를 다시 작성할 필요없이 원하는 때에 사용할 수 있기 때문에 복잡한 상황에서 과정이 더 수월해진다.
   
3) Prototypal
> 특정 개체를 프로토타입으로 하는 객체를 생성하고 활용하여 class를 생성하는 방식이다.
```
let someMethods = {};
someMethods.move = function () {
  this.position += 1;
}; // move라는 기능의 함수가 있는 객체

let Car = function(position) {
  let someInstance = Object.create(someMethods); // someInstance는 객체, someMethods는 someInstance의 프로토타입 객체
  someInstance.position = position; // === someInstance = {position: position(parameter)};
  return someInstance;
};
```
이 방식 또한 Functional Shared와 비슷하다.(다만 Functional Shared에 비해서 코드가 더 간결하다.) 기능들을 담고 있는 함수를 프로토타입 객체로 만들어 놓기 때문에 많은 기능들을 다뤄야할 때 한번만 작성해 놓으면 계속해서 간편하게 사용할 수 있다.
   
4) Pseudoclassical
> 객체의 프로토타입에 직접 함수를 추가하여 사용하는 방식이다.
```
let Car = function(position) {
  this.position = position;
};

Car.prototype.move = function() {
  this.position += 1;
};
```
이 방식은 가장 많이 쓰이는 방식이며 코드를 간편화하여 class를 생성한다. 하지만 만들어 놓은 객체의 프로토타입에 직접적으로 영향을 주는 것이기 때문에 잘못하면 객체 자체가 손상을 입을 수 있으므로 유의해야한다.   
   
솔직히 이미 더 쉽게 class를 만들수 있는 방법을 알고 있는 상황에서 이 방식들을 따라할 필요가 있는지는 의문이 들지만 이러한 방법들로도 class를 생성할 수 있고 class가 만들어지는 원리에 대해서 공부하는데는 많은 도움을 얻었다는 점에서는 좋은 공부가 되었다.   

*Reference*   
1.코드스테이츠 urclass Immersive Course - Lesson - OOP - Instantiation Patterns   
2.MDN(about Object.create): <https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Object/create>