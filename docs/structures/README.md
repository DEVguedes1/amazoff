# Estruturas de Dados e Algoritmos

## Escopo obrigatório

O Projeto 3 exige ABB para organização/listagem crescente por código e tabela hash para consulta rápida por código. Ambas devem ser implementadas pelo grupo; bibliotecas de árvores/listas ordenadas não substituem as estruturas. A especificação completa de entidades, métodos, erros, critérios e pendências está em [`docs/specification/README.md`](../specification/README.md).

## Contrato estrutural

- `Product.id` é o identificador único nas três disciplinas. Na ED, a “consulta pelo código” usa esse mesmo `id`; não existe campo separado ou identificador concorrente. A representação (tipo e comparação ordenada) ainda precisa ser definida.
- `BinarySearchTree` usa nós próprios (`product`, `left`, `right`), inserção, busca, remoção (folha, um filho, dois filhos) e percurso in-order.
- `ProductHashTable` usa função determinística, buckets e encadeamento separado; inserir, consultar, remover e atualizar por código. Capacidade, fator de carga e redimensionamento são parâmetros ainda a aprovar.
- `InventoryManager` coordena ambas; recomenda-se guardar referências ao mesmo produto lógico, rejeitar código duplicado e evitar divergência após falha parcial.
- Quantidade negativa deve ser rejeitada como recomendação de regra; limiar e semântica de estoque baixo ainda precisam de aprovação.

## Complexidades esperadas

Se `h` é a altura da ABB: busca/inserção/remoção custam O(h), O(log n) quando a árvore tem altura logarítmica e O(n) no pior caso degenerado. In-order custa O(n). Hash tem O(1) esperado sob distribuição adequada e O(n) no pior caso de colisões. Rehash custa O(n) quando ocorrer. Não se promete balanceamento automático.

## Consistência operacional

Cadastro verifica duplicidade e registra em ambas; se a segunda operação falhar, compensar/remover a primeira ou sinalizar erro sem deixar divergência silenciosa. Atualização altera o produto compartilhado. Remoção localiza e exclui dos dois índices. Produto inexistente retorna resultado explícito e não altera estado. A implementação e testes futuros devem cobrir colisões e os três casos de remoção da ABB.

## Separação entre disciplinas

Hash e ABB são requisito de ED. Arrays/objetos em memória são requisito de armazenamento da primeira entrega Web e não substituem as estruturas exigidas em ED. Persistência NoSQL de BD é outra entrega e não substitui as implementações próprias. Linguagem e compartilhamento de código entre etapas ED não estão definidos na informação oficial disponível.
