# 02. GitHub와 GitHub 블로그

## 🐙 GitHub란?

**GitHub**는 코드와 파일을 클라우드(Cloud)에 저장하고, 버전 관리 및 협업을 할 수 있는 플랫폼이다.

Git을 기반으로 작동하며, 프로젝트의 변경 이력을 관리하거나 다른 사람과 코드를 공유할 때 사용한다.

## 📁 1. 저장소(Repository)

**Repository(저장소)**는 코드와 관련 파일을 관리하는 공간이다.

```text
내 컴퓨터(Local) ── push ──▶ GitHub(Remote)
내 컴퓨터(Local) ◀─ pull ─── GitHub(Remote)
```

- **push**: 내 컴퓨터에서 만든 Git 커밋을 GitHub 원격 저장소에 올리는 작업
- **pull**: GitHub 원격 저장소의 변경 내용을 내 컴퓨터로 가져와 반영하는 작업

## 🕒 2. 버전 관리(Version Control)

파일을 수정한 이력을 기록하여 이전 상태로 돌아가거나 변경 과정을 확인할 수 있다.

### Commit

**Commit(커밋)**은 버전 관리의 기본 단위이다.

누가, 언제, 어떤 내용을 변경했는지 기록하며, 이를 통해 과거의 코드 상태를 확인하거나 복구할 수 있다.

### Branch

**Branch(브랜치)**는 기존 코드에서 분리된 작업 흐름이다.

- 현재 서비스 중인 코드에 바로 영향을 주지 않고 변경 사항을 시험할 수 있다.
- 여러 개발자가 서로 다른 기능을 동시에 작업할 수 있다.

### Pull Request와 Merge

별도 Branch에서 작업한 코드를 검토하고, 주 Branch(`main` 등)에 합치는 과정에서 사용한다.

- **Pull Request(PR)**: 다른 Branch에 변경 사항을 병합하도록 제안하는 기능
- **Merge**: 서로 다른 Branch의 변경 사항을 하나로 합치는 작업

## 🤝 3. 협업(Collaboration)

Repository는 **Public(공개)** 또는 **Private(비공개)**로 설정할 수 있다.

| 구분 | 설명 |
| --- | --- |
| Public | 누구나 저장소의 내용을 확인할 수 있음 |
| Private | 접근 권한이 있는 사용자만 저장소를 확인할 수 있음 |

공개 저장소라고 해서 누구나 파일을 직접 수정할 수 있는 것은 아니다. 실제 수정 권한이나 병합 권한은 저장소 설정에 따라 달라진다.

## 🌐 4. GitHub Pages와의 연결

HTML 파일을 GitHub Repository에 올리고 **GitHub Pages**를 설정하면 웹사이트로 공개할 수 있다.

```text
내 컴퓨터의 Calculator.html
           │ GitHub에 업로드
           ▼
       Repository
           │ GitHub Pages 설정
           ▼
https://<username>.github.io/calculator/
```

위 URL은 `calculator`라는 프로젝트 Repository를 사용하는 예시이다.

실제 주소에서는 `<username>`을 GitHub 사용자 이름으로 바꾸어야 한다.