# 브라우저 Extension Relay CORS 이슈

## 문제 설명

OpenClaw 2026.2.1 버전에서 Chrome Extension 모드 사용 시 CORS 에러가 발생합니다.

### 에러 메시지

```
Access to fetch at 'http://127.0.0.1:18792/' from origin
'chrome-extension://nghmikiaignlppflpiicenbbicplipje' has been blocked
by CORS policy: No 'Access-Control-Allow-Origin' header is present
on the requested resource.
```

### 원인

OpenClaw의 브라우저 relay 서버가 CORS 헤더를 포함하지 않아 Chrome 확장 프로그램이 연결할 수 없습니다.

## 진단 방법

```bash
# Relay 헤더 확인
curl -s -I http://127.0.0.1:18792/

# 예상 출력 (CORS 헤더 없음):
# HTTP/1.1 200 OK
# Date: ...
# Connection: keep-alive
```

정상적인 경우 `Access-Control-Allow-Origin: *` 헤더가 있어야 합니다.

## 현재 해결책: Managed 모드 사용

Extension 모드 대신 Managed 모드를 사용합니다:

```bash
# Managed 프로필 생성
openclaw browser create-profile --name managed --driver openclaw --color "#FF4500"

# 기본값으로 설정
openclaw config set browser.defaultProfile managed

# Gateway 재시작
openclaw gateway restart

# 브라우저 시작
openclaw browser start
```

## Extension 모드 설정 (현재 작동 안 함)

참고용으로 Extension 모드 설정 방법을 기록합니다:

### 1. 확장 프로그램 설치

```bash
openclaw browser extension install
```

### 2. Chrome에 로드

1. `chrome://extensions` 접속
2. "개발자 모드" 활성화
3. "압축해제된 확장 프로그램을 로드합니다" 클릭
4. `~/.openclaw/browser/chrome-extension` 선택

### 3. 권한 설정

manifest.json의 host_permissions를 수정해야 합니다:

```json
{
  "host_permissions": ["<all_urls>"]
}
```

기본값 `["http://127.0.0.1/*", "http://localhost/*"]`는 부족합니다.

### 4. 사이트 권한

Chrome에서:
1. 확장 프로그램 아이콘 우클릭
2. "This Can Read and Change Site Data"
3. "On All Sites" 선택

## 버그 리포트

이 문제는 OpenClaw 버그로 보입니다. 해결되면 Extension 모드를 다시 사용할 수 있습니다.

- 버전: OpenClaw 2026.2.1
- 증상: Relay 서버가 CORS 헤더 미포함
- 영향: Chrome Extension이 relay에 연결 불가

## Managed 모드와 Extension 모드 비교

| 항목 | Managed | Extension |
|------|---------|-----------|
| 작동 여부 | O | X (CORS 버그) |
| 기존 Chrome 사용 | X | O |
| 별도 창 필요 | O | X |
| 확장 프로그램 공유 | X | O |
| 안정성 | 높음 | - |
