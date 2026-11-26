# Banco de Dados II — Persistência poliglota

## Requisito

A entrega Aplicação Azul exige aplicação Java com Gradle, terminal, padrão DAO ou Repository e integração simultânea de pelo menos dois entre Riak, MongoDB e Apache Cassandra. A configuração Spring Boot mantida em `amazoff-backend/` não comprova, por si só, o cumprimento dos critérios da aplicação de terminal e da persistência poliglota. A especificação detalhada e os critérios estão em [`docs/specification/README.md`](../specification/README.md).

## Distribuição proposta para revisão

| Agregado | Banco sugerido | Justificativa resumida |
|---|---|---|
| Cliente, endereços | MongoDB | Dados de perfil e endereços agrupados no documento; acesso por cliente/email. |
| Produto e reviews | MongoDB | Documento de catálogo, campos flexíveis e consulta por categoria; definir índices e estratégia de shard para milhões de produtos. |
| Carrinho com TTL 48h | Riak KV | Acesso direto por chave de cliente/carrinho e dado efêmero; concorrência e política de siblings/Vector Clocks precisam ser definidas. |
| Eventos e métricas diárias | Cassandra | Consultas por produto e dia; partição produto+dia e clustering temporal como ponto de partida, sujeito a volume e padrões reais. |

Esta distribuição de três bancos é **PROPOSTA**, não decisão aprovada. A equipe pode optar por dois bancos, mas deve demonstrar todos os requisitos e justificar onde cada agregado ficará. Neo4j não substitui nenhum banco permitido pela especificação.

## Desenho técnico necessário antes de implementar

O relatório deve indicar para cada agregado/variante suas associações, eventos de leitura e escrita, opções de menu, banco, justificativa e configuração. Documentar também:

- MongoDB: shard key, índices, read/write concern e comportamento local; categoria sozinha pode ser baixa cardinalidade como shard key.
- Riak: chave, TTL, N/R/W/DW, siblings/Vector Clocks e resolução/rejeição de conflitos.
- Cassandra: partition key, clustering key, keyspace/replication factor, consistency level, retenção e materialização idempotente das métricas de 24h.
- Métricas: distinguir eventos views/add/remove de quantidade vendida; definir UTC/janelas, deduplicação, atraso tolerado e estratégia incremental ou batch.
- Integração: IDs entre agregados, consistência eventual, disponibilidade/durabilidade e comportamento em indisponibilidade.
- Ambiente: IP:PORTA padrão sobre localhost para os bancos usados e limitações do cluster de desenvolvimento.

Os números de quórum, versões e parâmetros de produção não foram fornecidos e não devem ser inventados como decisão oficial. As recomendações detalhadas, operações de Repository, modelo e pendências estão na [especificação integrada](../specification/README.md).
