---
layout: post
title:  "예제 코드와 함께 Codepen에 있는 콘텐츠를 Vue.js에 적용하는 방법 알아보기"
subtitle: "맥주 따르는 모션 넣기"
date:   2021-11-14 15:26:00
categories: [tools]
---

안녕하세요! 여러가지 멋진 모션을 웹페이지에 넣고 싶은데, 직접 모션효과를 주는 방법은 모르겠고!

이런 분들을 위해서 준비한 포스트입니다.

## 1. Codepen?

모션 소스 사이트 중 가장 눈에 띄고, 구글 검색하면 가장 먼저 뜨는 사이트 중에 하나가 바로 [Codepen](https://codepen.io/)이 아닐까싶습니다. 정말 개발자로써는 마성의 사이트인 것 같아요.
밤 10시에 들어갔다가 아이디어들이 너무 멋지고 신기해서 1시까지 계속 탐험(?)하게 되는 개미지옥같은 곳입니다!
예를들어 이런 효과들을 볼 수 있죠!

<p class="codepen" data-height="300" data-default-tab="html,result" data-slug-hash="QWMVgMj" data-user="thebabydino" style="height: 300px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;">
  <span>See the Pen <a href="https://codepen.io/thebabydino/pen/QWMVgMj">
  #codeVember #12/2021: bars animation</a> by Ana Tudor (<a href="https://codepen.io/thebabydino">@thebabydino</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://cpwebassets.codepen.io/assets/embed/ei.js"></script>

신기하죠?

다만 더 많은 정보를 얻으려면 계정을 구입해야합니다.  저는 무료 계정을 쓰고 있는 데 사용하기에 지장은 없는 것 같아요.

## 2. 넣고 싶은 소스 검색하기

로그인을 하시고 검색창에 원하시는 것을 찾으시면 됩니다.

![https://user-images.githubusercontent.com/48819383/141669232-56faad85-8440-471c-94c7-d7c9ff62df53.gif](https://user-images.githubusercontent.com/48819383/141669232-56faad85-8440-471c-94c7-d7c9ff62df53.gif)

저는 맥주잔을 채우는 이 화면을 구성해볼까합니다. 

[https://codepen.io/comehope/pen/rZeOQp](https://codepen.io/comehope/pen/rZeOQp)

## 3. Vue.js 프로젝트 생성하기

vue-cli로 vue.js project를 생성합니다.

```bash
npm install -g @vue/cli # 또는 yarn global add @vue/cli
vue create hello-vue3
# select vue 3 preset
```

( 더 자세한 내용은 [링크](https://v3.ko.vuejs.org/guide/migration/introduction.html)를 참고해주세요)

프로젝트 생성을 완료하시고, 프로젝트 폴더에 들어가셔서 한번 기본 프로젝트를 컴파일 해봅시다.

```bash
cd hello-vue3 && npm run serve
```

![컴파일 하시면 위 그림과 같이 로컬 호스팅을 해줍니다.](https://user-images.githubusercontent.com/48819383/141670146-e9518397-22b9-4597-a93c-afa36561ce84.png)

컴파일 하시면 위 그림과 같이 로컬 호스팅을 해줍니다.

Mac 기준 option버튼, Windows기준으로 ctrl버튼을 누른채로 링크를 클릭하셔서 Vue.js 기본 템플릿 사이트에 들어가 보겠습니다.

![https://user-images.githubusercontent.com/48819383/141669495-0051a56a-9333-4954-ba0b-7b3c1822617f.png](https://user-images.githubusercontent.com/48819383/141669495-0051a56a-9333-4954-ba0b-7b3c1822617f.png)

 잘 나왔죠? 그럼 다음 단계로 App.vue에 바로 Codepen 적용하러 가보시죠!

## 4. App.vue 수정하기

- 구현하고자하는 콘텐츠의 HTML 부분을 전체 복사하여 App.vue의 template 태그 하위에 붙여넣기
- 구현하고자하는 콘텐츠의 HTML 부분을 전체 복사하여 App.vue의 script 태그 하위에 붙여넣기
- 끝!

## 5. 결과 코드

```jsx
<template>
  <div class="container">
    <div class="keg">
      <span class="handle"></span>
      <span class="pipe"></span>
    </div>
    <div class="glass">
      <span class="beer"></span>
    </div>
  </div>
</template>

<style>
body {
    margin: 0;
    height: 100vh;
    display: flex;
    justify-content: center;
    background: linear-gradient(
        lightslategray 300px,
        #333 300px
    );
    overflow: hidden;
}

.container {
    width: 700px;
    height: 300px;
    position: relative;
}

.container *::before,
.container *::after {
    content: '';
    position: absolute;
}

.keg {
    position: absolute;
    width: 90px;
    height: 200px;
    background: linear-gradient(
        to right,
        #777 70px,
        #555 70px
    );
    bottom: 0;
    left: 310px;
}

.keg .pipe {
    position: absolute;
    width: 10px;
    height: 40px;
    background-color: #ccc;
    top: 33px;
    left: 10px;
}

.keg .pipe::before {
    width: 40px;
    height: 20px;
    background: 
        radial-gradient(
            circle at 10px 10px,
            #eee 7px,
            #ccc 7px, #ccc 10px,
            transparent 10px
        ),
        linear-gradient(
            #ccc 50%,
            #999 50%
        );
    border-radius: 10px;
    top: -2px;
    left: -5px;
}

.keg .pipe::after {
    width: 10px;
    background-color: rgba(255, 206, 84, 0.5);
    animation: flow 5s infinite;
}

@keyframes flow {
    0%, 15% {
        top: 40px;
        height: 0;
    }

    20% {
        height: 115px;
    }

    40% {
        height: 75px;
    }

    55% {
        top: 40px;
        height: 50px;
    }

    60%, 100% {
        top: 80px;
        height: 0;
    }
}

.keg .handle {
    position: absolute;
    border-style: solid;
    border-width: 50px 10px 0 10px;
    border-color: black transparent transparent transparent;
    top: -10px;
    left: 5px;
    transform-origin: center 50px;
    animation: handle 5s infinite;
}

@keyframes handle {
    10%, 60% {
        transform: rotate(0deg);
    }

    20%, 50% {
        transform: rotate(-90deg);
    }
}

.keg .handle::before {
    width: 20px;
    height: 10px;
    background-color: #ccc;
    top: -60px;
    left: -10px;
    border-radius: 5px 5px 0 0;
}

.keg .handle::after {
    width: 10px;
    height: 20px;
    background-color: #ccc;
    top: -20px;
    left: -5px;
}

.glass {
    position: absolute;
    width: 70px;
    height: 100px;
    color: rgba(255, 255, 255, 0.3);
    background-color: currentColor;
    bottom: 0;
    border-radius: 5px;
    animation: slide 5s ease infinite;
}

@keyframes slide {
    0% {
        left: 0;
        filter: opacity(0);
    }

    20%, 80% {
        left: 300px;
        filter: opacity(1);
    }

    100% {
        left: 600px;
        filter: opacity(0);
    }
}

.glass::before {
    width: 50px;
    height: 40px;
    border: 10px solid;
    top: 20px;
    right: -20px;
    border-radius: 0 40% 40% 0;
    clip-path: inset(0 0 0 72%);
}

.beer {
    position: absolute;
    width: 60px;
    background-color: rgba(255, 206, 84, 0.8);
    bottom: 15px;
    left: 5px;
    border-radius: 0 0 5px 5px;
    border-top: solid rgba(255, 206, 84, 0.8);
    animation: fillup 5s infinite;
}

@keyframes fillup {
    0%, 20% {
        height: 0px;
        border-width: 0px;
    }

    40% {
        height: 40px;
    }

    80%, 100% {
        height: 80px;
        border-width: 5px;
    }
}

.beer::before {
    width: inherit;
    background-color: #eee;
    border-radius: 5px 5px 0 0;
    animation: 
        wave 0.5s infinite alternate,
        fillup-foam 5s linear infinite;
}

@keyframes fillup-foam {
    0%, 20% {
        top: 0;
        height: 0;
    }

    60%, 100% {
        top: -15px;
        height: 15px;
    }
}

@keyframes wave {
    from {
        transform: skewY(-3deg);
    }

    to {
        transform: skewY(3deg);
    }
}

</style>
```

## 6. 마치며

생각보다 싱겁게 끝났네요ㅋㅋ 다음에는 더 좋은 콘텐츠로 뵙겠습니다!