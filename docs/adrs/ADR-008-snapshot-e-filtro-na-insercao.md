# ADR-008: Filtrar eventos e gravar snapshot do payload na inserção da outbox

## Status

Aceito

- **Decidido em:** reunião técnica, [09:34] Bruno (filtro) e [09:52] Bruno (snapshot)
- **Relacionados:** [ADR-001](ADR-001-outbox-no-mysql.md) (inserção transacional), [ADR-006](ADR-006-at-least-once-com-x-event-id.md) (identidade do evento)

## Contexto

Cada endpoint escolhe os status que quer ouvir ([09:33] Bruno, [09:33] Marcos). Faltava definir em que momento esse filtro é aplicado e o que a linha da outbox guarda.

O snapshot foi decidido depois da saída de Marcos e Sofia e não aparece no resumo final de [09:48] Larissa. A origem dele é a conversa entre [09:51] Bruno e [09:52] Bruno.

**Direcionadores**

- **ADR-008-CTX-01:** se nenhum webhook do customer quer aquele status, não há por que gravar uma linha ([09:34] Bruno).
- **ADR-008-CTX-02:** o evento deve refletir o estado de quando o status mudou, mesmo que o pedido mude depois ([09:52] Larissa).

## Decisão

O filtro por status é aplicado na inserção da outbox, e nada é inserido quando nenhum webhook do customer quer aquele status ([09:34] Bruno, [09:34] Diego).

A linha da outbox guarda o payload já renderizado, como um snapshot feito na inserção ([09:52] Larissa, [09:52] Diego, [09:52] Bruno).

## Alternativas Consideradas

### ADR-008-ALT-01: Filtrar na hora do envio

- **Origem:** discutida em [09:34] Diego
- **Prós:** o filtro em vigor seria avaliado no momento da entrega.
- **Contras:** grava linhas que podem nunca ser enviadas.
- **Por que foi descartada:** filtrar na inserção economiza linhas na tabela ([09:34] Bruno).

### ADR-008-ALT-02: Guardar só `order_id` e renderizar no envio

- **Origem:** discutida em [09:51] Bruno
- **Prós:** a linha da outbox ficaria menor.
- **Contras:** se o pedido mudar depois, o evento deixa de refletir o estado da mudança.
- **Por que foi descartada:** "Senão tem caso esquisito" ([09:52] Larissa).

## Consequências

### Positivas

- **ADR-008-CON-01:** a outbox recebe apenas eventos com algum interessado ([09:34] Bruno).
- **ADR-008-CON-02:** o evento continua refletindo o estado de quando o status mudou ([09:52] Larissa). Análise: reenvios e replays usam esse mesmo conteúdo.

### Negativas

- **ADR-008-CON-03:** análise: como o filtro é avaliado na inserção ([09:34] Bruno), uma alteração de filtro só vale para as próximas mudanças de status.
- **ADR-008-CON-04:** cada linha guarda o payload completo, o que torna importante mantê-lo enxuto, sem items ([09:43] Diego, [09:44] Bruno).
- **ADR-008-CON-05:** análise: a inserção faz parte da transação de `changeStatus` ([09:40] Bruno), que passa a incluir a consulta do filtro e a renderização.

**Não definido na reunião:** como um evento atende a vários webhooks do customer. A fala registrada é "uma linha" com o evento ([09:06] Diego), e o ponto está sem definição em FDD-ND-01 e RFC-QA-09.

### Trade-off aceito

**ADR-008-TO:** aceitamos mais trabalho e mais dados na inserção em troca de menos linhas sem interessados e de eventos que preservam o estado do momento da mudança ([09:34] Bruno, [09:52] Larissa).

## Referências

**Transcrição**

- [09:34] Bruno: filtro na inserção.
- [09:52] Larissa: motivo do snapshot.
- [09:43] Diego: payload sem items.

**Código**

- **ADR-008-COD-01:** `src/modules/orders/order.service.ts`: o bloco de `changeStatus` já tem os dados do pedido e os status de origem e destino.
- **ADR-008-COD-02:** `prisma/schema.prisma`: `totalCents`, `orderNumber` e `customerId` existem no modelo `Order`.

## Changelog

Detalhes e IDs afetados na seção Changelog de [TRACKER.md](../TRACKER.md).

| CHG | Resumo |
| --- | --- |
| CHG-000 | Versão inicial, linha de base do pacote |
| CHG-025 | Consequências não ditas rotuladas como análise; nota sobre vários webhooks sem proposta |
| CHG-048 | Link para o tracker com rótulo relativo a este arquivo e sem âncora |
