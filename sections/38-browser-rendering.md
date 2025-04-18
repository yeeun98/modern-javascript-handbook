# 브라우저 렌더링 과정 정리

## 1. 요청과 응답

브라우저는 필요한 리소스(HTML, CSS, JavaScript, 이미지, 폰트 등)를 서버에 요청하고, 응답받은 데이터를 시각적으로 렌더링합니다.  
브라우저 주소창에 URL을 입력하면, DNS를 통해 호스트 이름이 IP로 변환되고 해당 서버로 요청을 전송합니다. 개발자 도구의 Network 패널을 확인하면 HTML뿐 아니라 다양한 리소스들이 함께 로드되는 것을 볼 수 있습니다.  
브라우저는 HTML 파싱 도중 CSS, 이미지, JavaScript 등 외부 리소스를 만나면 해당 리소스를 먼저 요청하고 로딩 후 파싱을 이어갑니다.  

![11](https://github.com/user-attachments/assets/9b746df6-e2bb-4c98-ac62-353f389dd7dd)


## 2. HTTP 1.1과 HTTP 2.0
> HTTP는 웹에서 브라우저와 서버가 통신하기 위한 프로토콜이다.  

- **HTTP 1.1**: 한 커넥션당 하나의 요청/응답만 가능. 동시 전송 불가.
- **HTTP 2.0**: 커넥션당 다중 요청/응답 가능. 페이지 로딩 속도 향상.

## 3. HTML 파싱과 DOM 생성
> HTML 파일은 다음과 같은 과정을 거쳐 DOM으로 변환됩니다.

1. **바이트(Byte)**: 서버에서 받은 HTML은 바이트 형태입니다.
2. **문자(Character)**: 지정된 인코딩 방식(예: UTF-8)으로 문자열로 변환됩니다.
3. **토큰(Token)**: 문자열은 토큰으로 분해됩니다.
4. **노드(Node)**: 토큰은 노드 객체로 변환됩니다.
5. **DOM**: 생성된 노드들이 트리 형태로 구성됩니다.

## 4. CSS 파싱과 CSSOM 생성

DOM 생성 중 CSS를 만나면 파싱을 일시 중단하고 CSSOM 생성을 시작합니다.
CSSOM 또한 바이트 → 문자 → 토큰 → 노드 → CSSOM 과정을 거칩니다.

## 5. 렌더 트리 생성

DOM과 CSSOM은 결합되어 렌더 트리를 구성합니다.
렌더 트리는 화면에 표시될 노드만 포함합니다. (예: `display: none`, `script`, `meta` 제외)

## 6. 자바스크립트 파싱과 실행

- HTML 파싱 중 `<script>` 태그를 만나면 파싱을 일시 중단합니다.
- JavaScript 엔진이 코드를 파싱하여 AST를 만들고, 바이트코드로 변환하여 실행합니다.

## 7. 리플로우와 리페인트

> DOM/CSSOM이 변경되면 다시 렌더 트리를 만들고, 다음이 수행됩니다.

- **Reflow**: 레이아웃을 다시 계산
- **Repaint**: 시각적으로 다시 그리기

단순 스타일 변경은 Repaint만 발생합니다.

## 8. 자바스크립트 파싱에 의한 HTML 파싱 중단

script의 위치에 따라 HTML 파싱이 중단될 수 있으므로, 일반적으로 `<body>` 태그 하단에 script를 위치시킵니다.

## 9. `script` 태그의 `async`와 `defer`

```html
<script async src="extern.js"></script>
<script defer src="extern.js"></script>
```

- **async**
  - HTML 파싱과 병렬로 로드
  - 로드 완료 즉시 실행 → 파싱 중단 발생
  - 실행 순서 보장 ❌

- **defer**
  - HTML 파싱과 병렬로 로드
  - DOM 생성 완료 후 실행 → 파싱 중단 ❌
  - 실행 순서 보장 ⭕

---

📌 `defer`는 대부분의 경우에서 안정적이고 추천되는 방식입니다.
