# ADR-007: Construir webhooks como módulo comum, reusando os padrões do projeto

## Status

Aceito

- **Decidido em:** reunião técnica, [09:30] Larissa; reafirmado em [09:48] Larissa
- **Relacionados:** [ADR-002](ADR-002-worker-em-processo-separado-com-polling.md) (entrada do worker), [ADR-004](ADR-004-retry-com-backoff-e-dlq.md) (uso de `requireRole`)

## Contexto

O projeto já tem padrões consolidados para módulos, erros, logs e validação. A reunião avaliou como a feature se encaixa neles.

A conferência no código confirmou os padrões citados: `src/modules/customers/` e `src/modules/orders/` têm controller, service, repository, routes e schemas; `AppError` e as classes derivadas estão em `src/shared/errors/`; `src/middlewares/error.middleware.ts` trata `AppError`, Zod e erros conhecidos do Prisma; e `src/shared/logger/index.ts` configura o Pino.

**Direcionadores**

- **ADR-007-CTX-01:** cada domínio é um módulo em `src/modules` com controller, service, repository, routes e schemas ([09:27] Bruno).
- **ADR-007-CTX-02:** erros seguem `AppError`, classes específicas e códigos como `INSUFFICIENT_STOCK` ([09:28] Bruno).
- **ADR-007-CTX-03:** o Pino já está no projeto inteiro e não será adicionado nada novo ([09:29] Bruno).
- **ADR-007-CTX-04:** o middleware centralizado foi apontado como suficiente para tratar os novos erros ([09:29] Bruno).

## Decisão

Reuso máximo do que já existe: `AppError`, Pino, error middleware, padrão de módulos, schemas Zod e padrão de códigos de erro. Webhook fica como um módulo igual aos outros ([09:30] Larissa), em `src/modules/webhooks` (a criar) ([09:27] Bruno).

Os erros do módulo usam o prefixo `WEBHOOK_` ([09:29] Larissa). A autorização de ADMIN reaproveita o `requireRole` existente ([09:36] Larissa). Identificadores seguem o padrão UUID do projeto ([09:51] Larissa).

## Alternativas Consideradas

### ADR-007-ALT-01: Adicionar componentes novos de log ou erro para o módulo

- **Origem:** (plausível, não discutida)
- **Prós:** daria liberdade para um tratamento específico de webhooks.
- **Contras:** criaria um segundo padrão para o que o projeto já resolve.
- **Por que foi descartada:** a decisão foi de reuso máximo ([09:30] Larissa). Sobre o logger, a fala foi "Não vamos botar nada novo" ([09:29] Bruno).

### ADR-007-ALT-02: Componente de webhooks fora de `src/modules`

- **Origem:** (plausível, não discutida)
- **Prós:** isolaria a feature do restante da aplicação.
- **Contras:** quebraria a organização por domínio.
- **Por que foi descartada:** a decisão registrada é tratar webhook como módulo igual aos outros ([09:30] Larissa).

## Consequências

### Positivas

- **ADR-007-CON-01:** erros do módulo passam pelo middleware atual sem alterá-lo ([09:29] Bruno).
- **ADR-007-CON-02:** o time trabalha com a mesma estrutura dos outros módulos ([09:27] Bruno).

### Negativas

Análise do código: a conferência mostrou limites do reuso que a reunião não abordou.

- **ADR-007-CON-03:** `NotFoundError` fixa o código `NOT_FOUND` em `src/shared/errors/http-errors.ts`. `WEBHOOK_NOT_FOUND` precisa de classe própria, derivada de `AppError`.
- **ADR-007-CON-04:** `src/middlewares/validate.middleware.ts` converte toda falha de schema em `VALIDATION_ERROR`. Sem alterar o middleware, erros de formato do corpo não recebem o prefixo `WEBHOOK_`.
- **ADR-007-CON-05:** a lista de campos ocultados em `src/shared/logger/index.ts` não inclui secret. O tratamento está sem definição em FDD-ND-18.

### Trade-off aceito

**ADR-007-TO:** reusar os componentes atuais traz consistência e nenhuma dependência nova de log ou erro ([09:29] Bruno, [09:30] Larissa). Análise do código: o custo são os limites listados acima, que a reunião não discutiu.

## Referências

**Transcrição**

- [09:27] Bruno: padrão de módulos e pasta `src/modules/webhooks`.
- [09:28] Bruno: classes e códigos de erro.
- [09:29] Larissa: prefixo `WEBHOOK_`.
- [09:30] Larissa: fechamento da decisão.

**Código**

- **ADR-007-COD-01:** `src/shared/errors/app-error.ts`: classe base `AppError`.
- **ADR-007-COD-02:** `src/shared/errors/http-errors.ts`: classes específicas e `NotFoundError`.
- **ADR-007-COD-03:** `src/middlewares/error.middleware.ts`: tratamento centralizado de erros.
- **ADR-007-COD-04:** `src/middlewares/validate.middleware.ts`: validação por schema Zod.
- **ADR-007-COD-05:** `src/shared/logger/index.ts`: logger Pino e campos ocultados.

## Changelog

Detalhes e IDs afetados na seção Changelog de [TRACKER.md](../TRACKER.md).

| CHG | Resumo |
| --- | --- |
| CHG-000 | Versão inicial, linha de base do pacote |
| CHG-024 | Alternativa de componentes novos marcada como não discutida; limites do reuso rotulados como análise; secret em log sem sugestão |
| CHG-047 | Link para o tracker com rótulo relativo a este arquivo e sem âncora |
