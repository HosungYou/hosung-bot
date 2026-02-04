# Hosung-Bot

OpenClaw 기반 Discord AI 봇 설정 및 운영 가이드

## 개요

이 저장소는 OpenClaw를 사용한 Discord 봇 설정, 브라우저 자동화, PDF 다운로드, 트러블슈팅 가이드를 포함합니다.

## 현재 구성

| 항목 | 설정값 |
|------|--------|
| OpenClaw 버전 | 2026.2.1 |
| 기본 모델 | google-gemini-cli/gemini-3-pro-preview |
| 브라우저 모드 | managed (OpenClaw 관리형) |
| Discord 봇 | @Hosung-bot |

## 문서 구조

```
docs/
├── openclaw/
│   ├── setup.md              # 초기 설정 가이드
│   ├── browser-config.md     # 브라우저 설정
│   ├── discord-integration.md # Discord 연동
│   └── pdf-download-guide.md # PDF 다운로드 가이드 ⭐
└── troubleshooting/
    ├── common-issues.md      # 일반적인 문제 해결
    └── browser-relay-cors.md # 브라우저 CORS 이슈
```

## 빠른 시작

```bash
# Gateway 상태 확인
openclaw gateway status

# 브라우저 시작
openclaw browser start

# Discord 상태 확인
openclaw channels status
```

## 핵심 기능: PDF 다운로드

### 검증된 프롬프트

```
브라우저에서 [검색어] 검색해서 상위 5개를 PDF로 다운받아줘.

각 논문마다 이 순서를 정확히 따라:

Step 1: 논문 메인 페이지로 이동
  openclaw browser navigate "[논문 fulltext URL]"

Step 2: 페이지 스냅샷 찍기
  openclaw browser snapshot --labels

Step 3: PDF 버튼 ref 찾기
  스냅샷 결과에서 "Download PDF" [ref=eXX] 패턴 검색

Step 4: 다운로드 실행
  openclaw browser download [ref번호] ~/Desktop/papers/[논문제목].pdf

Step 5: 파일 확인
  file ~/Desktop/papers/[논문제목].pdf

⚠️ 주의:
- "openclaw browser pdf" 사용 금지 (페이지 변환임)
- PDF URL 직접 이동 금지 (로그인 페이지 저장됨)
```

자세한 내용: [PDF 다운로드 가이드](docs/openclaw/pdf-download-guide.md)

## 서브에이전트

복잡한 작업은 봇이 **서브에이전트**를 자동 생성하여 처리합니다.

```bash
# 세션 확인
openclaw sessions

# 출력 예시:
# Kind   Key                              Age    Model
# direct agent:main:main                  1m     gemini-3-pro-preview
# group  agent:main:subagent:56fa...     5m     gemini-3-pro-preview
```

### 서브에이전트 활용 팁

1. 여러 단계 작업은 자동으로 서브에이전트 생성
2. "작업 진행사항 보고해줘"로 상태 확인
3. 별도 세션이므로 메인 토큰에 영향 없음

## 세션 관리

### 토큰 확인
```bash
openclaw sessions
# Tokens (ctx %) 30% 이상이면 초기화 권장
```

### 세션 초기화
```bash
openclaw gateway stop
rm -f ~/.openclaw/agents/main/sessions/*.jsonl
rm -f ~/.openclaw/agents/main/sessions/sessions.json
openclaw gateway install && openclaw gateway start
openclaw browser start
```

## 봇 사용법

브라우저 기능 사용 시 명시적으로 "브라우저"를 언급하세요:

```
# 좋은 예시
"브라우저에서 Google Scholar 열고 'AI Ethics' 검색해줘"
"현재 브라우저 페이지 스크린샷 보여줘"
"브라우저에서 보이는 논문 목록 읽어줘"

# 피해야 할 예시
"논문 찾아줘" (브라우저 사용 명시 안 함)
```

## 라이선스

MIT License
