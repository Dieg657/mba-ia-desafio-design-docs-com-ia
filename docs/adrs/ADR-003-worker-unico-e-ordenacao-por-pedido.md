# ADR-003: Operar com worker único e ordenação apenas por pedido

## Status

Aceito

- **Decidido em:** reunião técnica, [09:13] Larissa (registrado como limitação conhecida)
- **Relacionados:** [ADR-002](ADR-002-worker-em-processo-separado-com-polling.md) (worker), [ADR-004](ADR-004-retry-com-backoff-e-dlq.md) (retry)

## Contexto

A reunião avaliou o caso de um pedido que muda de PAID para PROCESSING e depois para SHIPPED em sequência rápida ([09:12] Larissa). Essa sequência é válida na máquina de estados do pedido (`src/modules/orders/order.status.ts`).

**Direcionadores**

- **ADR-003-CTX-01:** com um único worker, o processamento segue a ordem de `created_at` da outbox e o cliente recebe em ordem ([09:12] Diego).
- **ADR-003-CTX-02:** com múltiplos workers em paralelo, essa garantia se perde ([09:12] Diego).
- **ADR-003-CTX-03:** os clientes nunca pediram ordenação global; querem saber se cada pedido mudou ([09:14] Marcos).

## Decisão

A feature opera com um único worker. A ordem de entrega é implícita por `order_id` e vale apenas enquanto houver um só worker ([09:12] Diego). Não há garantia de ordenação global, e isso fica documentado como limitação conhecida ([09:13] Larissa).

A reunião não definiu como eventos em retry afetam a ordem dos eventos seguintes do mesmo pedido. O autor do pacote registrou uma escolha, pendente de confirmação dos revisores, em FDD-AUT-01 e RFC-QA-08. Ela não é decisão da reunião.

## Alternativas Consideradas

### ADR-003-ALT-01: Múltiplos workers com particionamento por `order_id`

- **Origem:** discutida em [09:13] Diego
- **Prós:** permitiria escalar o processamento.
- **Contras:** adiciona uma complexidade que a reunião não considerou necessária agora.
- **Por que foi descartada:** adiada; "isso é problema do futuro, não agora" ([09:13] Diego).

### ADR-003-ALT-02: Múltiplos workers com lock pessimista

- **Origem:** discutida em [09:13] Diego
- **Prós:** também abriria caminho para escalar.
- **Contras:** exige coordenação entre workers, hoje desnecessária com um só worker.
- **Por que foi descartada:** adiada junto com a alternativa anterior ([09:13] Diego).

### ADR-003-ALT-03: Garantia de ordenação global

- **Origem:** discutida em [09:13] Larissa
- **Prós:** daria ao cliente uma ordem única entre todos os eventos.
- **Contras:** atende a uma necessidade que os clientes não apresentaram.
- **Por que foi descartada:** os clientes nunca pediram essa garantia ([09:14] Marcos).

## Consequências

### Positivas

- **ADR-003-CON-01:** enquanto houver um só worker, os eventos seguem a ordem de `created_at` sem mecanismo adicional de coordenação ([09:12] Diego).
- **ADR-003-CON-02:** a primeira entrega não precisa resolver particionamento nem locks entre workers ([09:13] Diego).

### Negativas

- **ADR-003-CON-03:** escalar para vários workers exige rever esta decisão, porque a garantia se perde ([09:12] Diego).
- **ADR-003-CON-04:** clientes não podem contar com ordem entre pedidos diferentes ([09:13] Larissa).
- **ADR-003-CON-05:** análise a validar em revisão: a vazão fica limitada a um único worker, que pode aguardar até 10 segundos em cada cliente lento ([09:42] Diego). A reunião não discutiu o efeito disso sobre os demais clientes.

### Trade-off aceito

**ADR-003-TO:** aceitamos operar com um só worker e sem ordem global em troca de ordenação por pedido sem coordenação entre processos ([09:13] Larissa, [09:14] Marcos).

## Referências

**Transcrição**

- [09:12] Diego: comportamento com um ou vários workers.
- [09:13] Larissa: registro da limitação.
- [09:14] Marcos: ausência de pedido por ordem global.

**Código**

- **ADR-003-COD-01:** `src/modules/orders/order.status.ts`: transições que tornam possível a sequência PAID, PROCESSING, SHIPPED.

## Changelog

Detalhes e IDs afetados na seção Changelog de [TRACKER.md](../TRACKER.md).

| CHG | Resumo |
| --- | --- |
| CHG-000 | Versão inicial, linha de base do pacote |
| CHG-022 | Nota sobre retry e ordem aponta para a escolha do autor; trade-off sem a limitação de vazão, que não foi dita |
| CHG-045 | Link para o tracker com rótulo relativo a este arquivo e sem âncora |
