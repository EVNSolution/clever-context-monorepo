# override guide

## 목적

이 문서는 customer-specific override를 어디까지 허용하는지 정리한다.

override는 current deploy baseline 위에 덧붙는 차이만 다룬다.

## 허용되는 override 범위

- image/tag 차이
- env category 차이
- secret 참조 차이
- 외부 연동 endpoint 또는 credential 차이
- rollout 단위 차이

## baseline 변경으로 다뤄야 하는 경우

아래는 override가 아니라 baseline 변경 또는 migration 성격으로 기록한다.

- app repo가 직접 deploy를 수행하도록 ownership을 바꾸는 경우
- central release를 우회하는 경우
- runtime inventory 없이 deploy scope를 정하려는 경우
- immutable artifact 규칙을 깨는 경우
- public contract probe를 생략하는 경우

## 주의

- 공통 deploy baseline 자체를 고객사별로 복제하지 않는다.
- service 문서에는 해당 서비스에서 실제로 쓰는 override만 기록한다.
- baseline 규칙을 바꾸는 변경이면 root 문서까지 같이 수정한다.
