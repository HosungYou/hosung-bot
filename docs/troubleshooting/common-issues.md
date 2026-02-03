# 일반적인 문제 해결

## Gateway 관련

### "Gateway not reachable" 에러

```bash
# Gateway 상태 확인
openclaw gateway status

# Gateway 재시작
openclaw gateway restart

# 강제 시작 (포트 충돌 시)
openclaw gateway --force
```

### "commands.restart=true" 필요 에러

```bash
openclaw config set commands.restart true
openclaw gateway restart
```

## 브라우저 관련

### "tab not found" 에러

```bash
# 브라우저 상태 확인
openclaw browser status

# 브라우저 시작
openclaw browser start

# 탭 확인
openclaw browser tabs
```

### "selector is not supported" 에러

OpenClaw 브라우저는 CSS selector 대신 snapshot ref를 사용합니다:

```bash
# 페이지 스냅샷 가져오기
openclaw browser snapshot --labels

# ref로 요소 조작
openclaw browser click e5
openclaw browser type e5 "텍스트"
```

### 브라우저가 응답하지 않음

```bash
# 브라우저 중지 후 재시작
openclaw browser stop
openclaw browser start

# 또는 Gateway 전체 재시작
openclaw gateway restart
sleep 3
openclaw browser start
```

## Discord 관련

### "Message Content Intent is disabled" 경고

1. Discord Developer Portal 접속
2. Bot → Privileged Gateway Intents
3. "Message Content Intent" 활성화
4. `openclaw gateway restart`

### "groupPolicy allowlist" 문제

```bash
# 모든 서버 허용
openclaw config set channels.discord.groupPolicy open
openclaw gateway restart
```

### DM 페어링 필요

```bash
# 페어링 코드 승인
openclaw pairing approve discord <CODE>
```

## 모델 관련

### 브라우저 도구 호출 실패 (Gemini)

Gemini 2.0 Flash는 브라우저 도구를 잘못 호출할 수 있습니다. Gemini 3 Pro로 변경:

```bash
openclaw config set agents.defaults.model.primary "google-gemini-cli/gemini-3-pro-preview"
openclaw gateway restart
```

### OAuth 토큰 만료

```bash
# 토큰 상태 확인
openclaw models

# 재인증
openclaw configure --section model
```

## 로그 확인

```bash
# 오늘 로그 확인
tail -100 /tmp/openclaw/openclaw-$(date +%Y-%m-%d).log

# 에러만 필터링
grep -i "error\|fail" /tmp/openclaw/openclaw-$(date +%Y-%m-%d).log | tail -20

# 브라우저 관련 로그
grep -i "browser" /tmp/openclaw/openclaw-$(date +%Y-%m-%d).log | tail -20
```

## 전체 리셋

문제가 지속되면 전체 리셋:

```bash
# 주의: 모든 설정이 초기화됩니다
openclaw reset

# 다시 설정
openclaw onboard
```
