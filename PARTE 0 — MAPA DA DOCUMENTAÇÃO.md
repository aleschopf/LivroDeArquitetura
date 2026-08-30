# PARTE 0 — MAPA DA DOCUMENTAÇÃO

# Arquitetura de Software — Master Guide

> 🎯 **ENTREVISTA — ALTA FREQUÊNCIA**

## objetivo da documentação

Esta documentação tem como objetivo ser uma base de conhecimento profissional completa sobre arquitetura de software, projetada para servir simultaneamente como:
- material de estudo
- guia de referência profissional  
- material de consulta no dia a dia como desenvolvedor
- preparação para entrevistas técnicas
- preparação especificamente para entrevistas de System Design
- material de revisão para profissionais Pleno/Sênior/Staff
- guia para tomada de decisões arquiteturais
- catálogo de padrões, *trade-offs* e boas práticas
- material que ajude a transformar requisitos de negócio em arquiteturas reais

## para quem ela foi criada

- Desenvolvedores que desejam evoluir para arquitetura de software
- Engenheiros de software Pleno e Sênior preparando-se para entrevistas
- Arquitetos de software em formação
- Tech Leads e Engineering Managers que precisam tomar decisões arquiteturais
- Qualquer profissional de tecnologia interessado em entender como sistemas complexos são projetados

## como estudar

1. **Comece pelo MAPA DA DOCUMENTAÇÃO** (esta página) para entender a estrutura geral
2. **Siga a trilha recomendada** conforme seu nível atual (iniciante, intermediário, avançado)
3. **Use o sistema de níveis de dificuldade** indicado em cada seção (🟢 Básico, 🟡 Intermediário, 🟠 Avançado, 🔴 Expert)
4. **Preste atenção nos marcadores de entrevista** para focar no que é realmente cobrado
5. **Faça os exercícios** ao final de cada seção para fixar o conhecimento
6. **Volte constantemente** nesta documentação como referência durante seu trabalho

## ordem recomendada de leitura

### Para iniciantes (trilha iniciante):
1. PARTE 0 — MAPA DA DOCUMENTAÇÃO
2. PARTE 1 — FUNDAMENTOS DE ARQUITETURA DE SOFTWARE
3. PARTE 2 — REQUISITOS E DECISÕES ARQUITETURAIS
4. PARTE 9 — SOLID
5. PARTE 10 — COESÃO E ACOPLAMENTO
6. PARTE 11 — DESIGN PATTERNS (comece pelos creacionais)
7. PARTE 3 — ARQUITETURA MONOLÍTICA
8. PARTE 4 — ARQUITETURA EM CAMADAS
9. PARTE 5 — CLEAN ARCHITECTURE
10. PARTE 6 — HEXAGONAL — PORTS AND ADAPTERS

### Para intermediários (trilha intermediária):
1. PARTE 0 — MAPA DA DOCUMENTAÇÃO (revisão)
2. PARTE 7 — ONION ARCHITECTURE
3. PARTE 8 — DOMAIN-DRIVEN DESIGN
4. PARTE 12 — ARCHITECTURAL PATTERNS
5. PARTE 13 — MICROSERVICES
6. PARTE 14 — SERVICE DISCOVERY
7. PARTE 15 — API DESIGN
8. PARTE 16 — COMUNICAÇÃO ENTRE SERVIÇOS
9. PARTE 17 — MESSAGE BROKERS E EVENT STREAMING
10. PARTE 18 — EVENT-DRIVEN ARCHITECTURE
11. PARTE 19 — DATABASES
12. PARTE 20 — DATABASE INDEXING

### Para avançados (trilha avançada):
1. PARTE 0 — MAPA DA DOCUMENTAÇÃO (revisão)
2. PARTE 21 — TRANSACTIONS
3. PARTE 22 — DISTRIBUTED TRANSACTIONS
4. PARTE 23 — CONSISTENCY
5. PARTE 24 — CAP THEOREM
6. PARTE 25 — PACELC
7. PARTE 26 — DISTRIBUTED SYSTEMS
8. PARTE 27 — REPLICATION
9. PARTE 28 — SHARDING — PARTITIONING
10. PARTE 29 — CONSISTENT HASHING
11. PARTE 30 — CACHING
12. PARTE 31 — LOAD BALANCING
13. PARTE 32 — CDN
14. PARTE 33 — API GATEWAY
15. PARTE 34 — RATE LIMITING
16. PARTE 35 — RESILIENCE
17. PARTE 36 — Confiabilidade e Disponibilidade
18. PARTE 37 — FAULT TOLERANCE
19. PARTE 38 — DISASTER RECOVERY
20. PARTE 39 — OBSERVABILIDADE
21. PARTE 40 — SECURITY ARCHITECTURE
22. PARTE 41 — IAM
23. PARTE 42 — CLOUD ARCHITECTURE
24. PARTE 43 — CONTAINERS
25. PARTE 44 — KUBERNETES
26. PARTE 45 — SERVERLESS

### Para preparação de entrevistas (trilha para entrevistas):
1. PARTE 0 — MAPA DA DOCUMENTAÇÃO
2. PARTE 61 — SYSTEM DESIGN
3. PARTE 62 — FRAMEWORK PARA RESOLVER SYSTEM DESIGN
4. PARTE 63 — SYSTEM DESIGN — PERGUNTAS QUE DEVEM SER FEITAS
5. PARTE 64 — SYSTEM DESIGN — ESTIMATIVAS
6. PARTE 65 — SYSTEM DESIGN — PROBLEMAS CLÁSSICOS
7. PARTE 66 — LOW LEVEL DESIGN
8. PARTE 67 — SYSTEM DESIGN VS LOW LEVEL DESIGN
9. PARTE 68 — ENTREVISTAS DE SYSTEM DESIGN
10. PARTE 69 — RUBRICA DE AVALIAÇÃO
11. PARTE 70 — ERROS EM ENTREVISTAS
12. PARTE 71 — DICAS PARA ENTREVISTAS DE EMPREGO
13. PARTE 72 — PERGUNTAS DE ENTREVISTA
14. PARTE 73 — FOLLOW-UP QUESTIONS DE ENTREVISTADOR
15. PARTE 74 — CHECKLIST DE SYSTEM DESIGN
16. PARTE 75 — CHEAT SHEETS
17. PARTE 76 — TABELAS COMPARATIVAS
18. PARTE 77 — GLOSSÁRIO
19. PARTE 78 — ROADMAP DE ESTUDO
20. PARTE 79 — PROJETOS PRÁTICOS
21. PARTE 80 — CENÁRIOS DE EVOLUÇÃO ARQUITETURAL
22. PARTE 81 — ARQUITETURA PARA SISTEMAS LEGADOS
23. PARTE 82 — MIGRAÇÃO DE ARQUITETURA
24. PARTE 83 — TESTES E ARQUITETURA
25. PARTE 84 — ENGENHARIA DO CAOS
26. PARTE 85 — ARCHITECTURE GOVERNANCE
27. PARTE 86 — DÍVIDA TÉCNICA
28. PARTE 87 — CUSTO DE ARQUITETURA
29. PARTE 88 — STAFF ENGINEER / ARCHITECT THINKING
30. PARTE 89 — COMO PENSAR COMO ARQUITETO
31. PARTE 90 — CENÁRIOS — E SE
32. PARTE 91 — ENTREVISTA FINAL — SIMULAÇÕES COMPLETAS
33. PARTE 92 — BANCO DE QUESTÕES PARA PRÁTICA
34. PARTE 93 — MAPA DE CONHECIMENTO
35. PARTE 94 — DECISION TREES
36. PARTE 95 — ARQUITETURAS COMPLETAS DE REFERÊNCIA
37. PARTE 96 — ARQUITETURA DE SISTEMAS COM IA
38. PARTE 97 — SISTEMAS REAL-TIME
39. PARTE 98 — NETWORKING PARA ARQUITETOS
40. PARTE 99 — OBSERVABILIDADE EM SYSTEM DESIGN
41. PARTE 100 — CHECKLIST FINAL DO ARQUITETO

### Para *System Design* específico (trilha para *System Design*):
1. PARTE 0 — MAPA DA DOCUMENTAÇÃO
2. PARTE 61 — SYSTEM DESIGN
3. PARTE 62 — FRAMEWORK PARA RESOLVER SYSTEM DESIGN
4. PARTE 63 — SYSTEM DESIGN — PERGUNTAS QUE DEVEM SER FEITAS
5. PARTE 64 — SYSTEM DESIGN — ESTIMATIVAS
6. PARTE 65 — SYSTEM DESIGN — PROBLEMAS CLÁSSICOS
7. PARTE 66 — LOW LEVEL DESIGN
8. PARTE 67 — SYSTEM DESIGN VS LOW LEVEL DESIGN
9. PARTE 68 — ENTREVISTAS DE SYSTEM DESIGN
10. PARTE 69 — RUBRICA DE AVALIAÇÃO
11. PARTE 70 — ERROS EM ENTREVISTAS
12. PARTE 71 — DICAS PARA ENTREVISTAS DE EMPREGO
13. PARTE 72 — PERGUNTAS DE ENTREVISTA
14. PARTE 73 — FOLLOW-UP QUESTIONS DE ENTREVISTADOR
15. PARTE 74 — CHECKLIST DE SYSTEM DESIGN
16. PARTE 75 — CHEAT SHEETS
17. PARTE 76 — TABELAS COMPARATIVAS
18. PARTE 77 — GLOSSÁRIO
19. PARTE 99 — OBSERVABILIDADE EM SYSTEM DESIGN
20. PARTE 100 — CHECKLIST FINAL DO ARQUITETO

## mapa completo das seções

Este documento contém exatamente 100 partes, organizadas de forma progressiva conforme descrito nas regras gerais da documentação. Cada parte aborda um tópico específico de arquitetura de software, indo desde os fundamentos mais básicos até conceitos avançados de sistemas distribuídos, System Design e preparação para entrevistas.

As partes estão numeradas de 0 a 100 e devem ser estudadas na ordem recomendada acima, dependendo do seu objetivo e nível atual.

## pré-requisitos

- Conhecimento básico de programação em qualquer linguagem
- Familiaridade com conceitos básicos de algoritmos e estruturas de dados
- Entendimento mínimo de como programas de computador funcionam
- Experiência prática com desenvolvimento de software (mesmo que básica)

## níveis de dificuldade

Cada seção importante contém um indicador de nível de dificuldade:
- 🟢 **Básico**: Conceitos fundamentais, adequados para iniciantes
- 🟡 **Intermediário**: Requer algum conhecimento prévio de programação e arquitetura
- 🟠 **Avançado**: Conceitos complexos, para profissionais com experiência
- 🔴 **Expert**: Tópicos avançados e especializados, para arquitetos experientes


### Legenda dos indicadores usados ao longo da documentação
- 🎯 **ENTREVISTA — ALTA FREQUÊNCIA**: Assuntos muito cobrados em entrevistas
- 🎯 **ENTREVISTA — FREQUENTE**: Assuntos relevantes e recorrentes em entrevistas
- 🎯 **ENTREVISTA — AVANÇADO**: Assuntos que aparecem principalmente em entrevistas avançadas
- 💡 **DICA DE ENTREVISTA**: Dicas práticas para entrevistas
- ⚠️ **ARMADILHA DE ENTREVISTA**: Erros comuns cometidos pelos candidatos
- 🧠 **COMO RACIOCINAR**: Quando o foco é explicar o processo de pensamento
- 🧠 **ESSENCIAL**: Conceitos fundamentais que são imprescindíveis
- ⚠️ **CUIDADO COM OVERENGINEERING**: Alertas sobre quando evitar soluções desnecessariamente complexas
- 🟢 básico / 🟡 intermediário / 🟠 avançado / 🔴 expert: indicadores de nível de dificuldade
- ⭐⭐⭐⭐⭐ muito frequente / ⭐⭐⭐⭐ frequente / ⭐⭐⭐ moderado / ⭐⭐ pouco frequente / ⭐ raro: indicadores de frequência em entrevistas

## glossário de termos usados na documentação

- **HLD**: High-Level Design (design de alto nível)
- **LLD**: Low-Level Design (design de baixo nível)
- **API**: Application Programming Interface
- **SDK**: Software Development Kit
- **ORM**: Object-Relational Mapping
- **OLTP**: Online Transaction Processing
- **OLAP**: Online Analytical Processing
- **ETL**: Extract, Transform, Load
- **CDC**: Change Data Capture
- **SLA**: Service Level Agreement
- **SLI**: Service Level Indicator
- **SLO**: Service Level Objective
- **MTBF**: Mean Time Between Failures
- **MTTR**: Mean Time To Recovery
- **RPO**: Recovery Point Objective
- **RTO**: Recovery Time Objective
- **TPS**: Transactions Per Second
- **QPS**: Queries Per Second
- **RPS**: Requests Per Second
- **DAU**: Daily Active Users
- **MAU**: Monthly Active Users
- **PAU**: Peak Active Users
- **GC**: Garbage Collection
- **JIT**: Just-In-Time compilation
- **AOT**: Ahead-Of-Time compilation
- **DNS**: Domain Name System
- **CDN**: Content Delivery Network
- **LB**: Load Balancer
- **API Gateway**: Ponto de entrada único para múltiplos serviços
- **Service Mesh**: Camada de infraestrutura para comunicação entre serviços
- **CQRS**: Command Query Responsibility Segregation
- **Event Sourcing**: Armazenamento de estado como sequência de eventos
- **Saga**: Padrão para gerenciar transações distribuídas
- **Circuit Breaker**: Padrão para evitar cascata de falhas
- **Bulkhead**: Isolamento de recursos para prevenir falhas em cascata
- **Rate Limiting**: Controle de taxa de requisições
- **Throttling**: Limitação dinâmica de taxa baseado em load
- **Sharding**: Particionamento horizontal de dados
- **Replication**: Cópia de dados para alta disponibilidade
- **Consensus**: Acordo entre nós distribuídos (ex: Raft, Paxos)
- **Quorum**: Número mínimo de nós para tomar decisões
- **Leader Election**: Processo de escolha de nó líder em sistemas distribuídos
- **Heartbeat**: Sinais periódicos para verificar saúde de nós
- **Gossip Protocol**: Protocolo de disseminação de informações em redes P2P
- **CAP Theorem**: Teorema sobre *trade-offs* em sistemas distribuídos
- **PACELC**: Extensão do CAP que considera latência vs consistência
- **Microservices**: Arquitetura baseada em pequenos serviços independentes
- **Monolith**: Arquitetura baseada em uma única aplicação unidade
- **Modular Monolith**: Monolito com boa modularização interna
- **Serverless**: Execução de código sem gerenciamento de servidores
- **FaaS**: Function as a Service
- **PaaS**: Platform as a Service
- **IaaS**: Infrastructure as a Service
- **SaaS**: Software as a Service
- **Kubernetes**: Plataforma de orquestração de containers
- **Docker**: Plataforma de containerização
- **Service Discovery**: Mecanismo para encontrar serviços em rede
- **Load Balancing**: Distribuição de carga entre múltiplas instâncias
- **Auto Scaling**: Ajuste automático de capacidade baseado em demanda
- **Caching**: Armazenamento temporário de dados para acesso rápido
- **Cache Invalidation**: Processo de atualização ou remoção de cache
- **Cache Stampede**: Corrente de requisições quando cache expira
- **Cache Penetration**: Requisições para dados que não existem
- **Write-Through**: Estratégia de escrita simultânea em cache e banco
- **Write-Back**: Estratégia de escrita primeiro no cache, depois no banco
- **Refresh-Ahead**: Pré-carregamento proativo de cache
- **Read-Through**: Leitura através do cache com carregamento sob demanda
- **Async Processing**: Processamento assíncrono de tarefas
- **Message Broker**: Intermediário para comunicação entre sistemas
- **Event Streaming**: Plataforma para processamento de fluxo de eventos
- **Pub/Sub**: Modelo de publicação e assinatura de mensagens
- **Message Queue**: Fila para comunicação assíncrona
- **Dead Letter Queue**: Fila para mensagens que falharam processamento
- **Exactly-Once**: Semântica de entrega garantindo processamento único
- **At-Least-Once**: Semântica garantindo entrega mínima (pode duplicar)
- **At-Most-Once**: Semântica garantindo no máximo uma entrega (pode perder)
- **Idempotency**: Propriedade onde operações múltiplas têm mesmo efeito que uma
- **Eventual Consistency**: Modelo onde dados convergem para consistência com o tempo
- **Strong Consistency**: Modelo onde dados são sempre consistentes imediatamente
- **Causal Consistency**: Modelo que preserva relações de causa-efeito
- **Session Consistency**: Consistência dentro de uma sessão de cliente
- **Read-Your-Writes**: Garantia de que você vê suas próprias escritas
- **Monotonic Reads**: Leituras que não veem dados mais antigos ao longo do tempo
- **Consistent Hashing**: Algoritmo para distribuição uniforme com mínimo de remapping
- **Virtual Nodes**: Técnica para melhor distribuição em consistent hashing
- **Partition Tolerance**: Capacidade de continuar operando apesar de particionamento de rede
- **Byzantine Fault Tolerance**: Resiliência a comportamento arbitrário ou malicioso
- **Leader-Follower**: Modelo onde um nó líder coordena nós seguidores
- **Multi-Leader**: Modelo com múltiplos nós líderes
- **Leaderless**: Modelo sem nós líderes designados
- **Synchronous Replication**: Escrita confirmada em todas as réplicas antes de retornar
- **Asynchronous Replication**: Escrita retornada antes de confirmação em réplicas
- **Semi-Synchronous Replication**: Híbrido entre síncrono e assíncrono
- **Quorum Replication**: Requer confirmação de maioria das réplicas
- **Read Replicas**: Réplicas usadas apenas para operações de leitura
- **Failover**: Troca automática para sistema de backup quando primário falha
- **Active-Active**: Ambos os sistemas processando tráfego simultaneamente
- **Active-Passive**: Sistema ativo processando, passivo em standby
- **Warm Standby**: Sistema de backup parcialmente inicializado
- **Hot Standby**: Sistema de backup totalmente inicializado e sincronizado
- **Pilot Light**: Mínimo de serviços essenciais mantidos ativos para recuperação rápida
- **Disaster Recovery**: Processo de recuperação após desastre maior
- **Business Continuity**: Capacidade de manter operações essenciais durante interrupções
- **Chaos Engineering**: Experimentos controlados para identificar fraquezas
- **Game Days**: Simulações de falha em ambiente controlado
- **Fault Injection**: Introdução intencional de falhas para teste de resiliência
- **Resilience Testing**: Testes focados na capacidade de recuperação
- **Load Testing**: Teste de comportamento sob carga esperada
- **Stress Testing**: Teste de comportamento além da capacidade normal
- **Soak Testing**: Teste de comportamento por período prolongado
- **Spike Testing**: Teste de comportamento sob picos súbitos de carga
- **Observabilidade**: Capacidade de entender estado interno através de outputs
- **Monitoring**: Coleta e visualização de métricas do sistema
- **Logging**: Registro de eventos do sistema
- **Tracing**: Rastreamento de requisições através de múltiplos serviços
- **Metrics**: Medidas quantitativas do comportamento do sistema
- **Logs**: Registros qualitativos de eventos do sistema
- **Traces**: Histórico detalhado de execução de requisições
- **Distributed Tracing**: Tracing em sistemas distribuídos
- **Correlation ID**: Identificador único para rastrear requisição através de serviços
- **OpenTelemetry**: Framework aberto para observabilidade
- **Prometheus**: Sistema de monitoramento e alerta
- **Grafana**: Plataforma de visualização de métricas
- **ELK Stack**: Elasticsearch, Logstash, Kibana para gerenciamento de logs
- **Jaeger**: Sistema de distributed tracing desenvolvido pelo Uber
- **Zipkin**: Sistema de distributed tracing inspirado no Google Dapper
- **SLO**: Service Level Objectives
- **SLI**: Service Level Indicators
- **Error Budget**: Quantidade tolerável de erros baseado em SLO
- **Burn Rate**: Taxa de consumo do error budget
- **Alerting**: Sistema de notificação de problemas
- **Dashboard**: Visualização consolidada de métricas e logs
- **Authentication**: Verificação de identidade
- **Authorization**: Verificação de permissões
- **RBAC**: Role-Based Access Control
- **ABAC**: Attribute-Based Access Control
- **OAuth2**: Framework para autorização delegada
- **OpenID Connect**: Camada de identidade sobre OAuth2
- **JWT**: JSON Web Tokens para transmissão segura de informações
- **mTLS**: Mutual TLS para autenticação bidirecional
- **API Key**: Chave simples para identificação de cliente
- **Session-Based Authentication**: Autenticação baseada em sessão servidor
- **Token-Based Authentication**: Autenticação baseada em tokens (ex: JWT)
- **Multi-Factor Authentication (MFA)**: Autenticação com múltiplos fatores
- **Single Sign-On (SSO)**: Acesso único a múltiplos sistemas
- **Federated Identity**: Identidade compartilhada entre múltiplos domínios
- **Identity Provider (IdP)**: Sistema que gerencia identidades de usuários
- **Service Account**: Conta para serviços automatizados
- **Least Princípio**: Princípio de conceder mínimos privilégios necessários
- **Defense in Depth**: Estratégia de múltiplas camadas de segurança
- **Zero Trust**: Modelo que não confia em nenhuma entidade por padrão
- **Threat Modeling**: Processo de identificação e mitigação de ameaças
- **Attack Surface**: Área exposta a possíveis ataques
- **Vulnerability Assessment**: Identificação de fraquezas de segurança
- **Penetration Testing**: Teste invasivo de segurança
- **Static Application Security Testing (SAST)**: Análise de código fonte
- **Dynamic Application Security Testing (DAST)**: Teste de aplicação em execução
- **Interactive Application Security Testing (IAST)**: Combinação de SAST e DAST
- **Software Composition Analysis (SCA)**: Análise de dependências de terceiros
- **Secrets Management**: Gerenciamento seguro de credenciais e chaves
- **Key Management**: Gerenciamento de chaves criptográficas
- **Hardware Security Module (HSM)**: Dispositivo físico para operações criptográficas
- **Trusted Platform Module (TPM)**: Chip de segurança integrado à placa-mãe
- **Encryption at Rest**: Criptografia de dados armazenados
- **Encryption in Transit**: Criptografia de dados sendo transferidos
- **End-to-End Encryption (E2EE)**: Criptografia que só os extremos podem descriptografar
- **Perfect Forward Secrecy (PFS)**: Chave de sessão não compromete chaves anteriores
- **Transport Layer Security (TLS)**: Protocolo para comunicações seguras
- **Secure Sockets Layer (SSL)**: Predecessor do TLS
- **Datagram Transport Layer Security (DTLS)**: TLS para UDP
- **HTTP Strict Transport Security (HSTS)**: Força uso de HTTPS
- **Content Security Policy (CSP)**: Política para prevenção de XSS
- **Cross-Origin Resource Sharing (CORS)**: Controle de acesso a recursos entre origens
- **SameSite Cookie**: Atributo para controle de envio de cookies
- **HttpOnly Cookie**: Cookie inaccessível a JavaScript
- **Secure Cookie**: Cookie apenas enviado sobre HTTPS
- **CSRF Token**: Token para prevenção de Cross-Site Request Forgery
- **Rate Limiting**: Limitação de requisições por período
- **Bot Management**: Detecção e controle de tráfego automatizado
- **Web Application Firewall (WAF)**: Proteção específica para aplicações web
- **API Gateway Security**: Mecanismos de proteção em nível de API
- **Service Mesh Security**: Segurança na camada de serviço a serviço
- **Container Security**: Proteção de imagens e runtime de containers
- **Image Scanning**: Análise de vulnerabilidades em imagens de container
- **Runtime Protection**: Monitoramento de comportamento em tempo de execução
- **Network Policies**: Regras de comunicação entre pods/containers
- **Pod Security Standards**: Diretrizes de segurança para pods Kubernetes
- **RBAC em Kubernetes**: Controle de acesso baseado em funções no k8s
- **Network Policies em Kubernetes**: Controle de tráfego de rede em k8s
- **PSP (Pod Security Policies)**: Políticas de segurança para pods (depreciado)
- **OPA (Open Policy Agent)**: Motor de políticas genérico
- **Gatekeeper**: Admission controller para OPA no Kubernetes
- **Istio**: Service mesh baseado em Envoy proxy
- **Linkerd**: Service mesh leve baseado em proxy Rust
- **Consul**: Service mesh e discovery da HashiCorp
- **Envoy**: Proxy de alta performance para service mesh
- **gRPC**: Framework RPC baseado em HTTP/2 e Protobuf
- **Protocol Buffers (Protobuf)**: Formato de serialização de dados eficiente
- **REST**: Arquitetura baseada em recursos e HTTP
- **GraphQL**: Linguagem de query para APIs com schema forte
- **gRPC vs REST vs GraphQL**: Comparação de tecnologias de API
- **HTTP/2**: Versão binária do HTTP com multiplexing
- **HTTP/3**: Versão baseada em QUIC do HTTP
- **QUIC**: Protocolo de transporte baseado em UDP
- **Server-Sent Events (SSE)**: Streaming unidirecional servidor→cliente
- **WebSockets**: Comunicação full-duplex persistente
- **Long Polling**: Técnica de simulação de push com polling frequente
- **WebRTC**: Comunicação em tempo real peer-to-peer no navegador
- **MQTT**: Protocolo leve para IoT e mensagens pub/sub
- **CoAP**: Protocolo para dispositivos constrained (REST-like over UDP)
- **AMQP**: Protocolo para message queuing orientado a negócios
- **STOMP**: Protocolo simples de texto para messaging
- **JMS**: API Java para mensagem orientada a objetos
- **Kafka**: Plataforma de streaming de eventos distribuída
- **RabbitMQ**: Message broker tradicional baseado em AMQP
- **Amazon SQS**: Serviço de fila gerenciado da AWS
- **Amazon SNS**: Serviço de pub/sub gerenciado da AWS
- **Azure Service Bus**: Serviço de messaging da Microsoft Azure
- **Google Pub/Sub**: Serviço de pub/sub gerenciado do Google Cloud
- **Apache Pulsar**: Plataforma de messaging e streaming unificada
- **Redis**: Armazenamento estrutura de dados em memória
- **Memcached**: Sistema de cache distribuído simples
- **Amazon ElastiCache**: Serviço de cache gerenciado da AWS
- **Azure Cache for Redis**: Serviço de Redis gerenciado do Azure
- **Google Cloud Memorystore**: Serviço de Redis/Memcached gerenciado do GCP
- **Persistence**: Durabilidade de dados armazenados
- **Volatile Storage**: Armazenamento que perde dados ao desligar
- **Sharding Key**: Chave usada para particionamento de dados
- **Consistent Hashing Ring**: Estrutura de dados para consistent hashing
- **Virtual Nodes**: Nós virtuais para melhor distribuição em hash ring
- **Rebalancing**: Redistribuição de dados após mudança no cluster
- **Hot Partition**: Partição com carga desproporcionalmente alta
- **Cold Partition**: Partição com carga desproporcionalmente baixa
- **Cross-Shard Query**: Consulta que abrange múltiplas partições
- **Distributed Transaction**: Transação que abrange múltiplos nós/serviços
- **Two-Phase Commit (2PC)**: Protocolo clássico para transações distribuídas
- **Three-Phase Commit (3PC)**: Variante do 2PC com fase adicional de preparação
- **Saga Pattern**: Série de transações locais com compensação para falhas
- **Choreography Saga**: Saga coordenada por eventos
- **Orchestration Saga**: Saga coordenada por orquestrador central
- **Compensating Transaction**: Transação que desfaz efeito de transação anterior
- **Idempotent Receiver**: Receptor que trata duplicatas com segurança
- **Duplicate Detecção**: Mecanismo para identificar mensagens duplicadas
- **Outbox Pattern**: Garantia de atomicidade entre mensagem e estado local
- **Transactional Outbox**: Variante do outbox com garantias transacionais
- **Eventual Consistency**: Modelo onde convergência ocorre com tempo
- **Read-After-Write Consistency**: Garantia de leitura imediata após escrita
- **Monotonic Write Consistency**: Escritas vistas na mesma ordem por todos
- **Monotonic Read Consistency**: Leituras vistas na mesma ordem por todos
- **Write-Follows-Read Consistency**: Leitura garante visibilidade em escritas subsequentes
- **Causal Consistency**: Preserva relação de causa-efeito entre operações
- **Session Consistency**: Consistência dentro de sessão de cliente
- **Read-Your-Writes Consistency**: Você sempre vê suas próprias escritas
- **Monotonic Read Consistency**: Leituras não retrocedem no tempo
- **Monotonic Write Consistency**: Escritas vistas na mesma ordem por todos os nós
- **Bounded Staleness**: Limite máximo de desatualização aceitável
- **Consistent Prefix**: Leituras veem prefixo consistente de writes
- **Redis Persistence**: Mecanismos de durabilidade do Redis (RDB, AOF)
- **Redis Replication**: Modelo leader-follower do Redis
- **Redis Sentinel**: Sistema de alta disponibilidade para Redis
- **Redis Cluster**: Sharding automático no Redis
- **Redis Memory Optimization**: Técnicas para uso eficiente de memória
- **Redis Eviction Policies**: Algoritmos para remoção quando memória cheia
- **LRU (*Least Recently Used*)**: Remove menos recentemente usado
- **LFU (*Least Frequently Used*)**: Remove menos frequentemente usado
- **Random**: Remove aleatoriamente
- **TTL (*Time To Live*)**: Expiração automática após tempo determinado
- **Lazy Freeing**: Liberação assíncrona de memória
- **Redis Modules**: Extensões para funcionalidades adicionais
- **RedisJSON**: Armazenamento nativo de JSON no Redis
- **RediSearch**: Engine de busca full-text no Redis
- **RedisTimeSeries**: Armazenamento otimizado para séries temporais
- **RedisBloom**: Filtros probabilísticos (Bloom, Cuckoo) no Redis
- **RedisGeolocalização**: Operações de geolocalização no Redis
- **RedisHyperLogLog**: Contagem aproximada de elementos únicos
- **Clustering vs Sharding**: Distinção entre particionamento lógico e físico
- **Horizontal Pod Autoscaler (HPA)**: Escala pods baseado em uso de CPU/memória
- **Vertical Pod Autoscaler (VPA)**: Ajusta recursos solicitados pelos pods
- **Cluster Autoscaler**: Ajusta número de nós no cluster Kubernetes
- **KEDA (Kubernetes Event-driven Autoscaling)**: Escala baseado em eventos externos
- **Horizontal Scaling**: Adicionar mais instâncias/máquinas
- **Vertical Scaling**: Aumentar recursos de instâncias existentes
- **Diagonal Scaling**: Combinação de horizontal e vertical
- **Elasticity**: Capacidade de escalar automaticamente baseado em demanda
- **Stateless Architecture**: Componentes que não mantêm estado entre requisições
- **Stateful Architecture**: Componentes que mantêm estado entre requisições
- **Session Affinity (*Sticky Sessions*)**: Direcionar mesmo cliente para mesma instância
- **Distributed Tracing**: Rastreamento de requisições em sistemas distribuídos
- **OpenTelemetry**: Framework Vendor-agnostic para observabilidade
- **Jaeger**: Sistema de tracing originalmente do Uber
- **Zipkin**: Sistema de tracing originalmente do Twitter
- **TempoDB**: Banco de dados otimizado para séries temporais
- **Prometheus**: Sistema de monitoring e alerta pull-based
- **VictoriaMetrics**: Alternativa eficiente ao Prometheus
- **TimescaleDB**: Extensão PostgreSQL para séries temporais
- **InfluxDB**: Banco de dados nativo para séries temporais
- **Amazon Timestream**: Serviço gerenciado de séries temporais da AWS
- **Azure Time Series Insights**: Serviço de análise de séries temporais do Azure
- **Google Cloud Monitoring**: Serviço de monitoring do Google Cloud
- **CloudWatch**: Serviço de monitoring e logging da AWS
- **Azure Monitor**: Serviço de monitoring do Azure
- **Logging Libraries**: Frameworks para geração de logs estruturados
- **Log Levels**: Níveis de severidade (DEBUG, INFO, WARN, ERROR, FATAL)
- **Structured Logging**: Logs em formato parseável (JSON, etc.)
- **Log Aggregation**: Coleta centralizada de logs de múltiplas fontes
- **Log Retention**: Política de retenção de logs históricos
- **Log Rotation**: Arquivamento periódico de logs ativos
- **Syslog**: Protocolo padrão para logging de sistema
- **Journald**: Sistema de logging do systemd
- **Fluentd**: Coletor e processador de logs unificado
- **Fluent Bit**: Versão leve do Fluentd para embarcado
- **Logstash**: Processador de logs parte do ELK Stack
- **Filebeat**: Shipper leve para envio de logs ao Logstash
- **Metricbeat**: Shipper leve para coleta de métricas ao Elasticsearch
- **Packetbeat**: Shipper para análise de tráfego de rede
- **Winlogbeat**: Shipper específico para logs de Windows
- **Heartbeat**: Shipper para monitoramento de disponibilidade de serviços
- **Auditbeat**: Shipper para dados de auditoria e segurança
- **Elasticsearch**: Engine de busca e analítica distribuído
- **Logstash**: Pipeline de processamento de dados
- **Kibana**: Interface de visualização para Elasticsearch
- **Beats**: Família de shippers leves para envio de dados
- **ELK Stack**: Elasticsearch, Logstash, Kibana combinados
- **EFK Stack**: Elasticsearch, Fluentd, Kibana (substitui Logstash por Fluentd)
- **Elasticsearch SQL**: Interface SQL para consulta no Elasticsearch
- **KQL (*Kusto Query Language*)**: Linguagem de consulta do Azure Data Explorer
- **PromQL (*Prometheus Query Language*)**: Linguagem de consulta do Prometheus
- **GraphQL**: Linguagem de query e manipulação para APIs
- **gRPC**: Framework RPC de alto desempenho
- **Protocol Buffers**: Mecanismo de serialização de dados eficiente
- **Apache Avro**: Sistema de serialização de dados com schema
- **Apache Parquet**: Formato de armazenamento columnar eficiente
- **Apache ORC**: Formato de armazenamento columnar otimizado
- **Apache Arrow**: Formato de coluna na memória para analytics
- **MessagePack**: Serialização binária eficiente semelhante a JSON
- **CBOR**: Concise Binary Object Representation
- **Protocol Buffers vs Avro vs Parquet**: Comparação de formatos de serialização
- **Schema Evolution**: Capacidade de modificar schema mantendo compatibilidade
- **Backward Compatibility**: Novos consumidores conseguem ler dados antigos
- **Forward Compatibility**: Consumidores antigos conseguem ler dados novos
- **Full Compatibility**: Ambas direções de compatibilidade são garantidas
- **Breaking Change**: Modificação que quebra compatibilidade existente
- **Versioning Strategy**: Abordagem para gerenciar mudanças de schema
- **URI Versioning**: Versão no caminho da API (/v1/resource)
- **Query Parameter Versioning**: Versão como parâmetro da query (?version=1)
- **Header Versioning**: Versão no header customizado (X-API-Version: 1)
- **Media Type Versioning**: Versão no tipo de mídia (application/vnd.myapi.v1+json)
- **Semantic Versioning**: Esquema MAJOR.MINOR.PATCH com significado claro
- **Calendar Versioning**: Versão baseada em data (YY.MM.DD)
- **Sequential Versioning**: Incrementação simples (1, 2, 3, ...)
- **Release Candidate (RC)**: Versão prévia para teste antes do lançamento final
- **Beta Release**: Versão prévia para teste por usuários selecionados
- **Alpha Release**: Versão inicial de teste interno
- **Deprecation**: Marcação de funcionalidade para remoção futura
- **Sunsetting**: Plano definido para descontinuação de funcionalidade
- **End of Life (EOL)**: Data após a qual suporte não é mais fornecido
- **Long Term Support (LTS)**: Versão com suporte estendido por período longo
- **Current Release**: Versão mais recente com todas as funcionalidades
- **Maintenance Release**: Versão focada em correções de bugs e segurança
- **Feature Release**: Versão com novas funcionalidades significativas
- **Breaking Change Release**: Versão que quebra compatibilidade intencionalmente
- **Patch Release**: Versão com correções menores e segurança
- **Minor Release**: Versão com novas funcionalidades não-breaking
- **Major Release**: Versão com mudanças significativas possivelmente breaking
- **Deprecation Period**: Período de aviso antes de remoção de funcionalidade
- **Migration Guide**: Documentação para atualização entre versões
- **Changelog**: Registro detalhado de mudanças entre versões
- **SemVer (*Semantic Versioning*)**: Padrão de versionamento amplamente adotado
- **CalVer (*Calendar Versioning*)**: Versionamento baseado em data
- **ZeroVer**: Versionamento iniciando em 0.y.z para desenvolvimento inicial
- **Release**: Ato de disponibilizar uma versão de software
- **Build**: Processo de compilação e empacotamento do código fonte
- **Compile**: Transformação de código fonte em código executável
- **Link**: Combinação de módulos compilados em executável final
- **Assemble**: Conversão de código assembly em código de máquina
- **Interpret**: Execução direta de código fonte sem compilação prévia
- **JIT Compilation**: Compilação durante execução para melhor performance
- **AOT Compilation**: Compilação antes da execução para startup mais rápida
- **Cross-Compilation**: Compilação para plataforma diferente da de build
- **Toolchain**: Conjunto de ferramentas usadas no processo de build
- **Makefile**: Script de automação para build baseado em dependências
- **CMake**: Sistema de build multiplataforma baseado em Makefile
- **Bazel**: Sistema de build escalável e reprodutível do Google
- **Gradle**: Sistema de build baseado em JVM com DSL Groovy/Kotlin
- **Maven**: Sistema de build baseado em convenção sobre configuração
- **Ant**: Sistema de build baseado em XML predecessor do Maven
- **npm**: Gerenciador de pacotes para JavaScript/Node.js
- **yarn**: Gerenciador de pacotes alternativo para JavaScript
- **pnpm**: Gerenciador de pacotes eficiente para JavaScript (symlink-based)
- **pip**: Gerenciador de pacotes para Python
- **conda**: Gerenciador de pacotes e ambientes para Python/R
- **Poetry**: Gerenciador de pacotes moderno para Python
- **Pipenv**: Gerenciador de pacotes que combina pip e virtualenv
- **RubyGems**: Gerenciador de pacotes para Ruby
- **Bundler**: Gerenciador de dependências para Ruby aplicações
- **NuGet**: Gerenciador de pacotes para .NET ecosystem
- **Chocolatey**: Gerenciador de pacotes para Windows
- **Homebrew**: Gerenciador de pacotes para macOS
- **Linuxbrew**: Versão Linux do Homebrew
- **APK**: Gerenciador de pacotes para Alpine Linux
- **dpkg**: Gerenciador de pacotes para Debian-based Linux
- **rpm**: Gerenciador de pacotes para RedHat-based Linux
- **Container Image**: Pacote contendo aplicação e dependências para execução isolada
- **Docker Image**: Formato de imagem container padronizado
- **OCI Image**: Formato de imagem container aberto (Open Container Initiative)
- **Image Layer**: Camada individual em imagem container
- **Copy-on-Write (CoW)**: Estratégia eficiente para sharing de layers
- **Union Filesystem**: Sistema que combina múltiplos filesystems em uma visão
- **OverlayFS**: Implementação moderna de union filesystem para Linux
- **Aufs**: Implementação mais antiga de union filesystem (depreciada)
- **Device Mapper**: Framework de kernel Linux para gerenciamento de dispositivos
- **Volume**: Persistência de dados além do ciclo de vida do container
- **Bind Mount**: Montagem direta de diretório host no container
- **Named Volume**: Volume gerenciado pelo Docker com nome explícito
- **Anonymous Volume**: Volume gerenciado pelo Docker sem nome explícito
- **Volume Driver**: Plugin para suporte a diferentes tipos de storage
- **Volume Plugins**: Extensões para funcionalidades avançadas de volume
- **Local Driver**: Driver padrão para volumes no host local
- **Cloud Driver**: Driver para serviços de storage em nuvem
- **NFS Driver**: Driver para sistema de arquivos em rede NFS
- **iSCSI Driver**: Driver para dispositivos de bloco via iSCSI
- **Azure File Driver**: Driver para compartilhamento de arquivos Azure
- **AWS EBS Driver**: Driver para volumes de bloco da AWS
- **Google PD Driver**: Driver para discos persistentes do Google Cloud
- **Volume Snapshots**: Pontos no tempo recuperáveis de volumes
- **Volume Backup**: Cópia completa de volume para recuperação
- **Volume Restore**: Restauração de volume a partir de backup
- **Volume Migration**: Movimento de volume entre diferentes storage
- **Volume Cloning**: Cópia idêntica de volume para provisionamento rápido
- **Volume Resizing**: Alteração do tamanho de volume provisionado
- **Volume Encryption**: Criptografia em nível de volume de storage
- **Volume Compression**: Redução de tamanho através de compressão de dados
- **Volume Deduplication**: Eliminação de dados duplicados em nível de storage
- **Volume Thin Provisioning**: Alocação sob demanda ao invés de pré-alocação
- **Volume Provisioning**: Processo de alocação de recursos de storage
- **Volume Deprovisioning**: Liberação de recursos de storage previamente alocados
- **Volume QoS**: Qualidade de serviço para tráfego de storage
- **Volume IOPS**: Operações de entrada/saída por segundo
- **Volume Throughput**: Taxa de transferência de dados em volume
- **Volume Latency**: Tempo de resposta para operações de storage
- **Volume Confiabilidade**: Durabilidade e disponibilidade de storage
- **Volume Scalability**: Capacidade de aumentar capacidade de storage
- **Volume Portability**: Facilidade de mover volume entre sistemas
- **Volume Compatibilidade**: Funcionamento com diferentes sistemas e versões
- **Volume Management Interface**: API para gerenciamento programático de volumes
- **Container Networking**: Comunicação entre containers e com o mundo externo
- **Bridge Network**: Rede padrão Docker para comunicação entre containers
- **Host Network**: Uso direto da rede de host no container
- **Overlay Network**: Rede múltipla-host para swarm mode e serviços
- **Macvlan Network**: Rede que atribui MAC addresses únicos a containers
- **None Network**: Isolamento completo de rede no container
- **Network Plugin**: Extensão para funcionalidades avançadas de rede
- **Flannel**: Rede simples overlay para Kubernetes
- **Calico**: Rede e política de rede avançada para Kubernetes
- **Weave**: Rede resiliente com encryption para Kubernetes
- **Contiv**: Rede empresarial para Kubernetes
- **Romana**: Rede baseada em BGP para Kubernetes
- **Cilium**: Rede e segurança baseada em eBPF para Kubernetes
- **Kube-router**: Solução leve para rede, proxy e policy for Kubernetes
- **NSX-T**: Rede virtualizada da VMware para Kubernetes
- **Antrea**: Rede native for Kubernetes based on Open vSwitch
- **Azure CNI**: Plugin de rede da Azure for Kubernetes
- **AWS VPC CNI**: Plugin de rede da AWS for Kubernetes
- **Google VPC NIC**: Plugin de rede do Google Cloud for Kubernetes
- **SR-IOV**: Virtualization of I/O for direct hardware access
- **DPDK**: Data Plane Development Kit for fast packet processing
- **eBPF**: Extended Berkeley Packet Filter for observability and security
- **XDP**: Express Data Path for extremely fast packet processing
- **TC (*Traffic Control*)**: Linux subsystem for traffic control
- **Queueing Disciplines (qdisc)**: Algorithms for managing network queues
- **Priority Queueing**: Queues based on traffic priority
- **Fair Queueing**: Fair distribution of bandwidth between flows
- **Hierarchical Token Bucket (HTB)**: Traffic control based on classes
- **Hierarchical Fair Service Curve (HFSC)**: Traffic control based on guaranteed service
- **Random Early Detection (RED)**: Early detection of congestion to avoid collapse
- **Explicit Congestion Notification (ECN)**: Congestion signaling at network layer
- **Modular QoS CLI (MQC)**: Command-line interface for QoS in Cisco IOS
- **Class-Based Weighted Fair Queueing (CBWFQ)**: Traffic control based on classes with weights
- **Low Latency Queuing (LLQ)**: Low latency queues for critical traffic
- **Weighted Fair Queueing (WFQ)**: Proportional bandwidth distribution based on weights
- **Class-Based Selective Packet Discard (CBSPD)**: Selective discard based on classes
- **Weighted Random Early Detection (WRED)**: Weighted early detection of congestion
- **Shaping vs Policing**: Traffic rate shaping vs rigid rate limiting
- **Traffic Shaping**: Smoothing traffic for compliance with contracted rate
- **Traffic Policing**: Rigid application of traffic rate limits
- **Rate Limiting**: Rate limiting based on fixed or sliding windows
- **Token Bucket Algorithm**: Algorithm based on consumption of available tokens
- **Leaky Bucket Algorithm**: Algorithm based on constant leakage with limited inputs
- **Fixed Window Counter**: Counter based on fixed time windows
- **Sliding Window Log**: Log based on sliding window of timestamps
- **Sliding Window Counter**: Counter based on sliding window of requests
- **Concurrent Request Limit**: Limitation based on concurrent request count
- **Active Connection Limit**: Limitation based on active connection count
- **Bandwidth Limiting**: Limitation based on bandwidth consumption
- **Request Size Limiting**: Limitation based on request size
- **Payload Size Limiting**: Limitation based on payload size
- **Header Size Limiting**: Limitation based on header size
- **IP Address Limiting**: Limitation based on source IP addresses
- **Geographic Limiting**: Limitation based on geographic location
- **User Agent Limiting**: Limitation based on user agent string
- **Referer Limiting**: Limitation based on referer header
- **Cookie Limiting**: Limitation based on cookie presence/value
- **JWT Claims Limiting**: Limitation based on JWT token claims
- **API Key Limiting**: Limitation based on API key usage
- **OAuth2 Scope Limiting**: Limitation based on authorized scopes
- **Role-Based Limiting**: Limitation based on user roles
- **Attribute-Based Limiting**: Limitation based on user/request attributes
- **Blacklisting**: Explicit denial of access to specified entities
- **Whitelisting**: Explicit allowance of access only to specified entities
- **Rate Limiting by User**: Individual rate limiting by user/identity
- **Rate Limiting by IP**: Individual rate limiting by IP address
- **Rate Limiting by API Key**: Individual rate limiting by API key
- **Rate Limiting by Endpoint**: Specific rate limiting by API endpoint
- **Rate Limiting by Method**: Specific rate limiting by HTTP method
- **Rate Limiting by Header**: Specific rate limiting by request header
- **Rate Limiting by Query Param**: Specific rate limiting by query string parameter
- **Rate Limiting by Request Size**: Specific rate limiting by request size
- **Rate Limiting by Response Size**: Specific rate limiting by response size
- **Rate Limiting by Status Code**: Specific rate limiting by returned status code
- **Rate Limiting by Error Type**: Specific rate limiting by error type returned
- **Rate Limiting by Latency**: Rate limiting based on response latency
- **Rate Limiting by *Throughput***: Rate limiting based on transfer rate
- **Adaptive Rate Limiting**: Rate limiting that adjusts based on observed behavior
- **Dynamic Rate Limiting**: Rate limiting that changes based on real-time conditions
- **Predictive Rate Limiting**: Rate limiting that anticipates spikes based on history
- **Machine Learning Rate Limiting**: Rate limiting that uses ML to detect anomalies
- **Behavioral Rate Limiting**: Rate limiting that analyzes usage patterns to apply rules
- **Geofencing Rate Limiting**: Rate limiting based on entry/exit of defined geographic areas
- **Time-Based Rate Limiting**: Rate limiting that varies by time of day/day of week
- **Event-Based Rate Limiting**: Rate limiting triggered by specific event occurrences
- **Burst Allowance**: Permission for short bursts above rate limit
- **Burst Size**: Maximum quantity allowed in a burst
- **Burst Duration**: Maximum duration allowed for a burst
- **Sustained Rate**: Average rate permitted beyond bursts
- **Peak Rate**: Maximum instantaneous rate permitted
- **Average Rate**: Average rate permitted over observation period
- **Percentile-Based Limiting**: Limitation based on latency/*throughput* percentiles
- **95th Percentile Latency**: Latency below which 95% of requests fall
- **99th Percentile Latency**: Latency below which 99% of requests fall
- **Median Latency**: Median latency (50th percentile)
- **Average Latency**: Arithmetic mean of observed latencies
- **Min Latency**: Minimum observed latency
- **Max Latency**: Maximum observed latency
- **Latency Jitter**: Variation in observed latency
- ***Throughput* Percentile**: *Throughput* below which certain percentage falls
- **Average *Throughput***: Arithmetic mean of observed *throughput*
- **Min *Throughput***: Minimum observed *throughput*
- **Max *Throughput***: Maximum observed *throughput*
- ***Throughput* Variability**: Variation in observed *throughput*
- **Request Size Distribution**: Statistical distribution of request sizes
- **Average Request Size**: Mean of observed request sizes
- **Min Request Size**: Minimum observed request size
- **Max Request Size**: Maximum observed request size
- **Request Size Variability**: Variation in observed request sizes
- **Response Size Distribution**: Statistical distribution of response sizes
- **Average Response Size**: Mean of observed response sizes
- **Min Response Size**: Minimum observed response size
- **Max Response Size**: Maximum observed response size
- **Response Size Variability**: Variation in observed response sizes
- **Status Code Distribution**: Statistical distribution of returned status codes
- **Success Rate (2xx)**: Percentage of requests with 2xx status codes
- **Client Error Rate (4xx)**: Percentage of requests with 4xx status codes
- **Server Error Rate (5xx)**: Percentage of requests with 5xx status codes
- **Redirect Rate (3xx)**: Percentage of requests with 3xx status codes
- **Informational Rate (1xx)**: Percentage of requests with 1xx status codes
- **Error Rate**: Total percentage of requests with error (4xx+5xx)
- **Availability**: Percentage of time system is operational
- **Uptime**: Accumulated time system has been operational
- **Downtime**: Accumulated time system has been unavailable
- **Mean Time Between Failures (MTBF)**: Average time between consecutive failures
- **Mean Time To Recovery (MTTR)**: Average time to recover after failure
- **Failure Rate**: Failure occurrence rate per time unit
- **Confiabilidade**: Probability of operation without failure for specified period
- **Survival Function**: Probability of surviving beyond specified time
- **Hazard Function**: Instantaneous failure rate given survival until then
- **Weibull Distribution**: Common statistical model for time-to-failure
- **Exponential Distribution**: Model for events with constant occurrence rate
- **Normal Distribution**: Statistical bell curve model for continuous variables
- **Log-Normal Distribution**: Distribution where logarithm is normally distributed
- **Poisson Distribution**: Model for number of events in fixed interval
- **Binomial Distribution**: Model for successes in fixed number of trials
- **Geometric Distribution**: Model for trials until first success
- **Negative Binomial Distribution**: Model for trials until r-th success
- **Hypergeometric Distribution**: Model for samples without replacement from finite population
- **Uniform Distribution**: Model where all outcomes are equally probable
- **Triangular Distribution**: Model with known minimum, maximum, and mode
- **PERT Distribution**: Triangular variant used in project estimates
- **Monte Carlo Simulation**: Estimation technique using random sampling
- **Bootstrapping**: Estimation technique using resampling with replacement
- **Jackknife**: Estimation technique leaving out one observation at a time
- **Cross-Validation**: Estimation technique partitioning data for training/testing
- **K-Fold Cross-Validation**: Division into k equal parts for cross-validation
- **Leave-One-Out Cross-Validation**: Each observation becomes test set once
- **Stratified Cross-Validation**: Preserving class proportions in data divisions
- **Time Series Cross-Validation**: Validation respecting temporal data order
- **Rolling Window Cross-Validation**: Validation using sliding window on data
- **Expanding Window Cross-Validation**: Validation with expanding window on data
- **Parametric Test**: Statistical test assuming specific data distribution
- **Non-Parametric Test**: Statistical test not assuming specific data distribution
- **T-Test**: Comparison of means between two groups
- **ANOVA**: Analysis of variance for comparing multiple groups
- **Chi-Square Test**: Test of association between categorical variables
- **Mann-Whitney U Test**: Non-parametric test for comparing two groups
- **Wilcoxon Signed-Rank Test**: Non-parametric test for related pairs
- **Kruskal-Wallis Test**: Non-parametric test for comparing multiple groups
- **Fisher's Exact Test**: Exact test for 2x2 contingency tables
- **McNemar's Test**: Test for changes in related dichotomous pairs
- **Sign Test**: Non-parametric test based on sign counting
- **Pearson Correlation**: Linear correlation measure between variables
- **Spearman Correlation**: Rank-based correlation of values
- **Kendall Tau**: Concordance measure based on ordered pairs
- **Point-Biserial Correlation**: Correlation between continuous and dichotomous variables
- **Phi Coefficient**: Association measure for dichotomous variables
- ***Cramér's V***: Association measure for general contingency tables
- **Contingency Coefficient**: Association measure based on chi-square
- **Odds Ratio**: Odds ratio between groups exposed to different factors
- **Relative Risk**: Probability ratio between groups exposed to factors
- **Attributable Risk**: Absolute probability difference between exposed groups
- **Population Attributable Risk**: Proportion of disease attributable to exposure in population
- **Confidence Interval**: Interval containing true parameter with certain probability
- **Prediction Interval**: Interval containing future observation with certain probability
- **Tolerance Interval**: Interval containing certain population proportion with probability
- **Standard Error**: Standard deviation of sampling distribution of an estimator
- **Margin of Error**: Quantity added/subtracted to create confidence interval
- **Statistical Significance**: Probability of observing effect as large as found if null true
- **P-Value**: Probability of obtaining result as extreme as observed if null true
- **Alpha Level**: Statistical significance threshold (typically 0.05)
- **Beta Error**: Probability of failing to reject null when alternative true (type II)
- **Power**: Probability of rejecting null when alternative true (1 - beta)
- **Effect Size**: Standardized magnitude of observed effect
- **Cohen's d**: Mean difference in pooled standard deviation units
- **Hedges' g**: Adjusted Cohen's d variant for small samples
- **Glass's Δ**: Mean difference in control group standard deviation
- **Pearson's r**: Pearson linear correlation coefficient
- **Coefficient of Determination (R²)**: Proportion of variance explained by model
- **Adjusted R²**: R² adjusted for number of predictors in model
- **Root Mean Square Error (RMSE)**: Square root of mean squared errors
- **Mean Absolute Error (MAE)**: Mean of absolute errors
- **Mean Absolute Percentage Error (MAPE)**: Mean of absolute percentage errors
- **Symmetric MAPE (sMAPE)**: Symmetric version of MAPE for values near zero
- **Mean Absolute Scaled Error (MASE)**: Absolute mean error scaled by naive model error
- **Mean Directional Accuracy (MDA)**: Percentage of predictions with correct direction
- **Root Mean Square Log Error (RMSLE)**: Square root of mean squared log errors
- **Mean Squared Log Error (MSLE)**: Mean of squared log errors
- **Mean Percentage Error (MPE)**: Mean of percentage errors (can cancel)
- **Mean Absolute Error (MAE)**: Mean of absolute errors (always positive)
- **Mean Squared Error (MSE)**: Mean of squared errors
- **Root Mean Squared Error (RMSE)**: Square root of MSE
- **Mean Bias Error (MBE)**: Mean of errors (can indicate systematic bias)
- **Mean Percentage Error (MPE)**: Mean of percentage errors
- **Mean Absolute Percentage Error (MAPE)**: Mean of absolute percentage errors
- **Mean Absolute Scaled Error (MASE)**: MAE divided by MAE of naive model
- **Mean Directional Accuracy (MDA)**: Percentage of changes in correct direction
- **Mean Absolute Directional Error (MADE)**: Mean of absolute directional errors
- **Root Mean Squared Log Error (RMSLE)**: RMSE applied to logarithmic values
- **Mean Squared Log Error (MSLE)**: MSE applied to logarithmic values
- **Mean Percentage Error (MPE)**: Mean of percentage errors
- **Mean Absolute Error (MAE)**: Mean of absolute errors
- **Mean Squared Error (MSE)**: Mean of squared errors
- **Root Mean Squared Error (RMSE)**: Square root of MSE
- **Mean Absolute Scaled Error (MASE)**: MAE divided by MAE of reference model
- **Mean Directional Accuracy (MDA)**: Percentage of predictions with correct direction
- **Hit Rate**: Percentage of correct predictions in binary classification
- **Miss Rate**: Percentage of incorrect predictions in binary classification
- **False Positive Rate (FPR)**: Percentage of actual negatives classified as positive
- **True Positive Rate (TPR)**: Percentage of actual positives classified as positive (sensitivity)
- **True Negative Rate (TNR)**: Percentage of actual negatives classified as negative (specificity)
- **False Negative Rate (FNR)**: Percentage of actual positives classified as negative
- **Precision**: Percentage of predicted positives that are actually positive
- **Recall**: Percentage of actual positives that were predicted as positive (same as TPR)
- **F1 Score**: Harmonic mean of precision and recall
- **Fβ Score**: Generalization of F1 with weight β for recall vs precision
- **Accuracy**: Percentage of total correct classifications
- **Error Rate**: Percentage of total incorrect classifications
- **Specificity**: TNR (same as above)
- **Sensitivity**: TPR (same as above)
- **Positive Predictive Value (PPV)**: Same as precision
- **Negative Predictive Value (NPV)**: Percentage of predicted negatives that are actually negative
- **False Discovery Rate (FDR)**: Percentage of predicted positives that are false positives
- **False Omission Rate (FOR)**: Percentage of predicted negatives that are false negatives
- **Positive Likelihood Ratio (PLR)**: TPR divided by FPR
- **Negative Likelihood Ratio (NLR)**: FNR divided by TNR
- **Diagnostic Odds Ratio (DOR)**: Ratio of positive and negative likelihood ratios
- **Youden's J Statistic**: Sensitivity + specificity - 1
- **Matthews Correlation Coefficient (MCC)**: Balanced measure for binary classification
- **Informedness**: Sensitivity + specificity - 1 (same as Youden's J)
- **Markedness**: PPV + NPV - 1
- **AUC (Area Under Curve)**: Area under ROC curve
- **ROC Curve**: Curve of true positive vs false positive at various thresholds
- **PRC Curve**: Curve of precision vs recall at various thresholds
- **Brier Score**: Mean of squared errors of predicted probability
- **Log Loss**: Logarithmic loss in probabilistic classification
- **Hinge Loss**: Loss function for SVMs
- **Exponential Loss**: Loss function for boosting
- **Logistic Loss**: Loss function for logistic regression
- **Poisson Loss**: Loss function for count data
- **Quantile Loss**: Loss function for quantile regression
- **Huber Loss**: Robust loss function resistant to outliers
- **Tukey's Biweight Loss**: Very robust loss function resistant to outliers
- **Support Vector Machine (SVM)**: Margin-based classifier
- **Kernel Trick**: Technique for operating in high-dimensional spaces implicitly
- **Linear SVM**: SVM with linear kernel
- **Polynomial Kernel**: Kernel based on polynomial of features
- **Radial Basis Function (RBF) Kernel**: Kernel based on Euclidean distance
- **Sigmoid Kernel**: Kernel based on sigmoid function
- **String Kernel**: Kernel for character sequences
- **Tree Kernel**: Kernel for tree structures
- **Graph Kernel**: Kernel for graph structures
- **Decision Tree**: Model based on hierarchical decision rules
- **Classification and Regression Tree (CART)**: Tree for classification and regression
- **ID3**: Tree construction algorithm based on information gain
- **C4.5**: Evolution of ID3 with continuous value handling
- **C5.0**: Commercial version of C4.5 with improvements
- **Random Forest**: Ensemble of decision trees with sampling and randomness
- **Extremely Randomized Trees**: Random Forest variant with more random splits
- **Gradient Boosting**: Additive model optimizing loss function via weak stages
- **AdaBoost**: Adaptive boosting adjusting weights based on errors
- **Gradient Boosting Machine (GBM)**: Generic gradient boosting implementation
- **XGBoost**: Optimized and scalable gradient boosting implementation
- **LightGBM**: Histogram-based gradient boosting for speed and efficiency
- **CatBoost**: Gradient boosting handling categorical features well
- **Stacking (Stacked Generalization)**: Model combining predictions of multiple base models
- **Blending**: Simpler stacking variant with holdout validation
- **Voting Ensemble**: Simple combination of predictions by voting
- **Bagging (Bootstrap Aggregating)**: Ensemble of models trained on bootstrap samples
- **Pasture**: Ensemble technique using models with different hyperparameters
- **Random Subspace**: Ensemble using random feature subsets
- **Rotation Forest**: Ensemble using random PCA transformations
- **ExtraTrees**: Extremely random variant of Random Forest
- **Isolation Forest**: Anomaly detection algorithm based on isolation
- **Local Outlier Factor (LOF)**: Local density-based deviation measure
- **DBSCAN**: Density-based clustering algorithm with outlier detection
- **OPTICS**: Ordering points to identify clustering structure
- **Hierarchical Clustering**: Hierarchical clustering based on similarity
- **Agglomerative Clustering**: Bottom-up clustering starting with individual points
- **Divisive Clustering**: Top-down clustering starting with entire set
- **Single Linkage**: Cluster distance defined by closest point pair
- **Complete Linkage**: Cluster distance defined by farthest point pair
- **Average Linkage**: Cluster distance defined by mean of all distances
- **Centroid Linkage**: Cluster distance defined by distance between centroids
- **Ward's Method**: Minimizes increase in total intra-cluster variance
- **K-Means**: Partitioning clustering algorithm based on centroids
- **K-Medoids**: K-Means variant using actual points as centers
- **Fuzzy C-Means**: Clustering where points can belong to multiple clusters with degrees
- **Gaussian Mixture Model (GMM)**: Models data as mixture of Gaussian distributions
- **Mean Shift**: Clustering algorithm based on mode seeking
- **Spectral Clustering**: Clustering using eigenvectors of Laplacian matrix
- **Affinity Propagation**: Clustering based on message exchange between points
- **BIRCH**: Scalable clustering algorithm for large datasets
- **CURE**: Clustering using well-spaced representatives in cluster
- **CHAMELEON**: Clustering that merges based on proximity and interconnectivity
- **DENCLUE**: Density-based clustering using attractor functions
- **CLIQUE**: Subspace clustering identifying dense clusters in subspaces
- **PROJECTED CLUSTERING**: Subspace clustering considering projections in specific subspaces
- **SUBCLU**: Subspace clustering based on density-relevance in subspaces
- **DBSCAN Variants**: DBSCAN variations for different requirements
- **HDBSCAN**: Hierarchical DBSCAN allowing variable density clusters
- **OPTICS Xi**: Cluster extraction from OPTICS reachability plot
- **Gaussian Mixture Models**: Probabilistic data modeling as mixture of Gaussians
- **Variational Inference**: Technique for approximating intractable posterior distributions
- **Markov Chain Monte Carlo (MCMC)**: Sampling complex distributions via Markov chains
- **Metropolis-Hastings**: MCMC algorithm based on acceptance/rejection of proposals
- **Gibbs Sampling**: Special case of Metropolis-Hastings with conditional sampling
- **Slice Sampling**: Uniform sampling under defined probability density level
- **Hamiltonian Monte Carlo (HMC)**: Sampling using Hamiltonian physics for efficient exploration
- **No-U-Turn Sampler (NUTS)**: HMC variant avoiding unnecessary backtracking
- **Variational Autoencoder (VAE)**: Generative model learning latent data representation
- **Generative Adversarial Network (GAN)**: Two competing networks: generator vs discriminator
- **Deep Convolutional GAN (DCGAN)**: GAN using convolutions for improved quality
- **Conditional GAN (cGAN)**: GAN conditioned on additional information (e.g., labels)
- **StyleGAN**: GAN generating high-quality images with style control
- **StyleGAN2**: StyleGAN evolution with reduced artifacts
- **StyleGAN3**: Further StyleGAN evolution with additional artifact reduction
- **Progressive Growing GAN**: GAN that gradually increases resolution during training
- **CycleGAN**: GAN for image-to-image translation without pairing
- **Pix2Pix**: GAN for image-to-image translation with pairing
- **Super-Resolution GAN**: GAN that increases image resolution
- **Inpainting GAN**: GAN that fills missing regions in images
- **Text-to-Image GAN**: GAN that generates images from text descriptions
- **Image-to-Text GAN**: GAN that generates text descriptions from images
- **Video GAN**: GAN that generates or processes videos
- **3D GAN**: GAN that works with 3D data
- **Graph GAN**: GAN that generates or processes graph structures
- **Recurrent GAN (RGAN)**: GAN using recurrent components for sequential data
- **Variational GAN (VGAN)**: Combination of VAE and GAN for improved stability
- **Adversarial Autoencoder (AAE)**: Adversarially trained autoencoder
- **Wasserstein GAN (WGAN)**: WGAN based on Wasserstein distance for improved stability
- **WGAN with Gradient Penalty (WGAN-GP)**: WGAN with gradient penalty for improved convergence
- **Energy-Based Model (EBM)**: Model defining energy of configurations and sampling low energy
- **Generative Stochastic Network (GSN)**: Network learning to generate data via Markov chains
- **Neural Autoregressive Distribution Estimator (NADE)**: Model estimating conditional distribution given predecessors
- **Masked Autoencoder for Distribution Estimation (MADE)**: Autoencoder estimating distribution via masks
- **Pixel Recurrent Neural Network (PixelRNN)**: Neural network generating images pixel by pixel with context
- **Pixel Convolutional Neural Network (PixelCNN)**: Neural network generating images pixel by pixel with convolutional context
- **Gated PixelCNN**: PixelCNN with gating mechanisms for improved control
- **PixelCNN++**: Evolved PixelCNN with improved modeling
- **Image GAN**: GAN specific for image generation and manipulation
- **Video GAN**: GAN specific for video generation and processing
- **Audio GAN**: GAN specific for audio generation and processing
- **Text GAN**: GAN specific for text generation and processing
- **3D GAN**: GAN specific for 3D data generation and processing
- **Graph GAN**: GAN specific for graph structure generation and processing
- **Point Cloud GAN**: GAN specific for point cloud generation and processing
- **Mesh GAN**: GAN specific for mesh generation and processing
- **Style Transfer**: Technique for applying style of one image to content of another
- **Neural Style Transfer**: Style transfer using deep neural networks
- **Gatys et al. (2015)**: Seminal paper introducing neural style transfer
- **Johnson et al. (2016)**: Real-time approach for neural style transfer
- **Ulyanov et al. (2016)**: Normalized instance for better control in style transfer
- **Chen et al. (2016)**: Style transfer based on Markov random fields
- **Li et al. (2018)**: Style transfer based on convolutional decoding
- **Zhang et al. (2019)**: Style transfer based on attention and transformers
- **Karras et al. (2019)**: StyleGAN2 - improvements in image generation architecture
- **Karras et al. (2020)**: Analyzing and improving StyleGAN2
- **Karras et al. (2021)**: Alias-free image generation with StyleGAN3
- **Esser et al. (2021)**: Variational autoencoder with improved perceptual loss
- **Rombach et al. (2021)**: High-resolution image synthesis with latent diffusion
- **Ho et al. (2022)**: Classifier-guided diffusion for better control
- **Song et al. (2020)**: Denoising diffusion probabilistic models (DDPM)
- **Nichol & Dhariwal (2021)**: Improved denoising diffusion probabilistic models
- **Zhang & Chen (2023)**: DPM-Solver: Fast ODE solver for diffusion modeling
- **Lu et al. (2022)**: DPM-Solver++: Further accelerated solver for diffusion modeling
- **Karras et al. (2022)**: Elucidating the diffusion project space
- **Bao et al. (2023)**: Consistency models for additional acceleration in generation
- **Song et al. (2023)**: Consistency models for image generation
- **Liu et al. (2023)**: Consistency models for data synthesis
- **Meng et al. (2023)**: Consistency models for data translation
- **Autoregressive Models**: Models where current value depends on previous values
- **Moving Average Models**: Models where current value depends on previous errors
- **ARIMA**: Autoregressive Integrated Moving Average for time series
- **SARIMA**: Seasonal ARIMA for data with seasonal patterns
- **ARFIMA**: Autoregressive Fractionally Integrated Moving Average for long-term dependence
- **GARCH**: Generalized Autoregressive Conditional Heteroskedasticity for variable volatility
- **EGARCH**: Exponential GARCH for volatility asymmetry
- **TGARCH**: Threshold GARCH for threshold effects on volatility
- **FIGARCH**: Fractionally Integrated GARCH for long-term dependence in volatility
- **SV Estimate**: Stochastic volatility model
- **Kalman Filter**: Recursive algorithm for estimating state of linear dynamic systems
- **Extended Kalman Filter (EKF)**: Kalman Filter variant for non-linear systems
- **Unscented Kalman Filter (UKF)**: Variant using unscented transformation for better approximation
- **Particle Filter**: Sample-based filter for estimating state distribution
- **Monte Carlo Localization**: Robot localization using particle filters
- **Simultaneous Localization and Mapping (SLAM)**: Map building while localizing in same space
- **Visual SLAM (VSLAM)**: SLAM using visual information from cameras
- **LiDAR SLAM**: SLAM using LiDAR scanner information
- **Graph-Based SLAM**: SLAM representing poses and landmarks in graph
- **Filter-Based SLAM**: SLAM estimating posterior distribution over poses
- **Pose Graph Optimization**: Optimizing pose graphs to reduce accumulated error
- **Bundle Adjustment**: Optimizing camera parameters and 3D structure in photogrammetry
- **Structure from Motion (SfM)**: 3D reconstruction from 2D image sequences
- **Multi-View Stereo (MVS)**: Detailed 3D reconstruction from multiple known views
- **Visual Odometry**: Movement estimation from image sequences
- **Wheel Odometry**: Movement estimation from wheel rotation counts
- **Inertial Navigation System (INS)**: Navigation using acceleration and gyroscope sensors
- **Global Positioning System (GPS)**: Navigation using satellite signals
- **Differential GPS (DGPS)**: GPS with correction from known ground stations
- **Real-Time Kinematic (RTK)**: GPS with real-time correction for centimeter precision
- **Precise Point Positioning (PPP)**: GPS with precise orbit and clock models
- **Global Navigation Satellite System (GNSS)**: Generic satellite-based positioning system
- **GLONASS**: Russian satellite navigation system
- **Galileo**: European satellite navigation system
- **BeiDou**: Chinese satellite navigation system
- **NavIC**: Indian satellite navigation system
- **LORAN**: Long-range radio-based navigation system
- **Decca**: Hyperbolic radio navigation system based on phase
- **Consolan**: Hyperbolic radio navigation system based on phase
- **Omega**: Very low frequency radio navigation system based on phase
- **VOR**: Aeronautical navigation system based on variable frequency
- **VORTAC**: Combination of VOR and TACAN for aeronautical navigation
- **TACAN**: Military navigation system based on ultrasonic pulses
- **NDB**: Radio navigation using non-directional signals
- **ILS**: Instrument landing system using runway and approach signals
- **MLS**: Microwave-based instrument landing system
- **GNSS Reflectometry**: Using reflected GNSS signals for remote sensing
- **Radio Occultation**: Atmospheric measurement using radio signal bending
- **GNSS-R**: GNSS Reflectometry for remote sensing applications
- **Reflectometry**: Technique measuring properties via signal reflection
- **Bistatic Radar**: Radar with separated transmitter and receiver
- **Multistatic Radar**: Radar with multiple transmitters and/or receivers
- **NetRadar**: Network-based radar for distributed sensing
- **Radar Tomography**: 3D structure reconstruction using multiple radar views
- **Interferometric Radar**: Radar using interference to measure minute movement
- **Synthetic Aperture Radar (SAR)**: Radar using sensor motion to synthesize large aperture
- **InSAR**: Interferometric SAR for measuring surface deformation
- **DInSAR**: Differential InSAR for measuring deformation changes over time
- **PSInSAR**: Permanent Scatterer InSAR for measuring persistent point movement
- **SBAS**: Satellite-Based Augmentation System for improving GNSS
- **GBAS**: Ground-Based Augmentation System for improving GNSS in specific areas
- **WAAS**: Wide Area Augmentation System for improving GPS in the US
- **EGNOS**: European Geostationary Navigation Overlay Service for improving GPS in Europe
- **MSAS**: Multi-functional Satellite Augmentation System for improving GPS in Japan
- **GAGAN**: GPS Aided Geo Augmented Navigation for improving GPS in India
- **SDCM**: Sistema de Diferença e Correção para melhoria de GLONASS na Rússia
- **BeiDou B1C**: BeiDou improvement signal similar to GPS L1C
- **BeiDou B2a**: BeiDou improvement signal similar to GPS L2C
- **BeiDou B3I**: BeiDou improvement signal similar to GPS L5
- **QZSS**: Quasi-Zenith Satellite System for improving GPS in Japan
- **IRNSS**: Indian Regional Navigation Satellite System for improving GPS in India
- **NavIC L5**: NavIC L5 signal for improved precision and robustness
- **BeiDou B1C**: Civil improvement signal for BeiDou with increased bandwidth
- **BeiDou B2a**: Civil improvement signal for BeiDou with increased interference resistance
- **BeiDou B3I**: Civil improvement signal for BeiDou with increased precision
- **BeiDou B1C**: BeiDou navigation system signal component
- **BeiDou B2a**: BeiDou navigation system signal component
- **BeiDou B3I**: BeiDou navigation system signal component
- **GPS L1C**: New civil GPS signal with improved characteristics
- **GPS L2C**: Second civil GPS signal with better obstacle penetration
- **GPS L5**: Third civil GPS signal with better accuracy and robustness
- **GPS L1CA**: Legacy civil GPS signal (Coarse/Acquisition)
- **GPS L1P**: Legacy precision GPS signal (Protected)
- **GPS L1W**: Legacy precision GPS signal (War Mode)
- **GPS L2P**: Legacy precision GPS signal (Protected)
- **GPS L2W**: Legacy precision GPS signal (War Mode)
- **GPS L5Q**: Legacy precision GPS signal (Protected)
- **GPS L5W**: Legacy precision GPS signal (War Mode)
- **GLONASS L1OF**: Legacy open GLONASS signal

*Esta seção será atualizada conforme avanço na criação das demais partes da documentação.*
