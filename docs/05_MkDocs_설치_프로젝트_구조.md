# 05. MkDocs 설치와 프로젝트 구조 설정

앞에서 Markdown 문서를 살펴보았습니다. 이번 장에서는 Markdown 파일을 **MkDocs로 HTML 웹페이지로 변환**하고 내 컴퓨터(Local)의 브라우저에서 확인합니다.

> GitHub Pages에도 기본적인 Markdown 처리 기능이 있지만, 여기서는 **MkDocs + Material 테마**를 사용하여 별도의 정적 사이트를 만들어 배포합니다.

## 1. MkDocs 설치

MkDocs는 Markdown을 활용하여 정적 사이트(HTML)를 생성하는 **Python 기반의 문서화 도구**입니다.

Python이 설치되어 있다면 터미널에서 다음 명령으로 설치할 수 있습니다.

```bash
pip install mkdocs-material
```

`mkdocs-material` 패키지는 MkDocs와 Material 테마를 함께 설치합니다.

설치가 되었는지 확인합니다.

```bash
mkdocs --version
```

버전 번호가 출력되면 설치에 성공한 것입니다. `pip` 명령이 동작하지 않는 환경에서는 `python -m pip install mkdocs-material` 형태도 사용할 수 있습니다.

## 2. 프로젝트 폴더 구조

원하는 위치에 프로젝트 폴더를 만듭니다. VS Code에서 **File → Open Folder**로 그 폴더를 열고, 터미널에서 다음을 실행합니다.

```bash
mkdocs new .
```

기본적으로 다음 파일이 만들어집니다.

```text
프로젝트 폴더/
├── docs/
│   └── index.md
└── mkdocs.yml
```

- `docs/`: Markdown 파일을 저장하는 폴더
- `docs/index.md`: 웹사이트 첫 화면의 원본 문서
- `mkdocs.yml`: 웹사이트 이름, 테마, 메뉴 등을 지정하는 설정 파일

이미 준비된 MkDocs 프로젝트 파일이 있다면 다시 초기화할 필요는 없습니다.

## 3. `mkdocs.yml` 설정

**YAML(YML)**은 설정을 작성할 때 자주 사용하는 데이터 형식입니다. 들여쓰기(Indentation)가 의미를 가지므로 정확하게 작성해야 합니다.

`mkdocs.yml` 파일을 열고 다음과 같이 작성합니다.

```yaml
site_name: "Juny's Log"
# site_url: https://<username>.github.io/

theme:
  name: material
  language: ko
```

- `site_name`: 웹사이트에 표시할 제목
- `site_url`: GitHub Pages에 실제 배포할 때 입력할 최종 URL
- `theme.name`: 사용할 테마
- `theme.language`: 테마의 표시 언어

변경 후 저장합니다(`Ctrl + S`; macOS에서는 `Command + S`). 이 실습에서 제공하는 프로젝트의 `mkdocs.yml`에는 **전체 학습 문서용 메뉴(nav)**도 미리 설정되어 있습니다.

## 4. 로컬에서 미리보기

터미널에서 다음을 실행합니다.

```bash
mkdocs serve
```

다음과 비슷한 메시지가 나타납니다.

```text
INFO - Serving on http://127.0.0.1:8000/
```

브라우저에서 [http://127.0.0.1:8000/](http://127.0.0.1:8000/)에 접속하면 `docs/index.md`를 바탕으로 만든 웹페이지를 확인할 수 있습니다.

- Material 테마가 적용되어 나타납니다.
- `docs/index.md`를 수정하고 저장하면 일반적으로 화면이 자동 갱신됩니다.
- 미리보기를 종료할 때는 터미널에서 `Ctrl + C`를 누릅니다.
- 변경 내용이 반영되지 않으면 서버를 종료하고 `mkdocs serve`를 다시 실행합니다.

`mkdocs serve`는 개발 서버를 실행하여 사이트를 미리 보여 줍니다. 변환된 HTML 파일을 **디스크에 저장**하려면 다음 명령을 사용합니다.

```bash
mkdocs build
```

기본적으로 생성된 HTML은 `site/` 폴더에 저장됩니다.

## 5. 테스트(Test)

지금까지 작성한 문서를 `.md` 확장자로 저장하여 `docs/` 안에 복사해 봅니다. 페이지가 탐색 메뉴에 나타나도록 하려면 `mkdocs.yml`의 `nav` 설정에도 해당 파일을 추가하면 됩니다.

## 6. 정리

이제 내 컴퓨터에서 Markdown 기반 웹사이트가 작동하는 것을 확인했습니다. 다음 장에서는 만든 사이트를 **GitHub Pages로 자동 배포**하는 방법을 알아봅니다.
