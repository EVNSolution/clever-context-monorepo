# 문서 관리 기준

## 원칙

- 정본은 중복 작성하지 않는다.
- 같은 내용을 여러 곳에 둘 경우 root 또는 contracts 한곳을 정본으로 정하고 나머지는 링크만 둔다.
- 서비스 문서는 서비스 범위만 다룬다.
- generated summary는 참고용으로만 사용한다.

## 변경 기준

- 전역 규칙 변경은 `docs/root` 또는 `contracts`에서 먼저 반영한다.
- 서비스 범위 변경은 `docs/services`에서 반영한다.
- change package는 변경 시점의 해석 보조 자료로만 사용한다.

## 이슈 해결 시 context 정리 기준

target repo에서 이슈를 해결 완료로 표시하기 전에는 `clever-context-monorepo` 반영 필요 여부를 확인한다.
`dev` 또는 `main`으로 들어가는 PR이 있으면 context/wiki 판단과 업로드 상태를 PR metadata에 먼저 묶는다.

- 서비스 책임, API, 데이터 흐름, public contract, deploy/runtime 기준, env/secret category, 운영 caveat가 바뀌면 `docs/services/<service>/index.md`를 우선 갱신한다.
- 전역 규칙, 공통 용어, contracts 기준이 바뀌면 `docs/root` 또는 `contracts`를 우선 갱신한다.
- `docs/wiki`는 정본이 아니다. 빠른 탐색 링크, runtime proof, 짧은 요약이 필요할 때만 갱신한다.
- PR metadata에는 target branch, linked issue, 확인한 context 문서, `context wiki upload status`, service 문서 수정 여부, wiki 수정 여부, `clever-context-monorepo` commit 또는 PR을 남긴다.
- 문서 반영이 필요 없으면 PR metadata와 이슈 종료 코멘트에 불필요 사유를 남긴다.
- issue close는 PR metadata를 참조한다. 같은 context/wiki 판단을 이슈 종료 단계에서 다시 작성하지 않는다.
- 여러 이슈가 같은 서비스 문서를 건드리면 중복 요약을 만들지 말고 service 문서의 기존 섹션을 보강한다.

## dev/main PR context metadata 기준

`dev` PR은 integration merge 단위, `main` PR은 deploy merge 단위로 본다.
둘 다 code merge와 별개로 context/wiki 반영 결과를 PR metadata에 포함해야 한다.

- `context wiki upload status`: `updated` 또는 `not-needed`
- `service doc update`: 수정 파일 또는 `not-needed`
- `wiki update`: 수정 파일 또는 `not-needed`
- `clever-context-monorepo commit/PR`: context 반영 commit 또는 PR 링크
- `linked issue close evidence`: 이슈 종료 코멘트에 복사할 context/wiki 결과

wiki는 탐색 입구일 뿐이다. 정본 변경은 먼저 `docs/root`, `contracts`, `docs/services/<service>/index.md` 중 적절한 위치에 반영한다.

## 검토 기준

- 문서 간 용어가 일치해야 한다.
- 상위 우선순위 문서와 충돌하면 하위 문서를 수정한다.
- 실행자가 빠르게 판단할 수 있게 짧고 명확하게 작성한다.
