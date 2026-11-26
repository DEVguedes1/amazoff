# Arquitetura do AmazOFF

## Fonte de referência e escopo

A especificação detalhada está em [`docs/specification/README.md`](../specification/README.md). Este documento resume separação de entregas e fluxo. As três disciplinas têm objetivos e ambientes diferentes; não se deve fundi-las numa arquitetura de execução única sem decisão da equipe.

## Entregas

| Entrega | Organização lógica | Dados e fluxo |
|---|---|---|
| Tecnologias Web | Interface HTML/CSS/JavaScript e serviços JS locais | Serviços manipulam arrays/objetos em memória; estado atualizado reflete-se dinamicamente na tela. Nenhuma API/backend/banco é exigido nessa etapa. |
| Estrutura de Dados e Algoritmos | Coordenador de estoque, ABB e tabela hash implementadas pelo grupo | Mesmo produto lógico referenciado pelas duas estruturas; Hash para consulta por código e ABB para ordenação. Linguagem e localização ainda dependem de confirmação. |
| Banco de Dados II | Aplicação Java/Gradle com interface de terminal, serviços de aplicação e DAO/Repository | Menus simulam leituras/escritas em agregados persistidos em pelo menos dois bancos permitidos. |

## Responsabilidades no módulo BD

- **CLI/menus:** recebem opções e dados, invocam casos de uso e mostram resultado/erro; não implementam regras nem acesso direto aos bancos.
- **Serviços:** coordenam validação e operações de Cliente, Produto, Carrinho e Métricas.
- **Agregados:** delimitam consistência local e formato lógico; sua forma persistida pode variar por banco.
- **DAO/Repository:** executa leituras e escritas para cada agregado/banco escolhido. Não há transação distribuída presumida.
- **Adapters:** encapsulam detalhes de Riak, MongoDB e/ou Cassandra e configuração local.

## Fluxos principais

**Web:** ação na interface → serviço JavaScript → arrays/objetos em memória → renderização dinâmica.

**Estrutura de Dados:** operação → InventoryManager (nome sugerido) → ABB e Hash → resultado. Cadastro/remoção precisam preservar coerência entre as duas estruturas.

**Banco de Dados II:** menu terminal → serviço de aplicação → DAO/Repository → banco(s) local(is) → resultado exibido. Carrinho e métricas seguem a proposta de agregados da especificação integrada; decisões de banco ainda requerem aprovação.

## Estado e decisões

O projeto Java permanece em `amazoff-backend/` e agora usa Gradle, preservando a configuração Spring Boot e a estrutura de diretórios. A entrega BD ainda exige menus de terminal e persistência simultânea em pelo menos dois bancos; esses critérios não são satisfeitos apenas pelo plugin Spring Boot. Não assumir integração HTTP com o frontend Web: não é requisito transcrito.

A distribuição sugerida MongoDB (Cliente/Produto), Riak (Carrinho) e Cassandra (Métricas) está em [`docs/database/README.md`](../database/README.md), como proposta. As configurações e decisões de cluster estão pendentes; detalhes de classes, métodos, regras e critérios estão na [especificação integrada](../specification/README.md).
