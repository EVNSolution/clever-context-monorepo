# Services Pointer

이 디렉터리는 compatibility entry다.

`clever-context-monorepo`는 runtime slice별 상세 문서를 소유하지 않는다.
`service-*` 디렉터리별 API, env, ECR, commit, rollout caveat, README 요약을
이 repo에 복제하지 않는다.

CLEVER MSA platform은 product service 단위에서 하나로 보고, 그 내부 구현
단위는 runtime slice로 부른다.

runtime slice 정본은 아래를 따른다.

1. `clever-msa-platform:WORKSPACE.md`
2. `clever-msa-platform:repo-map.md`
3. `clever-msa-platform:docs/mappings/current-runtime-inventory.md`
4. `clever-msa-platform:docs/mappings/repo-responsibility-matrix.md`

새 runtime slice나 boundary 변경은 소유 repo의 정본 문서에서 먼저 반영한다.
이 repo에는 정본 위치가 바뀐 경우에만 pointer를 갱신한다.

## Product service pointers

| service_id | owner_repo | context pointer | current authority | template lineage |
| --- | --- | --- | --- | --- |
| `clever-driver-app` | <https://github.com/EVNSolution/clever-driver-app> | <https://github.com/EVNSolution/clever-context-monorepo/blob/main/docs/services/clever-driver-app/index.md> | <https://github.com/EVNSolution/clever-driver-app/tree/dev> | Expo / React Native native iOS and Android app bootstrap |
| `clever-shopify-app` | <https://github.com/EVNSolution/shopify-clever> | <https://github.com/EVNSolution/clever-context-monorepo/blob/main/docs/services/clever-shopify-app/index.md> | <https://github.com/EVNSolution/shopify-clever/tree/main/apps/shopify-app> and <https://github.com/EVNSolution/shopify-clever/blob/main/NAMING.md> | React Router Shopify embedded admin app paired with delivery API |
| `clever-delivery-server` | <https://github.com/EVNSolution/shopify-clever> | <https://github.com/EVNSolution/clever-context-monorepo/blob/main/docs/services/clever-delivery-server/index.md> | <https://github.com/EVNSolution/shopify-clever/tree/main/apps/delivery-api> | Fastify / Prisma delivery API runtime paired with Shopify admin and driver app |
| `thundercrew-domain` | `EVNSolution/thundercrew-domain` | <https://github.com/EVNSolution/clever-context-monorepo/blob/main/docs/services/thundercrew-domain/index.md> | `EVNSolution/thundercrew-domain#106`, `EVNSolution/clever-change-control#98` | `Clever-OIDC-deploy` with existing-EC2 override; `msa-template` boundary principles |
