# Open Code Review Codex 플러그인 사용법

이 문서는 로컬 Codex에서 My Open Code Review를 사용하는 방법을 설명합니다.

## 개요

이 fork는 명시적인 OCR 설정이 없으면 로컬 Codex 호환 endpoint를 기본 LLM backend로 사용합니다. Codex에서 로컬 `ocr` CLI를 호출할 수 있도록 skill을 제공합니다.

```text
Codex
  └─ Open Code Review plugin
      └─ ocr review --audience agent
```

## 사전 준비

`ocr` CLI가 설치되어 있어야 합니다.

```bash
npm install -g my-open-code-review
```

설치 확인:

```bash
command -v ocr
ocr version
```

로컬 Codex endpoint가 없으면 OCR 자체의 LLM 설정도 필요합니다.

```bash
ocr llm test
```

이 명령이 실패하면 Codex 플러그인 설치와 별개로 OCR의 LLM 설정을 먼저 완료해야 합니다.

## Codex에서 설치

Codex에서 이 repo를 marketplace로 추가합니다.

```bash
codex plugin marketplace add Daishuyuan/my-open-code-review
codex
```

Codex 안에서 `/plugins`를 열고 `Open Code Review`를 설치 및 활성화합니다.

로컬 checkout 또는 fork에서 테스트할 때는 다음을 사용할 수 있습니다.

```bash
codex plugin marketplace add .
codex
```

## 사용 예시

새 Codex thread에서 다음처럼 요청합니다.

```text
@Open Code Review review my current changes
```

브랜치 비교:

```text
@Open Code Review review this branch against main
```

검토 후 안전한 항목만 수정:

```text
@Open Code Review review and fix high-confidence issues
```

## 내부적으로 실행되는 명령

현재 workspace 변경사항 검토:

```bash
ocr review --audience agent
```

특정 commit 검토:

```bash
ocr review --audience agent --commit <sha>
```

브랜치 비교:

```bash
ocr review --audience agent --from <base-ref> --to <head-ref>
```

미리보기:

```bash
ocr review --preview
```

## 주의사항

- 기본값은 `http://127.0.0.1:15721/v1`, `gpt-5.5`, `reasoning_effort=high`입니다.
- `OCR_CODEX_URL`, `OCR_CODEX_MODEL`, `OCR_CODEX_REASONING_EFFORT`로 기본값을 바꿀 수 있습니다.
- 로컬 Codex endpoint가 없으면 OCR provider 설정이 필요합니다.
- 파일 수정은 사용자가 명시적으로 요청한 경우에만 수행합니다.
- commit 생성은 사용자가 명시적으로 요청한 경우에만 수행합니다.
