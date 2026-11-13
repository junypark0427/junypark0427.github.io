# 08. Markdown 치트시트와 mkdocs.yml 옵션 정리

이 페이지에서는 Markdown으로 글을 작성할 때 자주 사용하는 문법과 `mkdocs.yml`의 주요 설정을 정리한다. 예시는 **Material for MkDocs** 테마를 기준으로 한다.

---

## 📝 1. Markdown 치트시트

Markdown은 일반 텍스트에 간단한 기호를 사용하여 제목, 목록, 표, 링크 등을 작성하는 문법이다. MkDocs에서는 `docs/` 폴더 안에 있는 `.md` 파일을 웹페이지로 변환한다.

### 제목

제목 앞에 `#`을 붙인다. `#`의 개수가 많아질수록 제목의 단계가 낮아진다.

````markdown
# 제목 1
## 제목 2
### 제목 3
#### 제목 4
````

일반적으로 한 페이지의 큰 제목에는 `#`을 한 번 사용하고, 세부 내용은 `##`, `###`으로 구분한다.

### 글자 강조

| 작성 방법 | 결과 |
| --- | --- |
| `**굵은 글씨**` | **굵은 글씨** |
| `*기울임*` | *기울임* |
| `~~취소선~~` | ~~취소선~~ |
| `` `코드` `` | `코드` |

### 목록

순서가 없는 목록에는 `-`, 순서가 있는 목록에는 숫자를 사용한다.

````markdown
- 첫 번째 항목
- 두 번째 항목
  - 하위 항목

1. 첫 번째 단계
2. 두 번째 단계
3. 세 번째 단계
````

하위 항목을 만들 때는 들여쓰기를 사용한다.

### 체크리스트

````markdown
- [x] 완료한 작업
- [ ] 아직 하지 않은 작업
````

Material 테마에서 체크리스트를 보기 좋게 표시하려면 `mkdocs.yml`의 `markdown_extensions`에 `pymdownx.tasklist`를 추가하면 된다.

### 🔗 링크와 이미지

````markdown
[GitHub 방문하기](https://github.com/)

![이미지 설명](images/example.png)
````

- **링크:** `[표시할 글자](주소)`
- **이미지:** `![대체 텍스트](이미지 경로)`

이미지 파일이 `docs/images/example.png`에 있고 현재 글이 `docs/index.md`라면 `images/example.png`로 연결할 수 있다. **현재 Markdown 파일의 위치에 따라 상대 경로가 달라질 수 있다.**

다른 Markdown 페이지로 이동하는 링크도 작성할 수 있다.

````markdown
[디자인 설정 보기](07_mkdocs_yml_디자인.md)
````

위 예시는 연결할 두 파일이 같은 폴더에 있을 때 사용한다.

### 인용문과 구분선

````markdown
> 이 문장은 인용문이다.
> 여러 줄로 작성할 수 있다.

---
````

- `>`: 인용문
- `---`: 문단을 구분하는 수평선 (단독 줄에 작성)

### 💻 코드 블록

짧은 명령어는 백틱(`) 한 쌍으로 감싸고, 여러 줄의 코드는 백틱 3개로 감싼다. 언어를 지정하면 문법 강조도 적용된다.

````markdown
```python
print("Hello, World!")
```

```bash
mkdocs serve
```
````

### 표

````markdown
| 항목 | 설명 |
| --- | --- |
| MkDocs | Markdown을 웹사이트로 변환 |
| GitHub Pages | 웹사이트를 공개하는 서비스 |
````

표의 두 번째 줄에 있는 `---`은 제목 행과 본문을 구분한다. 열은 `|` 기호로 나눈다.

### 줄바꿈과 문단

Markdown에서는 **빈 줄을 넣으면 새로운 문단**이 만들어진다. 문장 끝에서 줄만 바꾸면 화면에서는 이어져 보일 수 있다. 필요하다면 문장 끝에 공백 두 칸을 추가해 줄바꿈을 만들 수 있다.

> **참고:** Markdown 확장 기능을 추가하면 기본 문법 외에도 접을 수 있는 박스, 각주, 탭 등 다양한 형식을 사용할 수 있다.

---

## ⚙️ 2. mkdocs.yml 옵션 정리

`mkdocs.yml`은 MkDocs 프로젝트의 설정 파일이다. 사이트 정보, 탐색 메뉴, 테마, 플러그인 등을 지정한다. **최상위 Key의 작성 순서는 상관없지만, YAML의 들여쓰기는 정확해야 한다.**

### 기본 설정

```yaml
site_name: Juny's Log
site_description: 개인 기록 웹사이트
site_author: Juny
site_url: https://example.github.io/
```

| 옵션 | 설명 |
| --- | --- |
| `site_name` | 사이트 이름. 필수 항목이다. |
| `site_description` | 사이트를 소개하는 문구. 메타 정보에 사용된다. |
| `site_author` | 사이트 작성자 이름. 메타 정보에 사용된다. |
| `site_url` | 배포된 사이트의 기본 주소. 실제 주소로 변경해야 한다. |

### 📁 docs_dir / site_dir

```yaml
docs_dir: docs
site_dir: site
```

- `docs_dir`: 원본 Markdown 파일을 보관하는 폴더이다. 기본값은 `docs`이다.
- `site_dir`: `mkdocs build`로 생성한 HTML 등의 결과물이 저장되는 폴더이다. 기본값은 `site`이다.

```text
project/
├── docs/             # Markdown 원본
│   └── index.md
├── site/             # 빌드 결과물 (자동 생성)
└── mkdocs.yml
```

일반적으로 `site/`는 자동 생성되는 폴더이므로 Git 관리 대상에서 제외한다. `.gitignore` 파일에 `site/`를 추가하면 된다.

### 🧭 nav - 목차 구성

`nav`는 웹사이트 메뉴에 표시할 페이지의 이름과 순서를 지정한다.

```yaml
nav:
  - 홈: index.md
  - MkDocs 학습:
      - 디자인: 07_mkdocs_yml_디자인.md
      - Markdown 치트시트: 08_Markdown_치트시트와_mkdocs_yml_옵션.md
  - GitHub: https://github.com/
```

- `홈`, `디자인` 등은 화면에 표시할 메뉴 이름이다.
- `.md` 경로는 `docs_dir`(`docs/`)을 기준으로 작성한다.
- 메뉴 아래에 목록을 들여쓰면 하위 메뉴가 만들어진다.
- 웹사이트 밖의 링크도 메뉴에 추가할 수 있다.

`nav`에 없는 Markdown 파일도 빌드될 수 있지만, 메뉴에 자동으로 표시되지는 않는다. 실제로 존재하는 파일명과 경로를 정확하게 작성해야 한다.

### 🎨 theme - 테마 설정

```yaml
theme:
  name: material
  language: ko
  palette:
    primary: indigo
    accent: amber
  features:
    - navigation.top
    - content.code.copy
```

- `name`: 사용할 테마이다. `material`을 사용하려면 Material for MkDocs가 설치되어 있어야 한다.
- `language`: 테마에서 제공하는 인터페이스 언어를 설정한다.
- `palette`: 주요 색상(`primary`)과 강조 색상(`accent`)을 지정한다.
- `features`: 페이지 이동 버튼, 코드 복사 버튼 등 Material 테마의 부가 기능을 활성화한다.

색상과 `features`의 자세한 내용은 앞의 **07. mkdocs.yml 디자인** 페이지를 참고한다.

### 🌓 다크/라이트 모드 전환 버튼

`theme.palette`를 목록으로 작성하면 밝은 테마와 어두운 테마를 선택하는 버튼을 추가할 수 있다.

```yaml
theme:
  name: material
  palette:
    - scheme: default
      primary: indigo
      accent: amber
      toggle:
        icon: material/brightness-7
        name: 어두운 테마로 전환

    - scheme: slate
      primary: indigo
      accent: amber
      toggle:
        icon: material/brightness-4
        name: 밝은 테마로 전환
```

- `scheme: default`: 밝은 테마
- `scheme: slate`: 어두운 테마
- `toggle.icon`: 버튼에 사용하는 아이콘
- `toggle.name`: 버튼에 표시되는 설명

**주의:** `theme:`과 `palette:`는 같은 설정 파일에서 여러 번 중복하여 선언하지 않고, 기존 설정 안에 합쳐서 작성한다.

### 🖼️ icon - 로고와 파비콘

Material 테마에서는 **내장 아이콘**을 로고로 사용하거나, `docs/` 폴더에 저장한 이미지 파일을 사용할 수 있다.

**내장 아이콘을 로고로 사용하기**

```yaml
theme:
  name: material
  icon:
    logo: material/library
```

**직접 만든 이미지 사용하기**

```yaml
theme:
  name: material
  logo: images/logo.png
  favicon: images/favicon.png
```

```text
docs/
└── images/
    ├── logo.png
    └── favicon.png
```

- `theme.icon.logo`: Material 테마에 포함된 아이콘을 로고로 지정한다.
- `theme.logo`: 직접 준비한 로고 파일을 지정한다.
- `theme.favicon`: 브라우저 탭에 표시할 파비콘 파일을 지정한다.

`icon` 아래에 파비콘 경로를 넣는 것이 아니라 **`theme.favicon`으로 설정**한다. 로고는 `theme.icon.logo`와 `theme.logo` 중 사용할 방식을 선택하면 된다.

### 🧩 plugins - 플러그인

플러그인은 검색 기능이나 추가적인 페이지 처리처럼 기본 기능을 확장한다.

```yaml
plugins:
  - search
```

- `search`: 사이트 내부 검색 기능을 제공한다.
- 별도의 플러그인을 추가할 때는 해당 패키지를 설치해야 하는 경우가 있다.
- `plugins` 항목을 명시한 경우, 기본 검색 기능을 유지하려면 `search`도 목록에 포함해야 한다.

예를 들어 새로운 플러그인을 사용하려면 패키지 설치 방법과 설정 옵션을 해당 플러그인의 공식 문서에서 확인한다.

### 📝 markdown_extensions - Markdown 확장

`markdown_extensions`를 사용하면 기본 Markdown 이외의 문법을 지원할 수 있다.

```yaml
markdown_extensions:
  - admonition
  - pymdownx.details
  - pymdownx.superfences
  - pymdownx.tasklist:
      custom_checkbox: true
  - toc:
      permalink: true
```

| 확장 | 기능 |
| --- | --- |
| `admonition` | 안내, 주의, 정보 등의 박스 생성 |
| `pymdownx.details` | 접고 펼칠 수 있는 박스 생성 |
| `pymdownx.superfences` | 코드 블록 등 다양한 블록 문법 처리 |
| `pymdownx.tasklist` | 체크리스트 표시 |
| `toc` | 제목으로 목차 생성. `permalink` 설정 가능 |

**주의:** `Markdown_extensions`가 아니라 소문자 **`markdown_extensions`**로 작성한다. `pymdownx` 확장은 필요한 패키지가 설치된 상태여야 한다. Material for MkDocs 설치 시 주요 확장을 함께 사용할 수 있다.

### ➕ extra - 추가 정보

`extra`에는 테마에서 사용하는 추가 정보나 링크 등을 작성할 수 있다. 예를 들어 Material 테마의 소셜 링크를 넣을 수 있다.

```yaml
extra:
  social:
    - icon: fontawesome/brands/github
      link: https://github.com/example
```

`link`를 실제 GitHub 주소로 변경하면 사이트 하단에서 GitHub로 이동하는 링크를 표시할 수 있다.

`extra`는 임의의 값을 담을 수 있는 설정이지만, **값을 적는 것만으로 모든 기능이 자동으로 생기지는 않는다.** 실제 표시 여부와 형식은 사용 중인 테마에서 지원하는지에 따라 달라진다.

### ©️ copyright

사이트 하단에 저작권 안내 문구를 표시한다.

```yaml
copyright: "© 2026 Juny's Log"
```

일반적으로 사이트의 푸터에 표시된다. `copyright`를 적었다고 해서 다른 사람이 만든 이미지나 글의 사용 허가가 자동으로 생기는 것은 아니다.

### 🔗 repo 연동

`repo_url`을 설정하면 웹사이트에서 GitHub Repository로 이동하는 링크를 표시할 수 있다.

```yaml
repo_url: https://github.com/example/example.github.io
repo_name: GitHub Repository
edit_uri: edit/main/docs/
```

- `repo_url`: GitHub Repository 주소
- `repo_name`: Repository 링크에 표시할 이름
- `edit_uri`: 페이지의 편집 화면으로 이동할 때 사용하는 경로. 예시는 `main` 브랜치의 `docs/` 폴더를 기준으로 한다.

Material 테마에서 각 페이지에 **편집 버튼**을 추가하려면 다음 기능도 활성화할 수 있다.

```yaml
theme:
  name: material
  features:
    - content.action.edit
```

Repository가 공개되어 있더라도 다른 방문자가 바로 내 파일을 수정할 수 있는 것은 아니다. 편집 권한이 없다면 GitHub에서 Fork나 변경 제안 방식으로 작업해야 한다.

### 📌 admonition (주석 박스) 사용법

Admonition은 본문에서 특별히 강조하고 싶은 내용에 사용하는 **안내 박스**이다. 프로그래밍 코드의 주석(`comment`)과는 다르다.

먼저 `mkdocs.yml`에 다음 확장을 추가한다.

```yaml
markdown_extensions:
  - admonition
  - pymdownx.details
  - pymdownx.superfences
```

**기본 사용법**

````markdown
!!! note "참고"
    이 내용은 참고 사항이다.

!!! warning "주의"
    설정 파일의 들여쓰기에 주의한다.

!!! tip "팁"
    변경 사항은 `mkdocs serve`에서 먼저 확인한다.
````

- `!!!`: 항상 펼쳐져 있는 박스를 만든다.
- `note`, `warning`, `tip`: 박스의 종류이다. `info`, `danger`, `success`, `question` 등도 사용할 수 있다.
- `"참고"`: 제목을 원하는 문구로 변경한다.
- **박스 안의 내용은 네 칸 들여쓰기**하여 작성한다.

**접을 수 있는 박스**

````markdown
??? question "정답 보기"
    정답은 3번이다.

???+ info "처음부터 열려 있는 설명"
    이 박스는 펼쳐진 상태로 나타난다.
````

- `???`: 처음에는 접힌 상태이다.
- `???+`: 처음부터 펼쳐진 상태이다.

**제목 없애기**

````markdown
!!! note ""
    제목이 없는 안내 박스이다.
````

수업 정리나 개발 문서에서 **주의사항, 추가 설명, 실습 팁** 등을 구분할 때 유용하다.

### 📋 전체 예시

아래는 지금까지 설명한 기능을 하나로 합친 `mkdocs.yml` 예시이다. `example` 부분을 자신의 GitHub 계정 정보로 변경하고, 예시에 있는 Markdown 파일이 실제로 존재하는지 확인해야 한다.

```yaml
# 사이트 기본 정보
site_name: Juny's Log
site_description: 개인 기록 웹사이트
site_author: Juny
site_url: https://example.github.io/

# 폴더 설정
docs_dir: docs
site_dir: site

# 메뉴 구성
nav:
  - 홈: index.md
  - MkDocs:
      - 디자인: 07_mkdocs_yml_디자인.md
      - 치트시트 및 설정: 08_Markdown_치트시트와_mkdocs_yml_옵션.md

# 테마 및 디자인
theme:
  name: material
  language: ko
  palette:
    - scheme: default
      primary: indigo
      accent: amber
      toggle:
        icon: material/brightness-7
        name: 어두운 테마로 전환

    - scheme: slate
      primary: indigo
      accent: amber
      toggle:
        icon: material/brightness-4
        name: 밝은 테마로 전환
  icon:
    logo: material/library
  features:
    - navigation.top
    - content.code.copy
    - content.action.edit
    - search.suggest
    - search.highlight

# 플러그인
plugins:
  - search

# Markdown 확장
markdown_extensions:
  - admonition
  - pymdownx.details
  - pymdownx.superfences
  - pymdownx.tasklist:
      custom_checkbox: true
  - toc:
      permalink: true

# 추가 링크
extra:
  social:
    - icon: fontawesome/brands/github
      link: https://github.com/example

# 저작권 및 저장소
copyright: "© 2026 Juny's Log"

repo_url: https://github.com/example/example.github.io
repo_name: GitHub Repository
edit_uri: edit/main/docs/
```

위 예시는 로고로 내장 아이콘을 사용하므로 별도의 `logo.png` 파일은 필요하지 않다. 파비콘을 직접 등록하려면 이미지를 `docs/` 폴더 아래에 넣고 `theme.favicon`을 추가하면 된다.

### ✅ 설정 확인하기

프로젝트 폴더에서 다음 명령어를 실행한다.

```bash
mkdocs serve
```

브라우저에서 로컬 주소에 접속하여 메뉴와 디자인을 확인할 수 있다. 배포용 파일에 오류가 없는지 확인할 때는 다음 명령어를 사용한다.

```bash
mkdocs build --strict
```

`--strict`는 경고를 오류로 취급하므로 잘못된 링크나 설정을 점검할 때 도움이 된다. 단, 설정과 플러그인 종류에 따라 출력되는 경고는 다를 수 있다.

---

## 📚 참고 자료

- [MkDocs 공식 문서 - Configuration](https://www.mkdocs.org/user-guide/configuration/)
- [Material for MkDocs - 색상 설정](https://squidfunk.github.io/mkdocs-material/setup/changing-the-colors/)
- [Material for MkDocs - 로고와 아이콘](https://squidfunk.github.io/mkdocs-material/setup/changing-the-logo-and-icons/)
- [Material for MkDocs - Markdown 확장](https://squidfunk.github.io/mkdocs-material/setup/extensions/)
- [Material for MkDocs - Admonitions](https://squidfunk.github.io/mkdocs-material/reference/admonitions/)