# deploy checklist

## 배포 전

- image build 대상과 immutable tag 정책이 확인돼 있다.
- central release가 사용할 artifact 기준이 확인돼 있다.
- runtime inventory에 대상 workload가 등록돼 있다.
- 필요한 env/secret category가 정리돼 있다.
- monorepo면 single workload 기준이, MSA면 bundle 또는 workload scope가 확인돼 있다.
- rollback 시 되돌릴 artifact/tag와 절차가 정리돼 있다.

## 배포 직후

- 진입 URL이 정상 응답한다.
- 핵심 초기 API 요청이 기대한 status를 반환한다.
- browser console 또는 runtime log에 치명 오류가 없다.
- 변경된 public contract가 실제 endpoint 기준으로 기대한 shape를 반환한다.

## 배포 완료 판단

- build success만으로 완료로 닫지 않는다.
- central release success만으로 완료로 닫지 않는다.
- public contract probe까지 통과해야 완료로 본다.

## scope 확인

- monorepo는 single workload proof가 남아야 한다.
- MSA는 changed workload 또는 bundle scope proof가 남아야 한다.
