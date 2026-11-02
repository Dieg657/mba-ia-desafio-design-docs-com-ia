# Da Reunião ao Documento: Design Docs Gerados por IA

Pacote de design docs do Sistema de Webhooks de Notificação de Pedidos, produzido com IA a partir de uma transcrição de reunião e do código de um Order Management System.

O enunciado original está na [seção recolhida no fim deste documento](#enunciado-original) e no [repositório base do desafio](https://github.com/devfullcycle/mba-ia-desafio-design-docs-com-ia).

## Sobre o desafio

Uma empresa com um Order Management System em produção decidiu, em uma reunião técnica, construir webhooks de notificação para clientes B2B, mas só a transcrição da call ficou registrada. A tarefa foi transformar essa transcrição e o código existente em uma documentação que permita ao time começar a implementar: PRD, RFC, FDD, ADRs e um tracker de rastreabilidade.

A entrega é só documental, e a regra central é não inventar nada: cada requisito, decisão ou restrição precisa ter origem em uma fala da reunião, em um arquivo do código ou em uma exigência do enunciado. As escolhas minhas em pontos que a reunião não fechou ficam marcadas como `[Decisão do autor]`, pendentes de confirmação dos revisores. A IA produziu o conteúdo, enquanto eu defini o que fazer, revisei e direcionei as correções.

## Ferramentas de IA utilizadas

- **Claude Code (Claude Opus 5.5):** ferramenta principal. Leu enunciado, templates, transcrição e código, escreveu os documentos e executou as verificações.
- **Prompts de PRD, FDD e HLD do curso:** usados como base de formato. Como são prompts de entrevista, a transcrição e o código fizeram o papel das respostas. O HLD alimentou a proposta técnica do RFC.
- **Plugin `adrs-management` do curso:** lido como referência para o formato MADR e os filtros de decisão. Os plugins não foram executados, e o template de ADR foi adaptado, como descrito abaixo.
- **Scripts de verificação em Python:** criados durante o processo para conferir citações, caminhos, exemplos, IDs e changelog, sem alterar o código da aplicação.

## Workflow adotado

O trabalho teve duas fases: gerar o pacote e, depois, resolver os pontos em aberto.

**Fase 1: geração**

1. **Entendimento do enunciado:** antes de produzir, pedi à IA que explicasse o que precisava ser entregue.
2. **Contexto dos templates:** a IA leu os materiais do curso e apontou onde eles conflitavam com o enunciado.
3. **Análise da transcrição:** uma leitura exploratória separou decisões, alternativas, adiamentos e lacunas, sem interpretação além do que foi dito.
4. **Conferência no código:** o que a reunião afirmou sobre o código foi confirmado nos arquivos antes da escrita.
5. **Geração na ordem do enunciado:** ADRs, RFC, FDD e PRD, atualizando `docs/TRACKER.md` ao concluir cada documento.
6. **Verificação e revisão:** um script conferiu citações, arquivos e IDs, e uma revisão manual procurou o que o script não detecta, como uma citação que não sustenta a frase.

**Fase 2: resolução dos pontos em aberto**

Em uma branch própria, com o pacote da fase 1 como linha de base, o trabalho foi dividido em 5 iterações. A ordem saiu de uma medição de quantas referências cada documento recebia dos demais: primeiro o que afeta menos documentos ao mudar.

| Iteração | Documento | Referências recebidas |
| --- | --- | --- |
| 1 | PRD e estrutura de changelog | 1 |
| 2 | FDD | 7 |
| 3 | RFC | 9 |
| 4 | ADRs, do menos para o mais referenciado | 10 a 17 cada |
| 5 | README e fechamento | 0, mas descreve todos os demais |

Cada iteração alterou só os documentos dela e o tracker; o que precisava mudar em outro documento virou uma pendência. A exceção foi a iteração 5, que também alterou o PRD e o FDD para fechar o pacote (CHG-031, CHG-032). Cada uma terminou em uma revisão minha, e só foi commitada depois do meu OK explícito. O tracker e os changelogs cobrem PRD, RFC, FDD e ADRs.

Depois das iterações, uma revisão extensa de citações (CHG-036 a CHG-040) reabriu a `RFC-QA-12`, e ajustes no tracker e nos links fecharam o pacote (CHG-034, CHG-035, CHG-041 a CHG-052). O histórico da branch foi então unificado (CHG-053), por isso o log mostra o pacote em um único commit, sem um commit por iteração.

**Marcadores de conteúdo**

- **Sem marcador:** acordado na reunião ou existente no código.
- **`[Derivado]`:** decorre do código ou de uma decisão fechada, ou é exigido pelo enunciado, sem propor comportamento novo.
- **`[Decisão do autor]`:** escolha minha em ponto que a reunião não fechou, pendente de confirmação dos revisores.
- **`[Não definido]`:** sem resposta nas fontes, registrado sem sugestão. O FDD reúne esses pontos na seção 14, com IDs `FDD-ND`, e os ADRs usam "Não definido na reunião".
- **`análise:`:** conclusão que ninguém afirmou na reunião, usada nos ADRs, com a indicação de em que se apoia.

## Prompts customizados

**Análise exploratória da transcrição.** Escrito para evitar que a IA completasse lacunas por conta própria:

```text
Quero que você analise agora o @TRANSCRICAO.md de forma exploratória para entender o
conteúdo da reunião, objeto em discussão, e as decisões tomadas. Esqueça as frivolidades e
conteúdo não relevante da transcrição e foque no objetivo da discussão. Nunca faça
pré-julgamentos e deduções, trabalhe sempre no que é/foi discutido e também tenha o cuidado
de não extrapolar/superestimar/subestimar o conteúdo discutido da transcrição.
```

**Persona para o template de ADR.** Forneci à IA os documentos base das aulas do curso e o agente `adr-generator`. A geração dos ADRs usou a persona e as regras iniciais de origem, adaptadas do agente. Esta é a versão revisada do prompt, no formato Skeleton of Thought (primeiro o esqueleto, depois a expansão de cada item), montada depois dos ADRs prontos: o esqueleto e os limites vêm do padrão comum aos 8 ADRs, e os demais ajustes, de testes com o prompt:

```text
Use como base os documentos das aulas do curso e o agente adr-generator, adaptando-os a este
desafio. Se esse material não trouxer insumo suficiente, use o template de prompt a seguir,
escrito na persona de um Sênior Software Engineer com expertise em levantamento de requisitos
funcionais e análise arquitetural, e itere sobre ele até que atenda às decisões da reunião:

Você é um Sênior Software Engineer com expertise em levantamento de requisitos funcionais e
análise arquitetural. Você registra decisões que já foram tomadas: não decide de novo, não
melhora a decisão e não preenche lacunas com boas práticas de mercado.

Entrada: a decisão a registrar, @TRANSCRICAO.md e o código do repositório.

Regras:
1. Toda afirmação sobre decisão, motivo, alternativa ou restrição precisa de origem:
   [hh:mm] Nome ou caminho de arquivo real. Prós e Contras descrevem a própria alternativa: citam
   a fala se ela existir e, sem fala, dispensam origem e "análise:".
2. Copie o timestamp e o falante da fala exata, depois de conferir na transcrição.
3. Alternativa não discutida só entra se for plausível, marcada como
   (plausível, não discutida), e nunca como motivo atribuído aos participantes.
4. Não use [NEEDS INPUT]. Se faltar informação, escreva "Não definido na reunião" no ADR; o que
   exceder o limite de pontos vai para o FDD.
5. Conclusão que ninguém afirmou na reunião começa com "análise:" e indica em que se apoia.
6. Registre só o que sustenta esta decisão. Efeito ou limite vindo de outra decisão fica no ADR
   dela e aparece aqui apenas em Relacionados. O que a reunião fechou sobre esta decisão fica na
   Decisão, inclusive valores, rotas e permissões.

Etapa 1, esqueleto. Preencha cada item em até 10 palavras, sem contar a origem e sem redigir o
texto:
- Título: verbo no infinitivo seguido da decisão.
- Status: Aceito; fala que fechou cada parte da decisão (quem encerra o ponto; sem fechamento
  explícito, quem propôs); fala que a reafirmou, se houver; 2 ou 3 ADRs de dependência direta.
- Contexto: problema que a reunião precisava resolver; até 4 direcionadores: fatos e avaliações
  que motivaram a decisão, mesmo que reapareçam nas Consequências, e não a própria decisão.
- Decisão: o que foi fechado; leitura adotada só se falas sobre esta mesma decisão divergirem. A
  leitura cita as falas que divergem, escolhe entre elas sem criar comportamento novo, começa com
  "análise:" e conta no limite de análises.
- Alternativas: nome e origem de cada uma, com pelo menos 1.
- Consequências: até 4 positivas, com o que a decisão entrega frente aos direcionadores; até 3
  negativas, incluindo custo de construir ou operar e dependência de outra pessoa ou de revisão,
  com a fala que os registra; até 3 análises no ADR inteiro; até 2 pontos não definidos, só se a
  resposta não decorrer da própria decisão, com os demais para o FDD; trade-off que resume custos
  e benefícios já registrados, começa com "aceitamos", cita as falas que os sustentam e não leva
  "análise:".
- Referências: as 3 ou 4 falas que definem a decisão, fecham cada parte dela ou sustentam a
  principal negativa, sem a de reafirmação e sem repetir horário e falante; arquivos existentes em
  que ela se apoia, com pelo menos 1.
Confira cada origem na fonte e retire o item que ela não sustentar. Não complete item para
atingir uma quantidade.

Etapa 2, expansão. Expanda um item por vez, sem acrescentar item novo, neste formato:

# ADR-NNN: <título>

## Status
Aceito
- **Decidido em:** reunião técnica, [hh:mm] Nome (parte), uma fala por parte fechada; reafirmado
  em [hh:mm] Nome, se houver
- **Relacionados:** link para cada ADR escolhido, com a relação em até 3 palavras entre parênteses

## Contexto
<problema, com a origem>

**Direcionadores**
- **ADR-NNN-CTX-01:** <o que pesou na decisão> ([hh:mm] Nome).

## Decisão
<o que foi fechado, com a origem>

**ADR-NNN-INT-01:** <leitura adotada e motivo; omita se não houver>

## Alternativas Consideradas
### ADR-NNN-ALT-01: <nome>
- **Origem:** discutida em [hh:mm] Nome, ou (plausível, não discutida)
- **Prós:** <vantagem da alternativa>
- **Contras:** <desvantagem da alternativa>
- **Por que foi descartada:** <motivo> ([hh:mm] Nome).

## Consequências
### Positivas
- **ADR-NNN-CON-01:** <efeito> ([hh:mm] Nome).

### Negativas
- **ADR-NNN-CON-02:** <custo ou limite> ([hh:mm] Nome).

**Não definido na reunião:** <até 2 pontos em um único parágrafo, com o ID no FDD e no RFC, se
houver; omita se não houver>

### Trade-off aceito
**ADR-NNN-TO:** aceitamos <custo> em troca de <benefício> ([hh:mm] Nome).

## Referências
**Transcrição**
- [hh:mm] Nome: <o que a fala sustenta>.

**Código**
- **ADR-NNN-COD-01:** `<caminho>`: <papel do arquivo na decisão>.

## Changelog
Detalhes e IDs afetados na seção Changelog de [TRACKER.md](../TRACKER.md).

| CHG | Resumo |
| --- | --- |
| CHG-000 | Versão inicial, linha de base do pacote |
```

**Ordem de produção e papel do tracker:**

```text
Depois das ADR's iremos prosseguir com a geração dos documentos na ordem descrita pelo README, e também não muito menos importante, mantenha sempre o
@docs/TRACKER.md atualizado, pois é justamente seu documento orientador junto do README do
trabalho à ser feito e/ou atualmente realizado.
```

**Versão consolidada do prompt de iterações.** Reúne a ordem medida, as regras de conteúdo e o registro obrigatório, definidos ao longo do trabalho:

```text
Vamos resolver os pontos em aberto do pacote em 5 iterações, na branch
feat/add-arch-tech-design-docs-with-ai, com o commit atual como linha de base.

A ordem segue o acoplamento: quanto mais referências um documento recebe, mais tarde ele muda.

1. PRD: só o README aponta para ele. Crie aqui as seções Changelog e Pendências do tracker.
2. FDD: feche os pontos de implementação que puderem ser fechados com as fontes.
3. RFC: reflita o resultado do FDD e separe as questões em adiadas, aguardando revisores e
   fechadas.
4. ADRs: revise nesta ordem: 003, 005, 007, 008, 001, 006, 002, 004.
5. README e fechamento: resolva as pendências restantes e refaça a checklist do enunciado.

Regras de conteúdo:
- Mantenha o que foi acordado na reunião ou existe no código, com a origem indicada.
- Marque como [Derivado] o que decorre do código ou de uma decisão fechada, ou o que o enunciado
  exige.
- Registre o restante como "Não definido na reunião", sem sugerir resposta.
- Abra a fala de cada [hh:mm] Nome e confirme que ela sustenta a frase em que foi citada.

Regras de cada iteração:
- Altere somente os documentos da iteração e o docs/TRACKER.md.
- Se outro documento precisar mudar, registre o motivo em "## Pendências" no tracker.
- Não renomeie arquivos nem reutilize IDs.

Registro obrigatório de alterações:
- Toda alteração, inclusive de citação, recebe um CHG-NNN na seção "## Changelog" do tracker,
  com documento, IDs afetados e motivo.
- O mesmo CHG aparece na seção "## Changelog" do documento alterado, com link para o tracker.
- A matriz de rastreabilidade acompanha cada mudança.

Ao final de cada iteração, rode as verificações, me mostre o que mudou e o que continua aberto,
e pare. Só faça o commit depois do meu OK explícito.
```

## Iterações e ajustes

**Fase 1: geração**

1. **`adr-generator` insuficiente sozinho.** O agente extrai decisões de código e histórico git, grava em outra estrutura de pastas e aceita marcadores `[NEEDS INPUT]`. As decisões deste desafio vêm de uma reunião e ainda não existem no código, e esse material não bastava. Por isso, usamos o formato dele em conjunto com o nosso prompt personalizado, que traz a persona e exige origem para cada afirmação.
2. **Valores padrão dos templates.** O prompt de PRD sugere hipóteses como 99.9% de disponibilidade e p95 de 150 ms. Como a reunião não tratou disso, o PRD registra a ausência da meta em vez de adotar esses números.
3. **Afirmações da reunião que o código refinou.**
   - O débito de estoque não ocorre em toda mudança de status, só de PENDING para PAID.
   - As rotas ficam sob `/api/v1`, embora a reunião as tenha citado sem prefixo.
   - `NotFoundError` fixa o código `NOT_FOUND`, então `WEBHOOK_NOT_FOUND` precisa de classe própria.
   - `validate` converte toda falha de schema em `VALIDATION_ERROR`, então uma URL `http` não responde `WEBHOOK_INVALID_URL` com o middleware atual.
4. **Riscos encontrados só no código.** `POST /auth/register` é público e aceita `role: ADMIN`, e não há vínculo entre usuário e customer. Os dois pontos viraram riscos, porque afetam a proteção combinada para o replay e o CRUD.
5. **Ambiguidade de "5 tentativas".** A frase sobre "quase 15 horas" ([09:17] Diego) só fecha com um envio inicial mais 5 reenvios. A leitura ficou registrada como interpretação no ADR-004 e no RFC.
6. **Erros corrigidos na verificação e na revisão.**
   - O script apontou um falante inexistente no ADR-008, causado por um trecho de texto lido como citação.
   - A revisão manual removeu do PRD uma menção a "retorno dos três clientes", que a reunião não fez.
   - A mesma revisão acrescentou exemplos de requisição aos endpoints do FDD que só tinham resposta.

**Fase 2: resolução dos pontos em aberto**

7. **Conteúdo sem origem que a primeira revisão deixou passar (CHG-002, CHG-003, CHG-005).** O PRD tinha a meta "3 de 3 clientes", que ninguém declarou, e tratava o prazo sem separar estimativa, pedido do cliente e confirmação, que ainda não existe. Duas citações também não sustentavam a frase em que estavam.
8. **A mudança mais importante: o FDD deixou de sugerir (CHG-009 a CHG-016).** A primeira versão tinha 38 linhas com itens marcados como proposta, como lote de 10, regra de 2xx, formato da assinatura e cascatas. Quando a IA me pediu para escolher entre as opções, respondi que deveríamos nos ater ao que foi acordado. Isso entrava em conflito com o enunciado, que exige status codes, métricas e tracing que a reunião não detalhou. A solução veio do próprio enunciado: tudo precisa ser rastreável, então fica o acordado e o derivado do código ou exigido pelo enunciado, e o resto vira um ponto não definido, salvo as escolhas marcadas como `[Decisão do autor]`, que aguardam os revisores. O custo é um FDD menos pronto para codar: o caso de um customer com vários webhooks depende da revisão do design.
9. **Citações que pareciam certas (CHG-012, CHG-014, CHG-024).** O validador confirma que a fala existe, não que ela diz aquilo. A auditoria encontrou uma alternativa atribuída a Bruno, que falava apenas do logger, e o vazamento em log de um cliente usado como motivo para mudar o nosso logger.
10. **Questões em aberto reorganizadas (CHG-018, CHG-019).** O RFC separou adiadas, aguardando revisores e fechadas. Duas foram fechadas com base nas fontes, e o arquivamento saiu das questões, porque a reunião o colocou fora do escopo.
11. **Teste de uma afirmação técnica (CHG-015).** A montagem da rota aninhada foi testada em Express 4.21.1: sem `mergeParams`, o `customerId` chega indefinido, e registrada depois de `/customers`, ela passa duas vezes pela autenticação.

## Como navegar a entrega

| Ordem | Arquivo | O que responde |
| --- | --- | --- |
| 1 | [`docs/PRD.md`](docs/PRD.md) | Por que e o quê: problema, escopo e métricas |
| 2 | [`docs/RFC.md`](docs/RFC.md) | Como pretendemos resolver e o que está em aberto |
| 3 | [`docs/adrs/`](docs/adrs/README.md) | Por que cada decisão foi tomada assim |
| 4 | [`docs/FDD.md`](docs/FDD.md) | Como construir, em detalhe, e o que ainda não foi definido |
| 5 | [`docs/TRACKER.md`](docs/TRACKER.md) | De onde veio cada item e o que mudou em cada iteração |

A transcrição original está em [`TRANSCRICAO.md`](TRANSCRICAO.md). O código em `src/`, `prisma/` e `tests/` não foi alterado.

## Enunciado original

<details>
<summary>Ver o enunciado completo do desafio</summary>

# Da Reunião ao Documento: Design Docs Gerados por IA

## Descrição

Neste desafio você vai transformar a transcrição de uma reunião técnica em um pacote completo de design docs, usando IA como ferramenta principal de produção.

**Cenário:** uma empresa que opera um Order Management System (OMS) em produção vai construir uma nova feature, um Sistema de Webhooks de Notificação de Pedidos. A decisão técnica já foi tomada em uma reunião entre tech lead, PM, engenheiros e segurança, mas nada foi registrado além da transcrição da call (`TRANSCRICAO.md`).

**Sua tarefa:** produzir, a partir da transcrição e do código existente, a documentação técnica da feature, em nível acionável o suficiente para o time de engenharia iniciar a implementação.

## Sobre o uso de IA

A IA é sua ferramenta principal de produção neste desafio. Você vai usá-la para ler o código, analisar a transcrição, estruturar os documentos e gerar o conteúdo final. O que se espera de você é o papel de maestro: definir o que precisa ser feito, formular bons prompts, revisar criticamente o que a IA entrega, corrigir e refinar até o resultado ficar consistente.

## Estrutura do desafio

O desafio consiste em produzir um **pacote de design docs**: PRD, RFC, FDD, ADRs, Tracker e o README do processo a partir da transcrição e do código.

## Objetivo

Entregar, em um repositório público no GitHub (fork do repositório base), o seguinte pacote de documentação:

- PRD (Product Requirement Document) da feature
- RFC (Request for Comments) com a proposta técnica da solução, submetida à equipe para revisão
- FDD (Feature Design Document) da feature
- Entre 5 e 8 ADRs (Architecture Decision Records) das decisões discutidas
- Tracker de rastreabilidade ligando cada item à origem na transcrição ou no código
- README atualizado documentando o processo de produção

Toda informação registrada nos documentos deve ser rastreável à transcrição ou ao código fonte da aplicação. Não é permitido inventar requisitos, decisões ou restrições sem origem identificável.

### O pacote de documentos e o papel de cada um

Os documentos não se repetem: cada um opera em uma **altura** diferente. Antes de produzir, entenda a fronteira entre eles: conteúdo duplicado entre documentos é sinal de que algo está no lugar errado.

| Documento | Papel | Altura | Pergunta que responde |
| --- | --- | --- | --- |
| **PRD** | Problema, público, escopo e métricas de sucesso | Produto / negócio | *Por que e o quê?* |
| **RFC** | Proposta técnica da solução para revisão: abordagem geral, alternativas e questões em aberto | Arquitetura | *Como pretendemos resolver, e o que ainda está em aberto?* |
| **ADRs** | Cada decisão arquitetural isolada, com contexto e consequências | Decisão pontual | *Por que decidimos exatamente assim?* |
| **FDD** | Especificação de implementação: fluxos, contratos, erros, integração com o código | Implementação | *Como construir, em detalhe?* |
| **Tracker** | Rastreabilidade de cada item ao código ou à transcrição | Transversal | *De onde veio cada coisa?* |

Em uma frase: o **RFC propõe e abre para revisão**, os **ADRs registram cada decisão fechada** e o **FDD detalha como construir**. O RFC é conciso (2 a 4 páginas) e fala em decisão; o FDD é profundo e fala em implementação. Não repita no RFC o nível de detalhe do FDD.

## Contexto

### A aplicação existente

O repositório base contém uma aplicação Node.js + TypeScript funcional: um Order Management System com módulos de autenticação, usuários, clientes, produtos e pedidos. Banco MySQL via Prisma. O ciclo de vida do pedido tem máquina de estados controlada, controle transacional de estoque e auditoria de mudanças de status.

A aplicação não tem nenhum mecanismo de notificação externa, eventos, filas ou webhooks. Esse vácuo é proposital. É exatamente o que a feature discutida na reunião pretende preencher.

Seus documentos vão precisar referenciar componentes do código existente, como a estrutura modular, a máquina de estados, a transação do `changeStatus`, as classes de erro, o padrão de códigos de erro, o middleware `requireRole`, o error middleware centralizado e o logger Pino. Use a IA para mapear esses pontos a partir do código.

### A transcrição

O arquivo `TRANSCRICAO.md` contém a gravação literal da reunião técnica. Cinco participantes discutem por aproximadamente 55 minutos no formato `[hh:mm] Nome: fala`.

A transcrição inclui decisões fechadas, requisitos funcionais explícitos, restrições, ganchos com o código existente, pontos descartados ou adiados para fases futuras e detalhes técnicos secundários. Nem tudo que foi mencionado vira requisito. Algumas coisas foram explicitamente descartadas, outras foram adiadas. Identificar o que NÃO entra é tão importante quanto identificar o que entra. Use a IA com prompts dirigidos para fazer essa filtragem, não pedidos genéricos.

## Tecnologias e ferramentas

Liberdade total na escolha de ferramentas de IA. Você pode usar qualquer combinação de Claude, ChatGPT, Cursor, Copilot Chat, Gemini, agentes, prompts customizados, skills ou plugins. Aproveite os prompts e plugins disponibilizados pelo professor durante o curso como ponto de partida.

Os documentos devem ser entregues em formato Markdown.

A entrega é puramente documental: você não deve mexer no código da aplicação (`src/`, `prisma/`, `tests/`, configurações). O código serve de contexto e referência.

## Requisitos

### 1. PRD da feature

Produza o arquivo `docs/PRD.md` cobrindo a feature de Sistema de Webhooks de Notificação de Pedidos. O PRD deve seguir o formato apresentado no curso e incluir, no mínimo, as seguintes seções:

- Resumo e contexto da feature
- Problema e motivação
- Público-alvo e cenários de uso
- Objetivos e métricas de sucesso
- Escopo (incluso e fora de escopo)
- Requisitos funcionais
- Requisitos não funcionais
- Decisões e trade-offs principais
- Dependências
- Riscos e mitigação
- Critérios de aceitação
- Estratégia de testes e validação

A seção "Fora de escopo" deve listar explicitamente pelo menos 2 itens descartados ou adiados durante a reunião.

### 2. RFC da feature

Produza o arquivo `docs/RFC.md` com a proposta técnica da solução, no formato de um documento submetido à equipe para revisão. O RFC opera em nível de arquitetura: apresenta a abordagem escolhida, as alternativas que foram colocadas na mesa e as questões deixadas em aberto. É um documento conciso (2 a 4 páginas); o detalhamento de implementação fica no FDD. Deve seguir o formato apresentado no curso e incluir, no mínimo:

- Metadados (autor, status, data, revisores); use os participantes da reunião como revisores
- Resumo executivo (TL;DR) da proposta
- Contexto e problema
- Proposta técnica (visão geral da solução, sem descer ao detalhe de implementação do FDD)
- Alternativas consideradas (pelo menos 2 alternativas reais discutidas e descartadas na reunião, cada uma com o trade-off que levou ao descarte)
- Questões em aberto (pelo menos 2 pontos levantados na reunião e não decididos ou adiados)
- Impacto e riscos
- Decisões relacionadas (links para os ADRs correspondentes)

O RFC não deve duplicar o detalhamento do FDD. Ele responde "o que propomos e por quê"; o "como construir" em detalhe fica no FDD.

### 3. FDD da feature

Produza o arquivo `docs/FDD.md` detalhando o "como implementar" da feature. O FDD é o documento mais técnico e precisa estar acionável o suficiente para um desenvolvedor pegar e começar a codar. Deve seguir o formato apresentado no curso e incluir, no mínimo:

- Contexto e motivação técnica
- Objetivos técnicos
- Escopo e exclusões
- Fluxos detalhados (criação do evento na outbox, processamento pelo worker, retry, DLQ)
- Contratos públicos (endpoints HTTP com payloads de exemplo, headers, status codes, semântica)
- Matriz de erros previstos com códigos no padrão `WEBHOOK_*`
- Estratégias de resiliência (timeouts, retries, backoff, fallback)
- Observabilidade (métricas, logs, tracing)
- Dependências e compatibilidade
- Critérios de aceite técnicos
- Riscos e mitigação

Seção obrigatória adicional, específica deste desafio: **"Integração com o sistema existente"**. Esta seção deve nomear pelo menos 4 caminhos de arquivo reais do código base e descrever como o módulo de webhooks vai se integrar com cada um (por exemplo, como o método `changeStatus` será estendido, como as classes de erro existentes serão reutilizadas).

### 4. ADRs

Produza entre 5 e 8 ADRs em arquivos separados dentro de `docs/adrs/`, nomeados no formato `ADR-NNN-titulo-em-kebab-case.md` (ex: `ADR-001-outbox-no-mysql.md`).

Cada ADR deve seguir o formato MADR (ou variante padrão) com no mínimo as seções: Status, Contexto, Decisão, Alternativas Consideradas (pelo menos 1 alternativa real discutida ou plausível), Consequências (positivas e negativas, com trade-off explícito).

Pelo menos 1 ADR deve referenciar explicitamente arquivos, módulos ou padrões do código existente.

O conjunto de ADRs deve cobrir, no mínimo, 5 das 6 decisões principais discutidas na reunião:

- Padrão Outbox no MySQL
- Política de retry com backoff e DLQ
- Autenticação HMAC-SHA256 com secret por endpoint
- Garantia at-least-once com `X-Event-Id`
- Worker em processo separado em polling
- Reuso dos padrões existentes do projeto

Decisões técnicas secundárias (formato de payload, timeouts, headers, entre outras) podem virar ADRs adicionais ou ficar apenas no FDD, conforme você considerar mais adequado.

### 5. Tracker de Rastreabilidade

Produza o arquivo `docs/TRACKER.md`, uma tabela markdown que mapeia cada item registrado nos seus documentos à origem na transcrição ou no código. O tracker funciona como uma referência cruzada: permite que qualquer leitor entenda de onde veio cada decisão, requisito ou restrição, e garante que a documentação está alinhada com o que foi efetivamente discutido e com o que existe no código.

O tracker não é um conceito padrão do mercado nem é um documento abordado diretamente no curso. É uma exigência específica deste desafio que ajuda a manter a integridade da documentação contra alucinações da IA.

Formato obrigatório da tabela:

| ID | Documento | Tipo | Conteúdo (resumo) | Fonte | Localização |
| --- | --- | --- | --- | --- | --- |
|  |  |  |  |  |  |

Onde:

- **ID**: identificador único do item (ex: PRD-FR-01, RFC-ALT-02, FDD-CONTRATO-03, ADR-002)
- **Documento**: arquivo onde o item aparece (`docs/PRD.md`, `docs/RFC.md`, `docs/FDD.md`, `docs/adrs/ADR-002-...md`)
- **Tipo**: Requisito Funcional, Requisito Não Funcional, Decisão, Restrição, Trade-off, entre outros
- **Conteúdo (resumo)**: descrição de uma linha do item
- **Fonte**: `TRANSCRICAO` ou `CODIGO`
- **Localização**: para `TRANSCRICAO`, timestamp + nome do falante (ex: `[09:17] Diego`). Para `CODIGO`, caminho do arquivo (ex: `src/modules/orders/order.service.ts`).

Cobertura mínima: pelo menos 80% dos itens identificáveis nos seus documentos devem ter linha correspondente no tracker.

### 6. README com o processo

O `README.md` na raiz do repositório base contém este enunciado. Substitua o conteúdo dele pela documentação do seu processo de produção. Você pode manter um link ou seção fazendo referência ao enunciado original se quiser, mas o foco do novo conteúdo é descrever sua jornada.

Estrutura obrigatória do novo README:

- **Sobre o desafio**: 1 a 2 parágrafos descrevendo a tarefa em suas palavras
- **Ferramentas de IA utilizadas**: lista das ferramentas que você usou, com breve nota sobre o papel de cada uma
- **Workflow adotado**: como você organizou o trabalho. Em que ordem produziu os documentos, como organizou a interação com a IA
- **Prompts customizados**: pelo menos 2 prompts relevantes que você escreveu ou adaptou, mostrados em blocos de código
- **Iterações e ajustes**: descreva os principais momentos em que a IA gerou algo errado ou superficial e você teve que corrigir. Quantas iterações principais até chegar ao resultado final
- **Como navegar a entrega**: caminho dos arquivos entregues e ordem sugerida de leitura

---

## Critérios de Aceite

A entrega é avaliada contra os critérios abaixo. Todos são obrigatórios.

### PRD (`docs/PRD.md`)

- ☐ Arquivo existe e está em Markdown
- ☐ Contém todas as seções obrigatórias listadas no requisito 1
- ☐ Identifica no mínimo 8 requisitos funcionais discutidos na reunião
- ☐ Inclui pelo menos 1 objetivo com métrica e meta quantitativa
- ☐ Seção "Fora de escopo" lista pelo menos 2 itens explicitamente descartados ou adiados na reunião
- ☐ Seção "Riscos" inclui pelo menos 2 riscos com probabilidade, impacto e mitigação

### RFC (`docs/RFC.md`)

- ☐ Arquivo existe e está em Markdown
- ☐ Contém todas as seções obrigatórias listadas no requisito 2
- ☐ Seção "Alternativas consideradas" lista pelo menos 2 alternativas descartadas na reunião, cada uma com o trade-off que motivou o descarte
- ☐ Seção "Questões em aberto" lista pelo menos 2 pontos adiados ou não decididos na reunião
- ☐ Referencia, com link, pelo menos 2 ADRs do pacote

### FDD (`docs/FDD.md`)

- ☐ Arquivo existe e está em Markdown
- ☐ Contém todas as seções obrigatórias listadas no requisito 3
- ☐ Seção "Contratos públicos" inclui pelo menos 4 endpoints HTTP com payload de exemplo (request e response) e status codes
- ☐ Matriz de erros usa códigos com prefixo `WEBHOOK_`
- ☐ Seção "Integração com o sistema existente" referencia pelo menos 4 caminhos de arquivo reais do código base
- ☐ Seção "Observabilidade" cita métricas, logs e tracing

### ADRs (`docs/adrs/ADR-NNN-*.md`)

- ☐ Pasta `docs/adrs/` contém entre 5 e 8 arquivos no formato `ADR-NNN-titulo-em-kebab-case.md`
- ☐ Cada ADR contém as seções Status, Contexto, Decisão, Alternativas Consideradas, Consequências
- ☐ O conjunto cobre pelo menos 5 das 6 decisões principais listadas no requisito 4
- ☐ Pelo menos 1 ADR referencia explicitamente arquivos, módulos ou classes do código base

### Tracker (`docs/TRACKER.md`)

- ☐ Arquivo existe e segue o formato de tabela definido no requisito 5
- ☐ Pelo menos 80% dos itens identificáveis dos documentos têm linha correspondente
- ☐ Pelo menos 70% das linhas têm Fonte = `TRANSCRICAO` com timestamp válido no formato `[hh:mm] Nome`
- ☐ Pelo menos 5 linhas têm Fonte = `CODIGO` com caminho de arquivo real

### README (`README.md`)

- ☐ Contém todas as seções obrigatórias listadas no requisito 6
- ☐ Lista pelo menos 1 ferramenta de IA utilizada
- ☐ Mostra pelo menos 2 prompts customizados em blocos de código
- ☐ Descreve pelo menos 2 iterações ou ajustes concretos feitos durante a produção

### Consistência geral

- ☐ Nenhum requisito, decisão ou restrição registrada nos documentos contradiz a transcrição ou o código
- ☐ Nenhum arquivo de código mencionado nos documentos é inexistente no repositório

---

## Estrutura obrigatória do entregável

```
.
├── README.md                              (substituído pelo aluno)
├── TRANSCRICAO.md                         (não alterar)
├── docs/
│   ├── PRD.md                             (preenchido pelo aluno)
│   ├── RFC.md                             (preenchido pelo aluno)
│   ├── FDD.md                             (preenchido pelo aluno)
│   ├── TRACKER.md                         (preenchido pelo aluno)
│   └── adrs/
│       ├── ADR-001-titulo-curto.md
│       ├── ADR-002-titulo-curto.md
│       ├── ADR-003-titulo-curto.md
│       ├── ADR-004-titulo-curto.md
│       ├── ADR-005-titulo-curto.md
│       └── ... (até 8 ADRs)
├── src/                                   (não alterar)
├── prisma/                                (não alterar)
├── tests/                                 (não alterar)
└── ... (demais arquivos do boilerplate)
```

A entrega deve ser feita como repositório público no GitHub, a partir de fork do repositório base do desafio.

## Repositório base

O repositório base do desafio contém a aplicação completa, a transcrição e a estrutura de pastas pra você preencher:

https://github.com/devfullcycle/mba-ia-desafio-design-docs-com-ia

## Ordem de execução sugerida

1. **Fork e setup**: faça o fork do repositório base e clone localmente.
2. **Contextualização com IA**: forneça à IA acesso ao código (via Claude Code, Cursor lendo o repo, ou colando trechos relevantes) e à transcrição. Peça uma exploração inicial para entender estrutura, padrões e o que a feature precisa endereçar.
3. **ADRs primeiro**: identifique e produza as decisões principais antes dos demais documentos. As decisões formam o esqueleto do "como implementar".
4. **RFC**: consolide a proposta técnica em cima das decisões. As alternativas descartadas e as questões em aberto da reunião têm lugar natural aqui. Referencie os ADRs já escritos.
5. **FDD**: com as decisões formalizadas e a proposta consolidada, o desenho técnico se constrói em cima delas. Lembre da seção obrigatória "Integração com o sistema existente".
6. **PRD**: produza o PRD por último entre os grandes documentos. Como ele é mais alto nível, com RFC, FDD e ADRs em mãos vira praticamente uma consolidação.
7. **Tracker**: monte em paralelo com os outros documentos ou no fim, varrendo os documentos prontos.
8. **README do processo**: deixe por último, quando o processo já está completo e você pode documentá-lo com clareza.
9. **Revisão final**: passe pela checklist de critérios de aceite item por item antes do push final.
10. **Itere**: é esperado que o processo demande 3 a 5 ciclos de geração, revisão crítica, ajuste de prompt e nova geração. Se você gerou tudo de primeira sem ajustes, os documentos provavelmente estão genéricos demais.

## Dicas Finais

A qualidade do prompt determina a qualidade do documento. Prompts vagos do tipo "gere um PRD a partir dessa transcrição" produzem documentos vazios e genéricos. Aproveite os prompts disponibilizados pelo professor no curso como base e adapte-os ao contexto deste desafio.

O tracker é seu melhor aliado contra alucinações da IA. Se você não consegue preencher a coluna "Localização" para uma linha do PRD ou do FDD, é sinal de que aquela informação não tem origem identificável e provavelmente foi inventada pela IA. Ajuste ou remova.

Cuidado com o que NÃO entra na documentação. A reunião descarta explicitamente algumas ideias. Se essas coisas aparecerem como requisito nos seus documentos, é sinal de que a IA não está sendo cuidadosa com o que você pediu.

A restrição de não alterar o código da aplicação é absoluta: o código serve de contexto e referência, e o entregável é puramente documental.

Itere bastante. Os primeiros documentos que a IA gerar provavelmente serão superficiais ou redundantes. Volte com correções, peça refinamento de pontos específicos, peça para remover trechos vagos, peça exemplos concretos. O resultado final deve parecer escrito por alguém que pensou no problema com a IA ao lado, não por alguém que copiou e colou da transcrição.

</details>
