# Orientações para apoio automatizado

## Fonte normativa

Use [`specification/README.md`](specification/README.md) como especificação técnica de referência do AmazOFF. Ela separa Tecnologias Web, Estrutura de Dados e Algoritmos e Banco de Dados II, lista requisitos, classes, operações, critérios e pendências.

Os requisitos oficiais considerados nesta revisão foram transcritos pela equipe na solicitação; os anexos PDF/DOCX citados ainda não estão no repositório. Não atribua a eles regras além do conteúdo transcrito. Diferencie sempre requisito oficial, decisão de projeto, recomendação e pendência.

## Restrições importantes

- Tecnologias Web: arrays e objetos JavaScript em memória, sem exigir backend/banco/autenticação nessa entrega.
- Estrutura de Dados: ABB e tabela hash próprias; não substituir por estruturas prontas.
- Banco de Dados II: Java/Gradle, terminal, DAO/Repository e pelo menos dois bancos simultâneos escolhidos entre Riak, MongoDB e Cassandra.
- Não trate a configuração Spring Boot em `amazoff-backend/` como prova suficiente de atendimento a BD II; Java/Gradle, menus de terminal e dois bancos continuam critérios próprios.
- Não invente decisões para as pendências da especificação; peça confirmação quando a escolha alterar dados, arquitetura ou operação.
- Não implemente funcionalidades ao receber pedido de auditoria ou documentação.

## Histórico

[`../prompts/AI_PROMPTS.md`](../prompts/AI_PROMPTS.md) e `../prompts/prompt_inicial.md` preservam prompts anteriores, alguns baseados em propostas antigas. São arquivo histórico, não fonte normativa; conflitos devem ser resolvidos a favor da especificação integrada e dos requisitos oficiais confirmados pela equipe.
