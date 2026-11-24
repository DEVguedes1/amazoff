# Persistência e Banco de Dados II

## Contexto e status da proposta

As especificações oficiais do professor de Banco de Dados II ainda não foram disponibilizadas. Portanto, toda distribuição de responsabilidades neste documento é **PROPOSTA**, não requisito oficial da disciplina. A estratégia inicial considerada é começar com Riak KV e MongoDB; Cassandra e Neo4j ficam para avaliação posterior, conforme orientação do professor e viabilidade. A equipe deverá revisar esta proposta quando receber os requisitos oficiais.

## Riak KV — registros brutos importados

Riak KV é um banco NoSQL de modelo chave-valor: uma chave identifica um valor armazenado. Conceitualmente, a aplicação escolhe uma chave estável para cada registro e realiza operações de gravação, leitura, atualização e remoção usando essa identificação.

### Uso proposto no AmazOFF

O Riak deverá conservar registros brutos de produtos recebidos da API DummyJSON. Um exemplo conceitual de chave é `dummyjson:product:1`, em que o identificador externo distingue o registro importado. Formato exato do valor, política de sobrescrita, tratamento de versão e estratégia para chaves ausentes estão **PENDENTES DE DEFINIÇÃO**.

- **Gravação:** armazenar o conteúdo externo associado à chave do registro.
- **Leitura:** recuperar o registro bruto identificado pela chave.
- **Atualização:** substituir ou atualizar o conteúdo associado à chave conforme a política que vier a ser definida.
- **Remoção:** excluir a entrada identificada quando a regra de retenção permitir.

O interesse acadêmico e arquitetural é demonstrar persistência chave-valor e manter disponível uma representação de origem dos dados importados. Por definição desta proposta, os registros brutos no Riak **não são a fonte principal do catálogo operacional**. Eles também não substituem os produtos normalizados/transformados para uso da aplicação.

## MongoDB — catálogo operacional

MongoDB é um banco orientado a documentos. Os registros são documentos BSON (representação binária compatível com a estrutura JSON) organizados em coleções. O formato de documento permite agrupar campos relacionados e acomodar informações complementares sem exigir que todos os produtos tenham exatamente a mesma composição.

### Uso proposto no AmazOFF

MongoDB é proposto como persistência principal do catálogo operacional. Um documento de produto poderá representar código, nome, descrição, preço, categoria, marca, imagem e informações de estoque, de acordo com o modelo interno que a equipe definir. Não se estabelece aqui um schema final, índices, nomes de coleções ou estratégia de validação; esses itens estão **PENDENTES DE DEFINIÇÃO**.

O modelo por documentos combina com a consulta e apresentação de um produto como conjunto de atributos e admite evolução gradual de informações complementares. Avaliações e comentários poderão ser considerados em documentos ou estruturas associadas se forem mantidos no escopo; a decisão está pendente.

### Diferença de papel entre MongoDB e Riak

Na proposta, **MongoDB guarda o catálogo operacional transformado e utilizado pela aplicação**, enquanto **Riak conserva registros brutos de origem importados**. O backend deverá converter os dados externos para o modelo AmazOFF antes de disponibilizá-los no catálogo. A proposta não define que cada alteração operacional seja refletida de volta no registro bruto.

## Cassandra — possibilidade futura

Cassandra é um banco NoSQL distribuído orientado a colunas largas. É concebido para distribuir dados e cargas de trabalho entre nós. A modelagem é guiada pelas consultas que a aplicação precisa executar, portanto as tabelas e chaves de partição devem ser desenhadas a partir dos acessos definidos.

Como **PROPOSTA FUTURA**, poderá registrar movimentações de estoque (entradas, saídas e outras alterações) como eventos históricos, preservando o produto, o tipo de movimento, a quantidade e o momento conforme o modelo que venha a ser aprovado. Consultas por produto e período seriam um possível padrão de acesso. O uso se justificaria caso a disciplina exija explorar um modelo distribuído/colunar e o volume ou padrão de consulta torne esse histórico útil.

Implementação e responsabilidade dependem da especificação oficial de Banco de Dados II, da viabilidade e da decisão da equipe; não estão aprovadas.

## Neo4j — possibilidade futura

Neo4j é um banco orientado a grafos, em que nós representam entidades e relacionamentos representam conexões entre elas. Como **PROPOSTA FUTURA**, nós poderiam representar produtos, categorias e marcas, com relações de pertencimento a categorias, associação com marcas ou ligação entre produtos relacionados. Consultas de vizinhança poderiam apoiar a exploração de produtos relacionados ou recomendações, caso isso seja requerido.

A proposta não define algoritmo de recomendação nem requisito de recomendação. Implementação, entidades e tipos de relacionamento dependem da especificação do professor e de confirmação da equipe.

## Integração conceitual com DummyJSON

A fonte externa considerada é [DummyJSON Products](https://dummyjson.com/docs/products). O objetivo é oferecer dados para popular inicialmente o catálogo e apoiar a demonstração da importação. O frontend não deverá depender de uma chamada à DummyJSON a cada carregamento do catálogo: o backend deverá disponibilizar os dados operacionais a partir das persistências do projeto.

Fluxo proposto:

1. Uma operação de importação no backend Java obtém os produtos da DummyJSON.
2. O backend preserva o registro bruto no Riak KV, identificando-o por uma chave baseada no identificador externo (por exemplo, `dummyjson:product:1`).
3. O backend transforma os campos externos para o modelo interno do AmazOFF, tratando diferenças de nomes, tipos e campos ausentes conforme regras ainda a definir.
4. Os dados operacionais transformados são persistidos no MongoDB, proposta de fonte principal do catálogo.
5. O frontend consulta a API do AmazOFF; a aplicação não precisa consultar a DummyJSON para cada visualização.

Este fluxo é uma proposta e não está implementado. Mapeamento de campos, deduplicação, importação incremental, conflitos com edições manuais, tratamento de indisponibilidade, repetição segura, paginação e política de atualização estão **PENDENTES DE DEFINIÇÃO**.

## Visão geral da distribuição proposta

| Banco | Modelo | Responsabilidade proposta | Etapa |
|---|---|---|---|
| Riak KV | Chave-valor | Registros brutos importados da DummyJSON | Inicial |
| MongoDB | Documentos | Catálogo operacional de produtos | Inicial |
| Cassandra | Colunas largas | Histórico de movimentações de estoque | Futura |
| Neo4j | Grafos | Relacionamentos entre produtos, categorias e marcas | Futura |

## Decisões pendentes para Banco de Dados II

- Quais bancos e requisitos são obrigatórios segundo a especificação oficial do professor?
- O projeto precisa usar os quatro bancos ou a escolha depende da equipe?
- Quais operações e consultas devem ser demonstradas para cada modelo?
- Riak e MongoDB serão suficientes para a primeira entrega? Qual papel acadêmico cada um deve demonstrar?
- Histórico de movimentações e relações entre produtos são requisitos ou apenas possibilidades?
- Quais volumes, dados de demonstração e critérios de avaliação devem ser considerados?
- Como serão provisionados os bancos, definidos ambientes e administradas as credenciais? A estratégia operacional ainda não está definida.
- Como sincronizar o estado em memória das estruturas didáticas com o catálogo persistido, incluindo inicialização e falhas?

As respostas devem ser incorporadas aqui e na documentação de arquitetura antes de tratar a proposta como decisão aprovada.
