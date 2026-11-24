# Orientações para uso de ferramentas de IA

## Contexto do AmazOFF

O AmazOFF é um projeto acadêmico de gerenciamento de produtos e estoque, desenvolvido no contexto de Estrutura de Dados, Tecnologias Web e Banco de Dados II. A arquitetura planejada é um monólito modular com frontend em HTML, CSS e JavaScript Vanilla e backend Java com Spring Boot e Maven, expondo uma API REST por HTTP/JSON. As estruturas previstas são uma Tabela Hash com encadeamento separado e uma ABB convencional sem balanceamento automático. Riak KV e MongoDB são propostas iniciais de persistência; Cassandra e Neo4j dependem das exigências oficiais de Banco de Dados II. Consulte o README e os documentos técnicos antes de solicitar trabalho.

## Regras para a assistência

- Não alterar estruturas, arquitetura, tecnologias, requisitos ou decisões aprovadas sem autorização explícita da equipe.
- Distinguir fatos confirmados, propostas e itens **PENDENTES DE DEFINIÇÃO**; não preencher lacunas inventando requisitos.
- Não implementar funcionalidade fora do escopo registrado nem tratar exemplos conceituais como contrato aprovado.
- Não concentrar regras de negócio em controllers nem introduzir frameworks, abstrações ou serviços distribuídos sem decisão da equipe.
- Não criar, alterar ou remover arquivos de aplicação, configuração, dependências ou infraestrutura quando a solicitação se limitar à documentação.
- Preservar arquivos existentes. Antes de alterações técnicas, revisar a solicitação, o estado do repositório e os documentos relacionados; informar conflitos e impactos em vez de presumir uma solução.

## Como solicitar apoio

### Código

Ao solicitar implementação, indique o requisito ou tarefa do backlog, arquivos e módulos autorizados, comportamento esperado, decisões já tomadas e critérios de aceite. Peça que a proposta respeite as estruturas e camadas documentadas. Se faltar uma decisão que altere o desenho, a IA deve destacá-la como pendente e pedir esclarecimento antes de cristalizá-la no código.

### Testes

Indique qual comportamento e camada devem ser verificados, casos de sucesso, erro e limites, ferramentas disponíveis e critério de aceite. Os testes devem validar requisitos definidos e complexidades/invariantes pertinentes, sem introduzir novos requisitos por conta própria.

### Revisões

Informe o escopo da revisão e solicite achados concretos com arquivo, contexto, impacto e forma de reprodução. Diferencie defeitos confirmados de dúvidas ou sugestões; não faça alterações automaticamente quando a solicitação for apenas revisar.

### Documentação

Especifique o documento e a decisão ou comportamento que mudou. Solicite atualização de todas as referências afetadas, preservando conteúdo válido e histórico relevante. Marque como **PROPOSTA** ou **PENDENTE DE DEFINIÇÃO** aquilo que ainda não foi aprovado.

## Manutenção das decisões

Quando a equipe alterar arquitetura, tecnologias, regras de produto, bancos ou escopo, atualize primeiro o documento de referência da área e depois README, diagramas e backlog que dependam da decisão. Registre no pedido quais decisões foram aprovadas e por quem/qual instância da equipe, sem atribuir aprovação que não ocorreu. Se houver divergência entre documentos, sinalize-a e proponha a correção consistente antes de prosseguir.

## Histórico e registro

O histórico de prompts literais anterior a estas orientações foi mantido em [`prompts/AI_PROMPTS.md`](../prompts/AI_PROMPTS.md). Para os próximos usos, a equipe pode registrar data, autor, ferramenta, objetivo, prompt e arquivos resultantes nesse histórico, de acordo com a convenção já existente.
