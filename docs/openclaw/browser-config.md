# 브라우저 설정 가이드

## 브라우저 모드

OpenClaw는 두 가지 브라우저 모드를 지원합니다:

### 1. Managed 모드 (권장)

OpenClaw가 별도의 Chrome 인스턴스를 직접 실행하고 제어합니다.

```bash
# 프로필 생성
openclaw browser create-profile --name managed --driver openclaw --color "#FF4500"

# 기본값으로 설정
openclaw config set browser.defaultProfile managed

# Gateway 재시작
openclaw gateway restart
```

**장점:**
- 안정적인 연결
- CORS 문제 없음
- 완전한 제어 가능

**단점:**
- 기존 브라우저와 별도 실행
- 확장 프로그램 별도 설치 필요

### 2. Extension 모드

기존 Chrome에 확장 프로그램으로 연결합니다.

```bash
# 확장 프로그램 설치
openclaw browser extension install

# 프로필 생성
openclaw browser create-profile --name chrome-ext --driver extension --cdp-url http://127.0.0.1:18792
```

**주의:** 현재 버전(2026.2.1)에서 CORS 버그로 작동하지 않음

## 브라우저 시작/중지

```bash
# 시작
openclaw browser start

# 특정 프로필로 시작
openclaw browser start --browser-profile managed

# 상태 확인
openclaw browser status

# 중지
openclaw browser stop
```

## 브라우저 명령어

```bash
# 탭 목록
openclaw browser tabs

# URL 열기
openclaw browser navigate "https://scholar.google.com"

# 스크린샷
openclaw browser screenshot

# 페이지 스냅샷 (요소 ref 포함)
openclaw browser snapshot --labels

# 요소에 텍스트 입력
openclaw browser type e5 "검색어" --submit

# 요소 클릭
openclaw browser click e10
```

## 현재 설정 확인

```bash
openclaw config get browser
```

예상 출력:
```json
{
  "defaultProfile": "managed",
  "profiles": {
    "managed": {
      "cdpPort": 18801,
      "color": "#FF4500"
    }
  }
}
```

## 프로필 목록 확인

```bash
openclaw browser profiles
```

예상 출력:
```
managed: running (1 tabs) [default]
  port: 18801, color: #FF4500
```
