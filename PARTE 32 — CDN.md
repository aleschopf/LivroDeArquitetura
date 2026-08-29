---
trilha: "AVANÇADA"
---
**Navegação:** [[MOC — TRILHA AVANÇADA]]
← [[PARTE 31 — LOAD BALANCING]] | #trilha/avancada | [[PARTE 33 — API GATEWAY]] →

---
# PARTE 32 — CDN

> 🧠 **ESSENCIAL**
> Um API Gateway é um padrão de arquitetura que atua como ponto de entrada único para todos os clientes, fornecendo funcionalidades como roteamento, composição de serviços, autenticação, autorização, rate limiting, caching, logging e monitoramento para microserviços e arquiteturas distribuídas.

> 🎯 **ENTREVISTA — ALTA FREQUÊNCIA**
> Perguntas sobre quando usar API Gateway versus comunicação direta entre serviços, padrões de roteamento, segurança, rate limiting, circuit breaker, e comparação entre soluções como Kong, AWS API Gateway, Apigee e NGINX Plus são muito comuns em entrevistas de arquitetura de software.

## O que é API Gateway?

**API Gateway** é um servidor que atua como um front-end para APIs, recebendo chamadas de API, impondo políticas de throttling e segurança, passando requisições para os serviços de backend e depois retornando as respostas. Ele fornece um único ponto de entrada para um conjunto de microserviços, oferecendo funcionalidades que seriam difíceis ou inconvenientes de implementar em cada serviço individualmente.

### Por que usar API Gateway?

1. **Ponto de Entrada Único**: Simplifica a experiência do cliente ao não precisar conhecer a arquitetura interna dos microserviços
2. **Segurança Centralizada**: Autenticação, autorização e criptografia tratadas em um único local
3. **Roteamento e Load Balancing**: Distribui requisições entre múltiplas instâncias de serviços
4. **Composição de APIs**: Combina respostas de múltiplos serviços em uma única resposta para o cliente
5. **Tratamento Transversal de Preocupações**: Logging, monitoring, rate limiting, caching, etc.
6. **Versionamento de APIs**: Permite múltiplas versões de API coexistirem
7. **Desacoplamento**: Clientes não estão acoplados à arquitetura interna dos serviços
8. **Melhoria de Performance**: Caching de respostas, compression, e redução de round-trips

## Como funciona internamente

### Arquitetura Básica de API Gateway

1. **Cliente**: Fonte das requisições API (web app, mobile app, outro serviço)
2. **API Gateway**: Recebe requisições, aplica políticas, encaminha para serviços apropriados
3. **Serviços de Backend**: Microserviços que implementam a lógica de negócio real
4. **Serviços de Apoio**: Serviços de autenticação, cache, logging, etc. que o gateway pode usar
5. **Configuração e Policies**: Regras que definem como o gateway se comporta

### Fluxo de Trabalho Básico

1. **Cliente envia requisição HTTP** para o API Gateway (ex: GET /users/123/orders)
2. **Gateway recebe requisição** e aplica filtros pré-processamento (auth, rate limit, etc.)
3. **Gateway determina o serviço destino** baseado em regras de roteamento (path, headers, method, etc.)
4. **Gateway pode transformar a requisição** (adicionar/remover headers, mudar path, etc.)
5. **Gateway encaminha requisição** para o serviço de backend apropriado
6. **Serviço processa requisição** e retorna resposta
7. **Gateway pode transformar a resposta** (adicionar headers, modificar corpo, etc.)
8. **Gateway aplica filtros pós-processamento** (logging, métricas, caching, etc.)
9. **Gateway retorna resposta** ao cliente

### Padrões de Roteamento

#### 1. Roteamento Baseado em Path (Path-Based Routing)
- **Como funciona**: Mapeia caminhos de URL para serviços específicos
- **Exemplo**: 
  - `/users/*` → User Service
  - `/orders/*` → Order Service
  - `/products/*` → Product Service
- **Vantagens**: Simples, intuitivo, fácil de configurar
- **Desvantagens**: Pode se tornar complexo com muitos serviços e variações

#### 2. Roteamento Baseado em Subdomínio (Subdomain-Based Routing)
- **Como funciona**: Mapeia subdomínios para serviços específicos
- **Exemplo**:
  - `api-users.example.com` → User Service
  - `api-orders.example.com` → Order Service
  - `api-products.example.com` → Product Service
- **Vantagens**: Isolamento claro, fácil de gerenciar certificados SSL separados
- **Desvantagens**: Requer configuração DNS para cada serviço, menos flexível para mudanças frequentes

#### 3. Roteamento Baseado em Headers ou Custom Rules
- **Como funciona**: Usa headers específicos, métodos HTTP, ou regras customizadas para roteamento
- **Exemplo**:
  - Header `X-Version: v2` → roteia para versão v2 do serviço
  - Método `POST` com caminho `/webhooks/*` → serviço de webhook especializado
  - Combinação de path e query parameters para roteamento avançado
- **Vantagens**: Muito flexível, suporta cenários complexos de versionamento e experimentação
- **Desvantagens**: Configuração mais complexa, pode ser difícil de rastrear

#### 4. Service Discovery Integration
- **Como funciona**: Gateway integra-se com service discovery (Consul, Eureka, etcd) para obter instâncias disponíveis dinamicamente
- **Vantagens**: Escala automaticamente à medida que serviços são adicionados/removidos
- **Desvantagens**: Adiciona dependência em sistema de service discovery, complexidade aumentada

## Funcionalidades Principais do API Gateway

### 1. Autenticação e Autorização
- **API Keys**: Chaves simples para identificar e controlar acesso
- **OAuth 2.0 / OpenID Connect**: Integração com provedores de identidade (Auth0, Okta, Azure AD)
- **JWT Validation**: Validação de tokens JSON Web Token
- **Mutual TLS (mTLS)**: Autenticação baseada em certificados cliente
- **RBAC (Role-Based Access Control)**: Controle de acesso baseado em papéis e permissões
- **ABAC (Attribute-Based Access Control)**: Controle de acesso baseado em atributos

### 2. Rate Limiting e Throttling
- **Controle de Taxa**: Limita número de requisições por cliente, IP, API key, ou outros critérios
- **Algoritmos**: 
  - Fixed Window Contador
  - Sliding Window Log
  - Token Bucket (mais suave que fixed window)
  - Leaky Bucket
- **Escopo**: Pode ser aplicado globalmente, por serviço, por rota, ou por cliente
- **Headers de Retorno**: `X-RateLimit-Limit`, `X-RateLimit-Remaining`, `X-RateLimit-Reset`

### 3. Transformação de Requisição e Resposta
- **Modificação de Headers**: Adicionar, remover, ou alterar headers
- **Modificação de Path/URL**: Reescrever caminhos antes de encaminhar para backend
- **Modificação de Corpo**: Converter entre JSON/XML, adicionar/remover campos
- **Adição de Informação de Contexto**: Inserir user ID, tenant ID, ou outras informações de segurança
- **Remoção de Informação Sensível**: Strip de headers ou dados que não devem ir para backend

### 4. Composição e Agregação de Serviços
- **Fan-out/Fan-in**: Gateway faz múltiplas chamadas para serviços diferentes e agrega resultados
- **Exemplo**: Tela de perfil do usuário pode precisar de dados de User Service, Order Service, e Preferences Service
- **Padrões**: 
  - Sequential: Chamadas uma após outra
  - Parallel: Chamadas simultâneas para melhor performance
  - Conditional: Chamadas baseadas em resultados de chamadas anteriores
- **Considerações**: Timeout, circuit breaker, fallback para evitar cascata de falhas

### 5. Caching de Respostas
- **Cache de Resposta GET**: Armazenar respostas de requisições de leitura para melhorar performance
- **Chaves de Cache**: Baseadas em URL, headers, query parameters, ou outros fatores
- **Políticas de Expiração**: TTL, invalidation baseada em eventos ou time-based
- **Cache Personalizado**: Diferente cache por usuário, grupo, ou segmento quando apropriado
- **Cabeçalhos de Cache**: Respeita ou sobrescreve Cache-Control, ETag, etc. do backend

### 6. Logging, Monitoring e Tracing
- **Access Logging**: Registro de todas as requisições (path, method, status, latency, etc.)
- **Métricas**: Taxa de requisições, latência, taxas de erro, uso de recursos
- **Distributed Tracing**: Integração com sistemas como Jaeger, Zipkin, AWS X-Ray para rastreamento ponta a ponta
- **Alerting**: Notificações para condições anormais (alta latência, aumento de taxas de erro, etc.)
- **Integração com SIEM**: Envio de logs para sistemas de gerenciamento de eventos e segurança

### 7. Circuit Breaker e Retry Logic
- **Circuit Breaker**: Evita chamadas repetidas para serviços que estão falhando
- **Estados**: 
  - Closed: Permite requisições normais
  - Open: Bloqueia requisições imediatamente (após threshold de falhas)
  - Half-Open: Permite algumas requisições de teste para verificar se serviço recuperou
- **Retry Logic**: Tenta novamente requisições falhadas com backoff exponencial ou jitter
- **Timeouts**: Configuráveis para evitar requisições que ficam pendentes indefinidamente

### 8. SSL/TLS Termination
- **Funcionamento**: Gateway descriptografa tráfego HTTPS, comunica-se com backends via HTTP
- **Vantagens**:
  - Remove sobrecarga de criptografia dos serviços de backend
  - Centraliza gerenciamento de certificados
  - Permite inspeção de conteúdo para segurança avançada
- **Considerações**: 
  - Tráfego interno entre gateway e backends não é criptografado (requer rede confiável)
  - Pode usar mTLS para autenticação de serviço a serviço mesmo com terminação no gateway

## Tipos de API Gateways

### 1. API Gateways Open Source / Self-Hosted

#### Kong
- **Baseado em**: NGINX + Lua (OpenResty)
- **Funcionalidades**: Plugins extensíveis (auth, rate limiting, logging, etc.), dashboard, clustering
- **Implantação**: Pode rodar em containers, VMs, ou bare metal
- **Comunidade**: Grande ecossistema de plugins open source e comerciais

#### Apigee (versão open source também disponível)
- **Funcionalidades**: Gerenciamento completo de API lifecycle, portal de desenvolvedor, analytics avançado
- **Foco**: Gerenciamento de APIs empresariais com forte ênfase em developer experience

#### Tyk
- **Linguagem**: Go (alta performance)
- **Funcionalidades**: API Gateway, API Management, Developer Dashboard
- **Recursos**: Circuit breaker, rate limiting avançado, analytics em tempo real

#### Ambassador / Emissary Ingress
- **Baseado em**: Envoy Proxy
- **Foco**: Kubernetes-native, projetado especificamente para ambientes de container orquestrado
- **Integração**: CRDs Kubernetes para configuração declarativa

#### KrakenD
- **Linguagem**: Go
- **Funcionalidades**: Ultra performance, foco em simplicidade e baixa latência
- **Abordagem**: Configuração declarativa via JSON, minimal runtime dependencies

#### Würthless (ex: Express Gateway, LoopBack)
- **Baseado em**: Frameworks de aplicação (Node.js/Java)
- **Funcionalidades**: Mais flexível para customização, mas pode ter overhead maior

### 2. API Gateways Gerenciados (Cloud Services)

#### AWS API Gateway
- **Tipos**: 
  - REST API (clássico)
  - HTTP API (mais barato, menor latência, WebSocket support)
  - WebSocket API
- **Integração**: Serviços AWS nativos (Lambda, DynamoDB, SNS, SQS, etc.)
- **Funcionalidades**: Throttling, caching, autorização (Cognito, IAM, Lambda authorizers), stage/version deployment
- **Preço**: Por milhão de requisições, por hora de conexão WebSocket, por GB de dados transferidos

#### Azure API Management
- **Funcionalidades**: Gateway completo, portal de desenvolvedor, políticas avançadas, integração com Azure AD
- **Camadas**: Consumo (serverless), Premium, Desenvolvedor, Isolado
- **Integração**: Serviços Azure (Functions, App Service, Logic Apps, etc.)
- **Recursos avançados**: Virtual network integration, multi-geografia deployment, backup/restore

#### Google Cloud Apigee
- **Herança**: Baseado no Apigee adquirido pelo Google
- **Funcionalidades**: Gerenciamento full lifecycle, avançado analytics, monetização de APIs
- **Integração**: Serviços Google Cloud (Cloud Functions, Cloud Run, Pub/Sub, etc.)
- **Planos**: Standard, Enterprise, Enterprise Plus com diferentes SLAs e funcionalidades

#### Cloudflare API Gateway
- **Foco**: Segurança e performance na borda
- **Funcionalidades**: WAF, rate limiting, bot management, TLS 1.3, carregamento rápido via rede Cloudflare
- **Integração**: Workers para lógica customizada na borda
- **Vantagem**: Distribuição global com baixa latência

### 3. Service Meshes com Funcionalidades de Gateway

#### Istio Ingress Gateway
- **Baseado em**: Envoy Proxy
- **Funcionalidades**: Tráfego de entrada para malha de serviço, mTLS, políticas de tráfego avançadas
- **Integração**: Funciona junto com Istio service mesh para controle de tráfego leste-oeste

#### Linkerd
- **Abordagem**: Mais simples que Istio, foco em segurança e observabilidade
- **Gateway**: Recursos limitados de entrada, foco principalmente em service-to-service

#### Consul Connect
- **Gateway**: Capacidades de entrada/saída para integrar com redes externas
- **Integração**: Service discovery e segmentação de rede do Consul

## Quando Usar API Gateway

### Cenários Ideais

1. **Arquitetura de Microserviços**: Quando você tem múltiplos serviços que precisam ser expostos para clientes externos
2. **Necessidade de Funcionalidades Transversais**: Quando múltiplos serviços precisam de autenticação, rate limiting, logging similares
3. **Versionamento de API**: Quando você precisa suportar múltiplas versões de API simultaneamente
4. **Composição de Serviços**: Quando clientes precisam de dados de múltiplos serviços em uma única chamada
5. **Necessidade de Segurança Centralizada**: Quando você quer aplicar políticas de segurança uniformes em todos os pontos de entrada
6. **Experiência do Desenvolvedor**: Quando você quer fornecer um portal de desenvolvedor com documentação interativa, testes, etc.
7. **Modernização de Legados**: Quando você está expondo funcionalidades de sistemas legados através de uma interface API moderna
8. **Parceiros e Terceiros**: Quando você precisa expor APIs para parceiros de negócio com controle e monitoramento rigorosos

### Quando Não Usar API Gateway (ou Usar com Cautela)

1. **Arquitetura Monolítrica Simples**: Quando você tem um único serviço ou poucos serviços bem acoplados
   - Alternativa: Expor serviços diretamente ou usar um reverse proxy simples (NGINX, HAProxy)
2. **Latência Extremamente Crítica**: Quando cada microssegundo conta e o overhead do gateway é inaceitável
   - Alternativa: Otimizar serviços individuais, usar protocolos binários eficientes (gRPC, Thrift)
3. **Equipe Pequena com Recursos Limitados**: Quando a complexidade adicional do gateway supera os benefícios
   - Alternativa: Começar simples e adicionar gateway somente quando necessário
4. **Padrões de Comunicação Não-HTTP**: Quando seus serviços se comunicam principalmente via gRPC, messaging, ou outros protocolos
   - Alternativa: Use gateways específicos para esses protocolos ou service mesh com suporte nativo
5. **Quando Funcionalidades Podem ser Delegadas**: Quando autenticação pode ser feita no serviço, rate limiting no load balancer, etc.
   - Alternativa: Distribuir responsabilidades de forma mais granular (seguindo princípio de separação de preocupações)

## Perguntas de Entrevista Comuns

### Básicas
- "O que é API Gateway e quais problemas ele resolve?"
- "Como um API Gateway difere de um reverse proxy tradicional como NGINX ou HAProxy?"
- "Quais são as principais funcionalidades de um API Gateway?"

### Intermediárias
- "Como você implementaria autenticação e autorização em um API Gateway?"
- "Explique como você usaria rate limiting em um API Gateway para proteger serviços de backend."
- "Como um API Gateway lida com versionamento de API?"
- "Quais são as estratégias para lidar com falhas em serviços de backend através de um API Gateway?"

### Avançadas
- "Como você projetaria um API Gateway para lidar com milhares de requisições por segundo com baixa latência?"
- "Discuta trade-offs entre usar um API gateway gerenciado versus uma solução self-hosted."
- "Como você lidaria com o desafio de observabilidade em arquiteturas de microserviços usando API Gateway?"
- "Explique como você implementaria uma estratégia de canary release ou blue/green deployment usando API Gateway."

### Follow-ups Típicos
- "E se precisássemos mudar nossa estratégia de API Gateway após o sistema estar em produção?"
- "Como você validaria que seu API Gateway está realmente melhorando segurança e performance?"
- "Qual seria sua estratégia para migrar de comunicação direta entre serviços para um API Gateway sem downtime?"
- "E se descobríssemos que nosso padrão de uso tem características que tornam certas funcionalidades do API Gateway subutilizadas?"

## Checklist de Implementação de API Gateway

### Antes de Começar a Implementação
- [ ] Analisar requisitos de autenticação e autorização (quem pode acessar o quê)
- [ ] Definir requisitos de rate limiting e throttling (limites por cliente, serviço, etc.)
- [ ] Mapear serviços de backend e seus contratos (endpoints, modelos de dados, protocolos)
- [ ] Determinar necessidades de roteamento (path-based, header-based, versionamento, etc.)
- [ ] Planejar estratégias de composição e agregação de serviços (quando fazer chamadas múltiplas)
- [ ] Definir requisitos de logging, monitoring e tracing (o que precisamos observar)
- [ ] Avaliar necessidades de transformação de requisição/resposta (headers, paths, corpos)
- [ ] Decidir sobre abordagem de caching (o que cachear, por quanto tempo, invalidation)
- [ ] Orçar custos e recursos necessários (especialmente para soluções gerenciadas)
- [ ] Planejar estratégia de deployment e versionamento (como gerenciar mudanças no gateway)

### Durante a Implementação
- [ ] Selecionar tecnologia de API Gateway adequada às necessidades e ambiente
- [ ] Configurar rotas iniciais para serviços de backend críticos
- [ ] Implementar estratégias de autenticação e autorização (API keys, OAuth, JWT, etc.)
- [ ] Configurar rate limiting e throttling com limites iniciais razoáveis
- [ ] Implementar logging básico e métricas de performance (latência, taxas de erro)
- [ ] Configurar tratamento de erros e respostas padronizadas para clientes
- [ ] Adicionar funcionalidades de transformação conforme necessário (headers, paths, etc.)
- [ ] Implementar circuit breaker e retry logic para proteção contra falhas de backend
- [ ] Configurar SSL/TLS termination e gerenciamento de certificados
- [ ] Implementar saúde dos endpoints (health checks) para detecção automática de falhas
- [ ] Testar extensivamente em ambiente de staging com cargas realistas e cenários de falha

### Depois da Implementação e em Produção
- [ ] Monitorar distribuição de tráfego entre serviços de backend
- [ ] Alertar sobre aumento de latência, taxas de erro, ou violações de rate limiting
- [ ] Rastrear eficácia de caching (taxa de hit/miss) e ajustar TTLs conforme necessário
- [ ] Validar que autenticação e autorização estão funcionando conforme esperado
- [ ] Testar failover e recuperação de serviços de backend
- [ ] Revisar periodicamente eficácia das políticas de rate limiting e ajustar limites
- [ ] Manter certificados TLS atualizados e renovados
- [ ] Coletar feedback de desenvolvedores (se tiver portal de desenvolvedor) e consumidores da API
- [ ] Aplicar patches de segurança regularmente no gateway e dependências
- [ ] Planejar capacidade futura baseado em tendências de crescimento observadas
- [ ] Documentar procedures operacionais para adicionar/remover serviços, mudar configurações, etc.

## RESUMO

API Gateway é um componente essencial em arquiteturas modernas de microserviços, fornecendo um ponto de entrada unificado com funcionalidades transversais que melhoram segurança, performance, e gerenciabilidade:

**Princípios-chave:**
1. API Gateway fornece um único ponto de entrada para clientes, encapsulando a complexidade da arquitetura interna de microserviços
2. Funcionalidades como autenticação, rate limiting, logging, e caching são centralizadas no gateway, evitando duplicação em cada serviço
3. Roteamento pode ser baseado em path, headers, service discovery, ou outras estratégias flexíveis para atender a diferentes necessidades
4. Composição de serviços permite que o gateway faça chamadas múltiplas parabackend e agregue respostas, reduzindo round-trips do cliente
5. Funcionalidades avançadas como circuit breaker, retry logic, e SSL termination aumentam resiliência e segurança
6. A escolha entre soluções self-hosted e gerenciadas depende de fatores como controle necessário, expertise da equipe, e requisitos de escala e funcionalidades
- [ ] Lembre-se: Um API Gateway eficaz não é apenas sobre encaminhar requisições - é sobre entender profundamente seus serviços, padrões de acesso, requisitos de segurança, e necessidades de negócio para projetar uma solução que equilibre funcionalidade, performance, complexidade, e custo operacional.