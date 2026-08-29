---
trilha: "PARA ENTREVISTAS"
---
**Navegação:** [[MOC — TRILHA PARA ENTREVISTAS]]
← [[PARTE 72 — PERGUNTAS DE ENTREVISTA]] | #trilha/entrevistas | [[PARTE 74 — CHECKLIST DE SYSTEM DESIGN]] →

---
# PARTE 73 — LISTA DE VERIFICAÇÃO DE PROJETO DE SISTEMA

## Fundamentos

Projetar sistemas de software complexos envolve equilibrar múltiplas dimensões: funcionalidade, desempenho, escalabilidade, segurança, manutenibilidade e custo. Uma lista de verificação bem estruturada ajuda arquitetos e engenheiros a garantir que aspectos críticos não sejam esquecidos durante o processo de design e revisão.

Esta parte fornece uma lista de verificação abrangente para avaliar e guiar projetos de arquitetura de sistema. Ela é organizada por categorias de preocupação, com perguntas específicas que podem ser usadas em revisões de arquitetura, planejamento de projetos ou autoavaliação durante o desenvolvimento.

A lista de verificação não é destinada a ser uma lista rígida de "faça ou não faça", mas sim um gatilho para pensamento crítico e discussão em equipe. Adaptar-lhe o foco conforme o contexto do projeto (por exemplo, startup vs empresa estabelecida, sistema crítico de vida vs aplicativo de consumo) é essencial.

> **Nota**: Use esta lista como ponto de partida e a adapte conforme necessário para seu domínio específico, tecnologias escolhidas e restrições do projeto.

## 1. Requisitos e Restrições

Antes de arquitetar, é fundamental compreender claramente o que se está construindo e quais são os limites.

### 1.1. Requisitos Funcionais
- [ ] Quais são as funcionalidades principais que o sistema deve fornecer?
- [ ] Há fluxos de trabalho ou processos de negócio críticos que precisam ser modelados?
- [ ] Quais são os papéis de usuários (ou sistemas) e suas respectivas permissões e ações?
- [ ] Há requisitos regulatórios ou de compliance que impõem funcionalidades específicas?
- [ ] Quais são as integrações com sistemas externos (APIs, bancos de dados, serviços de terceiros) necessárias?

### 1.2. Requisitos Não-Funcionais (Qualidades do Sistema)
- [ ] **Desempenho**: Qual é a latência esperada para operações críticas? Qual é a taxa de transferência (throughput) necessária?
- [ ] **Escalabilidade**: O sistema precisa suportar crescimento no número de usuários, volume de dados ou taxa de transações? Como (verticalmente, horizontalmente)?
- [ ] **Disponibilidade**: Qual é o objetivo de tempo de atividade (uptime)? Há requisitos de recuperação após falha?
- [ ] **Confiabilidade**: Quão tolerante a falhas o sistema deve ser? Há requisitos de perda máxima de dados ou de consistência?
- [ ] **Segurança**: Quais são os riscos de segurança (vazamento de dados, acesso não autorizado, etc.) e quais controles são necessários?
- [ ] **Manutenibilidade**: Quão fácil será entender, modificar e depurar o código? Há padrões de código ou arquitetura que devem ser seguidos?
- [ ] **Escalabilidade de Desenvolvimento**: O arquitetura suporta múltiplas equipes trabalhando em paralelo sem conflitos excessivos?
- [ ] **Custo**: Qual é o orçamento para infraestrutura, licenciamento e operação? Há restrições de custo que afetam escolhas tecnológicas?
- [ ] **Legal e Compliance**: Há requisitos de soberania de dados, retenção, privacidade (LGPD/GDPR) ou outras obrigações legais?

### 1.3. Restrições e Premissas
- [ ] Que tecnologias são impostas ou preferidas pela organização (por exemplo, stack existente, acordos de licenciamento)?
- [ ] Há restrições de equipe (habilidades disponíveis, tamanho da equipe, experiência com certas tecnologias)?
- [ ] Quais são as limitações de infraestrutura (por exemplo, data center próprio, nuvem pública específica, ambientes híbridos)?
- [ ] Há restrições de tempo (prazo de lançamento, janelas de manutenção)?
- [ ] Quais são as premissas sobre o ambiente de operação (por exemplo, confiabilidade da rede, características do hardware)?
- [ ] Há dependências externas (serviços de terceiros, APIs) com SLAs conhecidos ou limitações?

## 2. Arquitetura de Alto Nível

A estrutura geral do sistema, incluindo componentes principais e seus relacionamentos.

### 2.1. Decomposição e Modularidade
- [ ] O sistema foi decomposto em componentes ou serviços com responsabilidades bem definidas?
- [ ] Os limites entre componentes são claros e minimizam acoplamento indesejado?
- [ ] Há uma camada de apresentação (UI/API) separada da lógica de negócio e do acesso a dados?
- [ ] O design segue princípios de coesão alta e baixo acoplamento?
- [ ] Os componentes são independentes o suficiente para serem desenvolvidos, testados e implantados separadamente?

### 2.2. Padrões Arquiteturais
- [ ] Qual padrão arquitetural primário está sendo usado (por exemplo, monolítica, camadas, hexagonal, microsserviços, event-driven, espaço tubular)?
- [ ] Esse padrão é adequado às restrições de escala, equipe e complexidade do problema?
- [ ] Se estiver usando microsserviços, os limites de serviço estão bem definidos (por domínio de negócio)?
- [ ] Se estiver usando arquitetura orientada a eventos, os eventos são bem modelados (como fatos imutáveis) e há tratamento adequado de versionamento?
- [ ] Há uso de padrões complementares (por exemplo, CQRS, repositório, camada de serviço) onde apropriado?

### 2.3. Comunicação e Integração
- [ ] Como os componentes se comunicam (chamadas síncronas de API, mensagens assíncronas, compartilhamento de banco de dados, etc.)?
- [ ] Os protocolos de comunicação são apropriados para o contexto (por exemplo, HTTP/REST, gRPC, WebSocket, filas de mensagens)?
- [ ] Há tratamento adequado de falhas de comunicação (timeouts, retry, circuit breaker)?
- [ ] Se houver comunicação assíncrona, o modelo de entrega (at-least-once, at-most-once, exatamente-once) está claro e implementado corretamente?
- [ ] Há necessidade de tradução ou adaptação de dados entre camadas (por exemplo, DTOs, mapeadores) e isso está bem definido?

### 2.4. Escalabilidade e Performance
- [ ] O projeto identifica gargalos potenciais de desempenho (por exemplo, acesso a banco de dados, chamadas de rede externas, processamento computacional intenso)?
- [ ] Há estratégias de escalabilidade horizontal (particionamento, sharding, réplicas de leitura) para componentes críticos?
- [ ] O uso de caching está bem pensado (o que cachear, por quanto tempo, estratégias de invalidação)?
- [ ] Há consideração de assincronia para operações que não precisam ser imediatas (por exemplo, envio de e-mails, processamento de lote)?
- [ ] O projeto considera o efeito de rede (latência, largura de banda) entre componentes distribuídos?
- [ ] Há planos para teste de carga e modelagem de desempenho antes da implementação em grande escala?

### 2.5. Consistência e Gerenciamento de Dados
- [ ] Qual modelo de consistência está sendo usado (forte, eventual, leitura de sua própria escrita, causal, etc.) e é adequado ao domínio?
- [ ] Se houver múltiplas cópias de dados (réplicas, caches), como a consistência é mantida ou eventualmente alcançada?
- [ ] O projeto aborda o desafio de gravações conflitantes em sistemas distribuídos (por exemplo, resolução de conflitos, last-write-wins, fusão)?
- [ ] Se houver transações que abrangem múltiplos serviços, como a atomicidade é alcançada (por exemplo, duas-fases commit, sagas, idempotência)?
- [ ] O projeto considera o impacto de particionamento de rede (split-brain) e como o sistema se comporta nesse cenário?

## 3. Infraestrutura e Deploy

Como o sistema será executado, operado e mantido em ambientes de produção.

### 3.1. Estratégia de Implantação
- [ ] O sistema será implantado como unidades implantáveis independentes (por exemplo, contêineres, VMs, funções serverless)?
- [ ] Há um pipeline de CI/CD definido (build, teste, staging, produção)?
- [ ] O processo de deploy é automatizado e reprodutível (infraestrutura como código)?
- [ ] Há estratégias de lançamento seguro (por exemplo, blue-green, canary, lançamentos em etapas)?
- [ ] Há planos para reversão rápida (rollback) em caso de problemas após o deploy?

### 3.2. Gerenciamento de Configuração
- [ ] Como as configurações (por exemplo, strings de conexão, flags de recurso, limites) são gerenciadas em diferentes ambientes (dev, teste, produção)?
- [ ] Há separação entre configuração e código, e informações sensíveis (segredos) são armazenadas de forma segura (por exemplo, cofres, variáveis de ambiente criptografadas)?
- [ ] As mudanças de configuração são versionadas e revisáveis?
- [ ] Há mecanismos para recarregar configurações sem reinicialização completa quando apropriado?

### 3.3. Escalabilidade de Infraestrutura
- [ ] A infraestrutura suporta escalonamento automático com base em métricas (por exemplo, uso de CPU, latência da fila)?
- [ ] Há provisionamento de recursos sob demanda (por exemplo, grupos de autoscaling, funções serverless)?
- [ ] O projeto considera o tempo de provisionamento (por exemplo, quanto tempo leva para iniciar uma nova instância) e seu impacto no tratamento de picos de tráfego?
- [ ] Há uso de tecnologias como containers orchestration (Kubernetes, ECS) ou plataformas gerenciadas (App Service, Cloud Run) que facilitam o escalonamento?

### 3.4. Resiliência e Tolerância a Falhas
- [ ] O projeto identifica pontos únicos de falha (SPOFs) e como eles são mitigados (por exemplo, redundância, failover automático)?
- [ ] Há uso de padrões de resiliência (por exemplo, circuito breaker, timeout, retry com backoff exponencial, bulkhead)?
- [ ] Se houver dependências externas, há fallback ou degradação gracioso quando elas estiverem indisponíveis?
- [ ] O projeto considera a possibilidade de falhas correlacionadas (por exemplo, falha de zona de disponibilidade afetando múltiplas instâncias)?
- [ ] Há testes de injeção de falha (por exemplo, engenharia do caos) planejados ou em prática?

## 4. Dados e Persistência

Como a informação é armazenada, acessada e gerenciada ao longo do tempo.

### 4.1. Escolha de Armazenamento
- [ ] Quais tipos de dados serão armazenados (por exemplo, transacionais, logs, arquivos binários, dados de séries temporais)?
- [ ] O tipo de armazenamento escolhido (por exemplo, banco de dados relacional, NoSQL chave-valor, documento, colunar, grafo, data warehouse, object storage) é adequado aos padrões de acesso e consultas necessárias?
- [ ] Se houver múltiplos tipos de armazenamento, os limites de responsabilidade são claros (por exemplo, qual dado vive onde)?
- [ ] Há consideração de custos de armazenamento e acesso (por exemplo, armazenamento frio vs quente, custos de transferência de dados)?

### 4.2. Modelagem de Dados
- [ ] O esquema de dados (tabelas, coleções, documentos) está bem definido e versionado?
- [ ] Há normalização ou desnormalização adequada considerando padrões de leitura e escrita?
- [ ] Índices foram planejados para suportar consultas críticas com desempenho aceitável?
- [ ] Há consideração de integridade de dados (por exemplo, restrições de chave estrangeira, checks, validação no nível da aplicação)?
- [ ] Como as mudanças de esquema serão gerenciadas (por exemplo, migrações, compatibilidade para frente e para trás)?

### 4.3. Acesso e Consultas
- [ ] Os padrões de acesso aos dados são eficientes (por exemplo, evitar N+1 selects, usar junções apropriadas)?
- [ ] Há uso de camadas de acesso a dados (por exemplo, repositórios, mappers) que abstraem detalhes de armazenamento?
- [ ] Se houver consultas complexas, elas foram testadas e otimizadas (por exemplo, planos de execução, uso de índices de cobertura)?
- [ ] Há consideração de limitações do armazenamento escolhido (por exemplo, consultas ad-hoc em alguns NoSQL podem ser caras)?
- [ ] O projeto aborda o risco de injeção (por exemplo, SQL injection, NoSQL injection) através de parametrização ou ORMs seguros?

### 4.4. Backup e Recuperação
- [ ] Há estratégias de backup regular (por exemplo, snapshots, dump lógico) e são testados para recuperação?
- [ ] Qual é o objetivo de ponto de recuperação (RPO) e objetivo de tempo de recuperação (RTO) para diferentes tipos de dados?
- [ ] Os backups são armazenados em local separado (por exemplo, outra região, cópia offline) para proteger contra desastres?
- [ ] Há procedimentos documentados para restauração de dados em caso de corrupção ou exclusão acidental?

## 5. Segurança

Proteção do sistema contra ameaças e garantia de privacidade e integridade dos dados.

### 5.1. Autenticação e Autorização
- [ ] Como os usuários (ou sistemas) se autenticam (por exemplo, senhas, autenticação multifator, tokens, certificados)?
- [ ] Há uso de protocolos padrão (por exemplo, OAuth 2.0, OpenID Connect, SAML, LDAP) quando apropriado?
- [ ] Os tokens de autenticação são armazenados e transmitidos de forma segura (por exemplo, HttpOnly, Secure flags, criptografia)?
- [ ] A autorização é baseada em papéis (RBAC), atributos (ABAC) ou outra modelo adequado?
- [ ] Há princípio do menor privilégio aplicado (usuários e serviços têm apenas as permissões necessárias)?
- [ ] Há proteção contra força bruta (por exemplo, limites de tentativa, bloqueio temporário, CAPTCHA)?

### 5.2. Segurança de Dados
- [ ] Dados sensíveis em repouso são criptografados (por exemplo, discos, backups, arquivos de log)?
- [ ] Dados em trânsito são protegidos (por exemplo, TLS/HTTPS para todas as comunicações externas e internas quando apropriado)?
- [ ] Há gerenciamento seguro de chaves (por exemplo, uso de cofres, rotação de chaves, separação de funções)?
- [ ] Se houver armazenamento de senhas, elas são hash com algoritmo lento e sal (por exemplo, bcrypt, scrypt, Argon2)?
- [ ] Há tokenização ou mascaramento de dados sensíveis (por exemplo, números de cartão de crédito, números de seguridade social) quando apropriado?

### 5.3. Proteção contra Vulnerabilidades Comuns
- [ ] O projeto aborda riscos de injeção (SQL, NoSQL, command injection) através de validação de entrada e parametrização?
- [ ] Há proteção contra cross-site scripting (XSS) em interfaces web (por exemplo, escape de saída, cabeçalhos de segurança como CSP)?
- [ ] Há proteção contra falsificação de solicitação entre sites (CSRF) quando apropriado (por exemplo, tokens anti-CSRF, SameSite cookies)?
- [ ] Há consideração de desserialização insegura (por exemplo, em Java, .NET, PHP) e uso de tipos seguros ou validação?
- [ ] Há uso de componentes com conhecido histórico de segurança e processo de atualização de dependências (por exemplo, scanning de vulnerabilidades)?

### 5.4. Logging e Monitoramento de Segurança
- [ ] Eventos de segurança relevantes (por exemplo, tentativas de login falhadas, mudanças de privilégios, acesso a dados sensíveis) são registrados?
- [ ] Os logs são protegidos contra adulteração e retenidos conforme necessário para auditoria?
- [ ] Há mecanismos de alerta para atividades suspeitas (por exemplo, múltiplas falhas de login, acesso de locais incomuns)?
- [ ] Há planos para resposta a incidentes de segurança (por exemplo, contenção, erradicação, recuperação, lições aprendidas)?

## 6. Observabilidade

Capacidade de entender o estado interno do sistema a partir de suas saídas externas.

### 6.1. Logging
- [ ] Há logging estruturado (por exemplo, JSON) que facilita consulta e análise?
- [ ] Os logs incluem contexto útil (por exemplo, IDs de solicitação, timestamps, IDs de usuário quando apropriado, níveis de gravidade)?
- [ ] Há níveis de log apropriados (por exemplo, DEBUG, INFO, WARN, ERROR) e são usados corretamente?
- [ ] Informações sensíveis (por exemplo, senhas, tokens, dados pessoais) são evitadas nos logs ou mascaradas?
- [ ] Há rotação e retenção de logs definidas com base em requisitos operacionais e de compliance?

### 6.2. Métricas
- [ ] Métricas-chave de desempenho estão sendo coletadas (por exemplo, latência de solicitação, taxa de erro, saturação de recursos)?
- [ ] Há uso de padrões como RED (Rate, Errors, Duration) ou USE (Utilization, Saturation, Errors) para recursos de infraestrutura?
- [ ] Métricas de negócio (por exemplo, taxas de conversão, engajamento do usuário) estão sendo acompanhadas quando relevante?
- [ ] As métricas são expostas em um formato consumível por sistemas de monitoramento (por exemplo, Prometheus, StatsD, CloudWatch)?
- [ ] Há baselines e limites definidos para detecção de anomalias?

### 6.3. Tracing Distribuído
- [ ] Em sistemas distribuídos, há tracing distribuído para acompanhar solicitações ao longo de múltiplos serviços?
- [ ] Há identificação de rastreamento (por exemplo, trace ID, span ID) propagada através de fronteiras de serviço?
- [ ] O tracing ajuda a identificar gargalos de latência e falhas pontuais?
- [ ] Há integração com ferramentas de visualização de tracing (por exemplo, Jaeger, Zipkin, AWS X-Ray)?
- [ ] Há consideração do overhead do tracing e amostragem quando apropriado?

### 6.4. Alertas e Painéis
- [ ] Há alertas configurados para condições críticas (por exemplo, alta taxa de erro, latência acima do threshold, esgotamento de disco)?
- [ ] Os alerts são açãóveis e evitam falsos positivos excessivos (por exemplo, uso de tendências, janelas de tempo)?
- [ ] Há painéis de operacionalidade (dashboards) que mostram a saúde do sistema em tempo real?
- [ ] Há revisão regular de métricas e logs para melhoria contínua (por exemplo, retrospectivas de incidentes, análise de tendências)?

## 7. Testabilidade e Qualidade

Quão fácil é verificar que o sistema funciona corretamente e manter sua qualidade ao longo do tempo.

### 7.1. Estratégia de Teste
- [ ] Há cobertura de teste adequada em diferentes níveis (unitário, integração, contrato, ponta a ponta)?
- [ ] Os testes são automatizados e fazem parte do pipeline de CI/CD?
- [ ] Há teste de componentes em isolamento (por exemplo, usando mocks ou stubs para dependências externas)?
- [ ] Há teste de contratos entre serviços (por exemplo, Pact) para garantir compatibilidade após mudanças?
- [ ] Há teste de desempenho e carga planejado para validar suposições de escalabilidade?
- [ ] Há teste de segurança (por exemplo, scanning de vulnerabilidades, teste de penetração) quando apropriado?

### 7.2. Qualidade de Código e Design
- [ ] Há padrões de codificação definidos e aplicados (por meio de linters, formatters)?
- [ ] Há revisão de código (pull requests) como parte do fluxo de trabalho de desenvolvimento?
- [ ] Há análise estática de código para detectar possíveis bugs, vulnerabilidades ou código morto?
- [ ] Há métricas de qualidade de código (por exemplo, complexidade ciclomática, duplicação) sendo monitoradas?
- [ ] Há documentação técnica (por exemplo, arquitetura, APIs, decisões de design) mantida atualizada?

### 7.3. Gerenciamento de Dependências
- [ ] Dependências de terceiros são atualizadas regularmente para receber correções de segurança e melhorias?
- [ ] Há scanning de vulnerabilidades em dependências (por exemplo, Dependabot, Snyk)?
- [ ] Há uso de travas de versão ou ambientes isolados para garantir builds reprodutíveis?
- [ ] Há consideração de licenciamento de software livre e conformidade com obrigações de distribuição?

### 7.4. Disponibilidade e Recuperabilidade
- [ ] Há testes de failover e recuperação de desastre planejados ou executados periodicamente?
- [ ] O sistema pode ser restaurado a partir de backups em ambiente isolado para validar o processo?
- [ ] Há procedimentos documentados para resposta a incidentes operacionais (por exemplo, degradação de desempenho, interrupção parcial)?
- [ ] Há treinamento da equipe em procedimentos de operação e recuperação?

## 8. Considerações Operacionais e de Equipe

Como o sistema será apoiado, mantido e evoluído pela equipe responsável.

### 8.1. Documentação e Transferência de Conhecimento
- [ ] Há documentação acessível que descreve a arquitetura, componentes, fluxos de dados e procedimentos operacionais?
- [ ] A documentação é mantida atualizada conforme o sistema evolui?
- [ ] Há materiais de onboarding para novos membros da equipe?
- [ ] Há sessões regulares de compartilhamento de conhecimento (por exemplo, brown bags, demonstrações)?
- [ ] Há uso de wikis, repositórios de documentação ou similares para centralizar informação?

### 8.2. Gerenciamento de Mudanças
- [ ] Há processo claro para propor, revisar e aprovar mudanças na arquitetura ou em componentes críticos?
- [ ] Há uso de registros de decisão de arquitetura (ADRs) para documentar escolhas importantes e seu contexto?
- [ ] Há avaliação de impacto antes de mudanças significativas (por exemplo, mudanças de esquema de banco de dados, atualizações de framework major)?
- [ ] Há possibilidades de teste em ambiente de pré-produção ou staging antes de liberação em produção?

### 8.3. Suporte e Incidentes
- [ ] Há rota definida ou responsabilidade clara para suporte em produção (por exemplo, plantão, equipe de operações)?
- [ ] Há runbooks ou guias de operação para cenários comuns (por exemplo, escalar um componente, responder a alto uso de CPU, failover de banco de dados)?
- [ ] Há pós-mortems conduzidos após incidentes significativos para aprender e melhorar?
- [ ] Há métricas de operação sendo acompanhadas (por exemplo, MTBF, MTTR, frequência de incidentes)?
- [ ] Há orçamento ou alocação de tempo para trabalho de redução de dívida técnica e melhorias proativas?

### 8.4. Evolução e Futuro
- [ ] O projeto considera como o sistema poderá evoluir para atender a novos requisitos ou tecnologias emergentes?
- [ ] Há pontos de extensão bem definidos (por exemplo, plugins, hooks, APIs públicas) para facilitar mudanças futuras?
- [ ] Há evitar projetar em cantos mortos (por exemplo, tecnologias obsoletas, escolhas que limitam fortemente opções futuras)?
- [ ] Há revisão periódica da arquitetura para garantir que ela continua adequada às necessidades do negócio e do contexto tecnológico?

## 9. Custos e Planejamento de Capacidade

Entendimento das implicações financeiras e de recursos do projeto.

### 9.1. Estimativa de Custos
- [ ] Há estimativa dos custos de infraestrutura (por exemplo, instâncias de computação, armazenamento, transferência de dados, licenciamento)?
- [ ] Há consideração de custos operacionais (por exemplo, suporte, monitoramento, backups)?
- [ ] Há modelos de custos previstos (por exemplo, pay-as-you-go em nuvem vs custos fixos de data center próprio)?
- [ ] Há sensibilidade analisada (por exemplo, como os custos mudam com aumento de volume de dados ou número de usuários)?

### 9.2. Planejamento de Capacidade
- [ ] Há modelo de uso projetado (por exemplo, número de usuários ativos, transações por segundo, volume de dados) para os próximos 6-12 meses?
- [ ] A infraestrutura está dimensionada para atender a essas projeções com margem de segurança?
- [ ] Há planos para aumento de capacidade (por exemplo, adicionar mais nós, aumentar tamanho de instâncias) conforme necessário?
- [ ] Há monitoramento de utilização de recursos para disparar ajustes de capacidade antes de atingir limites?
- [ ] Há consideração de sazonalidade ou picos previsíveis (por exemplo, eventos de marketing, feriados) no planejamento de capacidade?

### 9.3. Otimização de Custo
- [ ] Há oportunidades para direitos de dimensionamento (rightsizing) de recursos baseado em uso real?
- [ ] Há uso de instâncias spot ou preemptive para cargas tolerantes a interrupção quando apropriado?
- [ ] Há consideração de arquiteturas serverless para cargas esparsas ou imprevisíveis?
- [ ] Há oportunidades de consolidação ou descomissionamento de recursos subutilizados?
- [ ] Há revisão periódica de gastos para identificar desperdícios ou ineficiências?

## 10. Ética, Responsabilidade Social e Sustentabilidade

Considerações além do técnico e econômico que afetam usuários, sociedade e meio ambiente.

### 10.1. Privacidade e Proteção de Dados
- [ ] O projeto respeita princípios de minimização de dados (coleta apenas o necessário)?
- [ ] Há mecanismos para consentimento informado quando dados pessoais são coletados?
- [ ] Há atendimento a direitos de titulares (por exemplo, acesso, correção, exclusão, portabilidade) conforme regulamentações aplicáveis (LGPD/GDPR/CCPA)?
- [ ] Há anonimização ou pseudonimização quando dados são usados para análise ou compartilhados com terceiros?
- [ ] Há avaliação de impacto de proteção de dados (DPIA) quando o projeto envolve alto risco aos direitos e liberdades de indivíduos?

### 10.2. Justiça e Não Discriminação
- [ ] Há avaliação de potenciais vieses em dados ou algoritmos que poderiam levar a tratamento injusto de grupos de usuários?
- [ ] Há teste com conjuntos de dados diversos para descobrir disparidades em resultados?
- [ ] Há estratégias de mitigação de viés (por exemplo, pré-processamento de dados, ajustes algorítmicos, pós-processamento)?
- [ ] Há diversidade na equipe de desenvolvimento para reduzir risco de pontos cegos coletivos?

### 10.3. Segurança e Bem-Estar do Usuário
- [ ] O projeto considera riscos de abuso ou dano potencial (por exemplo, assédio, disseminação de desinformação, vício)?
- [ ] Há mecanismos de relato e moderação quando apropriado (por exemplo, denunciar conteúdo, bloquear usuários)?
- [ ] Há design que promove interações saudáveis e respeito entre usuários?
- [ ] Há fornecimento de controle ao usuário sobre sua experiência (por exemplo, personalização de notificações, opção de saída)?

### 10.4. Sustentabilidade Ambiental
- [ ] Há consideração do consumo de energia associado à execução do sistema (por exemplo, escolha de regiões com energia renovável, otimização de algoritmos para reduzir ciclos de CPU)?
- [ ] Há design para proporcionalidade de recursos (o consumo escala com a carga real, não há sobreprovisionamento constante)?
- [ ] Há uso de comprovação de desempenho por watt ou métricas similares quando relevante?
- [ ] Há considerar o ciclo de vida de hardware (por exemplo, vida útil de servidores, descarte responsável de equipamentos)?

## 11. Checklist de Revisão Pré-Implementação

Antes de começar a codificação em grande escala, use esta versão resumida para validar se os aspectos críticos foram abordados.

### [ ] Requisitos Claros
- [ ] Funcionais e não-funcionais bem compreendidos e documentados.
- [ ] Restrições e premissas explícitas.

### [ ] Arquitetura de Alto Nível
- [ ] Decomposição modular com responsabilidades claras.
- [ ] Padrão arquitetural escolhido justificado pelo contexto.
- [ ] Estratégias de comunicação e integração definidas.
- [ ] Considerações de escalabilidade, desempenho e consistência abordadas.

### [ ] Infraestrutura e Deploy
- [ ] Estratégia de implantação automatizada e reprodutível.
- [ ] Gerenciamento de configuração seguro.
- [ ] Mecanismos de resiliência e tolerância a falhas embutidos.
- [ ] Planejamento de capacidade e escalonamento automático considerados.

### [ ] Dados e Persistência
- [ ] Escolha de armazenamento adequada aos padrões de acesso.
- [ ] Modelagem de dados e índices planejados.
- [ ] Estratégias de backup e recuperação documentadas e testadas.

### [ ] Segurança
- [ ] Autenticação e autorização robustas.
- [ ] Proteção de dados em repouso e em trânsito.
- [ ] Defesas contra vulnerabilidades comuns implementadas.
- [ ] Logging e monitoramento de segurança planejados.

### [ ] Observabilidade
- [ ] Logging estruturado com contexto útil.
- [ ] Métricas-chave sendo coletadas e expostas.
- [ ] Tracing distribuído considerado para sistemas distribuídos.
- [ ] Alertas e painéis operacionais configurados.

### [ ] Testabilidade e Qualidade
- [ ] Estratégia de teste em múltiplos níveis definida.
- [ ] Padrões de qualidade de código e revisão em prática.
- [ ] Gerenciamento de dependências seguro e atualizado.
- [ ] Procedimentos operacionais e de recuperação documentados.

### [ ] Considerações Operacionais
- [ ] Documentação acessível e atualizada.
- [ ] Processo de gerenciamento de mudanças definido.
- [ ] Suporte e resposta a incidentes planejados.
- [ ] Planejamento para evolução futura evitando cantos mortos.

### [ ] Custos e Capacidade
- [ ] Estimativa de custos realizada e alinhada com orçamento.
- [ ] Modelo de uso projetado e dimensionamento de infraestrutura adequado.
- [ ] Oportunidades de otimização de custo identificadas.

### [ ] Ética e Sustentabilidade
- [ ] Privacidade e conformidade regulatória abordadas.
- [ ] Avaliação de viés e justiça considerada.
- [ ] Segurança e bem-estar do usuário pensados.
- [ ] Sustentabilidade ambiental incluída nas decisões de arquitetura.

## 12. Como Usar Esta Lista de Verificação

Esta lista de verificação é mais eficaz quando usada como ferramenta de colaboração e reflexão, não como uma lista de tarefas para marcar mecanicamente.

### Em Revisões de Arquitetura
- Distribua a lista com antecedência para que revisores prepararem comentários.
- Use as categorias como pauta da reunião, focando em áreas de maior risco ou incerteza.
- Registre decisões e ações decorrentes de cada item discutido.

### Durante o Desenvolvimento
- Aplique-a iterativamente: revise a lista a cada marco significativo ou antes de decisões arquiteturais importantes.
- Use-a para orientar reflexões em retrospectivas de sprint ou pós-mortems.
- Adapte-a ao contexto específico do seu projeto, adicionando ou removendo itens conforme necessário.

### Para Autoavaliação
- Antes de solicitar feedback, use a lista para verificar seu próprio trabalho.
- Identifique lacunas onde você precisa de mais informações ou expertise.
- Use-a como guia para pesquisas e aprendizado direcionado.

### Em Mentoria e Treinamento
- Use-a como estrutura para discutir decisões de arquitetura com membros menos experientes da equipe.
- Oriente conversas sobre trade-offs e por que certas escolhas foram feitas em contextos específicos.
- Incentive a questão de "e se?" para explorar limites e alternativas.

## 13. Exemplo de Aplicação: Revisando um Microserviço de Pedidos

Para ilustrar como a lista de verificação pode ser usada, vamos percorrer uma revisão simplificada de um microserviço responsável por criar e gerenciar pedidos em uma plataforma de e-commerce.

### Contexto
- Sistema monolítico sendo dividido em microsserviços.
- Este serviço lida com criação de pedido, validação de estoque, processamento de pagamento e geração de confirmação.
- Esperado volume moderado inicialmente, com crescimento planejado para 10x nos próximos 12 meses.
- Equipe familiarizada com Node.js e bancos de dados relacionais, mas aberta a aprender novas tecnologias.

### Aplicação da Lista de Verificação

#### 1. Requisitos e Restrições
- Funcionais: criar pedido, verificar estoque, processar pagamento, enviar confirmação.
- Não-funcionais: latência < 2s para 95% das solicitações, 99,9% de disponibilidade, conformidade PCI DSS para pagamento.
- Restrições: usar PostgreSQL existente, equipe limitada a dois desenvolvedores backend inicialmente.

#### 2. Arquitetura de Alto Nível
- Decidido por microsserviço com API REST assíncrona (usa fila para pagamento para desacoplar).
- Padrão escolhido: hexagonal (portas e adaptadores) para isolar núcleo de negócio de detalhes de infraestrutura.
- Comunicacao: HTTP síncrono para validação de estoque, fila de mensagens (RabbitMQ) para processamento de pagamento.

#### 3. Infraestrutura e Deploy
- Contêineres Docker implantados em cluster Kubernetes existente.
- Pipeline de CI/CD com testes de unidade, integração e segurança.
- Estratégia de lançamento canário com monitoramento de métricas chave.

#### 4. Dados e Persistência
- PostgreSQL escolhido para dados transacionais fortes necessários para pedidos.
- Esquemo definido com índices em ID de pedido, ID de cliente e status.
- Backups diários com retenção de 30 dias testados trimestralmente.

#### 5. Segurança
- Autenticação via token JWT emitido por serviço de identidade central.
- Autorização baseada em papéis (cliente pode apenas ver/modificar seus próprios pedidos).
- Dados de pagamento nunca tocados diretamente (tokenizados pelo processador de pagamento).
- Logging estruturado sem dados sensíveis, alertas para falhas de autenticação repetidas.

#### 6. Observabilidade
- Métricas de latência, taxa de erro e tamanho da fila coletadas via Prometheus.
- Tracing distribuído implementado com Jaeger para acompanhar solicitações end-to-end.
- Painéis de operacionalidade mostrando saúde do serviço e dependências.
- Alertas configurados para alta latência da fila ou aumento repentino de erros.

#### 7. Testabilidade e Qualidade
- Testes de unidade para lógica de negócio (validação, cálculos de imposto).
- Testes de integração simulando dependências externas (estoque, pagamento) com containers.
- Testes de contrato para garantir compatibilidade com serviços de estoque e pagamento.
- Revisão de código obrigatória, linting e formatador aplicados.

#### 8. Considerações Operacionais
- Documentação armazenada no wiki interno com diagramas de arquitetura e runbooks.
- ADRs usados para decisões como escolha de fila versus chamada síncrona para pagamento.
- Rota de plantão definida, pós-mortems após incidentes.
- Revisão de arquitetura trimestral planejada para avaliar necessidade de mudanças.

#### 9. Custos e Capacidade
- Estimativa de custos baseada em uso atual de Kubernetes e PostgreSQL, com margem para crescimento.
- Escalonamento automático de pods baseado em uso de CPU e tamanho da fila.
- Revisão mensal de custos para identificar oportunidades de rightsizing.

#### 10. Ética e Sustentabilidade
- Minimização de dados aplicada (apenas informações necessárias para pedido armazenadas).
- Nenhum dado sensível armazenado além do essencial; tokenização usada para pagamento.
- Equipe pequena promove colaboração e redução de pontos cegos.
- Cluster Kubernetes otimizado para uso de energia (nodes dimensionados adequadamente, escalonamento em baixa utilização).

### Resultado da Revisão
A lista de verificação ajudou a identificar pontos críticos que, de outra forma, poderiam ter sido esquecidos:
- Necessidade de idempotência no processamento de pagamento devido à natureza assíncrona da fila.
- Importância de testes de contrato para evitar rupturas acidentais ao atualizar serviços de dependência.
- Valor de ADRs para registrar trade-offs (por exemplo, consistência eventual vs imediata na atualização de estoque).
- Lacuna inicial no plano de recuperação de desastre para o banco de dados, posteriormente abordada com réplicas em outra região.

Este exemplo mostra como a lista de verificação funciona como gatilho para pensamento estruturado, garantindo que dimensões importantes do projeto de sistema sejam consideradas explicitamente.

## 14. Conclusão

Uma lista de verificação bem elaborada é uma ferramenta poderosa para arquitetos e engenheiros de software. Ela não substitui o julgamento profissional ou a discussão em equipe, mas garante que áreas críticas de atenção não sejam esquecidas no calor do desenvolvimento ou sob pressão de prazos.

Ao aplicar consistentemente uma lista de verificação como esta — adaptando-a ao seu contexto específico e revisitando-a ao longo do ciclo de vida do sistema — você aumenta a probabilidade de projetar sistemas que sejam não apenas funcionais, mas também resilientes, seguros, operáveis e alinhados com os objetivos de negócio e valores da organização.

Lembre-se de que a arquitetura de software é um exercício de equilíbrio e trade-offs. A lista de verificação ajuda a tornar esses trade-offs explícitos, baseados em informação e alinhados com as prioridades do projeto, em vez de serem decisões implícitas feitas por omissão ou hábito.

> **Próxima Parte**: PARTE 74 — FOLHAS DE CONSULTA RÁPIDA - Folhas de consulta para referência rápida durante o trabalho de arquitetura, contendo resumos de padrões, fórmulas e informações técnicas essenciais.