# ADR-004: Aplicar retry com backoff exponencial e DLQ em tabela separada

## Status

Aceito

- **Decidido em:** reunião técnica, [09:17] Larissa (retry), [09:19] Larissa (replay) e [09:36] Larissa (permissão); reafirmado em [09:48] Larissa
- **Relacionados:** [ADR-002](ADR-002-worker-em-processo-separado-com-polling.md) (executor), [ADR-006](ADR-006-at-least-once-com-x-event-id.md) (reenvios)

## Contexto

Clientes podem estar offline quando o worker tenta entregar ([09:14] Larissa). Era preciso definir quantas vezes tentar de novo, em que intervalos e o que fazer depois disso.

**Direcionadores**

- **ADR-004-CTX-01:** retry indefinido deixaria evento pendurado para sempre se o cliente sumisse ([09:15] Diego).
- **ADR-004-CTX-02:** já houve cliente com duas horas de indisponibilidade em manutenção planejada ([09:16] Diego).
- **ADR-004-CTX-03:** uma janela de quase 15 horas foi considerada aceitável, porque um cliente fora por esse tempo "já tá com problema sério dele" ([09:17] Marcos).
- **ADR-004-CTX-04:** a falha precisa ficar guardada como evidência para debug e reprocessamento ([09:18] Diego).

## Decisão

A entrega usa backoff exponencial, com 5 tentativas nos intervalos de 1 minuto, 5 minutos, 30 minutos, 2 horas e 12 horas ([09:17] Diego, [09:17] Larissa). Resposta que não chega em 10 segundos conta como falha e vai para retry ([09:42] Diego).

**ADR-004-INT-01:** leitura adotada para "5 tentativas": são 5 reenvios depois do envio inicial. Só assim os cinco intervalos se somam em 14h36, compatível com "quase 15 horas entre primeira falha e última tentativa" ([09:17] Diego). A reunião não afirmou isso literalmente, e o resumo final diz "total 5 tentativas" ([09:48] Larissa). O ponto está fechado como interpretação em RFC-QA-07, e os revisores podem contestar.

Esgotadas as tentativas, a falha é considerada permanente e o evento vai para a DLQ ([09:15] Diego), na tabela separada `webhook_dead_letter`, com payload, motivo da falha e timestamp ([09:18] Diego).

O reprocessamento é manual, em `POST /admin/webhooks/dead-letter/:id/replay`, que recoloca o evento na outbox como pendente ([09:18] Diego). O endpoint exige role ADMIN, reaproveita o `requireRole` ([09:36] Larissa) e registra em log quem executou ([09:36] Sofia).

## Alternativas Consideradas

### ADR-004-ALT-01: Retry indefinido com backoff

- **Origem:** discutida em [09:15] Diego
- **Prós:** nunca desistiria da entrega.
- **Contras:** o evento pode ficar pendurado para sempre.
- **Por que foi descartada:** um teto de tentativas cobre a janela necessária sem esse problema ([09:15] Diego).

### ADR-004-ALT-02: Apenas 3 tentativas

- **Origem:** discutida em [09:16] Bruno
- **Prós:** política mais agressiva, encerrando a tentativa mais cedo ([09:16] Bruno).
- **Contras:** uma indisponibilidade pela manhã esgotaria tudo em 30 minutos ([09:16] Diego).
- **Por que foi descartada:** "3 é pouco" diante de manutenções de duas horas já ocorridas ([09:16] Diego).

### ADR-004-ALT-03: Marcar como `failed` na própria outbox

- **Origem:** discutida em [09:17] Larissa
- **Prós:** dispensa uma tabela adicional.
- **Contras:** mistura eventos em falha permanente com a leitura principal da outbox.
- **Por que foi descartada:** a tabela separada deixa a leitura da outbox mais limpa e serve de evidência ([09:18] Diego).

## Consequências

### Positivas

- **ADR-004-CON-01:** a janela cobre indisponibilidades como a manutenção de duas horas citada ([09:16] Diego).
- **ADR-004-CON-02:** nenhum evento fica em retry para sempre ([09:15] Diego).
- **ADR-004-CON-03:** falhas permanentes ficam guardadas para debug e reprocessamento ([09:18] Diego).
- **ADR-004-CON-04:** o replay fica restrito a ADMIN e deixa rastro de auditoria ([09:36] Sofia).

### Negativas

- **ADR-004-CON-05:** cliente fora por mais tempo do que a janela deixa de receber automaticamente e depende de replay manual ([09:18] Diego).
- **ADR-004-CON-06:** o evento pode levar quase 15 horas até chegar à DLQ ([09:17] Diego).
- **ADR-004-CON-07:** há mais uma tabela para modelar, prevista na estimativa de esforço ([09:46] Larissa).

### Trade-off aceito

**ADR-004-TO:** aceitamos desistir da entrega automática depois de quase 15 horas, com reprocessamento manual, em troca de não manter eventos pendurados indefinidamente ([09:15] Diego, [09:17] Marcos).

## Referências

**Transcrição**

- [09:17] Larissa: fechamento de tentativas e progressão.
- [09:18] Diego: tabela de DLQ e replay.
- [09:36] Sofia: exigência de ADMIN e log de auditoria.
- [09:42] Diego: timeout tratado como falha.

**Código**

- **ADR-004-COD-01:** `src/middlewares/auth.middleware.ts`: `requireRole`, reaproveitado no replay.
- **ADR-004-COD-02:** `src/modules/users/user.routes.ts`: uso existente de `requireRole('ADMIN')`.

## Changelog

Detalhes e IDs afetados na seção Changelog de [TRACKER.md](../TRACKER.md).

| CHG | Resumo |
| --- | --- |
| CHG-000 | Versão inicial, linha de base do pacote |
| CHG-029 | Leitura de "5 tentativas" alinhada a RFC-QA-07, fechada como interpretação |
| CHG-037 | Interpretação passa a registrar o resumo final da reunião |
| CHG-052 | Link para o tracker com rótulo relativo a este arquivo e sem âncora |
