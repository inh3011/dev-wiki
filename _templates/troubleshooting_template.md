# Issue: title

## 문제 요약 (Issue Summary)

- 어떤 문제가 발생했는지 간단히 서술  
- 발생 위치/상황 (예: 특정 환경, 특정 요청 시 등)  
- 에러 메시지/현상 요약

예:  
빌드 환경에서 `NODE_ENV` 설정이 누락되어 Firebase Functions 초기화에 실패

---

## 발생 원인 (Root Cause)

- 원인을 어떻게 파악했는지 간략히 설명  
- 직접적인 트리거와 근본적인 원인을 구분

예:  
환경 변수 누락 → `initializeApp()`에서 undefined config 접근 → 런타임 에러

---

## 해결 방법 (Solution)

1. 어떤 순서로 문제를 해결했는지 단계별 정리
2. 해결 과정 중 시도했던 방법 (실패 포함)도 적을 수 있음
3. 설정 변경, 환경 수정, 문서 참고 등 구체적으로 명시

예:
1. `.env.production`에 누락된 `FIREBASE_CONFIG` 추가  
2. `firebase-admin` 초기화 로직 조건문 보완

---

## 참고 자료 (References)

- [공식 문서 링크 또는 GitHub 이슈](https://example.com)
- [[관련 내부 문서]] 또는 [[이전에 유사하게 발생한 이슈]]

---

## 추가 메모 및 유의사항 (Notes)

- 동일한 문제 재발을 막기 위한 조치 또는 고려사항  
- 환경 의존적인 특이사항, 타 팀 공유 시 참고할 내용

예:
- 로컬 환경에선 `.env.local`로 잘 작동하나, CI/CD에서 `.env.production`을 별도 관리해야 함  
- 다음 배포 전 `.env` 설정 자동 검증 스크립트 추가 필요
