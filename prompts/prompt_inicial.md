> **Arquivo histórico, não normativo:** registra uma proposta inicial anterior à separação/consolidação das três disciplinas. Use [`../docs/specification/README.md`](../docs/specification/README.md) como referência vigente.

Atue como um Arquiteto de Sistemas e Especialista em Documentação Técnica. Vamos iniciar o repositório do projeto **Sistema de Gerenciamento de Produtos e Estoque** (inspirado na experiência organizacional e visual de um catálogo/painel estilo Amazon), integrando as disciplinas de **Desenvolvimento Web** e **Estrutura de Dados**.

### REGRA CRÍTICA (LEIA COM ATENÇÃO):
**NÃO ESCREVA NENHUM CÓDIGO-FONTE NESTA ETAPA.**
Está estritamente proibido criar arquivos `.js`, `.html`, `.css`, `package.json` ou qualquer implementação de servidor, frontend ou estruturas de dados agora. O objetivo desta tarefa é **EXCLUSIVAMENTE** gerar a documentação arquitetural, os diagramas em Mermaid e o registro de governança de IA para que a equipe (Adones Gomes, Nicolas Guedes e Davi Renato) possa revisar e iniciar o desenvolvimento do zero.

### 1. Contexto Técnico Definido para o Projeto
- **Stack prevista:** Backend em Node.js puro (sem ORM e sem banco de dados externo; armazenamento em memória via estruturas de dados), Frontend em HTML5, CSS3 e JavaScript Vanilla (ES6+), versionado com Git e GitHub.
- **Estruturas de Dados obrigatórias:**
  1. **Tabela Hash:** Chave pelo `codigo` do produto, tratamento de colisões por encadeamento (*separate chaining*), usada para cadastro (verificação de duplicidade), consulta rápida ($O(1)$ médio), atualização e remoção.
  2. **Árvore Binária de Busca (ABB):** Organizada pelo `codigo` do produto, usada para manter a hierarquia e listar todos os produtos em ordem crescente de código através do percurso em ordem (*in-order traversal*, $O(n)$).
  3. **Integração:** Ambas as estruturas armazenam referências para o mesmo objeto `Produto` (`codigo`, `nome`, `descricao`, `preco`, `quantidade`, `estoqueMinimo`), evitando duplicação de dados em memória.
- **Funcionalidades planejadas:** Cadastrar produto, consultar por código, atualizar quantidade em estoque, remover produto, listar todos em ordem crescente de código, definir quantidade mínima e identificar/alertar produtos com estoque baixo.

### 2. Arquivos que você deve criar AGORA (Apenas Markdown e `.gitignore`):

1. **`README.md` (Completo e em Português):**
   - Título, visão geral do problema e objetivos do projeto.
   - Integrantes da equipe: Adones Gomes, Nicolas Guedes e Davi Renato.
   - Stack tecnológica e arquitetura planejada (explicando o papel do Node.js, HTML/CSS/JS, Tabela Hash e ABB).
   - Tabela de complexidade assintótica ($O(1)$, $O(\log n)$, $O(n)$) comparando as operações na Tabela Hash e na ABB.
   - **Diagramas em Mermaid (` ```mermaid `)** embutidos diretamente no README (ou linkados de `docs/`):
     - **Diagrama de Casos de Uso / Fluxo Funcional:** Mostrando o usuário realizando cadastro, busca rápida, atualização de estoque, remoção, listagem ordenada e verificação de estoque baixo.
     - **Modelagem de Dados e Classes (`classDiagram` e `erDiagram`):** Representando `Produto`, `NoABB`, `ArvoreBinariaBusca`, `TabelaHash` e `EstoqueManager` compartilhando a referência do objeto `Produto`.
     - **Diagramas de Sequência (`sequenceDiagram`):** Ilustrando o fluxo Frontend -> API Node -> `EstoqueManager` -> Tabela Hash + ABB para as operações de Cadastro (com validação de duplicidade) e Consulta por Código.

2. **`docs/MODELAGEM_MERMAID.md`:**
   - Um documento dedicado reunindo todos os diagramas Mermaid em detalhe (Casos de Uso, Modelagem de Dados/Entidades, Arquitetura Cliente-Servidor e Fluxos de Operação da ABB e Tabela Hash), prontos para serem visualizados no GitHub ou exportados.

3. **`docs/AI_PROMPTS.md` (Registro Obrigatório de Uso de IA):**
   - Crie este arquivo explicando a regra do projeto de documentar todos os prompts enviados às ferramentas de IA.
   - Inclua um template padrão no topo para os próximos registros.
   - Registre **este exato prompt** como o **`Prompt #01`** (Autor: Nicolas Guedes, Ferramenta: Codex CLI, Objetivo: Criação da documentação inicial, modelagem Mermaid e estrutura de governança sem geração de código).

4. **`.gitignore`:**
   - Arquivo padrão ignorando `node_modules/`, `.env`, `.DS_Store` e logs temporários.

Confirme ao final que nenhum arquivo de código (`.js`, `.html`, `.css`) foi criado e liste apenas os arquivos de documentação gerados.
