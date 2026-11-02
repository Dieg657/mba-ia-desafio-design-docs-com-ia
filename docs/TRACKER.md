# Tracker de Rastreabilidade

Liga cada item do pacote à origem na transcrição ou no código e mostra o estado do trabalho.

## Estado do trabalho

| Etapa | Estado | Observação |
| --- | --- | --- |
| Linha de base | Concluída | PRD, RFC, FDD, 8 ADRs e tracker |
| Iteração 1: PRD e changelog | Concluída | CHG-001 a CHG-008 |
| Iteração 2: FDD | Concluída | CHG-009 a CHG-017 |
| Iteração 3: RFC | Concluída | CHG-018 a CHG-021 |
| Iteração 4: ADRs | Concluída | CHG-022 a CHG-030 |
| Iteração 5: fechamento | Concluída | CHG-031 a CHG-033 |
| Ajustes do tracker | Concluídos | CHG-034, CHG-035 e CHG-041 |
| Revisão extensa de citações | Concluída | CHG-036 a CHG-040 |
| Ajuste dos links para o tracker | Concluído | CHG-042 a CHG-052 |
| Unificação do histórico da branch | Concluída | CHG-053 |

## Como ler a matriz

- **Fonte:** `TRANSCRICAO` quando a origem é uma fala da reunião; `CODIGO` quando é um arquivo do repositório.
- **Localização:** `[hh:mm] Nome` para a transcrição; caminho do arquivo para o código.
- **Tipo `Derivado`:** a reunião não detalhou o item, mas ele decorre do código ou de uma decisão fechada, ou é exigido pelo enunciado. Não propõe comportamento novo.
- **Tipo `Decisão do autor`:** escolha do autor para um ponto não fechado na reunião, pendente de confirmação dos revisores.
- **Tipo `Não definido`:** ponto sem resposta nas fontes, registrado sem sugestão. A localização aponta onde a necessidade surgiu.
- **Tipo `Questão em aberto`:** ponto citado e não decidido, ou necessário e não discutido.

## Pendências

| ID | Documento | Quando | Motivo |
| --- | --- | --- | --- |
| PEND-10 | `docs/FDD.md` | Sessão de revisão do design | Larissa, Bruno e Diego avaliam FDD-AUT-01 e os pontos da seção 14 do FDD |
| PEND-11 | `docs/FDD.md` | Revisão de segurança | Sofia avalia FDD-AUT-02, FDD-ND-02, FDD-ND-18, FDD-ND-22, RFC-RISK-03 e RFC-RISK-04 |
| PEND-12 | `docs/PRD.md` | Confirmação com a Atlas | Marcos confirma o prazo, conforme PRD-PRAZO-03 |

## Changelog

Toda alteração nos documentos tem uma linha aqui e é citada na seção Changelog do próprio documento.

| CHG | Iteração | Documento | IDs afetados | Alteração e motivo |
| --- | --- | --- | --- | --- |
| CHG-000 | Base | Todos | Todos | Versão 1.0 do pacote, linha de base |
| CHG-001 | 1 | `docs/TRACKER.md` | Estrutura | Criadas as seções Changelog e Pendências, e o estado passa a acompanhar branch e iterações, para separar o que foi feito do que falta |
| CHG-002 | 1 | `docs/PRD.md` | Removido: PRD-OBJ-04 | Meta "3 de 3" removida, pois não foi declarada na reunião |
| CHG-003 | 1 | `docs/PRD.md` | PRD-OBJ-05, PRD-RISK-01; novos: PRD-PRAZO-01 a PRD-PRAZO-03 | Separa estimativa, pedido da Atlas e prazo confirmado, que ainda não existe |
| CHG-004 | 1 | `docs/PRD.md` | PRD-FR-09, PRD-DEC-03 | Mantém "5 tentativas" como foi dito e remete a leitura ao ADR-004; trade-off passa a citar também o replay manual |
| CHG-005 | 1 | `docs/PRD.md` | PRD-NFR-06, PRD-NFR-13 | As falas citadas não sustentavam a ausência de meta; o texto agora separa o que foi dito do que falta, sem adotar números |
| CHG-006 | 1 | `docs/PRD.md` | PRD-RISK-05 | Risco reescrito em termos de produto; a análise técnica fica no RFC |
| CHG-007 | 1 | `docs/PRD.md` | PRD-CTX-02, PRD-PROB-01 a PRD-PROB-03, PRD-DEC-06, PRD-DEC-07, PRD-TEST-02 | Auditoria de citações: origem adicionada, prioridade sem origem removida, análise do código rotulada e teste de integração marcado como proposta |
| CHG-008 | 1 | `docs/PRD.md` | PRD-FR-12 | "Últimos 100" registrado como exemplo, sem limite fechado |
| CHG-009 | 2 | `docs/FDD.md` | Novos: FDD-AUT-01, FDD-AUT-02, FDD-ND-01 a FDD-ND-21 | Regra definida pelo enunciado: fica o acordado e o derivado do código ou exigido pelo enunciado; o resto vira não definido, sem sugestão. Registra as duas decisões do autor |
| CHG-010 | 2 | `docs/FDD.md` | FDD-DADOS-01, FDD-DADOS-02, FDD-DADOS-06 a FDD-DADOS-09; Removido: FDD-DADOS-10; Removido: FDD-DADOS-11; Removido: FDD-DADOS-12; Removido: FDD-DADOS-13; Removido: FDD-DADOS-14 | Bloco Prisma substituído por tabelas de colunas; saem linha por endpoint, cascatas e tipo do payload, que não foram acordados; outbox segue a fala "uma linha" com o evento |
| CHG-011 | 2 | `docs/FDD.md` | FDD-FLUXO-04, FDD-FLUXO-05, FDD-FLUXO-09, FDD-FLUXO-12, FDD-FLUXO-13, FDD-FLUXO-15; Removido: FDD-FLUXO-06; Removido: FDD-FLUXO-07; Removido: FDD-FLUXO-08; Removido: FDD-FLUXO-10; Removido: FDD-FLUXO-11; Removido: FDD-FLUXO-16; Removido: FDD-FLUXO-17 | Saem SQL, lote de 10, regra de 2xx, recuperação ao iniciar e demais comportamentos sem origem; retry passa a citar WEBHOOK_MAX_RETRIES |
| CHG-012 | 2 | `docs/FDD.md` | FDD-CONTRATO-02 a FDD-CONTRATO-09; Removido: FDD-CONTRATO-10; Removido: FDD-CONTRATO-11 | Sem formato de assinatura, vetor de teste, rota de rotação nem 409 não acordados; status 200 no replay segue o padrão de order.controller.ts; citação da secret corrigida |
| CHG-013 | 2 | `docs/FDD.md` | FDD-ERRO-02, FDD-ERRO-05, FDD-ERRO-08, FDD-ERRO-09, FDD-ERRO-11, FDD-ERRO-13, FDD-ERRO-15; Removido: FDD-ERRO-06; Removido: FDD-ERRO-07; Removido: FDD-ERRO-10 | Matriz limitada a condições rastreáveis; nomes derivados rotulados; com validate atual, URL http responde VALIDATION_ERROR |
| CHG-014 | 2 | `docs/FDD.md` | FDD-OBS-01, FDD-OBS-03 a FDD-OBS-06; Removido: FDD-OBS-07 | Métricas e tracing rotulados como exigência do enunciado; sai o alerta sugerido; secret em log vira ponto não definido, pois a fala citada tratava de log do cliente |
| CHG-015 | 2 | `docs/FDD.md` | FDD-INT-08, FDD-INT-09, FDD-INT-14, FDD-INT-16, FDD-INT-18, FDD-DEP-03, FDD-DEP-09, FDD-DEP-10; novo: FDD-INT-19 | Documenta montagem da rota, testada em Express 4.21.1, e configuração do worker; saem nome de arquivo, worker:dev e efeitos de cascata não acordados |
| CHG-016 | 2 | `docs/FDD.md` | FDD-OBJ-02, FDD-OBJ-03, FDD-RES-02, FDD-RES-08 a FDD-RES-10, FDD-AC-01, FDD-AC-10, FDD-AC-12, FDD-AC-14, FDD-AC-15, FDD-AC-18, FDD-RISK-01 a FDD-RISK-06 | Auditoria de citações: textos aproximados das falas e critérios e riscos sem referência a itens removidos |
| CHG-017 | 2 | `docs/TRACKER.md` | Matriz, pendências e legenda | Matriz atualizada aos novos IDs, tipos Derivado, Decisão do autor e Não definido, e pendências para RFC, ADRs e PRD |
| CHG-018 | 3 | `docs/RFC.md` | RFC-QA-06 a RFC-QA-15; novo: RFC-QA-16 | Resolve PEND-03: questões separadas em adiadas, aguardando revisores e fechadas, sem propor resposta; RFC-QA-07 fecha como interpretação, RFC-QA-12 por evidência e RFC-QA-13 como derivado |
| CHG-019 | 3 | `docs/RFC.md` | Removido: RFC-QA-05 | Arquivamento é fora do escopo da feature, segundo [09:08] Diego, e não uma questão; segue em PRD-FORA-05 e FDD-EXC-05 |
| CHG-020 | 3 | `docs/RFC.md` | RFC-RISK-02 a RFC-RISK-05; Removido: RFC-RISK-01 | Resolve PEND-01: risco de prazo fica em PRD-RISK-01; mitigação sugerida para cliente lento sai, pois não foi discutida |
| CHG-021 | 3 | `docs/TRACKER.md` | Matriz e pendências | Matriz alinhada ao RFC; PEND-01 e PEND-03 resolvidas e PEND-09 criada para o ADR-004 |
| CHG-022 | 4 | `docs/adrs/ADR-003-worker-unico-e-ordenacao-por-pedido.md` | ADR-003, ADR-003-TO | Resolve PEND-04: nota aponta para FDD-AUT-01 e RFC-QA-08; trade-off não atribui mais a limitação de vazão às falas citadas |
| CHG-023 | 4 | `docs/adrs/ADR-005-hmac-sha256-com-secret-por-endpoint.md` | ADR-005-CON-03, ADR-005-COD-01 | Resolve PEND-05: notas apontam para FDD-ND-02, FDD-AUT-02 e FDD-ND-18; fala sobre vazamento passa a ser a de Diego |
| CHG-024 | 4 | `docs/adrs/ADR-007-reuso-dos-padroes-do-projeto.md` | ADR-007-ALT-01, ADR-007-CON-03 a ADR-007-CON-05, ADR-007-TO | Resolve PEND-06: a fala de Bruno tratava do logger e não apresentava a alternativa, agora plausível e não discutida; limites do reuso rotulados como análise |
| CHG-025 | 4 | `docs/adrs/ADR-008-snapshot-e-filtro-na-insercao.md` | ADR-008-CON-02, ADR-008-CON-03, ADR-008-CON-05 | Resolve PEND-07: nota sem proposta para vários webhooks, apontando para FDD-ND-01; consequências não ditas rotuladas como análise |
| CHG-026 | 4 | `docs/adrs/ADR-001-outbox-no-mysql.md` | ADR-001-CON-05, ADR-001-TO | Crescimento da tabela rotulado como análise; trade-off inclui a dúvida de Bruno sobre acúmulo |
| CHG-027 | 4 | `docs/adrs/ADR-006-at-least-once-com-x-event-id.md` | ADR-006 | Auditoria de citações e alternativas sem correções; adicionada a seção Changelog |
| CHG-028 | 4 | `docs/adrs/ADR-002-worker-em-processo-separado-com-polling.md` | ADR-002-CON-06 | Consequência sobre a configuração do worker rotulada como análise do código |
| CHG-029 | 4 | `docs/adrs/ADR-004-retry-com-backoff-e-dlq.md` | ADR-004-INT-01 | Resolve PEND-09: leitura alinhada a RFC-QA-07, fechada como interpretação |
| CHG-030 | 4 | `docs/TRACKER.md` | Matriz e pendências | Matriz alinhada aos ADRs; PEND-04 a PEND-07 e PEND-09 resolvidas |
| CHG-031 | 5 | `docs/PRD.md` | PRD-NFR-13, PRD-TEST-02 | Resolve PEND-08: termo "proposta" substituído pelos marcadores do pacote |
| CHG-032 | 5 | `docs/FDD.md` | FDD-RISK-04 | Remete a análise de acesso a RFC-RISK-03 e RFC-RISK-04, eliminando duplicação entre alturas |
| CHG-033 | 5 | `docs/TRACKER.md` | Estado, pendências, legenda e matriz | Pendências de documentos zeradas; restam só as que dependem de pessoas; legenda sem o tipo Proposta |
| CHG-034 | Ajuste | `docs/TRACKER.md` | Estado e changelog | O tracker passa a cobrir só PRD, RFC, FDD e ADRs, a pedido do autor; as entradas de fora desse escopo saem |
| CHG-035 | Ajuste | `docs/TRACKER.md` | Changelog | Numeração refeita para ficar sequencial, sem lacunas; só o tracker citava os números alterados |
| CHG-036 | Revisão | `docs/RFC.md` | RFC-QA-07, RFC-QA-12, RFC-RISK-04 | RFC-QA-12 reaberta: a fala de Sofia tratava da role, não do limite de customer por usuário; RFC-QA-07 registra o resumo "total 5 tentativas", que pesa contra a interpretação |
| CHG-037 | Revisão | `docs/adrs/ADR-004-retry-com-backoff-e-dlq.md` | ADR-004-INT-01 | A interpretação passa a registrar o resumo "total 5 tentativas" de [09:48] Larissa |
| CHG-038 | Revisão | `docs/FDD.md` | Novo: FDD-ND-22 | Limite de customer por usuário registrado como ponto sem definição |
| CHG-039 | Revisão | `docs/PRD.md` | PRD-OBJ-01 | A forma de medir o limite de 10 segundos não foi dita na reunião e fica identificada como derivada |
| CHG-040 | Revisão | `docs/TRACKER.md` | 32 linhas da matriz | Linhas cujo resumo combina mais de uma fala passam a citar todas elas; FDD-AC-15 e FDD-RISK-06 têm o resumo alinhado ao documento |
| CHG-041 | Ajuste | `docs/TRACKER.md` | Estado do trabalho | Removidos branch e hashes de commit; ficam etapas, estado e CHGs |
| CHG-042 | Ajuste | `docs/PRD.md` | Link do changelog | O rótulo docs/TRACKER.md não vale como caminho a partir deste arquivo, e a âncora dependia do visualizador; o link agora aponta para o arquivo |
| CHG-043 | Ajuste | `docs/RFC.md` | Link do changelog | O rótulo docs/TRACKER.md não vale como caminho a partir deste arquivo, e a âncora dependia do visualizador; o link agora aponta para o arquivo |
| CHG-044 | Ajuste | `docs/FDD.md` | Link do changelog | O rótulo docs/TRACKER.md não vale como caminho a partir deste arquivo, e a âncora dependia do visualizador; o link agora aponta para o arquivo |
| CHG-045 | Ajuste | `docs/adrs/ADR-003-worker-unico-e-ordenacao-por-pedido.md` | Link do changelog | O rótulo docs/TRACKER.md não vale como caminho a partir deste arquivo, e a âncora dependia do visualizador; o link agora aponta para o arquivo |
| CHG-046 | Ajuste | `docs/adrs/ADR-005-hmac-sha256-com-secret-por-endpoint.md` | Link do changelog | O rótulo docs/TRACKER.md não vale como caminho a partir deste arquivo, e a âncora dependia do visualizador; o link agora aponta para o arquivo |
| CHG-047 | Ajuste | `docs/adrs/ADR-007-reuso-dos-padroes-do-projeto.md` | Link do changelog | O rótulo docs/TRACKER.md não vale como caminho a partir deste arquivo, e a âncora dependia do visualizador; o link agora aponta para o arquivo |
| CHG-048 | Ajuste | `docs/adrs/ADR-008-snapshot-e-filtro-na-insercao.md` | Link do changelog | O rótulo docs/TRACKER.md não vale como caminho a partir deste arquivo, e a âncora dependia do visualizador; o link agora aponta para o arquivo |
| CHG-049 | Ajuste | `docs/adrs/ADR-001-outbox-no-mysql.md` | Link do changelog | O rótulo docs/TRACKER.md não vale como caminho a partir deste arquivo, e a âncora dependia do visualizador; o link agora aponta para o arquivo |
| CHG-050 | Ajuste | `docs/adrs/ADR-006-at-least-once-com-x-event-id.md` | Link do changelog | O rótulo docs/TRACKER.md não vale como caminho a partir deste arquivo, e a âncora dependia do visualizador; o link agora aponta para o arquivo |
| CHG-051 | Ajuste | `docs/adrs/ADR-002-worker-em-processo-separado-com-polling.md` | Link do changelog | O rótulo docs/TRACKER.md não vale como caminho a partir deste arquivo, e a âncora dependia do visualizador; o link agora aponta para o arquivo |
| CHG-052 | Ajuste | `docs/adrs/ADR-004-retry-com-backoff-e-dlq.md` | Link do changelog | O rótulo docs/TRACKER.md não vale como caminho a partir deste arquivo, e a âncora dependia do visualizador; o link agora aponta para o arquivo |
| CHG-053 | Ajuste | `docs/TRACKER.md` | CHG-000 | Retirada a citação a um commit de trabalho, que deixou de existir ao unificar o histórico da branch |

## Matriz de rastreabilidade

| ID | Documento | Tipo | Conteúdo (resumo) | Fonte | Localização |
| --- | --- | --- | --- | --- | --- |
| ADR-001 | `docs/adrs/ADR-001-outbox-no-mysql.md` | Decisão | Outbox no MySQL, na mesma transação de changeStatus | TRANSCRICAO | [09:08] Larissa; [09:06] Diego |
| ADR-001-CTX-01 | `docs/adrs/ADR-001-outbox-no-mysql.md` | Contexto | HTTP na transação faria cliente lento travar outras mudanças de status | TRANSCRICAO | [09:04] Bruno |
| ADR-001-CTX-02 | `docs/adrs/ADR-001-outbox-no-mysql.md` | Restrição | Cliente fora do ar não pode causar rollback do status | TRANSCRICAO | [09:04] Bruno |
| ADR-001-CTX-03 | `docs/adrs/ADR-001-outbox-no-mysql.md` | Restrição | Commit registra o evento; rollback o descarta | TRANSCRICAO | [09:06] Diego |
| ADR-001-CTX-04 | `docs/adrs/ADR-001-outbox-no-mysql.md` | Restrição | Time pequeno; Redis Cluster seria overengineering | TRANSCRICAO | [09:07] Diego |
| ADR-001-ALT-01 | `docs/adrs/ADR-001-outbox-no-mysql.md` | Alternativa | Disparo HTTP síncrono descartado | TRANSCRICAO | [09:04] Bruno |
| ADR-001-ALT-02 | `docs/adrs/ADR-001-outbox-no-mysql.md` | Alternativa | Redis Streams ou Cluster descartado por exigir infraestrutura | TRANSCRICAO | [09:07] Larissa |
| ADR-001-ALT-03 | `docs/adrs/ADR-001-outbox-no-mysql.md` | Alternativa | Injeção do repository substituída por função que recebe tx | TRANSCRICAO | [09:41] Diego |
| ADR-001-CON-01 | `docs/adrs/ADR-001-outbox-no-mysql.md` | Consequência | Status e evento têm o mesmo resultado de commit ou rollback | TRANSCRICAO | [09:06] Diego |
| ADR-001-CON-02 | `docs/adrs/ADR-001-outbox-no-mysql.md` | Consequência | Entrega deixa de afetar a transação de status | TRANSCRICAO | [09:04] Bruno |
| ADR-001-CON-03 | `docs/adrs/ADR-001-outbox-no-mysql.md` | Consequência | Sem infraestrutura nova | TRANSCRICAO | [09:07] Diego |
| ADR-001-CON-04 | `docs/adrs/ADR-001-outbox-no-mysql.md` | Consequência | Acúmulo de eventos tratado com índices e batch pequeno | TRANSCRICAO | [09:08] Diego |
| ADR-001-CON-05 | `docs/adrs/ADR-001-outbox-no-mysql.md` | Consequência | Arquivamento de entregues fora da feature; crescimento da tabela por análise | TRANSCRICAO | [09:08] Diego |
| ADR-001-CON-06 | `docs/adrs/ADR-001-outbox-no-mysql.md` | Consequência | Falha na outbox passa a impedir a mudança de status | TRANSCRICAO | [09:40] Bruno |
| ADR-001-TO | `docs/adrs/ADR-001-outbox-no-mysql.md` | Trade-off | Mais escrita no banco em troca de consistência sem nova infraestrutura | TRANSCRICAO | [09:07] Bruno; [09:07] Diego |
| ADR-001-COD-01 | `docs/adrs/ADR-001-outbox-no-mysql.md` | Integração | Transação de changeStatus receberá a inserção | CODIGO | `src/modules/orders/order.service.ts` |
| ADR-001-COD-02 | `docs/adrs/ADR-001-outbox-no-mysql.md` | Referência de código | Movimento de estoque é condicional à transição | CODIGO | `src/modules/orders/order.status.ts` |
| ADR-001-COD-03 | `docs/adrs/ADR-001-outbox-no-mysql.md` | Referência de código | MySQL é o único serviço de infraestrutura | CODIGO | `docker-compose.yml` |
| ADR-002 | `docs/adrs/ADR-002-worker-em-processo-separado-com-polling.md` | Decisão | Worker em processo separado, com polling de 2 segundos | TRANSCRICAO | [09:10] Larissa; [09:11] Diego |
| ADR-002-CTX-01 | `docs/adrs/ADR-002-worker-em-processo-separado-com-polling.md` | Restrição | MySQL não tem listener nativo | TRANSCRICAO | [09:09] Diego |
| ADR-002-CTX-02 | `docs/adrs/ADR-002-worker-em-processo-separado-com-polling.md` | Requisito Não Funcional | Polling de 2 segundos aceito para o limite de 10 segundos | TRANSCRICAO | [09:10] Marcos |
| ADR-002-CTX-03 | `docs/adrs/ADR-002-worker-em-processo-separado-com-polling.md` | Restrição | Worker na API cairia junto em um reinício | TRANSCRICAO | [09:11] Diego |
| ADR-002-CTX-04 | `docs/adrs/ADR-002-worker-em-processo-separado-com-polling.md` | Restrição | Mesmo banco e stack, em outro processo | TRANSCRICAO | [09:11] Diego |
| ADR-002-ALT-01 | `docs/adrs/ADR-002-worker-em-processo-separado-com-polling.md` | Alternativa | Trigger no banco descartada: não notifica processo externo | TRANSCRICAO | [09:09] Diego |
| ADR-002-ALT-02 | `docs/adrs/ADR-002-worker-em-processo-separado-com-polling.md` | Alternativa | Worker dentro da API descartado | TRANSCRICAO | [09:11] Diego |
| ADR-002-CON-01 | `docs/adrs/ADR-002-worker-em-processo-separado-com-polling.md` | Consequência | Reinício da API não interrompe o worker | TRANSCRICAO | [09:11] Diego |
| ADR-002-CON-02 | `docs/adrs/ADR-002-worker-em-processo-separado-com-polling.md` | Consequência | Sem notificação improvisada entre banco e worker | TRANSCRICAO | [09:09] Diego |
| ADR-002-CON-03 | `docs/adrs/ADR-002-worker-em-processo-separado-com-polling.md` | Consequência | Stack reaproveitada no consumo | TRANSCRICAO | [09:11] Diego |
| ADR-002-CON-04 | `docs/adrs/ADR-002-worker-em-processo-separado-com-polling.md` | Consequência | Espera do polling aceita explicitamente | TRANSCRICAO | [09:10] Larissa |
| ADR-002-CON-05 | `docs/adrs/ADR-002-worker-em-processo-separado-com-polling.md` | Consequência | Segundo processo com entrada e script próprios | TRANSCRICAO | [09:11] Larissa |
| ADR-002-CON-06 | `docs/adrs/ADR-002-worker-em-processo-separado-com-polling.md` | Consequência | Análise do código: worker precisa de toda a configuração, inclusive JWT_SECRET | CODIGO | `src/config/env.ts` |
| ADR-002-TO | `docs/adrs/ADR-002-worker-em-processo-separado-com-polling.md` | Trade-off | Espera e processo extra em troca de simplicidade | TRANSCRICAO | [09:10] Larissa |
| ADR-002-COD-01 | `docs/adrs/ADR-002-worker-em-processo-separado-com-polling.md` | Referência de código | Entrada de processo atual usada como referência | CODIGO | `src/server.ts` |
| ADR-002-COD-02 | `docs/adrs/ADR-002-worker-em-processo-separado-com-polling.md` | Integração | Criação do PrismaClient | CODIGO | `src/config/database.ts` |
| ADR-002-COD-03 | `docs/adrs/ADR-002-worker-em-processo-separado-com-polling.md` | Integração | Validação de DATABASE_URL e demais variáveis | CODIGO | `src/config/env.ts` |
| ADR-002-COD-04 | `docs/adrs/ADR-002-worker-em-processo-separado-com-polling.md` | Integração | Local do novo script worker | CODIGO | `package.json` |
| ADR-003 | `docs/adrs/ADR-003-worker-unico-e-ordenacao-por-pedido.md` | Decisão | Worker único e ordem apenas por pedido, como limitação conhecida | TRANSCRICAO | [09:13] Larissa |
| ADR-003-CTX-01 | `docs/adrs/ADR-003-worker-unico-e-ordenacao-por-pedido.md` | Contexto | Worker único processa em ordem de created_at | TRANSCRICAO | [09:12] Diego |
| ADR-003-CTX-02 | `docs/adrs/ADR-003-worker-unico-e-ordenacao-por-pedido.md` | Restrição | Workers em paralelo perdem a garantia | TRANSCRICAO | [09:12] Diego |
| ADR-003-CTX-03 | `docs/adrs/ADR-003-worker-unico-e-ordenacao-por-pedido.md` | Contexto | Clientes nunca pediram ordem global | TRANSCRICAO | [09:14] Marcos |
| ADR-003-ALT-01 | `docs/adrs/ADR-003-worker-unico-e-ordenacao-por-pedido.md` | Alternativa | Particionar por order_id, adiado | TRANSCRICAO | [09:13] Diego |
| ADR-003-ALT-02 | `docs/adrs/ADR-003-worker-unico-e-ordenacao-por-pedido.md` | Alternativa | Lock pessimista, adiado | TRANSCRICAO | [09:13] Diego |
| ADR-003-ALT-03 | `docs/adrs/ADR-003-worker-unico-e-ordenacao-por-pedido.md` | Alternativa | Ordem global descartada | TRANSCRICAO | [09:14] Marcos |
| ADR-003-CON-01 | `docs/adrs/ADR-003-worker-unico-e-ordenacao-por-pedido.md` | Consequência | Ordem sem coordenação adicional | TRANSCRICAO | [09:12] Diego |
| ADR-003-CON-02 | `docs/adrs/ADR-003-worker-unico-e-ordenacao-por-pedido.md` | Consequência | Sem particionamento nem locks nesta entrega | TRANSCRICAO | [09:13] Diego |
| ADR-003-CON-03 | `docs/adrs/ADR-003-worker-unico-e-ordenacao-por-pedido.md` | Consequência | Escalar exige rever a decisão | TRANSCRICAO | [09:12] Diego |
| ADR-003-CON-04 | `docs/adrs/ADR-003-worker-unico-e-ordenacao-por-pedido.md` | Consequência | Sem ordem entre pedidos diferentes | TRANSCRICAO | [09:13] Larissa |
| ADR-003-CON-05 | `docs/adrs/ADR-003-worker-unico-e-ordenacao-por-pedido.md` | Risco | Análise: worker único pode esperar 10 segundos em cada cliente lento | TRANSCRICAO | [09:42] Diego |
| ADR-003-TO | `docs/adrs/ADR-003-worker-unico-e-ordenacao-por-pedido.md` | Trade-off | Um só worker e sem ordem global, em troca de ordem por pedido sem coordenação | TRANSCRICAO | [09:13] Larissa; [09:14] Marcos |
| ADR-003-COD-01 | `docs/adrs/ADR-003-worker-unico-e-ordenacao-por-pedido.md` | Referência de código | Transições permitem PAID, PROCESSING e SHIPPED | CODIGO | `src/modules/orders/order.status.ts` |
| ADR-004 | `docs/adrs/ADR-004-retry-com-backoff-e-dlq.md` | Decisão | 5 tentativas com backoff 1m/5m/30m/2h/12h e DLQ em tabela separada | TRANSCRICAO | [09:17] Larissa; [09:18] Diego; [09:48] Larissa |
| ADR-004-CTX-01 | `docs/adrs/ADR-004-retry-com-backoff-e-dlq.md` | Restrição | Retry indefinido deixaria evento pendurado | TRANSCRICAO | [09:15] Diego |
| ADR-004-CTX-02 | `docs/adrs/ADR-004-retry-com-backoff-e-dlq.md` | Contexto | Cliente já teve duas horas de indisponibilidade | TRANSCRICAO | [09:16] Diego |
| ADR-004-CTX-03 | `docs/adrs/ADR-004-retry-com-backoff-e-dlq.md` | Contexto | Janela de quase 15 horas considerada aceitável | TRANSCRICAO | [09:17] Marcos |
| ADR-004-CTX-04 | `docs/adrs/ADR-004-retry-com-backoff-e-dlq.md` | Requisito Funcional | Falha guardada como evidência para debug e reprocessamento | TRANSCRICAO | [09:18] Diego |
| ADR-004-INT-01 | `docs/adrs/ADR-004-retry-com-backoff-e-dlq.md` | Interpretação | 5 tentativas como 5 reenvios; soma de 14h36, fechado em RFC-QA-07 | TRANSCRICAO | [09:17] Diego |
| ADR-004-ALT-01 | `docs/adrs/ADR-004-retry-com-backoff-e-dlq.md` | Alternativa | Retry indefinido descartado | TRANSCRICAO | [09:15] Diego |
| ADR-004-ALT-02 | `docs/adrs/ADR-004-retry-com-backoff-e-dlq.md` | Alternativa | 3 tentativas descartadas | TRANSCRICAO | [09:16] Diego |
| ADR-004-ALT-03 | `docs/adrs/ADR-004-retry-com-backoff-e-dlq.md` | Alternativa | Marcar failed na outbox preterido em favor de tabela separada | TRANSCRICAO | [09:18] Diego |
| ADR-004-CON-01 | `docs/adrs/ADR-004-retry-com-backoff-e-dlq.md` | Consequência | Janela cobre manutenção de duas horas | TRANSCRICAO | [09:16] Diego |
| ADR-004-CON-02 | `docs/adrs/ADR-004-retry-com-backoff-e-dlq.md` | Consequência | Nenhum evento fica em retry para sempre | TRANSCRICAO | [09:15] Diego |
| ADR-004-CON-03 | `docs/adrs/ADR-004-retry-com-backoff-e-dlq.md` | Consequência | Falhas permanentes ficam guardadas | TRANSCRICAO | [09:18] Diego |
| ADR-004-CON-04 | `docs/adrs/ADR-004-retry-com-backoff-e-dlq.md` | Restrição | Replay restrito a ADMIN, com log de quem executou | TRANSCRICAO | [09:36] Sofia |
| ADR-004-CON-05 | `docs/adrs/ADR-004-retry-com-backoff-e-dlq.md` | Consequência | Depois da janela, entrega depende de replay manual | TRANSCRICAO | [09:18] Diego |
| ADR-004-CON-06 | `docs/adrs/ADR-004-retry-com-backoff-e-dlq.md` | Consequência | Evento pode levar quase 15 horas até a DLQ | TRANSCRICAO | [09:17] Diego |
| ADR-004-CON-07 | `docs/adrs/ADR-004-retry-com-backoff-e-dlq.md` | Consequência | Tabela adicional prevista na estimativa | TRANSCRICAO | [09:46] Larissa |
| ADR-004-TO | `docs/adrs/ADR-004-retry-com-backoff-e-dlq.md` | Trade-off | Desistir após quase 15 horas em troca de não manter eventos pendurados | TRANSCRICAO | [09:15] Diego; [09:17] Marcos |
| ADR-004-COD-01 | `docs/adrs/ADR-004-retry-com-backoff-e-dlq.md` | Integração | requireRole reaproveitado no replay | CODIGO | `src/middlewares/auth.middleware.ts` |
| ADR-004-COD-02 | `docs/adrs/ADR-004-retry-com-backoff-e-dlq.md` | Referência de código | Uso existente de requireRole com ADMIN | CODIGO | `src/modules/users/user.routes.ts` |
| ADR-005 | `docs/adrs/ADR-005-hmac-sha256-com-secret-por-endpoint.md` | Decisão | HMAC-SHA256 sobre o corpo, secret por endpoint e rotação com 24 horas | TRANSCRICAO | [09:22] Sofia |
| ADR-005-CTX-01 | `docs/adrs/ADR-005-hmac-sha256-com-secret-por-endpoint.md` | Contexto | HMAC-SHA256 é padrão de mercado | TRANSCRICAO | [09:20] Sofia |
| ADR-005-CTX-02 | `docs/adrs/ADR-005-hmac-sha256-com-secret-por-endpoint.md` | Restrição | Secret global faria um vazamento atingir todos | TRANSCRICAO | [09:21] Sofia |
| ADR-005-CTX-03 | `docs/adrs/ADR-005-hmac-sha256-com-secret-por-endpoint.md` | Requisito Funcional | Cliente precisa de tempo para migrar a secret | TRANSCRICAO | [09:21] Sofia |
| ADR-005-CTX-04 | `docs/adrs/ADR-005-hmac-sha256-com-secret-por-endpoint.md` | Contexto | Cliente já vazou secret em log da própria aplicação | TRANSCRICAO | [09:22] Diego |
| ADR-005-ALT-01 | `docs/adrs/ADR-005-hmac-sha256-com-secret-por-endpoint.md` | Alternativa | Secret global descartada | TRANSCRICAO | [09:21] Sofia |
| ADR-005-ALT-02 | `docs/adrs/ADR-005-hmac-sha256-com-secret-por-endpoint.md` | Alternativa | Troca imediata (plausível, não discutida) contrária às 24 horas decididas | TRANSCRICAO | [09:21] Sofia |
| ADR-005-CON-01 | `docs/adrs/ADR-005-hmac-sha256-com-secret-por-endpoint.md` | Consequência | Cliente verifica origem e integridade | TRANSCRICAO | [09:19] Sofia |
| ADR-005-CON-02 | `docs/adrs/ADR-005-hmac-sha256-com-secret-por-endpoint.md` | Consequência | Vazamento fica limitado ao endpoint | TRANSCRICAO | [09:21] Sofia |
| ADR-005-CON-03 | `docs/adrs/ADR-005-hmac-sha256-com-secret-por-endpoint.md` | Consequência | Rotação dá tempo para migrar; relevante diante de vazamentos | TRANSCRICAO | [09:22] Diego |
| ADR-005-CON-04 | `docs/adrs/ADR-005-hmac-sha256-com-secret-por-endpoint.md` | Consequência | Cliente implementa a verificação | TRANSCRICAO | [09:20] Sofia |
| ADR-005-CON-05 | `docs/adrs/ADR-005-hmac-sha256-com-secret-por-endpoint.md` | Restrição | Revisão de segurança de pelo menos dois dias úteis antes do deploy | TRANSCRICAO | [09:46] Sofia |
| ADR-005-CON-06 | `docs/adrs/ADR-005-hmac-sha256-com-secret-por-endpoint.md` | Risco | Análise: X-Timestamp fica fora da assinatura do corpo | TRANSCRICAO | [09:44] Diego |
| ADR-005-TO | `docs/adrs/ADR-005-hmac-sha256-com-secret-por-endpoint.md` | Trade-off | Gerenciar secrets e verificar no cliente em troca de limitar vazamentos | TRANSCRICAO | [09:21] Sofia |
| ADR-005-COD-01 | `docs/adrs/ADR-005-hmac-sha256-com-secret-por-endpoint.md` | Referência de código | Lista de campos ocultados não inclui secret; tratamento em FDD-ND-18 | CODIGO | `src/shared/logger/index.ts` |
| ADR-006 | `docs/adrs/ADR-006-at-least-once-com-x-event-id.md` | Decisão | Entrega at-least-once com X-Event-Id para deduplicação | TRANSCRICAO | [09:26] Larissa |
| ADR-006-CTX-01 | `docs/adrs/ADR-006-at-least-once-com-x-event-id.md` | Restrição | Cliente pode receber o mesmo evento duas vezes | TRANSCRICAO | [09:24] Diego |
| ADR-006-CTX-02 | `docs/adrs/ADR-006-at-least-once-com-x-event-id.md` | Contexto | Exactly-once exigiria coordenação dos dois lados | TRANSCRICAO | [09:25] Diego |
| ADR-006-CTX-03 | `docs/adrs/ADR-006-at-least-once-com-x-event-id.md` | Contexto | Stripe e GitHub citados como referência | TRANSCRICAO | [09:25] Diego |
| ADR-006-ALT-01 | `docs/adrs/ADR-006-at-least-once-com-x-event-id.md` | Alternativa | Exactly-once descartado pela complexidade | TRANSCRICAO | [09:25] Diego |
| ADR-006-CON-01 | `docs/adrs/ADR-006-at-least-once-com-x-event-id.md` | Consequência | Identificador permite deduplicar reenvios | TRANSCRICAO | [09:25] Diego |
| ADR-006-CON-02 | `docs/adrs/ADR-006-at-least-once-com-x-event-id.md` | Consequência | Sem coordenação com sistemas dos clientes | TRANSCRICAO | [09:25] Diego |
| ADR-006-CON-03 | `docs/adrs/ADR-006-at-least-once-com-x-event-id.md` | Consequência | Deduplicação fica com o cliente | TRANSCRICAO | [09:25] Sofia |
| ADR-006-CON-04 | `docs/adrs/ADR-006-at-least-once-com-x-event-id.md` | Dependência | Comportamento destacado no portal de desenvolvedor | TRANSCRICAO | [09:26] Marcos |
| ADR-006-TO | `docs/adrs/ADR-006-at-least-once-com-x-event-id.md` | Trade-off | Duplicatas possíveis em troca de entrega mais simples | TRANSCRICAO | [09:26] Larissa |
| ADR-006-COD-01 | `docs/adrs/ADR-006-at-least-once-com-x-event-id.md` | Referência de código | Dependência uuid já disponível | CODIGO | `package.json` |
| ADR-007 | `docs/adrs/ADR-007-reuso-dos-padroes-do-projeto.md` | Decisão | Reuso máximo dos padrões, com webhook como módulo comum | TRANSCRICAO | [09:30] Larissa |
| ADR-007-CTX-01 | `docs/adrs/ADR-007-reuso-dos-padroes-do-projeto.md` | Restrição | Módulo com controller, service, repository, routes e schemas | TRANSCRICAO | [09:27] Bruno |
| ADR-007-CTX-02 | `docs/adrs/ADR-007-reuso-dos-padroes-do-projeto.md` | Restrição | AppError, classes específicas e códigos padronizados | TRANSCRICAO | [09:28] Bruno |
| ADR-007-CTX-03 | `docs/adrs/ADR-007-reuso-dos-padroes-do-projeto.md` | Restrição | Pino reaproveitado, sem nada novo | TRANSCRICAO | [09:29] Bruno |
| ADR-007-CTX-04 | `docs/adrs/ADR-007-reuso-dos-padroes-do-projeto.md` | Contexto | Middleware centralizado trata os novos erros | TRANSCRICAO | [09:29] Bruno |
| ADR-007-ALT-01 | `docs/adrs/ADR-007-reuso-dos-padroes-do-projeto.md` | Alternativa | Componentes novos de log ou erro (plausível, não discutida), contrários ao reuso máximo | TRANSCRICAO | [09:30] Larissa |
| ADR-007-ALT-02 | `docs/adrs/ADR-007-reuso-dos-padroes-do-projeto.md` | Alternativa | Fora de src/modules (plausível, não discutida) contrário à decisão | TRANSCRICAO | [09:30] Larissa |
| ADR-007-CON-01 | `docs/adrs/ADR-007-reuso-dos-padroes-do-projeto.md` | Consequência | Erros do módulo passam pelo middleware sem alterá-lo | TRANSCRICAO | [09:29] Bruno |
| ADR-007-CON-02 | `docs/adrs/ADR-007-reuso-dos-padroes-do-projeto.md` | Consequência | Mesma estrutura dos outros módulos | TRANSCRICAO | [09:27] Bruno |
| ADR-007-CON-03 | `docs/adrs/ADR-007-reuso-dos-padroes-do-projeto.md` | Restrição | NotFoundError fixa NOT_FOUND; WEBHOOK_NOT_FOUND precisa de classe própria | CODIGO | `src/shared/errors/http-errors.ts` |
| ADR-007-CON-04 | `docs/adrs/ADR-007-reuso-dos-padroes-do-projeto.md` | Restrição | validate converte falhas de schema em VALIDATION_ERROR | CODIGO | `src/middlewares/validate.middleware.ts` |
| ADR-007-CON-05 | `docs/adrs/ADR-007-reuso-dos-padroes-do-projeto.md` | Referência de código | Lista de campos ocultados não inclui secret; tratamento em FDD-ND-18 | CODIGO | `src/shared/logger/index.ts` |
| ADR-007-TO | `docs/adrs/ADR-007-reuso-dos-padroes-do-projeto.md` | Trade-off | Consistência e nada novo de log ou erro; limites do reuso vêm de análise do código | TRANSCRICAO | [09:30] Larissa |
| ADR-007-COD-01 | `docs/adrs/ADR-007-reuso-dos-padroes-do-projeto.md` | Referência de código | Classe base AppError | CODIGO | `src/shared/errors/app-error.ts` |
| ADR-007-COD-02 | `docs/adrs/ADR-007-reuso-dos-padroes-do-projeto.md` | Referência de código | Classes específicas e NotFoundError | CODIGO | `src/shared/errors/http-errors.ts` |
| ADR-007-COD-03 | `docs/adrs/ADR-007-reuso-dos-padroes-do-projeto.md` | Integração | Tratamento centralizado de erros | CODIGO | `src/middlewares/error.middleware.ts` |
| ADR-007-COD-04 | `docs/adrs/ADR-007-reuso-dos-padroes-do-projeto.md` | Integração | Validação por schema Zod | CODIGO | `src/middlewares/validate.middleware.ts` |
| ADR-007-COD-05 | `docs/adrs/ADR-007-reuso-dos-padroes-do-projeto.md` | Integração | Logger Pino e campos ocultados | CODIGO | `src/shared/logger/index.ts` |
| ADR-008 | `docs/adrs/ADR-008-snapshot-e-filtro-na-insercao.md` | Decisão | Filtro por status e snapshot do payload na inserção | TRANSCRICAO | [09:52] Bruno; [09:34] Bruno |
| ADR-008-CTX-01 | `docs/adrs/ADR-008-snapshot-e-filtro-na-insercao.md` | Restrição | Sem interessado no status, nada é inserido | TRANSCRICAO | [09:34] Bruno |
| ADR-008-CTX-02 | `docs/adrs/ADR-008-snapshot-e-filtro-na-insercao.md` | Restrição | Evento reflete o estado de quando o status mudou | TRANSCRICAO | [09:52] Larissa |
| ADR-008-ALT-01 | `docs/adrs/ADR-008-snapshot-e-filtro-na-insercao.md` | Alternativa | Filtrar no envio descartado | TRANSCRICAO | [09:34] Bruno |
| ADR-008-ALT-02 | `docs/adrs/ADR-008-snapshot-e-filtro-na-insercao.md` | Alternativa | Renderizar no envio descartado | TRANSCRICAO | [09:52] Larissa |
| ADR-008-CON-01 | `docs/adrs/ADR-008-snapshot-e-filtro-na-insercao.md` | Consequência | Outbox só recebe eventos com interessado | TRANSCRICAO | [09:34] Bruno |
| ADR-008-CON-02 | `docs/adrs/ADR-008-snapshot-e-filtro-na-insercao.md` | Consequência | Evento reflete o momento da mudança; reenvios usam o mesmo conteúdo, por análise | TRANSCRICAO | [09:52] Larissa |
| ADR-008-CON-03 | `docs/adrs/ADR-008-snapshot-e-filtro-na-insercao.md` | Consequência | Análise: alteração de filtro vale para as próximas mudanças | TRANSCRICAO | [09:34] Bruno |
| ADR-008-CON-04 | `docs/adrs/ADR-008-snapshot-e-filtro-na-insercao.md` | Consequência | Payload deve ser enxuto, sem items | TRANSCRICAO | [09:43] Diego |
| ADR-008-CON-05 | `docs/adrs/ADR-008-snapshot-e-filtro-na-insercao.md` | Consequência | Análise: consulta do filtro e renderização entram na transação | TRANSCRICAO | [09:40] Bruno |
| ADR-008-TO | `docs/adrs/ADR-008-snapshot-e-filtro-na-insercao.md` | Trade-off | Mais trabalho na inserção em troca de menos linhas e snapshot fiel | TRANSCRICAO | [09:34] Bruno; [09:52] Larissa |
| ADR-008-COD-01 | `docs/adrs/ADR-008-snapshot-e-filtro-na-insercao.md` | Integração | changeStatus já tem os dados necessários para o evento | CODIGO | `src/modules/orders/order.service.ts` |
| ADR-008-COD-02 | `docs/adrs/ADR-008-snapshot-e-filtro-na-insercao.md` | Referência de código | Campos do payload existem no modelo Order | CODIGO | `prisma/schema.prisma` |
| RFC-CTX-01 | `docs/RFC.md` | Contexto | Três clientes B2B pediram notificação; polling atual é lento e caro | TRANSCRICAO | [09:00] Marcos |
| RFC-CTX-02 | `docs/RFC.md` | Requisito Não Funcional | Abaixo de 10 segundos já é tempo real para os clientes | TRANSCRICAO | [09:02] Marcos |
| RFC-CTX-03 | `docs/RFC.md` | Restrição | Webhooks apenas de saída | TRANSCRICAO | [09:02] Marcos |
| RFC-CTX-04 | `docs/RFC.md` | Restrição | Transação de status não pode depender da disponibilidade do cliente | TRANSCRICAO | [09:04] Bruno |
| RFC-PROP-01 | `docs/RFC.md` | Decisão | Publicação na transação, com filtro e snapshot | TRANSCRICAO | [09:40] Bruno; [09:34] Bruno; [09:52] Larissa |
| RFC-PROP-02 | `docs/RFC.md` | Decisão | Worker em processo próprio lendo pendentes em batch pequeno | TRANSCRICAO | [09:11] Diego; [09:08] Diego |
| RFC-PROP-03 | `docs/RFC.md` | Decisão | Backoff, timeout de 10 segundos, DLQ e replay por ADMIN | TRANSCRICAO | [09:48] Larissa; [09:42] Diego |
| RFC-PROP-04 | `docs/RFC.md` | Decisão | Secret por endpoint, HMAC-SHA256 e deduplicação por X-Event-Id | TRANSCRICAO | [09:48] Larissa |
| RFC-PROP-05 | `docs/RFC.md` | Requisito Funcional | Módulo com CRUD, rotação e histórico, sem painel visual | TRANSCRICAO | [09:40] Larissa; [09:33] Bruno; [09:21] Sofia; [09:34] Marcos |
| RFC-ALT-01 | `docs/RFC.md` | Alternativa | HTTP síncrono descartado | TRANSCRICAO | [09:04] Bruno |
| RFC-ALT-02 | `docs/RFC.md` | Alternativa | Redis Streams ou Cluster descartado | TRANSCRICAO | [09:07] Diego |
| RFC-ALT-03 | `docs/RFC.md` | Alternativa | Trigger no MySQL descartada | TRANSCRICAO | [09:09] Diego |
| RFC-ALT-04 | `docs/RFC.md` | Alternativa | Exactly-once descartado | TRANSCRICAO | [09:25] Diego |
| RFC-ALT-05 | `docs/RFC.md` | Alternativa | Retry indefinido descartado | TRANSCRICAO | [09:15] Diego |
| RFC-QA-01 | `docs/RFC.md` | Questão em aberto | Rate limiting de saída: observar e decidir depois | TRANSCRICAO | [09:39] Larissa |
| RFC-QA-02 | `docs/RFC.md` | Questão em aberto | E-mail de aviso adiado para possível próxima fase | TRANSCRICAO | [09:37] Larissa |
| RFC-QA-03 | `docs/RFC.md` | Questão em aberto | Escala para vários workers adiada | TRANSCRICAO | [09:13] Diego |
| RFC-QA-04 | `docs/RFC.md` | Questão em aberto | Endurecimento futuro das roles do CRUD | TRANSCRICAO | [09:37] Sofia |
| RFC-QA-06 | `docs/RFC.md` | Não definido | customer_id no body ou no path | TRANSCRICAO | [09:32] Larissa |
| RFC-QA-07 | `docs/RFC.md` | Interpretação | 5 tentativas como 5 reenvios; única leitura compatível com quase 15 horas | TRANSCRICAO | [09:17] Diego |
| RFC-QA-08 | `docs/RFC.md` | Decisão do autor | Evento em retry segura os seguintes do mesmo pedido | TRANSCRICAO | [09:13] Larissa |
| RFC-QA-09 | `docs/RFC.md` | Não definido | Como um evento atende a vários webhooks | TRANSCRICAO | [09:44] Sofia |
| RFC-QA-10 | `docs/RFC.md` | Não definido | Assinatura durante as 24 horas de validade paralela | TRANSCRICAO | [09:21] Sofia |
| RFC-QA-11 | `docs/RFC.md` | Decisão do autor | Secret em texto na tabela, levada à revisão de Sofia | TRANSCRICAO | [09:21] Bruno |
| RFC-QA-12 | `docs/RFC.md` | Não definido | Limite de customer por usuário; só a role foi definida | TRANSCRICAO | [09:37] Sofia |
| RFC-QA-13 | `docs/RFC.md` | Derivado | Métricas e rastreio só com dados definidos e sem ferramenta nova | TRANSCRICAO | [09:30] Larissa |
| RFC-QA-14 | `docs/RFC.md` | Não definido | Retenção do histórico de entregas | TRANSCRICAO | [09:34] Marcos |
| RFC-IMP-01 | `docs/RFC.md` | Impacto | changeStatus é o ponto crítico da alteração | TRANSCRICAO | [09:40] Bruno |
| RFC-IMP-02 | `docs/RFC.md` | Impacto | Segundo processo, novo módulo e novas tabelas | TRANSCRICAO | [09:11] Diego; [09:27] Bruno |
| RFC-IMP-03 | `docs/RFC.md` | Impacto | Clientes verificam assinatura e deduplicam; documentação no portal | TRANSCRICAO | [09:20] Sofia; [09:26] Marcos |
| RFC-IMP-04 | `docs/RFC.md` | Restrição | Três sprints, com revisão de segurança | TRANSCRICAO | [09:47] Larissa; [09:46] Sofia |
| RFC-RISK-02 | `docs/RFC.md` | Risco | Worker único pode atrasar por cliente lento; efeito não discutido | TRANSCRICAO | [09:42] Diego |
| RFC-RISK-03 | `docs/RFC.md` | Risco | Registro público aceita role ADMIN e enfraquece a proteção do replay | CODIGO | `src/modules/auth/auth.schemas.ts` |
| RFC-RISK-04 | `docs/RFC.md` | Risco | JWT sem vínculo com customer | CODIGO | `src/middlewares/auth.middleware.ts` |
| RFC-RISK-05 | `docs/RFC.md` | Risco | Falha na outbox impede a mudança de status | TRANSCRICAO | [09:40] Bruno |
| RFC-QA-15 | `docs/RFC.md` | Não definido | Como obter o id da DLQ sem listagem definida | TRANSCRICAO | [09:18] Bruno |
| RFC-QA-16 | `docs/RFC.md` | Não definido | Demais detalhes de implementação, para a sessão de revisão do design | TRANSCRICAO | [09:50] Larissa |
| FDD-CTX-01 | `docs/FDD.md` | Contexto | Clientes consultam GET /orders e querem notificação | TRANSCRICAO | [09:00] Marcos |
| FDD-CTX-02 | `docs/FDD.md` | Contexto | HTTP na transação travaria outras mudanças de status | TRANSCRICAO | [09:04] Bruno |
| FDD-CTX-03 | `docs/FDD.md` | Referência de código | Estoque só muda em transições específicas | CODIGO | `src/modules/orders/order.status.ts` |
| FDD-CTX-04 | `docs/FDD.md` | Contexto | Usuários autenticados representam o cliente | TRANSCRICAO | [09:32] Marcos |
| FDD-OBJ-01 | `docs/FDD.md` | Requisito Não Funcional | Primeira tentativa em menos de 10 segundos | TRANSCRICAO | [09:02] Marcos |
| FDD-OBJ-02 | `docs/FDD.md` | Requisito Não Funcional | Status e evento consistentes | TRANSCRICAO | [09:06] Diego |
| FDD-OBJ-03 | `docs/FDD.md` | Requisito Não Funcional | Entrega ao menos uma vez, com o mesmo event_id | TRANSCRICAO | [09:24] Diego |
| FDD-OBJ-04 | `docs/FDD.md` | Requisito Funcional | Intervalos de retry decididos | TRANSCRICAO | [09:17] Larissa |
| FDD-OBJ-05 | `docs/FDD.md` | Requisito Funcional | HMAC-SHA256 do corpo em toda entrega | TRANSCRICAO | [09:22] Sofia |
| FDD-OBJ-06 | `docs/FDD.md` | Restrição | Nenhuma biblioteca nova de log ou erro | TRANSCRICAO | [09:30] Larissa |
| FDD-ESC-01 | `docs/FDD.md` | Escopo | Publicação transacional em changeStatus | TRANSCRICAO | [09:40] Bruno |
| FDD-ESC-02 | `docs/FDD.md` | Escopo | Worker, polling, retry e DLQ | TRANSCRICAO | [09:48] Larissa |
| FDD-ESC-03 | `docs/FDD.md` | Escopo | CRUD de configuração | TRANSCRICAO | [09:33] Bruno |
| FDD-ESC-04 | `docs/FDD.md` | Escopo | Rotação de secret | TRANSCRICAO | [09:21] Sofia |
| FDD-ESC-05 | `docs/FDD.md` | Escopo | Histórico de entregas | TRANSCRICAO | [09:34] Marcos |
| FDD-ESC-06 | `docs/FDD.md` | Escopo | Replay manual da DLQ | TRANSCRICAO | [09:35] Diego |
| FDD-EXC-01 | `docs/FDD.md` | Fora de escopo | Webhooks de entrada | TRANSCRICAO | [09:02] Marcos |
| FDD-EXC-02 | `docs/FDD.md` | Fora de escopo | E-mail de aviso | TRANSCRICAO | [09:37] Larissa |
| FDD-EXC-03 | `docs/FDD.md` | Fora de escopo | Dashboard visual | TRANSCRICAO | [09:40] Larissa |
| FDD-EXC-04 | `docs/FDD.md` | Fora de escopo | Rate limiting de saída | TRANSCRICAO | [09:39] Larissa |
| FDD-EXC-05 | `docs/FDD.md` | Fora de escopo | Arquivamento de entregues | TRANSCRICAO | [09:08] Diego |
| FDD-EXC-06 | `docs/FDD.md` | Fora de escopo | Múltiplos workers | TRANSCRICAO | [09:13] Diego |
| FDD-EXC-07 | `docs/FDD.md` | Fora de escopo | Evento na criação do pedido, fora do escopo discutido | TRANSCRICAO | [09:40] Bruno |
| FDD-EXC-08 | `docs/FDD.md` | Questão em aberto | Listagem da DLQ não foi discutida | TRANSCRICAO | [09:18] Bruno |
| FDD-DADOS-01 | `docs/FDD.md` | Integração | Novas tabelas e migration; colunas em camelCase | CODIGO | `prisma/schema.prisma` |
| FDD-DADOS-02 | `docs/FDD.md` | Decisão | webhook_outbox com uma linha por evento, status e índices | TRANSCRICAO | [09:06] Diego; [09:08] Diego |
| FDD-DADOS-03 | `docs/FDD.md` | Decisão | Ids em UUID | TRANSCRICAO | [09:51] Larissa |
| FDD-DADOS-04 | `docs/FDD.md` | Decisão | Payload renderizado na inserção | TRANSCRICAO | [09:52] Larissa |
| FDD-DADOS-05 | `docs/FDD.md` | Requisito Funcional | Cadastro com URL, secret, customer_id e estado ativo | TRANSCRICAO | [09:21] Bruno |
| FDD-DADOS-06 | `docs/FDD.md` | Decisão | Dead letter com payload, motivo e timestamp | TRANSCRICAO | [09:18] Diego |
| FDD-DADOS-07 | `docs/FDD.md` | Derivado | Nome de trabalho da tabela de configuração | TRANSCRICAO | [09:21] Bruno |
| FDD-DADOS-08 | `docs/FDD.md` | Derivado | Tabela de histórico necessária para consultar as entregas | TRANSCRICAO | [09:34] Marcos |
| FDD-DADOS-09 | `docs/FDD.md` | Referência de código | Tipo de chave e índices já usados; relações dependem de pontos não definidos | CODIGO | `prisma/schema.prisma` |
| FDD-FLUXO-01 | `docs/FDD.md` | Requisito Funcional | publishWebhookEvent consulta, filtra, renderiza e insere | TRANSCRICAO | [09:41] Bruno |
| FDD-FLUXO-02 | `docs/FDD.md` | Restrição | Erro na publicação reverte a transação | TRANSCRICAO | [09:40] Bruno |
| FDD-FLUXO-03 | `docs/FDD.md` | Referência de código | Validações anteriores encerram o fluxo sem evento | CODIGO | `src/modules/orders/order.service.ts` |
| FDD-FLUXO-04 | `docs/FDD.md` | Decisão | Ciclo de 2 segundos sobre pendentes mais antigos | TRANSCRICAO | [09:09] Diego |
| FDD-FLUXO-05 | `docs/FDD.md` | Requisito Funcional | Sequência de processamento de cada evento | TRANSCRICAO | [09:08] Diego |
| FDD-FLUXO-09 | `docs/FDD.md` | Decisão do autor | Evento em retry segura os seguintes do mesmo pedido | TRANSCRICAO | [09:13] Larissa |
| FDD-FLUXO-12 | `docs/FDD.md` | Interpretação | Tabela de retry com envio inicial e 5 reenvios | TRANSCRICAO | [09:17] Diego |
| FDD-FLUXO-13 | `docs/FDD.md` | Requisito Funcional | Reenvio mantém corpo e X-Event-Id | TRANSCRICAO | [09:25] Diego |
| FDD-FLUXO-14 | `docs/FDD.md` | Requisito Funcional | Falha permanente grava a DLQ | TRANSCRICAO | [09:18] Diego |
| FDD-FLUXO-15 | `docs/FDD.md` | Requisito Funcional | Replay recoloca como pendente; event_id mantido por derivação | TRANSCRICAO | [09:18] Diego |
| FDD-CONTRATO-01 | `docs/FDD.md` | Referência de código | API montada em /api/v1 | CODIGO | `src/app.ts` |
| FDD-CONTRATO-02 | `docs/FDD.md` | Contrato | POST /api/v1/webhooks | TRANSCRICAO | [09:31] Marcos |
| FDD-CONTRATO-03 | `docs/FDD.md` | Contrato | GET /api/v1/customers/:customerId/webhooks, com path entre as opções citadas | TRANSCRICAO | [09:33] Bruno |
| FDD-CONTRATO-04 | `docs/FDD.md` | Contrato | PATCH /api/v1/webhooks/:id | TRANSCRICAO | [09:33] Bruno |
| FDD-CONTRATO-05 | `docs/FDD.md` | Contrato | DELETE /api/v1/webhooks/:id | TRANSCRICAO | [09:33] Bruno |
| FDD-CONTRATO-06 | `docs/FDD.md` | Contrato | Rotação de secret, sem rota definida | TRANSCRICAO | [09:21] Sofia |
| FDD-CONTRATO-07 | `docs/FDD.md` | Contrato | GET /api/v1/webhooks/:id/deliveries | TRANSCRICAO | [09:34] Marcos |
| FDD-CONTRATO-08 | `docs/FDD.md` | Contrato | POST /api/v1/admin/webhooks/dead-letter/:id/replay | TRANSCRICAO | [09:35] Diego |
| FDD-CONTRATO-09 | `docs/FDD.md` | Contrato | Entrega com headers e payload definidos | TRANSCRICAO | [09:43] Diego; [09:44] Diego; [09:44] Sofia |
| FDD-CONTRATO-12 | `docs/FDD.md` | Contrato | Assinatura de publishWebhookEvent | TRANSCRICAO | [09:41] Bruno |
| FDD-ERRO-01 | `docs/FDD.md` | Erro | WEBHOOK_NOT_FOUND | TRANSCRICAO | [09:28] Bruno |
| FDD-ERRO-02 | `docs/FDD.md` | Erro | WEBHOOK_INVALID_URL para URL que não seja https | TRANSCRICAO | [09:23] Sofia |
| FDD-ERRO-03 | `docs/FDD.md` | Erro | WEBHOOK_PAYLOAD_TOO_LARGE | TRANSCRICAO | [09:24] Larissa |
| FDD-ERRO-04 | `docs/FDD.md` | Erro | WEBHOOK_DELIVERY_TIMEOUT | TRANSCRICAO | [09:42] Diego |
| FDD-ERRO-05 | `docs/FDD.md` | Erro | WEBHOOK_DELIVERY_FAILED para cliente offline | TRANSCRICAO | [09:14] Larissa |
| FDD-ERRO-08 | `docs/FDD.md` | Derivado | WEBHOOK_CUSTOMER_NOT_FOUND, como a checagem de customer em OrderService.create | CODIGO | `src/modules/orders/order.service.ts` |
| FDD-ERRO-09 | `docs/FDD.md` | Derivado | WEBHOOK_DEAD_LETTER_NOT_FOUND no replay | TRANSCRICAO | [09:35] Diego |
| FDD-ERRO-11 | `docs/FDD.md` | Derivado | WEBHOOK_PUBLISH_FAILED com rollback | TRANSCRICAO | [09:40] Bruno |
| FDD-ERRO-12 | `docs/FDD.md` | Restrição | NotFoundError fixa o código; 404 exigem classes próprias | CODIGO | `src/shared/errors/http-errors.ts` |
| FDD-ERRO-13 | `docs/FDD.md` | Restrição | Com validate atual, URL http responde VALIDATION_ERROR | CODIGO | `src/middlewares/validate.middleware.ts` |
| FDD-ERRO-14 | `docs/FDD.md` | Referência de código | Erros genéricos continuam vindo dos middlewares | CODIGO | `src/middlewares/auth.middleware.ts` |
| FDD-ERRO-15 | `docs/FDD.md` | Restrição | Ramo de AppError não grava log | CODIGO | `src/middlewares/error.middleware.ts` |
| FDD-ERRO-16 | `docs/FDD.md` | Decisão | WEBHOOK_SECRET_REQUIRED fora da matriz por não ter fluxo | TRANSCRICAO | [09:31] Marcos |
| FDD-RES-01 | `docs/FDD.md` | Requisito Não Funcional | Timeout de 10 segundos | TRANSCRICAO | [09:42] Diego |
| FDD-RES-02 | `docs/FDD.md` | Requisito Não Funcional | Teto de tentativas | TRANSCRICAO | [09:15] Diego |
| FDD-RES-03 | `docs/FDD.md` | Requisito Não Funcional | Backoff 1m/5m/30m/2h/12h | TRANSCRICAO | [09:17] Larissa |
| FDD-RES-04 | `docs/FDD.md` | Decisão | Fallback por DLQ, sem e-mail nesta fase | TRANSCRICAO | [09:48] Larissa |
| FDD-RES-05 | `docs/FDD.md` | Restrição | Falha do cliente não afeta a transação | TRANSCRICAO | [09:04] Bruno |
| FDD-RES-06 | `docs/FDD.md` | Requisito Não Funcional | Worker sobrevive a reinícios da API | TRANSCRICAO | [09:11] Diego |
| FDD-RES-07 | `docs/FDD.md` | Decisão | Duplicatas admitidas e tratadas pelo cliente | TRANSCRICAO | [09:26] Larissa |
| FDD-RES-08 | `docs/FDD.md` | Restrição | Sem status confirmado sem evento | TRANSCRICAO | [09:40] Bruno |
| FDD-RES-09 | `docs/FDD.md` | Restrição | Sem HTTP dentro de transação | TRANSCRICAO | [09:04] Bruno |
| FDD-RES-10 | `docs/FDD.md` | Restrição | Snapshot imutável nos reenvios | TRANSCRICAO | [09:52] Larissa |
| FDD-OBS-01 | `docs/FDD.md` | Requisito Não Funcional | Logs estruturados com o Pino | TRANSCRICAO | [09:29] Bruno |
| FDD-OBS-02 | `docs/FDD.md` | Requisito Não Funcional | Log de auditoria do replay | TRANSCRICAO | [09:36] Sofia |
| FDD-OBS-03 | `docs/FDD.md` | Referência de código | redactPaths não cobre campos de secret | CODIGO | `src/shared/logger/index.ts` |
| FDD-OBS-04 | `docs/FDD.md` | Derivado | Métricas exigidas pelo enunciado, obtidas de tabelas e logs | CODIGO | `package.json` |
| FDD-OBS-05 | `docs/FDD.md` | Derivado | Tracing exigido pelo enunciado, por correlação com eventId | TRANSCRICAO | [09:25] Diego |
| FDD-OBS-06 | `docs/FDD.md` | Referência de código | requestId existente segue rastreando a chamada à API | CODIGO | `src/middlewares/request-logger.middleware.ts` |
| FDD-INT-01 | `docs/FDD.md` | Integração | changeStatus chama publishWebhookEvent | CODIGO | `src/modules/orders/order.service.ts` |
| FDD-INT-02 | `docs/FDD.md` | Integração | Status de destino possíveis para o filtro | CODIGO | `src/modules/orders/order.status.ts` |
| FDD-INT-03 | `docs/FDD.md` | Integração | Novas classes de erro | CODIGO | `src/shared/errors/http-errors.ts` |
| FDD-INT-04 | `docs/FDD.md` | Integração | Exportação das novas classes | CODIGO | `src/shared/errors/index.ts` |
| FDD-INT-05 | `docs/FDD.md` | Integração | Middleware de erro sem alteração | CODIGO | `src/middlewares/error.middleware.ts` |
| FDD-INT-06 | `docs/FDD.md` | Integração | authenticate e requireRole | CODIGO | `src/middlewares/auth.middleware.ts` |
| FDD-INT-07 | `docs/FDD.md` | Integração | validate sem alteração | CODIGO | `src/middlewares/validate.middleware.ts` |
| FDD-INT-08 | `docs/FDD.md` | Integração | Reuso do logger no módulo e no worker | CODIGO | `src/shared/logger/index.ts` |
| FDD-INT-09 | `docs/FDD.md` | Integração | Registro dos routers e montagem da rota aninhada, testada em Express | CODIGO | `src/routes/index.ts` |
| FDD-INT-10 | `docs/FDD.md` | Integração | Instanciação do módulo | CODIGO | `src/app.ts` |
| FDD-INT-11 | `docs/FDD.md` | Integração | Modelos e migration | CODIGO | `prisma/schema.prisma` |
| FDD-INT-12 | `docs/FDD.md` | Integração | Instância de Prisma por processo | CODIGO | `src/config/database.ts` |
| FDD-INT-13 | `docs/FDD.md` | Integração | Referência para a entrada do worker | CODIGO | `src/server.ts` |
| FDD-INT-14 | `docs/FDD.md` | Integração | Script worker | CODIGO | `package.json` |
| FDD-INT-15 | `docs/FDD.md` | Integração | Paginação das listagens | CODIGO | `src/shared/http/response.ts` |
| FDD-INT-16 | `docs/FDD.md` | Referência de código | Sem alteração; efeito de excluir customer não definido | CODIGO | `src/modules/customers/customer.repository.ts` |
| FDD-INT-17 | `docs/FDD.md` | Integração | Factories de teste | CODIGO | `tests/helpers/factories.ts` |
| FDD-INT-18 | `docs/FDD.md` | Derivado | Limpeza de testes precisa incluir as novas tabelas | CODIGO | `tests/setup.ts` |
| FDD-DEP-01 | `docs/FDD.md` | Dependência | MySQL 8.0 | CODIGO | `docker-compose.yml` |
| FDD-DEP-02 | `docs/FDD.md` | Dependência | Prisma 5.22.0 | CODIGO | `package.json` |
| FDD-DEP-03 | `docs/FDD.md` | Dependência | Node.js 20 ou superior | CODIGO | `package.json` |
| FDD-DEP-04 | `docs/FDD.md` | Dependência | uuid 11.0.3 | CODIGO | `package.json` |
| FDD-DEP-05 | `docs/FDD.md` | Dependência | Zod 3.23.8 | CODIGO | `package.json` |
| FDD-DEP-06 | `docs/FDD.md` | Dependência | Pino 9.5.0 | CODIGO | `package.json` |
| FDD-DEP-07 | `docs/FDD.md` | Dependência | Revisão de segurança de pelo menos dois dias úteis | TRANSCRICAO | [09:46] Sofia |
| FDD-DEP-08 | `docs/FDD.md` | Dependência | Documentação no portal de desenvolvedor | TRANSCRICAO | [09:40] Marcos |
| FDD-DEP-09 | `docs/FDD.md` | Restrição | Contrato de PATCH de status não muda | TRANSCRICAO | [09:40] Bruno |
| FDD-DEP-10 | `docs/FDD.md` | Referência de código | Novas tabelas não alteram colunas existentes | CODIGO | `prisma/schema.prisma` |
| FDD-AC-01 | `docs/FDD.md` | Critério de Aceite | Evento pendente na mesma transação | TRANSCRICAO | [09:40] Bruno |
| FDD-AC-02 | `docs/FDD.md` | Critério de Aceite | Falha na inserção mantém o status antigo | TRANSCRICAO | [09:40] Bruno |
| FDD-AC-03 | `docs/FDD.md` | Critério de Aceite | Status fora do filtro não cria linha | TRANSCRICAO | [09:34] Bruno |
| FDD-AC-04 | `docs/FDD.md` | Critério de Aceite | Payload gravado não muda | TRANSCRICAO | [09:52] Larissa |
| FDD-AC-05 | `docs/FDD.md` | Critério de Aceite | npm run worker independente da API | TRANSCRICAO | [09:11] Larissa; [09:11] Diego |
| FDD-AC-06 | `docs/FDD.md` | Critério de Aceite | Primeira tentativa em menos de 10 segundos | TRANSCRICAO | [09:02] Marcos |
| FDD-AC-07 | `docs/FDD.md` | Critério de Aceite | Cinco headers e corpo sem items | TRANSCRICAO | [09:44] Diego; [09:44] Sofia; [09:43] Diego |
| FDD-AC-08 | `docs/FDD.md` | Critério de Aceite | HMAC confere com X-Signature | TRANSCRICAO | [09:22] Sofia |
| FDD-AC-09 | `docs/FDD.md` | Critério de Aceite | Ordem por pedido com um worker | TRANSCRICAO | [09:12] Diego |
| FDD-AC-10 | `docs/FDD.md` | Critério de Aceite | Timeout gera falha e nova tentativa | TRANSCRICAO | [09:42] Diego |
| FDD-AC-11 | `docs/FDD.md` | Critério de Aceite | Agendamentos seguem o backoff | TRANSCRICAO | [09:17] Larissa |
| FDD-AC-12 | `docs/FDD.md` | Critério de Aceite | DLQ guarda payload, motivo e timestamp | TRANSCRICAO | [09:18] Diego |
| FDD-AC-13 | `docs/FDD.md` | Critério de Aceite | Payload acima de 64KB não é enviado | TRANSCRICAO | [09:24] Larissa |
| FDD-AC-14 | `docs/FDD.md` | Critério de Aceite | Replay recoloca como pendente e registra autor | TRANSCRICAO | [09:18] Diego; [09:36] Sofia |
| FDD-AC-15 | `docs/FDD.md` | Critério de Aceite | Usuário sem role ADMIN recebe 403 no replay e usa o CRUD | TRANSCRICAO | [09:36] Sofia; [09:37] Sofia |
| FDD-AC-16 | `docs/FDD.md` | Critério de Aceite | URL http recusada | TRANSCRICAO | [09:23] Sofia |
| FDD-AC-17 | `docs/FDD.md` | Critério de Aceite | Secret devolvida na criação e única por endpoint | TRANSCRICAO | [09:31] Marcos; [09:21] Sofia |
| FDD-AC-18 | `docs/FDD.md` | Critério de Aceite | Secret antiga válida por 24 horas após a rotação | TRANSCRICAO | [09:21] Sofia |
| FDD-AC-19 | `docs/FDD.md` | Critério de Aceite | Histórico traz resultado, payload, resposta e tempo | TRANSCRICAO | [09:34] Marcos |
| FDD-AC-20 | `docs/FDD.md` | Critério de Aceite | Testes ponta a ponta da integração | TRANSCRICAO | [09:46] Larissa |
| FDD-RISK-01 | `docs/FDD.md` | Risco | Cliente lento atrasa os demais | TRANSCRICAO | [09:42] Diego |
| FDD-RISK-02 | `docs/FDD.md` | Risco | Crescimento das tabelas sem arquivamento | TRANSCRICAO | [09:07] Bruno |
| FDD-RISK-03 | `docs/FDD.md` | Risco | Exposição de secret | TRANSCRICAO | [09:22] Diego |
| FDD-RISK-04 | `docs/FDD.md` | Risco | Limites de acesso no código, analisados em RFC-RISK-03 e RFC-RISK-04 | CODIGO | `src/modules/auth/auth.schemas.ts` |
| FDD-RISK-05 | `docs/FDD.md` | Risco | Evento em retry retém os seguintes do mesmo pedido | TRANSCRICAO | [09:17] Diego |
| FDD-RISK-06 | `docs/FDD.md` | Risco | Análise: worker parado; eventos confirmados seguem registrados | TRANSCRICAO | [09:06] Diego |
| FDD-INT-19 | `docs/FDD.md` | Integração | Worker precisa do mesmo .env, inclusive JWT_SECRET | CODIGO | `src/config/env.ts` |
| FDD-AUT-01 | `docs/FDD.md` | Decisão do autor | Evento em retry segura os seguintes do mesmo pedido | TRANSCRICAO | [09:13] Larissa |
| FDD-AUT-02 | `docs/FDD.md` | Decisão do autor | Secret em texto na tabela, levada à revisão de Sofia | TRANSCRICAO | [09:21] Bruno |
| FDD-ND-01 | `docs/FDD.md` | Não definido | Evento com vários webhooks interessados | TRANSCRICAO | [09:44] Sofia |
| FDD-ND-02 | `docs/FDD.md` | Não definido | Assinatura nas 24 horas de validade paralela | TRANSCRICAO | [09:21] Sofia |
| FDD-ND-03 | `docs/FDD.md` | Não definido | Codificação de X-Signature | TRANSCRICAO | [09:20] Sofia |
| FDD-ND-04 | `docs/FDD.md` | Não definido | Rota de rotação de secret | TRANSCRICAO | [09:21] Sofia |
| FDD-ND-05 | `docs/FDD.md` | Não definido | Status entre tentativas e papel de falhou | TRANSCRICAO | [09:08] Diego |
| FDD-ND-06 | `docs/FDD.md` | Não definido | Respostas HTTP que contam como sucesso | TRANSCRICAO | [09:42] Diego |
| FDD-ND-07 | `docs/FDD.md` | Não definido | Tamanho do batch | TRANSCRICAO | [09:08] Diego |
| FDD-ND-08 | `docs/FDD.md` | Não definido | Eventos em processando se o worker cair | TRANSCRICAO | [09:11] Diego |
| FDD-ND-09 | `docs/FDD.md` | Não definido | Unidade dos 64KB, momento da verificação e destino | TRANSCRICAO | [09:24] Larissa |
| FDD-ND-10 | `docs/FDD.md` | Não definido | Contagem, nova falha e entrada anterior no replay | TRANSCRICAO | [09:18] Diego |
| FDD-ND-11 | `docs/FDD.md` | Não definido | Como um ADMIN obtém o id da DLQ | TRANSCRICAO | [09:18] Bruno |
| FDD-ND-12 | `docs/FDD.md` | Não definido | Mudança e efeito do estado ativo | TRANSCRICAO | [09:21] Bruno |
| FDD-ND-13 | `docs/FDD.md` | Não definido | Efeito de remover webhook ou customer | TRANSCRICAO | [09:33] Bruno |
| FDD-ND-14 | `docs/FDD.md` | Não definido | Retenção do histórico e limite de entregas | TRANSCRICAO | [09:34] Marcos |
| FDD-ND-15 | `docs/FDD.md` | Não definido | Evento quando o pedido é removido | CODIGO | `src/modules/orders/order.service.ts` |
| FDD-ND-16 | `docs/FDD.md` | Não definido | customer_id no body ou no path | TRANSCRICAO | [09:32] Larissa |
| FDD-ND-17 | `docs/FDD.md` | Não definido | Como emitir WEBHOOK_INVALID_URL | TRANSCRICAO | [09:28] Bruno |
| FDD-ND-18 | `docs/FDD.md` | Não definido | Secret nos logs | CODIGO | `src/shared/logger/index.ts` |
| FDD-ND-19 | `docs/FDD.md` | Não definido | Biblioteca para HTTP de saída e assinatura | CODIGO | `package.json` |
| FDD-ND-20 | `docs/FDD.md` | Não definido | Segunda instância do worker por engano | TRANSCRICAO | [09:12] Diego |
| FDD-ND-21 | `docs/FDD.md` | Não definido | Painéis e alertas | TRANSCRICAO | [09:39] Diego |
| FDD-ND-22 | `docs/FDD.md` | Não definido | Limite de customer por usuário no CRUD | TRANSCRICAO | [09:32] Bruno |
| PRD-CTX-01 | `docs/PRD.md` | Contexto | Notificação automática substitui consultas repetidas | TRANSCRICAO | [09:00] Marcos |
| PRD-CTX-02 | `docs/PRD.md` | Contexto | Implantação no sistema existente, como módulo igual aos outros | TRANSCRICAO | [09:30] Larissa |
| PRD-CTX-03 | `docs/PRD.md` | Restrição | Somente saída e gestão por API | TRANSCRICAO | [09:02] Marcos; [09:40] Larissa |
| PRD-PROB-01 | `docs/PRD.md` | Problema | Pedido formal de três clientes | TRANSCRICAO | [09:00] Marcos |
| PRD-PROB-02 | `docs/PRD.md` | Problema | Polling deixa a integração lenta e cara | TRANSCRICAO | [09:00] Marcos |
| PRD-PROB-03 | `docs/PRD.md` | Problema | Atlas sugere migrar para concorrente | TRANSCRICAO | [09:00] Marcos |
| PRD-PUB-01 | `docs/PRD.md` | Público-alvo | Clientes B2B, começando pelos três solicitantes | TRANSCRICAO | [09:00] Marcos |
| PRD-PUB-02 | `docs/PRD.md` | Público-alvo | Usuários que representam o cliente | TRANSCRICAO | [09:32] Marcos |
| PRD-PUB-03 | `docs/PRD.md` | Público-alvo | Administradores que fazem replay | TRANSCRICAO | [09:36] Sofia |
| PRD-CEN-01 | `docs/PRD.md` | Cenário | Receber só SHIPPED e DELIVERED | TRANSCRICAO | [09:33] Marcos |
| PRD-CEN-02 | `docs/PRD.md` | Cenário | Manutenção de duas horas | TRANSCRICAO | [09:16] Diego |
| PRD-CEN-03 | `docs/PRD.md` | Cenário | Evento recebido duas vezes | TRANSCRICAO | [09:25] Diego |
| PRD-CEN-04 | `docs/PRD.md` | Cenário | Troca de secret após vazamento | TRANSCRICAO | [09:21] Sofia; [09:22] Diego |
| PRD-CEN-05 | `docs/PRD.md` | Cenário | Consulta às últimas entregas | TRANSCRICAO | [09:34] Marcos |
| PRD-CEN-06 | `docs/PRD.md` | Cenário | Replay após falha permanente | TRANSCRICAO | [09:18] Diego |
| PRD-OBJ-01 | `docs/PRD.md` | Métrica | Menos de 10 segundos até a primeira tentativa | TRANSCRICAO | [09:02] Marcos |
| PRD-OBJ-02 | `docs/PRD.md` | Métrica | Zero mudanças confirmadas sem evento | TRANSCRICAO | [09:40] Bruno |
| PRD-OBJ-03 | `docs/PRD.md` | Métrica | Janela de 14h36 de novas tentativas | TRANSCRICAO | [09:17] Diego |
| PRD-OBJ-05 | `docs/PRD.md` | Métrica | Estimativa de três sprints até a entrega | TRANSCRICAO | [09:47] Larissa |
| PRD-PRAZO-01 | `docs/PRD.md` | Restrição | Estimativa do time: três sprints | TRANSCRICAO | [09:47] Larissa |
| PRD-PRAZO-02 | `docs/PRD.md` | Restrição | Pedido da Atlas: fim do trimestre e depois fim de novembro | TRANSCRICAO | [09:00] Marcos; [09:45] Marcos |
| PRD-PRAZO-03 | `docs/PRD.md` | Questão em aberto | Nenhum prazo confirmado; Marcos ficou de confirmar | TRANSCRICAO | [09:47] Marcos |
| PRD-ESC-01 | `docs/PRD.md` | Escopo | CRUD de webhooks | TRANSCRICAO | [09:33] Bruno |
| PRD-ESC-02 | `docs/PRD.md` | Escopo | Filtro por status | TRANSCRICAO | [09:33] Marcos |
| PRD-ESC-03 | `docs/PRD.md` | Escopo | Notificação assinada e novas tentativas | TRANSCRICAO | [09:48] Larissa |
| PRD-ESC-04 | `docs/PRD.md` | Escopo | Rotação de secret | TRANSCRICAO | [09:21] Sofia |
| PRD-ESC-05 | `docs/PRD.md` | Escopo | Histórico de entregas | TRANSCRICAO | [09:34] Marcos |
| PRD-ESC-06 | `docs/PRD.md` | Escopo | Reprocessamento manual | TRANSCRICAO | [09:35] Larissa |
| PRD-FORA-01 | `docs/PRD.md` | Fora de escopo | E-mail de aviso | TRANSCRICAO | [09:37] Larissa |
| PRD-FORA-02 | `docs/PRD.md` | Fora de escopo | Dashboard visual | TRANSCRICAO | [09:40] Larissa |
| PRD-FORA-03 | `docs/PRD.md` | Fora de escopo | Rate limiting de envio | TRANSCRICAO | [09:39] Larissa |
| PRD-FORA-04 | `docs/PRD.md` | Fora de escopo | Webhooks de entrada | TRANSCRICAO | [09:02] Marcos |
| PRD-FORA-05 | `docs/PRD.md` | Fora de escopo | Arquivamento de entregues | TRANSCRICAO | [09:08] Diego |
| PRD-FORA-06 | `docs/PRD.md` | Fora de escopo | Ordem global e vários workers | TRANSCRICAO | [09:13] Larissa |
| PRD-FORA-07 | `docs/PRD.md` | Fora de escopo | Exactly-once | TRANSCRICAO | [09:25] Diego |
| PRD-FR-01 | `docs/PRD.md` | Requisito Funcional | Cadastrar webhook | TRANSCRICAO | [09:31] Marcos |
| PRD-FR-02 | `docs/PRD.md` | Requisito Funcional | Editar webhook | TRANSCRICAO | [09:33] Bruno |
| PRD-FR-03 | `docs/PRD.md` | Requisito Funcional | Remover webhook | TRANSCRICAO | [09:33] Bruno |
| PRD-FR-04 | `docs/PRD.md` | Requisito Funcional | Listar webhooks de um customer | TRANSCRICAO | [09:33] Bruno |
| PRD-FR-05 | `docs/PRD.md` | Requisito Funcional | Filtrar notificações por status | TRANSCRICAO | [09:33] Marcos |
| PRD-FR-06 | `docs/PRD.md` | Requisito Funcional | Notificar mudança de status | TRANSCRICAO | [09:00] Marcos |
| PRD-FR-07 | `docs/PRD.md` | Requisito Funcional | Assinar notificações | TRANSCRICAO | [09:20] Sofia |
| PRD-FR-08 | `docs/PRD.md` | Requisito Funcional | Rotacionar secret | TRANSCRICAO | [09:21] Sofia |
| PRD-FR-09 | `docs/PRD.md` | Requisito Funcional | Tentar novamente em caso de falha | TRANSCRICAO | [09:17] Larissa |
| PRD-FR-10 | `docs/PRD.md` | Requisito Funcional | Guardar falhas permanentes | TRANSCRICAO | [09:18] Diego |
| PRD-FR-11 | `docs/PRD.md` | Requisito Funcional | Reprocessar manualmente | TRANSCRICAO | [09:18] Diego |
| PRD-FR-12 | `docs/PRD.md` | Requisito Funcional | Consultar histórico de entregas; "últimos 100" citado como exemplo | TRANSCRICAO | [09:34] Marcos |
| PRD-NFR-01 | `docs/PRD.md` | Requisito Não Funcional | Notificação em menos de 10 segundos | TRANSCRICAO | [09:02] Marcos |
| PRD-NFR-02 | `docs/PRD.md` | Requisito Não Funcional | Verificação a cada 2 segundos | TRANSCRICAO | [09:10] Larissa |
| PRD-NFR-03 | `docs/PRD.md` | Requisito Não Funcional | Espera máxima de 10 segundos | TRANSCRICAO | [09:42] Diego |
| PRD-NFR-04 | `docs/PRD.md` | Requisito Não Funcional | Limite de 64KB | TRANSCRICAO | [09:24] Larissa |
| PRD-NFR-05 | `docs/PRD.md` | Requisito Não Funcional | Envio não para quando a API reinicia | TRANSCRICAO | [09:11] Diego |
| PRD-NFR-06 | `docs/PRD.md` | Questão em aberto | Disponibilidade tratada só no reinício da API; sem meta em percentual | TRANSCRICAO | [09:11] Diego |
| PRD-NFR-07 | `docs/PRD.md` | Requisito Não Funcional | https obrigatório | TRANSCRICAO | [09:23] Sofia |
| PRD-NFR-08 | `docs/PRD.md` | Requisito Não Funcional | HMAC-SHA256, secret por endpoint e rotação | TRANSCRICAO | [09:22] Sofia |
| PRD-NFR-09 | `docs/PRD.md` | Requisito Não Funcional | ADMIN no replay e qualquer role autenticada no restante | TRANSCRICAO | [09:36] Larissa; [09:37] Sofia |
| PRD-NFR-10 | `docs/PRD.md` | Requisito Não Funcional | Revisão de segurança antes do deploy | TRANSCRICAO | [09:46] Sofia |
| PRD-NFR-11 | `docs/PRD.md` | Requisito Não Funcional | Logger existente, sem ferramenta nova | TRANSCRICAO | [09:29] Bruno |
| PRD-NFR-12 | `docs/PRD.md` | Requisito Não Funcional | Registro de quem executou o replay | TRANSCRICAO | [09:36] Sofia |
| PRD-NFR-13 | `docs/PRD.md` | Questão em aberto | Reuso máximo definido; sem metas de métricas e tracing; FDD trata do tema por exigência do enunciado | TRANSCRICAO | [09:30] Larissa |
| PRD-NFR-14 | `docs/PRD.md` | Requisito Não Funcional | Evento e mudança de status são atômicos | TRANSCRICAO | [09:06] Diego |
| PRD-NFR-15 | `docs/PRD.md` | Requisito Não Funcional | At-least-once com identificador único | TRANSCRICAO | [09:26] Larissa |
| PRD-NFR-16 | `docs/PRD.md` | Requisito Não Funcional | Evento reflete o momento da mudança | TRANSCRICAO | [09:52] Larissa |
| PRD-NFR-17 | `docs/PRD.md` | Requisito Não Funcional | Segue os padrões atuais do projeto | TRANSCRICAO | [09:30] Larissa |
| PRD-NFR-18 | `docs/PRD.md` | Requisito Não Funcional | Sem nova infraestrutura | TRANSCRICAO | [09:07] Diego |
| PRD-NFR-19 | `docs/PRD.md` | Restrição | Sem interface visual nesta fase; compliance não tratado | TRANSCRICAO | [09:40] Larissa |
| PRD-DEC-01 | `docs/PRD.md` | Decisão | Outbox na própria transação | TRANSCRICAO | [09:06] Diego |
| PRD-DEC-02 | `docs/PRD.md` | Decisão | Worker separado com verificação a cada 2 segundos | TRANSCRICAO | [09:10] Larissa; [09:11] Diego |
| PRD-DEC-03 | `docs/PRD.md` | Decisão | 5 tentativas e reprocessamento manual | TRANSCRICAO | [09:17] Larissa; [09:18] Diego |
| PRD-DEC-04 | `docs/PRD.md` | Decisão | HMAC-SHA256 e secret por endpoint | TRANSCRICAO | [09:22] Sofia |
| PRD-DEC-05 | `docs/PRD.md` | Decisão | Entrega pelo menos uma vez | TRANSCRICAO | [09:26] Larissa |
| PRD-DEC-06 | `docs/PRD.md` | Decisão | Reuso de padrões | TRANSCRICAO | [09:30] Larissa |
| PRD-DEC-07 | `docs/PRD.md` | Decisão | Filtro e snapshot no registro do evento | TRANSCRICAO | [09:34] Bruno; [09:52] Larissa |
| PRD-DEP-01 | `docs/PRD.md` | Dependência | Revisão de segurança de pelo menos dois dias úteis | TRANSCRICAO | [09:46] Sofia |
| PRD-DEP-02 | `docs/PRD.md` | Dependência | Documentação no portal | TRANSCRICAO | [09:26] Marcos |
| PRD-DEP-03 | `docs/PRD.md` | Dependência | Confirmação do prazo com a Atlas | TRANSCRICAO | [09:47] Marcos |
| PRD-DEP-04 | `docs/PRD.md` | Dependência | Cliente com https, verificação e deduplicação | TRANSCRICAO | [09:23] Sofia; [09:20] Sofia; [09:24] Diego |
| PRD-DEP-05 | `docs/PRD.md` | Dependência | Sessão de revisão do design | TRANSCRICAO | [09:50] Larissa |
| PRD-RISK-01 | `docs/PRD.md` | Risco | Perda de cliente por atraso | TRANSCRICAO | [09:00] Marcos; [09:45] Marcos |
| PRD-RISK-02 | `docs/PRD.md` | Risco | Cliente trata mal duplicatas | TRANSCRICAO | [09:25] Sofia |
| PRD-RISK-03 | `docs/PRD.md` | Risco | Volume alto de notificações | TRANSCRICAO | [09:38] Diego |
| PRD-RISK-04 | `docs/PRD.md` | Risco | Cliente fora além da janela | TRANSCRICAO | [09:17] Marcos |
| PRD-RISK-05 | `docs/PRD.md` | Risco | Pessoas sem a autorização esperada podem administrar webhooks ou reprocessar entregas | CODIGO | `src/modules/auth/auth.schemas.ts` |
| PRD-AC-01 | `docs/PRD.md` | Critério de Aceite | CRUD por usuário autenticado | TRANSCRICAO | [09:33] Bruno |
| PRD-AC-02 | `docs/PRD.md` | Critério de Aceite | Secret gerada e devolvida na criação | TRANSCRICAO | [09:31] Marcos |
| PRD-AC-03 | `docs/PRD.md` | Critério de Aceite | http recusado | TRANSCRICAO | [09:23] Sofia |
| PRD-AC-04 | `docs/PRD.md` | Critério de Aceite | Só status escolhidos são notificados | TRANSCRICAO | [09:33] Marcos |
| PRD-AC-05 | `docs/PRD.md` | Critério de Aceite | Primeira tentativa em menos de 10 segundos | TRANSCRICAO | [09:02] Marcos |
| PRD-AC-06 | `docs/PRD.md` | Critério de Aceite | Falha no registro mantém o status | TRANSCRICAO | [09:40] Bruno |
| PRD-AC-07 | `docs/PRD.md` | Critério de Aceite | Campos e headers definidos, sem items | TRANSCRICAO | [09:43] Diego; [09:44] Diego; [09:44] Sofia |
| PRD-AC-08 | `docs/PRD.md` | Critério de Aceite | Assinatura validável | TRANSCRICAO | [09:20] Sofia |
| PRD-AC-09 | `docs/PRD.md` | Critério de Aceite | Duas secrets por 24 horas | TRANSCRICAO | [09:21] Sofia |
| PRD-AC-10 | `docs/PRD.md` | Critério de Aceite | Retry nos intervalos definidos | TRANSCRICAO | [09:17] Larissa |
| PRD-AC-11 | `docs/PRD.md` | Critério de Aceite | Evento guardado com conteúdo, motivo e momento | TRANSCRICAO | [09:18] Diego |
| PRD-AC-12 | `docs/PRD.md` | Critério de Aceite | Replay só por ADMIN, com autoria em log | TRANSCRICAO | [09:36] Sofia |
| PRD-AC-13 | `docs/PRD.md` | Critério de Aceite | Histórico com resultado, payload, resposta e tempo | TRANSCRICAO | [09:34] Marcos |
| PRD-AC-14 | `docs/PRD.md` | Critério de Aceite | Revisão de segurança concluída antes do deploy | TRANSCRICAO | [09:49] Sofia |
| PRD-TEST-01 | `docs/PRD.md` | Teste | Testes ponta a ponta da integração | TRANSCRICAO | [09:46] Larissa |
| PRD-TEST-02 | `docs/PRD.md` | Derivado | Testes de integração com a stack atual; a reunião só citou ponta a ponta | CODIGO | `tests/orders.test.ts` |
| PRD-TEST-03 | `docs/PRD.md` | Teste | Revisão de segurança de HMAC e secret | TRANSCRICAO | [09:46] Sofia |
| PRD-TEST-04 | `docs/PRD.md` | Teste | Checklist técnica do FDD e revisão do design | TRANSCRICAO | [09:50] Larissa |
| PRD-TEST-05 | `docs/PRD.md` | Teste | Medir impacto para decidir e-mail e rate limiting | TRANSCRICAO | [09:37] Larissa |
