# Hosung-Bot

OpenClaw 기반 Discord AI 봇 설정 및 운영 가이드

## 개요

이 저장소는 OpenClaw를 사용한 Discord 봇 설정, 브라우저 자동화, 트러블슈팅 가이드를 포함합니다.

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
│   └── discord-integration.md # Discord 연동
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
