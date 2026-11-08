# Troubleshooting and Glossary

MkDocs 웹사이트를 만들거나 GitHub로 파일을 관리하면서 자주 발생하는 문제와 관련 용어를 정리한다. 오류가 발생하면 **에러 메시지 확인 → 현재 경로와 파일 상태 확인 → 원인에 맞는 수정** 순서로 접근한다.

이 문서의 예시는 **MkDocs + Material for MkDocs + GitHub Pages**를 기준으로 하며, Git의 기본 브랜치는 `main`, 원격 저장소 이름은 `origin`으로 가정한다.

---

## 1. Troubleshooting — 문제 해결

### 먼저 확인할 것

어떤 문제가 발생했는지에 따라 아래 명령어로 상태를 확인한다.

```bash
# MkDocs 설정 및 문서 인식 상태 확인
mkdocs build --verbose

# Git 변경 상태 및 현재 브랜치 확인
git status
git branch --show-current

# 원격 Repository 주소 확인
git remote -v
```

!!! tip "오류 메시지를 그대로 읽기"
    단순히 "작동하지 않는다"고 판단하기보다 터미널에서 `ERROR`, `WARNING`, `fatal`, `rejected` 등이 표시된 **첫 부분**부터 확인한다. 해결 방법을 검색하거나 질문할 때도 해당 메시지를 함께 제시하면 원인을 찾기 쉽다.

### MkDocs / Markdown

#### 새 Markdown 파일이 왼쪽 메뉴에 나타나지 않음

**증상:** 기존 01~06번 문서는 보이는데 새로 추가한 07~09번 파일이 나타나지 않는다.

**확인 방법:**

```bash
mkdocs build --verbose
```

출력 내용을 확인한다.

```text
DEBUG - Reading: Web blog 만들기/06_Git_기초_배포.md
DEBUG - Copying media file: 'Web blog 만들기/07_MkDocs_Design'
```

- `Reading: ... .md` → Markdown 문서로 인식했다는 뜻이다.
- `Copying media file: ...` → 해당 파일을 **정적 파일**로 처리했다는 뜻이다.

위 예시처럼 파일 이름 끝에 `.md`가 없다면 `07_MkDocs_Design.md`로 이름을 바꾸면 된다.

!!! note "VS Code의 Markdown 언어 모드와 확장자는 다름"
    VS Code 화면 오른쪽 아래에 `Markdown`이라고 표시되어도 실제 파일 이름에는 `.md`가 없을 수 있다. **문법 강조를 위한 언어 모드**와 **디스크에 저장된 파일의 확장자**는 별개의 정보다. MkDocs에서 문서로 사용하려면 일반적으로 `.md` 확장자를 붙인다.

확장자가 정확한데도 나타나지 않는다면 다음을 확인한다.

1. 문서가 `mkdocs.yml`에서 지정한 `docs_dir`(기본값 `docs/`) 안에 있는지 확인한다.
2. `mkdocs.yml`에 `nav`가 있다면 새 문서가 포함되어 있는지 확인한다. `nav`가 없다면 메뉴는 자동으로 만들어진다.
3. `exclude_docs`, `draft_docs`, `not_in_nav` 등 파일 표시와 관계된 설정이 있는지 확인한다.
4. 서버를 종료한 뒤 `mkdocs serve`를 다시 실행한다.

#### `mkdocs.yml` 오류 또는 설정이 반영되지 않음

YAML은 **공백 들여쓰기**에 따라 구조가 결정된다. 탭 대신 공백을 사용하고, `theme`, `nav` 같은 같은 수준의 키를 잘못 중복 작성하지 않았는지 확인한다.

```yaml
site_name: Juny's Log

theme:
  name: material
  language: ko
```

변경 후에는 다음 명령어로 빌드 오류를 확인할 수 있다.

```bash
mkdocs build
```

#### 이미지 또는 CSS가 적용되지 않음

예를 들어 아래 설정이 있다면:

```yaml
extra_css:
  - stylesheets/extra.css
```

파일이 실제로 `docs/stylesheets/extra.css`에 있어야 한다. 마찬가지로 `![배너](images/mainpage.png)`를 사용한다면 Markdown 파일의 위치를 기준으로 `images/mainpage.png`가 존재하는지 확인한다. **Markdown의 상대 경로와 `extra_css`에 지정하는 경로의 기준은 같지 않을 수 있다.**

브라우저의 캐시 때문에 이전 디자인이 보일 수도 있으므로 강력 새로고침을 해 본다.

#### `mkdocs serve` 명령어를 찾지 못함

```text
'mkdocs' is not recognized ...
```

MkDocs가 설치되지 않았거나 현재 터미널의 Python 환경에서 실행 경로를 찾지 못했을 수 있다.

```bash
python -m pip install mkdocs-material
python -m mkdocs --version
python -m mkdocs serve
```

가상환경을 사용하고 있다면 먼저 해당 환경을 활성화한다. 운영체제나 설치 방식에 따라 Python 실행 명령어가 `py` 또는 `python3`일 수도 있다.

### Git / GitHub

#### `fatal: not a git repository`

현재 터미널이 Git 저장소 외부에서 실행되었을 때 발생한다.

```bash
pwd
git status
```

실제 프로젝트 폴더로 이동해 다시 실행한다. 아직 Git 저장소를 만들지 않은 **새 프로젝트**라면 `git init`을 사용할 수 있지만, 이미 GitHub에 있는 프로젝트라면 대개 `git clone`을 이용하는 편이 안전하다.

#### `remote origin already exists`

이미 `origin`이라는 원격 저장소가 등록되어 있다는 뜻이다.

```bash
git remote -v
```

등록된 주소가 맞다면 그대로 사용한다. 틀렸다면 주소를 확인한 뒤 수정한다.

```bash
git remote set-url origin https://github.com/USERNAME/REPOSITORY.git
```

#### `nothing to commit, working tree clean`

현재 브랜치에 Commit할 변경 사항이 없다는 뜻이다. 오류가 아니다. 파일을 저장했는지, 실제 Git 저장소 안에서 수정했는지 확인한다.

새 파일이 추가되지 않는다면 `.gitignore`에 의해 제외된 파일인지 살펴본다.

```bash
git status
git check-ignore -v 경로/파일명
```

`git check-ignore -v`는 일치하는 제외 규칙을 찾을 때 사용할 수 있다.

#### `src refspec main does not match any`

`main`이라는 로컬 브랜치가 없거나, 첫 Commit이 아직 만들어지지 않은 경우에 발생할 수 있다.

```bash
git branch --show-current
git log -1 --oneline
```

브랜치 이름이 다른지, Commit이 있는지 먼저 확인한다. 새 저장소라면 파일을 Stage에 추가하고 첫 Commit을 만든 뒤 Push한다.

#### `rejected (non-fast-forward)` / `failed to push some refs`

원격 브랜치에 로컬에 없는 Commit이 있어 기존 기록을 덮어쓸 수 없을 때 Push가 거부될 수 있다. 다른 컴퓨터나 GitHub 웹에서 수정한 내용을 로컬로 반영해야 한다.

```bash
git status
git fetch origin
git log --oneline --graph --all
```

변경 이력을 확인한 뒤 필요한 경우 `git merge origin/main` 등으로 원격의 변경 사항을 반영한다. 충돌이 없다면 이후 `git push origin main`을 실행한다.

!!! warning "강제 Push 주의"
    문제를 해결하려고 `git push --force`를 무작정 실행하면 다른 사람이 올린 Commit을 덮어쓸 수 있다. 먼저 원격 기록을 확인한다.

#### Merge conflict (병합 충돌)

같은 부분을 서로 다르게 수정해서 Git이 어떤 내용을 남길지 판단할 수 없는 상태다.

```text
<<<<<<< HEAD
내가 수정한 내용
=======
다른 브랜치에서 수정한 내용
>>>>>>> design
```

충돌 파일을 열어 최종적으로 남길 내용을 선택하고, `<<<<<<<`, `=======`, `>>>>>>>` 같은 표시를 제거한 뒤 저장한다. 이후 `git add <파일명>`으로 해결되었음을 표시하고, 진행 중이던 Merge를 마무리한다. 병합을 처음부터 취소하고 싶다면 진행 중인 작업 상태를 확인한 뒤 `git merge --abort`를 검토한다.

#### GitHub에 Push했는데 웹사이트가 바뀌지 않음

GitHub 저장소에 Commit이 올라갔다는 사실과 **GitHub Pages 배포가 완료되었다는 사실은 다르다.**

1. GitHub Repository의 **Actions** 탭에서 Workflow 실행 결과를 확인한다.
2. `Settings → Pages`에서 Source가 현재 배포 방식과 맞는지 확인한다.
3. GitHub Actions 직접 배포 방식이라면 Source를 `GitHub Actions`로 설정한다.
4. Workflow에서 `mkdocs build`가 성공하고, 결과물인 `site/` 폴더를 올바르게 업로드했는지 확인한다.
5. 사이트 주소와 `mkdocs.yml`의 `site_url`을 확인한다. 사용자 사이트와 프로젝트 사이트의 URL 경로는 다르다.

!!! note "두 가지 배포 방식을 혼용하지 않기"
    `mkdocs gh-deploy`로 `gh-pages` 브랜치에 올리는 방법과 GitHub Actions에서 `site/` 빌드 결과를 직접 배포하는 방법은 설정이 다르다. 사용 중인 배포 방식에 맞게 Pages Source를 선택한다.

---

## 2. Glossary — 용어 정리

### Git / GitHub

| 용어 | 설명 |
| --- | --- |
| Git | 소스 코드와 파일의 변경 기록을 관리하는 버전 관리 도구 |
| GitHub | Git 저장소를 호스팅하고 협업 기능을 제공하는 서비스 |
| Repository (Repo) | 프로젝트 파일과 Git 변경 이력이 저장되는 공간 |
| Local Repository | 내 컴퓨터에 있는 Git 저장소 |
| Remote Repository | GitHub 등 서버에 있는 Git 저장소 |
| Working Directory | 실제 파일을 수정하는 작업 폴더 |
| Staging Area / Index | 다음 Commit에 포함할 변경 사항을 선택해 두는 공간 |
| Commit | 선택한 변경 사항을 하나의 이력으로 기록한 것 |
| Commit Hash | 각 Commit을 구분하는 고유 ID |
| HEAD | 현재 체크아웃된 Commit 또는 브랜치 위치를 가리키는 참조 |
| Branch | 독립적으로 작업 이력을 진행하기 위한 이름 붙은 포인터 |
| main | 자주 사용하는 기본 브랜치 이름 |
| origin | 원격 저장소에 일반적으로 붙이는 기본 별칭 |
| upstream | 원격 추적 관계 또는 Fork의 원본 저장소에 자주 사용하는 이름. 문맥에 따라 의미가 다름 |
| Clone | 원격 Repository를 로컬에 복제하는 작업 |
| Fetch | 원격 변경 이력을 가져오되 현재 작업 브랜치에 자동 병합하지 않는 작업 |
| Pull | 원격 변경을 가져와 현재 브랜치에 통합하는 작업 (설정에 따라 Merge 또는 Rebase) |
| Push | 로컬 Commit을 원격 Repository로 보내는 작업 |
| Merge | 서로 다른 브랜치의 변경 이력을 합치는 작업 |
| Merge Conflict | 서로 충돌하는 변경 사항을 Git이 자동으로 합치지 못한 상태 |
| Pull Request (PR) | 다른 브랜치나 저장소에 변경 사항 병합을 제안하는 기능 |
| Issue | 버그, 개선 사항, 질문, 할 일을 기록·관리하는 기능 |
| Fork | 다른 사람의 GitHub Repository를 내 계정의 별도 Repository로 만드는 기능 |
| Star | GitHub Repository에 관심을 표시하고 목록에 저장하는 기능 |
| Watch | Repository의 활동에 대한 알림을 구독하는 기능 |
| `.gitignore` | Git이 추적하지 않을 파일·경로를 지정하는 파일 |

### VS Code 상태 표시

| 표시 | 의미 |
| --- | --- |
| `U` | VS Code 탐색기에서 주로 **Untracked**, 아직 Git이 추적하지 않는 파일 |
| `M` | Modified, 기존 추적 파일의 수정 사항 |
| `A` | Added, 새 파일이 Stage에 추가된 상태 |
| `D` | Deleted, 추적 파일이 삭제된 상태 |

!!! warning "상태 코드의 문맥 구분"
    위 표는 **VS Code 탐색기에서 흔히 보이는 표시**를 설명한 것이다. 터미널에서 병합 충돌을 확인할 때의 `U`는 Unmerged 상태를 가리킬 수 있으므로 `git status`로 정확한 의미를 확인한다.

### MkDocs / 웹사이트

| 용어 | 설명 |
| --- | --- |
| MkDocs | Markdown 파일을 정적 웹사이트로 만드는 문서 사이트 생성 도구 |
| Material for MkDocs | MkDocs에 디자인과 추가 기능을 제공하는 테마 |
| Markdown | 문서의 구조와 서식을 표현하는 경량 마크업 언어 |
| `mkdocs.yml` | 사이트 이름, 테마, 메뉴, 플러그인 등을 설정하는 YAML 파일 |
| `docs_dir` | 원본 Markdown 문서를 읽는 폴더 (기본값 `docs`) |
| `site_dir` | 생성된 HTML 등 웹사이트 결과물을 저장하는 폴더 (기본값 `site`) |
| `nav` | 사이트의 탐색 메뉴 구성을 정의하는 설정 |
| `theme` | 사이트의 디자인과 관련된 설정 |
| `plugins` | MkDocs 빌드 과정에 추가 기능을 붙이는 설정 |
| `markdown_extensions` | Markdown 해석·렌더링 기능을 확장하는 설정 |
| Admonition | `!!! note` 등으로 작성하는 안내·경고·팁 박스 |
| Localhost | 자기 컴퓨터에서 실행 중인 로컬 서버의 주소를 가리킬 때 사용하는 이름 |
| Build | Markdown 등 원본 파일을 HTML·CSS 등 배포 결과물로 생성하는 과정 |
| Deploy | 빌드된 결과물을 웹 서버 또는 호스팅 서비스에 게시하는 과정 |
| Static Site | 페이지 요청 시 서버에서 매번 내용을 계산하지 않고 미리 생성한 파일을 제공하는 웹사이트 |
| GitHub Pages | GitHub Repository와 연결해 정적 웹사이트를 게시하는 서비스 |
| GitHub Actions | Push 등 이벤트를 계기로 빌드·테스트·배포 작업을 자동 수행하는 서비스 |
| Workflow | GitHub Actions에서 자동으로 실행할 작업의 규칙을 정의한 YAML 파일 |
| Artifact | 빌드 과정에서 생성하거나 다음 작업에 전달하기 위해 보관한 파일 묶음 |

---

## 3. 빠른 점검 순서

문제가 생겼을 때는 다음 순서로 확인한다.

1. **현재 경로:** 터미널이 의도한 프로젝트 폴더를 가리키는지 확인
2. **파일 이름:** 실제 확장자가 `.md`인지 확인 (VS Code 언어 모드와 구분)
3. **설정:** `mkdocs.yml`, `docs_dir`, `nav`, 상대 경로 확인
4. **빌드:** `mkdocs build --verbose` 로그에서 `Reading` / `ERROR` 등을 확인
5. **Git 상태:** `git status`, `git remote -v`, 현재 브랜치 확인
6. **배포:** GitHub Actions 로그와 Pages Source 설정 확인

---

## 참고 자료

- [MkDocs 공식 설정 문서](https://www.mkdocs.org/user-guide/configuration/)
- [Git 공식 명령어 문서](https://git-scm.com/docs)
- [GitHub Docs — Git 사용하기](https://docs.github.com/en/get-started/using-git)
- [GitHub Docs — GitHub Pages 배포 설정](https://docs.github.com/en/pages/getting-started-with-github-pages/configuring-a-publishing-source-for-your-github-pages-site)
