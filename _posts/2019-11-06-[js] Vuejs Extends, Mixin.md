---
title: Vuejs Extends, Mixin?
subtitle: Vuejs에서 Extends, Mixin 은 무엇인가?
description: Vuejs에서 Extends, Mixin 은 무엇인가?
date: 2019-11-06 23:30:09
layout: post
category: js
tags:
  - vuejs
  - extends
  - mixin
paginate: false
author: kingsae
image: https://vuejs.org/images/logo.svg
---

최근에는 Atomic 구조로 설계되는 프레임워크들이 많이 발생하여, 작은 컴포넌트 형태로 잘게 잘라 개발하다보니 최근에는 중복되는 컴포넌트 형태에 대해서 확장해주는 기능들이 존재한다. 이번에는 Vue에서 사용되는 방법에 대해 알아보자.
흔히 개발 Language에서는 extends함수들을 이용해서 특정 함수 혹은 클래스들을 상속받아 확장시킨다. Vuejs에서도 마찬가지다. Extends를 이용해서 중복되는 컴포넌트를 쉽게 확장 가능하다

## 1. Extends

가장먼저 Base가 되는 컴포넌트를 생성한다

```vue
<template>
  <div class="base_component">
    <h4>{{ name }}</h4>
  </div>
</template>

<script>
export default {
  data() {
    return { name: "Base" };
  },
  mounted() {
    console.log("My name is " + this.name);
  },
};
</script>
```

다음은 확장할 컴포넌트를 만들어보자

```vue
<script>
import BaseComp from "./BaseComp.vue";
export default {
  extends: BaseComp,
  data() {
    return { name: "Sub" };
  },
  mounted() {
    console.log("My name is " + this.name);
    console.log("Welcome!!!!");
  },
};
</script>
```

위처럼 정의를 하게되면 BaseComp에서 생성된 Data의 Name은 Sub로 Overwrite하고 새로운 컴포넌트로 역할을 하게 된다.
Function과 같은경우는 Extend 되는 부모 Function이 호출되고나서 확장하는 Function이 호출된다. (부모>자식)
그와는 달리 methods, components, directive (template) 은 오버라이딩 되기 때문에 사용할때 주의하여 사용하자.

## 2. Mixin

가장먼저 Base가 되는 컴포넌트를 생성한다

```vue
<template>
  <div class="base_component">
    <h4>{{ name }}</h4>
  </div>
</template>
<script>
export default {
  data() {
    return { name: "Base" };
  },
  mounted() {
    console.log("My name is " + this.name);
  },
};
</script>
```

다음은 Mixin을 이용해 확장 컴포넌트를 만들어보자

```vue
<script>
import BaseComp from "./BaseComp.vue";
export default {
  mixins: [BaseComp],
  data() {
    return { name: "Sub" };
  },
  mounted() {
    console.log("My name is " + this.name);
    console.log("Welcome!!!!");
  },
};
</script>
```

Mixins는 Array형태로 여러부모를 상속받을 수가 있다. (중복 Method가 있다면 마지막에 선언된 부모 Method로 Overwrite한다)
사용방법을 보면 거의 Extends와 Mixin의 차이는 느끼지 못한다. 하지만상속 여부에 따라 다르다

- 다중 상속 가능 여부
- mixins는 다중 상속이 가능(array로 컴포넌트를 연결),
- extends는 단일 상속만 됨(extends 프로퍼티에 컴포넌트가 객체로 들어감).

사실 상속의 개념은 동일하기 때문에 어떤 방식을 이용해도 문제 없다. 다만 mixins은 다중 부모를 상속받을수가 있기 때문에 사용방법이 동일하다면 extends보다는 Mixins을 사용하는게 바람직하지 않을 까 생각된다.
