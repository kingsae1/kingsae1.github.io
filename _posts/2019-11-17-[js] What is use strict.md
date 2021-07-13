---
title: ‘use strict’은 무엇인가?
subtitle: js에서 ‘use strict’ 은 무엇인가?
description: js에서 ‘use strict’ 은 무엇인가?
date: 2019-11-17 23:30:09
layout: post
category: js
tags:
  - js
  - use strict
paginate: false
author: kingsae
image: https://2.bp.blogspot.com/-dms_wK2RddE/We72Lrxgo_I/AAAAAAAACQ4/CAmEFV-v3k8ceIFDuGyoEseYBXpjQqkxACLcBGAs/s1600/maxresdefault.jpg
---

Strict Mode는 ECMA5 문법부터 사용되었고, ‘use strict’를 js 코드내에 포함시키게 되면 해당 라인 아래부터 해당 문법이 적용된다

<em> 그렇다면 use strict가 뭘까???</em>

strict의 어원적 의미로 보게 되더라도 엄격한 상태를 만들 것이라는 걸 눈치 챌수가 있다. 실제로는 브라우저가 js를 로드하면서 해석하게 되는데, use strict모드가 되면 “해당 코드들을 좀더 엄격하게 문법검사를 하겠다” 정도로 이해하면 될것 같다

## Strict 모드

### 1.변수선언

SCRIPT5042: Strict 모드에서 변수가 정의되지 않음
testvar = 4;

### 2. 읽기 전용 속성

SCRIPT5045: Strict 모드에서는 읽기 전용 속성에 할당할 수 없음

```js
var testObj = Object.defineProperties(
  {},
  {
    prop1: {
      value: 10,
      writable: false, // by default
    },
    prop2: {
      get: function () {},
    },
  }
);
testObj.prop1 = 20;
testObj.prop2 = 30;
```

### 3. 확장할 수 없는 속성

extensible 특성이 false로 설정된 개체에 속성을 추가
SCRIPT5046: 확장할 수 없는 개체의 속성을 만들 수 없음

```js
var testObj = new Object();
Object.preventExtensions(testObj);
testObj.name = "Bob";
```

### 4. delete

변수, 함수 또는 인수를 삭제합니다. configurable 특성이 false로 설정된 속성을 삭제
SCRIPT1045: strict 모드에서는 <식>에 대해 delete를 호출할 수 없음

```js
var testvar = 15;

function testFunc() {};

delete testvar;
delete testFunc;

Object.defineProperty(testObj, "testvar", {
  value: 10,
  configurable: false
});
delete testObj.testvar;
```

### 5. 속성 중복

개체 리터럴에서 속성을 두 번 이상 정의
SCRIPT1046: strict 모드에서는 한 속성을 여러 번 정의할 수 없다

```js
var testObj = {
  prop1: 10,
  prop2: 15,
  prop1: 20,
};
```

### 6.매개 변수 이름 중복

함수에서 매개 변수 이름을 두 번 이상 사용
SCRIPT1038: strict 모드에서는 정식 매개 변수 이름이 중복될 수 없다

```js
function testFunc(param1, param1) {
  return 1;
}
```

### 7. 예약된 키워드

예약된 키워드를 변수 또는 함수 이름으로 사용
SCRIPT1050: 식별자에 대해 다음에 사용하기 위한 예약어를 잘못 사용. strict 모드에서는 식별자 이름이 예약

- implements
- interface
- package
- private
- protected
- public
- static
- yield

### 8. 8진수

숫자 리터럴에 8진수 값을 할당하거나 8진수 값에 이스케이프를 사용
SCRIPT1039: strict 모드에서는 8진수 숫자 리터럴 및 이스케이프 문자를 사용할 수 없다

```js
var testoctal = 010;
var testescape = \010; 8. this
```

this의 값이 null또는 undefined인 경우 전역 개체로 변환되지 않는다

```js
function testFunc() {
  return this;
}
var testvar = testFunc();
```

strict 모드가 아닌 경우 testvar 값은 전역 개체이지만 strict 모드에서 이 값은 undefined입니다.

### 9. 식별자인 eval

문자열 “eval”은 식별자(변수 또는 함수 이름, 매개 변수 이름 등)로 사용할 수 없습니다.

```js
var eval = 10;
```

### 10. 문 또는 블록 내에서 선언된 함수

문 또는 블록 내에서 함수를 선언할 수 없습니다.
SCRIPT1047: strict 모드에서는 함수 선언을 문이나 블록 내에 중첩할 수 없다.함수 선언은 함수 본문 내에 직접 나오거나 최상위 수준에만 나와야 한다.

```js
var arr = [1, 2, 3, 4, 5];
var index = null;
for (index in arr) {
  function myFunc() {}
}
```

### 11. eval 함수 내에서 선언된 변수

변수가 eval 함수 내에서 선언되는 경우 해당 함수 밖에서 사용할 수 없다.
SCRIPT1041: strict 모드에서 ‘eval’을 잘못 사용

```js
eval("var testvar = 10");
testvar = 15;
```

간접 계산이 가능하지만 eval 함수 외부에 선언된 변수는 여전히 사용할 수 없다.
이 코드는 “SCRIPT5009: ‘testVar’이 정의되지 않았습니다” 오류 발생

```js
var indirectEval = eval;
indirectEval("var testvar = 10;");
document.write(testVar);
```

### 12. 식별자인 Arguments

문자열 “arguments”는 식별자(변수 또는 함수 이름, 매개 변수 이름 등)로 사용할 수 없다.
SCRIPT1042: strict 모드에서 ‘arguments’를 잘못 사용
var arguments = 10; 13. 함수 내 arguments
로컬 arguments개체의 멤버 값을 변경할 수 없다.

```js
function testArgs(oneArg) {
arguments[0] = 20;
}
function (testInt) {
if (testInt-- == 0)
return;
arguments.callee(testInt--);
}
```

### 14. WITH

허용되지 않습니다.
SCRIPT1037: strict 모드에서는 ‘with’ 문을 사용할 수 없다.

```js
with (Math) {
  x = cos(3);
  y = tan(7);
}
```

추가적으로 ES6에서는 디폴트가 strict모드이기 때문에, 굳이 사용할 필요가 없다

### 참고

> https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Strict_mode
