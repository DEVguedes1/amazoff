# Diagramas conceituais do AmazOFF

Este documento reúne diagramas de arquitetura e estruturas de dados previstos para apoiar a discussão e a implementação futura. Eles descrevem propostas e não comprovam funcionalidades implementadas. A separação entre os bancos é uma hipótese inicial sujeita à validação acadêmica.

## 1. Arquitetura geral

```mermaid
flowchart LR
  U[Pessoa usuária] <--> FE[Frontend: HTML, CSS e JavaScript]
  FE <-->|HTTP e JSON| API[API REST: Java e Spring Boot]
  API --> SV[Serviços e regras de negócio]
  SV --> EM[EstoqueManager]
  EM --> H[Tabela Hash]
  EM --> ABB[Árvore Binária de Busca]
  SV --> REP[Camada de persistência proposta]
  REP --> R[(Riak KV)]
  REP --> M[(MongoDB)]
  REP --> C[(Cassandra — fase posterior)]
  REP --> N[(Neo4j — fase posterior)]
```

## 2. Comunicação frontend/backend

```mermaid
sequenceDiagram
  actor U as Pessoa usuária
  participant FE as Frontend HTML/CSS/JS
  participant API as API REST Java/Spring Boot
  participant S as Serviço de produtos
  participant EM as EstoqueManager
  participant DB as Persistência proposta
  U->>FE: Interage com catálogo ou controles
  FE->>API: Requisição HTTP com JSON
  API->>S: Encaminha operação validada
  S->>EM: Coordena operação nas estruturas
  EM-->>S: Resultado conceitual
  S->>DB: Leitura ou gravação prevista
  DB-->>S: Resultado da persistência prevista
  S-->>API: Resultado da operação
  API-->>FE: Resposta HTTP com JSON
  FE-->>U: Apresenta resultado
```

O fluxo representa uma comunicação planejada. Serviços, persistência e integração não são declarados como concluídos.

## 3. Fluxo de gerenciamento de produtos

```mermaid
flowchart TD
  A[Solicitação recebida pela API] --> B{Operação}
  B -->|Cadastrar| C[Verificar código na Hash]
  C --> D{Código disponível?}
  D -->|Sim| E[Inserir referência em Hash e ABB]
  D -->|Não| F[Informar duplicidade]
  B -->|Consultar por código| G[Buscar na Hash]
  B -->|Atualizar estoque| H[Localizar produto e alterar quantidade]
  B -->|Remover| I[Remover da Hash e da ABB]
  B -->|Listar| J[Percorrer ABB em ordem]
  B -->|Estoque baixo| K[Comparar quantidade com mínimo]
  E --> L[Resultado planejado]
  F --> L
  G --> L
  H --> L
  I --> L
  J --> L
  K --> L
```

## 4. Organização conceitual das estruturas de dados

```mermaid
classDiagram
  class EstoqueManager {
    coordena operações
    mantém consistência
  }
  class TabelaHash {
    chave: codigo
    colisões: encadeamento separado
    consulta média: O(1)
    pior caso: O(n)
  }
  class ArvoreBinariaBusca {
    ordenação: codigo
    percurso: in-order
    caso balanceado: O(log n)
    pior caso: O(n)
    balanceamento automático: não previsto
  }
  class Produto {
    codigo: String
    nome: String
    quantidade: Integer
  }
  EstoqueManager --> TabelaHash : coordena
  EstoqueManager --> ArvoreBinariaBusca : coordena
  TabelaHash --> Produto : referência compartilhada
  ArvoreBinariaBusca --> Produto : mesma referência
```

A listagem crescente pela ABB percorre `n` nós e custa O(n). As duas estruturas devem apontar para o mesmo objeto Produto para evitar divergência de dados.

### Colisões na Tabela Hash

```mermaid
flowchart LR
  K1[Código A] --> F[Função de hash]
  K2[Código B] --> F
  F --> B[Mesmo bucket calculado]
  B --> E1[Entrada A]
  E1 --> E2[Entrada B encadeada]
```

Com distribuição adequada, a consulta esperada é O(1); no pior caso, uma cadeia com todas as `n` entradas resulta em O(n).

### Percurso in-order da ABB

```mermaid
flowchart LR
  E[Subárvore esquerda] --> N[Nó atual]
  N --> D[Subárvore direita]
  D --> O[Sequência crescente por código]
```

## 5. Organização conceitual dos bancos

```mermaid
flowchart TB
  APP[Aplicação Java: integração proposta] --> F1[Fase 1 — prioridade inicial]
  APP --> F2[Fase 2 — posterior]
  F1 --> R[Riak KV: registros brutos DummyJSON e CRUD por chave]
  F1 --> M[MongoDB: catálogo operacional e documentos de produtos]
  M -. opcional, se escopo confirmado .-> AV[Avaliações e informações complementares]
  F2 --> C[Cassandra: histórico de movimentações por produto e período]
  F2 --> N[Neo4j: relações entre produtos, categorias e marcas]
```

As responsabilidades dos quatro bancos são hipóteses arquiteturais, não requisitos aprovados. As especificações do professor de Banco de Dados II ainda serão recebidas.

## Histórico conceitual

A primeira modelagem usava Node.js e armazenamento apenas em memória para ilustrar a Hash e a ABB. Essa etapa conceitual permanece registrada, mas a arquitetura atualmente planejada considera backend Java/Spring Boot e persistência NoSQL. A mudança de proposta não altera os conceitos didáticos das estruturas: suas complexidades e a necessidade de manter referências compartilhadas continuam documentadas.
