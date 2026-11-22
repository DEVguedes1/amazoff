# Sistema de Gerenciamento de Produtos e Estoque

## Visão geral

O projeto planeja um catálogo administrativo de produtos e estoque, inspirado na organização de painéis de e-commerce. Seu objetivo é integrar desenvolvimento web e estruturas de dados: oferecer operações de cadastro e controle de estoque e, ao mesmo tempo, demonstrar como uma tabela hash e uma árvore binária de busca atendem a necessidades diferentes de consulta e ordenação.

Esta etapa define a arquitetura e a documentação. A implementação será iniciada posteriormente pela equipe, a partir desta especificação.

## Objetivos

- Planejar o cadastro, consulta, atualização e remoção de produtos.
- Consultar produtos rapidamente pelo código.
- Listar o catálogo em ordem crescente de código.
- Identificar e alertar sobre produtos abaixo do estoque mínimo.
- Demonstrar a aplicação e os custos das estruturas de dados implementadas manualmente.

## Equipe

- Adones Gomes
- Nicolas Guedes
- Davi Renato

## Stack tecnológica prevista

- **Backend:** Node.js puro e módulos nativos para disponibilizar a API, sem ORM ou banco de dados externo.
- **Armazenamento:** memória do processo, com estruturas de dados próprias.
- **Frontend:** HTML5 semântico, CSS3 responsivo e JavaScript Vanilla ES6+, usando `fetch` para acessar a API.
- **Versionamento:** Git e GitHub.

## Arquitetura planejada

O navegador apresenta o catálogo e os controles administrativos. O JavaScript do frontend enviará solicitações à API HTTP do Node.js. Um serviço `EstoqueManager` coordenará as operações e manterá consistência entre as estruturas.

| Componente | Papel previsto |
|---|---|
| Frontend HTML/CSS/JS | Exibir produtos, formulários, estado do estoque e alertas; enviar operações à API |
| API Node.js | Receber e validar solicitações HTTP e devolver respostas ao frontend |
| `EstoqueManager` | Coordenar cadastro, consulta, atualização, remoção e configuração de estoque mínimo |
| Tabela Hash | Indexar pelo código para detectar duplicidade e realizar busca rápida |
| ABB | Organizar os códigos e fornecer listagem crescente pelo percurso *in-order* |
| `Produto` | Representar código, nome, descrição, preço, quantidade e estoque mínimo |

As estruturas deverão guardar referências para o mesmo objeto `Produto`, sem duplicar os dados. O código do produto será a chave da Tabela Hash e a chave de ordenação da ABB. A Hash usará encadeamento separado para tratar colisões. A ABB não é auto-balanceada; portanto, seu custo depende da forma da árvore.

## Funcionalidades planejadas

1. Cadastrar produto, validando os campos e verificando duplicidade pela Tabela Hash.
2. Consultar produto por código pela Tabela Hash e apresentar detalhes e passos da busca para fins didáticos.
3. Atualizar a quantidade em estoque por código.
4. Remover um produto de ambas as estruturas.
5. Listar todos os produtos em ordem crescente de código pelo percurso *in-order* da ABB.
6. Definir quantidade mínima global e/ou por produto e alertar quando a quantidade estiver abaixo do limite aplicável.

## Complexidade assintótica prevista

| Operação | Tabela Hash (encadeamento separado) | ABB |
|---|---:|---:|
| Cadastro/inserção | O(1) médio para localizar/inserir no bucket; O(n) no pior caso de colisões | O(log n) em árvore balanceada; O(n) no pior caso |
| Consulta por código | O(1) médio; O(n) no pior caso | O(log n) em árvore balanceada; O(n) no pior caso |
| Atualização por código | O(1) médio; O(n) no pior caso | O(log n) em árvore balanceada; O(n) no pior caso |
| Remoção por código | O(1) médio; O(n) no pior caso | O(log n) em árvore balanceada; O(n) no pior caso |
| Listagem crescente | O(n) para percorrer todos os buckets, sem garantia de ordenação | O(n) pelo percurso *in-order* |

`n` representa a quantidade de produtos. A ABB especificada não se auto-balanceia: O(log n) descreve o caso balanceado, e não uma garantia para qualquer entrada.

## Diagramas principais

### Casos de uso e fluxo funcional

```mermaid
flowchart LR
  U[Usuário] --> UI[Painel web]
  UI --> C[Cadastrar produto]
  C --> V{Código já cadastrado?}
  V -- Sim --> D[Informar duplicidade]
  V -- Não --> I[Inserir referência na Hash e na ABB]
  UI --> B[Busca rápida por código]
  B --> H[Consultar Tabela Hash]
  H --> R[Exibir dados e passos da busca]
  UI --> A[Atualizar quantidade]
  A --> S[Atualizar o objeto Produto compartilhado]
  UI --> X[Remover produto]
  X --> E[Remover da Hash e da ABB]
  UI --> L[Listar catálogo]
  L --> O[Percurso in-order da ABB]
  O --> Q[Exibir códigos em ordem crescente]
  UI --> M[Verificar estoque baixo]
  M --> F[Comparar quantidade com mínimo aplicável]
  F --> G[Exibir alerta de reposição]
```

### Classes e objetos compartilhados

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
    +buckets
    +inserir(produto)
    +buscar(codigo)
    +atualizar(codigo, dados)
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
  }
  EstoqueManager *-- TabelaHash
  EstoqueManager *-- ArvoreBinariaBusca
  ArvoreBinariaBusca *-- NoABB
  TabelaHash --> Produto : referência ao objeto
  NoABB --> Produto : mesma referência
```

### Sequência de cadastro e consulta

```mermaid
sequenceDiagram
  actor U as Usuário
  participant F as Frontend
  participant API as API Node
  participant EM as EstoqueManager
  participant H as Tabela Hash
  participant A as ABB
  U->>F: Preenche dados do produto
  F->>API: Solicita cadastro
  API->>EM: cadastrar(dados)
  EM->>H: buscar(codigo) para validar duplicidade
  H-->>EM: código disponível ou duplicado
  alt Código disponível
    EM->>H: inserir(referência de Produto)
    EM->>A: inserir(a mesma referência)
    EM-->>API: Produto cadastrado
    API-->>F: Confirmação
  else Código duplicado
    EM-->>API: Erro de duplicidade
    API-->>F: Mensagem de validação
  end
  U->>F: Consulta código
  F->>API: Solicita busca pelo código
  API->>EM: consultar(codigo)
  EM->>H: buscar(codigo)
  H-->>EM: Produto e passos examinados
  EM-->>API: Resultado da busca
  API-->>F: Detalhes do produto e passos
  F-->>U: Apresenta resultado
```

Para os diagramas detalhados de entidades, arquitetura cliente-servidor e fluxos internos das estruturas, consulte [docs/MODELAGEM_MERMAID.md](docs/MODELAGEM_MERMAID.md).

## Governança de IA

Todo prompt enviado a ferramentas de IA para este projeto deverá ser registrado em [docs/AI_PROMPTS.md](docs/AI_PROMPTS.md), com data, autor, ferramenta, objetivo, arquivos resultantes e o texto literal do prompt.
