# 02. GitHub와 GitHub 블로그

## GitHub란?

**GitHub**는 코드와 파일을 클라우드(Cloud)에 저장하고, 버전 관리 및 협업을 할 수 있는 플랫폼입니다.

## 1. 저장소(Repository)

**Repository(저장소)**는 코드와 관련 파일을 관리하는 공간입니다.

```text
내 컴퓨터(Local) ── push ──▶ GitHub(Remote)
내 컴퓨터(Local) ◀─ pull ─── GitHub(Remote)
```

- **push**: 내 컴퓨터에서 만든 Git 커밋을 GitHub 원격 저장소에 올립니다.
- **pull**: GitHub 원격 저장소의 변경 내용을 내 컴퓨터로 가져와 반영합니다.

## 2. 버전 관리(Version Control)

파일을 수정한 이력을 기록하여 이전 상태로 돌아가거나 변경 과정을 확인할 수 있습니다.

### Commit

커밋(Commit)은 버전 관리의 기본 단위입니다. **누가, 언제, 어떤 내용을 변경했는지** 이력으로 남길 수 있습니다. 이를 통해 과거의 코드 상태를 확인하고 복구할 수 있습니다.

### Branch

브랜치(Branch)는 기존 코드에서 분리된 작업 흐름입니다.

- 현재 서비스 중인 코드에 바로 영향을 주지 않고 변경 사항을 시험할 수 있습니다.
- 여러 개발자가 서로 다른 기능을 동시에 작업할 수 있습니다.

### Pull Request와 Merge

별도 브랜치에서 작업한 코드를 검토하고, 주 브랜치(main 등)에 합치는 과정에서 **Pull Request(변경 제안)**와 **Merge(병합)**를 사용합니다.

## 3. 협업(Collaboration)

저장소는 **Public(공개)** 또는 **Private(비공개)**로 설정할 수 있습니다. 공개 저장소는 누구나 내용을 볼 수 있으며, 실제 수정 권한이나 병합 권한은 저장소 설정에 따라 달라집니다.

## 4. GitHub Pages와의 연결

HTML 파일을 GitHub 저장소에 올리고 **GitHub Pages**를 설정하면 웹에서 공개할 수 있습니다.

```text
내 컴퓨터의 Calculator.html
       │ GitHub에 업로드
       ▼
Repository
       │ GitHub Pages 설정
       ▼
https://<username>.github.io/calculator/
```

> 위 URL은 `calculator`라는 프로젝트 저장소를 사용하는 예시입니다. 실제 주소의 `<username>`은 GitHub 사용자 이름으로 바꾸어야 합니다.
