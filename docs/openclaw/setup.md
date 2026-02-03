# OpenClaw 초기 설정 가이드

## 설치

```bash
npm install -g openclaw
```

## 초기 설정

```bash
# 설정 마법사 실행
openclaw onboard

# 또는 doctor로 자동 수정
openclaw doctor --repair
```

## Gateway 서비스 설정

```bash
# Gateway 서비스 설치
openclaw gateway install

# Gateway 시작
openclaw gateway start

# 상태 확인
openclaw gateway status
```

## 모델 설정

### Google Gemini (OAuth)

```bash
openclaw configure --section model
```

프롬프트에 따라 Google 계정으로 로그인하면 자동 설정됩니다.

### 기본 모델 변경

```bash
# Gemini 3 Pro로 변경 (브라우저 작업에 더 안정적)
openclaw config set agents.defaults.model.primary "google-gemini-cli/gemini-3-pro-preview"
openclaw gateway restart
```

## 설정 확인

```bash
# 전체 상태 확인
openclaw doctor

# 모델 상태 확인
openclaw models

# Gateway 상태 확인
openclaw gateway health
```

## 주요 설정 파일 위치

| 파일 | 경로 |
|------|------|
| 메인 설정 | `~/.openclaw/openclaw.json` |
| 에이전트 설정 | `~/.openclaw/agents/main/` |
| 세션 저장소 | `~/.openclaw/agents/main/sessions/` |
| 로그 파일 | `/tmp/openclaw/openclaw-YYYY-MM-DD.log` |
