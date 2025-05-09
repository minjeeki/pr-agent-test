# 스크립트 종류

크롬 확장 프로그램은 여러 종류의 스크립트를 사용하여 다양한 기능을 구현합니다.

## Background Script

- 확장 프로그램의 백그라운드에서 실행
- 브라우저 이벤트 모니터링 및 처리
- 지속적인 작업 수행
- 메모리 관리 중요

## Content Script

- 웹 페이지에 주입되는 스크립트
- 페이지의 DOM 조작 가능
- 페이지 콘텐츠 수정
- 외부 API와 통합

## Popup Script

- 팝업 UI의 동작 제어
- 사용자 인터페이스 이벤트 처리
- 확장 프로그램 설정 관리
- 사용자 상호작용 처리

## 각 스크립트의 특징

| 스크립트 유형 | 실행 환경          | 접근 가능한 API      | 주요 용도                    |
| ------------- | ------------------ | -------------------- | ---------------------------- |
| Background    | 확장 프로그램 내부 | chrome.\* API 전체   | 이벤트 처리, 백그라운드 작업 |
| Content       | 웹 페이지 내부     | 제한된 chrome.\* API | 페이지 조작, 콘텐츠 수정     |
| Popup         | 팝업 UI 내부       | chrome.\* API 전체   | UI 제어, 사용자 상호작용     |
