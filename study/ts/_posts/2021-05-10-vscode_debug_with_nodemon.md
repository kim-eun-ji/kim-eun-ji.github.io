---
layout: post
title: vscode 디버깅 with nodemon
categories: [study, ts]
tags: [study, ts]
image: /assets/img/study/ts/vscode_debug_with_nodemon/logo.png
description: >
  vscode에서 nodemon으로 실행중인 프로세스(로컬서버)를 디버깅하기 위한 설정을 한다.
invert_sidebar: false
---

# vscode 디버깅 with nodemon

nodemon으로 로컬 서버를 돌리면서 브레이크 포인트를 찍으면 바로바로 확인 가능하도록 설정해본다.

## before launch.json

---

기본적으로 node로 디버깅을 한다고 선택하면 생성되는 `launch.json` 을 조금 수정해줄 것이다.

```json
{
  "version": "0.2.0",
  "configurations": [
    {
      "type": "node",
      "request": "launch",
      "name": "Launch Program",
      "skipFiles": ["<node_internals>/**"],
      "program": "${workspaceFolder}\\src\\App.ts",
      "preLaunchTask": "tsc: build - tsconfig.json",
      "outFiles": ["${workspaceFolder}/**/*.js"]
    }
  ]
}
```

## 프로세스에 연결

---

1. Add Configuration 선택

![Add Configuration 선택](/assets/img/study/ts/vscode_debug_with_nodemon/Untitled.png)

2. Attach to Process 선택

![Attach to Process 선택](/assets/img/study/ts/vscode_debug_with_nodemon/Untitled%201.png)

3. `nodemon` 으로 서버 실행

4. 브레이크 포인트 찍기

5. 하단 사진의 1번모양을 선택, (디버깅 네임은 2에서 선택하여 생성된 구성의 `name` 항목과 일치하는 것으로.)

6. 3개중 메인에 해당하는 것(내경우엔 src/App.ts)을 선택하여 실행(보통 첫번째에 위치한다.)

![하단 사진의 1번모양을 선택](/assets/img/study/ts/vscode_debug_with_nodemon/Untitled%202.png)

## After launch.json

---

```json
{
  "version": "0.2.0",
  "configurations": [
    // 추가된 부분 start
    {
      "name": "프로세서에 연결", // 이름은 본인 취향껏 바꾸기
      "processId": "${command:PickProcess}",
      "request": "attach",
      "skipFiles": ["<node_internals>/**"],
      "type": "pwa-node"
    },
    // 추가된 부분 end
    {
      "type": "node",
      "request": "launch",
      "name": "Launch Program",
      "skipFiles": ["<node_internals>/**"],
      "program": "${workspaceFolder}\\src\\App.ts",
      "preLaunchTask": "tsc: build - tsconfig.json",
      "outFiles": ["${workspaceFolder}/**/*.js"]
    }
  ]
}
```

---

{:style="margin-top:5rem"}

- 🔗 참고 URL
  - [https://nerv2000.tistory.com/105](https://nerv2000.tistory.com/105)

{:style="margin-top:5rem"}

---

<script src="https://utteranc.es/client.js" repo="kim-eun-ji/blog-comments" issue-term="pathname" theme="github-light" crossorigin="anonymous" async></script>
