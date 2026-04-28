# 도메인 용어집

## 공통 용어

- project-start issue #: root 작업 라인을 식별하는 canonical 시작 식별자
- change id: 승인 후 scope가 고정된 실행 단위를 식별하는 값
- target repo: 변경이 반영되는 저장소
- product service: 사용자가 하나의 서비스로 인식하는 제품/플랫폼 단위. CLEVER MSA platform은 이 층에서 하나로 본다.
- runtime slice: product service를 구성하는 구현/배포 단위. 예: `service-*`, `front-*`, `edge-*`, `runtime-*`
- workload: compose, ECS, release inventory에서 다루는 실행 단위
- target slice: 변경이 반영되는 runtime slice
- API edge: runtime slice 사이의 호출, routing, public contract 노출, authentication handoff가 만나는 연결부. 데이터 소유권은 API edge가 아니라 owning slice가 가진다.
- owned data boundary: write authority를 가진 slice와 그 slice가 소유하는 table/read source 경계
- authority statement: 새 service/slice 후보가 데이터, API contract, 외부 provider, 운영 권한 중 무엇을 소유하는지 설명하는 문장
- read model: 원천 데이터를 직접 소유하지 않고 조회 목적에 맞게 투영한 모델. 원천 owning slice와 consumer slice를 구분해서 기록한다.
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
- 새 MSA slice 여부가 애매하면 책임, owned data boundary, API edge, workload boundary를 먼저 확인한다.
- 에이전트가 새 service/slice를 제안하면 authority statement와 대안 검토를 같이 남긴다.
- 새로운 용어를 추가할 때는 먼저 이 문서를 갱신한다.
