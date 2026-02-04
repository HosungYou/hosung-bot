# PDF 다운로드 가이드 (봇용)

## 검증된 프롬프트 템플릿

아래 프롬프트는 실제 테스트에서 성공적으로 작동했습니다:

```
브라우저에서 [검색어] 이렇게 검색해서 상위 5개를 PDF로 다운받아줘.

각 논문마다 이 순서를 정확히 따라:

Step 1: 논문 메인 페이지로 이동
  openclaw browser navigate "[논문 fulltext URL]"

Step 2: 페이지 스냅샷 찍기
  openclaw browser snapshot --labels

Step 3: PDF 버튼 ref 찾기
  스냅샷 결과에서 다음 패턴 검색:
  - "Download PDF" [ref=eXX]
  - "PDF" [ref=eXX]
  - "View PDF" [ref=eXX]
  ref 번호(예: e28, e15 등)를 기록

Step 4: 다운로드 실행
  openclaw browser download [ref번호] ~/Desktop/papers/[논문제목].pdf

Step 5: 파일 확인
  ls -la ~/Desktop/papers/
  file ~/Desktop/papers/[논문제목].pdf

⚠️ 주의:
- "openclaw browser pdf"는 사용하지 마 (페이지 변환임)
- PDF URL로 직접 이동하지 마 (로그인 페이지가 저장됨)
- 반드시 버튼의 ref를 클릭해서 다운로드해야 실제 PDF가 저장됨
```

## 서브에이전트 활용

복잡한 작업은 봇이 자동으로 **서브에이전트(sub-agent)**를 생성하여 처리합니다.

### 서브에이전트란?
- 메인 에이전트가 복잡한 작업을 위임하는 별도의 실행 단위
- 독립적인 세션으로 작업 수행
- 작업 완료 후 메인 에이전트에 결과 보고

### 서브에이전트 세션 확인
```bash
openclaw sessions
```

출력 예시:
```
Kind   Key                                    Age    Model
direct agent:main:main                        1m     gemini-3-pro-preview
group  agent:main:subagent:56fa2dde-...      5m     gemini-3-pro-preview
```

### 서브에이전트 활용 팁

1. **복잡한 작업 위임**: 여러 단계가 필요한 작업은 봇이 자동으로 서브에이전트 생성
2. **진행 상황 확인**: "작업 진행사항 보고해줘"로 상태 확인 가능
3. **세션 분리**: 서브에이전트는 별도 세션이므로 메인 세션 토큰에 영향 없음

## CLI 명령어 레퍼런스

### 핵심 명령어

```bash
# 페이지 이동
openclaw browser navigate "[URL]"

# 스냅샷 (요소 ref 확인)
openclaw browser snapshot --labels

# PDF 버튼 찾기
openclaw browser snapshot --labels | grep -i "pdf\|download"

# ref 클릭 + 다운로드 (핵심!)
openclaw browser download [ref] ~/Desktop/papers/[파일명].pdf

# 파일 타입 확인
file ~/Desktop/papers/[파일명].pdf
```

### 실제 작동 예시

```bash
# Cell/Heliyon 논문 다운로드
openclaw browser navigate "https://www.cell.com/heliyon/fulltext/S2405-8440(24)10553-1"

# 스냅샷에서 PDF 버튼 찾기
openclaw browser snapshot --labels | grep -i pdf
# 출력: link "PDF Download PDF" [ref=e28]

# 다운로드 실행
openclaw browser download e28 ~/Desktop/papers/Heliyon_AI_adoption.pdf
# 출력: downloaded: ~/Desktop/papers/Heliyon_AI_adoption.pdf

# 확인
file ~/Desktop/papers/Heliyon_AI_adoption.pdf
# 출력: PDF document, version 1.7
```

## 잘못된 방법 (사용 금지)

### ❌ 방법 1: PDF 변환
```bash
# 이건 현재 페이지를 PDF로 "변환"하는 것 - 실제 논문 PDF가 아님!
openclaw browser pdf
```

### ❌ 방법 2: PDF URL 직접 이동
```bash
# PDF URL로 직접 이동하면 로그인 페이지가 열릴 수 있음
openclaw browser navigate "https://example.com/paper.pdf"
```

## 출판사별 PDF 버튼 패턴

| 출판사 | 버튼 텍스트 | 예시 ref |
|--------|------------|----------|
| Cell/Heliyon | "PDF Download PDF" | e28 |
| Springer | "Download PDF" | e15 |
| Frontiers | "Download" | e22 |
| Nature | "Download PDF" | e30 |
| Elsevier/ScienceDirect | "Download" | e18 |
| Taylor & Francis | "Download PDF" | e25 |

## 트러블슈팅

### 다운로드된 파일이 PDF가 아님
```bash
# 파일 타입 확인
file ~/Desktop/papers/paper.pdf
# HTML document = 로그인 페이지가 저장됨 → 기관 인증 필요
```

### PDF 버튼이 스냅샷에 없음
- 로그인이 필요한 페이지일 수 있음
- Penn State 등 기관 인증 후 다시 시도

### 타임아웃 발생
```bash
# 타임아웃 연장 (기본 120초)
openclaw browser download e28 ~/Desktop/papers/paper.pdf --timeout-ms 180000
```

### 봇이 응답하지 않음
```bash
# 세션 초기화
openclaw gateway stop
rm -f ~/.openclaw/agents/main/sessions/*.jsonl
rm -f ~/.openclaw/agents/main/sessions/sessions.json
openclaw gateway install && openclaw gateway start
openclaw browser start
```

## 세션 관리

### 토큰 누적 확인
```bash
openclaw sessions
# Tokens (ctx %) 확인 - 30% 이상이면 초기화 권장
```

### 세션 초기화
```bash
openclaw gateway stop
rm -f ~/.openclaw/agents/main/sessions/*.jsonl
rm -f ~/.openclaw/agents/main/sessions/sessions.json
openclaw gateway install && openclaw gateway start
openclaw browser start
```

## 성공 사례

### AI Ethics and Adoption 검색 (2026-02-03)

**프롬프트:**
```
브라우저에서 AI Ethics and Adoption 이렇게 검색해서 상위 5개를 PDF로 다운받아줘.
[위 5단계 절차 포함]
```

**결과:**
- Ethics_and_Artificial_Intelligence_Adoption.pdf ✅
- Ethical_and_Sustainable_AI_Adoption.pdf ✅
- Understanding_the_Adoption_Intention_of_AI.pdf ✅

**특이사항:**
- 봇이 서브에이전트를 자동 생성하여 작업 수행
- 일부 논문은 기관 인증 필요로 실패
