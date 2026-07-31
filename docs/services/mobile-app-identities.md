# Mobile App Identity Registry

This registry prevents naming regressions between the existing Shopify mobile
runtime and the future DSV mobile runtime. It records identity names only; it
does not create a service pointer for reserved apps.

| Field | Shopify delivery app | DSV delivery app |
| --- | --- | --- |
| display_name | `CLEVER Routes` | `CLEVER Driver` |
| repo_service_id | `clever-routes-app` | `clever-driver-app` |
| repository_status | current authority | reserved; not yet created |
| bundle_package_id | `com.evnsolution.clever.routes` | `com.evnsolution.clever.driver` |
| url_scheme | `clever-routes` | `clever-driver` |
| target | Shopify delivery | DSV delivery |
| context_pointer | [`docs/services/clever-routes-app/index.md`](./clever-routes-app/index.md) | not created; keep reserved until the DSV app repository exists |

## Interpretation

- `clever-routes-app` is the existing Shopify delivery mobile runtime and owns
  the `CLEVER Routes` identity.
- `clever-driver-app` is reserved for the future DSV delivery mobile runtime and
  owns the `CLEVER Driver` identity.
- Do not add a `docs/services/clever-driver-app/` service pointer until the DSV
  app repository exists and becomes a current authority.
