---
trilha: "AVANÇADA"
---
**Navegação:** [[MOC — TRILHA AVANÇADA]]
← [[PARTE 30 — CACHING]] | #trilha/avancada | [[PARTE 32 — CDN]] →

---
# PARTE 30 — LOAD BALANCING

> 🧠 **ESSENCIAL**
> Load balancing (balanceamento de carga) é a técnica de distribuir o tráfego de rede ou de aplicações entre múltiplos servidores para otimizar o uso de recursos, maximizar o throughput, minimizar o tempo de resposta e evitar sobrecarga em qualquer singolo recurso.

> 🎯 **ENTREVISTA — ALTA FREQUÊNCIA**
> Perguntas sobre algoritmos de load balancing (round-robin, least connections, IP hash, etc.), load balancers de camada 4 vs camada 7, health checks, persistência de sessão, e problemas comuns como o "thundering herd" são extremamente comuns em entrevistas de arquitetura de software.

## O que é Load Balancing?

**Load balancing** é o processo de distribuir solicitações de rede ou de aplicação entre múltiplos servidores de backend (também chamados de pool, farm ou conjunto de servidores) para garantir que nenhum servidor individual fique sobrecarregado.

### Por que usar load balancing?

1. **Escalabilidade**: Permite adicionar mais servidores para lidar com aumento de tráfego
2. **Disponibilidade Alta**: Se um servidor falhar, o tráfego é redirecionado para outros servidores operacionais
3. **Performance Melhorada**: Distribui carga para evitar gargalos em servidores individuais
4. **Manutenção Sem Downtime**: Servidores podem ser retirados para manutenção sem afetar o serviço
5. **Uso Eficiente de Recursos**: Evita que alguns servidores fiquem ociosos enquanto outros estão sobrecarregados
6. **Tolerância a Falhas**: Detecta e remove automaticamente servidores com falha do pool

## Como funciona internamente

### Componentes Básicos de um Load Balancer

1. **Cliente**: Fonte das solicitações (navegador, aplicativo móvel, outro serviço)
2. **Load Balancer (LB)**: Dispositivo ou software que distribui as solicitações
3. **Pool de Servidores**: Conjunto de servidores de backend que processam as solicitações
4. **Health Checks**: Mecanismo para monitorar a saúde dos servidores de backend
5. **Algoritmo de Balanceamento**: Regra que determina como distribuir as solicitações

### Fluxo de Trabalho Básico

1. **Cliente envia solicitação** para o load balancer
2. **Load balancer aceita a conexão** e aplica o algoritmo de balanceamento
3. **Load balancer seleciona um servidor** do pool baseado no algoritmo e health checks
4. **Load balancer encaminha a solicitação** para o servidor selecionado
5. **Servidor processa a solicitação** e envia resposta de volta
6. **Load balancer encaminha a resposta** para o cliente (em modo proxy) ou permite conexão direta (em modo NAT)

### Tipos de Load Balancing por Camada OSI

#### Load Balancer de Camada 4 (Transport Layer)
- Opera no nível de TCP/UDP
- Decisões baseadas em endereços IP e portas
- Não vê conteúdo da aplicação (HTTP headers, cookies, etc.)
- Exemplos: HAProxy em modo TCP, AWS Network Load Balancer, LVS (Linux Virtual Server)
- Vantagens: Muito rápido, baixo latency, mínimo overhead
- Desvantagens: Limitado a balanceamento baseado em IP/porta, sem percepção de aplicação

#### Load Balancer de Camada 7 (Application Layer)
- Opera no nível de aplicação (geralmente HTTP/HTTPS)
- Decisões baseadas em conteúdo da aplicação (URL, headers, cookies, etc.)
- Pode realizar operações como SSL termination, inspeção de conteúdo, rewriting
- Exemplos: HAProxy em modo HTTP, AWS Application Load Balancer, NGINX, F5 BIG-IP
- Vantagens: Balanceamento inteligente baseado em conteúdo, recursos avançados
- Desvantagens: Maior overhead devido ao processamento de aplicação, latency ligeiramente maior

## Algoritmos de Load Balancing

### 1. Round Robin (RR)
- **Como funciona**: Distribui solicitações sequencialmente para cada servidor no pool
- **Exemplo**: Servidores A, B, C → solicitações vão na sequência A→B→C→A→B→C...
- **Vantagens**: Simples, justo, distribuição uniforme quando servidores têm capacidade semelhante
- **Desvantagens**: Não considera carga atual ou capacidade dos servidores
- **Quando usar**: Servidores homogêneos com carga relativamente uniforme

### 2. Weighted Round Robin (WRR)
- **Como funciona**: Similar ao round robin, mas atribui pesos aos servidores baseados em capacidade
- **Exemplo**: Servidor A (peso 3), B (peso 1) → sequência A→A→A→B→A→A→A→B...
- **Vantagens**: Leva em conta diferenças de capacidade entre servidores
- **Desvantagens**: Ainda não considera carga atual em tempo real
- **Quando usar**: Servidores heterogêneos com conhecidas diferenças de capacidade

### 3. Least Connections
- **Como funciona**: Envia nova solicitação para o servidor com menor número de conexões ativas
- **Vantagens**: Distribui carga baseado no estado atual dos servidores
- **Desvantagens**: Requer tracking de conexões, pode ser desigual se conexões têm durações muito diferentes
- **Quando usar**: Sessões com durações variáveis significativamente

### 4. Weighted Least Connections
- **Como funciona**: Combina least connections com pesos de capacidade
- **Fórmula**: Seleciona servidor com menor valor de (conexões_ativas / peso)
- **Vantagens**: Considera tanto capacidade quanto carga atual
- **Desvantagens**: Mais complexo de calcular e implementar
- **Quando usar**: Ambientes heterogêneos onde se deseja balanceamento sensível à carga

### 5. IP Hash (Source IP Affinity)
- **Como funciona**: Usa hash do IP do cliente para determinar qual servidor receberá a solicitação
- **Mesmo cliente sempre vai para o mesmo servidor** (desde que o pool não mude)
- **Vantagens**: Mantém afinidade de sessão sem necessidade de armazenamento de estado
- **Desvantagens**: Distribuição pode ser desigual se certos IPs gerarem mais tráfego
- **Quando usar**: Quando affinidade de sessão é necessária e sessões são relativamente uniformes

### 6. URL Hash
- **Como funciona**: Usa hash da URL da solicitação para determinar o servidor
- **Mesma URL sempre vai para o mesmo servidor**
- **Vantagens**: Boa para caching - aumenta probabilidade de cache hit
- **Desvantagens**: Pode criar hot spots se certos URLs forem muito populares
- **Quando usar**: Sistemas onde caching é importante e URLs têm distribuição razoavelmente uniforme

### 7. Least Response Time
- **Como funciona**: Envia solicitação para o servidor com menor tempo de resposta médio
- **Requer medição contínua do tempo de resposta**
- **Vantagens**: Direciona tráfego para servidores que estão respondendo mais rápido
- **Desvantagens**: Pode oscilar se todos servidores começarem a enviar tráfego para o mesmo servidor "rápido"
- **Quando usar**: Quando tempo de resposta é crítico e varia significativamente entre servidores

### 8. Random Choice with Two Options (Power of Two Choices)
- **Como funciona**: Seleciona dois servidores aleatoriamente e escolhe o que tiver menos conexões
- **Vantagens**: Muito melhor que random puro, quase tão bom quanto least connections, mas mais simples
- **Desvantagens**: Ligeiramente menos preciso que least connections puro
- **Quando usar**: Quando se quer um bom balanceamento com menor overhead de tracking

## Health Checks e Detecção de Falhas

### Tipos de Health Checks

1. **TCP Connect Health Check**
   - Simplesmente tenta estabelecer conexão TCP
   - Rápido, mas não verifica se a aplicação está realmente funcionando
   - Bom para serviços simples onde conectar TCP = serviço funcionando

2. **HTTP Health Check**
   - Faz uma requisição HTTP (geralmente GET) para um endpoint específico
   - Verifica código de status de resposta (200 = saudável)
   - Pode verificar conteúdo da resposta
   - Bom para aplicações web

3. **HTTPS Health Check**
   - Similar ao HTTP, mas sobre TLS/SSL
   - Necessário quando backend só aceita HTTPS

4. **Health Check Específico da Aplicação**
   - Endpoint personalizado que verifica saúde profunda (ex: conexão com DB, fila, cache)
   - Pode retornar informações detalhadas sobre saúde
   - Ex: `/health` que verifica DB, cache, dependências externas

### Configuração de Health Checks

- **Intervalo**: Quão frequentemente realizar o check (ex: a cada 5 segundos)
- **Timeout**: Quanto tempo aguardar por resposta antes considerar falha
- **Limites**: Número de falhas consecutivas para marcar como unhealthy, número de sucessos para marcar como healthy novamente
- **Endpoint**: URL ou porta específica para verificar
- **Expectativa**: Código de status esperado, conteúdo esperado na resposta

## Persistência de Sessão (Session Affinity / Sticky Sessions)

### Quando é Necessária

- Aplicações que armazenam estado localmente no servidor de backend
- Ex: carrinho de compras em memória, sessões de jogo, caches locais
- Quando não há mecanismo externo de compartilhamento de estado (como Redis, banco de dados)

### Tipos de Persistência

1. **Source IP Affinity**
   - Baseado no IP do cliente (como discutido no algoritmo IP Hash)
   - Simples, mas pode quebrar com NATs, proxies, ou quando IP do cliente muda

2. **Cookie-Based Persistence**
   - Load balancer injeta ou lê um cookie para identificar a sessão
   - Cookie contém informação sobre qual servidor deve receber a solicitação
   - Pode ser inserido pelo LB (insert cookie) ou lido de cookie existente (rewrite cookie)

3. **Session ID Persistence**
   - Extrai session ID de cookies ou URL e mapeia para servidor
   - Requer que o LB entenda o formato de session ID da aplicação
   - Exemplos: JSESSIONID, PHPSESSID, ASP.NET_SessionId

4. **SSL Session ID Persistence**
   - Usa o ID da sessão SSL para afinidade
   - Funciona apenas para tráfego HTTPS
   - Não requer inserção de cookies pelo LB

### Desvantagens da Persistência de Sessão

- Pode levar à distribuição desigual de carga se algumas sessões forem muito mais ativas
- Reduz eficácia do failover (se um servidor falhar, todas suas sessões "grudadas" são perdidas)
- Complexidade aumentada no load balancer
- Pode interferir com algoritmos de balanceamento ótimo

## Recursos Avançados de Load Balancers

### 1. SSL/TLS Termination
- **Como funciona**: LB descriptografa tráfego HTTPS, comunica-se com backends via HTTP
- **Vantagens**: 
  - Remove sobrecarga de criptografia dos servidores de aplicação
  - Permite inspeção de conteúdo para balanceamento avançado
  - Centraliza gerenciamento de certificados
- **Desvantagens**:
  - LB se torna ponto único de falha para criptografia
  - Tráfego interno entre LB e backends não é criptografado (precisa de rede confiável)

### 2. HTTP/2 e HTTP/3 Support
- **HTTP/2**: Multiplexing, header compression, server push
- **HTTP/3**: Baseado em QUIC, melhor performance em redes com perda
- LB moderno suporta estes protocollos tanto na frente quanto nos backends

### 3. Content-Based Routing
- Roteia baseado em conteúdo da requisição:
  - Path-based: `/api/users` → pool de usuários, `/api/orders` → pool de pedidos
  - Header-based: `X-Version: v2` → pool de versão 2
  - Host-based: `api.example.com` vs `www.example.com` → pools diferentes
  - Query parameter-based: `?region=us-east` → pool específico da região

### 4. Rate Limiting e Throttling
- Limita número de solicitações por cliente, IP, ou API key
- Pode ser aplicado globalmente ou por rota/endpoint
- Técnicas: token bucket, leaky window, fixed window

### 5. Caching e Compression
- **Caching**: Armazena respostas estáticas para servir diretamente
- **Compression**: Compacta respostas (gzip, brotli) para reduzir largura de banda
- Ambos podem ser feitos pelo LB para descarregar trabalho dos backends

### 6. WAF (Web Application Firewall) Integration
- Protege contra ataques comuns: SQL injection, XSS, CSRF, etc.
- Pode ser integrado ao LB ou como função separada
- Regras configuráveis para bloquear tráfego malicioso

### 7. Observabilidade e Métricas
- Métricas de throughput, latência, taxas de erro
- Distribuição de carga por servidor
- Contadores de conexões ativas, sessões, etc.
- Logging detalhado para troubleshooting
- Integração com sistemas de monitoramento (Prometheus, Datadog, etc.)

## Algoritmos e Estratégias Avançadas

### Consistent Hashing Load Balancing
- Aplica consistent hashing para distribuir solicitações
- Quando servidores são adicionados/removidos, apenas fração mínima de sessões precisa ser remapeada
- Útil quando afinidade de sessão é importante e mudanças de cluster são frequentes

### Least Pending Requests (LPR)
- Similar a least connections, mas conta requisições que foram enviadas mas ainda não receberam resposta completa
- Melhor para protocollos pipeline ou multiplexed (HTTP/2, gRPC)

### Adaptive Load Balancing
- Ajusta dinamicamente pesos ou algoritmos baseado em métricas de performance
- Ex: aumentar peso de servidores com menor latência ou menor taxa de erro

### Geographic Load Balancing (GSLB - Global Server Load Balancing)
- Distribui tráfego baseado em localização geográfica do cliente
- Usa DNS para retornar IP de servidor mais próximo
- Pode considerar saúde de datacenters inteiros, não apenas servidores individuais

### Microseconds-Level Load Balancing
- Para aplicações de alta frequência de trading ou HPC
- Algoritmos extremamente otimizados para tomar decisões em microssegundos
- Geralmente implementado em hardware especializado (FPGA, SmartNIC)

## Implementações Populares de Load Balancers

### Software Load Balancers (L7)

#### HAProxy
- **Tipo**: L4/L7, proxy
- **Vantagens**: Extremamente rápido, confiável, rico em funcionalidades
- **Uso comum**: Load balancing geral, API gateways, microsevicess
- **Recursos avançados**: ACLs extensivas, stick tables, Lua scripting

#### NGINX/Nginx Plus
- **Tipo**: L7 proxy/reverse proxy
- **Vantagens**: Boa performance, fácil configuração, bom para servir conteúdo estático também
- **Uso comum**: Web serving, reverse proxy, load balancing
- **Recursos avançados**: Módulo de streaming, cache, WAF no Nginx Plus

#### Envoy Proxy
- **Tipo**: L3/L4/L7 proxy (originalmente L7, expandido)
- **Vantagens**: Alta performance, observabilidade excelente, cloud-native
- **Uso comum**: Service mesh (com Istio), edge proxy, ingress controller
- **Recursos avançados**: Filtros configuráveis, statistics avançadas, hot restart

#### Traefik
- **Tipo**: L7 reverse proxy/load balancer
- **Vantagens**: Configuração dinâmica via providers (Docker, Kubernetes, Consul, etc.)
- **Uso comum**: Ambientes containerizados, microserviços
- **Recursos avançados**: Integração natural com orquestradores de container

#### Apache HTTP Server (mod_proxy, mod_balancer)
- **Tipo**: L7 proxy
- **Vantagens**: Amplamente conhecido, bom ecossistema de módulos
- **Uso comum**: Ambientes onde Apache já está em uso

### Software Load Balancers (L4)

#### LVS (Linux Virtual Server)
- **Tipo**: L4 no kernel Linux
- **Vantagens**: Muito rápido (processamento no kernel), escalável
- **Tipos**: NAT, Tunneling, Direct Routing (DR)
- **Uso comum**: Alta performance L4 balancing em Linux

#### IPVS (IP Virtual Server)
- **Sucessor do LVS** com mais funcionalidades
- **Integração**: Parte do kernel Linux moderno

### Hardware Load Balancers

#### F5 BIG-IP
- **Tipo**: L4/L7 appliance
- **Vantagens**: Altamente funcional, bom suporte, recursos avançados
- **Desvantagens**: Caro, proprietário
- **Uso comum**: Grandes empresas, data centers, provedores de serviço

#### Citrix ADC (NetScaler)
- **Tipo**: L4/L7 appliance
- **Vantagens**: Bom para aplicações Citrix, recursos avançados de otimização
- **Uso comum**: Ambientes corporativos com foco em aplicações Windows

#### A10 Networks Thunder Series
- **Tipo**: L4/L7 appliance
- **Vantagens**: Boa performance, foco em eficiência
- **Uso comum**: Provedores de serviço, empresas médio-grandes

### Cloud Load Balancers (Managed Services)

#### AWS
- **Application Load Balancer (ALB)**: L7, HTTP/HTTPS, avançado
- **Network Load Balancer (NLB)**: L4, ultra-baixo latency, milhões de requisições por segundo
- **Classic Load Balancer (CLB)**: Legado, suporte a múltiplos protocolos
- **Gateway Load Balancer (GLB)**: Para appliance de terceiros (firewalls, WAF)

#### Azure
- **Application Gateway**: L7, WAF integrado
- **Load Balancer**: L4, básico
- **Front Door**: L7 global, com CDN e WAF

#### Google Cloud
- **External HTTP(S) Load Balancer**: L7 global
- **Internal HTTP(S) Load Balancer**: L7 interno
- **Network Load Balancer**: L4 regional
- **TCP Proxy Load Balancer**: L4 global
- **SSL Proxy Load Balancer**: L4 global com SSL termination

#### Cloudflare
- **Load Balancing**: L7 global com health checks e failover
- **Argo Smart Routing**: Otimização de rota baseado em condições de rede
- **Magic Transit**: L3/L4 DDoS protection e load balancing

## Quando Usar Cada Tipo de Load Balancer

### Escolha por Requisitos Funcionais

#### Use L7 Load Balancer quando:
- Precisa de balanceamento baseado em conteúdo (URL, headers, cookies)
- Necessita de SSL termination no load balancer
- Quer recursos avançados como caching, compression, WAF
- Está fazendo microserviços ou API management
- Precisa de observabilidade detalhada (métricas, logging, tracing)

#### Use L4 Load Balancer quando:
- Performance extrema é crítica (microsegundos importam)
- Balanceamento simples baseado em IP/porta é suficiente
- Não precisa de inspeção de conteúdo da aplicação
- Está balanceando protocolos não-HTTP (banco de dados, mensageria, etc.)
- Quer minimizar pontos de falha e complexity

### Escolha por Ambiente de Deploy

#### Bare Metal / VMs Tradicionais:
- HAProxy, NGINX, LVS/IPVS, soluções hardware

#### Kubernetes:
- Ingress Controllers (NGINX, HAProxy, Envoy, Traefik, Istio)
- Services (ClusterIP, NodePort, LoadBalancer tipo cloud provider)
- Service Mesh (Istio, Linkerd, Consul Connect) para L7 service-to-service

#### Ambientes Containerizados (Docker Swarm, ECS):
- HAProxy, NGINX, Traefik, soluções específicas da plataforma
- AWS ELB com ECS, Azure Load Balancer com AKS, etc.

#### Serverless / Funções como Serviço:
- Geralmente não precisa de LB explícito (plataforma fornece)
- Pode usar API Gateway (que inclui LB) como front end

#### Arquiteturas de Microserviços:
- Service Mesh para comunicação entre serviços (Istio, Linkerd)
- API Gateway / Ingress Controller para entrada no sistema
- Possivelmente LB adicional para frontends públicos

## Desafios e Problemas Comuns

### 1. The Thundering Herd Problem
- **Problema**: Múltiplos workers/processos são despertados simultaneamente quando uma nova conexão chega, mas apenas um consegue aceitá-la
- **Impacto**: Desperdício de recursos CPU, aumento de latency
- **Soluções**:
  - Use accept mutexes (como no NGINX)
  - Algoritmos que distribuem accept() de forma mais uniforme
  - Arquiteturas baseado em eventos (Node.js, Go netpoll) que evitam o problema

### 2. Sessão Perdida Após Failover
- **Problema**: Quando um servidor de backend falha, sessões "grudadas" nele são perdidas
- **Impacto**: Usuários perdem estado (carrinho de compras, progresso de jogo, etc.)
- **Soluções**:
  - Armazenar estado externamente (Redis, banco de dados, cache distribuído)
  - Usar técnicas de replicação de sessão entre servidores
  - Acceptar perda ocasional de sessão se o impacto for baixo

### 3. Distribuição Desigual de Carga (Hot Spots)
- **Problema**: Algoritmo de balanceamento ou padrões de tráfego causam carga desigual
- **Impacto**: Alguns servidores sobrecarregados enquanto outros estão ociosos
- **Soluções**:
  - Monitore métricas de carga por servidor (conexões, latência, taxas de erro)
  - Ajuste algoritmos ou pesos baseado em observação
  - Considere algoritmos mais sensíveis à carga atual (least connections, response time)
  - Verifique se problemas estão na aplicação (alguns endpoints muito lentos)

### 4. Problemas com HTTP Keep-Alive e Conexões Longas
- **Problema**: Conexões HTTP persistentes permanecem abertas por muito tempo, causando desbalanceamento
- **Impacto**: Servidores podem ficar com muitas conexões ociosas enquanto novos arrivals vão para outros
- **Soluções**:
  - Configure timeouts apropriados para keep-alive
  - Use algoritmos que considerem conexões ociosas vs ativas
  - Implemente draining gracefully ao remover servidores do pool

### 5. Sticky Sessions Causando Desequilíbrio
- **Problema**: Afinidade de sessão leva alguns servidores a receberem desproporcionalmente mais carga
- **Impacto**: Similar ao hot spot, mas causado pela persistência de sessão
- **Soluções**:
  - Monitore distribuição de carga mesmo com persistência ativa
  - Considere reduzir tempo de vida das sessões ou usar armazenamento externo
  - Avalie se realmente é necessária (muitas aplicações modernas não precisam)

### 6. SSL/TLS Overhead e Certificate Management
- **Problema**: Sobrecarga computacional de criptografia, gerenciamento complexo de certificados
- **Impacto**: Consumo de CPU, possibilidade de certificados expirados
- **Soluções**:
  - Offload SSL para load balancer especializado ou hardware de aceleração
  - Automatize renovação de certificados (Let's Encrypt, ACME)
  - Use certificados wildcard ou SAN para simplificar gerenciamento
  - Considere internal TLS entre LB e backends se segurança interna for necessária

### 7. Problemas de Escalabilidade do Próprio Load Balancer
- **Problema**: O load balancer se torna gargalo ao escalar demais
- **Impacto**: Limita a escala máxima do sistema inteiro
- **Soluções**:
  - Escalamento horizontal do load balancer (clustering, failover pairs)
  - Use load balancers projetados para alta escala (hardware especializado, soluções cloud-native)
  - Considere arquiteturas em múltiplas camadas (global LB → regional LB → local LB)

## Best Practices para Load Balancing

### 1. Projeto e Planejamento
- Entenda profundamente os padrões de tráfego da sua aplicação
- Defina requisitos claros de performance, disponibilidade e funcionalidades
- Considere tanto necessidades atuais quanto futuras de escala
- Planeje para falhas - como o sistema se comporta quando componentes falham?

### 2. Configuração de Health Checks
- Use health checks que reflitam verdadeiramente a saúde da aplicação
- Não verifique apenas conectividade TCP se a aplicação pode estar "up" mas quebrada
- Configure intervalos e timeouts apropriados (nem muito agressivo nem muito lento)
- Tenha limites razoáveis para marcação de unhealthy/recovery

### 3. Escolha de Algoritmo
- Comece com algoritmos simples (round robin, least connections)
- Ajuste baseado em observação de métricas reais
- Considere pesos para servidores heterogêneos
- Evite algoritmos complexos demais a menos que haja benefício comprovado

### 4. Persistência de Sessão
- Evite quando possível - projete aplicações para serem stateless
- Se necessário, use mecanismos externos de compartilhamento de estado
- Monitore impacto na distribuição de carga
- Configure timeouts razoáveis para sessões grudadas

### 5. Segurança
- Mantenha load balancer e sistemas operacionais atualizados
- Configure apenas portas e protocolos necessários
- Implemente rate limiting e proteção contra DDoS básica
- Considere WAF para proteção de aplicação
- Use criptografia forte e gerencie certificados adequadamente

### 6. Observabilidade e Monitoramento
- Métricas essenciais: throughput, latência, taxas de erro, distribuição de carga
- Alertas para: alta latência, baixa taxa de sucesso, servidores marcados como unhealthy
- Logging estruturado para troubleshooting fácil
- Integre com sistemas de monitoramento e alertas existentes
- Considere tracing distribuído para entender fluxo de requisições

### 7. Testes e Validação
- Teste sob carga realista antes de ir para produção
- Simule falhas de servidor para verificar failover
- Teste mudanças de algoritmo e configuração em ambiente de staging
- Valide que persistência de sessão funciona como esperado (se usada)
- Faça testes de carga para encontrar limites e gargalos

### 8. Manutenção e Operações
- Procedures claros para adicionar/remover servidores do pool
- Estratégias de draining gracefully para manutenção sem perder solicitações
- Planos de resposta para incidentes comuns (LB down, todos backends unhealthy, etc.)
- Documentação clara da configuração e procedures operacionais
- Revisões periódicas de configuração e eficiência

## Perguntas de Entrevista Comuns

### Básicas
- "O que é load balancing e por que ele é usado?"
- "Quais são as diferenças entre load balancing de camada 4 e camada 7?"
- "Explique o algoritmo round robin e suas limitações."

### Intermediárias
- "Como você escolheria um algoritmo de load balancing para um sistema específico?"
- "Quais são os diferentes tipos de health checks e quando usar cada um?"
- "Explique o problema do 'thundering herd' e como mitigá-lo."
- "Como funcionam as sessões grudadas (sticky sessions) e quais são suas desvantagens?"

### Avançadas
- "Como você projetaria um sistema de load balancing para lidar com milhões de requisições por segundo?"
- "Discuta trade-offs entre usar um load balancer de hardware versus soluções de software."
- "Como você lidaria com o desafio de balanceamento de carga em arquiteturas de microserviços?"
- "Explique como você implementaria um sistema de load balancing que seja consciente da localização geográfica dos usuários."

### Follow-ups Típicos
- "E se precisássemos mudar nosso algoritmo de load balancing após o sistema estar em produção?"
- "Como você validaria que seu load balancer está distribuindo carga uniformemente sob carga real?"
- "Qual seria sua estratégia para migrar de um load balancer legado para uma solução moderna sem downtime?"
- "E se descobríssemos que nosso padrão de acesso tem características que tornam certos algoritmos ineficazes?"

## Checklist de Projeto e Implementação de Load Balancing

### Antes de Começar o Projeto
- [ ] Analisar padrões de tráfego esperados (volume, distribuição temporal, geografica)
- [ ] Definir requisitos de performance (latência alvo, throughput necessário)
- [ ] Definir requisitos de disponibilidade (SLA, tolerância a falhas)
- [ ] Determinar necessidades de funcionalidades (SSL termination, WAF, caching, etc.)
- [ ] Avaliar arquitetura da aplicação (stateless vs stateful, necessidades de afinidade)
- [ ] Pesquisar tecnologias de load balancing adequadas ao ambiente e requisitos
- [ ] Planejar estratégia de health checks apropriada para suas aplicações
- [ ] Definir requisitos de observabilidade (métricas, logging, alertas)
- [ ] Planejar estratégia de escala (como adicionar capacidade quando necessário)

### Durante a Implementação
- [ ] Selecionar e configurar tecnologia de load balancing apropriada
- [ ] Implementar health checks que refletem verdadeiramente saúde da aplicação
- [ ] Configurar algoritmo de balanceamento inicial baseado em análise de necessidades
- [ ] Implementar persistência de sessão apenas se absolutamente necessário
- [ ] Configurar SSL/TLS termination se necessário (certificados, protocolos, cifras)
- [ ] Implementar recursos avançados conforme necessário (rate limiting, WAF, caching)
- [ ] Configurar logging e métricas para observabilidade
- [ ] Implementar procedures de adicionar/remover servidores do pool (drain mode)
- [ ] Testar extensivamente em ambiente de staging com cargas realistas

### Depois da Implementação e em Produção
- [ ] Monitorar distribuição de carga entre servidores de backend
- [ ] Alertar sobre servidores marcados como unhealthy por health checks
- [ ] Rastrear métricas de performance (latência, taxas de erro, throughput)
- [ ] Validar que failover funciona corretamente quando servidores falham
- [ ] Revisar periodicamente eficácia do algoritmo de balanceamento escolhido
- [ ] Ajustar pesos ou algoritmos baseado em observação de métricas reais
- [ ] Testar e validar procedures de manutenção (adicionar/remover servidores)
- [ ] Manter documentação atualizada de configuração e procedures operacionais
- [ ] Revisar e aplicar patches de segurança regularmente
- [ ] Planejar capacidade futura baseado em tendências de crescimento observadas

## RESUMO

Load balancing é uma técnica fundamental para construir sistemas escaláveis, altamente disponíveis e com boa performance:

**Princípios-chave:**
1. Load balancing distribui tráfego entre múltiplos servidores para otimizar recursos e melhorar disponibilidade
2. Existe um espectro de soluções desde L4 (rápido, simples) até L7 (inteligente, rico em funcionalidades)
3. A escolha do algoritmo de balanceamento deve ser baseada em padrões de tráfego e características dos servidores
4. Health checks eficazes são críticos para detectar e remover servidores com falha automaticamente
5. Persistência de sessão deve ser usada com cautela devido ao potencial de desequilíbrio de carga e perda de estado
6. Observabilidade, monitoramento e procedures operacionais claros são essenciais para sucesso em produção
- [ ] Lembre-se: O load balancing eficaz não se trata apenas de escolher o algoritmo certo, mas de entender profundamente seu aplicativo, padrões de tráfego, e requisitos de negócio para projetar uma solução que escale adequadamente enquanto mantém alta disponibilidade e boa performance.

