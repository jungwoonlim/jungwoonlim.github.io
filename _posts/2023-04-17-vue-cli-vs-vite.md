---
title: vue-cli가 아닌 vite를 선택한 이유
date: 2023-04-17 00:00:00 -0500
categories: ['Vue']
tags: ['Vue']
---

## vue-cli

`vue-cli`는 웹팩 기반의 빌드 툴입니다.
기존 웹팩과 같은 번들링 툴은 프로젝트가 커지면 서버의 실행 속도가 느려지는 문제가 존재합니다.
또한 HMR에 걸리는 시간도 애플리케이션이 커짐에 따라서 더 길어지게 됩니다.

## vite

`vite`는 자바스크립트 네이티브 모듈(ESM)을 기반으로 한 빌드 툴입니다.
ESM을 사용하여 필요한 모듈을 필요할때마다 불러오도록 해서 개발 서버의 실행 시작을 프로젝트의 크기와 관계없이 빠른 상태로 유지 시켜줍니다.
또한 esbuild를 사용하기 때문에 기존의 번들러(웹팩)보다 훨씬 빠른 속도를 보여줍니다.

## vue-cli가 아닌 vite의 사용 이유

1. 속도

* `vite`, `vue-cli`는 프로젝트의 크기에 따라서 개발 속도에 차이가 발생합니다.
* 프로젝트의 크기가 클 경우 `ESM` 기반의 `vite`가 `Webpack` 기반의 `vue-cli`보다 빠릅니다.
* 오더 통합 관리자 프로젝트는 다양한 기능이 들어갈 것이고 점점 크기가 커져갈 것입니다.
* 그러므로 오더 통합 관리자 프로젝트에는 `vite`를 기반으로 개발하는 것이 좋을 것 같습니다.

2. `vue-cli`의 공식 홈페이지에서도 현재 `vite`의 사용을 권장하고 있습니다.

> Vue CLI is in Maintenance Mode!
> 
> For new projects, please use create-vue to scaffold Vite-based projects. Also refer to the Vue 3 Tooling Guide for the latest recommendations.

### 웹팩 기반의 빌드 툴보다 ESM 기반의 빌드 툴이 빠른 이유

웹팩 기반의 툴은 Javascript에서 ESM이 등장하기 전까지 모듈화 방식이 없었습니다.
그래서 `const a = requrie('')` 같이 모듈 로더를 사용해야 모듈화가 가능했습니다.
이후 `import a from "b"`, `export const b` 와 같은 모듈화 문법이 Javascript에 추가되면서 로더를 거치지 않아도 되는 방법이 생겼고, 로더를 거치느냐 안거치느냐에 따른 속도 차이가 생긴겁니다.

### 참고 자료

* [Cracking Vue.js - vite란?](https://joshua1988.github.io/vue-camp/vite/intro.html)
* [vue-cli에서 vite로 옮긴 후기](https://www.cloudless.blog/post/vue-cli%EC%97%90%EC%84%9C-vite%EB%A1%9C-%EC%98%AE%EA%B8%B4-%ED%9B%84%EA%B8%B0)
* [Javascript Modeuls](https://developer.mozilla.org/ko/docs/Web/JavaScript/Guide/Modules)
* [vue-cli 공식 홈페이지](https://cli.vuejs.org/)
