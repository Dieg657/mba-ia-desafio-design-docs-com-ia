# ADR-001: Registrar eventos de webhook com padrão Outbox no MySQL existente

## Status

Aceito

- **Decidido em:** reunião técnica, [09:08] Larissa; reafirmado em [09:48] Larissa
- **Relacionados:** [ADR-002](ADR-002-worker-em-processo-separado-com-polling.md) (consumo da outbox), [ADR-006](ADR-006-at-least-once-com-x-event-id.md) (garantia de entrega), [ADR-008](ADR-008-snapshot-e-filtro-na-insercao.md) (conteúdo do evento)

## Contexto

Clientes B2B pediram notificação quando o status dos pedidos muda ([09:00] Marcos). A mudança de status acontece em `OrderService.changeStatus`, dentro de um `prisma.$transaction` que atualiza o pedido, grava o histórico e movimenta estoque nas transições aplicáveis (`src/modules/orders/order.service.ts`). A aplicação não tem hoje fila nem mensageria: as dependências em `package.json` e os serviços em `docker-compose.yml` não incluem nenhum componente desse tipo.

A reunião precisou definir como acoplar essa transação a uma entrega HTTP externa sem perder eventos.

**Direcionadores**

- **ADR-001-CTX-01:** uma chamada HTTP dentro da transação faria um cliente lento travar mudanças de status de outros pedidos ([09:04] Bruno).
- **ADR-001-CTX-02:** cliente fora do ar não pode provocar rollback da mudança de status ([09:04] Bruno).
- **ADR-001-CTX-03:** se a transação principal confirmou, o evento precisa estar registrado; se ela reverteu, o evento some junto ([09:06] Diego).
- **ADR-001-CTX-04:** o time é pequeno, e subir Redis Cluster para isso foi considerado overengineering ([09:07] Diego).

## Decisão

Eventos de webhook serão gravados na tabela `webhook_outbox`, no MySQL existente, dentro da mesma transação de `changeStatus` ([09:06] Diego, [09:08] Larissa). Se a inserção falhar, a transação inteira sofre rollback: não pode haver status alterado sem evento ([09:40] Bruno).

A inserção será feita pela função `publishWebhookEvent(tx, order, fromStatus, toStatus)`, que recebe o client da transação em curso ([09:41] Bruno, [09:41] Diego). As chamadas HTTP ficam fora da transação, em um worker separado ([09:06] Diego).

## Alternativas Consideradas

### ADR-001-ALT-01: Disparo HTTP síncrono no service de orders

- **Origem:** discutida em [09:03] Larissa
- **Prós:** não exige tabela nem processo novos.
- **Contras:** acopla a mudança de status à disponibilidade e à latência de cada cliente.
- **Por que foi descartada:** "Síncrono não rola" ([09:04] Bruno) e "está fora de questão" ([09:06] Diego).

### ADR-001-ALT-02: Redis Streams ou Redis Cluster

- **Origem:** discutida em [09:07] Larissa
- **Prós:** mecanismo dedicado a fluxo de eventos.
- **Contras:** exige subir mais infraestrutura ([09:07] Larissa).
- **Por que foi descartada:** overengineering para um time pequeno; a outbox no MySQL existente resolve ([09:07] Diego).

### ADR-001-ALT-03: Injetar um repository de webhook no OrderService

- **Origem:** discutida em [09:41] Bruno
- **Prós:** segue a forma como os repositories já são injetados nos services.
- **Contras:** traz mais dependência do que o necessário para uma única inserção.
- **Por que foi descartada:** uma função recebendo o `tx` basta, sem injetar o repository inteiro ([09:41] Diego).

## Consequências

### Positivas

- **ADR-001-CON-01:** commit e rollback passam a valer para o status e para o evento ao mesmo tempo ([09:06] Diego).
- **ADR-001-CON-02:** a entrega deixa de influenciar a duração da transação e o resultado da mudança de status ([09:04] Bruno).
- **ADR-001-CON-03:** nenhuma infraestrutura nova é adicionada, porque a solução usa o MySQL existente ([09:07] Diego).

### Negativas

- **ADR-001-CON-04:** a tabela acumula eventos, o que motivou a dúvida sobre a performance do worker ([09:07] Bruno). A resposta foi indexar status e `created_at` e ler em batch pequeno ([09:08] Diego).
- **ADR-001-CON-05:** o arquivamento de linhas entregues, "depois de 30 dias ou assim", fica fora desta feature ([09:08] Diego). Análise: até lá, a tabela só cresce.
- **ADR-001-CON-06:** `changeStatus`, que já é uma transação pesada ([09:04] Bruno), ganha mais escritas, e uma falha na outbox passa a impedir a mudança de status ([09:40] Bruno).

### Trade-off aceito

**ADR-001-TO:** aceitamos mais escrita e crescimento de tabela no banco transacional em troca de consistência entre status e evento, sem operar infraestrutura nova ([09:07] Bruno, [09:07] Diego, [09:08] Larissa).

## Referências

**Transcrição**

- [09:04] Bruno: motivos contra o disparo síncrono.
- [09:06] Diego: explicação e garantia do padrão.
- [09:40] Bruno: rollback se a inserção falhar.
- [09:41] Bruno: proposta de `publishWebhookEvent`.

**Código**

- **ADR-001-COD-01:** `src/modules/orders/order.service.ts`: transação de `changeStatus` que receberá a inserção.
- **ADR-001-COD-02:** `src/modules/orders/order.status.ts`: regras que tornam o débito e a reposição de estoque condicionais à transição.
- **ADR-001-COD-03:** `docker-compose.yml`: MySQL como único serviço de infraestrutura.

## Changelog

Detalhes e IDs afetados na seção Changelog de [TRACKER.md](../TRACKER.md).

| CHG | Resumo |
| --- | --- |
| CHG-000 | Versão inicial, linha de base do pacote |
| CHG-026 | Crescimento da tabela rotulado como análise; trade-off cita também a dúvida sobre acúmulo |
| CHG-049 | Link para o tracker com rótulo relativo a este arquivo e sem âncora |
