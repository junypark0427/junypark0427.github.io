# 01. 웹사이트(Web Site)와 웹페이지(Web Page)

## 1. Web Site란?

**Web Site(웹사이트)**란 인터넷에 콘텐츠(Contents)가 게시되어 있는 장소로, 웹 브라우저(Web Browser)를 통해 접속할 수 있습니다.

- **Server(서버)**: 웹사이트의 파일과 내용을 보관합니다. 클라이언트가 요청하면 필요한 파일을 전송합니다.
- **Client(클라이언트)**: 웹사이트에 파일을 요청하고, 응답으로 전달받은 코드를 브라우저에서 실행하거나 표시합니다.

브라우저의 주소창에 **URL(Uniform Resource Locator)**을 입력하면 서버에 요청을 보냅니다. 서버는 HTML, CSS, JavaScript 등의 파일을 응답으로 보낼 수 있고, 브라우저는 이를 해석해 웹페이지를 보여 줍니다.

```text
Client ─────── Request ───────▶ Server
Client ◀────── Response ─────── Server
```

## 2. 웹페이지의 언어: HTML

**HTML(HyperText Markup Language)**은 브라우저가 읽는 웹 문서의 기본 형식입니다. 제목, 문단, 링크 등 페이지의 **구조와 내용**을 정의합니다.

```html
<!DOCTYPE html>
<html lang="ko">
  <head>
    <meta charset="UTF-8">
    <title>내 첫 페이지</title>
  </head>
  <body>
    <h1>안녕하세요</h1>
    <p>이게 web page입니다.</p>
  </body>
</html>
```

이 코드를 `index.html` 파일에 저장하고 브라우저로 열면 제목과 문단이 나타납니다.

## 3. 웹페이지의 디자인: CSS

**CSS(Cascading Style Sheets)**는 웹페이지의 **디자인**을 담당합니다. HTML만으로는 세밀한 디자인을 표현하기 어려우므로 CSS를 사용합니다.

- 글자색과 배경색
- 버튼의 모양
- 여백과 크기
- 화면의 레이아웃과 정렬

HTML 안에서는 `<style>` 태그를 이용해 CSS를 작성할 수 있으며, 별도의 `.css` 파일로 분리할 수도 있습니다.

## 4. 웹페이지의 동작: JavaScript

**JavaScript**는 웹페이지에 **상호작용과 동작**을 추가합니다.

예를 들어 버튼을 클릭하거나 특정 이벤트가 발생했을 때 실행할 기능을 정의할 수 있습니다. HTML 안에서는 `<script>` 태그를 이용해 작성할 수 있습니다.

## 5. 정리

| 기술 | 담당하는 역할 |
| --- | --- |
| HTML | 구조와 내용 |
| CSS | 디자인과 스타일 |
| JavaScript | 상호작용과 동작 |

지금까지 만든 HTML 파일은 내 컴퓨터(Local)에서도 확인할 수 있습니다. **다른 사람도 인터넷을 통해 접속**하도록 하려면 GitHub Pages 같은 호스팅 서비스를 이용해 배포해야 합니다.
