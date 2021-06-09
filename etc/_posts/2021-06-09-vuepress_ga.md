---
layout: post
title: vuepress 블로그에 Google Analytics 붙이기 (GA4 기준)
categories: [etc]
tags: [etc]
image: /assets/img/etc/ga/logo.png
description: >
  [vuePress] 블로그에 Google Analytics 붙이기
invert_sidebar: false
---

# vuepress 블로그에 Google Analytics 붙이기 (GA4 기준)

> 💡 vuePress 블로그, ga4 기준으로 작성되었습니다.

## 신규 추적 ID 발급 시 @vuepress/plugin-google-analytics 의 사용

---

[문서](https://v1.vuepress.vuejs.org/plugin/official/plugin-google-analytics.html#install){:target="\_blank"}를 확인하면, 다음과 같이 사용하라고 되어있다. 하지만 `UA-XXXXXXXXXX` 형식의 측정ID는 더이상 신규로 발급받을 수 없다.

```jsx
module.exports = {
  plugins: [
    [
      "@vuepress/google-analytics",
      {
        ga: "", // UA-00000000-0
      },
    ],
  ],
};
```

그럼 어떻게 설정을 해야할까? 2가지 방법이 있다. 그리고, 2번째 방법을 추천한다.

### 1. @vuepress/plugin-google-analytics@next

---

해당 방법은 베타 버전이고 확실하게 작동한다는 보장이 없어 시도해보지 않았다.

- v1 과 호환되지 않음 (v2에서 지원)
- [공식 문서](https://vuepress2.netlify.app/reference/plugin/google-analytics.html#reporting-events){:target="\_blank"}

### 2. 직접 config.js에 설정하세요

---

1. `.vuepress/config.js` 파일을 오픈
2. `head` 영역에 다음과 같이 입력합니다.

   ```jsx
   module.exports = {
   	... 생략 ...
   	head: [
   	    [
   	      "script",
   	      {
   	        async: true,
   	        src: "https://www.googletagmanager.com/gtag/js?id=G-XXXXXXXXXX",
   	      },
   	    ],
   	    ["script", {}, ["window.dataLayer = window.dataLayer || []; function gtag(){dataLayer.push(arguments);} gtag('js', new Date()); gtag('config', 'G-XXXXXXXXXX');"]],
   	  ],
   	... 생략 ...
   }
   ```

3. `G-XXXXXXXXXX` 부분을 본인의 GA 측정 ID로 변경합니다.
4. 설정되었는지 확인하기 위해 배포 후 `F12 - 개발자 도구` 를 열어 콘솔에 `gtag`를 검색합니다.  
    다음과 같이 함수 정의가 뜨면 성공 한 것.(설정 실패 시 에러가 뜸)

   ```jsx
   input  > gtag
   output > ƒ gtag(){dataLayer.push(arguments);} // 함수 정의가 나오면 성공
   ```

---

{:style="margin-top:2rem"}

- 🔗 참고 URL
  - [https://github.com/vuejs/vuepress/issues/2713](https://github.com/vuejs/vuepress/issues/2713)

{:style="margin-top:2rem"}

---
