# Diagramas do AmazOFF

Os diagramas abaixo resumem a separação por disciplina. A especificação detalhada de entidades, atributos, operações, agregados e pendências está em [`specification/README.md`](specification/README.md). A distribuição de bancos permanece proposta até aprovação.

## Entregas separadas

```mermaid
flowchart LR
  subgraph TW[Tecnologias Web]
    U[Interface] --> JS[Serviços JavaScript]
    JS --> MEM[(Arrays e objetos em memória)]
    JS --> U
  end
  subgraph ED[Estrutura de Dados]
    IM[InventoryManager] --> ABB[ABB própria]
    IM --> HASH[Tabela hash própria]
  end
  subgraph BD[Banco de Dados II]
    CLI[Terminal e menus] --> SVC[Serviços]
    SVC --> REPO[DAO / Repository]
    REPO --> M[(MongoDB proposto)]
    REPO --> R[(Riak proposto)]
    REPO --> C[(Cassandra proposto)]
  end
```

## Modelo conceitual

```mermaid
erDiagram
  PRODUCT ||--o{ REVIEW : receives
  CUSTOMER ||--o{ ADDRESS : has
  CUSTOMER ||--o{ CART : owns
  CART ||--o{ CART_ITEM : contains
  PRODUCT ||--o{ CART_ITEM : referenced_by
  PRODUCT ||--o{ PRODUCT_METRIC_DAILY : measured_by
```

Cada `productId` em Review, CartItem e métricas referencia exatamente `Product.id`. Em Estrutura de Dados, a chave “código” da busca é o próprio `Product.id`, sem outro atributo identificador.

### Entidades e atributos do domínio

O identificador interno de todo produto é unicamente `Product.id`. Na atividade ED, a expressão “código do produto” refere-se ao mesmo `id`, usado como chave da ABB e da tabela hash. Não existe campo `codigo` adicional. Referências como `productId` abaixo armazenam o valor de `Product.id`; não são uma segunda identidade.

| Entidade | Responsabilidade | Identificador e atributos (tipos lógicos) | Relacionamentos |
|---|---|---|---|
| Product | Dados comuns do catálogo e estoque | `id: String` (representação técnica pendente); `name: String`; `description: String`; `price: Decimal`; `category/categoryId: String`; `brand: String`; `image: String`; `stock: Integer`; `createdAt: Instant`; `soldQuantity: Long` (BD); reviews/average derivados ou associados | 1 Product recebe 0..N Review; pode ser referenciado por CartItem e métricas. |
| Review | Nota e comentário de um produto | `reviewId: String`; `productId: Product.id`; `score: Decimal/Integer`; `comment: String`; `createdAt: Instant` | Cada Review pertence a exatamente um Product. Escala de nota pendente. |
| Customer | Cadastro e dados de acesso | `customerId: String`; `email: String`; `passwordHash: String`; `accessData: Map<String,String>`; `createdAt/updatedAt: Instant` | 1 Customer tem 0..N Address e pode ter 0..N Cart (históricos). |
| Address | Endereço de entrega do cliente | `addressId: String`; destinatário, rua, número, complemento?, bairro, cidade, estado, CEP, país, instruções? (`String`) | Pertence a um Customer; cardinalidade 0..N. Campos exatos ainda são recomendação. |
| Cart | Agregado efêmero associado ao cliente, TTL 48h | `cartId: String`; `customerId: String`; `createdAt/updatedAt/expiresAt: Instant`; `items: CartItem[]`; `status: ACTIVE/EXPIRED` (derivado) | Um Customer possui carrinhos; acesso rápido por cliente. Não exige durabilidade permanente. |
| CartItem | Produto, quantidade e preço no carrinho | `productId: Product.id`; `quantity: Integer`; `unitPrice: Decimal`; `productNameSnapshot?: String` | Interno a exatamente um Cart; referencia um Product por id; snapshot de preço/política pendentes. |
| ProductMetricEvent | Ocorrência de visualização, adição ou remoção | `eventId: String`; `productId: Product.id`; `eventType: VIEW/ADD/REMOVE`; `occurredAt: Instant`; `quantity?: Integer`; `windowStart: Instant` | Cada evento refere-se a um Product; identidade de usuário não é necessária para a contagem global transcrita. |
| ProductMetricDaily | Contadores agregados por produto e janela de 24h | `productId: Product.id`; `dayUtc: LocalDate`; `views/cartAdds/cartRemovals: Long`; `aggregatedAt: Instant` | Projeção de eventos de um Product numa janela diária; não representa quantidade vendida. |

`averageRating`, `subtotal` e total do carrinho são valores derivados das avaliações/itens; persistir cópias exige política de atualização. `soldQuantity` é exigido no catálogo BD, mas seu significado e relação com estoque ainda precisam ser confirmados. A imutabilidade de `Product.id` após criação é recomendada, mas permanece decisão técnica. `TreeNode`, `HashEntry` e `Bucket` são estruturas internas ED, não entidades persistidas do domínio.

## Estruturas ED

```mermaid
classDiagram
  class Product {
    String id
    String name
    String description
    Decimal price
    String category
    String brand
    String image
    Integer stock
    Instant createdAt
  }
  class Review {
    String reviewId
    String productId
    Number score
    String comment
    Instant createdAt
  }
  class TreeNode {
    Product product
    TreeNode left
    TreeNode right
  }
  class ProductHashTable {
    Bucket[] buckets
    put(Product)
    get(id)
    remove(id)
  }
  class InventoryManager {
    register(Product)
    find(id)
    updateStock(id, quantity)
    delete(id)
    listByCode()
    lowStock()
  }
  class BinarySearchTree {
    insert(Product)
    search(id)
    remove(id)
    inOrder()
  }
  class ProductHashTable {
    put(Product)
    get(id)
    remove(id)
  }
  InventoryManager --> BinarySearchTree
  InventoryManager --> ProductHashTable
  TreeNode --> Product
  Product --> Review
```

## Fluxos de persistência BD propostos

```mermaid
flowchart TB
  MENU[Menu terminal] --> SERVICE[Serviço de aplicação]
  SERVICE --> CUSTOMER[Customer Repository]
  SERVICE --> PRODUCT[Product Repository]
  SERVICE --> CART[Cart Repository]
  SERVICE --> METRIC[Metric Repository]
  CUSTOMER --> M[(MongoDB: clientes)]
  PRODUCT --> M2[(MongoDB: catálogo)]
  CART --> R[(Riak: carrinho TTL 48h)]
  METRIC --> C[(Cassandra: eventos e agregado diário)]
```

MongoDB/Riak/Cassandra são uma recomendação de distribuição, sujeita à escolha de ao menos dois bancos e justificativa da equipe. Não há API REST obrigatória na especificação transcrita. A configuração Spring Boot do projeto Gradle não representa, sozinha, a entrega BD concluída.
