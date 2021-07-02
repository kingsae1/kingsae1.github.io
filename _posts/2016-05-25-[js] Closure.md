---
title: JS Closure
subtitle: Javascript 클로저에 대한 개념 정리
description: Javascript 클로저에 대한 개념 정리
date: 2016-05-25 23:30:09
layout: post
categories:
  - javascript
tags:
  - javascript
  - closure
author: kingsae
paginate: true
image: https://res.cloudinary.com/practicaldev/image/fetch/s--LGEPLGl5--/c_imagga_scale,f_auto,fl_progressive,h_420,q_auto,w_1000/https://dev-to-uploads.s3.amazonaws.com/i/tesu953czx4ww9q3z1s3.jpg
optimized_image: https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcRz9rPrd_mUldml_Wbesd55ke2by0ZeacDrCA&usqp=CAU
---

# Closure

아래와 같이 MDN에서 정의한 모호한 내용의 클로저를 이해하기 위해서는 선행되어야할 두가지 존재한다. 첫번째는 스코프, 두번째는 함수 내 함수 리턴이다. 이 두가지가 클로저의 주요 매커니즘이다.
자바스크립트는 기본적으로 함수가 선언되는 시점에서 스코프가 결정된다.

> MDN에서는 “클로저를 함수와 함수가 선언된 어휘적 환경의 조합”이라고 정의 하고있다.굉장히 모호한 표현이다.

```javascript
function outer() {
  var name = "kingsae";
  console.log(name);
}

outer();
```

위와 같은 코드가 있다고 생각해보자. outer를 실행하면 어렵지 않게 console에 name은 kingsae가 나오게 될 것을 예측 했을 것이다.

> 변수에는 전역변수 (global variable), 지역변수 (local variable)이 존재한다. 전역변수는 브라우저가 Document를 생성하는 가장 최상위 (window)에 선언된 변수이며, 지역변수는 함수가 선언될때 함수 단위로 변수가 선언 되는 것을 지역 변수라고 한다.

위와 같이 결과가 발생하는 원인에 대해 생각해보면, 함수가 선언 될때 함수내에 있는 변수들 (지역변수)들은 함수에서 외부함수에 변수의 범위로 점차 올라가며 최종적으로 전역변수까지 찾는데 (스코프 체인), outer내에 name이 선언 되어있기 때문에 kingsae를 나타내는 것이다.

```javascript
function outer() {
  var name = "kingsae";

  function inner() {
    console.log(name);
  }
}

outer();
```

위처럼 사용하게 되면, inner 내부에 함수에서 name이라는 지역변수를 찾아보고, 없기에 상위 outer함수내에 name을 찾아 사용한다. 위와 같은 예제들은 함수에서 참조가 끝나게 되면 메모리 Garbage collection에 등록되고 메모리 해제가 일어난다. 그러나 다음과 같은 예제를 살펴보자.

```javascript
function outer() {
  var name = "kingsae";

  function inner() {
    console.log(name);
  }

  return inner;
}

var closure = outer();
closure();
```

함수내에서 함수가 리턴되며 이를 closure라는 변수에 담아 호출하게 된다. 이렇게 되면 outer함수내에서 inner 함수가 name을 계속해서 참조하게 되는데, 변수에 해당 함수가 저장 되면서 함수내부 스코프를 계속해서 참조해서 사용가능하다.

쉽게 말해 클로저는 함수내에서 함수를 리턴할때 내부에 참조되는 변수가 해제되지 못하는 상태를 갖게 될때 이를 클로저라고 생각하면 된다.

아무래도 참조되는 변수에 의해 메모리 해제가 되지 않는 현상이 발생하기 때문에 알고사용해야한다. 다음번에는 클로저를 이용해 함수선언하는 방법에 대해 알아보도록 하겠다.
