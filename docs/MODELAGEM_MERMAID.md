# Modelagem e fluxos Mermaid

Este documento reúne diagramas previstos para a revisão arquitetural e para a documentação do desenvolvimento. Os diagramas representam o comportamento planejado, não uma implementação existente.

## 1. Casos de uso e fluxo funcional

```mermaid
flowchart TD
  Usuario[Usuário] --> Painel[Painel administrativo]
  Painel --> Cadastrar[Cadastrar produto]
  Cadastrar --> Validar{Código duplicado na Tabela Hash?}
  Validar -- Sim --> Rejeitar[Rejeitar cadastro e informar motivo]
  Validar -- Não --> InserirHash[Inserir referência na Tabela Hash]
  InserirHash --> InserirABB[Inserir a mesma referência na ABB]
  Painel --> Consultar[Consultar produto por código]
  Consultar --> HashBuscar["Busca média O(1) na Tabela Hash"]
  HashBuscar --> Resultado[Exibir produto e passos examinados]
  Painel --> Atualizar[Atualizar quantidade em estoque]
  Atualizar --> HashUpdate[Localizar produto pela Hash]
  HashUpdate --> Mutar[Alterar o objeto compartilhado]
  Painel --> Remover[Remover produto]
  Remover --> HashDelete[Remover da Tabela Hash]
  HashDelete --> ABBDelete[Remover da ABB]
  Painel --> Listar[Listar produtos]
  Listar --> InOrder["Percurso in-order O(n)"]
  InOrder --> Crescente[Apresentar códigos crescentes]
  Painel --> Estoque[Verificar estoque baixo]
  Estoque --> Minimo[Comparar quantidade com estoque mínimo]
  Minimo --> Alerta["Marcar para reposição quando quantidade < mínimo"]
```

## 2. Diagrama de classes e referências

```mermaid
classDiagram
  class Produto {
    +String codigo
    +String nome
    +String descricao
    +Number preco
    +Number quantidade
    +Number estoqueMinimo
  }
  class NoABB {
    +Produto produto
    +NoABB esquerda
    +NoABB direita
  }
  class ArvoreBinariaBusca {
    +NoABB raiz
    +inserir(produto)
    +buscar(codigo)
    +remover(codigo)
    +percursoEmOrdem()
  }
  class TabelaHash {
    +Array buckets
    +hash(codigo)
    +inserir(produto)
    +buscar(codigo)
    +atualizar(codigo, novosDados)
    +remover(codigo)
  }
  class EstoqueManager {
    +TabelaHash tabela
    +ArvoreBinariaBusca arvore
    +cadastrar(dados)
    +consultar(codigo)
    +atualizarEstoque(codigo, quantidade)
    +remover(codigo)
    +listarOrdenado()
    +listarEstoqueBaixo()
  }
  EstoqueManager *-- TabelaHash : coordena
  EstoqueManager *-- ArvoreBinariaBusca : coordena
  ArvoreBinariaBusca *-- NoABB : organiza
  TabelaHash --> Produto : guarda referência
  NoABB --> Produto : guarda a mesma referência
```

## 3. Modelo de entidades

```mermaid
erDiagram
  PRODUTO {
    string codigo PK
    string nome
    string descricao
    decimal preco
    int quantidade
    int estoqueMinimo
  }
  TABELA_HASH {
    string chave_codigo
    string politica_colisao "encadeamento separado"
  }
  NO_ABB {
    string codigo_produto
    string codigo_esquerda
    string codigo_direita
  }
  ESTOQUE_MANAGER {
    string limiteMinimoGlobal
  }
  ESTOQUE_MANAGER ||--|| TABELA_HASH : coordena
  ESTOQUE_MANAGER ||--|| NO_ABB : coordena
  TABELA_HASH }o--|| PRODUTO : referencia
  NO_ABB }o--|| PRODUTO : referencia_compartilhada
```

As ligações com `PRODUTO` representam referências em memória ao objeto, não cópias ou tabelas de banco de dados.

## 4. Arquitetura cliente-servidor

```mermaid
flowchart LR
  subgraph Cliente[Cliente — navegador]
    UI[HTML semântico e CSS]
    JS[JavaScript Vanilla]
    UI <--> JS
  end
  subgraph Servidor[Servidor Node.js]
    HTTP[API HTTP]
    EM[EstoqueManager]
    Hash[Tabela Hash]
    ABB[Árvore Binária de Busca]
    HTTP --> EM
    EM --> Hash
    EM --> ABB
  end
  JS <-->|fetch / JSON| HTTP
  Hash -. referência compartilhada .-> Produto[(Objeto Produto)]
  ABB -. mesma referência .-> Produto
```

O Node.js servirá a API e, conforme a organização de implantação definida pela equipe, poderá servir também os arquivos estáticos do frontend. Os dados residirão na memória do processo; não há persistência após reiniciar o servidor prevista nesta arquitetura.

## 5. Fluxo interno de cadastro na Tabela Hash e na ABB

```mermaid
sequenceDiagram
  participant F as Frontend
  participant API as API Node
  participant EM as EstoqueManager
  participant H as Tabela Hash
  participant A as ABB
  F->>API: Enviar campos do produto
  API->>EM: cadastrar(dados)
  EM->>H: buscar(codigo)
  alt Código já cadastrado
    H-->>EM: Produto existente
    EM-->>API: Rejeitar duplicidade
    API-->>F: Resposta de erro
  else Código livre
    H-->>EM: Não encontrado
    EM->>EM: Criar objeto Produto
    EM->>H: inserir(referência de Produto)
    H->>H: Calcular bucket e encadear colisão se necessário
    EM->>A: inserir(a mesma referência)
    A->>A: Comparar código e posicionar nó
    EM-->>API: Cadastro concluído
    API-->>F: Resposta de sucesso
  end
```

## 6. Fluxo interno de consulta por código

```mermaid
sequenceDiagram
  participant F as Frontend
  participant API as API Node
  participant EM as EstoqueManager
  participant H as Tabela Hash
  F->>API: Solicitar produto pelo código
  API->>EM: consultar(codigo)
  EM->>H: buscar(codigo)
  H->>H: Calcular índice do bucket
  H->>H: Percorrer cadeia e contar comparações
  alt Produto localizado
    H-->>EM: Referência Produto e número de passos
    EM-->>API: Resultado da consulta
    API-->>F: Dados do produto e passos
  else Código ausente
    H-->>EM: Não encontrado
    EM-->>API: Resultado vazio
    API-->>F: Resposta de não encontrado
  end
```

## 7. Percurso in-order da ABB

```mermaid
flowchart TD
  Inicio[Iniciar no nó raiz] --> Esquerda[Visitar subárvore esquerda]
  Esquerda --> Atual[Adicionar produto do nó atual à lista]
  Atual --> Direita[Visitar subárvore direita]
  Direita --> Fim[Retornar lista em ordem crescente]
```

Para uma árvore com `n` nós, o percurso visita cada nó uma vez: **O(n)**. Inserção, busca e remoção custam **O(log n)** em uma árvore balanceada e **O(n)** no pior caso de uma ABB degenerada.

## 8. Tratamento de colisões por encadeamento separado

```mermaid
flowchart LR
  C1[ código A ] --> H[Função hash]
  C2[ código B ] --> H
  H --> B[Bucket calculado]
  B --> E1[Entrada: chave A e referência Produto]
  E1 --> E2[Entrada: chave B e referência Produto]
```

Entradas que produzem o mesmo índice permanecem encadeadas no mesmo bucket. A busca compara os códigos da cadeia correspondente. A complexidade esperada é **O(1)** com distribuição adequada e fator de carga controlado; no pior caso, todas as entradas podem cair no mesmo bucket, levando a **O(n)**.
