# Architectural Decision Records

Este diretório armazena os ADRs (Architectural Decision Records) da feature de webhooks.
Cada decisão arquitetural relevante fica em um arquivo individual, nomeado no formato
`ADR-NNN-titulo-em-kebab-case.md`.

| ADR | Decisão |
| --- | --- |
| [ADR-001](ADR-001-outbox-no-mysql.md) | Padrão Outbox no MySQL existente |
| [ADR-002](ADR-002-worker-em-processo-separado-com-polling.md) | Worker em processo separado com polling de 2 segundos |
| [ADR-003](ADR-003-worker-unico-e-ordenacao-por-pedido.md) | Worker único e ordenação apenas por pedido |
| [ADR-004](ADR-004-retry-com-backoff-e-dlq.md) | Retry com backoff exponencial e DLQ em tabela separada |
| [ADR-005](ADR-005-hmac-sha256-com-secret-por-endpoint.md) | HMAC-SHA256 com secret por endpoint |
| [ADR-006](ADR-006-at-least-once-com-x-event-id.md) | Entrega at-least-once com `X-Event-Id` |
| [ADR-007](ADR-007-reuso-dos-padroes-do-projeto.md) | Reuso dos padrões existentes do projeto |
| [ADR-008](ADR-008-snapshot-e-filtro-na-insercao.md) | Filtro e snapshot do payload na inserção |
