# AmazOFF

## Apresentação

O **AmazOFF** é um projeto acadêmico de gerenciamento de produtos e estoque, com uma aplicação web para catálogo, consulta e administração de produtos. O trabalho integra as disciplinas de **Tecnologias Web**, **Estrutura de Dados** e **Banco de Dados II**.

Este README consolida os requisitos fornecidos para essas etapas. Ele distingue os requisitos da especificação acadêmica das tecnologias e propostas que aparecem em outros documentos ou artefatos do repositório.

## Objetivo do projeto

Desenvolver o sistema em etapas complementares: iniciar com a aplicação Web e manipulação de registros em memória, aplicar as estruturas de dados exigidas para consulta e ordenação e, posteriormente, evoluir para persistência e integração com backend conforme os requisitos de Banco de Dados II.

## Escopo

O escopo funcional inclui gerenciamento de produtos e estoque, catálogo, detalhes, pesquisa, filtros, avaliações e comentários. A especificação inicial de Tecnologias Web utiliza arrays e objetos JavaScript em memória. Isso descreve a etapa inicial e **não proíbe** a evolução posterior para backend e bancos de dados.

## Requisitos Funcionais (RF)

### Gerenciamento de produtos

| ID | Requisito |
|---|---|
| RF-001 | Cadastrar novos produtos. |
| RF-002 | Consultar produtos pelo código. |
| RF-003 | Editar informações de produtos cadastrados. |
| RF-004 | Excluir produtos cadastrados. |
| RF-005 | Atualizar a quantidade disponível em estoque. |
| RF-006 | Listar produtos em ordem crescente de código. |
| RF-007 | Identificar produtos com estoque baixo. O limite numérico está pendente de definição. |

### Catálogo e consulta

| ID | Requisito |
|---|---|
| RF-008 | Exibir os produtos cadastrados em formato de catálogo. |
| RF-009 | Apresentar imagem, nome, preço, categoria e marca no catálogo. |
| RF-010 | Permitir acessar os detalhes de cada produto. |
| RF-011 | Exibir as informações completas do produto selecionado. |
| RF-012 | Pesquisar produtos pelo nome. |
| RF-013 | Filtrar produtos por categoria. |

### Avaliações e comentários

| ID | Requisito |
|---|---|
| RF-014 | Permitir atribuir notas aos produtos. |
| RF-015 | Permitir adicionar comentários sobre os produtos. |
| RF-016 | Exibir as avaliações cadastradas. |
| RF-017 | Calcular e apresentar a média das avaliações de cada produto. |

## Requisitos Não Funcionais (RNF)

| ID | Requisito |
|---|---|
| RNF-001 | Na primeira etapa de desenvolvimento, utilizar JavaScript com arrays e objetos para armazenar e manipular registros em memória. |
| RNF-002 | As operações deverão atualizar a interface dinamicamente. |
| RNF-003 | A especificação inicial de Tecnologias Web não exige banco de dados, autenticação ou backend. |
| RNF-004 | A solução deverá permitir evolução e integração com backend e persistência em banco de dados nas etapas posteriores. |

RNF-003 delimita a primeira etapa Web; não é uma proibição permanente de backend ou persistência.

## Requisitos Estruturais e Técnicos

Os requisitos abaixo pertencem à atividade de **Estrutura de Dados** e complementam a etapa inicial de Tecnologias Web. A especificação fornecida exige as estruturas, mas não define técnicas específicas de balanceamento, colisão ou algoritmos internos.

| ID | Requisito |
|---|---|
| RT-001 | Utilizar uma Árvore Binária de Busca (ABB) para organizar os produtos. |
| RT-002 | Utilizar a ABB para realizar listagens em ordem crescente de código. |
| RT-003 | Utilizar uma tabela hash para permitir consultas de produtos pelo código. |

Não se fixa nesta seção uma técnica de balanceamento da ABB, um método de tratamento de colisões da tabela hash ou um algoritmo específico. Esses detalhes devem seguir as orientações aprovadas para a atividade.

## Regras de Negócio (RN)

| ID | Regra |
|---|---|
| RN-001 | Cada produto deverá possuir informações que permitam sua identificação e consulta. |
| RN-002 | A quantidade disponível em estoque deverá ser mantida como informação do produto. |
| RN-003 | Avaliações e comentários deverão estar associados aos respectivos produtos. |
| RN-004 | A média apresentada deverá representar as notas recebidas pelo produto. |
| RN-005 | A listagem exigida pela atividade de Estrutura de Dados deverá seguir a ordem crescente do código. |

## Modelo de Dados

O modelo abaixo representa a referência inicial da especificação Web. Os atributos estão apresentados com os nomes fornecidos, sem impor tipos ou formato de armazenamento que ainda não tenham sido definidos. O modelo poderá ser adaptado durante a integração entre as disciplinas.

### Produto

| Atributo |
|---|
| `id` |
| `name` |
| `description` |
| `price` |
| `category` |
| `brand` |
| `image` |
| `stock` |
| `rating` |
| `evaluations` |
| `createdAt` |

### Avaliação

Cada avaliação poderá conter:

| Atributo |
|---|
| `id` |
| `score` |
| `comment` |
| `createdAt` |

O campo `id` pertence ao modelo de referência Web. A consulta por **código**, exigida em Estrutura de Dados, permanece requisito independente; a relação entre `id` e código ainda precisa ser definida.

## Evolução do Sistema e Integração entre Disciplinas

O desenvolvimento está organizado em etapas complementares. O armazenamento em memória é uma etapa inicial do projeto, não a arquitetura definitiva de persistência.

### Etapa 1 — Tecnologias Web

- Desenvolver a aplicação web.
- Gerenciar produtos.
- Apresentar catálogo e detalhes dos produtos.
- Permitir pesquisa e filtros.
- Oferecer avaliações e comentários.
- Armazenar inicialmente os registros em memória utilizando arrays e objetos JavaScript.
- Atualizar a interface dinamicamente após as operações.
- A especificação inicial desta etapa não exige banco de dados, autenticação ou backend.

### Etapa 2 — Estrutura de Dados

- Organizar os produtos por meio de uma ABB.
- Consultar produtos pelo código utilizando uma tabela hash.
- Realizar listagem crescente por código utilizando a ABB.
- Seguir as orientações da atividade para detalhes internos das estruturas.

### Etapa 3 — Banco de Dados II

- Evoluir a aplicação para persistência de dados.
- Integrar a aplicação com um backend.
- Integrar com os bancos de dados definidos para o projeto, respeitando as decisões técnicas aprovadas pela equipe e pela disciplina.
- Adaptar o modelo inicial às necessidades de integração.

Os bancos Riak KV, MongoDB, Cassandra e Neo4j aparecem em [documentação complementar de Banco de Dados](docs/database/README.md) como uma proposta arquitetural anterior. Essa lista e a distribuição de responsabilidades ali descrita não são requisitos confirmados por este README; devem ser validadas com a especificação oficial e pela equipe antes de adoção.

## Definições Pendentes

1. Definir o limite numérico que caracteriza estoque baixo.
2. Definir a relação entre o campo `id` da especificação Web e o código utilizado em Estrutura de Dados.
3. Definir a escala permitida para as avaliações dos produtos.

## Equipe

- Adones Gomes
- Nicolas Guedes
- Davi Renato
- Erick Farias

A divisão individual de responsabilidades ainda não está definida.

## Estrutura atual do repositório

```text
.
├── frontend/
│   ├── assets/
│   ├── css/
│   └── js/                 # estrutura preparada para desenvolvimento futuro
├── amazoff-backend/         # scaffold Spring Boot/Maven existente
├── docs/
│   ├── architecture/
│   ├── api/
│   ├── backlog/
│   ├── database/
│   ├── structures/
│   ├── AI_PROMPTS.md
│   └── MODELAGEM_MERMAID.md
└── docker/
```

O repositório contém diretórios preparados para o desenvolvimento futuro e um scaffold Spring Boot/Maven em `amazoff-backend/`. A presença desse scaffold descreve o estado dos arquivos, mas não altera RNF-003 nem estabelece que backend faça parte da primeira etapa da especificação Web. A relação entre o scaffold e a evolução prevista deverá ser alinhada pela equipe.

## Documentação complementar

- [Arquitetura](docs/architecture/README.md)
- [Estruturas de Dados](docs/structures/README.md)
- [Banco de Dados](docs/database/README.md)
- [Backlog](docs/backlog/README.md)
- [API](docs/api/README.md)
- [Diagramas Mermaid](docs/MODELAGEM_MERMAID.md)
- [Orientações para uso de IA](docs/AI_PROMPTS.md)
