# Backlog de desenvolvimento

Este backlog organiza o trabalho planejado por épicos e fases, com dependências explícitas para apoiar distribuição e acompanhamento. Ele é uma base editável: **não é cronograma**, não estima duração e não atribui tarefas a pessoas. O escopo de Banco de Dados II deve ser revisto quando o professor fornecer as especificações oficiais. As tarefas refletem o planejamento atual e não declaram implementação concluída.

## Como ler e atualizar

Cada item apresenta identificador, descrição/resultado esperado, disciplina, prioridade, complexidade, dependências, critério de conclusão, responsável e status. `—` na coluna de dependências significa que o item não depende de outro item deste backlog. Dependência indica ordem lógica, não prazo.

Antes de iniciar uma tarefa, a equipe deve confirmar seu escopo, preencher **Responsável** por acordo entre os integrantes e alterar o status para **Em andamento**. Ao concluir o critério de aceite, marcar **Em revisão**; após revisão da equipe, marcar **Concluído**. Se houver impedimento, marcar **Bloqueado** e registrar no próprio item o motivo e o que o desbloqueia. Se requisito mudar, ajustar o item e seus dependentes e atualizar os documentos relacionados. Revisar dependências ao mover tarefas entre fases.

Todos os itens começam com status **Pendente** e responsável não atribuído. Prioridade expressa importância relativa para organizar o trabalho, não compromisso de prazo. Complexidade é uma estimativa inicial relativa, a revisar pela equipe antes de iniciar.

### Legenda

| Campo | Valores / significado |
|---|---|
| Status | Pendente; Em andamento; Em revisão; Concluído; Bloqueado |
| Prioridade | Alta, média ou baixa; ordenação relativa, sujeita a revisão |
| Complexidade | Pequena, média ou grande; estimativa inicial, não duração |
| Responsável | Em branco até distribuição acordada pela equipe |

## Épico 1 — Planejamento e arquitetura

**Fase 1 — Planejamento e preparação.** Primeiro consolida-se o que foi solicitado pelas disciplinas e as decisões necessárias. O detalhamento do Banco de Dados II aguarda a especificação do professor.

| ID | Tarefa e descrição | Disciplina | Prioridade | Complexidade | Dependências | Critério de conclusão (DoD) | Responsável | Status |
|---|---|---|---|---|---|---|---|---|
| INT-001 | Consolidar requisitos conhecidos e separar requisitos confirmados de propostas e pendências. | Integração | Alta | Média | — | Requisitos funcionais e acadêmicos registrados; dúvidas explicitamente marcadas para validação. |  | Pendente |
| INT-002 | Confirmar a arquitetura modular e fronteiras entre frontend, backend, estruturas e persistência. | Integração | Alta | Média | INT-001 | Decisões revisadas pela equipe e refletidas em `docs/architecture/README.md`. |  | Pendente |
| WEB-001 | Confirmar tecnologias e organização: HTML/CSS/JS Vanilla, Java, Spring Boot, Maven e monólito modular. | Tecnologias Web | Alta | Pequena | INT-001 | Stack e limites registrados sem adotar microserviços ou decisões não aprovadas. |  | Pendente |
| INT-003 | Organizar repositório e alinhar o scaffold `amazoff-backend/` com a estrutura-alvo `backend/`. | Integração | Média | Média | INT-002, WEB-001 | Equipe decide e documenta qual diretório será fonte do projeto e como preservar artefatos existentes. |  | Pendente |
| INT-004 | Definir responsabilidades individuais e forma de acompanhamento das tarefas. | Integração | Média | Pequena | INT-001 | Distribuição acordada pelos integrantes e registrada no backlog, sem pressupostos sobre disciplinas. |  | Pendente |
| BD-001 | Obter e analisar a especificação oficial do professor de Banco de Dados II. | Banco de Dados II | Alta | Média | INT-001 | Requisitos oficiais registrados e propostas de banco revistas à luz deles. |  | Pendente |

## Épico 2 — Estrutura de Dados

**Fase 2 — Estruturas de dados.** A especificação de chave, comparação e regras de consistência antecede as implementações. Testes e análise devem acompanhar cada estrutura e a coordenação entre elas.

| ID | Tarefa e descrição | Disciplina | Prioridade | Complexidade | Dependências | Critério de conclusão (DoD) | Responsável | Status |
|---|---|---|---|---|---|---|---|---|
| ED-001 | Definir unicidade, comparação e normalização do código de produto para Hash e ABB. | Estrutura de Dados | Alta | Pequena | INT-001 | Regra de chave documentada e igual nas duas estruturas. |  | Pendente |
| ED-002 | Especificar função hash, capacidade, fator de carga e política de redimensionamento. | Estrutura de Dados | Média | Média | ED-001 | Decisões justificadas e casos de colisão definidos em documentação técnica. |  | Pendente |
| ED-003 | Implementar Tabela Hash com encadeamento separado e operações de inserção, busca, atualização e remoção. | Estrutura de Dados | Alta | Grande | ED-002 | Operações obedecem chave e política definidas; colisões preservam todas as entradas distintas. |  | Pendente |
| ED-004 | Criar testes das operações e colisões da Tabela Hash. | Estrutura de Dados | Alta | Média | ED-003 | Testes cobrem chave ausente, duplicidade, colisão, atualização e remoção, com resultados reproduzíveis. |  | Pendente |
| ED-005 | Implementar ABB convencional por código, sem balanceamento automático, com inserção, busca e remoção. | Estrutura de Dados | Alta | Grande | ED-001 | Casos de remoção (folha, um filho, dois filhos) preservam a propriedade da ABB. |  | Pendente |
| ED-006 | Implementar e validar percurso *in-order* para listagem crescente. | Estrutura de Dados | Alta | Média | ED-005 | Percurso visita cada produto uma vez e devolve códigos em ordem crescente em casos variados. |  | Pendente |
| ED-007 | Criar testes de operações, remoção e ordenação da ABB. | Estrutura de Dados | Alta | Média | ED-005, ED-006 | Casos vazios, único nó, árvore degenerada e remoções validam invariantes e resultados. |  | Pendente |
| ED-008 | Implementar coordenação entre Hash e ABB usando referências aos mesmos objetos de produto. | Estrutura de Dados | Alta | Grande | ED-003, ED-005, ED-001 | Cadastro e remoção deixam as estruturas coerentes; atualização de estoque reflete no objeto compartilhado. |  | Pendente |
| ED-009 | Definir e testar comportamento diante de falha parcial e alteração do código do produto. | Estrutura de Dados | Média | Grande | ED-008 | Política documentada e testes demonstram que não há divergência silenciosa entre estruturas. |  | Pendente |
| ED-010 | Validar complexidades e explicar resultados observados contra a análise teórica. | Estrutura de Dados | Média | Média | ED-004, ED-007 | Documentação distingue média/pior caso da Hash e altura balanceada/pior caso da ABB; resultados são reproduzíveis. |  | Pendente |

## Épico 3 — Backend e Tecnologias Web

**Fase 3 — Backend e persistência inicial.** A API depende de decisões de arquitetura, modelo e regras de produto. Os endpoints serão definidos pela equipe antes de sua implementação.

| ID | Tarefa e descrição | Disciplina | Prioridade | Complexidade | Dependências | Critério de conclusão (DoD) | Responsável | Status |
|---|---|---|---|---|---|---|---|---|
| WEB-002 | Preparar projeto Java/Spring Boot com Maven na localização decidida para o backend. | Tecnologias Web | Alta | Média | INT-003, WEB-001 | Projeto compila no ambiente acordado e segue a estrutura aprovada, sem configuração de ambiente não documentada. |  | Pendente |
| WEB-003 | Definir modelo de produto e formatos de entrada/saída (DTOs), incluindo tipos e validações. | Tecnologias Web | Alta | Média | INT-001, WEB-001 | Modelo revisado pelas disciplinas e regras de campos obrigatórios/limites documentadas. |  | Pendente |
| WEB-004 | Organizar módulos/camadas do backend para entrada HTTP, serviços, estruturas, persistência e integrações. | Tecnologias Web | Alta | Média | INT-002, WEB-002 | Responsabilidades separadas e regras centrais fora dos controllers, conforme arquitetura aprovada. |  | Pendente |
| WEB-005 | Implementar serviços de negócio para operações acordadas de produto e estoque. | Tecnologias Web | Alta | Grande | WEB-003, WEB-004, ED-008 | Serviço coordena regras e estruturas; cenários funcionais acordados têm resultados verificáveis. |  | Pendente |
| WEB-006 | Definir contrato REST, incluindo recursos, métodos, JSON, validações e respostas de erro. | Tecnologias Web | Alta | Média | WEB-003, INT-001 | Contrato revisado e registrado em `docs/api/README.md` antes de congelar os endpoints. |  | Pendente |
| WEB-007 | Disponibilizar endpoints REST do contrato aprovado. | Tecnologias Web | Alta | Grande | WEB-005, WEB-006 | API implementa apenas operações aprovadas e documenta respostas e códigos HTTP observáveis. |  | Pendente |
| WEB-008 | Tratar validações de entrada e erros de negócio de modo consistente na API. | Tecnologias Web | Alta | Média | WEB-006, WEB-007 | Entradas inválidas e recursos ausentes retornam respostas consistentes, sem expor detalhes internos indevidos. |  | Pendente |

## Épico 4 — Integração e persistência inicial

**Fase 3 — Backend e persistência inicial.** A sequência e o escopo dependem da confirmação oficial de Banco de Dados II. A proposta atual começa com Riak e MongoDB.

| ID | Tarefa e descrição | Disciplina | Prioridade | Complexidade | Dependências | Critério de conclusão (DoD) | Responsável | Status |
|---|---|---|---|---|---|---|---|---|
| BD-002 | Definir e preparar ambiente Riak conforme requisitos confirmados e recursos disponíveis. | Banco de Dados II | Alta | Média | BD-001, INT-002 | Ambiente reproduzível e instruções de execução/acesso documentadas sem credenciais versionadas. |  | Pendente |
| BD-003 | Implementar persistência de registros brutos no Riak, caso o papel proposto seja aprovado. | Banco de Dados II | Alta | Grande | BD-002, INT-002 | Gravação, leitura, atualização e remoção por chave são demonstradas conforme especificação aprovada. |  | Pendente |
| BD-004 | Definir e preparar ambiente MongoDB conforme requisitos confirmados e recursos disponíveis. | Banco de Dados II | Alta | Média | BD-001, INT-002 | Ambiente reproduzível e instruções documentadas sem credenciais versionadas. |  | Pendente |
| BD-005 | Implementar persistência do catálogo operacional no MongoDB, caso aprovada. | Banco de Dados II | Alta | Grande | BD-004, WEB-003 | Operações necessárias ao catálogo persistem documentos e mantêm os dados recuperáveis pela aplicação. |  | Pendente |
| BD-006 | Definir mapeamento e regras de importação entre DummyJSON e o modelo AmazOFF. | Integração | Alta | Média | BD-001, WEB-003 | Campos, identificadores, duplicidade e tratamento de campos ausentes documentados e revisados. |  | Pendente |
| INT-005 | Implementar importação backend da DummyJSON, preservando bruto no Riak e catálogo transformado no MongoDB, se aprovado. | Integração | Média | Grande | BD-003, BD-005, BD-006, WEB-005 | Importação pode ser repetida conforme política definida; frontend consulta catálogo local sem chamada externa por carregamento. |  | Pendente |
| INT-006 | Verificar fluxo integrado de importação, transformação, persistência e consulta do catálogo. | Integração | Alta | Média | INT-005, WEB-007 | Cenário de ponta a ponta confirma origem bruta, modelo operacional e resposta disponibilizada pelo backend. |  | Pendente |

## Épico 5 — Interface web

**Fase 4 — Frontend e integração.** O contrato da API deve estar acordado para que a interface não dependa de endpoints presumidos.

| ID | Tarefa e descrição | Disciplina | Prioridade | Complexidade | Dependências | Critério de conclusão (DoD) | Responsável | Status |
|---|---|---|---|---|---|---|---|---|
| WEB-009 | Organizar HTML, CSS e JavaScript Vanilla em módulos/pastas aprovados. | Tecnologias Web | Alta | Média | INT-002, WEB-001 | Estrutura da interface documentada e arquivos da aplicação seguem separação aprovada. |  | Pendente |
| WEB-010 | Criar visualização do catálogo e detalhes do produto. | Tecnologias Web | Alta | Média | WEB-009, WEB-006 | Catálogo apresenta dados recebidos e detalhes são acessíveis com estados de carregamento, vazio e erro definidos. |  | Pendente |
| WEB-011 | Criar pesquisa por nome e filtros por categoria. | Tecnologias Web | Média | Média | WEB-010, WEB-006 | Busca e filtros apresentam resultados coerentes com os critérios definidos e permitem limpar/refazer consulta. |  | Pendente |
| WEB-012 | Criar controles de gerenciamento para cadastro, atualização, remoção e consulta de produtos. | Tecnologias Web | Alta | Grande | WEB-009, WEB-006, WEB-008 | Formulários e ações apresentam confirmação ou erro e não simulam sucesso sem resposta da API. |  | Pendente |
| WEB-013 | Apresentar identificação de estoque baixo e quantidade de estoque na interface. | Tecnologias Web | Média | Média | WEB-010, WEB-005 | Estado de estoque baixo usa a regra de mínimo aprovada e é distinguível visualmente. |  | Pendente |
| INT-007 | Integrar frontend à API REST e validar fluxos de catálogo e administração. | Integração | Alta | Grande | WEB-007, WEB-008, WEB-010, WEB-011, WEB-012 | Interface consome API real por HTTP/JSON e fluxos acordados funcionam sem depender de dados simulados locais. |  | Pendente |

## Épico 6 — Banco de Dados II: expansão

**Fase 5 — Expansão de Banco de Dados II.** Nenhum banco desta fase deve ser adicionado só por constar na proposta: primeiro confirmar exigência, objetivo acadêmico e viabilidade.

| ID | Tarefa e descrição | Disciplina | Prioridade | Complexidade | Dependências | Critério de conclusão (DoD) | Responsável | Status |
|---|---|---|---|---|---|---|---|---|
| BD-007 | Avaliar requisitos oficiais e adequação de Cassandra para histórico de movimentações. | Banco de Dados II | Média | Média | BD-001 | Decisão registrada com consultas, modelo conceitual, justificativa e custo de implementação. |  | Pendente |
| BD-008 | Avaliar requisitos oficiais e adequação de Neo4j para relacionamentos entre produtos. | Banco de Dados II | Média | Média | BD-001 | Decisão registrada com entidades/relações candidatas, consultas, justificativa e custo de implementação. |  | Pendente |
| BD-009 | Definir responsabilidades definitivas e escopo dos bancos usados no projeto. | Banco de Dados II | Alta | Média | BD-001, BD-003, BD-005, BD-007, BD-008 | Matriz de bancos aprovada segundo especificação do professor e refletida na documentação. |  | Pendente |
| INT-008 | Implementar integrações futuras de Cassandra e/ou Neo4j somente se solicitadas e aprovadas. | Integração | Baixa | Grande | BD-009 | Implementação limita-se aos bancos e operações aprovados; documentação e demonstração atendem ao requisito oficial. |  | Pendente |

## Épico 7 — Testes e finalização

**Fase 6 — Testes e entrega.** Cada teste depende da funcionalidade correspondente; a preparação de demonstração depende de um fluxo integrado estável e documentação revisada.

| ID | Tarefa e descrição | Disciplina | Prioridade | Complexidade | Dependências | Critério de conclusão (DoD) | Responsável | Status |
|---|---|---|---|---|---|---|---|---|
| INT-009 | Testar integração entre serviços, estruturas de dados e persistência para operações de estoque. | Integração | Alta | Média | ED-008, WEB-005, BD-005 | Cenários de cadastro, consulta, atualização, remoção, listagem e estoque baixo têm resultados verificáveis. |  | Pendente |
| WEB-014 | Criar testes unitários para regras e serviços do backend. | Tecnologias Web | Alta | Média | WEB-005, WEB-008 | Regras relevantes têm testes reproduzíveis para sucesso, entradas inválidas e erros previstos. |  | Pendente |
| INT-010 | Criar testes de integração para persistência e importação. | Integração | Alta | Grande | BD-003, BD-005, INT-005 | Testes cobrem leitura/gravação nos bancos aprovados e fluxo de importação conforme ambientes disponíveis. |  | Pendente |
| WEB-015 | Verificar comunicação frontend/backend nos fluxos prioritários. | Tecnologias Web | Alta | Média | INT-007 | Casos de sucesso, erro HTTP e indisponibilidade do backend são tratados e registrados. |  | Pendente |
| INT-011 | Revisar documentação técnica após decisões e implementação. | Integração | Média | Média | INT-002, BD-009, WEB-006, INT-007 | README e documentos de arquitetura, estruturas, banco e API correspondem ao estado e às decisões aprovadas. |  | Pendente |
| INT-012 | Preparar demonstração acadêmica com cenários e explicações por disciplina. | Integração | Média | Média | ED-010, INT-006, INT-009, WEB-015, INT-011 | Roteiro demonstra funcionalidades acordadas, estruturas e persistência sem alegar capacidades não entregues. |  | Pendente |

## Dependências e ordem sugerida

O encadeamento macro é: **planejamento (`INT-001` a `INT-004`, `WEB-001`, `BD-001`) → Estrutura de Dados (`ED-001` a `ED-010`) e preparação do backend (`WEB-002` a `WEB-006`) → persistência e contrato aprovado (`BD-002` a `BD-006`, `WEB-007`/`WEB-008`) → interface e integração (`WEB-009` a `WEB-013`, `INT-005` a `INT-007`) → expansão opcional (`BD-007` a `BD-009`, `INT-008`) → testes, revisão e demonstração (`INT-009` a `INT-012`, `WEB-014`/`WEB-015`).**

As dependências individuais nas tabelas são a referência para execução. Algumas frentes podem ocorrer em paralelo depois de seus pré-requisitos, conforme a disponibilidade e a distribuição combinada pela equipe. Requisitos oficiais novos podem alterar a sequência.

## Distribuição da equipe

A equipe é composta por Adones Gomes, Nicolas Guedes, Davi Renato e Eric Farias. Nenhum item está atribuído nominalmente neste backlog, e não se presume que todos pertençam às mesmas disciplinas. Preencher o responsável somente após acordo da equipe e registrar mudanças no próprio item.
