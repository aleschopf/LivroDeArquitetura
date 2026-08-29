---
trilha: "PARA ENTREVISTAS"
---
**Navegação:** [[MOC — TRILHA PARA ENTREVISTAS]]
← [[PARTE 74 — CHECKLIST DE SYSTEM DESIGN]] | #trilha/entrevistas | [[PARTE 76 — TABELAS COMPARATIVAS]] →

---
# PARTE 75 — TABELAS COMPARATIVAS

## Fundamentos

A comparação estruturada entre diferentes tecnologias, padrões e abordagens é uma habilidade essencial para arquitetos de software. Tabelas comparativas bem elaboradas permitem tomada de decisão informada, evidenciando trade-offs claros e ajudando equipes a escolher a solução mais adequada para seu contexto específico.

Esta parte fornece uma coleção de tabelas comparativas organizadas por categoria, contendo análises lado a lado de opções populares em diversos domínios da arquitetura de software. Cada tabela foca em critérios relevantes para decisões de arquitetura, considerando fatores como desempenho, escalabilidade, complexidade, custos, maturidade e adequação a diferentes cenários de uso.

> **Nota**: As comparações apresentadas são pontos de partida. Avalie sempre o contexto específico do seu projeto, pois o "melhor" escolha varia significativamente baseado em requisitos, restrições e objetivos particulares.

## 1. Tabela Comparativa: Estilos Arquiteturais

| Critério | Monolítica | Camadas | Hexagonal | Microserviços | Serverless | Event-Driven |
|----------|------------|---------|-----------|---------------|------------|--------------|
| **Complexidade de Desenvolvimento** | Baixa | Média | Média | Alta | Média-Alta | Alta |
| **Deploy Inicial** | Simples | Simples | Moderado | Complexo | Muito Simples | Moderado |
| **Escalabilidade** | Vertical apenas | Vertical/limitada | Vertical/limitada | Horizontal excelente | Automática granular | Excelente |
| **Isolamento de Falhas** | Pobre | Moderado | Bom | Excelente | Bom | Bom |
| **Tecnologia Heterogênea** | Não | Não | Limitada | Sim | Sim | Sim |
| **Latência de Comunicação** | Muito baixa (in-processo) | Baixa | Baixa | Alta (rede) | Variável | Média-Alta |
| **Consistência de Dados** | Forte fácil | Forte fácil | Forte fácil | Eventual desafiadora | Eventual | Eventual |
| **Overhead Operacional** | Baixo | Baixo | Médio | Alto | Baixo-Médio | Médio |
| **Maturidade do Ecossistema** | Alta | Alta | Média | Alta | Média-Alta | Média-Alta |
| **Quando Usar** | MVP, equipes pequenas | Apps empresariais tradicionais | Domínios complexos, múltiplas interfaces | Grandes equipes, escalabilidade independente | Cargas esparsas/imprevisíveis | Alto throughput, fluxos assíncronos |
| **Exemplos de Tecnologias** | Qualquer linguagem/framework | Java EE, .NET N-layer | Spring Boot, Clean Arch | Docker/K8s, Service Mesh | AWS Lambda, Azure Functions | Kafka, RabbitMQ, Pulsar |

## 2. Tabela Comparativa: Bancos de Dados SQL

| Critério | PostgreSQL | MySQL | Microsoft SQL Server | Oracle | SQLite |
|----------|------------|-------|----------------------|--------|--------|
| **Licença** | Open Source (PostgreSQL) | Open Source (GPL) | Comercial | Comercial | Open Source (PD) |
| **Performance Leitura** | Excelente | Muito Bom | Excelente | Excelente | Bom (embedded) |
| **Performance Escrita** | Excelente | Bom | Muito Bom | Excelente | Limitado |
| **Escalabilidade Vertical** | Excelente | Bom | Excelente | Excelente | Limitado |
| **Escalabilidade Horizontal** | Limitado (sharding manual) | Limitado (sharding manual) | Bom (Always On) | Excelente (RAC) | Não aplicável |
| **Replicação** | Síncrona/Assíncrona | Assíncrona/semi-síncrona | Síncrona/Assíncrona | Avançada | Não nativa |
| **JSON/BSON** | Excelente (JSONB) | Bom (JSON 5.7+) | Bom | Excelente | Bom (JSON1 extension) |
| **Extensibilidade** | Excelente (PostGIS, etc.) | Moderada | Boa | Excelente | Limitada |
| **Conformidade SQL** | Muito Alta | Média | Alta | Muito Alta | Parcial |
| **Ferramentas de Admin** | Excelente (pgAdmin, psql) | Bom (MySQL Workbench) | Excelente (SSMS) | Excelente | Limitada |
| **Custo** | Gratuito | Gratuito/Comercial | Comercial | Muito Alto | Gratuito |
| **Quando Usar** | Aplicações complexas, GIS, alta integridade | Web apps, LAMP, baixa complexidade | Ambientes Microsoft, enterprise | Grandes enterprises, alta performance crítica | Embedded, protótipos, apps móveis |
| **Exemplos de Uso** | Instagram, Reddit, AWS RDS | Facebook, Twitter, YouTube | SAP, bancos, governos | Grandes corporações, telecom | Mozilla Firefox, Android, iOS apps |

## 3. Tabela Comparativa: Bancos de Dados NoSQL

| Critério | MongoDB | Cassandra | Redis | CouchDB | DynamoDB |
|----------|---------|-----------|-------|---------|----------|
| **Tipo de Dados** | Documento (BSON) | Wide-column | Key-valor (estruturas) | Documento (JSON) | Key-valor/Documento |
| **Modelo de Consistência** | Eventual/Forte (por documento) | Eventual tunable | Forte (single node) | Eventual | Eventual/Forte (consistente leitura) |
| **Escalabilidade Horizontal** | Excelente (sharding automático) | Excelente | Limitado (cluster) | Bom | Excelente (gerenciado) |
| **Performance Leitura** | Excelente | Excelente | Excelente (memória) | Bom | Excelente |
| **Performance Escrita** | Excelente | Excelente | Excelente (memória) | Bom | Excelente |
| **Consultas Ad-hoc** | Excelente (índices flexíveis) | Limitado (modelo de partição) | Limitado (por chave) | Excelente (MapReduce) | Limitado (chave/índice) |
| **Transações ACID** | Multi-documento (4.0+) | Limitado (por linha) | Limitado (lua scripting) | Limitado | Limitado (recentemente) |
| **Esquema** | Flexível | Flexível | Nenhum | Flexível | Flexível |
| **Sobrecarga Operacional** | Médio | Alto | Baixo-Médio | Médio | Baixo (gerenciado) |
| **Maturidade** | Alta | Alta | Alta | Média | Alta (AWS) |
| **Quando Usar** | Catálogos de produtos, CMS, perfis de usuário | Dados de série temporal, logging, IoT | Cache, filas, leaderboards, sessões | Apps offline-first, replicação pêra-a-pês | Apps web/mobile escaláveis, gaming, ad tech |
| **Exemplos de Uso** | Forbes, Bosch, EA | Netflix, Apple, eBay | Twitter, GitHub, Stack Overflow | Couchbase, IBM Cloud | Lyft, Snapchat, Toyota |

## 4. Tabela Comparativa: Mensageria e Streaming

| Critério | RabbitMQ | Apache Kafka | AWS SQS/SNS | Apache Pulsar | Redis Streams |
|----------|----------|--------------|-------------|---------------|---------------|
| **Modelo** | Filas/Tópicos (AMQP) | Log distribuído | Filas/Tópicos gerenciados | Pub/Sub com segmentação | Estruturas Redis |
| **Garantia de Entrega** | At-most-once/At-least-once | At-least-once | At-least-once (SQS) | At-least-once/Effectively-once | At-least-once |
| **Ordem de Mensagens** | Por fila | Por partição | Nenhuma (SQS padrão) | Por chave | Por stream |
| **Throughput** | Médio-Alto (100k msg/s) | Muito Alto (mihões msg/s) | Médio (SQS limitado) | Alto | Alto (memória) |
| **Latência** | Baixa-Média | Baixa-Média | Média-Alta | Baixa | Muito Baixa |
| **Persistência** | Discop configurável | Discop (segmentos) | Discop (gerenciado) | Discop (segmentos) | Discop configurável |
| **Escalabilidade** | Bom (clustering) | Excelente | Excelente (gerenciado) | Excelente | Limitado (memória) |
| **Monitoramento** | Bom (plugin management) | Excelente (JMX, herramientas) | Bom (CloudWatch) | Excelente | Bom (INFO, comandos) |
| **Complexidade de Setup** | Média | Alta | Baixa (gerenciado) | Média-Alta | Baixa |
| **Ecossistema e Integrações** | Excelente (muitos clientes) | Excelente (Connect, Streams, etc.) | Bom (AWS integrations) | Crescente | Bom (Redis ecosystem) |
| **Quando Usar** | Roteamento complexo, workflows, RPC | Log de eventos, stream processing, alta throughput | Desacoplamento simples, notificações, serverless | Multi-tenancy, geo-replicação, funções serverless | Casos de uso simples, baixa latência, já usando Redis |
| **Exemplos de Uso** | Bancos, telecomunicações, e-commerce | LinkedIn, Uber, Netflix | Startups, apps serverless, backends móveis | Yahoo, Splunk, Oath | Cacheamento avançado, rate limiting, sessões |

## 5. Tabela Comparativa: Plataformas de Container e Orquestração

| Critério | Docker Swarm | Kubernetes | Apache Mesos | Nomad | Amazon ECS |
|----------|--------------|------------|--------------|-------|------------|
| **Complexidade de Instalação** | Baixa | Alta | Média | Baixa | Muito Baixa (AWS) |
| **Escalabilidade** | Bom | Excelente | Excelente | Bom | Excelente (AWS) |
| **Orquestração de Serviços** | Bom | Excelente | Excelente | Bom | Bom |
| **Auto-scaling** | Limitado | Excelente (HPA/VPA) | Excelente | Bom | Excelente (AWS ASG) |
| **Service Discovery** | Embutido | Excelente (CoreDNS/kube-dns) | Embutido | Embutido (Consul integratioon) | AWS Cloud Map |
| **Balanceamento de Carga** | Embutido | Excelente (Ingress, Services) | Embutido | Embutido | ELB/ALB |
| **Gerenciamento de Configuração** | Limitado | Excelente (ConfigMaps, Secrets) | Excelente | Bom | Bom (SSM Parameter Store) |
| **Health Checks** | Bom | Excelente (liveness/readiness) | Excelente | Bom | Bom |
| **Dashboard/UI** | Básico | Excelente (Dashboard, Lens) | Bom | Bom | Console AWS |
| **Curva de Aprendizado** | Baixa | Alta | Média | Baixa | Baixa (se já na AWS) |
| **Maturidade do Ecossistema** | Bom | Excelente | Bom | Crescente | Excelente (AWS) |
| **Custo** | Gratuito | Gratuito | Gratuito | Gratuito | Pago por uso (ECS + EC2/Fargate) |
| **Quando Usar** | Pequenas/médias equipes, simplicidade desejada | Grandes escala, necessidades complexas | Grandes escala, cargas variadas híbridas | Simplicidade com boa performance | Ambientes AWS puros, equipes focadas em negócio |
| **Exemplos de Uso** | Startups, POCs, equipes DevOps pequenas | Adobe, IBM, Pokémon GO, Shopify | Twitter, Uber, Airbnb | Citadel, Bosch, SoundCloud | Startups AWS, empresas já na AWS |

## 6. Tabela Comparativa: Serviços de Nuvem Pública

| Critério | AWS | Azure | Google Cloud | IBM Cloud | Oracle Cloud |
|----------|-----|-------|--------------|-----------|--------------|
| **Participação de Mercado** | Líder (~32%) | #2 (~20%) | #3 (~9%) | <2% | <2% |
| **Regiões Geográficas** | 25+ | 60+ | 24+ | <20 | <20 |
| **Serviços de Computação** | EC2, Lambda, ECS/EKS | VMs, Functions, AKS | Compute Engine, Functions, GKE | VMs, Functions, IKS | VMs, Functions, OKE |
| **Serviços de Banco de Dados** | RDS, DynamoDB, Aurora | SQL DB, Cosmos DB | Cloud SQL, Firestore, Spanner | DB2, MongoDB, PostgreSQL | Autonomous DB, MySQL |
| **Serviços de Armazenamento** | S3, EBS, EFS | Blob Storage, Disks | Cloud Storage, Persistent Disk | Object Storage, File Storage | Object Storage, Block Storage |
| **Serviços de Rede** | VPC, CloudFront, Route 53 | VNET, CDN, DNS | VPC, Cloud CDN, Cloud DNS | VLAN, CDN, DNS | VCN, DNS, Traffic Mgmt |
| **Ferramentas de Desenvolvimento** | CDK, SAM, Ampliz | ARM, Azure CLI, Azure DevOps | Deployment Manager, Firebase | Cloud Pak, IBM CLI | OCI CLI, Terraform Provider |
| **Integração com Open Source** | Excelente | Bom (melhorando) | Excelente | Bom | Limitado |
| **Contratos de Nível de Serviço (SLA)** | Fortes (99.99%+) | Fortes (99.95%+) | Fortes (99.9%+) | Fortes | Fortes |
| **Modelo de Precificação** | Complexo (pay-as-you-go) | Complexo (pay-as-you-go) | Simples em alguns serviços | Enterprise-focused | Complexo |
| **Ferramentas de Migração** | Excelente (DMS, SMS) | Bom (Azure Migrate) | Bom | Excelente (for mainframe) | Bom |
| **Suporte ao Cliente** | Bom (planos variados) | Bom (planos variados) | Excelente (planos variados) | Bom (enterprise-focused) | Bom (enterprise-focused) |
| **Quando Usar** | Maior variedade de serviços, mercado maduro | Ambientes Microsoft híbridos, integração com .NET | Análise de dados, ML, cargas com alta necessidade de rede | Grandes empresas com legado IBM, mainframes | Ambientes Oracle existentes, bancos de dados Oracle |
| **Exemplos de Uso** | Netflix, Airbnb, Unilever | Adobe, GE, HP | Spotify, Snapchat, PayPal | Crédit Munier, Mitsubishi | Xerox, Lockheed Martin, BNP Paribas |

## 7. Tabela Comparativa: Linguagens de Programação para Backend

| Critério | Java | C#/.NET | Python | Node.js | Go | Rust |
|----------|------|---------|--------|---------|----|------|
| **Performance** | Excelente (JVM) | Excelente (CLR) | Bom (interpretado) | Bom (V8) | Excelente (compilado nativo) | Excelente (compilado nativo) |
| **Escalabilidade** | Excelente | Excelente | Bom | Bom (single-threaded) | Excelente | Excelente |
| **Curva de Aprendizado** | Média | Média | Baixa | Baixa | Baixa | Alta |
| **Produtividade do Desenvolvedor** | Alta | Alta | Muito Alta | Alta | Alta | Média |
| **Ecossistema e Bibliotecas** | Excelente | Excelente | Excelente | Excelente | Bom | Crescente |
| **Tipagem** | Estática forte | Estática forte | Dinâmica | Dinâmica | Estática forte | Estática forte com inferência |
| **Concorrência** | Bom (threads, executores) | Bom (async/await, TPL) | Bom (asyncio, GIL limitado) | Excelente (event loop) | Excelente (goroutines, channels) | Excelente (ownership, async/await) |
| **Gerenciamento de Memória** | GC automático | GC automático | GC automático | GC automático | GC automático | Ownership (sem GC) |
| **Deploy e Distribuição** | JVM necessária | .NET Runtime necessária | Interpretador necessário | Node.js necessária | Binário estático | Binário estático |
| **Maturidade** | Muito Alta | Alta | Alta | Média-Alta | Alta | Média |
| **Quando Usar** | Grandes empresas, Android, sistemas críticos | Ambientes Microsoft, jogos Unity, enterprise | Ciência de dados, scripting, protótipos rápidos | APIs em tempo real, real-time apps, full-stack JS | Sistemas de infraestrutura, DevOps, alta performance | Sistemas de baixo nível, blockchains, dispositivos embarcados |
| **Exemplos de Uso** | LinkedIn, Amazon, Murex | Stack Overflow, Mercado Livre, Unity | Instagram, Spotify, NASA | Netflix, Uber, PayPal | Docker, Kubernetes, Dropbox | Mozilla Firefox, Discord, Cloudflare |

## 8. Tabela Comparativa: Frameworks de Frontend

| Critério | React | Angular | Vue.js | Svelte | Ember.js |
|----------|-------|---------|--------|--------|----------|
| **Tipo** | Biblioteca | Framework | Framework | Compilador | Framework |
| **Curva de Aprendizado** | Baixa-Média | Alta | Baixa | Baixa | Média |
| **Tamanho do Bundle** | Médio (com dependências) | Grande | Médio-Baixo | Muito Pequeno | Grande |
| **Performance de Renderização** | Excelente (Virtual DOM) | Excelente (Change Detection) | Excelente (Reatividade) | Excelente (Compilado puro) | Bom (Glimmer) |
| **Ecossistema e Bibliotecas** | Excelente | Bom | Bom | Crescente | Limitado |
| **Suporte da Comunidade** | Excelente | Bom | Excelente | Crescente | Limitado |
| **Tooling e DevX** | Excelente (Create React App, DevTools) | Excelente (CLU, DevTools) | Excelente (CLI, DevTools) | Excelente (CLI, DevTools) | Bom (CLI, DevTools) |
| **Tipagem** | JS/TS | TS (padrão) | JS/TS | JS/TS | JS/TS |
| **Gerenciamento de Estado** | Redux, Context, MobX | Services, RxJS, NgRx | Vuex, Pinia | Stores externos | Ember Data, Services |
| **Rendering do Lado do Servidor** | Next.js | Angular Universal | Nuxt.js | SvelteKit | FastBoot |
| **Quando Usar** | Grandes aplicações flexíveis, equipes experientes | Grandes aplicações estruturadas, equipes com experiência TS | Aplicações médias a grandes, protótipos rápidos | Aplicações de alta performance, tamanhos mínimos críticos | Aplicações ambiciosas, convenção sobre configuração |
| **Exemplos de Uso** | Facebook, Instagram, Airbnb | Google, Microsoft, Forbes | Alibaba, Xiaomi, Nintendo | The New York Times, TikTok, GitLab | LinkedIn, Twitch, Yahoo Notas |

## 9. Tabela Comparativa: Métodos de Autenticação

| Critério | Senha + Hash | OAuth 2.0 | OpenID Connect | JWT | SAML 2.0 | WebAuthn/FIDO2 |
|----------|--------------|-----------|----------------|-----|----------|----------------|
| **Fatores de Autenticação** | Algo que você sabe | Delegation (2º fator possível) | Algo que você sabe + opcional | Token portador | Algo que você sabe | Algo que você tem + opcional biométrico |
| **Complexidade de Implementação** | Baixa | Média | Média | Baixa-Média | Alta | Média-Alta |
| **Segurança** | Boa (se implementado corretamente) | Muito Boa | Muito Boa | Boa (depende de implementação) | Muito Boa | Excelente |
| **Experiência do Usuário** | Familiar | Bom (SSO) | Excelente (SSO) | Bom (requer gerenciamento de token) | Bom (SSO corporativo) | Excelente (biométrico/senha menos) |
| **Escalabilidade** | Excelente | Excelente | Excelente | Excelente (stateless) | Bom (pode ser gargalo) | Excelente |
| **Suporte a Múltiplos Dispositivos** | Bom | Excelente | Excelente | Excelente | Limitado | Excelente |
| **Revogação de Sessão** | Simples (invalida token/sessão) | Complexa (requer token blacklist) | Complexa (requer token blacklist) | Complexa (requer token blacklist ou short expiry) | Complexa | Complexa (depende de implementação) |
| **Quando Usar** | Aplicações simples, interna | APIs públicas, autorização de terceiros | Aplicações web empresariais, SSO | APIs stateless, mobile apps | SSO empresarial legado | Alta segurança, autenticação sem senha |
| **Exemplos de Uso** | Aplicações legadas, interna corporativa | Google Login, Facebook Login, GitHub | Azure AD, Okta, Auth0 | APIs RESTful, aplicativos móveis | SAP, Oracle Apps, sistemas governamentais | Windows Hello, Apple Touch ID, YubiKey |

## 10. Tabela Comparativa: Estratégias de Cache

| Critério | Cache Local (In-process) | Redis | Memcached | CDN | Browser Cache |
|----------|--------------------------|-------|-----------|-----|---------------|
| **Latência** | Muito Baixa (nanosegundos-microssegundos) | Baixa (microssegundos-milisegundos) | Baixa (microssegundos-milisegundos) | Variável (edge (ms para conteúdo estático) | Muito Baixa (se em cache) |
| **Compartilhável entre Instâncias** | Não | Sim | Sim | Sim (por geografia) | Não (por cliente) |
| **Persistência** | Volátil (RAM) | Configurável (RDB/AOF) | Volátil (RAM) | Não (edge volátil, origem persistente) | Volátil (disco/memória cliente) |
| **Escalabilidade Vertical** | Limitado pela memória do processo | Excelente (clustering) | Excelente (clustering) | Excelente (distribuído geograficamente) | Limitado pelo dispositivo cliente |
| **Escalabilidade Horizontal** | Não | Excelente | Excelente | Excelente | Não (por cliente) |
| **Estruturas de Dados** | Simples (key-valor) | Rica (strings, listas, sets, sorted sets, hashes, streams) | Simples (key-valor) | Simples (key-valor para conteúdo estático) | Simples (key-valor) |
| **Políticas de Eviction** | LRU, LFU, FIFO (dependendo da implementação) | LRU, LFU, random, TTL | LRU, FIFO | Baseado em popularidade e TTL | Baseado em cabeçalhos HTTP (Cache-Control, Expires) |
| **Funcionalidades Adicionais** | Nenhuma | Pub/Sub, Lua scripting, transações, GEO | Nenhuma | Otimização de imagem, DDoS protection, WAF | Nenhuma |
| **Sobrecarga Operacional** | Baixa | Médio | Baixo | Médio (provedor gerencia infra) | Baixa (gerenciado pelo navegador) |
| **Quando Usar** | Dados temporários de sessão, computação intermediária | Cache distribuído, filas, leaderboards, sessões | Cache simples, alta performance, sessões | Conteúdo estático global (imagens, CSS, JS, vídeos) | Recursos estáticos por usuário, bibliotecas JS |
| **Exemplos de Uso** | Guava Cache (Java), C# MemoryCache | GitHub, Twitter, Stack Overflow | Wikipedia, Flickr, YouTube | Cloudflare, Akamai, Amazon CloudFront | Todos os navegadores modernos |

## 11. Tabela Comparativa: Padrões de Comunicação Síncrona

| Critério | HTTP/REST | gRPC | GraphQL | WebSocket | Thrift |
|----------|-----------|------|---------|-----------|--------|
| **Modelo de Comunicação** | Request/Response | Request/Response | Request/Response | Duplex bidirecional | Request/Response |
| **Formato de Dados** | JSON/XML | Protocol Buffers (binário) | JSON (tipado via schema) | Qualquer (texto/binário) | Binário (compacto) |
| **Performance** | Bom | Excelente | Bom | Excelente (baixa latência após handshake) | Excelente |
| **Tipagem** | Fraca (JSON) ou Mista (XML) | Forte (Protobuf) | Forte (schema) | Nenhuma (depende da aplicação) | Forte (IDL) |
| **Ferramentas e Ecossistema** | Excelente (muitas ferramentas) | Bom (crescente) | Excelente (ferramentas de schema, clientes) | Bom (bibliotecas em muitas linguagens) | Limitado |
| **Navegador Support** | Excelente | Limitado (requer gRPC-Web) | Excelente | Excelente | Nenhum |
| **Streaming Nativo** | Não | Server-side streaming, client-side streaming, duplex | Subscriptions (via eventos) | Sim (duplex nativo) | Não |
| **Versionamento** | Path, headers, query params | Protobuf evolution | Schema evolution | Negotiado na aplicação | IDL evolution |
| **Quando Usar** | APIs públicas, integrações simples, CRUD | Microserviços de alta performance, comunicações internas | APIs flexíveis, experiências de dados complexas | Aplicações em tempo real, jogos, colaboração | Sistemas legados, integrações de baixa latência especializada |
| **Exemplos de Uso** | Qualquer API pública (Twitter, Stripe, GitHub) | Netflix, Square, IBM | GitHub, Shopify, Twitter | Slack, Discord, jogos online | Apache Hadoop, sistemas financeiros legados |

## 12. Tabela Comparativa: Estratégias de Versionamento de API

| Critério | Versionamento por URL | Versionamento por Header | Versionamento por Parâmetro de Query | Versionamento por Negociação de Conteúdo |
|----------|----------------------|--------------------------|--------------------------------------|------------------------------------------|
| **Exemplo** | `/api/v1/users` | `Accept: application/vnd.myapi.v1+json` | `/api/users?version=1` | `Accept: application/json; version=1.0` |
| **Visibilidade** | Alta (visível na URL) | Baixa (requer inspeção de header) | Média (visível na query string) | Baixa (requer inspeção de header) |
| **Facilidade de Implementação** | Excelente | Boa | Excelente | Média |
| **Cache Friendliness** | Excelente (URLs diferentes = caches diferentes) | Boa (depende da chave de cache) | Boa (depende da chave de cache) | Média (requer chave de cache customizada) |
| **Documentação e Discoverability** | Excelente (fácil de ver e testar) | Boa (requer conhecimento do header) | Boa (visível na URL) | Limitada (oculta nos headers) |
| **Risk of Breaking Changes** | Baixo (versionamento explícito) | Médio (pode ser perdido) | Médio (pode ser perdido ou sobrescrito) | Médio (pode ser perdido) |
| **Suporte a Múltiplas Versões Simultâneas** | Excelente | Excelente | Excelente | Excelente |
| **Quando Usar** | APIs públicas onde visibilidade e simplicidade são importantes | APIs onde se quer URLs limpas e controle fino | APIs simples onde parâmetros de query são aceitáveis | APIs onde se quer controle fino sobre representação de dados |
| **Exemplos de Uso** | Twitter API, GitHub API, Stripe API | Microsoft Azure APIs, niektóre API Google | Algumas APIs REST simples, APIs internas | APIs de mídia, APIs de negociação de conteúdo especializada |

## 13. Tabela Comparativa: Estilos de Documentação de API

| Critério | OpenAPI/Swagger | RAML | API Blueprint | GraphQL Schema |
|----------|-----------------|------|---------------|----------------|
| **Formato** | YAML/JSON | YAML | Markdown | Schema Definition Language (SDL) |
| **Popularidade** | Muito Alta | Média | Baixa | Alta (crescendo com GraphQL) |
| **Ferramentas de Geração** | Excelente (codegen, mocks, teste) | Bom | Bom | Excelente (codegen, mocks, teste) |
| **Ferramentas de Visualização** | Excelente (Swagger UI, Redoc) | Bom (RAML Console) | Bom (APIary) | Excelente (GraphQL Playground, Apollo Studio) |
| **Suporte a Versionamento** | Excelente | Excelente | Excelente | Evolutivo (schema evolution) |
| **Suporte a Autorização** | Excelente (OAuth2, API Key, etc.) | Bom | Bom | Bom (diretivas customizadas) |
| **Legibilidade para Humanos** | Boa | Excelente | Excelente | Boa |
| **Legibilidade para Máquinas** | Excelente | Excelente | Excelente | Excelente |
| **Quando Usar** | APIs REST padrão, necessidade de amplo ecossistema de ferramentas | Projetos onde se quer foco em design primeiro | Documentação rica em narrativa e exemplos | APIs GraphQL, onde o schema é o contrato |
| **Exemplos de Uso** | Quase todas as APIs REST modernas (AWS, Azure, Google APIs) | MuleSoft, algumas APIs empresariais | Apiary (antes da aquisição), algumas documentações técnicas | GitHub GraphQL API, Shopify GraphQL API, GraphQL.org |

## 14. Tabela Comparativa: Métodos de Testagem

| Critério | Teste de Unidade | Teste de Integração | Teste de Contrato | Teste de Ponta a Ponta (E2E) |
|----------|------------------|---------------------|-------------------|-----------------------------|
| **Escopo** | Função/método/classe isolado | Interação entre componentes | Promessa entre provedor/consumidor | Fluxo completo de usuário |
| **Velocidade** | Milisegundos | Segundos a minutos | Milisegundos a segundos | Minutos a horas |
| **Frequência de Execução** | A cada salvamento/commit | Pull request ou schedule | A cada mudança no provedor/consumidor | Pipeline de release ou schedule noturno |
| **Dependências Externas** | Mockadas/Stubbed | Parcialmente reais ou em containers | Mockadas (validam contrato) | Quase tudo real ou com high-fidelity mocks |
| **Manutenção** | Baixa | Média | Baixa | Alta (frágil a mudanças de UI) |
| **Confiança no Resultado** | Alta (lógica específica) | Média (integração) | Alta (contrato cumprido) | Alta (experiência do usuário) |
| **Quando Usar** | Validar lógica de negócio específica | Validar integração entre serviços | Validar compatibilidade entre serviços | Validar experiência completa do usuário |
| **Ferramentas Comuns** | JUnit, NUnit, pytest, Jest | Mesmas + Docker/TestContainers | Pact, Spring Cloud Contract | Selenium, Cypress, Playwright, TestCafe |
| **Exemplo de Uso** | Validar cálculo de desconto | Validar que serviço A chama serviço B corretamente | Validar que formato de resposta do serviço B está correto | Validar que usuário pode fazer login, buscar produto e completar compra |

## 15. Tabela Comparativa: Estratégias de Deploy

| Critério | Deploy Manual | Deploy Scripted | CI/CD básico | CI/CD avançado (GitOps) | Blue/Green Deployment | Canary Release |
|----------|---------------|-----------------|--------------|-------------------------|-----------------------|----------------|
| **Automação** | Nenhuma | Scripts (shell, batch) | Pipeline básico (build/test/deploy) | Pipeline completo (Git como fonte da verdade) | Automatizado com switch de tráfego | Automatizado com roteamento percentual |
| **Complexidade de Setup** | Baixa | Baixa-Média | Média | Alta | Média | Média-Alta |
| **Rollback Simples** | Difícil (manual) | Médio (depende do script) | Bom (re-deploy versão anterior) | Excelente (revert commit) | Excelente (switch de volta) | Bom (pare ou diminua canário) |
| **Visibilidade de Mudanças** | Baixa | Média | Boa | Excelente (Git history) | Boa (ambientes separados) | Boa (métricas de canário vs produção) |
| **Risco de Deploy** | Alto | Médio | Médio-Baixo | Baixo | Baixo-Médio | Baixo |
| **Quando Usar** | Emergências, ambientes muito simples | Pequenas equipes, projetos simples | Maioria dos projetos de médio porte | Grandes escala, equipes maduras em DevOps | Releases críticas onde downtime deve ser minimizado | Releases onde se quer validar com pequena porcentagem de usuários primeiro |
| **Ferramentas Comuns** | FTP, SSH, painel de controle | Shell scripts, Make, Ant, Maven | Jenkins, GitLab CI, GitHub Actions | Argo CD, Flux, Jenkins X | Kubernetes, Service Mesh, Load Balancers | Kubernetes, Service Mesh, Load Balancers, Feature Flags |
| **Exemplo de Uso** | Correção de bug crítico em produção | Site WordPress pequeno, aplicação interna | Aplicação web de médio porte, API REST | Plataforma de e-commerce, sistema financeiro | Sistema bancário, aplicação de saúde | Lançamento de nova funcionalidade em rede social, jogo online |

## 16. Conclusão

As tabelas comparativas apresentadas nesta parte fornecem uma base sólida para tomada de decisão arquitetural informada. Elas ajudam a visualizar trade-offs claros entre diferentes opções, considerando aspectos técnicos, operacionais e de negócio.

Lembre-se de que estas comparações são pontos de partida. A "melhor" escolha sempre dependerá do contexto específico do seu projeto, incluindo:

- Requisitos funcionais e não-funcionais
- Restrições de tempo, orçamento e recursos
- Experiência e habilidades da equipe
- Estratégia de longo prazo e evolução esperada do sistema
- Fatores de negócio específicos do seu domínio

Use estas tabelas como ferramentas para estruturar suas discussões de arquitetura, mas sempre complemente-as com análise profunda do seu contexto específico e validação através de protótipos ou proof-of-concepts quando necessário.

> **Próxima Parte**: PARTE 76 — GLOSSÁRIO - Definições de termos-chave em arquitetura de software para referência rápida e padronização de linguagem.