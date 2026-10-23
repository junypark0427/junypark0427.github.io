# 06. Git 기초, 배포

## 1. Repository 만들기(Page 설정)

GitHub Pages는 Repository의 이름을 사용자 이름으로 만들면 `.github.io` 형식의 URL로 Web Site를 만들어 준다.

따라서 우리는 사용자 이름과 같은 Repository를 만든다.

Repository 이름 규칙: `<username>.github.io`

### 생성 순서

1. GitHub에 로그인한다.
2. 오른쪽 상단의 `+` 버튼을 클릭하고 `New repository`를 선택한다.
3. Repository name에 `<username>.github.io`를 입력한다.
4. 공개 범위를 `Public`으로 설정한다.
5. `Create repository` 버튼을 클릭한다.

### 파일 Upload

Local에서 작성했던 MkDocs 설정 파일을 GitHub에서 직접 Upload한다. 작성한 파일이 없다면 `Creating new file`을 이용한다.

**폴더 구조 확인**

```text
Juny's Log/
├── docs/
│   └── index.md
└── mkdocs.yml
```

GitHub에는 이 폴더의 내용 전체를 올린다.

### 웹에서 만든 파일 Upload

1. 방금 만든 Repository 페이지로 이동한다.
2. `Add file` → `Upload files` 클릭
3. `docs/` 폴더와 `mkdocs.yml` 파일을 드래그하거나 선택한다.
4. 하단 `Commit changes`에서 적당한 메모를 작성한 후 `Commit changes` 버튼 클릭

폴더째로 드래그하면 폴더 구조 그대로 올라간다.

### mkdocs.yml 수정

(Local에서 수정한 후 Uploading 권장)

MkDocs의 설정 파일을 GitHub의 Web Site에 맞게 수정한다.

`Repository` → `Code` → `mkdocs.yml` → `Edit this file(연필 아이콘)` → 수정 → `Commit`

```yaml
site_name: Juny's Log
site_url: https://<username>.github.io/
```

**설명**

- `site_name`: Web Site의 이름
- `site_url`: GitHub Pages를 통해 배포할 Web Site의 주소

### 파일을 올리는 과정

Git을 이용해 파일을 GitHub에 올리는 과정은 일반적으로 `add → commit → push`의 순서로 이루어진다.

1. **add**: 변경한 파일을 Git의 Staging Area에 추가한다. 다음 Commit에 포함할 파일을 선택하는 과정이다.
2. **commit**: 추가한 파일의 변경 내용을 하나의 버전으로 기록한다. 언제 어떤 부분을 수정했는지 관리할 수 있다.
3. **push**: Local Repository에 기록한 Commit을 GitHub의 원격 Repository로 전송한다.

GitHub 웹에서 `Upload files` → `Commit changes`를 클릭하면 별도로 Git 명령어를 입력하지 않고 원격 Repository에 파일과 Commit을 생성할 수 있다.

**Commit 메시지:** Commit할 때는 어떤 변경인지 설명하는 메시지를 남긴다.

---

## 2. GitHub Actions 설정

GitHub Actions는 반복적인 작업을 자동으로 처리해 주는 기능이다.

"파일을 Push하면 자동으로 빌드 + 배포"하도록 설정할 수 있다.

1. 내가 파일을 올린다. (Push)
2. GitHub Actions가 감지한다.
3. Markdown → HTML 변환 (Build)
4. GitHub Pages에 배포한다.
5. 사이트 업데이트 완료

Actions는 `.github/workflows/` 폴더 안에 `.yml` 파일을 넣어서 설정한다.

### Workflow 파일 만들기

1. Repository 페이지에서 `Add file` → `Create new file` 클릭
2. 파일 이름 입력란에 `.github/workflows/deploy.yml` 입력
3. 아래 내용을 붙여 넣는다.

```yaml
name: Deploy MkDocs

on:
  push:
    branches:
      - main
  workflow_dispatch:

permissions:
  contents: read
  pages: write
  id-token: write

jobs:
  deploy:
    runs-on: ubuntu-latest
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}

    steps:
      - uses: actions/checkout@v4

      - uses: actions/setup-python@v5
        with:
          python-version: '3.12'

      - name: Install MkDocs
        run: pip install mkdocs-material

      - name: Build
        run: mkdocs build

      - name: Configure Pages
        uses: actions/configure-pages@v5

      - name: Upload
        uses: actions/upload-pages-artifact@v4
        with:
          path: site

      - name: Deploy
        id: deployment
        uses: actions/deploy-pages@v4
```

4. 오른쪽 위 `Commit changes` 클릭 → 메시지 확인 후 `Commit changes`

### Workflow 파일 설명

Workflow 파일에는 GitHub Actions가 언제, 어떤 작업을 실행할지 설정한다.

- `name`: Workflow의 이름
- `on`: Workflow가 실행되는 조건이다. 여기서는 main 브랜치에 Push하면 실행된다.
- `workflow_dispatch`: GitHub 웹에서 수동으로 Workflow를 실행할 수 있도록 한다.
- `permissions`: GitHub Pages 배포에 필요한 권한을 설정한다.
- `jobs`: 자동으로 실행할 작업들을 정의한다.
- `runs-on`: 작업을 실행할 환경이다. 여기서는 Ubuntu를 사용한다.
- `steps`: 작업을 수행하는 구체적인 단계이다.
- `checkout`: Repository의 파일을 가져온다.
- `setup-python`: Python 실행 환경을 준비한다.
- `pip install mkdocs-material`: MkDocs와 Material 테마를 설치한다.
- `mkdocs build`: Markdown 파일을 HTML로 변환한다.
- `upload-pages-artifact`: 변환된 파일을 배포할 수 있도록 업로드한다.
- `deploy-pages`: 생성된 웹사이트를 GitHub Pages에 배포한다.

---

## 3. GitHub Pages 설정 변경

GitHub Pages를 통해 웹사이트가 공개되도록 설정한다.

1. GitHub Repository 페이지로 이동한다.
2. 상단의 `Settings` 클릭
3. 왼쪽 메뉴에서 `Pages` 선택
4. `Build and deployment` 항목 확인
5. `Source`를 `GitHub Actions`로 설정한다.

이번에는 GitHub Actions에서 직접 배포하도록 설정했기 때문에 Branch를 별도로 선택하지 않는다.

참고로 `Deploy from a branch`는 특정 브랜치에 저장된 파일을 배포할 때 사용하는 방식이다. `mkdocs gh-deploy`로 `gh-pages` 브랜치에 배포하는 경우에는 `gh-pages / (root)`를 선택할 수 있다.

---

## 4. Actions 실행 확인

Workflow 파일을 생성한 후에는 GitHub Actions가 정상적으로 실행되는지 확인한다.

1. Repository 페이지의 `Actions` 탭 클릭
2. 왼쪽에서 `Deploy MkDocs` Workflow 선택
3. 실행 기록을 클릭하여 진행 상태 확인
4. 초록색 체크 표시가 나타나면 실행 성공

만약 빨간색 X 표시가 나타난다면 실행 과정에서 오류가 발생한 것이다.

해당 실행 기록을 클릭하면 어느 단계에서 오류가 발생했는지 확인할 수 있다.

주로 `mkdocs.yml` 파일의 문법이나 파일 경로, Markdown 파일 이름 등을 확인하면 된다.

---

## 5. 브라우저에서 확인

배포가 정상적으로 완료되었다면 브라우저에서 Web Site를 확인할 수 있다.

주소 창에 다음 URL을 입력한다.

`https://<username>.github.io/`

`docs/index.md`에 작성한 내용이 첫 화면으로 나타나면 배포에 성공한 것이다.

웹사이트가 나타나지 않는다면 다음 사항을 확인한다.

1. Repository 이름이 정확한지 확인
2. GitHub Actions 실행이 성공했는지 확인
3. `Settings` → `Pages` 설정 확인

첫 배포 후에는 웹사이트가 반영되기까지 약간의 시간이 걸릴 수 있다.

---

## 6. 앞으로의 작업 흐름

1. VSCode에서 Markdown 파일 작성, 수정
2. GitHub 웹에서 `Upload files` → `Commit changes`
3. Actions가 자동으로 Build, 배포
4. 브라우저에서 확인

이 과정을 반복하면 Markdown 파일만 수정해도 Web Site를 업데이트할 수 있다.

---

## 참고: 터미널로 배포하는 방법

앞에서는 GitHub 웹사이트에서 파일을 직접 Upload하였다.

하지만 Git을 설치하면 터미널에서도 파일을 업로드할 수 있다.

### 1. Repository 가져오기

먼저 GitHub에 있는 Repository를 Local로 복제한다.

```bash
git clone https://github.com/<username>/<username>.github.io.git
```

`git clone`은 원격 Repository의 파일과 Git 기록을 내 컴퓨터로 가져오는 명령어이다.

복제가 완료되면 해당 폴더로 이동한다.

```bash
cd <username>.github.io
```

### 2. 변경 사항 업로드

Markdown 파일을 수정한 후 터미널에서 다음 명령어를 실행한다.

```bash
git status
git add .
git commit -m "Update Markdown files"
git push origin main
```

각 명령어의 역할은 다음과 같다.

- `git status`: 현재 변경된 파일 확인
- `git add .`: 변경된 파일을 Staging Area에 추가
- `git commit -m "..."`: 변경 사항을 기록
- `git push origin main`: 변경 사항을 GitHub에 업로드

Push가 완료되면 GitHub Actions가 자동으로 실행되어 Web Site가 업데이트된다.

### 3. 공용 PC에서 로그인 정보 지우기

학교 컴퓨터 등 공용 PC에서 GitHub를 이용했다면 사용 후 계정 정보를 정리해야 한다.

1. GitHub 웹사이트에서 로그아웃한다.
2. VSCode에서 GitHub 계정에 로그인했다면 로그아웃한다.
3. GitHub CLI를 사용했다면 다음 명령어로 로그아웃한다.

```bash
gh auth logout --hostname github.com
```

4. Windows의 `자격 증명 관리자`에서 자신이 저장한 GitHub 인증 정보가 있는지 확인하고 삭제한다.
5. 필요하다면 Local에 복제한 Repository 폴더도 삭제한다.

웹사이트에서 로그아웃하더라도 Git 프로그램에 인증 정보가 별도로 저장되어 있을 수 있으므로 주의해야 한다.
