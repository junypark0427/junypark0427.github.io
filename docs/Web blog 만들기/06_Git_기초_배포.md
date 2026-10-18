# 06. Git 기초와 GitHub Pages 자동 배포

이번 장에서는 로컬에서 작성한 MkDocs 학습 노트를 GitHub에 올리고, **GitHub Actions가 Markdown을 HTML로 변환하여 GitHub Pages에 자동으로 배포**하도록 설정합니다.

> **앞선 실습과의 차이:** 03장에서는 `calculator` 저장소에 HTML을 직접 올려 공개했습니다. 이번에는 **`<username>.github.io` 저장소**에 MkDocs 원본 파일을 저장하고 GitHub Actions로 빌드 및 배포합니다.
>
> 이 장의 기본 방식은 **GitHub Actions 직접 배포**입니다. `Deploy from a branch → gh-pages → /(root)` 방식과 혼용하지 마세요. 두 방식의 차이는 3장과 아래의 '터미널로 배포하기'에서 설명합니다.

## 1. Repository 만들기와 파일 업로드

### 1-1. 사용자 웹사이트 저장소 만들기

GitHub는 사용자 이름과 일치하는 저장소 이름을 사용하면 다음과 같은 주소의 **사용자 사이트(User site)**를 제공할 수 있습니다.

- 저장소 이름: `<username>.github.io`
- 웹사이트 URL: `https://<username>.github.io/`

예를 들어 사용자 이름이 `junypark0427`이면 저장소 이름은 `junypark0427.github.io`입니다.

**생성 순서**

1. [GitHub](https://github.com/)에 로그인합니다.
2. 우측 상단 **+ → New repository**를 선택합니다.
3. **Owner**가 자신의 계정인지 확인합니다.
4. **Repository name**에 `<username>.github.io`를 입력합니다.
5. **Description**에는 원하는 설명을 입력합니다.
6. **Public**을 선택합니다(무료 계정에서 공개 사이트를 만드는 기본적인 방법).
7. **Add a README file**은 체크하지 않아도 됩니다. 아래 파일 업로드 과정에서 첫 커밋을 만들 수 있습니다.
8. **Create repository**를 클릭합니다.

### 1-2. 프로젝트 폴더 구조 확인

로컬 프로젝트는 다음처럼 구성합니다.

```text
프로젝트 폴더/ (예: Juny's Log)
├── docs/
│   ├── index.md
│   ├── 01_WebSite_WebPage.md
│   ├── 02_github_blog.md
│   ├── 03_GitHub_Pages.md
│   ├── 04_Markdown_MkDocs.md
│   ├── 05_MkDocs_설치_프로젝트_구조.md
│   └── 06_Git_기초_배포.md
├── mkdocs.yml
├── requirements.txt
└── .github/
    └── workflows/
        └── deploy.yml
```

- `docs/`: 웹사이트로 보여 줄 Markdown 원본
- `mkdocs.yml`: 사이트 제목, 주소, 테마, 탐색 메뉴 설정
- `requirements.txt`: 설치할 Python 패키지 목록
- `.github/workflows/deploy.yml`: 자동 빌드와 배포 규칙

**중요:** `docs/`와 `mkdocs.yml`은 저장소 **최상위 폴더**에 있어야 합니다. 프로젝트를 압축했다면 ZIP 파일을 그대로 업로드하는 것이 아니라 **압축을 풀고 내부 파일을 업로드**해야 합니다.

### 1-3. GitHub 웹에서 파일 올리기

1. 방금 만든 저장소의 **Code** 화면을 엽니다.
2. 빈 저장소라면 **uploading an existing file**, 이미 파일이 있다면 **Add file → Upload files**를 클릭합니다.
3. `docs/` 폴더와 `mkdocs.yml`, `requirements.txt`를 끌어 놓습니다. 이때 부모 폴더까지 한 단계 더 들어가지 않게 주의합니다.
4. 변경 파일 목록에서 경로가 `docs/index.md`, `mkdocs.yml`처럼 보이는지 확인합니다.
5. 아래 **Commit changes**에 `Add MkDocs documentation`처럼 작업 내용을 적고 저장합니다.
6. **`.github/workflows/deploy.yml`**은 다음 절에서 GitHub의 **Create new file** 기능으로 만들 수 있습니다. 파일 탐색기에서 `.`으로 시작하는 폴더가 숨겨질 수 있기 때문입니다.

> `mkdocs.yml`의 `site_url`은 자신의 실제 사용자 이름에 맞게 수정하세요. 이 예제 프로젝트에는 제공된 자료에 등장하는 주소 `https://junypark0427.github.io/`가 입력되어 있습니다.

### 1-4. Git의 `add`, `commit`, `push` 이해하기

터미널에서 Git을 사용할 때 변경 사항을 업로드하는 기본 흐름은 다음과 같습니다.

1. **`git add` (스테이징)**: 다음 커밋에 포함할 수정 파일을 선택합니다. 예: `git add .`
2. **`git commit` (기록)**: 선택한 변경을 로컬 저장소의 새 버전으로 기록합니다. 예: `git commit -m "Add MkDocs pages"`
3. **`git push` (업로드)**: 로컬 커밋을 GitHub 원격 저장소에 전송합니다. 예: `git push origin main`

```text
작업 파일 ── git add ──▶ Staging Area
                             │ git commit
                             ▼
                       로컬 Git 기록
                             │ git push
                             ▼
                       GitHub Repository
```

**GitHub 웹에서 `Upload files → Commit changes`를 사용하면** 웹사이트가 원격 저장소에 파일과 커밋을 바로 생성하므로, 사용자가 별도로 `git add`, `git push`를 실행할 필요는 없습니다.

## 2. GitHub Actions 설정

**GitHub Actions**는 저장소에서 지정한 이벤트가 발생하면 자동으로 작업을 실행하는 기능입니다. 여기서는 다음 과정을 자동화합니다.

1. 내가 `main` 브랜치에 파일을 올리거나 수정합니다(`push`).
2. GitHub Actions가 변경을 감지합니다.
3. Python과 MkDocs를 설치합니다.
4. Markdown을 HTML로 변환합니다(`mkdocs build`).
5. 만들어진 `site/` 결과물을 GitHub Pages에 배포합니다.
6. 브라우저에서 최신 내용을 확인할 수 있습니다.

워크플로 파일은 `.github/workflows/` 안에 `.yml` 확장자로 저장합니다.

### 2-1. 워크플로 파일 만들기

1. GitHub 저장소 화면에서 **Add file → Create new file**을 클릭합니다.
2. 파일 이름에 **`.github/workflows/deploy.yml`**을 입력합니다. `/`를 입력하면 GitHub에서 폴더 구조를 만듭니다.
3. 아래 YAML 전체를 붙여넣습니다.
4. 오른쪽 위 **Commit changes**를 클릭하고 변경 사항을 `main` 브랜치에 저장합니다.

```yaml
name: Deploy MkDocs to GitHub Pages

on:
  push:
    branches:
      - main
  workflow_dispatch:

permissions:
  contents: read
  pages: write
  id-token: write

concurrency:
  group: pages
  cancel-in-progress: false

jobs:
  deploy:
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    runs-on: ubuntu-latest
    steps:
      - name: Checkout source
        uses: actions/checkout@v4

      - name: Set up Python
        uses: actions/setup-python@v5
        with:
          python-version: '3.12'

      - name: Install dependencies
        run: |
          python -m pip install --upgrade pip
          pip install -r requirements.txt

      - name: Build MkDocs site
        run: mkdocs build --clean

      - name: Configure GitHub Pages
        uses: actions/configure-pages@v5

      - name: Upload built site
        uses: actions/upload-pages-artifact@v4
        with:
          path: site

      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v4
```

> 위 내용은 제공된 프로젝트의 `.github/workflows/deploy.yml` 파일에도 **그대로 포함**되어 있습니다. 이미 해당 파일을 업로드했다면 GitHub 웹에서 다시 만들 필요는 없습니다.

### 2-2. 워크플로 내용 이해하기

| 항목 | 하는 일 |
| --- | --- |
| `name` | Actions 목록에 표시될 워크플로 이름 |
| `on.push.branches: [main]` | `main` 브랜치에 변경 사항이 올라오면 실행 |
| `workflow_dispatch` | GitHub 웹의 **Run workflow** 버튼으로 수동 실행 가능 |
| `permissions` | 저장소 읽기와 Pages 배포에 필요한 권한 설정 |
| `actions/checkout` | GitHub 저장소의 파일을 작업 환경으로 내려받기 |
| `actions/setup-python` | Python 설치 환경 준비 |
| `pip install -r requirements.txt` | Material 테마를 비롯한 Python 의존성 설치 |
| `mkdocs build --clean` | Markdown 문서를 HTML로 변환하여 `site/` 생성 |
| `actions/configure-pages` | GitHub Pages 배포 환경 구성 |
| `actions/upload-pages-artifact` | 생성된 정적 파일을 배포용 아티팩트로 업로드 |
| `actions/deploy-pages` | 아티팩트를 GitHub Pages 사이트에 공개 |

`${{ steps.deployment.outputs.page_url }}`는 배포 단계에서 반환된 웹사이트 주소를 의미합니다. 처음 본다면 '배포가 끝난 뒤 사이트 URL로 사용되는 값' 정도로 이해하면 됩니다.

## 3. GitHub Pages 설정 변경

**이 강의에서 채택한 Actions 직접 배포 방식**을 기준으로 설정합니다.

1. GitHub에서 해당 저장소로 이동합니다.
2. 상단 **Settings** 탭을 클릭합니다.
3. 왼쪽 메뉴에서 **Pages**를 클릭합니다.
4. **Build and deployment → Source**를 찾습니다.
5. **GitHub Actions**를 선택합니다. 별도의 브랜치나 폴더를 고를 필요는 없습니다.
6. 설정을 변경했다면 저장된 상태인지 확인합니다.

**왜 `Deploy from a branch → gh-pages / (root)`가 아닌가요?**

- `gh-pages` 브랜치 방식은 **이미 HTML로 빌드된 파일**을 해당 브랜치에 올려 공개하는 배포 방식입니다.
- 이 문서에서 작성한 워크플로는 `actions/deploy-pages`를 통해 **GitHub Pages에 직접 배포**하므로 **Source를 GitHub Actions**로 설정해야 합니다.
- `main / (root)`를 선택하면 MkDocs 소스 파일만 공개하려는 잘못된 설정이 될 수 있습니다.

> **기존 메모의 `gh-pages / (root)` 설정을 유지하려면** 아래의 '별도 방식: `mkdocs gh-deploy`'를 읽어야 합니다. 이번 자동 배포 워크플로와 두 방식을 동시에 설정할 필요는 없습니다.

## 4. Actions 실행 확인

1. 저장소 상단의 **Actions** 탭을 클릭합니다.
2. 왼쪽의 **Deploy MkDocs to GitHub Pages** 워크플로를 선택합니다.
3. 가장 최근 실행 내역을 열어 진행 상태를 확인합니다.
4. **초록색 체크 표시(Success)**가 보이면 워크플로가 성공한 것입니다.
5. 자동으로 실행되지 않았다면 **Run workflow → Run workflow**로 수동 실행할 수 있습니다.
6. 실패했다면 실행 내역에서 빨간색 단계의 로그를 펼쳐 **파일 경로, YAML 문법, 설치 오류** 등을 확인합니다.

자주 발생하는 문제:

- **`mkdocs.yml`을 찾지 못함**: 파일이 저장소 최상위에 있는지 확인합니다.
- **`requirements.txt`를 찾지 못함**: 파일을 함께 업로드했는지 확인합니다.
- **`Documentation file ... is not found`**: `mkdocs.yml`의 `nav`에 적은 `.md` 이름과 실제 파일 이름이 정확히 같은지 확인합니다.
- **Pages 배포 권한 오류**: Settings → Pages의 Source가 **GitHub Actions**인지 확인하고, 워크플로에 `pages: write`, `id-token: write` 권한이 있는지 확인합니다.

## 5. 브라우저에서 확인

1. 저장소 **Settings → Pages**에서 안내하는 사이트 주소를 확인합니다.
2. 브라우저 주소창에 `https://<username>.github.io/`를 입력합니다.
3. **Juny's Log** 첫 화면과 왼쪽 메뉴의 강의 목록이 나타나면 성공입니다.
4. 문서 내용이 이전 버전이면 Actions 실행이 끝났는지 확인한 뒤 브라우저에서 새로고침합니다.

> 사이트 주소가 `404 Not Found`로 나타나면 저장소 이름, 사이트 주소, Pages 설정, Actions 실행 상태를 순서대로 확인합니다. 첫 배포의 공개 반영에는 약간의 시간이 걸릴 수 있습니다.

## 6. 앞으로의 작업 흐름

가장 간단한 작업 과정은 다음과 같습니다.

```text
VS Code에서 docs/*.md 작성 또는 수정
                 ↓
GitHub 웹에서 Upload files → Commit changes
   (또는 Git으로 commit + push)
                 ↓
GitHub Actions가 자동으로 MkDocs 빌드
                 ↓
GitHub Pages에 배포
                 ↓
브라우저에서 결과 확인
```

문서를 새로 추가한다면 `docs/`에 `.md` 파일을 저장한 뒤 `mkdocs.yml`의 `nav` 메뉴에도 파일을 추가합니다.

---

## 참고 1. 터미널로 GitHub에 올리는 방법

이미 GitHub에 저장소가 있다면 **clone → 파일 수정 → add → commit → push** 순서로 작업할 수 있습니다.

먼저 Git이 설치되어 있는지 확인합니다.

```bash
git --version
```

작업할 컴퓨터에서 저장소를 복제합니다.

```bash
git clone https://github.com/<username>/<username>.github.io.git
cd <username>.github.io
```

그 폴더 안에 `docs/`, `mkdocs.yml`, `requirements.txt`, `.github/workflows/deploy.yml` 파일을 복사합니다. 이후 다음 명령을 실행합니다.

```bash
git status
git add .
git commit -m "Update MkDocs notes"
git push origin main
```

- `git status`: 변경된 파일과 스테이징 상태 확인
- `git add .`: 현재 폴더의 수정 사항을 스테이징
- `git commit -m "..."`: 로컬 버전 이력 기록
- `git push origin main`: GitHub로 전송하여 Actions 자동 배포 시작

Git 사용자 이름/이메일 등록이 필요하면, **공용 PC에서는 `--global` 없이 현재 저장소에만 적용**하는 편이 안전합니다.

```bash
git config user.name "내 GitHub 이름"
git config user.email "내 이메일 주소"
```

### 공용 PC 사용 후 로그인 정보 정리

1. GitHub 웹사이트에서 **로그아웃**하고, GitHub 계정이 열린 다른 탭도 닫습니다.
2. VS Code에서 GitHub 계정으로 로그인했다면 **계정 메뉴에서 로그아웃**합니다.
3. GitHub CLI(`gh`)로 로그인했다면 다음 명령을 실행합니다.

   ```bash
   gh auth logout --hostname github.com
   ```

4. **Windows**에서는 **자격 증명 관리자 → Windows 자격 증명**에서 자신이 사용한 GitHub/Git 관련 로그인 정보를 찾아 제거합니다. **macOS**에서는 **키체인 접근(Keychain Access)**에서 자신이 저장한 GitHub 인증 정보를 확인하고 제거합니다. 다른 사용자의 로그인 정보는 지우지 마세요.
5. 공용 PC의 **로컬 프로젝트 폴더**에 개인정보나 코드가 남아 있지 않도록 정리하고, 필요하다면 휴지통도 비웁니다.
6. 자신이 만든 접근 토큰(Personal Access Token)을 공용 PC에 저장하지 않습니다. 노출이 의심되면 GitHub 계정 설정에서 해당 토큰을 폐기합니다.

> GitHub 웹에서 로그인만 한 경우와 Git 자격 증명 관리자에 인증 정보를 저장한 경우는 다릅니다. 웹 로그아웃만으로 **Git 도구에 저장된 인증 정보까지 항상 삭제되는 것은 아닙니다.**

## 참고 2. 별도 방식: 터미널의 `mkdocs gh-deploy`

GitHub Pages를 **브랜치에서 배포**하는 방식으로 운영하고 싶을 때 사용할 수 있습니다. 이 경우에는 위의 **GitHub Actions 직접 배포 워크플로를 사용하지 않는 별도 구성**으로 진행하세요.

1. `mkdocs.yml`과 `docs/`가 있고 GitHub 원격 저장소가 연결된 프로젝트에서 아래 명령을 실행합니다.

   ```bash
   mkdocs gh-deploy
   ```

2. MkDocs가 사이트를 빌드하여 `gh-pages` 브랜치에 HTML을 업로드합니다.
3. GitHub **Settings → Pages → Source**를 **Deploy from a branch**로 바꿉니다.
4. **Branch: `gh-pages`, Folder: `/(root)`**를 선택하고 저장합니다. `gh-pages` 브랜치가 목록에 없으면 `mkdocs gh-deploy`가 정상 완료되었는지 확인합니다.
5. 사이트 주소에서 결과를 확인합니다.

이 방식은 로컬에서 `mkdocs gh-deploy`를 실행할 때마다 배포됩니다. 원본 파일을 `main`에 `push`하는 것만으로 항상 자동 배포되는 방식은 아닙니다. **현재 제공한 프로젝트는 위의 GitHub Actions 직접 배포를 기본값**으로 사용합니다.

### 참고 링크

- [GitHub Pages: 게시 소스 설정 공식 문서](https://docs.github.com/en/pages/getting-started-with-github-pages/configuring-a-publishing-source-for-your-github-pages-site)
- [MkDocs: GitHub Pages에 배포하기](https://www.mkdocs.org/user-guide/deploying-your-docs/)
