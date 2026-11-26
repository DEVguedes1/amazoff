# AmazOFF

Projeto acadêmico de e-commerce/gerenciamento de produtos desenvolvido em três entregas disciplinares distintas: Tecnologias Web, Estrutura de Dados e Algoritmos e Banco de Dados II.

## Especificação de referência

[`docs/specification/README.md`](docs/specification/README.md) é a especificação integrada de referência. Ela separa requisitos oficiais por disciplina, critérios de aceitação, regras, modelo conceitual, classes e operações, propostas de persistência poliglota, matriz de rastreabilidade e pendências. Os requisitos dos anexos oficiais foram transcritos pelo grupo na solicitação de consolidação; os PDFs/DOCX fonte ainda não estão versionados neste repositório e devem ser conferidos.

## Limites entre entregas

- **Tecnologias Web:** JavaScript, arrays e objetos em memória; interface atualizada dinamicamente. A entrega não exige backend, autenticação nem banco de dados.
- **Estrutura de Dados e Algoritmos:** implementação própria de ABB e tabela hash para estoque, busca por código e listagem ordenada.
- **Banco de Dados II:** aplicação Java com Gradle, terminal e DAO/Repository, usando simultaneamente pelo menos dois entre Riak, MongoDB e Cassandra.

As entregas podem compartilhar conceitos de domínio, mas uma não substitui os critérios da outra. O projeto Java mantido em `amazoff-backend/` agora usa Gradle e Java 21; sua configuração Spring Boot preserva o scaffold existente. A presença dessa configuração não substitui os requisitos de interface de terminal, DAOs/Repositories e pelo menos dois bancos da entrega de Banco de Dados II.

## Organização existente

```text
frontend/                 aplicação web
amazoff-backend/          projeto Java/Spring Boot com Gradle
docs/specification/       especificação integrada
docs/architecture/        visão arquitetural
docs/structures/          ABB e tabela hash
docs/database/            agregados e persistência poliglota
docs/api/                 estado/contrato de API, se vier a ser definido
docs/backlog/             planejamento local de trabalho
```

A estrutura de diretórios existente foi preservada; a migração alterou o build do módulo Java dentro de `amazoff-backend/` sem reorganizar frontend ou documentação.

## Build Java

O projeto Java usa o Gradle Wrapper (`amazoff-backend/gradlew`) com Gradle 9.9.2 e toolchain Java 21. As dependências de desenvolvimento e testes estão declaradas em `amazoff-backend/build.gradle`. Esta documentação não afirma que o build ou os testes foram executados nesta atualização.

## Documentos

- [Especificação integrada, classes, métodos, matriz e pendências](docs/specification/README.md)
- [Arquitetura](docs/architecture/README.md)
- [Estruturas de dados](docs/structures/README.md)
- [Persistência poliglota](docs/database/README.md)
- [Diagramas](docs/MODELAGEM_MERMAID.md)
- [Contrato de API](docs/api/README.md)
- [Backlog local](docs/backlog/README.md)
