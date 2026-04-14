# deploy checklist

## 배포 전

- image build 대상과 tag 정책이 확인돼 있다.
- 필요한 env/secret 범주가 정리돼 있다.
- central deploy 대상 bundle과 runtime 경로가 확인돼 있다.
- rollback 시 되돌릴 image/tag와 절차가 정리돼 있다.

## 배포 직후

- 진입 URL이 정상 응답한다.
- 핵심 초기 API 요청이 기대한 status를 반환한다.
- browser console 또는 runtime log에 치명 오류가 없다.

## 배포 완료 판단

- build success만으로 완료로 닫지 않는다.
- central deploy success만으로 완료로 닫지 않는다.
- public contract probe까지 통과해야 완료로 본다.
