# Governança e registro de prompts de IA

Todos os prompts enviados às ferramentas de IA no contexto deste projeto devem ser registrados neste arquivo. O registro permite revisar a origem das decisões e identificar os artefatos produzidos. Cada entrada deve preservar o texto integral do prompt, sem paráfrases.

## Template para os próximos registros

````markdown
## Prompt #NN — Título

- **Data:** AAAA-MM-DD
- **Autor:** Nome
- **Ferramenta:** Nome da ferramenta
- **Objetivo:** Resultado pretendido
- **Arquivos gerados/alterados:** caminhos dos arquivos

### Prompt literal

```text
Texto integral do prompt enviado.
```
````

## Prompt #01 — Criação da documentação inicial e modelagem Mermaid

- **Data:** 2026-11-22
- **Autor:** Nicolas Guedes
- **Ferramenta:** Codex CLI
- **Objetivo:** Criação da documentação inicial, modelagem Mermaid e estrutura de governança sem geração de código.
- **Arquivos gerados/alterados:** `README.md`, `docs/MODELAGEM_MERMAID.md`, `docs/AI_PROMPTS.md`, `.gitignore`

### Prompt literal

```text
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
```


# PROMPT PARA CODEX — REESTRUTURAÇÃO DOCUMENTAL DO AMAZOFF

## 1. Contexto do projeto

Você está trabalhando no repositório GitHub:

https://github.com/DEVguedes1/amazoff

O projeto AmazOFF é um sistema de gerenciamento de produtos e estoque, desenvolvido como projeto acadêmico interdisciplinar envolvendo três disciplinas:

- Estrutura de Dados.
- Tecnologias Web.
- Banco de Dados II.

O projeto está sendo ampliado. Inicialmente, a arquitetura previa Node.js e armazenamento em memória, mas agora o backend será desenvolvido em Java, utilizando Spring Boot de maneira enxuta.

O frontend continuará utilizando HTML, CSS e JavaScript Vanilla.

O objetivo desta solicitação é exclusivamente reorganizar a documentação e preparar a estrutura de diretórios do repositório para o desenvolvimento futuro.

## 2. RESTRIÇÃO ABSOLUTA: NÃO IMPLEMENTAR CÓDIGO

NÃO ESCREVA NENHUM CÓDIGO DE APLICAÇÃO.

Esta restrição é obrigatória.

Você NÃO deverá:

- Criar classes Java.
- Implementar métodos.
- Criar interfaces Java.
- Criar controllers.
- Criar services.
- Criar repositories.
- Implementar árvores ou tabelas hash.
- Criar endpoints.
- Implementar integrações com bancos.
- Criar scripts JavaScript.
- Criar páginas HTML.
- Criar folhas CSS.
- Criar configurações de Docker.
- Criar arquivos de configuração de aplicação.
- Gerar um pom.xml.
- Implementar testes automatizados.
- Instalar dependências.
- Executar instalações ou configurar bancos de dados.
- Implementar qualquer funcionalidade.

Não tente antecipar o desenvolvimento.

O projeto será implementado posteriormente pelos integrantes da equipe.

Seu trabalho termina quando a documentação estiver atualizada e a estrutura de diretórios estiver preparada.

## 3. Primeira etapa: analisar o repositório

Antes de realizar alterações:

1. Examine o README.md atual.
2. Examine a estrutura de diretórios existente.
3. Examine os documentos presentes em docs.
4. Identifique os diagramas Mermaid já existentes.
5. Preserve informações técnicas válidas sobre a Tabela Hash e a Árvore Binária de Busca.
6. Identifique referências antigas a Node.js e armazenamento exclusivamente em memória que precisam ser atualizadas.
7. Preserve o histórico conceitual do projeto.

Não exclua informações relevantes sem justificativa.

Não substitua o conteúdo existente por um README genérico.

## 4. Segunda etapa: reformular o README.md

O README da raiz deverá ser completamente reorganizado, com apresentação profissional e documentação técnica detalhada.

Utilize Markdown do GitHub e diagramas Mermaid quando forem úteis.

Inclua as seguintes seções:

### 4.1. Apresentação

- Nome AmazOFF.
- Descrição do sistema.
- Objetivo geral.
- Contexto acadêmico.
- Disciplinas envolvidas.

### 4.2. Objetivos do projeto

Explicar que a aplicação tem como objetivo gerenciar produtos e estoque, demonstrando conceitos de estruturas de dados, desenvolvimento web e persistência NoSQL.

### 4.3. Disciplinas envolvidas

Separar claramente:

Estrutura de Dados

- Árvore Binária de Busca.
- Tabela Hash com encadeamento separado.
- Operações de inserção, consulta, atualização e remoção.
- Listagem ordenada.
- Análise de complexidade.
- Consistência entre as estruturas.

Tecnologias Web

- Frontend HTML5.
- CSS3.
- JavaScript Vanilla.
- Backend Java.
- Spring Boot.
- API REST.
- Comunicação HTTP.
- JSON.
- Integração entre frontend e backend.

Banco de Dados II

- Riak.
- MongoDB.
- Cassandra.
- Neo4j.

Informar que as especificações acadêmicas detalhadas de Banco de Dados II ainda serão fornecidas pelo professor. Portanto, a divisão das responsabilidades dos bancos descrita no documento é uma proposta arquitetural inicial, sujeita à validação.

### 4.4. Funcionalidades do sistema

Documentar:

1. Cadastro de produtos.
2. Consulta por código.
3. Atualização de estoque.
4. Remoção de produtos.
5. Listagem crescente por código.
6. Identificação de estoque baixo.
7. Catálogo de produtos.
8. Pesquisa por nome.
9. Filtros por categoria.
10. Detalhes dos produtos.

Preservar as funcionalidades de avaliações e comentários da especificação original como funcionalidades complementares planejadas, deixando claro que seu escopo será confirmado pela equipe.

### 4.5. Arquitetura geral

Documentar a seguinte arquitetura:

Frontend:

- HTML.
- CSS.
- JavaScript.

Backend:

- Java.
- Spring Boot.
- API REST.
- Regras de negócio.
- Serviços.
- Estruturas de dados.
- Integrações com bancos.

Persistência:

- Riak.
- MongoDB.
- Cassandra.
- Neo4j.

Criar um diagrama Mermaid representando a comunicação entre frontend, API Java, estruturas de dados e persistência.

Não representar funcionalidades como implementadas. Utilizar termos como planejado, proposto e previsto.

### 4.6. Estruturas de dados

Preservar e melhorar a documentação já existente sobre:

Tabela Hash

- Consulta por código.
- Tratamento de colisões por encadeamento separado.
- Complexidade média e pior caso.

Árvore Binária de Busca

- Organização pelo código.
- Percurso in-order.
- Listagem crescente.
- Complexidade média e pior caso.
- Ausência de balanceamento automático.

EstoqueManager

- Coordenação das operações.
- Sincronização entre as estruturas.
- Referências compartilhadas para o mesmo objeto Produto.

### 4.7. Responsabilidade dos bancos de dados

Documentar a seguinte proposta inicial:

FASE 1 — Prioridade inicial

Riak KV:

- Armazenamento chave-valor.
- Dados brutos importados da DummyJSON.
- Identificação dos registros por chave.
- Demonstração de operações CRUD.

MongoDB:

- Persistência principal do catálogo operacional.
- Documentos de produtos.
- Informações complementares.
- Avaliações, se mantidas no escopo.

FASE 2 — Implementação posterior

Cassandra:

- Proposta de histórico de movimentações de estoque.
- Registro de entradas e saídas.
- Consultas por produto e período.

Neo4j:

- Proposta de relacionamentos entre produtos.
- Categorias.
- Marcas.
- Produtos relacionados.

IMPORTANTE: essas atribuições são hipóteses de arquitetura e não requisitos oficiais já aprovados pelo professor.

### 4.8. Integração com DummyJSON

Documentar a utilização planejada da API:

https://dummyjson.com/docs/products

Explicar que o backend Java poderá importar produtos externos, converter os dados para o modelo interno e disponibilizá-los para a aplicação.

Não implementar a integração.

### 4.9. Contrato inicial da API

Documentar como proposta os seguintes endpoints:

| Método | Endpoint |
|---|---|
| GET | /api/v1/produtos |
| GET | /api/v1/produtos/{codigo} |
| POST | /api/v1/produtos |
| PUT | /api/v1/produtos/{codigo} |
| PATCH | /api/v1/produtos/{codigo}/estoque |
| DELETE | /api/v1/produtos/{codigo} |
| GET | /api/v1/produtos/estoque-baixo |
| GET | /api/v1/produtos/busca?nome= |
| GET | /api/v1/categorias |
| POST | /api/v1/importacao/dummyjson |

Deixar claro que são contratos planejados, ainda não implementados.

### 4.10. Modelo inicial de Produto

Documentar os atributos previstos:

- codigo: String.
- nome: String.
- descricao: String.
- preco: BigDecimal.
- categoria: String.
- marca: String.
- imagem: String.
- quantidade: Integer.
- estoqueMinimo: Integer.
- dataCadastro: LocalDateTime.

Não criar a classe Java.

### 4.11. Organização do repositório

Apresentar a estrutura planejada de pastas.

### 4.12. Equipe

A equipe possui quatro integrantes:

- Adones Gomes.
- Nicolas Guedes.
- Davi Renato.
- Eric Farias.

Não inventar a distribuição de disciplinas ou responsabilidades individuais.

Indicar que a atribuição nominal das tarefas será documentada posteriormente no backlog.

### 4.13. Cronograma e backlog

Criar uma seção explicando que o backlog detalhado será elaborado após a definição da estrutura do projeto e a confirmação das especificações dos professores.

Não inventar prazos nem atribuir tarefas individuais neste momento.

### 4.14. Status do projeto

Criar uma seção que diferencie:

- Planejamento.
- Estruturação do repositório.
- Implementação futura.
- Integração futura.
- Testes futuros.

Não marcar como concluídas atividades que ainda não foram executadas.

## 5. Terceira etapa: organizar os diretórios

Preparar a seguinte estrutura de diretórios:

amazoff/

- frontend/
  - assets/
  - css/
  - js/
- backend/
  - src/
    - main/
      - java/
        - com/
          - amazoff/
            - controller/
            - model/
            - dto/
            - service/
            - structures/
              - hash/
              - tree/
            - repository/
            - integration/
              - dummyjson/
              - riak/
              - mongodb/
              - cassandra/
              - neo4j/
            - config/
            - exception/
      - resources/
    - test/
      - java/
        - com/
          - amazoff/
- docs/
  - architecture/
  - api/
  - database/
  - structures/
  - backlog/
- docker/

Criar somente os diretórios.

Se necessário, utilizar arquivos .gitkeep vazios para preservar diretórios vazios no Git.

Não criar arquivos-fonte.

Não criar pom.xml.

Não criar arquivos de configuração.

O projeto Spring Boot será gerado separadamente pelo responsável pelo backend, utilizando Spring Initializr, e colocado posteriormente dentro de backend/.

## 6. Quarta etapa: documentação complementar

Criar somente os seguintes documentos Markdown:

- docs/architecture/README.md
- docs/api/README.md
- docs/database/README.md
- docs/structures/README.md
- docs/backlog/README.md
- docs/AI_PROMPTS.md

Cada documento deverá conter uma breve apresentação de sua finalidade e os tópicos que serão documentados futuramente.

Não duplicar integralmente o README principal.

O backlog deverá permanecer como estrutura de planejamento, sem tarefas individuais ou datas definidas nesta etapa.

## 7. Quinta etapa: atualizar diagramas

Atualizar os diagramas Mermaid existentes para representar a nova arquitetura Java e a separação das responsabilidades.

Não remover os diagramas de Estrutura de Dados.

Não inserir código-fonte Java nos diagramas.

Utilizar diagramas de:

- Arquitetura geral.
- Comunicação frontend/backend.
- Fluxo de gerenciamento de produtos.
- Organização das estruturas de dados.
- Organização conceitual dos bancos.

## 8. Regras de qualidade

- Escrever toda a documentação em português brasileiro.
- Utilizar linguagem técnica, clara e objetiva.
- Manter consistência na nomenclatura.
- Evitar informações duplicadas.
- Não inventar requisitos acadêmicos.
- Identificar claramente as decisões provisórias.
- Não declarar integrações como concluídas.
- Preservar informações úteis do README anterior.
- Não modificar funcionalidades ou códigos já existentes.
- Não excluir arquivos de implementação.

## 9. Resultado esperado

Ao terminar, apresentar um relatório contendo:

1. Arquivos modificados.
2. Arquivos Markdown criados.
3. Diretórios criados.
4. Informações antigas atualizadas.
5. Diagramas preservados ou modificados.
6. Decisões que permaneceram provisórias.
7. Confirmação explícita de que nenhum código de aplicação foi escrito.

CRITÉRIO FINAL DE ACEITAÇÃO:

O repositório deverá estar documentalmente organizado, com README atualizado e diretórios preparados para receber o projeto Java Spring Boot e o frontend.

Nenhuma funcionalidade deverá ter sido implementada.

Não avance para a implementação. Encerre o trabalho após a reorganização documental e estrutural.



# Tarefa: Preencher e estruturar a documentação técnica do projeto AmazOFF

Você está trabalhando no repositório GitHub do projeto AmazOFF.

Sua tarefa é analisar a estrutura atual do repositório e transformar os arquivos README e documentos existentes em uma documentação técnica completa, organizada, coerente e útil para orientar o desenvolvimento do projeto.

IMPORTANTE: nesta etapa, trabalhe exclusivamente com documentação. Não implemente nenhuma funcionalidade da aplicação.

## 1. Antes de começar

1. Analise o README.md principal.
2. Analise todos os arquivos e diretórios existentes.
3. Leia a documentação que já existe sobre o projeto, especialmente as informações sobre Estrutura de Dados.
4. Aproveite informações válidas já documentadas.
5. Corrija informações que estejam desatualizadas em relação às decisões atuais do projeto.
6. Não invente requisitos que não foram definidos.
7. Quando alguma decisão ainda estiver pendente, identifique-a claramente como PENDENTE DE DEFINIÇÃO ou PROPOSTA.

Não exclua arquivos de implementação existentes.

## 2. Contexto geral do projeto

O AmazOFF é um sistema de gerenciamento de produtos e estoque, com uma interface web para consulta e administração de produtos.

O projeto está sendo desenvolvido como atividade acadêmica envolvendo três disciplinas:

- Estrutura de Dados.
- Tecnologias Web.
- Banco de Dados II.

A equipe é composta por quatro integrantes:

- Adones Gomes.
- Nicolas Guedes.
- Davi Renato.
- Eric Farias.

Ainda não existe uma divisão definitiva de todas as responsabilidades individuais. Portanto, não atribua tarefas específicas a integrantes sem confirmação.

### Funcionalidades previstas

O sistema deverá contemplar:

- Cadastro de produtos.
- Consulta de produtos pelo código.
- Atualização de informações e quantidade em estoque.
- Remoção de produtos.
- Listagem de produtos.
- Pesquisa de produtos por nome.
- Organização por categorias.
- Identificação de produtos com estoque baixo.
- Exibição de informações e detalhes dos produtos.

O escopo poderá ser ampliado conforme as orientações dos professores.

## 3. Arquitetura e tecnologias

Documente a arquitetura atualmente planejada:

### Frontend

- HTML.
- CSS.
- JavaScript puro (Vanilla JavaScript).

### Backend

- Java.
- Spring Boot como infraestrutura para disponibilização da API REST.
- Maven para gerenciamento de dependências e build.

A intenção é manter a lógica de negócio e as estruturas de dados implementadas em Java, evitando colocar regras importantes diretamente nos controllers ou depender excessivamente de abstrações de frameworks.

### Arquitetura geral

Adotar inicialmente uma arquitetura de monólito modular.

O frontend deverá se comunicar com o backend por meio de requisições HTTP e troca de dados em JSON.

O backend será responsável por coordenar as regras de negócio, estruturas de dados, persistência e integrações externas.

Não adotar microserviços nesta etapa.

Documente:

- Responsabilidade do frontend.
- Responsabilidade do backend.
- Organização em módulos.
- Comunicação entre as camadas.
- Fluxo geral de uma operação, desde a interface até a persistência.
- Justificativa para a escolha de um monólito modular.

Não crie código nem diagramas que dependam de decisões ainda não tomadas.

## 4. Documentação de Estrutura de Dados

Preencha docs/structures/README.md com uma explicação técnica detalhada das estruturas de dados previstas.

### Tabela Hash

A estrutura Hash será utilizada para localizar produtos rapidamente pelo código.

Documente:

- Objetivo da estrutura no projeto.
- Funcionamento de uma tabela hash.
- Utilização do código do produto como chave.
- Função hash e tratamento de colisões.
- Estratégia de encadeamento separado.
- Inserção, busca e remoção.
- Complexidade média e pior caso.
- Justificativa da escolha.

### Árvore Binária de Busca (ABB)

A ABB será utilizada para organizar os produtos pelo código e permitir a listagem ordenada.

Documente:

- Conceito de árvore binária de busca.
- Organização dos nós.
- Inserção.
- Busca.
- Remoção.
- Percurso em ordem (in-order).
- Complexidade média e pior caso.
- Justificativa da escolha.

Não utilizar árvore AVL ou Rubro-Negra nesta proposta inicial. A estrutura planejada é uma ABB convencional, sem balanceamento automático.

### Integração entre as estruturas

Explique que a Tabela Hash e a ABB deverão trabalhar sobre os mesmos objetos de produto, evitando manter cópias independentes que possam ficar inconsistentes.

Documente também o papel previsto de um componente coordenador, chamado provisoriamente de EstoqueManager, responsável por manter as operações sincronizadas entre as duas estruturas.

Apresente exemplos conceituais de:

- Cadastro de produto.
- Consulta pelo código.
- Atualização de estoque.
- Remoção.
- Listagem ordenada.
- Identificação de estoque baixo.

Não escreva código Java.

## 5. Documentação de Banco de Dados

Preencha docs/database/README.md com a proposta de utilização dos bancos de dados no projeto.

ATENÇÃO: as especificações oficiais do professor de Banco de Dados II ainda não foram disponibilizadas. Portanto, a distribuição abaixo é uma proposta inicial de arquitetura, não uma exigência oficial da disciplina.

A estratégia é começar com Riak e MongoDB. Cassandra e Neo4j serão considerados em uma etapa posterior, conforme a orientação do professor e a viabilidade do projeto.

### Riak KV

Uso inicialmente proposto: armazenamento de registros brutos importados da API DummyJSON.

Documente:

- Conceito de banco de dados chave-valor.
- Funcionamento básico do Riak KV.
- Organização dos registros por chaves.
- Exemplo conceitual de chave: dummyjson:product:1.
- Operações de gravação, leitura, atualização e remoção.
- Motivo para utilizar Riak no projeto.
- Papel do Riak na integração com os dados externos.

Esclareça que os registros brutos armazenados no Riak não serão, por definição, a fonte principal do catálogo operacional.

### MongoDB

Uso inicialmente proposto: persistência principal do catálogo operacional de produtos.

Documente:

- Conceito de banco de dados orientado a documentos.
- Estrutura de documentos JSON/BSON.
- Organização em coleções.
- Possibilidade de armazenar informações flexíveis sobre produtos.
- Informações de preço, descrição, categoria, marca, imagem e estoque.
- Possibilidade de expansão para avaliações e comentários, caso o escopo exija.
- Motivo para utilizar MongoDB no catálogo.

Deixe clara a diferença entre o papel do MongoDB e o papel do Riak.

### Cassandra

Uso futuro proposto: histórico de movimentações de estoque.

Documente conceitualmente:

- Banco de dados NoSQL orientado a colunas largas.
- Distribuição e escalabilidade.
- Registro de entradas, saídas e alterações de estoque.
- Possibilidade de consultas por produto e período.
- Justificativa da proposta.

Marque sua implementação como futura e dependente da especificação da disciplina.

### Neo4j

Uso futuro proposto: representação de relacionamentos entre produtos.

Documente conceitualmente:

- Banco de dados orientado a grafos.
- Nós e relacionamentos.
- Produtos, categorias e marcas como possíveis entidades.
- Relações como pertencimento a categorias e associação entre produtos.
- Possibilidade de recomendar produtos relacionados.
- Justificativa da proposta.

Marque sua implementação como futura e dependente da especificação da disciplina.

### Integração com DummyJSON

Documente a utilização prevista da API pública:

https://dummyjson.com/docs/products

Explique:

- Objetivo de utilizar dados externos para popular inicialmente o catálogo.
- Processo conceitual de importação pelo backend Java.
- Transformação dos dados externos para o modelo interno do AmazOFF.
- Armazenamento dos registros brutos no Riak, conforme a proposta.
- Persistência dos dados operacionais no MongoDB.
- O backend deve disponibilizar os dados para o frontend sem depender de uma consulta externa à DummyJSON a cada carregamento do catálogo.

Não implemente integração ou escreva código.

### Visão geral da distribuição

Inclua uma tabela comparando os quatro bancos:

| Banco | Modelo | Responsabilidade proposta | Etapa |
| Riak KV | Chave-valor | Registros brutos importados | Inicial |
| MongoDB | Documentos | Catálogo operacional | Inicial |
| Cassandra | Colunas largas | Histórico de estoque | Futura |
| Neo4j | Grafos | Relacionamentos entre produtos | Futura |

Finalize com uma seção de decisões pendentes para a disciplina Banco de Dados II.

## 6. Backlog e planejamento do desenvolvimento

Preencha docs/backlog/README.md com um backlog de desenvolvimento que possa ser utilizado pela equipe durante todo o projeto.

Não quero apenas uma lista genérica de tarefas. Quero uma estrutura que permita entender o que precisa ser feito, em qual ordem, por que precisa ser feito e quais atividades dependem de outras.

### Organização do backlog

Divida o trabalho em épicos:

Épico 1 — Planejamento e arquitetura

- Consolidar requisitos.
- Definir arquitetura.
- Definir tecnologias.
- Organizar repositório.
- Definir responsabilidades da equipe.

Épico 2 — Estrutura de Dados

- Implementar Tabela Hash.
- Implementar Árvore Binária de Busca.
- Implementar operações de sincronização.
- Implementar gerenciamento de produtos.
- Criar testes das estruturas.
- Validar complexidades e resultados.

Épico 3 — Backend e Tecnologias Web

- Preparar projeto Java/Spring Boot.
- Definir modelo de produto.
- Organizar camadas da aplicação.
- Criar serviços de negócio.
- Disponibilizar endpoints REST.
- Integrar backend e frontend.
- Tratar erros e validações.

Épico 4 — Integração e persistência inicial

- Preparar ambiente Riak.
- Implementar persistência no Riak.
- Preparar ambiente MongoDB.
- Implementar persistência no MongoDB.
- Planejar importação DummyJSON.
- Testar fluxo de importação e consulta.

Épico 5 — Interface web

- Organizar HTML, CSS e JavaScript.
- Criar catálogo.
- Criar visualização de detalhes.
- Criar pesquisa e filtros.
- Criar funcionalidades de gerenciamento de produtos.
- Integrar interface com backend.

Épico 6 — Banco de Dados II: expansão

- Analisar as exigências oficiais do professor.
- Avaliar a utilização de Cassandra.
- Avaliar a utilização de Neo4j.
- Definir responsabilidades definitivas para cada banco.
- Implementar somente o que for efetivamente solicitado e viável.

Épico 7 — Testes e finalização

- Testes unitários.
- Testes de integração.
- Testes das operações de estoque.
- Testes da comunicação frontend/backend.
- Revisão da documentação.
- Preparação da demonstração acadêmica.

### Detalhamento das tarefas

Para cada tarefa, utilize um padrão consistente com:

- Identificador: exemplo ED-001, WEB-001, BD-001.
- Nome da tarefa.
- Descrição objetiva.
- Disciplina relacionada.
- Prioridade: alta, média ou baixa.
- Complexidade estimada: pequena, média ou grande.
- Dependências.
- Critério de conclusão (Definition of Done).
- Status inicial: Pendente.

Use identificadores que permitam reconhecer a disciplina:

- ED para Estrutura de Dados.
- WEB para Tecnologias Web.
- BD para Banco de Dados II.
- INT para tarefas de integração entre disciplinas.

### Organização por fases

Organize as tarefas em uma sequência lógica:

1. Planejamento e preparação.
2. Estruturas de dados.
3. Backend e persistência inicial.
4. Frontend e integração.
5. Expansão de Banco de Dados II.
6. Testes e entrega.

Identifique dependências entre tarefas. Por exemplo, a integração da interface depende de endpoints disponíveis; os testes de persistência dependem das respectivas integrações.

### Planejamento da equipe

A equipe possui quatro integrantes, mas a divisão individual das atividades ainda não está fechada.

Portanto:

- Não atribua tarefas nominalmente.
- Crie um campo de responsável ainda não preenchido.
- Estruture as tarefas para que possam ser distribuídas posteriormente.
- Permita que atividades de diferentes disciplinas sejam acompanhadas separadamente.
- Não presuma que todos os integrantes estão na mesma disciplina.

### Controle do backlog

Inclua uma legenda de status:

- Pendente.
- Em andamento.
- Em revisão.
- Concluído.
- Bloqueado.

Inclua também uma explicação de como atualizar o backlog durante o desenvolvimento.

Não estabeleça datas fictícias, duração obrigatória ou cronograma fechado. A equipe ainda precisa definir o prazo e distribuir as atividades.

O backlog deve ser prático e utilizável, não apenas uma descrição acadêmica de metodologias ágeis.

## 7. README principal

Atualize o README.md da raiz para funcionar como a página inicial de documentação do projeto.

Ele deve conter:

1. Nome do projeto.
2. Descrição do AmazOFF.
3. Objetivo acadêmico.
4. Disciplinas envolvidas.
5. Funcionalidades previstas.
6. Tecnologias utilizadas.
7. Visão geral da arquitetura.
8. Visão geral das estruturas de dados.
9. Visão geral dos bancos de dados.
10. Integrantes da equipe.
11. Estrutura de diretórios.
12. Links relativos para os documentos técnicos.

Os links devem apontar para:

- docs/architecture/README.md
- docs/structures/README.md
- docs/database/README.md
- docs/backlog/README.md
- docs/api/README.md

Não invente informações pessoais ou atribuições individuais.

## 8. Documentação de arquitetura

Preencha docs/architecture/README.md com:

- Visão geral da arquitetura.
- Objetivo de cada módulo.
- Separação entre frontend e backend.
- Responsabilidades das camadas Java.
- Organização das estruturas de dados.
- Integração com os bancos.
- Fluxo geral das informações.
- Decisões arquiteturais tomadas.
- Decisões ainda pendentes.

A arquitetura deve refletir as decisões descritas neste documento e nos demais arquivos.

## 9. Documentação da API

Por enquanto, não desenvolva a documentação detalhada dos endpoints em docs/api/README.md.

Mantenha esse arquivo apenas com uma breve descrição do papel da API REST, da comunicação HTTP/JSON e de que sua especificação detalhada será preenchida quando o contrato da API for efetivamente definido.

Não invente endpoints definitivos.

## 10. Arquivo de prompts e apoio de IA

Preencha docs/AI_PROMPTS.md com orientações para utilização de ferramentas de IA durante o desenvolvimento.

Inclua:

- Contexto resumido do projeto.
- Arquitetura e tecnologias previstas.
- Orientação para não modificar estruturas ou decisões sem autorização da equipe.
- Orientação para não implementar funcionalidades fora do escopo.
- Recomendações para solicitar código, testes, revisões e documentação.
- Orientação para manter os documentos atualizados quando decisões forem alteradas.

Não inclua prompts genéricos sem relação com o AmazOFF.

## 11. Padrão de qualidade da documentação

Todos os documentos devem:

- Ser escritos em português brasileiro.
- Utilizar Markdown bem organizado.
- Ter títulos e subtítulos hierárquicos.
- Apresentar tabelas quando realmente facilitarem a leitura.
- Utilizar exemplos relacionados ao AmazOFF.
- Evitar repetições desnecessárias entre arquivos.
- Utilizar linguagem técnica, mas compreensível para estudantes.
- Diferenciar decisões confirmadas, propostas e pendências.
- Ser consistentes entre si.

Não escreva textos superficiais apenas para preencher espaço. Cada seção precisa explicar algo útil para a equipe.

## 12. Restrições obrigatórias

NÃO FAÇA:

- Não implemente código Java.
- Não implemente HTML, CSS ou JavaScript.
- Não crie controllers, services, repositories ou classes.
- Não implemente Tabela Hash ou ABB.
- Não implemente integrações com bancos.
- Não crie endpoints.
- Não configure Docker.
- Não instale dependências.
- Não altere pom.xml.
- Não modifique configurações da aplicação.
- Não exclua arquivos existentes.
- Não faça commits ou pushes.

Você está autorizado a modificar os arquivos Markdown de documentação e criar arquivos Markdown que estejam faltando, além de criar diretórios de documentação quando necessário.

## 13. Verificação final

Ao terminar:

1. Revise todos os documentos criados e atualizados.
2. Verifique se os links relativos funcionam.
3. Verifique se as decisões de arquitetura são consistentes em todos os documentos.
4. Verifique se as propostas de Banco de Dados II estão identificadas como provisórias.
5. Verifique se o backlog contém tarefas organizadas, dependências e critérios de conclusão.
6. Verifique se não foram atribuídas tarefas individuais sem autorização.
7. Verifique se nenhum código da aplicação foi criado ou modificado.

Ao final, apresente um resumo contendo:

- Arquivos atualizados.
- Arquivos criados.
- Principais conteúdos documentados.
- Quantidade de tarefas incluídas no backlog.
- Decisões que ainda precisam ser tomadas pela equipe.
- Confirmação explícita de que nenhuma funcionalidade ou código da aplicação foi implementado.

Objetivo final: deixar o repositório AmazOFF com uma documentação técnica consistente, que sirva como referência oficial de planejamento para os quatro integrantes antes do início da implementação.



# Tarefa: Documentar os requisitos oficiais do projeto AmazOFF no README principal

Você deverá analisar o repositório atual do AmazOFF e atualizar seu README principal (`README.md`), organizando e documentando os requisitos funcionais, não funcionais, regras de negócio e requisitos estruturais do projeto.

## 1. Objetivo

Produzir uma documentação clara, organizada e coerente com as especificações acadêmicas do AmazOFF, consolidando os requisitos das disciplinas de:

- Tecnologias Web;
- Estrutura de Dados;
- Banco de Dados II.

O README deverá funcionar como referência central dos requisitos do projeto para toda a equipe.

## 2. Instruções obrigatórias antes de editar

1. Analise a estrutura atual do repositório.
2. Localize o README principal.
3. Leia o conteúdo existente e preserve as informações que ainda forem pertinentes.
4. Identifique a estrutura atual da aplicação e documente somente tecnologias e decisões que estejam confirmadas no projeto ou nas especificações fornecidas.
5. Não invente funcionalidades, tecnologias, regras ou decisões arquiteturais.

## 3. Requisitos funcionais

Crie uma seção chamada **Requisitos Funcionais (RF)** e documente os seguintes requisitos:

### Gerenciamento de produtos

- RF-001: Cadastrar novos produtos.
- RF-002: Consultar produtos pelo código.
- RF-003: Editar informações de produtos cadastrados.
- RF-004: Excluir produtos cadastrados.
- RF-005: Atualizar a quantidade disponível em estoque.
- RF-006: Listar produtos em ordem crescente de código.
- RF-007: Identificar produtos com estoque baixo.

### Catálogo e consulta

- RF-008: Exibir os produtos cadastrados em formato de catálogo.
- RF-009: Apresentar imagem, nome, preço, categoria e marca no catálogo.
- RF-010: Permitir acessar os detalhes de cada produto.
- RF-011: Exibir as informações completas do produto selecionado.
- RF-012: Pesquisar produtos pelo nome.
- RF-013: Filtrar produtos por categoria.

### Avaliações e comentários

- RF-014: Permitir atribuir notas aos produtos.
- RF-015: Permitir adicionar comentários sobre os produtos.
- RF-016: Exibir as avaliações cadastradas.
- RF-017: Calcular e apresentar a média das avaliações de cada produto.

## 4. Requisitos não funcionais

Crie uma seção chamada **Requisitos Não Funcionais (RNF)**.

Documente:

- RNF-001: Na primeira etapa de desenvolvimento, utilizar JavaScript com arrays e objetos para armazenar e manipular registros em memória.
- RNF-002: As operações deverão atualizar a interface dinamicamente.
- RNF-003: A especificação inicial de Tecnologias Web não exige banco de dados, autenticação ou backend.
- RNF-004: A solução deverá permitir evolução e integração com backend e persistência em banco de dados nas etapas posteriores.

Não interprete a ausência de banco de dados na primeira etapa como uma proibição permanente de integração com bancos de dados.

## 5. Requisitos estruturais de Estrutura de Dados

Crie uma seção própria chamada **Requisitos Estruturais e Técnicos**.

Documente:

- RT-001: Utilizar uma Árvore Binária de Busca (ABB) para organizar os produtos.
- RT-002: Utilizar a ABB para realizar listagens em ordem crescente de código.
- RT-003: Utilizar uma tabela hash para permitir consultas de produtos pelo código.

Deixe claro que essas estruturas são exigências da atividade de Estrutura de Dados.

Não determine arbitrariamente técnicas de balanceamento, tratamento de colisões ou algoritmos específicos que não tenham sido aprovados.

## 6. Regras de negócio

Crie uma seção chamada **Regras de Negócio (RN)**.

Documente:

- RN-001: Cada produto deverá possuir informações que permitam sua identificação e consulta.
- RN-002: A quantidade disponível em estoque deverá ser mantida como informação do produto.
- RN-003: Avaliações e comentários deverão estar associados aos respectivos produtos.
- RN-004: A média apresentada deverá representar as notas recebidas pelo produto.
- RN-005: A listagem exigida pela atividade de Estrutura de Dados deverá seguir a ordem crescente do código.

Não invente regras adicionais.

## 7. Evolução e integração entre disciplinas

Crie uma seção chamada **Evolução do Sistema e Integração entre Disciplinas**.

Explique que o desenvolvimento acontece em etapas complementares:

### Etapa 1 — Tecnologias Web

- Desenvolvimento da aplicação web.
- Gerenciamento de produtos.
- Catálogo e detalhes dos produtos.
- Pesquisa e filtros.
- Avaliações e comentários.
- Armazenamento inicial em memória utilizando arrays e objetos JavaScript.
- Atualização dinâmica da interface.

### Etapa 2 — Estrutura de Dados

- Organização dos produtos por meio de ABB.
- Consultas por código utilizando tabela hash.
- Listagem ordenada dos produtos.

### Etapa 3 — Banco de Dados II

- Evolução da aplicação para persistência de dados.
- Integração com backend.
- Integração com os bancos de dados definidos para o projeto, respeitando as decisões técnicas aprovadas pela equipe e pela disciplina.

**Importante:** o armazenamento em memória é uma etapa inicial do projeto, não a arquitetura definitiva de persistência.

Não invente tecnologias ou bancos específicos caso não estejam confirmados no repositório ou na documentação existente.

## 8. Modelo de dados

Documente os atributos de produto previstos na especificação Web:

- id;
- name;
- description;
- price;
- category;
- brand;
- image;
- stock;
- rating;
- evaluations;
- createdAt.

Cada avaliação poderá conter:

- id;
- score;
- comment;
- createdAt.

Explique que esse modelo representa a referência inicial da especificação Web e poderá ser adaptado durante a integração entre as disciplinas.

Não remova o requisito de consulta por código da disciplina de Estrutura de Dados.

## 9. Pontos que precisam permanecer pendentes

Crie uma seção chamada **Definições Pendentes** e registre:

1. Definir o limite numérico que caracteriza estoque baixo.
2. Definir a relação entre o campo `id` da especificação Web e o código utilizado em Estrutura de Dados.
3. Definir a escala permitida para as avaliações dos produtos.

Não escolha valores arbitrários para essas definições.

## 10. Organização do README

Organize o documento com uma hierarquia clara de títulos e subtítulos.

Sugestão de estrutura:

1. Apresentação do AmazOFF.
2. Objetivo do projeto.
3. Escopo.
4. Requisitos Funcionais.
5. Requisitos Não Funcionais.
6. Requisitos Estruturais e Técnicos.
7. Regras de Negócio.
8. Modelo de Dados.
9. Evolução do Sistema e Integração entre Disciplinas.
10. Definições Pendentes.

Adapte a estrutura se o README já possuir seções úteis.

## 11. Restrições de execução

- Não altere arquivos de código.
- Não crie classes, funções ou componentes.
- Não instale dependências.
- Não altere configurações.
- Não modifique a arquitetura da aplicação.
- Não exclua informações válidas do README.
- Não invente tecnologias, funcionalidades ou decisões.
- Não implemente os requisitos: apenas documente-os.

## 12. Verificação final

Após editar o README:

1. Confira se todos os requisitos estão documentados.
2. Verifique se os identificadores RF, RNF, RT e RN estão consistentes.
3. Confira se não existem requisitos duplicados.
4. Verifique se as três etapas de evolução estão corretamente descritas.
5. Confirme que nenhum arquivo de implementação foi alterado.

Ao finalizar, apresente um resumo contendo:
- Arquivo atualizado.
- Seções criadas ou modificadas.
- Quantidade de requisitos documentados por categoria.
- Definições que permaneceram pendentes.
- Confirmação de que nenhum código-fonte foi alterado.