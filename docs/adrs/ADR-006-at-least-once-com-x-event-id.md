# ADR-006: Garantir entrega at-least-once com deduplicação por X-Event-Id

## Status

Aceito

- **Decidido em:** reunião técnica, [09:26] Larissa; reafirmado em [09:48] Larissa
- **Relacionados:** [ADR-001](ADR-001-outbox-no-mysql.md) (registro do evento), [ADR-004](ADR-004-retry-com-backoff-e-dlq.md) (reenvios), [ADR-005](ADR-005-hmac-sha256-com-secret-por-endpoint.md) (headers de entrega)

## Contexto

A entrega acontece por HTTP, fora da transação de mudança de status, e tem retries. A reunião definiu qual garantia de entrega a plataforma oferece e como o cliente lida com repetições.

**Direcionadores**

- **ADR-006-CTX-01:** o cliente pode receber o mesmo evento duas vezes e precisa estar preparado ([09:24] Diego).
- **ADR-006-CTX-02:** exactly-once exigiria coordenação dos dois lados e ficaria muito mais complexo ([09:25] Diego).
- **ADR-006-CTX-03:** foi citado que Stripe e GitHub adotam o mesmo modelo ([09:25] Diego).

## Decisão

A plataforma garante entrega at-least-once ([09:24] Diego, [09:26] Larissa).

Cada evento leva um `event_id` no header `X-Event-Id`, um UUID gerado quando o evento entra na outbox e único por evento. O cliente usa esse valor para deduplicar do lado dele ([09:25] Diego).

## Alternativas Consideradas

### ADR-006-ALT-01: Garantia exactly-once

- **Origem:** discutida em [09:25] Diego
- **Prós:** o cliente não precisaria tratar duplicatas.
- **Contras:** exige coordenação dos dois lados e fica muito mais complexo.
- **Por que foi descartada:** at-least-once com `event_id` foi considerado suficiente, resolvendo "99% dos casos" na avaliação de quem propôs ([09:25] Diego).

## Consequências

### Positivas

- **ADR-006-CON-01:** um evento recebido mais de uma vez pode ser identificado pelo cliente, porque o `event_id` é único por evento ([09:25] Diego).
- **ADR-006-CON-02:** a solução dispensa coordenação com os sistemas dos clientes ([09:25] Diego).

### Negativas

- **ADR-006-CON-03:** a responsabilidade pela deduplicação fica com o cliente ([09:25] Sofia).
- **ADR-006-CON-04:** o comportamento precisa estar bem destacado no portal de desenvolvedor, o que depende de Marcos ([09:26] Marcos).

### Trade-off aceito

**ADR-006-TO:** aceitamos duplicatas possíveis e deduplicação no cliente em troca de uma entrega muito mais simples do que exactly-once ([09:25] Diego, [09:26] Larissa).

## Referências

**Transcrição**

- [09:24] Diego: definição da garantia.
- [09:25] Diego: `X-Event-Id` e comparação com exactly-once.
- [09:25] Sofia: responsabilidade transferida ao cliente.
- [09:26] Larissa: fechamento da decisão.

**Código**

- **ADR-006-COD-01:** `package.json`: a dependência `uuid` já está disponível no projeto.

## Changelog

Detalhes e IDs afetados na seção Changelog de [TRACKER.md](../TRACKER.md).

| CHG | Resumo |
| --- | --- |
| CHG-000 | Versão inicial, linha de base do pacote |
| CHG-027 | Auditoria de citações e alternativas sem correções; adicionado o changelog |
| CHG-050 | Link para o tracker com rótulo relativo a este arquivo e sem âncora |
