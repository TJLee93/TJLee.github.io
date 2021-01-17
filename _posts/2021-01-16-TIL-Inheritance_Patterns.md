---
title: "Inheritance에 대해 자세히 알아보자"
date: 2021-01-16 14:12:33 -0400
update: 2021-01-17 19:00:00 -0400
categories: JavaScript codestates github Prototype Inheritance ES6 
---
(시간날때마다 추가하겠습니다... sprint 어렵네요 ㅜㅜ)

Inheritance(상속)란 상위 객체(parent)의 특징을 하위 객체(child)한테 넘겨주는 것이다. 그리고 하위 객체는 상위 객체로부터 물려받은 특징을 베이스로 해서 새로운 특징을 추가할 수 있다. 상속이 어떤 원리로 이루어지고 어떤 개념인지 좀더 구체적으로 알기 위해서는 다른 개념들도 같이 알아둬야 한다. 그래서 오늘은 다른 개념들도 정리하면서 상속에 관해 알아보려 한다.   

**1.prototype**   
상속에 대해서 정확히 이해하기 위해선 prototype을 빼놓을 수 없다.   
prototype은 모든 함수(object)에 property로 들어가있는데,  prototype이란 어떤 object를 만들 때 쓰는 원형 객체(original form)을 말한다. (ex. Array.prototype : Array의 원형 객체, Object.prototype : Object의 원형 객체)   
<img width="1010" alt="prototype 예시1" src="https://user-images.githubusercontent.com/70124288/104836835-16a35700-58f4-11eb-8ebe-76236575e7e5.png">   
그림과 같이 Object와 Array의 prototype에는 무수히 많은 property들이 담겨있는 것을 볼 수 있는데, 이는 과거 JavaScript를 사용하던 선배들이 객체 지향 프로그래밍을 하기 위해서 하나하나씩 만들고 추가하신 것이다. 이렇게 우리가 코플릿을 풀면서 사용했던 method들은 다 prototype에 담겨있던 method였기에 사용이 가능했던 것이다.   
만약 우리가 만든 새로운 객체에서 원하는 property를 사용할 수 있게 하고 싶으면 추가하려는 property를 새로운 객체의 prototype에 추가시키면 된다. 그렇게 하면 어느때든 property를 사용할 수 있기 때문에 유용하다.   
```js
let Human = function(name) {
    this.name = name;
} // Human이라는 class를 생성
Human.prototype.sleep = function() {
    console.log(this.name + ' is sleeping...zzz);
} // Human class의 prototype에 sleep(property)이라는 함수(기능)를 넣어줌.
// 원하는 때에 Human의 instance는 sleep 함수를 사용할 수 있다.
   
let human = new Human("Kim"); // human은 Human class의 instance
   
let myObj = function() {}; // myObj라는 class를 생성
   
myObj.prototype = Object.create(String.prototype); // String의 prototype에 있는 property들을 복사해서 myObj의 prototype에 넣는다.(Array.slice랑 비슷하다고 생각했다.) 
// -> 갖고있는 properties는 같지만, 주소값이 서로 다르다.(Object.create을 통해 prototype을 '상속'받는다.)
   
// myObj.prototype = String.prototype; // 같은 property를 공유하고 있으며 주소값도 같다.(myObj.prototype === String.prototype)
   
myObj.prototype.sayHello = function() {
    console.log("Hello");
} // myObj의 prototype에 sayHello(property)라는 함수(기능)을 넣어준다.
// 원하는 때에 myObj의 instance는 sayHello 함수를 사용할 수 있다.
   
let mine = new myObj(); // mine은 myObj의 instance
   
mine.sayHello(); // "Hello"   
mine.sleep("Lee"); // "Lee is sleeping...zzz"   
// mine은 myObj의 instance지만 myObj가 Object.create을 통해 Human으로부터 
// prototype의 property들을 상속 받았기 때문에 Human의 기능도 사용할 수 있다.   
human.sayHello(); // TypeError: human.sayHello is not a function   
human.sleep(); // "Kim is sleeping...zzz"
```
   
**2.constructor**
constructor는 class내에서 객체를 생성하고 초기화하기 위한 method이다.

/* prototype, constructor, ES6 Syntex, __proto__, Object.create 에 대해서 스스로 학습 후 이들 중 하나를 골라 TIL 작성 */