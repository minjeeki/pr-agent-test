# 고급 개념 및 추가 정보

## 1. 확장 프로그램 아키텍처

### 서비스 워커 (Service Worker)

- Manifest V3에서 도입된 백그라운드 스크립트 대체
- 이벤트 기반 실행
- 메모리 효율적 사용
- 예시:
  ```javascript
  // service-worker.js
  chrome.runtime.onInstalled.addListener(() => {
    console.log("Service Worker 설치됨");
  });
  ```

### 웹 액세스 가능 리소스

- 외부 웹 페이지에서 접근 가능한 리소스 정의
- 보안을 고려한 접근 제한
- 예시:
  ```json
  {
    "web_accessible_resources": [
      {
        "resources": ["images/*"],
        "matches": ["https://*.example.com/*"]
      }
    ]
  }
  ```

## 2. 성능 최적화

### 지연 로딩 (Lazy Loading)

- 필요한 시점에 리소스 로드
- 초기 로딩 시간 단축
- 예시:
  ```javascript
  // 필요한 시점에 스크립트 로드
  const loadScript = (src) => {
    return new Promise((resolve, reject) => {
      const script = document.createElement("script");
      script.src = src;
      script.onload = resolve;
      script.onerror = reject;
      document.head.appendChild(script);
    });
  };
  ```

### 메모리 관리

- 불필요한 리소스 해제
- 이벤트 리스너 정리
- 예시:
  ```javascript
  // 이벤트 리스너 정리
  const cleanup = () => {
    window.removeEventListener("resize", handleResize);
    chrome.runtime.onMessage.removeListener(messageHandler);
  };
  ```

## 3. 사용자 경험 개선

### 상태 관리

- 확장 프로그램 상태 유지
- 사용자 설정 저장
- 예시:
  ```javascript
  // 상태 관리
  const state = {
    settings: {},
    async init() {
      this.settings = await chrome.storage.sync.get("settings");
    },
    async update(key, value) {
      this.settings[key] = value;
      await chrome.storage.sync.set({ settings: this.settings });
    },
  };
  ```

### 오프라인 지원

- 오프라인 상태 처리
- 데이터 동기화
- 예시:
  ```javascript
  // 오프라인 상태 감지
  window.addEventListener("online", handleOnline);
  window.addEventListener("offline", handleOffline);
  ```

## 4. 디버깅 및 테스트

### 디버깅 도구

- Chrome 개발자 도구 활용
- 로깅 전략
- 예시:
  ```javascript
  // 디버깅 로그
  const debug = {
    log: (message, data) => {
      if (process.env.NODE_ENV === "development") {
        console.log(`[DEBUG] ${message}`, data);
      }
    },
  };
  ```

### 테스트 전략

- 단위 테스트
- 통합 테스트
- 예시:
  ```javascript
  // 테스트 예시
  describe("Extension Functionality", () => {
    it("should handle messages correctly", async () => {
      const response = await chrome.runtime.sendMessage({ type: "test" });
      expect(response).toBeDefined();
    });
  });
  ```

## 5. 국제화 (i18n)

### 다국어 지원

- 메시지 번역
- 지역화 설정
- 예시:
  ```json
  {
    "_locales": {
      "en": {
        "messages": {
          "welcome": {
            "message": "Welcome to the extension!"
          }
        }
      },
      "ko": {
        "messages": {
          "welcome": {
            "message": "확장 프로그램에 오신 것을 환영합니다!"
          }
        }
      }
    }
  }
  ```

## 6. 확장성 및 유지보수

### 모듈화

- 코드 구조화
- 재사용 가능한 컴포넌트
- 예시:
  ```javascript
  // 모듈화된 코드 구조
  // components/
  //   ├── popup/
  //   ├── content/
  //   └── background/
  // utils/
  //   ├── storage.js
  //   └── messaging.js
  ```

### 버전 관리

- 의미적 버전 관리
- 호환성 유지
- 예시:
  ```json
  {
    "version": "1.2.3",
    "version_name": "1.2.3-beta.1"
  }
  ```

## 7. 보안 고려사항

### 콘텐츠 보안 정책 (CSP)

- 리소스 출처 제한
- 인라인 스크립트 제한
- 예시:
  ```json
  {
    "content_security_policy": {
      "extension_pages": "script-src 'self'; object-src 'self'",
      "sandbox": "sandbox allow-scripts allow-forms allow-popups allow-modals"
    }
  }
  ```

### 데이터 검증

- 입력 데이터 검증
- 출력 데이터 이스케이프
- 예시:
  ```javascript
  // 데이터 검증
  const validateInput = (input) => {
    if (typeof input !== "string") return false;
    return /^[a-zA-Z0-9]+$/.test(input);
  };
  ```

## 8. 성능 모니터링

### 성능 메트릭

- 로딩 시간 측정
- 메모리 사용량 모니터링
- 예시:
  ```javascript
  // 성능 측정
  const measurePerformance = async () => {
    const start = performance.now();
    await performOperation();
    const end = performance.now();
    console.log(`Operation took ${end - start}ms`);
  };
  ```

### 에러 추적

- 에러 로깅
- 사용자 피드백 수집
- 예시:
  ```javascript
  // 에러 추적
  window.onerror = (message, source, lineno, colno, error) => {
    console.error("Error:", { message, source, lineno, colno, error });
    // 에러 보고 시스템으로 전송
  };
  ```
