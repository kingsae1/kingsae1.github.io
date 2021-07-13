---
date: 2016-05-24 23:30:09
title: Element Event Delegation
subtitle: Element간 이벤트 전달 패턴에 대한 설명
description: Element간 이벤트 전달 패턴에 대한 설명
layout: post
category: javascript
tags:
  - javascript
  - event
  - delegation
  - capture
  - bubble
image: >-
  https://miro.medium.com/max/1120/1*93dh-xp6TodXWp4RIj2rtw.jpeg
author: kingsae
paginate: false
---

# Event Delegation

Event Delegation은 하나의 HTML페이지에서 다수의 이벤트를 할당해야하는 구조에서 매우 유용한 패턴이다.
간략하게 말하면 다수의 DOM에 이벤트를 부여하는 방식이 아닌 부모 DOM에 이벤트를 부여하여 처리하는 방식이다.
동작원리는 이벤트가 버블링에 의해 Child DOM -> Parent DOM으로 이벤트가 전달되는 방식을 이용한다.
일단 위 방식을 이해하기 위해서는 2가지의 이벤트 전달방식에 대해 알아야한다.

## Event Capture

이벤트 캡쳐는 이벤트 버블링과 정반대 되는 개념으로써 window 객체-> 이벤트가 발생한 DOM으로 탐색하는 방식이다

```html
<body>
  <div class="one">
    <div class="two">
      <div class="three"></div>
    </div>
  </div>
</body>
```

```js
var dom = document.querySelectorAll("div");
dom.forEach(function (dom) {
  dom.addEventListener("click", callback, {
    capture: true, // default 값은 false
  });
});

function dom(event) {
  console.log(event.currentTarget.className);
}
```

위와 같이 이벤트를 생성하고 three div를 클릭했을 경우 어떤 이벤트가 발생할까?

답은 Parent DOM부터 one two three 클래스가 로그에 남게 된다. 이유는 Parent DOM -> Target DOM까지 탐색하기 때문이다.

## Event Bubbling

이벤트 버블링은 이벤트가 발생한 DOM -> window 객체로 이벤트를 전달하는 방식이다

```js
var dom = document.querySelectorAll("div");
dom.forEach(function (dom) {
  dom.addEventListener("click", callback);
});

function callback(event) {
  console.log(event.currentTarget.className);
}
```

이벤트를 위와같이 달았다고 생각해보자. 그렇다면 Event Capture와는 다르게 Event Bubbling은 어떤식으로 로그를 남기게 될까?

답은 Target DOM부터 three two one이 로그에 남게 된다. 이유는 Target DOM -> Parent DOM까지 이벤트를 전달하기 때문이다.

위 두가지 방식을 가지고 Event Delegation 패턴을 이용하게 된다. 상위 DOM에 이벤트를 붙이고나서, callback으로 넘어 오는 event의 target을 가지고 적절하게 컨트롤하게 되면 각각의 DOM에 이벤트를 주입하지 않고도 이벤트를 관리 할 수 있게 된다.

DOM에 이벤트를 많이 달수록 성능에 이슈가 올 수 있기 때문에 복잡해지는 WebPage의 경우에는 위 Event Delegation 패턴을 이용하는 것이 바람직해 보인다. 다만 Body 태그와 같이 최상위 DOM에 이벤트를 걸게 되면 Event Capture하는 데에 너무 많은 Cost가 들기 때문에 적절한 Parent DOM을 선정하는 것 또한 성능에 도움이 될 것이다
