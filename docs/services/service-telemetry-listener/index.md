# service-telemetry-listener

## 기준 source

- 분석 기준 repo: `/Users/jiin/Documents/Files/02_EVnSolution/00_Source_code/CLEVER/clever-msa-platform/development/service-telemetry-listener`
- 확인 기준 브랜치: `main`
- 확인한 최근 커밋: `014461c94756797272bcf4e6ffb5a839c6cf1e77 2026-04-08 feat: add image build pipeline for telemetry listener`

## 서비스 개요

`service-telemetry-listener` is the MQTT ingress worker for telemetry payloads.

## 주요 기능

- subscribe to MQTT topics from the broker
- parse source identity hints from incoming payloads
- forward raw-first ingest requests to `service-telemetry-hub`
- apply retry classification and log ingest outcomes

## 아키텍처와 실행 흐름

- `telemetry_listener.main`이 `ListenerConfig`를 로드하고 hub/dead-letter/MQTT client를 조립한다.
- `TelemetryListenerRuntime.run()`이 MQTT ingest loop를 수행한다.
- 수집 payload는 `service-telemetry-hub` raw ingest로 전달되고 실패 건은 dead-letter로 전송된다.

## 디렉터리 구조

```text
service-telemetry-listener/
├── telemetry_listener/
├── tests/
├── README.md
├── Dockerfile
├── entrypoint.sh
└── requirements.txt
```

## 핵심 엔트리포인트/모듈

- `telemetry_listener/`: 서비스 핵심 앱 디렉터리
- `telemetry_listener/main.py`: listener 진입점
- `telemetry_listener/config.py`: MQTT/hub/dead-letter 설정 로딩
- `tests/`: listener 테스트

## API / worker / batch / 외부 연동

### API
- 확인된 API route 없음 또는 별도 worker runtime

### Worker / batch
- 상시 worker: `python -m telemetry_listener.main`

### 외부 연동
- 외부/연동 설정: `TELEMETRY_DEAD_LETTER_BASE_URL`
- 외부/연동 설정: `TELEMETRY_DEAD_LETTER_INGEST_PATH`
- 외부/연동 설정: `TELEMETRY_DEAD_LETTER_KEY_SERVICE_TELEMETRY_LISTENER`
- 외부/연동 설정: `TELEMETRY_DEAD_LETTER_SOURCE_SERVICE`
- 외부/연동 설정: `TELEMETRY_HUB_BASE_URL`
- 외부/연동 설정: `TELEMETRY_HUB_INGEST_KEY`
- 외부/연동 설정: `TELEMETRY_HUB_INGEST_PATH`
- 외부/연동 설정: `TELEMETRY_LISTENER_CLIENT_ID`
- 외부/연동 설정: `TELEMETRY_LISTENER_IDLE_SLEEP_SECONDS`
- 외부/연동 설정: `TELEMETRY_LISTENER_LOG_LEVEL`
- 외부/연동 설정: `TELEMETRY_LISTENER_MQTT_HOST`
- 외부/연동 설정: `TELEMETRY_LISTENER_MQTT_PASSWORD`
- 외부/연동 설정: `TELEMETRY_LISTENER_MQTT_PORT`
- 외부/연동 설정: `TELEMETRY_LISTENER_MQTT_TOPICS`
- 외부/연동 설정: `TELEMETRY_LISTENER_MQTT_USERNAME`
- 외부/연동 설정: `TELEMETRY_LISTENER_RETRY_BACKOFF_SECONDS`
- 외부/연동 설정: `TELEMETRY_LISTENER_RETRY_COUNT`

## 로컬 실행 방법

- 확인된 worker 진입점은 `entrypoint.sh`다.
- 기본 실행 명령은 `python -m telemetry_listener.main`이다.

## 필수 환경변수

- `TELEMETRY_DEAD_LETTER_BASE_URL`
- `TELEMETRY_DEAD_LETTER_INGEST_PATH`
- `TELEMETRY_DEAD_LETTER_KEY_SERVICE_TELEMETRY_LISTENER`
- `TELEMETRY_DEAD_LETTER_SOURCE_SERVICE`
- `TELEMETRY_HUB_BASE_URL`
- `TELEMETRY_HUB_INGEST_KEY`
- `TELEMETRY_HUB_INGEST_PATH`
- `TELEMETRY_LISTENER_CLIENT_ID`
- `TELEMETRY_LISTENER_IDLE_SLEEP_SECONDS`
- `TELEMETRY_LISTENER_LOG_LEVEL`
- `TELEMETRY_LISTENER_MQTT_HOST`
- `TELEMETRY_LISTENER_MQTT_PASSWORD`
- `TELEMETRY_LISTENER_MQTT_PORT`
- `TELEMETRY_LISTENER_MQTT_TOPICS`
- `TELEMETRY_LISTENER_MQTT_USERNAME`
- `TELEMETRY_LISTENER_RETRY_BACKOFF_SECONDS`
- `TELEMETRY_LISTENER_RETRY_COUNT`

## 배포/운영 방식

- `.github/workflows/build-image.yml`가 `main` push와 수동 실행을 지원한다.
- AWS region: `ap-northeast-2`
- ECR repository: `service-telemetry-listener`
- allowed account id: `902837199612`
- 컨테이너 엔트리포인트는 `python -m telemetry_listener.main` 실행 방식이다.

## 운영 시 주의사항

- 외부 service base URL 의존성이 있어 upstream 장애 시 일부 API가 실패할 수 있다.
- MQTT 브로커, hub ingest key, dead-letter key가 모두 준비되지 않으면 기동에 실패한다.

## 확인 필요 항목

- 실제 운영 환경의 배포 manifest와 rollout 절차
- 로컬 개발용 `.env` 템플릿과 표준 실행 절차
- gateway 외부 prefix 또는 edge 라우팅 연결 여부
