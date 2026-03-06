# Backend — rules

- Keep controllers thin: parse input, call service, return response.
- Put business logic in services/use-cases, not in controllers or repositories.
- Use repositories or data layer for persistence only; no business rules there.
- Validate input at the boundary; use DTOs or schemas for request/response shapes.
- Return consistent error shapes and HTTP status codes; document in API specs.
- Do not hardcode secrets or env-specific values; use configuration or env vars.
- Do not implement UI or change frontend code; only expose and document APIs.
