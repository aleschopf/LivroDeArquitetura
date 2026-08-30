---
trilha: "PARA ENTREVISTAS"
---
**Navegação:** [[MOC — TRILHA PARA ENTREVISTAS]]
← [[PARTE 78 — ROADMAP DE ESTUDO]] | #trilha/entrevistas | [[PARTE 80 — CENÁRIOS DE EVOLUÇÃO ARQUITETURAL]] →

---
# PARTE 79 — PROJETOS PRÁTICOS

## Fundamentos

Projetos práticos são essenciais para consolidar o aprendizado em arquitetura de software. Eles permitem aplicar conceitos teóricos em cenários reais, identificar desafios práticos e desenvolver habilidades de tomada de decisão, comunicação e implementação. Esta parte fornece diretrizes para selecionar, planejar e executar projetos práticos que reforcem o estudo de arquitetura.

### Objetivos dos Projetos Práticos

1. **Aplicação de Conceitos**: transformar teoria em prática, implementando padrões arquiteturais e princípios de design.
2. **Resolução de Problemas Reais**: abordar desafios como escalabilidade, resiliência, segurança e manutenibilidade.
3. **Desenvolvimento de Habilidades**: melhorar capacidade de análise, documentação, comunicação e trabalho em equipe.
4. **Avaliação de Trade-offs**: experimentar diferentes abordagens e avaliar seus prós e contras em um contexto controlado.
5. **Portfólio de Trabalho**: criar artefatos que demonstrem competência em arquitetura de software para entrevistas e progresso de carreira.

## Técnicas

### Técnica 1: Escolha do Projeto
- **Critérios de Seleção**:
  - Complexidade adequada ao nível de habilidade atual (não muito simples, não excessivamente complexo).
  - Relevância para os tópicos de arquitetura que se deseja praticar (ex: microservices, event-driven, CQRS).
  - Disponibilidade de requisitos claros ou possibilidade de defini-los.
  - Potencial para iterar e evoluir o projeto ao longo do tempo.
- **Fontes de Ideias**:
  - Problemas pessoais ou de hobby que precisam de uma solução de software.
  - Estudos de caso de empresas conhecidos (simplificados).
  - Desafios de arquitetura em plataformas como GitHub, LeetCode (design problems) ou exercícios de livros.
  - Requisitos de projetos acadêmicos ou de trabalho adaptados para estudo.

### Técnica 2: Planejamento do Projeto
- **Etapas**:
  1. **Definição de Escopo**: elencar funcionalidades principais e não-funcionais (desempenho, segurança, etc.).
  2. **Análise de Requisitos**: distinguir entre requisitos funcionais e qualidade de arquitetura.
  3. **Visão Arquitetural**: criar diagrama C4 de contexto e containers.
  4. **Seleção de Estilos e Padrões**: escolher um ou mais estilos arquiteturais (monolítico, camadas, microservices, etc.) e justificar.
  5. **Planejamento de Iterações**: dividir o projeto em incrementos que entreguem valor e permitam feedback.
  6. **Definição de Métricas de Sucesso**: estabelecer como medir se o projeto atingiu seus objetivos (ex: tempo de resposta, taxa de erro, facilidade de deploy).
- **Ferramentas**: documentos de requisitos (Markdown), ferramentas de diagramação (draw.io, Miro), ferramentas de gestão (Trello, Jira, ou simplesmente uma lista de tarefas).

### Técnica 3: Implementação Iterativa
- **Práticas**:
  - Começar com uma arquitetura simples (ex: monolítico) e evoluir para mais complexa conforme necessário.
  - Implementar uma funcionalidade por vez, focando em qualidade e clareza do código.
  - Após cada incremento, revisar a arquitetura e refatorar se necessário.
  - Utilizar testes automatizados (unitários, de integração) para garantir que mudanças não quebrem funcionalidades existentes.
  - Documentar decisões arquiteturais em ADRs à medida que surgem.
- **Abordagens Comuns**:
  - **Strangler Fig Applied to Learning**: iniciar com um monolítico e extrair serviços para microservices à medida que se aprende.
  - **Event-Driven from Start**: usar um message broker (como RabbitMQ ou Kafka) desde o início para praticar decoupling.
  - **Camadas Claras**: mesmo em um monolítico, enforcar separação de camadas (presentation, business, data access) para praticar boas práticas.

### Técnica 4: Revisão e Refinamento
- **Atividades**:
  - Revisão de código com foco em arquitetura: verificar se os padrões escolhidos estão sendo seguidos.
  - Análise de métricas: se o projeto inclui instrumentação, observar logs, métricas e traces.
  - Busca de feedback: compartilhar o projeto com colegas ou mentores para obter perspectiva externa.
  - Retrospectiva: após cada iteração, anotar o que funcionou bem e o que poderia ser melhorado.
- **Ferramentas**: pull requests com descrição de mudanças arquiteturais, ferramentas de revisão (GitHub/GitLab reviews), sessões de pair programming ou arquitetura em conjunto.

## Checklist

### Antes de Começar
- [ ] Definir o problema que o projeto irá resolver.
- [ ] Escrever requisitos funcionais claros e mensuráveis.
- [ ] Identificar requisitos não-funcionais importantes (escalabilidade, segurança, usabilidade).
- [ ] Escolher um estilo arquitetural inicial e justificar a escolha.
- [ ] Criar um repositório de versionamento (Git) para o projeto.
- [ ] Planejar a primeira iteração com entregas tangíveis.

### Durante o Desenvolvimento
- [ ] Implementar seguindo os princípios de modularidade e separação de preocupações.
- [ ] Escrever testes automatizados para código crítico.
- [ ] Criar diagramas C4 atualizados conforme a arquitetura evolui.
- [ ] Documentar decisões importantes em ADRs.
- [ ] Revisar o código regularmente para identificar anti-padrões ou oportunidades de melhoria.
- [ ] Medir e monitorar características de qualidade (desempenho, taxas de erro).

### Após Cada iteração
- [ ] Demonstrar a funcionalidade implementada.
- [ ] Revisar se os requisitos não-funcionais estão sendo atendidos.
- [ ] Atualizar documentação e diagramas.
- [ ] Refatorar se necessário para melhorar a arquitetura.
- [ ] Planejar a próxima iteração com base no aprendizado e feedback.

### Conclusão do Projeto
- [ ] Verificar se todos os requisitos principais foram implementados.
- [ ] Produzir um relatório final resumindo as decisões arquiteturais, desafios enfrentados e lições aprendidas.
- [ ] Arquitetar o código para facilitar futuras manutenções ou extensões (se o projeto for continuar).
- [ ] Compartilhar o projeto (se apropriado) em portfólio, GitHub ou em discussões de entrevistas.

## Estudos de Caso

### Caso 1: Plataforma de Tarefas (Todo List) Evoluindo de Monolítico para Microservices
- **Contexto**: desenvolvedor queria praticar migração de arquitetura e começou com um monolítico simples de lista de tarefas.
- **Evolução**:
  - **Iteração 1**: monolítico com camadas claras (API, serviço, repositório) usando Node.js e Express.
  - **Iteração 2**: separação do serviço de autenticação em um microservice independente, comunicando-se via REST.
  - **Iteração 3**: introdução de um message broker (RabbitMQ) para notificações de atualização de tarefa, desacoplando o serviço de tarefas do serviço de notificação.
  - **Iteração 4**: adoção do padrão CQRS para operações de leitura e escrita, usando bancos de dados diferentes para cada modelo.
- **Desafios Enfrentados**:
  - Gerenciamento de dados compartilhados entre serviços (resolvido com eventos de atualização e eventual consistency).
  - Complexidade de deploy e orquestração (resolvida com docker-compose inicialmente, depois explorando Kubernetes básico).
  - Latência aumentada devido à comunicação entre serviços (mitigada com batching e caching onde apropriado).
- **Resultados**:
  - Melhor compreensão dos tradeços entre acoplamento e complexidade operacional.
  - Capacidade de implantar e escalar serviços de tarefas independentemente do serviço de notificação.
  - Código mais organizado e mais fácil de testar em isolamento.

### Caso 2: Sistema de Recomendação de Filmes com Arquitetura Serverless
- **Contexto**: estudante interessado em arquiteturas serverless e processamento de dados em tempo real criou um sistema que recomenda filmes baseado em avaliações de usuários.
- **Componentes**:
  - Função AWS Lambda para receber avaliações via API Gateway e armazená-las em DynamoDB.
  - Função Lambda acionada por stream do DynamoDB para atualizar modelos de recomendação em memória (simplificado para o exercício).
  - API Gateway expõe endpoint para obter recomendações pessoais, acionando uma Lambda que consulta o modelo armazenado em ElastiCache (Redis).
  - Uso de AWS Step Functions para orquestrar o retreinamento periódico do modelo de recomendação em lote usando um container Fargate.
- **Desafios Enfrentados**:
  - Gerenciamento de estado em funções stateless (resolvido usando ElastiCache para compartilhar modelos de recomendação).
  - Depuração de funções Lambda distribuídas (mitigada com estruturamento de logs e uso de AWS X-Ray).
  - Custos inesperados devido a loops invocação (corrigido adicionando limites de concorrência e monitoramento cuidadoso).
- **Resultados**:
  - Experiência prática com construção de aplicações verdadeiramente serverless.
  - Entendimento de como lidar com estado e dependências externas em funções Lambda.
  - Aprendizado sobre monitoramento e otimização de custos em arquiteturas baseadas em eventos.

### Caso 3: Plataforma de Leitura de Livros com CQRS e Event Sourcing
- **Contexto**: arquiteto em treinamento queria explorar padrões avançados de armazenamento e modelagem de domínio em um sistema de gerenciamento de biblioteca pessoal.
- **Arquitetura**:
  - Modelo de escrita (Command) processa comandos como "AdicionarLivro", "MarcarComoLido", "AvaliarLivro".
  - Cada comando gera um ou mais eventos armazenados em um log de eventos (Apache Kafka).
  - Modelo de leitura (Query) consiste em materialized views atualizadas por consumidores dos eventos, armazenadas em tabelas otimizadas para consultas (PostgreSQL para consultas complexas, Redis para caches de contagem).
  - Interface web construída com React consumindo APIs REST que consultam o modelo de leitura.
- **Desafios Enfrentados**:
  - Consistência eventual entre modelo de escrita e leitura (gerenciado com mecanismos de retry e verificação de lacunas no stream de eventos).
  - Modelagem de eventos como fonte da verdade e evolução de esquemas de eventos (abordada com versionamento de eventos e scripts de migração).
  - Complexidade de consultas em um sistema baseado em event signing (supervisionada pela criação de views específicas para casos de uso comuns).
- **Resultados**:
  - Domínio profundo dos padrões CQRS e Event Sourcing, incluindo suas complexidades.
  - Sistema altamente auditável, capaz de reconstruir qualquer estado passado a partir do log de eventos.
  - Flexibilidade para experimentar diferentes modelos de leitura sem afetar o modelo de escrita.

## Tendências Futuras

### 1. Projetos com Infraestrutura como Código (IaC) desde o Início
- **Descrição**: integrar ferramentas como Terraform, Pulumi ou AWS CDK no projeto prático para provisionar e gerenciar a infraestrutura necessária (bancos de dados, filas, funções serverless, clusters de Kubernetes).
- **Benefício**: aprender a tratar infraestrutura como parte do código do projeto, promovendo reprodutibilidade e versionamento completo.
- **Habilidades Relevantes": conhecimento de declarativos de infraestrutura, estratégias de deploy (blue/green, canary), gerenciamento de estado de IaC.

### 2. Incorporação de Observabilidade e Monitoramento Avançado
- **Descrição**: além de logs básicos, implementar distributed tracing (Jaeger, Zipkin), métricas customizadas (Prometheus) e dashboards (Grafana) desde as primeiras iterações.
- **Benefício**: desenvolver habilidade de observar sistemas em produção e entender como a arquitetura afeta a facilidade de depuração e otimização.
- **Habilidades Relevantes": instrumentação de código, configuração de coletores de telemetria, interpretação de traces e métricas em contexto distribuído.

### 3. Projetos Multi-região e Resiliência Avançada
- **Descrição**: simular falhas de região inteira ou provedor de nuvem usando múltiplos clusters ou serviços em diferentes zonas de disponibilidade, praticando estratégias de failover e recuperação de desastre.
- **Benefício": preparação para arquiteturas de alta criticidade onde downtime não é uma opção.
- **Habilidades Relevantes": compreensão de padrões de resiliência (circuit breaker, bulkhead, retry com backoff), gerenciamento de failover de dados e orquestração de recuperação.

### 4. Integração de Práticas de Segurança DevSecOps
- **Descrição**: incorporar varredura de vulnerabilidades, análise de dependências e testes de segurança no pipeline de CI/CD do projeto prático.
- **Benefício": mindset de segurança incorporada desde o início, não como um após pensar.
- **Habilidades Relevants": uso de ferramentas de SCA (Software Composition Analysis), SAST (Static Application Security Testing), DAST (Dynamic Application Security Testing), gerenciamento de segredos e princípios de menor privilégio.

### 5. Arquiteturas Native Cloud com Service Mesh e Política como Código
- **Descrição**: experimentar com service mesh (Istio, Linkerd) para gerenciar tráfego, segurança e observabilidade entre microservices, e usar ferramentas como OPA (Open Policy Agent) para impor políticas de governança.
- **Benefício": experiência com camadas de infraestrutura que transcendem o código da aplicação, preparando para arquiteturas de grande escala em organizações maduras na nativa cloud.
- **Habilidades Relevants": configuração de proxies de sidecar, definição de regras de tráfego e segurança, escrita de políticas em Rego.

## Resumo

Projetos práticos são o elo crítico entre o estudo teórico da arquitetura de software e a capacidade de aplicá-la em situações reais. Ao escolher projetos com escopo adequado, planejar com intenção arquitetural, implementar iterativamente com foco em qualidade e revisar continuamente, o profissional não apenas consolida conhecimentos, mas desenvolve o julgamento necessário para tomar decisões arquiteturais eficazes.

O checklist fornecido guia desde a concepção até a conclusão, garantindo que aspectos importantes não sejam negligenciados. Os estudos de caso ilustram caminhos comuns de evolução, destacando tanto os benefícios quanto os desafios encontrados em cada abordagem. Finalmente, ao observar tendências futuras como IaC integrada, observabilidade avançada, resiliência multi-região, segurança DevSecOps e service mesh, o estudante se prepara não apenas para os desafios de hoje, mas também para as práticas que definirão a arquitetura de software nos próximos anos.

Lembre-se: o valor de um projeto prático não está apenas em seu código funcionando, mas nas lições arquiteturais que ele ensina. Cada decisão, cada trade-off identificado e cada refatoração realizada contribui para o desenvolvimento de um arquiteto mais capaz e reflexivo.