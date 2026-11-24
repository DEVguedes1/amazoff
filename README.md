# AmazOFF

O **AmazOFF** é um sistema acadêmico de gerenciamento de produtos e estoque, com interface web para consulta e administração de um catálogo. O projeto integra conteúdos de Estrutura de Dados, Tecnologias Web e Banco de Dados II. Esta documentação orienta o planejamento e o desenvolvimento; funcionalidades, integrações e propostas acadêmicas ainda precisam ser implementadas e validadas conforme indicado em cada documento.

## Objetivo acadêmico

Planejar e desenvolver uma aplicação que permita gerenciar produtos e estoque enquanto demonstra estruturas de dados implementadas em Java, comunicação entre frontend e backend e persistência NoSQL. O projeto também permite estudar como diferentes modelos de banco podem apoiar necessidades distintas, segundo as orientações oficiais da disciplina.

## Disciplinas

- **Estrutura de Dados:** Tabela Hash com encadeamento separado, Árvore Binária de Busca convencional, análise de complexidade e consistência entre estruturas.
- **Tecnologias Web:** interface em HTML, CSS e JavaScript Vanilla; backend Java/Spring Boot; API REST e comunicação HTTP/JSON.
- **Banco de Dados II:** tecnologias consideradas incluem Riak KV e MongoDB inicialmente, e Cassandra e Neo4j como possibilidades futuras. A distribuição é **PROPOSTA**, pois a especificação do professor ainda está pendente.

## Funcionalidades previstas

- Cadastrar, consultar pelo código, atualizar e remover produtos.
- Listar produtos e pesquisar por nome.
- Organizar e filtrar produtos por categoria.
- Identificar produtos com estoque baixo.
- Exibir informações e detalhes dos produtos.

O escopo poderá ser ampliado conforme as orientações dos professores. Avaliações e comentários são possibilidades complementares ainda não confirmadas.

## Tecnologias e arquitetura

- **Frontend:** HTML, CSS e JavaScript puro (Vanilla JavaScript).
- **Backend:** Java, Spring Boot para disponibilização da API REST e Maven para dependências e build.
- **Organização:** monólito modular, com módulos de responsabilidade clara dentro de uma aplicação backend. Não estão previstos microserviços nesta etapa.
- **Comunicação:** requisições HTTP e troca de dados JSON entre frontend e backend.

O frontend apresenta catálogo e controles, envia solicitações à API e mostra resultados. O backend coordena regras de negócio, estruturas de dados, persistência e integrações externas. A documentação de [Arquitetura](docs/architecture/README.md) descreve camadas, fluxo e pendências.

## Estruturas de dados

A proposta usa uma Tabela Hash indexada pelo código para busca rápida e uma ABB convencional, sem balanceamento automático, para ordenar produtos por código. Ambas deverão manter referências aos mesmos objetos de produto. Um componente coordenador chamado provisoriamente `EstoqueManager` deverá sincronizar operações entre elas. Consulte a documentação detalhada em [Estruturas de Dados](docs/structures/README.md).

## Bancos de dados

Como proposta inicial, Riak KV poderá guardar registros brutos importados da DummyJSON e MongoDB persistir o catálogo operacional transformado. Cassandra para histórico de estoque e Neo4j para relacionamentos ficam para avaliação futura. Essas responsabilidades dependem da especificação oficial de Banco de Dados II; veja [Banco de Dados](docs/database/README.md).

## Equipe

- Adones Gomes
- Nicolas Guedes
- Davi Renato
- Eric Farias

A divisão individual de atividades ainda não está definida e não é presumida nesta documentação.

## Estrutura do repositório

```text
.
├── frontend/
│   ├── assets/
│   ├── css/
│   └── js/
├── backend/
│   └── src/
│       ├── main/java/com/amazoff/
│       │   ├── controller/  model/  dto/  service/
│       │   ├── structures/hash/  structures/tree/
│       │   ├── repository/  integration/
│       │   └── config/  exception/
│       ├── main/resources/
│       └── test/java/com/amazoff/
├── docs/
│   ├── architecture/  api/  database/  structures/  backlog/
│   ├── AI_PROMPTS.md
│   └── MODELAGEM_MERMAID.md
├── docker/
└── amazoff-backend/  # scaffold Spring Boot existente no repositório
```

As pastas de `frontend/`, `backend/` e `docker/` preparadas para o desenvolvimento podem conter apenas `.gitkeep`. A árvore `amazoff-backend/` já existe e contém um scaffold Spring Boot/Maven e uma classe de inicialização gerada; isso, por si só, não indica que funcionalidades do AmazOFF estejam implementadas. A equipe deverá decidir como esse scaffold se relaciona com a estrutura-alvo `backend/`. Os arquivos existentes foram preservados.

## Documentação técnica

- [Arquitetura](docs/architecture/README.md) — módulos, fluxo e decisões arquiteturais.
- [Estruturas de Dados](docs/structures/README.md) — Hash, ABB, operações e complexidade.
- [Banco de Dados](docs/database/README.md) — papéis propostos para os bancos e DummyJSON.
- [Backlog](docs/backlog/README.md) — épicos, tarefas, dependências e acompanhamento.
- [API](docs/api/README.md) — papel da API e estado da definição do contrato.
- [Diagramas Mermaid](docs/MODELAGEM_MERMAID.md) — representações conceituais de arquitetura e dados.
- [Orientações para IA](docs/AI_PROMPTS.md) — cuidados para apoiar o desenvolvimento com ferramentas de IA.

## Estado e decisões pendentes

As funcionalidades descritas são planejadas. O contrato da API, as responsabilidades oficiais dos bancos, a estratégia de sincronização entre estruturas e persistência, o escopo de avaliações/comentários, requisitos de implantação e a distribuição das tarefas ainda precisam ser definidos. O backlog não estabelece prazos; responsáveis devem ser combinados pela equipe.
