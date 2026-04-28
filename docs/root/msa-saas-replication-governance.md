# MSA SaaS 복제 거버넌스

## 목적

이 문서는 MSA/SaaS 복제형 작업의 분기 규칙만 고정한다.

제품과 runtime 사실은 이 repo에 복제하지 않는다.
MSA 경계 자체의 판단은 [`msa-boundary-governance.md`](./msa-boundary-governance.md)를 따른다.

## 기준 용어

- product service: 사용자가 하나로 인식하는 제품/플랫폼 서비스
- runtime slice: product service를 구성하는 구현/배포 단위
- workload: runtime inventory와 release lane이 다루는 실행 단위

CLEVER MSA platform은 product service로는 하나다. `service-*`, `front-*`,
`edge-*`, `runtime-*`는 runtime slice다.

## 분기 규칙

아래에 가까우면 MSA/SaaS 복제형 작업으로 본다.

- 같은 product service를 고객사별로 복제 또는 변형한다.
- runtime slice, workload, image, env, secret, 외부 연동값의 차이를 다룬다.
- 공통 platform template 위에 customer-specific override를 얹는다.
- 데이터/table ownership, API edge, release/workload boundary를 고객사별로 분기한다.

아래에 가까우면 일반 개발 작업으로 본다.

- 단일 runtime slice 내부의 app-only 변경이다.
- product service, workload, deploy contract가 바뀌지 않는다.

## 문서 위치 규칙

- 전역 분기 규칙만 이 문서에 둔다.
- customer-specific 사실과 runtime slice별 값은 소유 repo 문서에 둔다.
- 이 repo에는 service leaf 문서를 만들지 않는다.
- target이 정해지면 `target service`가 아니라 `target slice`로 기록한다.
- 새 slice 여부는 책임, 데이터 소유권, API edge, workload 경계가 분리되는지로 판단한다.

## 하지 말아야 할 것

- product service와 runtime slice를 둘 다 `service`라고 부르지 않는다.
- 고객사별 차이를 이 repo에 요약하지 않는다.
- runtime proof나 rollout evidence를 이 repo에 올리지 않는다.
- 소유 repo 문서를 복제해서 context 문서로 만들지 않는다.
- 단순히 adapter, controller, 화면을 하나 더 붙이는 것을 MSA 분리로 기록하지 않는다.
