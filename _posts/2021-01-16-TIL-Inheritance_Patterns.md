---
title: "Inheritance에 대해 자세히 알아보자"
date: 2021-01-16 14:12:33 -0400
update: 2021-01-19 23:46:49 -0400
categories: JavaScript codestates github Prototype Inheritance ES6 
---
(시간날때마다 추가하겠습니다... sprint 어렵네요 ㅜㅜ)

<img width="514" alt="스크린샷 2021-01-18 오후 5 30 27" src="https://user-images.githubusercontent.com/70124288/104912712-f8ab2480-59cf-11eb-99c4-c87fa2c11a2d.png">

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
    console.log(this.name + ' is sleeping...zzz');
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
constructor는 class내에서 객체를 생성하고 초기화하기 위한 method이며
(instance 생성시 초기화시키는 함수), class당 1개씩만 가지고 있는 특별한 method이다.   
```js
class Human {
  constructor(name) {
    this.name = name;
  }
  sleep() {
    console.log(this.name + ' is sleeping...zzz');
  }
}

let steve = new Human(‘steve’); // steve === Human의 instance

class Student extends Human { 
  constructor(name) {
    super(name); // parentClass에 childClass의 this를 바인딩 시키는 작업
  } // parent의 constructor와 child의 constructor가 동일하면 (구성이 동일하면) child의 constructor는 생략 가능하다.
  learn() {
    console.log('아직 배워야할게 많습니다.')
  };
} // Student === childClass, Human === parentClass
// super : 상속시 this를 전달하기 위해 사용하는 .call(), .apply()등 대신, 이 키워드 하나로 대체 가능. (부모 생성자 호출)
// extends : prototype이나 constructor를 연결시키는 작업을 해주는 키워드. (Student를 Human의 childClass로 만듬.)

let john = new Student(‘john’); // john === Student의 instance
john.learn(); // '아직 배워야할게 많습니다.'
john.sleep(); // john is sleeping….
```   
   

**3.__proto__**   
__proto__는 new 키워드를 통해 새로운 instance가 만들어졌을 때, 그 instance의 기능으로 추가된다.   
```js
function Human(name) {
  this.name = name;
}

let steve = new Human(‘steve’); // 변수 Steve는 함수 Human의 instance

steve.__proto__; // === Human.prototype 
```   
위의 코드와 같이 new로 steve라는 Human(class)의 instance가 생성이 되었고, steve에 __proto__라는 기능도 같이 추가가 되는데 이때 steve의 __proto__는 Human의 prototype을 가리킨다. 즉, __proto__는 자신보다 상위 객체의 prototype을 참조해 상위 객체의 기능을 가져와서 사용할 수 있게 한다.(상속)   
좀 더 자세히 알아보기 위해 다른 코드도 보도록 하자.   
```js
let Human = function(name) {
  this.name = name;
} // class 생성

Human.prototype.sleep = function() {}; // Human의 prototype에 sleep 기능 추가.

let steve = new Human(‘steve’); // steve는 Human의 instance

steve.toString(); // 결과물 : [Object Object]
// toString은 상속에서 상위 객체인 Object의 prototype에 있는 기능.
// Object.prototype에 있는 toString method를 가져와서 사용. (상속)
// steve.__proto__ === Human.prototype
// steve.__proto__.__proto__ === Object.prototype -> Object는 상속의 가장 상위 단계 (MDN Object)
```   
위의 코드를 통해 Human의 instance인 steve가 자신의 부모 객체인 Human의 부모 객체인 Object의 prototype을 참조하여 toString method를 사용하는 것을 알 수 있다.   
요약하자면 __proto__는 new 키워드를 통해 만들어진 자식 객체가 자신보다 상위의 객체의 prototype을 참조하는 것이고, 이를 통해 상위 객체의 method를 사용할 수 있게 된다.   
   

**4.Object.create(proto)**   
이 함수는 Object.prototype에 내장되어 있는 함수(기능)으로, 인자로 받은 prototype을 새 객체에 복사해서 할당한다(Array.slice 같은 기능이라고 생각하면 될 것 같다). 즉, 인자로 받은 prototype의 내장 기능들을 가지고 있지만 주소값은 다른 객체를 만들 때 사용한다.   
```js
let Human = function(name) {
  this.name = name;
};

Human.prototype.sleep = function() {
  console.log('zzz');
}

function Student() {
};

Student.prototype = Object.create(Human.prototype);
// Student.prototype.constructor = Student;
// Object.create시 constructor의 연결이 끊어지므로 다시 연결해줘야한다.

let student1 = new Student();

Student.prototype.learn = function() { 
    console.log(‘배우는중…’);
}

student1.learn(); // 배우는중…
student1.sleep(); // zzz

student1 instanceof Student; // true
student1 instanceof Human; // true
```   
다만 이때 주의해야할 점은 Object.create을 사용시, student1의 constructor가 Student와 연결이 끊어지게 된다. 직접 코딩을 해서 student1의 constructor를 찾으면 Human과 연결된 것을 알 수 있다.
그래서 제대로 constructor를 연결시켜주기 위해서는 위의 주석으로 달아둔 것 처럼 constructor를 Student에 연결시키는 작업을 해줘야 한다.
   
   
이렇게 Inheritance의 4가지 pattern에 대해서 정리를 해봤다. 솔직히 아직 부족한 점도 많고 완벽하게 이해했다고 생각이 되지는 않지만, 블로깅을 진행하면서 뭔가 잡스럽게 머릿속에 있던 것들이 조금은 정리가 된 느낌이라 다음에 봤을 때는 좀 더 쉽게 이해가 될 것 같다.