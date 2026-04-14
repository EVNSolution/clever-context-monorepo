# override guide

## 목적

이 문서는 customer-specific override를 어디까지 허용하는지 정리한다.

## 허용되는 override 범위

- image/tag 차이
- env 값 차이
- secret 참조 차이
- 외부 연동 endpoint 또는 credential 차이
- rollout 단위 차이

## 주의

- 공통 deploy baseline 자체를 고객사별로 복제하지 않는다.
- service 문서에는 해당 서비스에서 실제로 쓰는 override만 기록한다.
- baseline 규칙을 바꾸는 변경이면 root 문서까지 같이 수정한다.
