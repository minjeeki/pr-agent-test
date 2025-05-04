# 메시지 전달 시스템

확장 프로그램의 각 부분이 서로 통신하기 위한 메시지 전달 시스템에 대해 설명합니다.

## 주요 메시지 전달 방법

### chrome.runtime.sendMessage

- 확장 프로그램의 다른 부분 간 메시지 전달
- 백그라운드 스크립트와 팝업 스크립트 간 통신
- 응답 처리 가능

### chrome.tabs.sendMessage

- 특정 탭과 통신
- 백그라운드 스크립트에서 컨텐트 스크립트로 메시지 전달
- 탭 ID 지정 필요

## 메시지 수신 방법

```javascript
// 메시지 수신 예시
chrome.runtime.onMessage.addListener(function (request, sender, sendResponse) {
  // 메시지 처리
  sendResponse({ response: "메시지 수신 완료" });
});
```

## 메시지 전송 예시

```javascript
// 메시지 전송 예시
chrome.runtime.sendMessage({ greeting: "안녕하세요" }, function (response) {
  console.log(response);
});
```

## 주의사항

- 비동기 통신 처리
- 메시지 형식 통일
- 에러 처리
- 메시지 크기 제한 고려
