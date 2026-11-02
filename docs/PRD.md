# PRD: Order Management System, Sistema de Webhooks de Notificação de Pedidos

Versão: 1.4
Data: 2026-11-02
Responsável: Diego Soares, autor deste pacote de documentos (não é o Diego participante da reunião)

---

## Resumo e contexto da feature

**PRD-CTX-01:** clientes B2B da plataforma serão notificados automaticamente quando o status dos seus pedidos mudar, sem precisar consultar a API repetidamente ([09:00] Marcos).

**PRD-CTX-02:** a feature será implantada no Order Management System existente, como um novo módulo e um processo adicional, sem novo serviço de infraestrutura ([09:30] Larissa, [09:11] Diego, [09:07] Diego).

**PRD-CTX-03:** a notificação é apenas de saída: os clientes querem receber, não enviar ([09:02] Marcos). A gestão é feita pela API, sem painel visual nesta fase ([09:40] Larissa).

Como a solução será construída está no [RFC](RFC.md), nos [ADRs](adrs/README.md) e no [FDD](FDD.md).

---

## Problema e motivação

- **PRD-PROB-01:** Atlas Comercial, MaxDistribuição e Nova Cargo fizeram um pedido formal por notificação em tempo real ([09:00] Marcos).
- **PRD-PROB-02:** hoje esses clientes consultam `GET /orders` de tempos em tempos, o que deixa a integração lenta e cara para eles ([09:00] Marcos).
- **PRD-PROB-03:** a Atlas sugeriu que pode migrar para um concorrente se não houver entrega no prazo ([09:00] Marcos).

A reunião não trouxe números sobre o custo atual dessas consultas nem priorizou os problemas.

---

## Público-alvo e cenários de uso

**Público-alvo**

- **PRD-PUB-01:** clientes B2B que integram seus sistemas à plataforma, começando pelos três que fizeram o pedido ([09:00] Marcos).
- **PRD-PUB-02:** usuários do sistema que representam o cliente e configuram webhooks pela API, autenticados com JWT ([09:32] Marcos).
- **PRD-PUB-03:** administradores que reprocessam entregas que falharam em definitivo ([09:36] Sofia).

**Cenários de uso**

- **PRD-CEN-01:** o cliente quer saber só quando o pedido vira SHIPPED ou DELIVERED ([09:33] Marcos).
- **PRD-CEN-02:** o cliente fica indisponível por duas horas em manutenção planejada e recebe o evento depois ([09:16] Diego).
- **PRD-CEN-03:** o cliente recebe o mesmo evento duas vezes e o identifica como repetido ([09:25] Diego).
- **PRD-CEN-04:** o cliente vaza uma secret e pede uma nova, com tempo para migrar seus sistemas ([09:21] Sofia, [09:22] Diego).
- **PRD-CEN-05:** o cliente consulta as últimas entregas para entender sucessos e falhas ([09:34] Marcos).
- **PRD-CEN-06:** o cliente ficou fora por mais tempo do que a janela de tentativas, e um administrador reprocessa o evento ([09:18] Diego).

---

## Objetivos e métricas de sucesso

| ID | Objetivo | Métrica | Meta | Origem |
| --- | --- | --- | --- | --- |
| **PRD-OBJ-01** | Notificar no tempo que os clientes consideram real | Tempo entre a mudança de status e o início da primeira tentativa de entrega, em operação normal | Abaixo de 10 segundos | [09:02] Marcos |
| **PRD-OBJ-02** | Não perder notificações | Mudanças de status confirmadas sem evento para um webhook interessado | 0 | [09:40] Bruno |
| **PRD-OBJ-03** | Absorver indisponibilidades do cliente | Janela automática de novas tentativas | 14h36, "quase 15 horas" | [09:17] Diego |
| **PRD-OBJ-05** | Entregar dentro da estimativa do time | Sprints até a entrega, com a revisão de segurança incluída | 3 sprints, como estimativa | [09:47] Larissa |

Em PRD-OBJ-01, o limite de 10 segundos vem da reunião; medir até o início da primeira tentativa é `[Derivado]`. A reunião não definiu meta para a redução das consultas a `GET /orders`.

**Situação do prazo**

| ID | Item | O que a reunião registrou | Origem |
| --- | --- | --- | --- |
| **PRD-PRAZO-01** | Estimativa do time | Três sprints, com a revisão de segurança incluída | [09:47] Larissa |
| **PRD-PRAZO-02** | Pedido da Atlas | Primeiro, entrega até o fim do trimestre para não considerar a migração; depois, "fim de novembro" | [09:00] Marcos, [09:45] Marcos |
| **PRD-PRAZO-03** | Prazo confirmado | Nenhum; Marcos ficou de confirmar com a Atlas | [09:47] Marcos |

A transcrição não informa a duração da sprint nem a data da reunião. Por isso, não é possível afirmar se a estimativa cabe no pedido da Atlas.

---

## Escopo

**Incluso**

- **PRD-ESC-01:** cadastro, edição, remoção e listagem de webhooks pela API ([09:31] Marcos, [09:33] Bruno).
- **PRD-ESC-02:** filtro por status em cada webhook ([09:33] Marcos).
- **PRD-ESC-03:** notificação assinada, com novas tentativas automáticas ([09:48] Larissa).
- **PRD-ESC-04:** rotação de secret ([09:21] Sofia).
- **PRD-ESC-05:** histórico de entregas ([09:34] Marcos).
- **PRD-ESC-06:** reprocessamento manual por administrador ([09:35] Larissa).

**Fora de escopo**

- **PRD-FORA-01:** e-mail ao cliente quando o webhook estiver com problema. Descartado nesta fase; talvez entre em uma próxima, depois de medir o impacto ([09:37] Larissa).
- **PRD-FORA-02:** dashboard visual para o cliente. Painel é projeto separado do time de frontend ([09:40] Larissa).
- **PRD-FORA-03:** rate limiting de envio. Fica para "observar e decidir depois" ([09:39] Larissa).
- **PRD-FORA-04:** webhooks enviados pelos clientes para a plataforma ([09:02] Marcos).
- **PRD-FORA-05:** arquivamento de eventos entregues ([09:08] Diego).
- **PRD-FORA-06:** ordenação global e execução com vários workers ([09:13] Larissa, [09:13] Diego).
- **PRD-FORA-07:** garantia de entrega exatamente uma vez ([09:25] Diego).

---

## Requisitos funcionais

A reunião não classificou prioridades entre os requisitos. Todos estão no escopo desta fase e, por isso, constam como prioridade alta.

### PRD-FR-01 Cadastrar webhook

O cliente cadastra um endpoint para receber notificações ([09:31] Marcos).

**Fluxo principal**

- O usuário autenticado informa o customer, a URL e os status de interesse ([09:31] Marcos, [09:32] Larissa).
- A plataforma gera a secret e a devolve na criação ([09:31] Marcos).

**Fluxos alternativos e exceções**

- O customer não é obtido do JWT; ele é informado na requisição ([09:32] Larissa).

**Erros previstos**

- URL que não seja `https` é recusada com erro de validação ([09:23] Sofia).

**Prioridade:** alta

### PRD-FR-02 Editar webhook

O cliente altera um cadastro existente ([09:33] Bruno).

**Fluxo principal**

- O usuário autenticado envia os dados a alterar.
- As novas regras valem para as próximas mudanças de status.

**Fluxos alternativos e exceções**

- Uma alteração de filtro não muda eventos já registrados, porque o filtro é aplicado no registro ([09:34] Bruno).

**Erros previstos**

- Webhook inexistente ([09:28] Bruno).
- URL que não seja `https` ([09:23] Sofia).

**Prioridade:** alta

### PRD-FR-03 Remover webhook

O cliente remove um cadastro ([09:33] Bruno).

**Fluxo principal**

- O usuário autenticado solicita a remoção.
- O endpoint deixa de receber notificações.

**Fluxos alternativos e exceções**

- Nenhum definido na reunião.

**Erros previstos**

- Webhook inexistente ([09:28] Bruno).

**Prioridade:** alta

### PRD-FR-04 Listar webhooks de um customer

O cliente consulta os endpoints cadastrados para um customer ([09:33] Bruno).

**Fluxo principal**

- O usuário autenticado informa o customer.
- A plataforma devolve os cadastros.

**Fluxos alternativos e exceções**

- Nenhum definido na reunião.

**Erros previstos**

- Falta de autenticação ([09:32] Larissa).

**Prioridade:** alta

### PRD-FR-05 Filtrar notificações por status

Cada endpoint define a lista de status que quer ouvir ([09:33] Marcos).

**Fluxo principal**

- O cliente informa, por exemplo, SHIPPED e DELIVERED.
- A plataforma só registra evento para webhooks interessados naquele status ([09:33] Marcos).

**Fluxos alternativos e exceções**

- Se nenhum webhook do customer quiser o status, nenhum evento é registrado ([09:34] Bruno).

**Erros previstos**

- Nenhum definido na reunião.

**Prioridade:** alta

### PRD-FR-06 Notificar mudança de status

Cada mudança de status de interesse gera uma notificação HTTP ao endpoint do cliente ([09:00] Marcos).

**Fluxo principal**

- O status do pedido muda, e o evento é registrado junto com a mudança ([09:40] Bruno).
- A plataforma envia o evento, com identificação do pedido, status de origem e destino, customer e total ([09:43] Diego).
- Para mais detalhes, o cliente consulta o pedido pela API ([09:43] Diego).

**Fluxos alternativos e exceções**

- O mesmo evento pode chegar mais de uma vez, e o cliente deduplica pelo identificador do evento ([09:25] Diego).
- A ordem só é garantida por pedido, enquanto houver um só worker ([09:13] Larissa).

**Erros previstos**

- Se o registro do evento falhar, a mudança de status é revertida ([09:40] Bruno).
- Evento acima de 64KB não é enviado e gera erro ([09:24] Larissa).

**Prioridade:** alta

### PRD-FR-07 Assinar notificações

O cliente consegue validar que a notificação veio da plataforma e não foi adulterada ([09:19] Sofia).

**Fluxo principal**

- A plataforma assina o corpo com HMAC-SHA256 e a secret do endpoint ([09:22] Sofia).
- A assinatura vai em um header, e o cliente a verifica do lado dele ([09:20] Sofia).

**Fluxos alternativos e exceções**

- Cada endpoint tem secret própria ([09:21] Sofia).

**Erros previstos**

- Nenhum definido na reunião.

**Prioridade:** alta

### PRD-FR-08 Rotacionar secret

O cliente pede uma nova secret pela API ([09:21] Sofia).

**Fluxo principal**

- O cliente solicita a rotação e recebe a nova secret.
- A secret antiga continua válida por 24 horas, em paralelo, para ele migrar seus sistemas ([09:21] Sofia).

**Fluxos alternativos e exceções**

- Passadas as 24 horas, a secret antiga deixa de valer ([09:21] Sofia).

**Erros previstos**

- Webhook inexistente ([09:28] Bruno).

**Prioridade:** alta

### PRD-FR-09 Tentar novamente em caso de falha

A plataforma tenta de novo quando o cliente não recebe a notificação ([09:14] Larissa).

**Fluxo principal**

- Falha ou falta de resposta em 10 segundos conta como falha ([09:42] Diego).
- A plataforma faz 5 tentativas, nos intervalos de 1 minuto, 5 minutos, 30 minutos, 2 horas e 12 horas ([09:17] Larissa). A leitura técnica dessa contagem está no [ADR-004](adrs/ADR-004-retry-com-backoff-e-dlq.md).

**Fluxos alternativos e exceções**

- Se a entrega tiver sucesso em uma das tentativas, o evento é encerrado como entregue ([09:08] Diego).

**Erros previstos**

- Esgotadas as tentativas, a falha é considerada permanente ([09:15] Diego).

**Prioridade:** alta

### PRD-FR-10 Guardar falhas permanentes

Eventos que esgotaram as tentativas ficam guardados para debug e reprocessamento ([09:18] Diego).

**Fluxo principal**

- A plataforma guarda o conteúdo do evento, o motivo da falha e o momento ([09:18] Diego).

**Fluxos alternativos e exceções**

- Nenhum definido na reunião.

**Erros previstos**

- Nenhum definido na reunião.

**Prioridade:** alta

### PRD-FR-11 Reprocessar manualmente

Um administrador recoloca um evento com falha permanente na fila de entrega ([09:18] Diego).

**Fluxo principal**

- O administrador aciona o endpoint de replay do evento ([09:35] Diego).
- O evento volta a ficar pendente de envio ([09:18] Diego).
- A plataforma registra quem fez o replay, para auditoria ([09:36] Sofia).

**Fluxos alternativos e exceções**

- Nenhum definido na reunião.

**Erros previstos**

- Usuário sem role ADMIN é recusado ([09:36] Larissa).

**Prioridade:** alta

### PRD-FR-12 Consultar histórico de entregas

O cliente vê as últimas entregas de um webhook ([09:34] Marcos).

**Fluxo principal**

- O cliente consulta o histórico do webhook.
- A plataforma devolve sucesso ou falha, payload, resposta e tempo de resposta ([09:34] Marcos).

**Fluxos alternativos e exceções**

- "Últimos 100" foi citado como exemplo; a reunião não fechou esse limite ([09:34] Marcos).

**Erros previstos**

- Webhook inexistente ([09:28] Bruno).

**Prioridade:** alta

---

## Requisitos não funcionais

**Performance**

- **PRD-NFR-01:** notificação em menos de 10 segundos ([09:02] Marcos).
- **PRD-NFR-02:** verificação de novos eventos a cada 2 segundos ([09:10] Larissa).
- **PRD-NFR-03:** tempo máximo de espera de 10 segundos pela resposta do cliente ([09:42] Diego).
- **PRD-NFR-04:** limite de 64KB por evento ([09:24] Larissa).

**Disponibilidade**

- **PRD-NFR-05:** o envio de notificações não para quando a API reinicia ([09:11] Diego).
- **PRD-NFR-06:** a reunião tratou da disponibilidade do envio apenas no caso de reinício da API ([09:11] Diego) e não definiu meta em percentual. A meta segue ausente.

**Segurança e autorização**

- **PRD-NFR-07:** todo endpoint de webhook precisa usar `https` ([09:23] Sofia).
- **PRD-NFR-08:** assinatura HMAC-SHA256, com secret por endpoint e rotação com 24 horas ([09:22] Sofia).
- **PRD-NFR-09:** replay exige role ADMIN; o restante da configuração aceita qualquer role autenticada, por enquanto ([09:36] Larissa, [09:37] Sofia).
- **PRD-NFR-10:** revisão de segurança antes do deploy ([09:46] Sofia).

**Observabilidade**

- **PRD-NFR-11:** logs com o logger já usado no projeto, sem ferramenta nova ([09:29] Bruno).
- **PRD-NFR-12:** registro de quem executou cada replay ([09:36] Sofia).
- **PRD-NFR-13:** a reunião definiu reuso máximo do que já existe ([09:30] Larissa) e não definiu metas de métricas nem de tracing. O [FDD](FDD.md) trata do tema com os dados já definidos, porque o enunciado o exige.

**Confiabilidade e integridade de dados**

- **PRD-NFR-14:** o evento e a mudança de status são confirmados ou revertidos juntos ([09:06] Diego).
- **PRD-NFR-15:** entrega pelo menos uma vez, com identificador único por evento ([09:26] Larissa).
- **PRD-NFR-16:** o evento reflete o estado do pedido no momento da mudança ([09:52] Larissa).

**Compatibilidade e portabilidade**

- **PRD-NFR-17:** o módulo segue os padrões atuais do projeto ([09:30] Larissa).
- **PRD-NFR-18:** sem nova infraestrutura: mesmo banco MySQL ([09:07] Diego).

**Compliance e acessibilidade**

- **PRD-NFR-19:** a reunião não tratou de requisitos regulatórios. Não há interface visual nesta fase, então acessibilidade não se aplica ([09:40] Larissa).

---

## Decisões e trade-offs principais

### Decisão: registrar eventos na própria transação do pedido, com outbox no MySQL

- **PRD-DEC-01:** [ADR-001](adrs/ADR-001-outbox-no-mysql.md)
- **Justificativa:** o evento não pode se perder nem existir sem a mudança ([09:06] Diego).
- **Trade-off:** mais escrita no banco atual, em vez de subir nova infraestrutura ([09:07] Diego).

### Decisão: enviar por um worker separado, verificando novos eventos a cada 2 segundos

- **PRD-DEC-02:** [ADR-002](adrs/ADR-002-worker-em-processo-separado-com-polling.md) e [ADR-003](adrs/ADR-003-worker-unico-e-ordenacao-por-pedido.md)
- **Justificativa:** atende ao limite de 10 segundos e não depende de reinícios da API ([09:09] Diego, [09:11] Diego).
- **Trade-off:** há uma espera aceita até a próxima verificação, e a ordem só vale por pedido ([09:10] Larissa, [09:13] Larissa).

### Decisão: fazer 5 tentativas e depois exigir reprocessamento manual

- **PRD-DEC-03:** [ADR-004](adrs/ADR-004-retry-com-backoff-e-dlq.md)
- **Justificativa:** cobre indisponibilidades reais sem deixar eventos pendurados ([09:15] Diego, [09:16] Diego).
- **Trade-off:** cliente fora por mais de quase 15 horas depende de ação manual ([09:17] Marcos, [09:18] Diego).

### Decisão: assinar com HMAC-SHA256 e secret por endpoint

- **PRD-DEC-04:** [ADR-005](adrs/ADR-005-hmac-sha256-com-secret-por-endpoint.md)
- **Justificativa:** o cliente valida a origem, e um vazamento não atinge os demais ([09:19] Sofia, [09:21] Sofia).
- **Trade-off:** o cliente precisa implementar a verificação ([09:20] Sofia).

### Decisão: garantir entrega pelo menos uma vez

- **PRD-DEC-05:** [ADR-006](adrs/ADR-006-at-least-once-com-x-event-id.md)
- **Justificativa:** exactly-once exigiria coordenação dos dois lados e muito mais complexidade ([09:25] Diego).
- **Trade-off:** a deduplicação fica com o cliente ([09:25] Sofia).

### Decisão: reaproveitar os padrões do projeto

- **PRD-DEC-06:** [ADR-007](adrs/ADR-007-reuso-dos-padroes-do-projeto.md)
- **Justificativa:** mantém o módulo igual aos demais ([09:30] Larissa).
- **Trade-off:** análise do código: a feature herda os limites dos componentes atuais, como detalhado no ADR.

### Decisão: filtrar por status e congelar o conteúdo no registro do evento

- **PRD-DEC-07:** [ADR-008](adrs/ADR-008-snapshot-e-filtro-na-insercao.md)
- **Justificativa:** evita registros sem interessado e preserva o estado do momento da mudança ([09:34] Bruno, [09:52] Larissa).
- **Trade-off:** como o filtro é aplicado na inserção ([09:34] Bruno), mudanças de filtro só valem para as próximas mudanças de status.

---

## Dependências

### Organizacional: revisão de segurança

**PRD-DEP-01:** Sofia precisa de pelo menos dois dias úteis para revisar HMAC e geração de secret antes do deploy, e pediu para ser agendada ([09:46] Sofia, [09:49] Sofia).

### Organizacional: portal de desenvolvedor

**PRD-DEP-02:** Marcos documenta a integração e o comportamento de duplicatas no portal ([09:26] Marcos, [09:40] Marcos).

### Externa: confirmação do prazo

**PRD-DEP-03:** Marcos confirma o prazo com a Atlas e atualiza os clientes ([09:47] Marcos, [09:49] Marcos).

### Externa: implementação no cliente

**PRD-DEP-04:** cada cliente precisa expor um endpoint `https`, verificar a assinatura e deduplicar eventos ([09:23] Sofia, [09:20] Sofia, [09:24] Diego).

### Técnica: revisão do design

**PRD-DEP-05:** Larissa marca uma sessão de revisão do design com Bruno e Diego antes do início da codificação ([09:50] Larissa).

---

## Riscos e mitigação

Probabilidades são avaliações do autor, para validação na revisão, porque a reunião não as quantificou.

### PRD-RISK-01: perda de um cliente por atraso na entrega

- **Probabilidade:** média
- **Impacto:** a Atlas sugeriu que pode migrar para um concorrente se não houver entrega até o fim do trimestre ([09:00] Marcos); depois, o pedido citado foi fim de novembro ([09:45] Marcos).
- **Mitigação:**
  - Estimativa de três sprints, com a revisão de segurança incluída ([09:47] Larissa).
  - Confirmação do prazo com a Atlas, ainda pendente ([09:47] Marcos).
- **Plano de contingência:** não definido na reunião.

### PRD-RISK-02: cliente trata mal eventos duplicados

- **Probabilidade:** média
- **Impacto:** o cliente pode processar o mesmo evento duas vezes, porque a responsabilidade de deduplicar é dele ([09:25] Sofia).
- **Mitigação:**
  - Documentação em destaque no portal ([09:26] Marcos).
- **Plano de contingência:** não definido na reunião.

### PRD-RISK-03: volume alto de notificações para um cliente

- **Probabilidade:** média
- **Impacto:** um cliente com 50 pedidos mudando de status em um minuto recebe 50 chamadas ([09:38] Diego).
- **Mitigação:**
  - Observar o comportamento e decidir depois ([09:39] Larissa).
- **Plano de contingência:** implementar rate limiting se virar problema ([09:39] Diego).

### PRD-RISK-04: cliente fora do ar além da janela de tentativas

- **Probabilidade:** baixa
- **Impacto:** o cliente deixa de receber automaticamente depois de quase 15 horas, situação considerada aceitável ([09:17] Marcos).
- **Mitigação:**
  - Evento guardado para debug e reprocessamento ([09:18] Diego).
- **Plano de contingência:** replay manual por administrador ([09:18] Diego).

### PRD-RISK-05: controle de acesso menos restrito do que o previsto

- **Probabilidade:** média
- **Impacto:** pessoas sem a autorização esperada podem administrar webhooks de outros clientes ou reprocessar entregas. A análise técnica está em RFC-RISK-03 e RFC-RISK-04.
- **Mitigação:**
  - Levar o ponto à revisão de segurança ([09:46] Sofia).
- **Plano de contingência:** endurecer as permissões, possibilidade citada na reunião ([09:37] Sofia).

---

## Critérios de aceitação

- [ ] **PRD-AC-01:** um usuário autenticado consegue cadastrar, editar, remover e listar webhooks ([09:33] Bruno).
- [ ] **PRD-AC-02:** a criação devolve uma secret gerada pela plataforma ([09:31] Marcos).
- [ ] **PRD-AC-03:** URL com `http` é recusada ([09:23] Sofia).
- [ ] **PRD-AC-04:** o cliente só recebe notificação dos status escolhidos ([09:33] Marcos).
- [ ] **PRD-AC-05:** em operação normal, a primeira tentativa de entrega começa em menos de 10 segundos após a mudança de status ([09:02] Marcos).
- [ ] **PRD-AC-06:** se o registro do evento falhar, o status do pedido não muda ([09:40] Bruno).
- [ ] **PRD-AC-07:** a notificação traz os campos e headers definidos, sem items ([09:43] Diego, [09:44] Diego).
- [ ] **PRD-AC-08:** o cliente consegue validar a assinatura com a secret do endpoint ([09:20] Sofia).
- [ ] **PRD-AC-09:** depois de rotacionar, as duas secrets valem por 24 horas, e depois só a nova ([09:21] Sofia).
- [ ] **PRD-AC-10:** uma entrega que falha é retentada nos intervalos definidos ([09:17] Larissa).
- [ ] **PRD-AC-11:** esgotadas as tentativas, o evento fica guardado com conteúdo, motivo e momento da falha ([09:18] Diego).
- [ ] **PRD-AC-12:** só ADMIN consegue fazer replay, e o log registra quem o fez ([09:36] Sofia).
- [ ] **PRD-AC-13:** o histórico mostra sucesso ou falha, payload, resposta e tempo de resposta ([09:34] Marcos).
- [ ] **PRD-AC-14:** a revisão de segurança foi concluída antes do deploy ([09:49] Sofia).

---

## Estratégia de testes e validação

**Tipos de teste obrigatórios**

- **PRD-TEST-01:** testes ponta a ponta da integração com a mudança de status, previstos na estimativa ([09:46] Larissa).
- **PRD-TEST-02:** `[Derivado]` testes de integração da API com a stack atual do projeto, como em `tests/orders.test.ts`. A reunião citou apenas testes ponta a ponta.
- **PRD-TEST-03:** revisão de segurança do código de HMAC e de geração de secret ([09:46] Sofia).

**Estratégia de validação**

- **PRD-TEST-04:** a checklist técnica detalhada está nos critérios de aceite do [FDD](FDD.md), e o design passa por revisão antes da codificação ([09:50] Larissa).
- **PRD-TEST-05:** depois da entrega, a medição de impacto orienta a decisão sobre e-mail, e a observação do comportamento orienta a decisão sobre rate limiting ([09:37] Larissa, [09:39] Diego).

---

## Changelog

Detalhes e IDs afetados na seção Changelog de [TRACKER.md](TRACKER.md).

| CHG | Resumo |
| --- | --- |
| CHG-000 | Versão 1.0, linha de base do pacote |
| CHG-002 | Removida a meta "3 de 3", que não tinha origem na reunião |
| CHG-003 | Prazo separado em estimativa, pedido do cliente e confirmação |
| CHG-004 | Requisito de retry passa a remeter a leitura de "5 tentativas" ao ADR-004 |
| CHG-005 | Metas de disponibilidade e de métricas seguem ausentes, com citações corrigidas |
| CHG-006 | Risco de controle de acesso reescrito em termos de produto |
| CHG-007 | Auditoria de citações: origens ajustadas, sem prioridade atribuída aos problemas, e teste de integração marcado como proposta |
| CHG-008 | "Últimos 100" registrado como exemplo, sem limite fechado |
| CHG-031 | Termo "proposta" substituído pelo marcador usado no pacote |
| CHG-039 | Forma de medir o limite de 10 segundos identificada como derivada |
| CHG-042 | Link para o tracker com rótulo relativo a este arquivo e sem âncora |
