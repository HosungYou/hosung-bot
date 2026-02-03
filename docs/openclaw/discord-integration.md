# Discord 연동 가이드

## Discord 봇 설정

### 1. Discord Developer Portal 설정

1. [Discord Developer Portal](https://discord.com/developers/applications) 접속
2. "New Application" 클릭
3. Bot 탭에서 "Add Bot" 클릭
4. Token 복사

### 2. 필수 권한 (Privileged Gateway Intents)

Bot 탭에서 활성화:
- [x] **Message Content Intent** - 메시지 내용 읽기
- [x] **Server Members Intent** (선택)
- [x] **Presence Intent** (선택)

### 3. Bot Permissions

OAuth2 URL Generator에서 선택:
- [x] Read Messages/View Channels
- [x] Send Messages
- [x] Read Message History
- [x] Embed Links
- [x] Attach Files
- [x] Add Reactions

### 4. OpenClaw에 토큰 등록

```bash
openclaw channels add --channel discord --token "YOUR_BOT_TOKEN"
```

## Discord 설정

### 채널 정책 설정

```bash
# 모든 서버에서 응답 허용
openclaw config set channels.discord.groupPolicy open

# Gateway 재시작
openclaw gateway restart
```

### DM 정책 설정

```bash
# DM 페어링 승인
openclaw pairing approve discord <PAIRING_CODE>

# 또는 DM 정책 변경
openclaw config set channels.discord.dm.policy open
```

## 상태 확인

```bash
# 채널 상태
openclaw channels status

# 예상 출력:
# - Discord default: enabled, configured, running, bot:@Hosung-bot
```

## 봇 사용법

### 서버 채널에서
봇을 멘션해야 응답합니다:
```
@Hosung-bot 안녕하세요
```

### DM에서
멘션 없이 바로 대화 가능:
```
안녕하세요
```

### 브라우저 기능 사용
"브라우저"를 명시적으로 언급:
```
@Hosung-bot 브라우저에서 Google Scholar 열고 AI Ethics 검색해줘
@Hosung-bot 현재 브라우저 페이지 스크린샷 보여줘
@Hosung-bot 브라우저에서 보이는 논문 목록 읽어줘
```

## 트러블슈팅

### "Message Content Intent is disabled" 경고

Discord Developer Portal → Bot → Privileged Gateway Intents에서 "Message Content Intent" 활성화

### 봇이 응답하지 않음

1. `openclaw channels status`로 연결 상태 확인
2. `openclaw gateway restart`로 Gateway 재시작
3. `openclaw doctor --repair`로 자동 수정
