# Estruturas de Dados

Este documento especifica o papel acadêmico e o comportamento esperado das estruturas previstas para o catálogo. É documentação conceitual; não contém nem declara implementações existentes. A ABB considerada é convencional e não possui balanceamento automático.

## Tabela Hash

### Objetivo e conceito

A Tabela Hash deverá localizar produtos rapidamente a partir de seu código. Em termos gerais, uma função hash transforma uma chave em um índice de uma tabela de posições, chamados *buckets*. Para o AmazOFF, a chave será `codigo` do produto. A estrutura deve confirmar igualdade das chaves: colisões de índice não significam que dois códigos sejam iguais.

### Função hash e colisões

A função deverá ser determinística: o mesmo código deve conduzir ao mesmo bucket enquanto a tabela e seus parâmetros forem os mesmos. A escolha da função, capacidade inicial, política de crescimento e critério para redimensionamento estão **PENDENTES DE DEFINIÇÃO** durante a implementação.

Como chaves distintas podem produzir o mesmo índice, a estratégia proposta é o **encadeamento separado**: cada bucket mantém uma coleção de entradas. A operação examina essa cadeia e compara os códigos. Uma distribuição razoável das chaves e um fator de carga controlado sustentam o custo médio esperado; uma cadeia longa aumenta o custo.

### Operações previstas

- **Inserção:** calcular o bucket do código; verificar se já existe uma entrada com essa chave; inserir a referência do produto na cadeia correspondente quando o código estiver disponível.
- **Busca:** calcular o bucket e comparar as chaves da cadeia até localizar o código ou concluir que não existe.
- **Atualização:** localizar o produto pela chave e alterar os campos permitidos do objeto encontrado. A atualização do produto não deve criar uma segunda cópia usada por outra estrutura.
- **Remoção:** localizar e retirar da cadeia a entrada com o código informado; a coordenação com a remoção da ABB cabe ao componente coordenador.

### Complexidade

Com hash bem distribuído e fator de carga limitado, inserção, busca e remoção têm custo médio esperado **O(1)**. A busca em uma cadeia, e as operações que precisam encontrá-la, chegam a **O(n)** no pior caso, quando todas as chaves colidem. O comportamento depende da função, distribuição, capacidade e redimensionamento, decisões que ainda serão especificadas.

### Justificativa

A Hash atende diretamente à consulta por código, central para identificar e atualizar um produto. Também permite demonstrar colisões, função hash e diferença entre desempenho médio e pior caso. Ela não fornece ordem crescente; por isso é combinada com uma ABB.

## Árvore Binária de Busca (ABB)

### Conceito e organização

Uma ABB convencional organiza cada nó com até dois filhos. Para a ordenação por código, os códigos da subárvore à esquerda são menores que o código do nó, e os da subárvore à direita são maiores. O critério para códigos iguais deverá ser coerente com a regra de unicidade do catálogo. A proposta não usa AVL, Rubro-Negra ou qualquer balanceamento automático.

Cada nó deverá referenciar o produto associado ao código e os filhos esquerdo e direito. A decisão sobre a representação e comparação do código (inclusive normalização e sensibilidade a maiúsculas) está **PENDENTE DE DEFINIÇÃO**.

### Operações previstas

- **Inserção:** comparar o código a partir da raiz e descer à esquerda ou à direita até encontrar a posição apropriada. A regra de duplicidade deverá ser consistente com a Hash.
- **Busca:** seguir as comparações da raiz até encontrar o código ou chegar a uma posição vazia. No fluxo usual do sistema, a Hash é a estrutura prevista para a consulta rápida.
- **Remoção:** localizar o nó e ajustar a ligação com seu pai. Conceitualmente, há três casos: nó folha; nó com um filho; nó com dois filhos, quando se substitui a posição pelo sucessor ou predecessor em ordem e se reorganizam as ligações.
- **Listagem:** percorrer recursivamente esquerda, nó atual e direita (*in-order*). Com a propriedade de ordenação, os produtos são apresentados em ordem crescente de código.

### Complexidade

Inserção, busca e remoção custam **O(log n)** quando a forma da árvore tem altura proporcional a log n, como em uma árvore razoavelmente equilibrada. Como não há balanceamento automático, a árvore pode degenerar, por exemplo, quando códigos chegam em ordem crescente; nesse pior caso a altura é `n` e as operações custam **O(n)**. O percurso *in-order* visita cada nó uma vez e custa **O(n)**. `n` representa o número de produtos.

### Justificativa

A ABB demonstra organização hierárquica por chave e produz listagem ordenada naturalmente por percurso *in-order*. Sua comparação com a Hash permite discutir busca, ordenação e efeitos de uma árvore não balanceada, alinhada à atividade de Estrutura de Dados.

## Coordenação e consistência

As duas estruturas deverão trabalhar sobre **os mesmos objetos de produto**, armazenando referências ao mesmo objeto, em vez de cópias independentes. Assim, uma alteração no produto localizado por código fica visível para ambas. A regra para alterar o próprio código, que é chave de indexação, precisa ser definida: conceitualmente, isso envolve remover as entradas antigas e reinseri-las com a nova chave.

O componente coordenador é chamado provisoriamente de `EstoqueManager`. Ele deverá coordenar operações que afetem as duas estruturas e evitar estados divergentes. Ainda estão **PENDENTES DE DEFINIÇÃO** a política para falha parcial, atomicidade e a relação entre essas estruturas em memória e a persistência.

## Exemplos conceituais de operações

1. **Cadastro:** o coordenador verifica na Hash se o código já existe. Se estiver livre, cria/recebe um produto e insere a referência correspondente na Hash e na ABB. Se uma inserção falhar, a política para restaurar a consistência precisa estar definida.
2. **Consulta pelo código:** a solicitação é encaminhada à Hash, que calcula o bucket e percorre sua cadeia. O resultado pode então ser apresentado pela interface por meio da API.
3. **Atualização de estoque:** a Hash localiza o objeto pelo código; o coordenador altera a quantidade no objeto compartilhado. Como a chave não mudou, a ordenação da ABB permanece válida.
4. **Remoção:** o coordenador localiza o produto e remove sua entrada da Hash e seu nó da ABB, mantendo as duas estruturas alinhadas.
5. **Listagem ordenada:** o coordenador solicita à ABB o percurso *in-order*, que retorna os produtos em ordem crescente de código.
6. **Estoque baixo:** uma regra compara quantidade disponível ao limite mínimo aplicável e seleciona os produtos abaixo do limite. A existência de limite global, individual ou ambos ainda precisa ser confirmada pela equipe.

Os exemplos descrevem comportamento esperado, não código, decisões finais de API ou implementação.

## Resumo de custos esperados

| Operação | Tabela Hash com encadeamento separado | ABB convencional |
|---|---:|---:|
| Inserção | O(1) médio; O(n) pior caso | O(log n) em árvore de altura logarítmica; O(n) pior caso |
| Busca | O(1) médio; O(n) pior caso | O(log n) em árvore de altura logarítmica; O(n) pior caso |
| Atualização por código | O(1) médio; O(n) pior caso | O(log n) em árvore de altura logarítmica; O(n) pior caso |
| Remoção | O(1) médio; O(n) pior caso | O(log n) em árvore de altura logarítmica; O(n) pior caso |
| Listagem crescente | Não garantida pela ordem dos buckets | O(n) com percurso *in-order* |

Os custos médios da Hash pressupõem distribuição adequada. A ABB não oferece garantia O(log n) para qualquer entrada, pois não se auto-balanceia.
