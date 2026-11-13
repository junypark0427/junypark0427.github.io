# 09. GitHub 더 알아보기

GitHub에서 파일을 업로드하는 방법뿐 아니라, 터미널을 이용해 **Local Repository와 원격 Repository를 관리하고 협업하는 방법**을 정리한다.

예시에서는 기본 브랜치 이름을 `main`, 원격 저장소 이름을 `origin`으로 가정한다. 실제 Repository에서 다른 이름을 사용한다면 해당 이름으로 바꿔 입력한다.

---

## 💻 1. 터미널로 Git 다루기

VSCode에서 **Terminal → New Terminal**을 선택하면 현재 작업 폴더를 기준으로 터미널을 열 수 있다. 명령어를 실행하기 전에는 현재 경로와 Repository 상태부터 확인한다.

```bash
pwd                 # 현재 폴더 확인 (Windows PowerShell에서도 사용 가능)
git status          # 변경된 파일 및 현재 브랜치 확인
git remote -v       # 연결된 원격 Repository 확인
```

!!! note "Git과 GitHub의 차이"

    **Git**은 파일 변경 이력을 관리하는 도구이고, **GitHub**는 Git Repository를 인터넷에 저장하고 공유할 수 있는 서비스이다. `commit`은 Local에 기록하는 작업이고, `push`는 기록한 Commit을 원격 Repository로 보내는 작업이다.

### 🔗 원격 Repository와 Local 폴더 일치시키기

작업 폴더의 상태에 따라 방법이 달라진다. **이미 GitHub에 파일이 있는지**, **현재 폴더가 Git Repository인지**를 먼저 확인해야 한다.

#### 1. VSCode에서 새로운 폴더를 열었을 경우

GitHub에 이미 Repository가 있고, 아직 Local에서 작업을 시작하지 않았다면 `clone`을 사용한다.

1. GitHub의 Repository 페이지에서 **Code → HTTPS** 주소를 복사한다.
2. VSCode 터미널에서 Repository를 저장할 **상위 폴더**로 이동한다.
3. 다음 명령어를 실행한다.

```bash
# 예시: GitHub에 있는 Repository를 현재 위치에 복제
git clone https://github.com/USERNAME/REPOSITORY.git

# 복제된 폴더로 이동
cd REPOSITORY
```

`git clone`을 실행하면 Repository 이름의 폴더가 만들어지고, 원격 파일·Commit 기록·원격 연결(`origin`)이 함께 설정된다. 이후 VSCode에서 복제된 폴더를 열어 작업한다.

!!! tip "이미 빈 폴더를 열어 둔 경우"

    **정말 아무 파일도 없는 빈 폴더**에서 `git clone <URL> .`을 실행하면 새 하위 폴더를 만들지 않고 현재 폴더에 복제할 수도 있다. 기존 파일이 있다면 이 방법 대신 별도 폴더에 복제하는 편이 안전하다.

#### 2. VSCode에서 기존에 작업 폴더가 있을 경우

**A. 이미 GitHub Repository와 연결된 폴더**

먼저 연결 상태를 확인한다.

```bash
git status
git remote -v
git branch --show-current
```

`origin`에 원하는 GitHub Repository 주소가 표시되고, 현재 브랜치가 `main`이라면 다음과 같이 최신 변경 내용을 가져온다.

```bash
git fetch origin
git pull --ff-only origin main
```

- `fetch`: 원격 변경 이력을 받아오되 현재 작업 파일에는 바로 합치지 않는다.
- `pull`: 원격 변경 사항을 현재 브랜치에 반영한다.
- `--ff-only`: 로컬과 원격의 기록이 서로 갈라져 자동으로 합칠 수 없으면 작업을 중단한다.

작업 중인 파일에 저장되지 않은 변경 사항이나 Commit되지 않은 수정 사항이 있다면 먼저 확인하고 Commit하거나 별도로 백업한다. `pull`에서 오류가 나면 무작정 강제 덮어쓰기를 하지 않는다.

!!! tip "현재 폴더가 어떤 Repository와 연결되어 있는지 확인"

    `git remote -v`로 `origin`의 주소를 확인할 수 있다. 주소가 잘못 연결되어 있다면, **정확한 Repository 주소를 확인한 뒤** 다음 명령어로 변경할 수 있다.

    ```bash
    git remote set-url origin https://github.com/USERNAME/REPOSITORY.git
    git remote -v
    ```

    원격 주소를 바꾸는 작업은 파일 내용을 업로드하지 않는다. 업로드하려면 별도로 `git push`를 실행해야 한다.

**B. 기존 작업 폴더이지만 아직 Git과 연결되지 않은 경우**

GitHub Repository에도 이미 파일과 Commit 기록이 있다면 **해당 Repository를 다른 위치에 `clone`한 뒤**, 기존 작업 파일을 복제된 폴더에 옮기는 방법이 가장 이해하기 쉽고 안전하다.

1. 원래 작업 폴더를 백업한다.
2. `git clone <Repository URL>`로 새 폴더를 만든다.
3. 기존 파일을 복제된 폴더에 복사하고 변경 내용을 확인한다. (`.git` 폴더는 복사하지 않는다.)
4. 필요한 파일만 Commit하고 Push한다.

이렇게 하면 GitHub에 이미 있던 Commit 기록을 유지하면서 기존 파일을 반영할 수 있다.

**C. 이미 Git 저장소이지만 원격 주소가 없는 경우**

`git status`는 정상적으로 작동하지만 `git remote -v`에 아무것도 표시되지 않는 상황이다. **GitHub에 만든 원격 Repository가 비어 있을 때**는 다음처럼 연결할 수 있다.

```bash
git remote add origin https://github.com/USERNAME/REPOSITORY.git
git branch --show-current
git push -u origin main
```

마지막 명령어는 현재 브랜치가 `main`일 때의 예시다. 원격에 이미 Commit이 있다면 먼저 원격의 기록을 확인하고, 2번 B처럼 복제 후 파일을 옮기는 방식을 고려한다.

!!! warning "주의"

    내용이 있는 GitHub Repository와 별도로 만들어진 Local Repository를 무리하게 합치면 `unrelated histories` 오류가 발생할 수 있다. 이때 `--force`나 `reset --hard`를 곧바로 사용하는 것은 권장하지 않는다. 먼저 파일과 Commit 기록이 어느 쪽에 있는지 확인한다.

#### 3. 현재 VSCode의 폴더 내용을 그대로 GitHub에 올리기

**GitHub에서 새로 만든 빈 Repository**에 현재 Local 폴더를 처음 업로드하는 상황이다. GitHub에서 Repository를 만들 때 README 등으로 첫 Commit을 생성하지 않았다면 다음 순서로 진행할 수 있다.

```bash
# 현재 폴더를 Git Repository로 초기화
git init

# Git에 추가할 파일 확인 후 스테이징
git status
git add .

# 변경 이력 기록
git commit -m "Initial commit"

# 기본 브랜치를 main으로 지정
git branch -M main

# GitHub Repository 연결
git remote add origin https://github.com/USERNAME/REPOSITORY.git

# 처음 업로드하고 추적 브랜치 설정
git push -u origin main
```

한 번 연결한 다음부터는 보통 아래 명령어만 사용하면 된다.

```bash
git status
git add .
git commit -m "Update files"
git push
```

!!! warning "업로드 전 확인"

    `git add .`은 현재 폴더의 변경 사항을 넓게 추가하므로 `.env`, 비밀번호, API Key, 개인 파일 등이 포함되지 않았는지 확인한다. 올리지 않을 파일이나 빌드 결과물은 `.gitignore`에 등록한다. **원격 Repository에 이미 Commit이 있다면** 위의 `git init` → `push` 과정을 그대로 적용하지 말고 1번 또는 2번 방법으로 진행한다.

### 🕒 버전 관리 (Commit 히스토리 보기)

Git은 파일의 변경 내역을 **Commit 단위**로 기록한다. 이전에 어떤 파일을 수정했는지 확인하거나 특정 변경 사항을 살펴볼 수 있다.

```bash
git log                             # 자세한 Commit 기록
git log --oneline                   # 한 줄씩 간략하게 표시
git log --oneline --graph --all     # 브랜치를 포함한 Commit 흐름 확인
git log -5 --oneline --decorate     # 최근 5개의 Commit과 브랜치 이름 표시
git show HEAD                       # 가장 최근 Commit의 내용 확인
git diff                            # 아직 Stage에 추가하지 않은 수정 확인
git diff --staged                   # Stage에 추가한 수정 확인
```

예를 들어 `git log --oneline`의 결과는 다음과 같을 수 있다.

```text
c8d20a1 Update MkDocs notes
7a3e921 Add GitHub chapter
b6f4e05 Initial commit
```

맨 앞의 문자는 각 Commit을 구분하는 ID의 일부이다. 특정 Commit의 변경 내용을 보고 싶다면 다음처럼 입력한다.

```bash
git show 7a3e921
```

GitHub 웹사이트에서도 Repository의 **Commits** 기록을 눌러 변경 내역을 확인할 수 있다.

!!! tip "이전 변경을 취소할 때"

    아직 Commit하지 않은 파일을 이전 상태로 되돌릴 때는 `git restore <파일명>`을 사용할 수 있지만 **해당 파일의 수정 내용이 사라질 수 있다.** 이미 공유한 Commit을 취소할 때는 새 취소 Commit을 만드는 `git revert <Commit ID>`를 검토한다. 작업 전 차이와 백업을 먼저 확인한다.

### 🌿 Branch

Branch는 **작업을 분기해서 독립적으로 진행할 수 있게 해주는 기능**이다. 기본 브랜치인 `main`을 유지하면서 새로운 기능이나 디자인을 따로 개발할 수 있다.

#### Branch를 사용하는 이유

- 기존 코드에 영향을 최소화하면서 새 기능을 실험할 수 있다.
- 여러 사람이 각자 다른 기능을 병렬로 개발할 수 있다.
- 작업이 완료되면 검토 후 `main`에 합칠 수 있다.
- 기능별 변경 이력을 구분하기 쉽다.

예를 들어 MkDocs 사이트의 디자인을 바꾸는 작업을 `design` 브랜치에서 진행할 수 있다.

```bash
git switch main                 # main 브랜치로 이동
git pull --ff-only origin main  # 최신 상태 확인
git switch -c design            # design 브랜치 생성 후 이동

# 파일을 수정한 뒤
git add .
git commit -m "Update website design"
git push -u origin design       # 원격에 새 브랜치 업로드
```

자주 사용하는 Branch 명령어는 다음과 같다.

| 명령어 | 역할 |
| --- | --- |
| `git branch` | Local 브랜치 목록 보기 |
| `git branch -a` | Local과 원격 브랜치 목록 보기 |
| `git switch <브랜치명>` | 다른 브랜치로 이동 |
| `git switch -c <새이름>` | 새 브랜치를 만들고 이동 |
| `git push -u origin <브랜치명>` | 새 브랜치를 GitHub에 올리고 추적 관계 설정 |
| `git branch -d <브랜치명>` | 병합된 Local 브랜치 삭제 |

!!! note "Branch와 폴더의 차이"

    Branch는 폴더를 복제하는 것이 아니라 **서로 다른 작업 이력을 관리하는 방식**이다. 브랜치를 전환하면 현재 폴더에서 보이는 파일 상태도 해당 브랜치에 맞게 바뀔 수 있다. 아직 Commit하지 않은 변경 사항이 있으면 브랜치 전환이 거부되거나 작업에 영향을 줄 수 있으므로 먼저 `git status`로 확인한다.

#### 작업한 Branch를 합치기

`design` 브랜치의 작업을 `main`에 반영하는 방법 중 하나는 로컬에서 `merge`하는 것이다.

```bash
git switch main
git pull --ff-only origin main
git merge design
git push origin main
```

`merge` 중 같은 부분을 서로 다르게 수정했다면 충돌(Conflict)이 발생할 수 있다. 이때는 파일을 확인해 충돌을 해결해야 한다. 팀 프로젝트에서는 직접 `merge`하기보다 **Pull Request로 변경 사항을 검토한 뒤 병합하는 방식**도 많이 사용한다.

### 🔀 Pull Request, Issues

#### Pull Request (PR)

**Pull Request**는 한 브랜치의 변경 사항을 다른 브랜치에 합치자고 제안하는 기능이다. 팀원이 변경된 코드를 검토하고 의견을 남길 수 있다.

일반적인 순서는 다음과 같다.

1. `main`이 아닌 새 브랜치에서 작업한다.
2. Commit 후 해당 브랜치를 GitHub에 Push한다.
3. GitHub Repository의 **Pull requests → New pull request**로 이동한다.
4. 합칠 대상(`base: main`)과 작업 브랜치(`compare: design`)를 확인한다.
5. 변경 내용과 목적을 작성해 PR을 생성한다.
6. 리뷰와 테스트를 거친 뒤 문제가 없다면 Merge한다.

Pull Request는 여러 명이 개발할 때 특히 유용하지만, 혼자 개발하더라도 작업 내용을 검토하거나 변경 이력을 정리할 목적으로 사용할 수 있다.

#### Issues

**Issues**는 해야 할 작업, 버그, 개선 사항, 질문 등을 기록하고 관리하는 기능이다.

예를 들어 다음과 같이 작성할 수 있다.

```text
제목: 모바일 화면에서 메뉴가 겹치는 문제

문제:
모바일 환경에서 상단 메뉴가 본문과 겹친다.

할 일:
- [ ] 문제 발생 조건 확인
- [ ] CSS 수정
- [ ] 모바일 화면에서 다시 테스트
```

Issue에는 담당자(`Assignee`), 분류용 `Label`, 일정용 `Milestone` 등을 설정할 수 있다. PR 본문에 `Closes #12`처럼 작성하면 PR이 기본 브랜치에 병합될 때 연결된 Issue를 자동으로 닫도록 할 수도 있다.

### ⭐ Fork, Star, Watch

GitHub Repository의 오른쪽 위에서 자주 볼 수 있는 기능이다.

| 기능 | 의미 | 주로 사용하는 상황 |
| --- | --- | --- |
| **Fork** | 다른 사람의 Repository를 자신의 계정 아래에 복사해 독립적으로 관리 | 오픈소스 프로젝트 수정 및 기여 |
| **Star** | 관심 있는 Repository를 저장하고 관심을 표시 | 나중에 다시 찾아보거나 프로젝트를 응원할 때 |
| **Watch** | Repository 활동의 알림을 구독 | 새 Issue, PR, Release 등의 업데이트를 확인할 때 |

`clone`과 `fork`는 서로 다르다.

- **Clone:** Repository를 내 컴퓨터(Local)에 복제한다.
- **Fork:** GitHub의 Repository를 내 GitHub 계정 아래에 별도로 만든다.

보통 오픈소스에 기여할 때는 원본 Repository를 Fork하고, Fork한 Repository를 Clone한 다음 변경 사항을 Push하여 원본 프로젝트에 PR을 보낸다.

### 🌍 오픈소스와 GitHub

**오픈소스(Open Source)**는 일정한 라이선스에 따라 소스 코드를 공개하고, 다른 사람이 사용·수정·배포할 수 있도록 한 소프트웨어를 말한다. GitHub에는 다양한 오픈소스 프로젝트의 코드와 개발 기록이 공개되어 있다.

GitHub를 통해 다른 사람의 코드를 살펴보거나, 버그를 제보하고, 직접 수정한 내용을 제안할 수 있다.

오픈소스 프로젝트에서 자주 확인하는 파일은 다음과 같다.

| 파일 | 역할 |
| --- | --- |
| `README.md` | 프로젝트 소개, 설치·실행 방법 |
| `LICENSE` | 사용·수정·배포에 관한 라이선스 조건 |
| `CONTRIBUTING.md` | 프로젝트에 기여하는 방법과 규칙 |
| `CODE_OF_CONDUCT.md` | 프로젝트 참여자 간의 행동 규칙 |
| `.gitignore` | Git으로 추적하지 않을 파일과 폴더 지정 |

!!! warning "코드가 공개되어 있다고 모두 자유롭게 사용할 수 있는 것은 아니다"

    공개된 Repository라도 **라이선스가 없으면** 해당 코드를 마음대로 복제·수정·배포해도 된다는 뜻이 아니다. 사용 전 `LICENSE`와 관련 규칙을 확인한다.

### 👤 GitHub Profile

GitHub Profile은 자신이 관리하는 Repository, Commit 활동, 참여 프로젝트 등을 보여주는 페이지이다. 프로필에는 소개글을 작성하거나 대표 Repository를 고정(`Pin`)할 수 있다.

또한 **Profile README**를 사용하면 프로필 상단에 Markdown으로 작성한 소개 페이지를 표시할 수 있다.

#### Profile README 만들기

1. GitHub에서 새로운 Repository를 생성한다.
2. **Repository 이름을 자신의 GitHub 사용자 이름과 완전히 동일하게** 설정한다.
3. 공개 범위를 `Public`으로 설정한다.
4. Repository의 최상위 경로에 `README.md`를 작성한다.
5. 프로필로 이동해 표시되는지 확인한다.

```text
USERNAME/                 ← Repository 이름이 사용자 이름과 같음
└── README.md             ← 프로필 상단에 표시되는 내용
```

!!! note "GitHub Profile과 GitHub Pages의 차이"

    `USERNAME`이라는 이름의 Repository는 **Profile README**를 위한 것이고, `USERNAME.github.io`라는 이름의 Repository는 **GitHub Pages 사용자 웹사이트**를 만들 때 사용한다. 두 기능은 목적이 서로 다르다.

GitHub Profile에는 진행했던 프로젝트의 Repository를 고정하고, 프로젝트마다 적절한 `README.md`를 작성해 두면 다른 사람이 작업 내용을 이해하기 쉽다.

---

## 📋 2. 자주 쓰는 Git 명령어

다음은 터미널에서 자주 사용하는 명령어를 정리한 표이다.

| 명령어 | 설명 |
| --- | --- |
| `git --version` | 설치된 Git 버전 확인 |
| `git init` | 현재 폴더를 Git Repository로 초기화 |
| `git clone <URL>` | 원격 Repository 복제 |
| `git status` | 현재 파일 변경 상태 확인 |
| `git status -sb` | 현재 브랜치와 변경 상태를 간략히 확인 |
| `git add <파일명>` | 특정 파일을 Stage에 추가 |
| `git add .` | 현재 경로의 변경 사항을 Stage에 추가 |
| `git commit -m "메시지"` | 변경 사항을 Commit으로 기록 |
| `git log --oneline` | Commit 기록 간단히 확인 |
| `git diff` | Stage에 추가하지 않은 변경 사항 비교 |
| `git diff --staged` | Stage에 추가된 변경 사항 비교 |
| `git show <Commit ID>` | 특정 Commit 확인 |
| `git remote -v` | 원격 Repository 연결 정보 확인 |
| `git remote add origin <URL>` | 원격 Repository 연결 추가 |
| `git remote set-url origin <URL>` | 기존 `origin` 주소 변경 |
| `git fetch origin` | 원격 변경 이력 가져오기 (자동 병합하지 않음) |
| `git pull --ff-only origin main` | Fast-forward가 가능한 경우에만 원격 `main` 반영 |
| `git push origin main` | Local `main`을 GitHub에 업로드 |
| `git push -u origin <브랜치명>` | 원격 브랜치 생성 및 추적 관계 설정 |
| `git branch` | Local 브랜치 목록 확인 |
| `git branch -vv` | 브랜치와 원격 추적 정보 확인 |
| `git switch <브랜치명>` | 브랜치 이동 |
| `git switch -c <새이름>` | 새 브랜치 생성 및 이동 |
| `git merge <브랜치명>` | 지정한 브랜치를 현재 브랜치에 병합 |
| `git restore <파일명>` | 아직 Stage에 올리지 않은 수정 되돌리기 (**주의**) |
| `git restore --staged <파일명>` | Stage에 올린 변경을 Stage에서 내리기 (작업 파일은 유지) |
| `git revert <Commit ID>` | 기존 Commit을 되돌리는 새 Commit 생성 |

### 🔄 기본 작업 흐름

이미 GitHub와 연결된 Repository에서 작업한다면 다음과 같은 순서로 진행할 수 있다.

```bash
# 1. 최신 내용 가져오기 (현재 main에 있고, 작업 상태가 정리된 경우)
git pull --ff-only origin main

# 2. 파일 수정 후 변경 확인
git status

# 3. 변경 사항 기록
git add .
git commit -m "Update documents"

# 4. GitHub에 반영
git push origin main
```

!!! tip "명령어를 외우는 것보다 중요한 점"

    `status`로 상태를 확인하고, `add`로 기록할 파일을 선택한 다음, `commit`으로 저장하고, `push`로 GitHub에 올리는 흐름을 이해하는 것이 중요하다. 다른 컴퓨터나 팀원이 수정한 내용이 있을 때는 먼저 `fetch` 또는 `pull`로 원격 상태를 확인한다.

---

### 🏷️ VS Code 탐색기의 Git 표시

VS Code 탐색기에서 보이는 상태 표시는 Git이 파일을 어떻게 관리하는지 알려준다.

| 표시 | 의미 |
| --- | --- |
| `U` | Untracked: 아직 Git으로 추적하지 않은 새 파일 |
| `M` | Modified: 기존 파일이 수정됨 |
| `A` | Added: 파일이 Stage에 추가됨 |
| `D` | Deleted: 파일이 삭제됨 |

새 파일에 `U`가 나타났다면 `git add <파일명>`으로 Stage에 추가하고 Commit하면 된다. 다만 **VS Code의 `U`(Untracked)와 Git 충돌 메시지의 `U`(Unmerged)는 문맥이 다르다.** 정확한 상태는 `git status`로 확인한다.

---

## 📚 참고 자료

- [Git 공식 명령어 문서](https://git-scm.com/docs)
- [GitHub Docs — 원격 Repository 변경 사항 가져오기](https://docs.github.com/en/get-started/using-git/getting-changes-from-a-remote-repository)
- [GitHub Docs — Pull Request](https://docs.github.com/en/pull-requests/get-started/about-pull-requests)
- [GitHub Docs — Profile README](https://docs.github.com/en/account-and-profile/how-tos/profile-customization/managing-your-profile-readme)