# 도메인 용어집

## 공통 용어

- project-start issue #: root 작업 라인을 식별하는 canonical 시작 식별자
- change id: 승인 후 scope가 고정된 실행 단위를 식별하는 값
- target repo: 변경이 반영되는 저장소
- product service: 사용자가 하나의 서비스로 인식하는 제품/플랫폼 단위. CLEVER MSA platform은 이 층에서 하나로 본다.
- runtime slice: product service를 구성하는 구현/배포 단위. 예: `service-*`, `front-*`, `edge-*`, `runtime-*`
- workload: compose, ECS, release inventory에서 다루는 실행 단위
- target slice: 변경이 반영되는 runtime slice
- spec: 기능과 동작 기준 문서
- ui-spec: 화면, 상태, 상호작용 기준 문서
- contract: 서비스 간 또는 전역 규칙의 약속
- generated summary: 자동 생성된 요약 문서. 이 repo의 정본 입력으로 쓰지 않는다.

## 배차/근태 용어

- 배송없음 row: 배차표에서 `박스수 = 0` 이고 `가구수 = 0` 인 row를 뜻한다. 정산 코드가 아니라 배차 해석 입력으로 본다.

## 사용 기준

- 용어는 문서마다 같은 의미로 사용한다.
- `service-*` 디렉터리를 product service라고 부르지 않는다.
- target이 애매하면 `target slice`를 먼저 쓰고, 사용자가 보는 상위 단위는 `product service`로 분리한다.
- 새로운 용어를 추가할 때는 먼저 이 문서를 갱신한다.
