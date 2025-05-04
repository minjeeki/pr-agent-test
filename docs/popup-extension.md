# 팝업 익스텐션 앱

팝업 익스텐션 앱은 브라우저 도구 모음에서 확장 앱의 아이콘을 클릭하면 나타나는 인터페이스를 제공하는 확장 프로그램입니다.

## 기본 구조

```
popup-extension/
├── manifest.json
|
├── popup/
|   ├── popup.html
|   ├── popup.css
|   └── popup.js
|
└── icons/
    ├── icon16.png
    ├── icon32.png
    ├── icon48.png
    └── icon128.png
```

- popup.html, popup.js. popup.css 등은 popup/ 폴더의 하위에 명시하기도 함

## 주요 특징

### 사용자 인터페이스

- HTML, CSS, JavaScript로 구성된 작은 웹 페이지
- 브라우저 도구 모음의 아이콘 클릭 시 표시
- 현재 웹 페이지에서 벗어나지 않고 빠른 액세스 가능

### manifest.json 설정

```json
{
  "manifest_version": 3,
  "name": "팝업 익스텐션",
  "version": "1.0",
  "action": {
    "default_title": "Catify",
    "default_popup": "popup.html",
    "default_icon": {
      "16": "icons/icon16.png",
      "32": "icons/icon32.png",
      "48": "icons/icon48.png",
      "128": "icons/icon128.png"
    }
  }
}
```

### action

: 유저가 URL 표시줄 옆에 있는 아이콘을 클릭할 때 상호작용을 받을 수 있음을 의미미

- action 필드가 없거나 default_icon이 설정되지 않은 경우 확장 프로그램이 기본적으로 비활성화 상태로 표시됨
  (아이콘이 회색으로 표시 / 사용자와의 상호작용이 제한)

#### 구성 요소

- default_icon : 확장 프로그램이 활성화되어 있음을 의미하는 아이콘
- default_title : 아이콘을 클릭했을 때 상단에 보이는 제목
- default_popup : 아이콘을 클릭했을 때 보여줄 팝업 HTML 코드 (경로 명시)

## 주의사항

- 팝업 크기 제한 (일반적으로 800x600 픽셀)
- 빠른 로딩 시간 유지
- 간결한 UI/UX 디자인
- 메모리 사용량 최적화
