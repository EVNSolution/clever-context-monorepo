# MSA 경계 거버넌스

## 목적

이 문서는 CLEVER에서 MSA를 해석할 때의 전역 경계 규칙을 고정한다.

MSA는 서비스를 하나 더 붙이는 방식이 아니다. 역할과 책임을 먼저 나누고,
그 책임에 맞는 데이터 소유권과 API edge 계약을 정한 뒤 runtime slice와
workload를 연결하는 방식이다.

제품별 runtime 사실은 이 repo에 복제하지 않는다. 실제 slice 목록, 테이블,
API, release inventory는 소유 repo의 정본 문서를 따른다.

## 기준 원칙

- product service는 사용자가 하나로 인식하는 제품/플랫폼 단위다.
- runtime slice는 product service 안에서 하나의 책임을 소유하는 구현/배포 단위다.
- workload는 release lane과 runtime inventory가 다루는 실행 단위다.
- API edge는 slice 사이의 호출·라우팅·계약 연결부다. API edge 자체가 데이터 소유권을 만들지는 않는다.

runtime slice는 아래 질문에 답할 수 있어야 한다.

1. 이 slice가 소유하는 책임은 무엇인가?
2. 이 slice가 직접 쓰는 데이터나 read model은 무엇인가?
3. 다른 slice가 이 책임에 접근할 때 어떤 API edge 또는 contract를 거치는가?
4. deploy 가능한 slice라면 어떤 workload와 health/release boundary로 검증하는가?
5. 정본 문서는 어느 repo의 어느 문서인가?

위 질문에 답하지 못하면 새 service를 만든 것이 아니라 책임 없는 코드 묶음을
늘린 것으로 본다.

## 새 runtime slice 판단 규칙

새 기능이 들어올 때는 아래 순서로 판단한다.

1. 기존 slice 내부 변경인가?
   - 같은 책임, 같은 데이터 소유권, 같은 release boundary 안에서 끝나면 기존 slice 변경으로 둔다.
2. 기존 slice의 API contract 확장인가?
   - 책임은 그대로이고 외부 접근 방식만 늘어나면 새 slice가 아니라 contract/API edge 변경으로 둔다.
3. read model 또는 operations view인가?
   - 원천 데이터 소유자는 따로 있고 조회 책임만 분리하면 read model slice로 기록한다.
4. 새 runtime slice인가?
   - 새로운 책임, 독립된 데이터 소유권 또는 독립 release/workload 경계가 생길 때만 새 slice로 둔다.

편의상 컨트롤러, cron, adapter, 화면을 하나 더 붙이는 것만으로는 새 runtime
slice가 되지 않는다.

## 데이터와 테이블 경계

- write authority는 한 slice가 소유한다.
- 다른 slice는 소유 slice의 API, event, read model을 통해 접근한다.
- 공유 식별자는 contract일 뿐이며, 공유 테이블 소유권을 뜻하지 않는다.
- read model은 원천 소유자와 소비자 책임을 구분해서 문서화한다.
- 테이블을 추가하거나 ownership을 옮기는 변경은 app-only 변경이 아니라 boundary 변경으로 본다.

## API edge 경계

- gateway 또는 edge slice는 routing, authentication handoff, public contract 노출을 담당한다.
- upstream slice의 business responsibility와 데이터 소유권은 upstream slice 문서가 소유한다.
- gateway 문서는 라우팅과 공개 표면을 소유하고, upstream의 내부 API/env/ECR/runtime 사실을 복제하지 않는다.
- slice 간 연결을 바꾸면 source slice, consumer slice, API edge, contract owner를 함께 기록한다.

## 문서 위치 규칙

이 repo에는 전역 해석 규칙만 둔다.

허용:

- product service / runtime slice / workload / API edge의 용어와 판단 규칙
- 새 slice 여부를 판단하는 기준
- 데이터 소유권과 API edge의 전역 원칙
- 소유 repo 정본 위치 pointer

금지:

- runtime slice별 API, env, secret, ECR, port, health path
- 고객사별 테이블/필드/단가/외부 연동값
- release evidence, smoke proof, host 상태
- 소유 repo 문서 본문 복제

제품별 사실은 `clever-msa-platform` 같은 소유 repo 문서를 따른다. 이 repo는
정본 위치가 바뀌었을 때만 pointer를 갱신한다.

## change-control 기록 기준

MSA/SaaS 변경 요청에는 최소 아래 해석 값을 남긴다.

- product service
- target slice
- 새 slice 여부
- workload/release 영향 여부
- API edge 또는 contract 영향 여부
- 데이터/table ownership 영향 여부

기존 기록이 `target service`라고만 되어 있으면 실행 단계에서 `target slice`로
정규화한다. product service와 runtime slice를 같은 `service`라는 단어로
섞어 쓰지 않는다.
