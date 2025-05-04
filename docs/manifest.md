# manifest.json (권한 관리)

manifest.json은 크롬 확장 프로그램의 설정 파일로, 브라우저에 확장 프로그램의 내용을 알려주고 필요한 권한을 요청하는 역할을 합니다.

- manifest.json 파일을 읽어서 확장 프로그램 목록에서 이름, 버전, 설명, 아이콘을 보여주게 됨

## Manifest 버전 비교

### Manifest V2 vs V3

| 기능                | Manifest V2                                 | Manifest V3                             |
| ------------------- | ------------------------------------------- | --------------------------------------- |
| 백그라운드 스크립트 | `background.scripts` 또는 `background.page` | `service_worker` 사용                   |
| 콘텐츠 보안 정책    | `content_security_policy` 문자열            | `content_security_policy` 객체          |
| 웹 리소스 접근      | `web_accessible_resources` 배열             | `web_accessible_resources` 객체 배열    |
| 권한 요청           | `permissions` 배열                          | `permissions`와 `host_permissions` 분리 |
| 네트워크 요청       | `webRequest` API 자유로운 사용              | `declarativeNetRequest` API 권장        |
| 리모트 코드         | 원격 코드 실행 가능                         | 로컬 코드만 허용                        |
| 메모리 사용         | 지속적인 백그라운드 프로세스                | 이벤트 기반 서비스 워커                 |

### 주요 변경사항

1. **백그라운드 스크립트**

   - V2: 지속적으로 실행되는 백그라운드 페이지
   - V3: 이벤트 기반 서비스 워커로 변경

   ```json
   // V2
   "background": {
     "scripts": ["background.js"],
     "persistent": true
   }

   // V3
   "background": {
     "service_worker": "background.js"
   }
   ```

2. **콘텐츠 보안 정책**

   - V2: 단일 문자열로 정의
   - V3: 확장 페이지와 샌드박스에 대해 별도로 정의

   ```json
   // V2
   "content_security_policy": "script-src 'self'; object-src 'self'"

   // V3
   "content_security_policy": {
     "extension_pages": "script-src 'self'; object-src 'self'",
     "sandbox": "sandbox allow-scripts allow-forms allow-popups allow-modals"
   }
   ```

3. **웹 리소스 접근**

   - V2: 단순 리소스 목록
   - V3: 도메인 기반 접근 제어

   ```json
   // V2
   "web_accessible_resources": ["images/*", "fonts/*"]

   // V3
   "web_accessible_resources": [{
     "resources": ["images/*", "fonts/*"],
     "matches": ["https://*.example.com/*"]
   }]
   ```

4. **권한 관리**

   - V2: 모든 권한을 `permissions` 배열에 포함
   - V3: 호스트 권한을 `host_permissions`로 분리

   ```json
   // V2
   "permissions": ["storage", "tabs", "https://*.example.com/*"]

   // V3
   "permissions": ["storage", "tabs"],
   "host_permissions": ["https://*.example.com/*"]
   ```

## 주요 기능

### 확장 프로그램의 메타데이터 정의 (이름, 버전, 설명 등)

```json
{
  // manifest_version : 버전
  "manifest_version": 3,
  // name : 버전
  "name": "Catify",
  // description : 설명
  "description": "Choose a cat image to set as a background for all the pages you visit",
  // version : 개발 버전
  "version": "1.0.0",
  // 확장 프로그램에 보일 아이콘
  "icons": {
    "16": "icons/icon16.png",
    "32": "icons/icon32.png",
    "48": "icons/icon48.png",
    "128": "icons/icon128.png"
  }
}
```

### 팝업 UI 설정

```json
{
  "action": {
    // 확장 프로그램 활성화 시 보일 아이콘
    "default_icon": "icons/icon16.png",
    // 확장 프로그램의 제목
    "default_title": "Catify",
    // 팝업 UI가 있는 HTML 경로
    "default_popup": "popup/popup.html"
  }
}
```

### 권한 요청

```json
{
  "permission": ["storage"]
}
```

#### 주요 권한 종류

- `tabs`: 브라우저 탭에 접근
- `storage`: 데이터 저장소 사용
- `activeTab`: 현재 활성화된 탭에 접근
- `scripting`: 페이지에 스크립트 삽입
- `webRequest`: 웹 요청 모니터링 및 수정

### 웹 페이지에 삽입할 스크립트와 스타일 정의

```json
{
  "content_scripts": [
    {
      // 스크립트가 삽입될 웹 페이지 URL 패턴
      "matches": ["<all_urls>"],
      // 삽입할 JavaScript 파일
      "js": ["scripts/inject.js"],
      // 삽입할 CSS 파일
      "css": ["styles/inject.css"],
      // 스크립트 실행 시점 (선택적)
      "run_at": "document_end"
    }
  ]
}
```

#### content_scripts 주요 설정

- `matches`: 스크립트가 삽입될 웹 페이지의 URL 패턴

  - `<all_urls>`: 모든 웹 페이지
  - `*://*.example.com/*`: 특정 도메인의 모든 페이지
  - `*://*.example.com/path/*`: 특정 경로의 페이지

- `js`: 삽입할 JavaScript 파일 목록

  - 파일은 확장 프로그램의 루트 디렉토리를 기준으로 경로 지정
  - 여러 파일을 배열로 지정 가능
  - 순서대로 실행됨

- `css`: 삽입할 CSS 파일 목록

  - 스타일시트 파일 경로 지정
  - 여러 파일을 배열로 지정 가능

- `run_at`: 스크립트 실행 시점 (선택적)
  - `document_start`: DOM이 로드되기 전
  - `document_end`: DOM이 로드된 후, 이미지 등 리소스가 로드되기 전
  - `document_idle`: 기본값, 페이지가 완전히 로드된 후

## 보안 원칙

### 1. 최소 권한 원칙 (Principle of Least Privilege)

- 필요한 최소한의 권한만 요청
- 불필요한 권한은 제거
- 권한 요청 시 사용자에게 명확한 설명 제공
- 예시:

  ```json
  // 나쁜 예: 불필요한 권한 포함
  "permissions": ["tabs", "storage", "bookmarks", "history"]

  // 좋은 예: 필요한 권한만 포함
  "permissions": ["storage"]
  ```

### 2. 콘텐츠 보안 정책 (Content Security Policy)

- 외부 리소스 로딩 제한
- 인라인 스크립트 실행 제한
- 안전한 소스만 허용
- 예시:
  ```json
  {
    "content_security_policy": {
      "extension_pages": "script-src 'self'; object-src 'self'"
    }
  }
  ```

### 3. 데이터 보호

- 민감한 사용자 데이터 암호화
- 로컬 스토리지 사용 시 주의
- 데이터 전송 시 HTTPS 사용
- 예시:
  ```javascript
  // 민감한 데이터는 암호화하여 저장
  chrome.storage.local.set({
    encryptedData: encryptSensitiveData(userData),
  });
  ```

### 4. 웹 페이지 접근 제한

- 필요한 도메인만 matches에 지정
- 민감한 사이트 접근 제한
- 예시:
  ```json
  {
    "content_scripts": [
      {
        "matches": ["https://*.example.com/*"],
        "js": ["content.js"]
      }
    ]
  }
  ```

### 5. 업데이트 보안

- 정기적인 보안 업데이트
- 취약점 패치
- 이전 버전과의 호환성 유지
- 예시:
  ```json
  {
    "version": "1.0.1",
    "update_url": "https://example.com/updates.xml"
  }
  ```

### 6. 사용자 프라이버시 보호

- 개인정보 수집 최소화
- 데이터 수집 시 사용자 동의 필수
- 개인정보 처리방침 명시
- 예시:
  ```json
  {
    "privacy_policy": "https://example.com/privacy.html"
  }
  ```

### 7. 보안 취약점 방지

- XSS(Cross-Site Scripting) 방지
- CSRF(Cross-Site Request Forgery) 방지
- 인젝션 공격 방지
- 예시:

  ```javascript
  // 안전하지 않은 코드
  element.innerHTML = userInput;

  // 안전한 코드
  element.textContent = userInput;
  ```

### 8. 보안 모니터링

- 보안 로그 기록
- 이상 징후 감지
- 보안 이벤트 알림
- 예시:
  ```javascript
  // 보안 이벤트 로깅
  function logSecurityEvent(event) {
    console.log(`Security Event: ${event.type} - ${event.details}`);
  }
  ```

### 9. 보안 테스트

- 정기적인 보안 감사
- 취약점 스캔
- 침투 테스트
- 예시:
  ```json
  {
    "web_accessible_resources": [
      {
        "resources": ["test/*"],
        "matches": ["https://*.example.com/*"]
      }
    ]
  }
  ```

### 10. 보안 문서화

- 보안 관련 설정 문서화
- 취약점 보고 절차 명시
- 보안 업데이트 기록 유지
- 예시:

  ```markdown
  ## 보안 업데이트 기록

  - v1.0.1: XSS 취약점 패치
  - v1.0.2: CSRF 보호 강화
  ```
