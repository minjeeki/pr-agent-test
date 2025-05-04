# 스토리지 옵션

크롬 확장 프로그램에서 데이터를 저장하고 관리하기 위한 다양한 스토리지 옵션을 제공합니다.

## 스토리지 유형

### chrome.storage.local

- 로컬 저장소
- 사용자의 컴퓨터에만 저장
- 용량 제한: 약 5MB
- 영구 저장

### chrome.storage.sync

- 동기화 저장소
- 사용자의 Chrome 계정에 저장
- 여러 기기에서 동기화
- 용량 제한: 약 100KB

### chrome.storage.session

- 세션 저장소
- 브라우저 세션 동안만 유지
- 메모리 기반 임시 저장
- 페이지 새로고침 시 유지

## 사용 예시

```javascript
// 데이터 저장
chrome.storage.local.set({ key: value }, function () {
  console.log("저장 완료");
});

// 데이터 조회
chrome.storage.local.get(["key"], function (result) {
  console.log(result.key);
});
```

## 주의사항

- 데이터 크기 제한 준수
- 민감한 정보 저장 시 암호화 고려
- 동기화 저장소 사용 시 네트워크 상태 고려
- 에러 처리 구현
