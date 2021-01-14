---
title: "Welcome to My Blog!"
date: 2021-01-14 02:56:05 -0400
categories: jekyll update
---
이 블로그는 제가 자바스크립트를 배우면서 느낀점과 이해한 것들을 적어두는 공간입니다. 또한 저의 부족한 점을 알아가는 공간이기도 합니다.   
아직 더 배우고 있는 입장이기에 제가 잘못 작성하거나 잘못 이해한 부분에 관한 지적은 언제든 환영합니다.

```js
class JavaScript {
    constructor(name, negFeeling, posFeeling) {
        this.name = name;
        this.negFeeling = negFeeling;
        this.posFeeling = posFeeling;
    } 

    consoleIt() {
        console.log(`JavaScript를 배우면서 ${this.name}은 많은 ${this.negFeeling}을 느낀다. 하지만 해냈을때는 큰 ${this.posFeeling}을 느낀다.`);
    }
}

let me = new JavaScript("TJLee", "좌절", "성취감");
me.consoleIt();
````

*블로그 테마는 Jekyll 테마이며 그중에서 minimal-mistakes라는 테마를 사용하셨습니다.
   
1.Link: [theme][Jekyll-theme]   
[Jekyll-theme]: https://github.com/topics/jekyll-theme "Go Jekyll-theme"
   
2.Link: [mmistakes][mmistakes / minimal-mistake]   
[mmistakes / minimal-mistake]: https://github.com/mmistakes/minimal-mistake "Go mmistakes github"
