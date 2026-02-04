# PDF 다운로드 가이드 (봇용)

## 올바른 워크플로우

### 핵심 명령어

```bash
# 1. 논문 페이지로 이동
openclaw browser navigate "[논문 URL]"

# 2. 스냅샷으로 PDF 버튼 ref 찾기
openclaw browser snapshot --labels | grep -i "pdf\|download"

# 3. ref로 클릭 + 다운로드 (핵심!)
openclaw browser download [ref] ~/Desktop/papers/[파일명].pdf

# 4. 파일 확인
file ~/Desktop/papers/[파일명].pdf
```

### 예시

```bash
# Cell/Heliyon 논문
openclaw browser navigate "https://www.cell.com/heliyon/fulltext/S2405-8440(24)10553-1"

# 스냅샷 결과에서 찾음: link "PDF Download PDF" [ref=e28]
openclaw browser download e28 ~/Desktop/papers/Heliyon_paper.pdf

# 결과: downloaded: ~/Desktop/papers/Heliyon_paper.pdf
```

## 잘못된 방법 (사용 금지)

### ❌ 방법 1: PDF 변환 (페이지를 PDF로 만듦)
```bash
# 이건 현재 페이지를 PDF로 "변환"하는 것
# 실제 논문 PDF가 아님!
openclaw browser pdf
```

### ❌ 방법 2: PDF URL 직접 이동
```bash
# PDF URL로 직접 이동하면 로그인 페이지가 열림
openclaw browser navigate "https://example.com/paper.pdf"
# 그 후 저장하면 로그인 페이지 HTML이 저장됨
```

## 올바른 방법 (반드시 사용)

### ✅ 방법: ref 클릭 + 다운로드
```bash
# 1. 논문 페이지에서 PDF 버튼의 ref 찾기
openclaw browser snapshot --labels | grep -i pdf
# 출력: link "Download PDF" [ref=e28]

# 2. ref로 다운로드 (클릭 + 저장 한 번에)
openclaw browser download e28 ~/Desktop/papers/paper.pdf
```

## 봇에게 줄 프롬프트 템플릿

```
PDF 다운로드 시 반드시 이 순서를 따라:

1. 논문 fulltext 페이지로 이동
2. "openclaw browser snapshot --labels" 실행
3. 결과에서 "PDF" 또는 "Download PDF" 링크의 ref 찾기
4. "openclaw browser download [ref] ~/Desktop/papers/[파일명].pdf" 실행
5. "file" 명령으로 실제 PDF인지 확인

절대 금지:
- "openclaw browser pdf" 사용 금지 (페이지 변환임)
- PDF URL로 직접 navigate 금지 (로그인 페이지 저장됨)

이 방법으로 [검색어] 논문 [N]개 다운로드해줘.
```

## 출판사별 PDF 버튼 패턴

| 출판사 | 버튼 텍스트 | 예시 ref |
|--------|------------|----------|
| Cell/Heliyon | "PDF Download PDF" | e28 |
| Springer | "Download PDF" | e15 |
| Frontiers | "Download" | e22 |
| Nature | "Download PDF" | e30 |
| Elsevier | "Download" | e18 |

## 다운로드 실패 시

1. 스냅샷에서 PDF 버튼이 안 보임 → 로그인 필요
2. 다운로드 후 HTML 파일 → 기관 인증 필요
3. 타임아웃 → `--timeout-ms 120000` 옵션 추가

```bash
# 타임아웃 연장
openclaw browser download e28 ~/Desktop/papers/paper.pdf --timeout-ms 120000
```
