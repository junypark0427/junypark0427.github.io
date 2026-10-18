# 04. Markdown과 MkDocs

## 1. Markdown

**Markdown**은 텍스트만으로 문서의 구조(제목, 목록, 링크 등)를 표현하는 경량 마크업 언어입니다.

`#`, `*`, `-` 같은 기호로 서식을 작성하며 복잡한 규칙이 적고, HTML 등의 형식으로 변환하기 쉽습니다. AI 채팅 도구에서도 Markdown 형식의 답변을 자주 볼 수 있습니다.

### Markdown이 사용되는 곳

- **GitHub**: `README.md`, 이슈, 코멘트 등의 문서 작성
- **Notion, Obsidian**: 메모와 문서 작성
- **AI 도구**: ChatGPT, Claude 등의 구조화된 텍스트 응답
- **기술 블로그**: 개발 및 학습 내용 정리

### 간단한 Markdown 문법

````markdown
# 큰 제목
## 작은 제목

- 목록 하나
- 목록 둘

**굵은 글씨**

[링크 이름](https://www.mkdocs.org/)

`코드`
````

우리도 웹페이지의 내용을 Markdown 파일(`.md`)로 작성할 것입니다.

## 2. MkDocs

**MkDocs**는 Markdown 문서를 읽어 HTML 기반의 **정적 웹사이트**를 생성하는 도구입니다. 여러 Markdown 파일을 메뉴로 묶고 테마를 적용하는 작업을 편리하게 할 수 있습니다.

- 공식 홈페이지: [MkDocs](https://www.mkdocs.org/)

```text
내가 작성한 Markdown(.md) 파일
             │
             ▼
        MkDocs 빌드
             │
             ▼
        HTML 웹사이트
             │
             ▼
       GitHub Pages
             │
             ▼
          인터넷 공개
```

> Markdown 파일을 그냥 업로드하는 것과 MkDocs로 사이트를 빌드하는 것은 다릅니다. Material 같은 MkDocs 테마를 적용하려면 MkDocs로 HTML을 생성하는 과정이 필요합니다.
