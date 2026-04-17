# ev-dashboard runtime proof 요약

이 문서는 `ev-dashboard` runtime proof를 빠르게 다시 찾기 위한 내부 wiki 요약이다.

정본 판단은 아래로 되돌아간다.

- `clever-msa-platform/development/infra-ev-dashboard-platform/README.md`
- `clever-msa-platform/development/infra-ev-dashboard-platform/lesson.md`
- `clever-msa-platform/lesson.md`
- [`docs/templates/msa-template/index.md`](../templates/msa-template/index.md)
- [`docs/templates/msa-template/versions/v1.md`](../templates/msa-template/versions/v1.md)

## 왜 저장하나

이 항목은 template 정본에 직접 숫자와 run id를 과도하게 섞지 않기 위한 내부 검색용 저장소다.

즉 여기에는 아래를 적는다.

- 실제로 성공한 lane
- host size별 관측값
- bootstrap CPU와 post-smoke steady-state 차이
- strict full이 아직 아닌 이유

## proven lanes

### warm-host partial

- prod in-place partial deploy proof 있음
- 성공 run: `24508317262`
- 대상: `service-announcement-registry`
- old image: `dac56b6-slice6-20260414-150946`
- new image: `partial-rehearsal-20260416-202446`
- app host launch time 불변
  - meaning: EC2 replacement 없이 running host에서 서비스 1개만 reconcile 됨

이 proof에서 같이 닫힌 운영 조건:

- GitHub infra role에 `ssm:SendCommand`, `ssm:GetCommandInvocation` 필요
- direct SSM CLI 경로는 `PYTHONPATH=/opt/ev-dashboard/bootstrap` 필요
- internal-only backend는 public host-port service와 다른 drift rule이 필요

### cumulative expand / current full

- `strict full`은 아직 아님
- 현재 proven 최대치는 현재 운영 `full`
- meaning:
  - `core-entry`
  - `people-and-assets`
  - `dispatch-inputs`
  - `dispatch-read-models`
  - `settlement`
  - `support-surface`
  - `terminal-registry`
  - `telemetry-hub`
  - `telemetry-dead-letter`
  - `service-telemetry-listener`는 제외

strict full이 아직 아닌 이유:

- `service-telemetry-listener`에 필요한 `TELEMETRY_LISTENER_MQTT_HOST`와 실제 broker truth가 아직 닫히지 않음
- 따라서 현재 proof는 listener-disabled 상태의 현재 운영 `full`로 적는다

## host sizing evidence

### `t3.small`

- honest scope: `bootstrap-proof`
- cumulative lane은 `settlement`까지는 버팀
- `support-surface` 이후에는 CPU credit wall이 먼저 드러남
- worker tuning 전 관측:
  - `1561 MiB used / 178 MiB available` 근처까지 감소
- backend `GUNICORN_WORKERS=1` 적용 후:
  - 메모리는 완화됐지만
  - `support-surface` 이후 누적 bootstrap에서 `CPUCreditBalance=0`
  - public edge가 `502`로 무너짐

요약:

- `t3.small`의 hard limit은 RAM만이 아니라 RAM + CPU credit exhaustion

### `t3.medium`

- 성공 run: `24508999204`
- scope: current `full`
- backend `GUNICORN_WORKERS=1`

bootstrap/create-smoke 구간:

- CPU average about `66.5%`
- peak bucket maximum about `87.8%`
- `CPUCreditBalance=0`
- `CPUSurplusCreditBalance≈1.85`

post-smoke settled snapshot:

- `2096 MiB used / 1458 MiB available`
- load around `0.70 / 1.12 / 0.57`
- one-second `mpstat` sample about `98.51%` idle

요약:

- `t3.medium`은 현재 운영 `full`까지 proven
- 다만 tight minimum이지 comfortable headroom은 아님
- bootstrap은 burst-heavy, steady-state는 low CPU

### `t3.large`

- 17-service prod-like proof 있음
- 이후 현재 운영 `full`도 proof됨

17-service proof:

- post-smoke snapshot about `1602 MiB used / 5921 MiB available`
- deploy/smoke CPU average about `53.6%`
- peak about `84.4%`

current `full` proof:

- 성공 run: `24506332677`
- post-smoke snapshot about `2188 MiB used / 5334 MiB available`
- CloudWatch create-and-smoke window:
  - CPU average about `49.2%`
  - busiest 5-minute bucket average about `91.2%`
  - `CPUCreditBalance=0`
  - `CPUSurplusCreditBalance≈3.42`

요약:

- `t3.large`는 safer burstable margin
- 그래도 burst-free host는 아님

### `m6i.2xlarge`

- release-grade `RUN_PROFILE=full` proof host로 남아 있음
- all-in one-shot full proof를 credit behavior 없이 보고 싶을 때의 기준점

## worker count lesson

backend `GUNICORN_WORKERS=2 -> 1`은 단순 app tuning이 아니라 sizing evidence를 바꾼 공통 입력값이다.

관측:

- old backend memory range: about `75-82 MiB`
- worker `1` 후:
  - dev cumulative lane은 low-50 MiB대로 감소
  - prod full-ish proof에서는 대부분 `57-66 MiB`

해석:

- host size를 논의할 때는 worker count와 함께 적어야 한다
- `worker=2` 수치와 `worker=1` 수치를 섞으면 sizing 판단이 틀어진다

## bootstrap vs steady-state

이번 proof의 가장 중요한 lesson 중 하나는 bootstrap CPU와 steady-state CPU를 분리해야 한다는 점이다.

- bootstrap 동안:
  - image pull
  - container create
  - gateway/public smoke stabilization
  때문에 CPU가 높게 튄다
- post-smoke steady-state:
  - CPU는 매우 낮게 내려갈 수 있다

따라서 burstable host 결론을 남길 때는 항상 둘을 같이 적는다.

- bootstrap bucket
- post-smoke settled sample

둘 중 하나만 적으면 sizing 기록이 불완전하다.
