# 코드 삽입 익스텐션 앱

코드 삽입 익스텐션 앱은 웹 페이지에 HTML, CSS, JavaScript 코드를 삽입하여 페이지의 콘텐츠나 동작을 수정하는 확장 프로그램입니다.

## 기본 구조

```
content-extension/
├── manifest.json
├── content.js
├── content.css
└── icons/
    ├── icon16.png
    ├── icon32.png
    ├── icon48.png
    └── icon128.png
```

## 주요 특징

### 페이지 수정 기능

- DOM 요소 추가/수정/삭제
- CSS 스타일 적용
- JavaScript 기능 추가
- 외부 API 통합

### manifest.json 설정

```json
{
  "manifest_version": 3,
  "name": "코드 삽입 익스텐션",
  "version": "1.0",
  "content_scripts": [
    {
      "matches": ["<all_urls>"],
      "css": ["content.css"],
      "js": ["content.js"]
    }
  ],
  "permissions": ["activeTab", "scripting"]
}
```

## 구현 예시

### content.js

```javascript
// 페이지 로드 시 실행
document.addEventListener("DOMContentLoaded", function () {
  // DOM 요소 수정
  const body = document.body;
  body.style.backgroundColor = "#f0f0f0";

  // 새로운 요소 추가
  const newElement = document.createElement("div");
  newElement.id = "extension-content";
  newElement.innerHTML = "확장 프로그램이 삽입한 내용";
  body.appendChild(newElement);

  // 이벤트 리스너 추가
  document.addEventListener("click", function (event) {
    console.log("클릭 이벤트 발생:", event.target);
  });
});
```

### content.css

```css
#extension-content {
  position: fixed;
  top: 10px;
  right: 10px;
  padding: 10px;
  background-color: white;
  border: 1px solid #ccc;
  border-radius: 5px;
  box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
}
```

## 사용 사례

1. 페이지 개선

   - 광고 제거
   - 다크 모드 적용
   - 폰트 최적화

2. 기능 추가

   - 번역 기능
   - 메모 기능
   - 북마크 기능

3. 데이터 수집
   - 가격 비교
   - 리뷰 분석
   - 통계 수집

## 주의사항

- 페이지 성능 영향 최소화
- 충돌 가능성 고려
- 보안 문제 주의
- 사용자 경험 고려

## 디버깅 방법

1. 개발자 도구 사용

   - 콘솔 로그 확인
   - DOM 변경 확인
   - 네트워크 요청 모니터링

2. 테스트 방법
   - 다양한 웹사이트에서 테스트
   - 에러 처리 구현
   - 성능 모니터링
