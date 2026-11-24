# Arquitetura do AmazOFF

## Visão geral

O AmazOFF está planejado como um **monólito modular**: uma aplicação backend Java única, organizada em módulos com responsabilidades claras, usando Spring Boot para disponibilizar a API REST e Maven para dependências e build. O frontend será uma aplicação separada em HTML, CSS e JavaScript puro, comunicando-se com o backend por HTTP e JSON.

A escolha mantém simples a execução, a integração e a demonstração acadêmica, sem distribuir a aplicação em serviços independentes. Cada módulo poderá ser desenvolvido e entendido separadamente dentro do mesmo sistema. **Microserviços não fazem parte da arquitetura desta etapa.** A implementação das regras de negócio e estruturas de dados deverá permanecer em Java e não ser concentrada em controllers nem substituída por abstrações de framework sem decisão da equipe.

## Módulos e responsabilidades

| Módulo | Responsabilidade prevista |
|---|---|
| Frontend | Apresentar catálogo, pesquisa, filtros, detalhes e controles administrativos; coletar ações e exibir resultados e erros recebidos da API. |
| API / controllers | Receber requisições HTTP, interpretar entradas, delegar a operação e converter o resultado em resposta HTTP/JSON. Não deve concentrar regras centrais de negócio. |
| Serviços de negócio | Aplicar regras de produtos e estoque, coordenar os componentes envolvidos e decidir quando ler ou persistir dados. |
| Estruturas de dados | Implementar em Java a Tabela Hash, a ABB e a coordenação prevista pelo `EstoqueManager`, conforme os requisitos acadêmicos confirmados. |
| Persistência / repositories | Isolar o acesso aos bancos escolhidos e apoiar a leitura e gravação do catálogo e dos demais dados definidos. A divisão final depende da disciplina. |
| Integrações externas | Importar e transformar dados da DummyJSON e interagir com bancos, por meio do backend. |
| Modelo e DTOs | Representar dados internos e formatos de entrada/saída da API. Formato e validações ainda dependem do contrato definido. |
| Configuração e tratamento de erros | Reunir configurações e respostas de erro de forma consistente, conforme convenções a serem acordadas. |

Os nomes acima descrevem responsabilidades conceituais, não exigem uma estrutura de pacotes ou classes fechada. A organização proposta de diretórios está na raiz do repositório.

## Separação entre frontend e backend

O frontend não acessará diretamente os bancos nem a DummyJSON. Ele enviará requisições à API própria e receberá respostas JSON. O backend centralizará regras, coordenação das estruturas de dados, persistência e integração externa. Essa separação evita duplicar regras na interface e permite que o servidor forneça os dados operacionais sem exigir que o navegador conheça os detalhes de armazenamento.

## Fluxo geral de informação

1. A pessoa usuária consulta ou altera informações na interface.
2. O frontend envia a operação ao backend por HTTP, com dados JSON quando aplicável.
3. A camada de entrada encaminha a solicitação ao serviço correspondente.
4. O serviço aplica as regras e, para as operações de catálogo, coordena as estruturas de dados previstas.
5. Quando necessário, o backend consulta ou atualiza a persistência por meio do módulo responsável. Na importação externa, o backend busca os dados e faz a transformação para o modelo interno.
6. O backend devolve uma resposta HTTP/JSON e o frontend apresenta o resultado.

Este é um fluxo conceitual. Contrato da API, transações, ordem exata entre estruturas e persistência, tratamento de falhas e política de sincronização ainda precisam ser definidos.

## Organização das estruturas de dados

A Tabela Hash indexará produtos pelo código para operações de acesso direto em média. A Árvore Binária de Busca convencional organizará os mesmos produtos pelo código e permitirá listagem crescente com percurso *in-order*. O `EstoqueManager` é um nome provisório para o componente que deverá coordenar cadastro, consulta, atualização, remoção e consistência entre ambas. A documentação detalhada está em [Estruturas de Dados](../structures/README.md).

## Integração com persistência

A estratégia inicial proposta considera Riak KV para conservar registros brutos importados da DummyJSON e MongoDB como persistência principal do catálogo operacional. O frontend deverá receber os dados do backend, sem consultar a DummyJSON a cada carregamento do catálogo. Cassandra para histórico de movimentações e Neo4j para relacionamentos são possibilidades posteriores. Nenhum desses papéis é requisito oficial confirmado pelo professor; os detalhes estão em [Banco de Dados](../database/README.md).

## Decisões arquiteturais

### Planejadas para esta etapa

- Frontend em HTML, CSS e JavaScript Vanilla.
- Backend em Java com Spring Boot e Maven.
- API REST com comunicação HTTP e JSON.
- Organização do backend como monólito modular.
- Regras de negócio e estruturas de dados implementadas na aplicação Java, fora da concentração nos controllers.
- Sem adoção de microserviços nesta etapa.

### PENDENTE DE DEFINIÇÃO

- Contrato definitivo da API, incluindo recursos, validações e erros.
- Estrutura final de módulos/pacotes, modelo de produto e limites entre serviço, estrutura e persistência.
- Estratégia de consistência entre estruturas em memória e bancos, inclusive comportamento diante de falha parcial.
- Ciclo de vida e reconstrução da Hash e da ABB quando o catálogo for carregado da persistência.
- Bancos, responsabilidades, ambiente de execução e requisitos oficiais de Banco de Dados II.
- Autenticação, autorização, implantação, configuração por ambiente e observabilidade, caso sejam necessários.
- Divisão das responsabilidades entre os integrantes e entre as disciplinas.

As páginas de [Estruturas de Dados](../structures/README.md), [Banco de Dados](../database/README.md), [API](../api/README.md) e [Backlog](../backlog/README.md) complementam esta visão sem fechar as decisões pendentes.
