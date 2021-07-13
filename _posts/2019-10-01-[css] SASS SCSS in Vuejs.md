---
title: SASS SCSS in Vuejs
subtitle: Vuejs에서 SASS SCSS 이용하기
description: Vuejs에서 SASS SCSS 이용하기
date: 2019-10-01 23:30:09
layout: post
category: css
tags:
  - css
  - sass
  - scss
  - vuejs
paginate: false
author: kingsae
image: https://upload.wikimedia.org/wikipedia/commons/thumb/9/96/Sass_Logo_Color.svg/1280px-Sass_Logo_Color.svg.png
---

# 1. Introduction

SASS는 CSS Pre-processor로서 CSS한계와 단점을 보완하여 가독성이 높고 코드의 재사용에 유리한 CSS를 생성하기 위한 CSS확장이다
그간 CSS에서 태생적 한계가 아래 기능들에 의해 보완되었다.

- 변수의 사용
- 조건문과 반복문
- Import (기존CSS import와 다름)
- Nesting
- Mixin
- Extend/ Inheritance

# 2. Install

브라우저에서는 SASS의 문법을 바로 인지하지 못하기때문에 SASS/SCSS -> CSS 로 컴파일(트랜스파일)을 해주어야한다. 이는 노드환경/웹팩에서 가능하다.
보통은 sass-loader, node-sass를 이용하도록 되어있다. 하지만 node-sass의 경우 python에 의존성을 지니고 있기에 node-sass보다 기존 sass를 loaderOption에 포함해 사용하도록 하였다

```sh
$ npm i -D sass-loader
$ npm i -D sass
```

sass를 설치가 완료 되었다면, vue.config.js에 loaderOption을 이용해 vue 컴포넌트에서 sass/scss를 로드할 수 있도록 설정한다.

```javascript
module.exports = {
    ... ,
    css : {
        loaderOptions: {
            sass: {
                implementation: require('sass')
            }
        }
    }
}
```

# 3. SASS/SCSS 사용법

## 3.1 SASS/SCSS 차이점

SASS와 SCSS의 차이점은 중괄호 ‘{}’ 와 세미콜론 ‘;’ 의 유무이다. 따라서 기존 CSS처럼 사용하는 스타일을 따른다면 SCSS문법이 좀더 우리에게 친숙하다. (따라서 아래 SCSS문법으로 설명하도록 한다)

```css
SASS :
.list
    width: 100px
    float: left
    li
        color: red
        background: url("./image.jpg")
        &:last-child
        margin-right: -10px
```

```css
scss: .list {
  width: 100px;
  float: left;
  li {
    color: red;
    background: url("./image.jpg");
    &:last-child {
      margin-right: -10px;
    }
  }
}
```

## 3.2 SCSS 문법

### 3.2.1 주석

```console
// 컴파일되지 않는 주석
/* 컴파일되는 주석 */
```

### 3.2.2 데이터 타입

![](https://miro.medium.com/max/2400/1*2Yc4jNrDaVh1FBjfopMbxA.png)

### 3.3.3 중첩 (Nesting)

상위 선택자의 반복을 피하고 좀 더 편리하게 복잡한 구조를 작성할 수 있다.

```css
.section {
  width: 100%;
  .list {
    padding: 20px;
    li {
      float: left;
    }
  }
}
```

### 3.3.4 상위 선택자 참조 (&)

중첩 안에서 & 키워드는 상위(부모) 선택자를 참조하여 치환한다.

```css
.btn {
  position: absolute;
  &.active {
    color: red;
  }
}
.list {
  li {
    &:last-child {
      // .list li:last-child 동일하다
      margin-right: 0;
    }
  }
}
```

### 3.3.5 변수 ($)

반복적으로 사용되는 값을 변수로 지정할 수 있다. 변수 이름 앞에는 항상 $를 붙인다. Scope는 기본적으로 Block Scope를 따른다. 끝에’!global’ 을 붙이게 되면 변수의 유효 범위는 Global로 변경된다.

```css
$변수이름: 속성값;
```

### 3.3.6 문자보간 (#{})

```css
$family: unquote("Droid+Sans"); // 따옴표 제거 내장 함수
@import url("http://fonts.googleapis.com/css?family=#{$family}");
@import url("http://fonts.googleapis.com/css?family=Droid+Sans");
```

### 3.3.7 가져오기 (import)

@import로 외부에서 가져온 파일은 모두 단일 CSS 출력 파일로 병합된다. 또한, 가져온 파일에 정의된 모든 변수 또는 Mixins 등을 주 파일에서 사용할 수 있다.

```css
// main.scss
@import "header", "side-menu";
```

### 3.3.8 연산

- 산술 연산자:

![](https://miro.medium.com/max/451/1*UKyoiZyBEYVfSebBOeh8nA.png)

- 비교 연산자:
  ![](https://miro.medium.com/max/328/1*Q_KcQzdpvrfoCwmPAIqhxA.png)

- 불린 연산자:
  ![](https://miro.medium.com/max/157/1*ZDrzUQI-8U09kPhKSMWGPA.png)

```css
div {
  width: 20px + 20px; // 더하기
  height: 40px - 10px; // 빼기
  font-size: 10px * 2; // 곱하기
  margin: 30px / 2; // 나누기
}
```

### 3.3.9 Mixins

Sass Mixins는 스타일 시트 전체에서 재사용 할 CSS 선언 그룹 을 정의하는 아주 훌륭한 기능이다. 약간의 Mixin(믹스인)으로 다양한 스타일을 만들어낼 수 있다.

선언하기(@mixin)와 포함하기(@include)

```css
@mixin 믹스인이름 {
  스타일;
}
@include 믹스인이름;
```

### 3.3.10 확장 (Extend)

Extend는 중복되는 Selector에 의해 문제가 발생할 여지가 매우 높기때문에 가급적 사용을 권장하지않는다. 이에 사용법에 대해서는 Skip한다

### 3.3.11 함수 (‘@Function’)

```css
// Functions
@function 함수이름($매개변수) {
  @return 값;
}
```

### 3.3.12 조건(‘@if’)과 반복 (‘@for’)

if(조건, 표현식1, 표현식2)

```css
div {
  @if $color == strawberry {
    color: #fe2e2e;
  } @else if $color == orange {
    color: #fe9a2e;
  } @else if $color == banana {
    color: #ffff00;
  } @else {
    color: #2a1b0a;
  }
}

// 종료 만큼 반복
@for $변수 from 시작 through 종료 {
  // 반복 내용
}
// to
// 종료 직전까지 반복
@for $변수 from 시작 to 종료 {
  // 반복 내용
}
```

### 3.3.13 내장 함수

Sass Built-in Functions에서 모든 내장 함수를 확인할 수 있다.

#### 1) 색상(RGB / HSL / Opacity) 함수

- mix($color1, $color2) : 두 개의 색을 섞는다.
- lighten($color, $amount) : 더 밝은색을 만든다.
- darken($color, $amount) : 더 어두운색을 만든다.
- saturate($color, $amount) : 색상의 채도를 올립니다.
- desaturate($color, $amount) : 색상의 채도를 낮춘다.
- grayscale($color) : 색상을 회색으로 변환한다.
- invert($color) : 색상을 반전시킨다.
- rgba($color, $alpha) : 색상의 투명도를 변경한다.
- opacify($color, $amount) / fade-in($color, $amount) : 색상을 더 불투명하게 만든다.
- transparentize($color, $amount) / fade-out($color, $amount) : 색상을 더 투명하게 만든다.

#### 2) 문자(String) 함수

- unquote($string) : 문자에서 따옴표를 제거한다.
- quote($string) : 문자에 따옴표를 추가한다.
- str-insert($string, $insert, $index) : 문자의 index번째에 특정 문자를 삽입한다.
- str-index($string, $substring) : 문자에서 특정 문자의 첫 index를 반환한다.
- str-slice($string, $start-at, [$end-at]) : 문자에서 특정 문자(몇 번째 글자부터 몇 번째 글자까지)를 추출한다.
- to-upper-case($string) : 문자를 대문자를 변환한다.
- to-lower-case($string) : 문자를 소문자로 변환한다.

#### 3) 숫자(Number) 함수

- percentage($number) : 숫자(단위 무시)를 백분율로 변환한다.
- round($number) : 정수로 반올림한다.
- ceil($number) : 정수로 올림한다.
- floor($number) : 정수로 내림(버림)한다.
- abs($number) : 숫자의 절대 값을 반환한다.
- min($numbers…) : 숫자 중 최소 값을 찾는다.
- max($numbers…) : 숫자 중 최대 값을 찾는다.
- random() : 0 부터 1 사이의 난수를 반환한다.

#### 4) List 함수

    모든 List 내장 함수는 기존 List 데이터를 갱신하지 않고 새 List 데이터를 반환한다.
    모든 List 내장 함수는 Map 데이터에서도 사용할 수 있다.

- length($list) : List의 개수를 반환한다.
- nth($list, $n) : List에서 n번째 값을 반환한다.
- set-nth($list, $n, $value) : List에서 n번째 값을 다른 값으로 변경한다.
- join($list1, $list2, [$separator]) : 두 개의 List를 하나로 결합한다.
- zip($lists…) : 여러 List들을 하나의 다차원 List로 결합한다.
- index($list, $value) : List에서 특정 값의 index를 반환한다

#### 5) Map 함수

    모든 Map 내장 함수는 기존 Map 데이터를 갱신하지 않고 새 Map 데이터를 반환한다.

- map-get($map, $key) : Map에서 특정 key의 value를 반환한다.
- map-merge($map1, $map2) : 두 개의 Map을 병합하여 새로운 Map를 만든다.
- map-keys($map) : Map에서 모든 key를 List로 반환한다.
- map-values($map) : Map에서 모든 value를 List로 반환한다.

#### 6) 관리(Introspection) 함수

- variable-exists(name) : 변수가 현재 범위에 존재하는지 여부를 반환한다.(인수는 $없이 변수의 이름만 사용한다.)
- unit($number) : 숫자의 단위를 반환한다.
- unitless($number) : 숫자에 단위가 있는지 여부를 반환한다.
- comparable($number1, $number2) : 두 개의 숫자가 연산 가능한지 여부를 반환한다.
- unitless($number) : 숫자에 단위가 있는지 여부를 반환합니다.
- comparable($number1, $number2) : 두 개의 숫자가 연산 가능한지 여부를 반환합니다.

### 참고

> 1. https://sass-lang.com/documentation
> 2. https://www.sitepoint.com/sass-basics-operators/
> 3. https://sass-guidelin.es/ko/
> 4. https://thesassway.com/
