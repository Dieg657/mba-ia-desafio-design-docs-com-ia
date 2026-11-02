# ADR-005: Autenticar entregas com HMAC-SHA256 e secret por endpoint

## Status

Aceito

- **Decidido em:** reunião técnica, [09:22] Sofia; reafirmado em [09:48] Larissa
- **Relacionados:** [ADR-006](ADR-006-at-least-once-com-x-event-id.md) (headers de entrega), [ADR-007](ADR-007-reuso-dos-padroes-do-projeto.md) (padrões do módulo)

## Contexto

Eventos com dados de pedidos serão enviados a endpoints fora da infraestrutura da plataforma. O cliente precisa validar que a requisição veio da plataforma e que o payload não foi adulterado ([09:19] Sofia). O código atual não tem nenhum mecanismo de assinatura de saída.

**Direcionadores**

- **ADR-005-CTX-01:** HMAC-SHA256 é padrão de mercado, e "todo cliente sério tem biblioteca pra isso" ([09:20] Sofia).
- **ADR-005-CTX-02:** com uma secret global, "se vaza uma, vaza tudo" ([09:21] Sofia).
- **ADR-005-CTX-03:** o cliente precisa de tempo para migrar seus sistemas ao trocar de secret ([09:21] Sofia).
- **ADR-005-CTX-04:** já houve cliente que vazou secret em log da própria aplicação ([09:22] Diego).

## Decisão

Cada entrega é assinada com HMAC-SHA256 sobre o corpo do request, e a assinatura vai no header `X-Signature` ([09:20] Sofia, [09:22] Sofia).

Cada endpoint de webhook tem secret única ([09:21] Sofia), gerada pela plataforma e devolvida na criação ([09:31] Marcos). O cadastro guarda URL, secret, `customer_id` e estado ativo ([09:21] Bruno).

A secret é rotacionável por API. Depois da rotação, a secret antiga continua válida em paralelo por 24 horas e então deixa de valer ([09:21] Sofia).

A obrigatoriedade de `https` e o limite de 64KB também foram tratados, mas a própria reunião os classificou como validação e requisito não funcional, sem decisão arquitetural separada ([09:23] Sofia, [09:24] Larissa). Eles ficam no [FDD](../FDD.md).

## Alternativas Consideradas

### ADR-005-ALT-01: Secret global da plataforma

- **Origem:** discutida em [09:21] Sofia
- **Prós:** teria uma única secret para gerenciar.
- **Contras:** um único vazamento comprometeria todos os clientes.
- **Por que foi descartada:** "Senão se vaza uma, vaza tudo" ([09:21] Sofia).

### ADR-005-ALT-02: Rotação com troca imediata, sem período paralelo

- **Origem:** (plausível, não discutida)
- **Prós:** a secret antiga deixaria de valer imediatamente.
- **Contras:** exigiria que o cliente trocasse a secret em todos os sistemas ao mesmo tempo.
- **Por que foi descartada:** a decisão registrada prevê 24 horas para o cliente migrar ([09:21] Sofia).

## Consequências

### Positivas

- **ADR-005-CON-01:** o cliente consegue verificar origem e integridade do payload ([09:19] Sofia).
- **ADR-005-CON-02:** o vazamento de uma secret não atinge os demais endpoints ([09:21] Sofia).
- **ADR-005-CON-03:** clientes podem trocar a secret, com tempo para migrar ([09:21] Sofia), o que importa diante de vazamentos como o citado ([09:22] Diego).

### Negativas

- **ADR-005-CON-04:** cada cliente precisa implementar a verificação do lado dele ([09:20] Sofia).
- **ADR-005-CON-05:** a plataforma passa a guardar secrets no cadastro ([09:21] Bruno), e o código de HMAC e geração de secret exige revisão de segurança de pelo menos dois dias úteis antes do deploy ([09:46] Sofia).
- **ADR-005-CON-06:** análise a validar em revisão: como a assinatura cobre o corpo ([09:22] Sofia) e o `X-Timestamp` é enviado em header ([09:44] Diego), o timestamp não fica protegido pela assinatura.

**Não definido na reunião:** como as duas secrets são usadas durante as 24 horas, ponto sem definição em FDD-ND-02 e RFC-QA-10; e como a secret é armazenada em repouso, com escolha do autor pendente da revisão de Sofia em FDD-AUT-02 e RFC-QA-11.

### Trade-off aceito

**ADR-005-TO:** aceitamos gerenciar uma secret por endpoint, com rotação, e transferir a verificação para o cliente em troca de limitar o alcance de vazamentos e usar um padrão conhecido ([09:20] Sofia, [09:21] Sofia).

## Referências

**Transcrição**

- [09:19] Sofia: necessidade de validar origem e integridade.
- [09:21] Sofia: secret única e rotação com 24 horas.
- [09:22] Sofia: fechamento da decisão.
- [09:31] Marcos: secret gerada pela plataforma e devolvida na criação.

**Código**

- **ADR-005-COD-01:** `src/shared/logger/index.ts`: a lista atual de campos ocultados nos logs não inclui secret; o tratamento está sem definição em FDD-ND-18.

## Changelog

Detalhes e IDs afetados na seção Changelog de [TRACKER.md](../TRACKER.md).

| CHG | Resumo |
| --- | --- |
| CHG-000 | Versão inicial, linha de base do pacote |
| CHG-023 | Notas apontam para o resultado no FDD e no RFC; citação de vazamento corrigida |
| CHG-046 | Link para o tracker com rótulo relativo a este arquivo e sem âncora |
