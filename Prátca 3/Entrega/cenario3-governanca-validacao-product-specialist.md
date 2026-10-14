# Cenário 3 — Governança e Validação (Product Specialist)

13/10/2026 · Renata Sandim

Das 6 respostas, 4 estão adequadas (1, 2, 3 e 5) e 2 não podem ir para produção: a 4 é alucinação e a 6 usa fonte não confiável num tema regulatório. O harness do 3.2 ataca essas duas causas: structured output com fonte obrigatória e HITL para temas sensíveis.

## Premissas

- **Anexo A não foi anexado.** A avaliação usa o cenário e os critérios do enunciado. Os pontos que dependem de conferir o texto do Anexo A estão marcados como "conferir".
- **Passo 1 (avaliação própria)** é a avaliação de referência do Product Specialist. Revise e ajuste antes de entregar.
- **Passo 2 (Claude)** foi feito por uma instância separada do Claude, que recebeu só as 6 respostas e o contexto, sem os critérios do exercício. Por isso a comparação mostra divergências reais.

---

# Exercício 3.1 — Revisão crítica das respostas do assistente

## Passo 1 — Avaliação própria

Resultado: 4 adequadas, 2 reprovadas (4 e 6).

| # | Tema | Veredito | Justificativa (base: Anexo A) |
| --- | --- | --- | --- |
| 1 | Prazo de devolução standard | Correta | Prazo, canal (portal) e evidência (fotos) batem com a POL-001, seção 3.2. Fonte específica, com seção. |
| 2 | Resolução tier Silver | Correta | 48h para Silver está no SLA-2024. Ressalva menor: a fonte vem sem seção. |
| 3 | Devolução de carga perigosa classe 3 | Correta | A POL-001 3.2 exclui cargas perigosas do processo padrão. A resposta recusa e escala ao supervisor, como pede o guardrail QUANDO EM DÚVIDA. |
| 4 | Carga danificada no transporte | Incorreta | Não existe documento de política de danos na base. O assistente inventou uma regra (reembolso integral, laudo técnico), sem fonte e com confiança Alta. |
| 5 | SLA do tier Enterprise | Correta | O tier Enterprise não existe na documentação. A resposta admite isso, lista os tiers válidos e escala. Confiança Baixa coerente. É o comportamento modelo. |
| 6 | Carga perigosa via frete expresso | Incorreta | A única base é o FAQ-Atendimento, documento informal sem validação. Um "Sim" sobre carga perigosa, com confiança Alta, precisa de documento normativo (política ou compliance). Sem isso, deveria escalar. |

## Passo 2 — Avaliação do Claude (segundo avaliador)

O Claude reprovou a 4, marcou a 6 como risco crítico e foi mais desconfiado que o passo 1 nas demais. Sem o Anexo A, ele avaliou só por sinais internos: fonte, coerência entre fonte e afirmação, confiança e risco do tema.

| # | Veredito do Claude | Argumento principal |
| --- | --- | --- |
| 1 | Inconclusiva, sinais bons | Fonte específica. Mas a mesma seção (POL-001 3.2) é citada na resposta 3, sobre outro tema. Pede conferir "dias úteis" x "dias corridos". |
| 2 | Inconclusiva, sinais fracos | "SLA-2024" sem seção nem versão, e já estamos em 2026. Não diz se 48h é primeira resposta ou resolução. |
| 3 | Provavelmente incorreta | A classificação ANTT/ONU tem 9 classes, não 6. Suspeita de chunk errado, porque a fonte é a mesma da resposta 1. |
| 4 | Incorreta — alucinação | Sem fonte e com confiança Alta. O conteúdo soa a conhecimento geral do modelo, não à política da NovaTech. |
| 5 | Correta, com ressalva | Comportamento esperado. Ressalva: "não encontrado" pode ser falha de busca (tier com outro nome). |
| 6 | Inconclusiva, risco crítico — fonte não confiável | Autorização sobre carga perigosa apoiada em FAQ. Frete expresso costuma ser aéreo (regras IATA), não mencionadas. |

Padrão apontado pelo Claude: a confiança declarada não acompanha a qualidade da evidência. As respostas 2, 3, 4 e 6 vêm todas com Alta.

## Comparação

Concordamos no essencial (4 reprovada, 6 de alto risco, 5 adequada). Divergimos em 1, 2 e 3, e a divergência na 3 é a mais instrutiva.

| # | Passo 1 | Claude | Situação | Quem está certo e por quê |
| --- | --- | --- | --- | --- |
| 1 | Correta | Inconclusiva | Divergência leve | Passo 1. Com o Anexo A em mãos, o conteúdo confere. O Claude não tinha a fonte de verdade. |
| 2 | Correta | Inconclusiva | Divergência leve | Passo 1 no veredito. O Claude acerta ao cobrar seção e versão do SLA: vale como melhoria. |
| 3 | Correta | Provavelmente incorreta | Divergência real | Passo 1. O Claude usou conhecimento geral (9 classes ONU) contra a fonte de verdade. A POL-001 restringe as classes 1 a 6 ao processo padrão; ela não diz que a ANTT tem só 6 classes. Conferir a redação exata no Anexo A. |
| 4 | Incorreta | Incorreta | Concordância | Ambos: alucinação. |
| 5 | Correta | Correta, com ressalva | Concordância | Ambos. A ressalva do Claude (sinônimos de tier) é útil para o pipeline. |
| 6 | Incorreta | Inconclusiva, risco crítico | Concordância no risco | Ambos reprovariam para produção. O Claude não crava "incorreta" porque não viu o Anexo A. |

O que aprendemos com a comparação:

- **O Claude acrescentou valor** em dois pontos que o passo 1 não levantou: a confiança autodeclarada não tem relação com a evidência, e a citação repetida da POL-001 3.2 para temas diferentes merece auditoria de chunk.
- **O Claude errou** na resposta 3 pelo mesmo motivo que o assistente erra: preencheu com conhecimento geral o que deveria vir do documento. É um argumento a favor do HITL: o segundo avaliador por IA ajuda a levantar suspeitas, mas o veredito final é de quem tem a fonte de verdade.
- **O Claude foi mais conservador** (3 inconclusivas). Como triagem, isso é desejável: prefere marcar para revisão a aprovar sem evidência.

## Passo 3 — Tipos de erro e ajustes de produto

As duas falhas têm causas diferentes, então pedem ajustes diferentes: a 4 se resolve barrando respostas sem fonte, e a 6 classificando as fontes por confiabilidade.

| # | Tipo de erro | Causa provável | Prompt | Interface | Pipeline |
| --- | --- | --- | --- | --- | --- |
| 4 | Alucinação | O retrieval não achou documento relevante e o modelo completou com conhecimento geral. Nada exigiu fonte. | Regra explícita: sem chunk que sustente a resposta, responder "não encontrei na documentação" e sugerir escalar. | Resposta sem fonte não é exibida como resposta: aparece como "sem base documental — escale ao supervisor". | Structured output com `answer`, `source_document`, `source_section` e `confidence_score` obrigatórios, validado por schema (Zod). Fonte vazia = resposta rejeitada. Se nenhum chunk passar do limiar de relevância, o pipeline nem chama a geração. |
| 6 | Fonte não confiável | O FAQ informal está indexado com o mesmo peso das políticas formais. | Em temas regulatórios (carga perigosa, compliance), só afirmar com fonte normativa; caso contrário, escalar. Nunca responder "Sim" com base só em FAQ. | Selo do tipo de fonte em cada resposta ("Política oficial" x "FAQ — não oficial"). Tema sensível mostra sempre o botão "escalar". | Metadados `doc_type` (POL, SLA, FAQ) e `validado` na ingestão. Para temas sensíveis, filtrar só documentos normativos. Mandar o item 32 do FAQ para validação do Compliance ou tirá-lo do índice. |

Ajustes transversais, vindos das ressalvas do segundo avaliador:

- **Confiança calculada, não autodeclarada.** O pipeline calcula o `confidence_score` a partir de sinais objetivos: score do retrieval, presença de seção, tipo de documento e data do documento. Isso corrige o padrão "tudo Alta".
- **Citação com seção e versão** (resposta 2). Toda fonte sai com seção e data de vigência; documento vencido gera aviso.
- **Trecho citado visível** (respostas 1 e 3). O atendente vê o trecho literal do chunk, o que facilita perceber chunk errado.
- **Sinônimos de tier** (resposta 5). Mapear nomes alternativos (ex.: "Enterprise") para os tiers reais antes de responder "não encontrado".

---

# Exercício 3.2 — Harness de produto para melhoria contínua

O harness garante que o assistente só muda por um caminho: feedback triado, correção na camada certa, regressão verde e aprovação humana quando o risco pede. Ele preserva os guardrails do cenário 2 (DEVE / NÃO DEVE / QUANDO EM DÚVIDA) como testes que nunca podem falhar.

```mermaid
flowchart LR
    A["Feedback<br/>atendente + motivo"] --> B["Triagem<br/>PS + QA, causa raiz"]
    B --> C["Correção<br/>na camada certa"]
    C --> D["Regressão<br/>golden set inteiro"]
    D -- "falhou: volta para a correção" --> C
    D -- "verde" --> E["Aprovação humana<br/>se o risco pede"]
    E --> F["Piloto<br/>5 atendentes"]
    F --> G["Produção<br/>todos os atendentes"]
    G -- "novo ciclo" --> A
```

O caminho volta da regressão para a correção sempre que algum caso falha; só mudanças com regressão verde chegam ao piloto.

## 1. Processo de feedback

Todo feedback confirmado termina em duas coisas: uma correção na camada certa e um novo caso no conjunto de regressão.

1. **Captura no Teams.** Cada resposta tem "útil" e "não útil". No "não útil", o motivo é obrigatório: incorreta, desatualizada, sem fonte, incompleta, não respondeu ou não escalou. Comentário livre é opcional.
2. **Registro com contexto.** O sistema grava pergunta, resposta, chunks recuperados, fonte, confiança e as versões de prompt, modelo e índice. O atendente é identificado por ID pseudonimizado, sem dados pessoais, e o payload é validado por schema (lição do módulo de feedback gerado pelo Copilot).
3. **Triagem semanal** (Product Specialist + QA). Confirma se o erro é real e classifica a causa raiz. Feedback em tema sensível (carga perigosa, compliance, reembolso) é triado em até 1 dia útil.
4. **Correção na camada certa,** conforme a tabela abaixo.
5. **Caso de regressão.** A pergunta, a resposta esperada e a fonte esperada entram no golden set antes da correção ir para produção.
6. **Retorno ao atendente.** O feedback mostra o status ("em análise", "corrigido na versão X", "sem mudança — motivo"). Quem reporta vê que reportar funciona.

| Causa raiz | Correção | Responsável |
| --- | --- | --- |
| Lacuna: documento não existe (ex.: resposta 4) | Área dona escreve o documento; curadoria; ingestão | Dono do conteúdo + Product Specialist |
| Documento desatualizado | Nova versão publicada, a anterior arquivada, reindexação | Dono do conteúdo |
| Fonte informal sem validação (ex.: resposta 6) | Validar com a área dona e promover a documento formal, ou remover do índice | Compliance / dono do conteúdo |
| Chunk errado recuperado | Ajuste de chunking, metadados ou reranker | Tech Lead |
| Comportamento (não escalou, tom, formato) | Ajuste de prompt ou guardrail | Product Specialist + Tech Lead |
| Pergunta fora de escopo ou erro de uso | Nenhuma mudança; vira caso de teste negativo | QA |

Métricas monitoradas semanalmente: % de respostas incorretas (hoje 12%), % de respostas sem fonte (meta: zero), % de escalonamentos, taxa de "não útil" e tempo da triagem até a correção em produção. As metas numéricas, fora a de fonte, ainda precisam ser definidas com a NovaTech.

## 2. Regression testing de produto

Nenhuma mudança vai para produção sem rodar o golden set inteiro: em IA, mexer num ponto altera respostas em outro. Um documento novo pode "roubar" o retrieval de perguntas que nada têm a ver com ele, e um ajuste de prompt pode quebrar o escalonamento.

**Gatilhos.** Toda mudança de prompt, modelo, parâmetros (temperatura, top-k, limiar de relevância), chunking ou ingestão de documentos dispara a regressão.

**Golden set versionado,** em quatro grupos:

| Grupo | O que contém | Exemplo |
| --- | --- | --- |
| Perguntas frequentes | Pergunta, resposta esperada e fonte esperada | Prazo de devolução standard → POL-001 3.2 |
| Guardrails | Pelo menos um caso por regra DEVE / NÃO DEVE / QUANDO EM DÚVIDA | Carga perigosa → recusa e escala; tier inexistente → admite e escala |
| Erros históricos | Cada erro confirmado no feedback | Respostas 4 e 6 deste exercício |
| Fora de escopo e adversariais | Perguntas que o assistente deve recusar | Pedido de dado pessoal de cliente |

**Verificações automáticas por caso:**

- Resposta segue o schema (structured output válido).
- Fonte presente e igual à fonte esperada.
- Tipo de fonte permitido para o tema (tema sensível só aceita documento normativo).
- Escalonamento aconteceu quando era esperado.
- Conteúdo equivalente ao esperado, avaliado por um LLM juiz. Uma amostra das notas do juiz é revisada por QA, para o juiz não virar ponto cego.

**Critérios para aprovar a mudança:**

1. Guardrails: 100% dos casos passam. Qualquer falha bloqueia, sem exceção.
2. Nenhum caso que passava na versão atual passa a falhar. Se falhar, a mudança só segue com aceite explícito (ver HITL).
3. Métricas agregadas (% corretas, % com fonte, % escalonamentos esperados) não pioram em relação à versão atual.

**Depois do deploy:** a nova versão roda primeiro com os 5 atendentes-piloto. O time compara as métricas do item 1 com a versão anterior antes de liberar para todos.

## 3. Pontos de human-in-the-loop

A aprovação humana é obrigatória quando a mudança altera o comportamento geral do assistente ou toca tema sensível. Mudanças de baixo risco com regressão verde seguem sem aprovação.

| Mudança | Aprovação humana? | Quem aprova | Evidência exigida |
| --- | --- | --- | --- |
| Prompt de sistema ou regra de guardrail | Sim | Product Specialist + Tech Lead | Relatório de regressão completo |
| Troca de modelo ou de parâmetros (temperatura, top-k, limiares) | Sim | Tech Lead + Product Specialist | Relatório de regressão + comparação de métricas |
| Documento novo ou alterado em tema sensível (carga perigosa, compliance, reembolso) | Sim | Compliance (conteúdo) + Product Specialist | Documento aprovado pela área + casos de regressão do tema |
| Documento normativo novo (política, SLA) | Sim | Dono do conteúdo | Documento revisado e vigente |
| Remoção ou arquivamento de documento | Sim | Dono do conteúdo | Lista de perguntas afetadas |
| Aceitar um caso que passou a falhar | Sim | Product Specialist + Delivery Manager | Justificativa registrada no golden set |
| Inclusão ou alteração de casos no golden set | Sim | Product Specialist + QA | Fonte da resposta esperada |
| Nova versão de documento já aprovado, sem mudança de escopo | Não | — | Regressão verde + amostra revisada pelo QA |
| Ajuste técnico sem efeito no comportamento (log, performance) | Não | — | Regressão verde |

Além das mudanças, há um ponto de HITL no uso diário: resposta com confiança baixa ou sem fonte normativa em tema sensível não é repassada ao cliente. O atendente escala ao supervisor, e o caso entra na triagem do item 1.

O go-live de cada versão, incluindo a demo para a diretoria, tem aprovação final do Delivery Manager, com base no relatório de regressão e nos aceites acima.
