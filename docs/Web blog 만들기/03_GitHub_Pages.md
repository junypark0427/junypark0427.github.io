# 03. GitHub Pages로 HTML 웹페이지 공개하기

이 장에서는 **HTML 파일을 직접 업로드하는 방식**으로 웹페이지를 공개합니다. 뒤에서 다룰 MkDocs 자동 배포와는 별개의 실습입니다.

## 0. GitHub 가입

[GitHub](https://github.com/)에 가입하고 로그인합니다.

## 1. Repository 생성

1. 오른쪽 위 **+** 버튼에서 **New repository**를 클릭합니다.
2. **Repository name**: `calculator` 입력
3. **Description**: 간단한 설명 입력
4. **Public / Private**: `Public` 선택
5. **Add a README file**: 체크하지 않음
6. **Create repository** 클릭

저장소가 만들어지면 처음에는 파일이 없는 빈 페이지가 나타납니다.

## 2. 파일 업로드(GitHub 웹에서 직접)

1. 저장소 화면에서 **uploading an existing file** 링크를 클릭합니다. 이미 파일이 있다면 **Add file → Upload files**를 선택합니다.
2. 파일을 끌어 놓거나 **choose your files**로 선택합니다.
3. 업로드 목록을 확인하고 아래로 스크롤합니다.
4. **Commit changes**를 클릭합니다.

이제 파일이 Repository에 추가됩니다.

## 3. GitHub Pages 설정

Repository에 업로드한 HTML 파일을 웹에 공개합니다.

1. 저장소 상단의 **Settings** 탭을 엽니다.
2. 왼쪽 **Pages** 메뉴를 클릭합니다.
3. **Build and deployment → Source**에서 **Deploy from a branch**를 선택합니다.
4. **Branch**에서 `main`을, 폴더에서 `/(root)`를 선택합니다.
5. **Save**를 클릭합니다.

배포가 완료되면 해당 화면에 웹사이트 URL이 표시됩니다.

## 4. Web Browser에서 확인

URL을 브라우저에 입력하여 배포한 페이지를 확인합니다.

예시 주소:

`https://junypark0427.github.io/calculator/`

> 예시 주소는 저장소 이름과 GitHub 사용자 이름을 기준으로 만들어진 것입니다. 페이지가 바로 보이지 않으면 GitHub Pages의 배포 진행 상황을 확인합니다.

## 5. `index.html`과 URL

GitHub Pages는 폴더 주소에 접속했을 때 해당 폴더의 **`index.html`**을 기본 페이지로 사용할 수 있습니다. 따라서 파일 이름을 URL에 적지 않아도 첫 화면을 보여 줄 수 있습니다.

예를 들어 `index.html`을 다음과 같이 만들 수 있습니다.

```html
<!DOCTYPE html>
<html lang="ko">
  <body>
    <p><a href="Calculator.html">계산기 바로가기</a></p>
  </body>
</html>
```

`Calculator.html` 파일이 같은 폴더에 있다면 이 링크를 클릭하여 계산기 페이지로 이동할 수 있습니다.
