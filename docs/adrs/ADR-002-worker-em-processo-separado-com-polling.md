# ADR-002: Consumir a outbox com worker em processo separado e polling de 2 segundos

## Status

Aceito

- **Decidido em:** reunião técnica, [09:10] Larissa (polling) e [09:12] Larissa (processo separado); reafirmado em [09:48] Larissa
- **Relacionados:** [ADR-001](ADR-001-outbox-no-mysql.md) (origem dos eventos), [ADR-003](ADR-003-worker-unico-e-ordenacao-por-pedido.md) (quantidade de workers)

## Contexto

Com a outbox definida, faltava decidir como os eventos seriam lidos e onde essa leitura rodaria ([09:08] Larissa). Para os clientes, qualquer tempo abaixo de 10 segundos já é "tempo real" ([09:02] Marcos).

Hoje a aplicação tem uma única entrada de processo, `src/server.ts`. O client do Prisma é criado em `src/config/database.ts`, a partir da mesma `DATABASE_URL` validada em `src/config/env.ts`.

**Direcionadores**

- **ADR-002-CTX-01:** MySQL não tem listener nativo como o NOTIFY/LISTEN do Postgres ([09:09] Diego).
- **ADR-002-CTX-02:** o intervalo de 2 segundos foi considerado suficiente para atender o requisito de menos de 10 segundos ([09:09] Diego, [09:10] Marcos).
- **ADR-002-CTX-03:** se o worker rodasse na instância da API, um reinício da API também o derrubaria ([09:11] Diego).
- **ADR-002-CTX-04:** o worker usa o mesmo banco e a mesma stack; só não pode ser o mesmo processo ([09:11] Diego).

## Decisão

O worker roda em loop de polling: a cada 2 segundos busca os eventos pendentes mais antigos, processa e marca o resultado ([09:09] Diego, [09:10] Larissa). A leitura é feita em batch pequeno ([09:08] Diego).

Ele roda em processo próprio, com a entrada `src/worker.ts` (a criar) e o script `npm run worker` ([09:11] Larissa). A lógica de processamento fica dentro do módulo de webhooks, em arquivo a criar ([09:28] Bruno). O processo usa a mesma `DATABASE_URL`, com instância própria de `PrismaClient`, porque é outro processo Node ([09:30] Bruno).

A reunião registrou a latência decorrente do polling com a frase "a latência mínima vai ser 2 segundos no pior caso. Aceitamos" ([09:10] Larissa). Nenhuma medição de latência de ponta a ponta foi apresentada.

## Alternativas Consideradas

### ADR-002-ALT-01: Trigger no banco para reagir às inserções

- **Origem:** discutida em [09:09] Bruno
- **Prós:** seria mais reativo do que o polling ([09:09] Bruno).
- **Contras:** a trigger só executa SQL e não notifica processo externo ([09:09] Diego).
- **Por que foi descartada:** avisar o worker exigiria improvisar, por exemplo escrevendo em arquivo ou chamando um endpoint ([09:09] Diego).

### ADR-002-ALT-02: Worker dentro da instância da API

- **Origem:** discutida em [09:11] Diego
- **Prós:** dispensa um processo adicional.
- **Contras:** o ciclo de vida do worker ficaria preso ao da API.
- **Por que foi descartada:** "se a API reinicia, perde o worker" ([09:11] Diego).

## Consequências

### Positivas

- **ADR-002-CON-01:** reiniciar a API não interrompe o worker ([09:11] Diego).
- **ADR-002-CON-02:** não há mecanismo de notificação improvisado entre banco e worker ([09:09] Diego).
- **ADR-002-CON-03:** a stack é reaproveitada, sem nova tecnologia para o consumo ([09:11] Diego).

### Negativas

- **ADR-002-CON-04:** o polling impõe uma espera até o próximo ciclo, aceita explicitamente pela reunião ([09:10] Larissa).
- **ADR-002-CON-05:** a aplicação passa a ter um segundo processo para iniciar e manter, com script e entrada próprios ([09:11] Larissa).
- **ADR-002-CON-06:** análise do código: como `src/config/env.ts` valida todas as variáveis ao carregar, o worker precisa receber a mesma configuração da API, inclusive `JWT_SECRET`, mesmo sem usá-lo.

### Trade-off aceito

**ADR-002-TO:** aceitamos a espera do polling e um processo a mais para operar em troca de uma solução simples, que atende ao limite de 10 segundos pedido e sobrevive a reinícios da API ([09:09] Diego, [09:10] Larissa, [09:11] Diego).

## Referências

**Transcrição**

- [09:09] Diego: polling, ausência de listener no MySQL e limite da trigger.
- [09:11] Diego: exigência de processo separado.
- [09:11] Larissa: `src/worker.ts` e `npm run worker`.
- [09:30] Bruno: instância própria de `PrismaClient`.

**Código**

- **ADR-002-COD-01:** `src/server.ts`: entrada de processo atual, usada como referência.
- **ADR-002-COD-02:** `src/config/database.ts`: criação do `PrismaClient`.
- **ADR-002-COD-03:** `src/config/env.ts`: validação de `DATABASE_URL` e demais variáveis.
- **ADR-002-COD-04:** `package.json`: local do novo script `worker`.

## Changelog

Detalhes e IDs afetados na seção Changelog de [TRACKER.md](../TRACKER.md).

| CHG | Resumo |
| --- | --- |
| CHG-000 | Versão inicial, linha de base do pacote |
| CHG-028 | Consequência sobre a configuração do worker rotulada como análise do código |
| CHG-051 | Link para o tracker com rótulo relativo a este arquivo e sem âncora |
