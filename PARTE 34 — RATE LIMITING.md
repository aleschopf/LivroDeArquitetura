---
trilha: "AVANÇADA"
---
**Navegação:** [[MOC — TRILHA AVANÇADA]]
← [[PARTE 32 — CDN]] | #trilha/avancada | [[PARTE 34 — RESILIENCE]] →

---
# PARTE 33 — API GATEWAY

> 🧠 **ESSENCIAL**
> Rate limiting (limitação de taxa) é uma técnica de controle de tráfego que restringe o número de requisições ou ações que um cliente pode realizar dentro de um intervalo de tempo específico, protegendo serviços de sobrecarga, abuso e garantindo uso justo de recursos.

> 🎯 **ENTREVISTA — ALTA FREQUÊNCIA**
> Perguntas sobre algoritmos de rate limiting (fixed window, sliding window, token bucket, leaky bucket), onde aplicar (edge, API gateway, serviço), headers de resposta, estratégias de distribuição em sistemas escaláveis, e trade-offs entre precisão e performance são muito comuns em entrevistas de arquitetura de software.

## O que é Rate Limiting?

**Rate limiting** é o processo de controlar a taxa de requisições ou ações que um cliente pode fazer em um sistema dentro de um determinado período de tempo. Ele é usado para prevenir sobrecarga de recursos, proteger contra ataques de negação de serviço (DoS), garantir uso justo entre usuários, e cumprir limites de contratos de serviço (SLAs).

### Por que usar rate limiting?

1. **Proteção contra Abuso**: Previne usuários mal-intencionados ou bots de esgotarem recursos
2. **Garantia de Qualidade de Serviço (QoS)**: Assegura que nenhum consumidor monopolize os recursos
3. **Proteção contra Sobrecarga**: Evita que picos inesperados de tráfego derrubem o serviço
4. **Conformidade com SLAs**: Ajuda a garantir que limites contratuais de uso sejam respeitados
5. **Custo Controlado**: Evita uso excessivo de recursos pagos (como chamadas a APIs de terceiros)
6. **Segurança**: Mitiga certos tipos de ataques como força bruta, credential stuffing, e scraping agressivo
7. **Experiência do Usuário Justa**: Distribui recursos de forma mais equitativa entre todos os usuários

## Como funciona internamente

### Componentes Básicos de um Sistema de Rate Limiting

1. **Identificador do Cliente**: Como identificamos quem está fazendo a requisição (IP, API key, user ID, token, etc.)
2. **Algoritmo de Contagem**: Método usado para contar requisições dentro de janelas de tempo
3. **Armazenamento de Contadores**: Onde mantemos o estado de contagem (memória, Redis, banco de dados, etc.)
4. **Limite Configurado**: Número máximo de ações permitidas no intervalo de tempo
5. **Janela de Tempo**: Período sobre o qual o limite se aplica (ex: 10 requisições por minuto)
6. **Ação ao Exceder Limite**: O que acontece quando o limite é ultrapassado (bloquear, throttling, etc.)

### Fluxo de Trabalho Básico

1. **Cliente faz requisição** com identificador (ex: API key, IP, token)
2. **Sistema identifica o cliente** e busca seu contador atual
3. **Sistema verifica se contador está abaixo do limite**
4. **Se dentro do limite**: 
   - Incrementa o contador
   - Permite a requisição prosseguir
   - Retorna headers informativos (opcional)
5. **Se acima do limite**:
   - Não incrementa o contador (ou incrementa dependendo da política)
   - Bloqueia a requisição
   - Retorna erro 429 (Too Many Requests) com headers informativos
   - Pode incluir informações sobre quando tentar novamente

## Algoritmos de Rate Limiting

### 1. Fixed Window Contador (Fixed Window Counter)

- **Como funciona**: Divide o tempo em janelas fixas de tamanho igual ao intervalo de limite. No início de cada janela, o contador é zerado.
- **Exemplo**: Limite de 10 requisições por minuto
  - Janela 1: 12:00:00 - 12:00:59
  - Janela 2: 12:01:00 - 12:01:59
  - Contador resetado em 12:01:00
- **Vantagens**: 
  - Simples de entender e implementar
  - Baixo consumo de memória
  - Bom desempenho
- **Desvantagens**: 
  - Pode permitir até 2x o limite no limite entre janelas (burst no fim de uma janela + início da próxima)
  - Ex: 10 requisições no último segundo da janela 1 + 10 requisições no primeiro segundo da janela 2 = 20 requisições em menos de 2 segundos
- **Quando usar**: Quando simplicidade e performance são mais importantes que precisão estrita

### 2. Sliding Window Log (Sliding Window Log)

- **Como funciona**: Mantém um registro com timestamps de todas as requisições recentes. Conta quantas requisições ocorreram nos últimos intervalo de tempo.
- **Exemplo**: 
  - Limite: 10 requisições por minuto
  - Quando nova requisição chega, remove timestamps mais antigos que 1 minuto e conta os restantes
- **Vantagens**:
  - Muito preciso - não permite bursts além do limite
  - Suave transição entre janelas
- **Desvantagens**:
  - Alto consumo de memória (precisa armazenar todos os timestamps)
  - Complexidade aumentada para limpeza de dados antigos
  - Performance pode degradas com muitos requisições
- **Quando usar**: Quando precisar de controle estrito e tiver recursos para suportar o overhead

### 3. Sliding Window Contador (Sliding Window Counter)

- **Como funciona**: Approximação do sliding window log que combina fixed window com peso baseado na posição atual na janela.
- **Fórmula**: 
  ```
  current_count = (previous_window_count * weight_of_previous_window) + current_window_count
  weight_of_previous_window = (window_size - time_into_current_window) / window_size
  ```
- **Exemplo**: 
  - Limite: 10 requisições por minuto
  - Em 12:00:30 (30 segundos na janela atual):
    - weight_of_previous_window = (60-30)/60 = 0.5
    - current_count = (count_last_minute * 0.5) + count_current_30_seconds
- **Vantagens**:
  - Bom compromisso entre precisão e memória
  - Muito melhor que fixed window para controlar bursts
  - Menor consumo de memória que sliding window log
- **Desvantagens**:
  - Ainda é uma aproximação (não tão preciso quanto sliding window log)
  - Mais complexo de implementar que fixed window
- **Quando usar**: Quando se deseja bom equilíbrio entre precisão e eficiência

### 4. Token Bucket (Balde de Tokens)

- **Como funciona**: 
  - Um balde contém tokens que são adicionados a uma taxa constante (ex: 10 tokens por minuto)
  - Cada requisição consome um token
  - Se o balde estiver vazio, a requisição é rejeitada
  - O balde tem capacidade máxima (burst size)
- **Exemplo**:
  - Capacidade do balde: 5 tokens
  - Taxa de reposição: 10 tokens por minuto
  - Inicialmente balde cheio (5 tokens)
  - Pode fazer 5 requisições imediatamente (burst)
  - Depois, taxa sustentada de 10 requisições por minuto
- **Vantagens**:
  - Permite bursts controlados (tamanho do balde)
  - Muito simples de entender e implementar
  - Bom para modelar limites com tolerância a bursts curtos
- **Desvantagens**:
  - Pode permitir bursts maiores que o esperado se o balde começar cheio
  - Não tão suave quanto alguns outros algoritmos para tráfego muito variável
- **Quando usar**: Quando se quer permitir bursts naturais enquanto limita taxa média (ex: APIs que esperam uso interativo com occasional bursts)

### 5. Leaky BuckeT (Vazamento Constante)

- **Como funciona**:
  - Requisições entram em uma fila (bucket) com tamanho fixo
  - Processamento ocorre em taxa constante (ex: 10 requisições por minuto)
  - Se fila estiver cheia, novas requisições são rejeitadas
  - Se fila não estiver cheia, requisição é adicionada e será processada eventualmente
- **Exemplo**:
  - Tamanho da fila: 5 requisições
  - Taxa de processamento: 10 requisições por minuto
  - Se 5 requisições chegarem simultaneamente, todas são aceitas e serão processadas nos próximos 30 segundos
  - Se uma 6ª chegar enquanto a fila estiver cheia, ela é rejeitada
- **Vantagens**:
  - Suaviza tráfego de entrada (saída em taxa constante)
  - Simples de conceber
- **Desvantagens**:
  - Pode adicionar latência significativa (requisições esperam na fila)
  - Não permite bursts de saída - taxa de saída é rígida
  - Fila pode ficar cheia durante períodos ociosos, rejeitando requisições que poderiam ser processadas imediatamente
- **Quando usar**: Quando se deseja tráfego de saída extremamente estável e previsível (menos comum para APIs públicas)

### 6. Concurrency Limiting (Limitação de Concorrência)

- **Como funciona**: Em vez de limitar por taxa de requisições ao longo do tempo, limita o número de requisições sendo processadas simultaneamente.
- **Exemplo**: Máximo de 10 conexões simultâneas
- **Vantagens**:
  - Protege diretamente contra esgotamento de recursos de conexão ou memória
  - Útil para proteger recursos como pools de conexão de banco de dados
- **Desvantagens**:
  - Não controla taxa de requisições ao longo do tempo (pode ter muitas requisições sequenciais rápidas)
  - Não responde bem a padrões de burst seguidos de ociosidade
- **Quando usar**: Quando o recurso limitante é concorrência em vez de taxa ao longo do tempo

## Headers Padrão para Rate Limiting

Quando uma requisição é limitada ou para informar sobre limites, é comum retornar headers específicos:

### Headers de Informação (em todas as respostas ou respostas de sucesso)

- `X-RateLimit-Limit`: Número máximo de requisições permitidas na janela atual
- `X-RateLimit-Remaining`: Número de requisições restantes na janela atual
- `X-RateLimit-Reset`: Timestamp (geralmente Unix epoch) quando a janela será resetada
- `X-RateLimit-Window`: Tamanho da janela em segundos (opcional)

### Headers em Resposta de Limite Excedido (HTTP 429)

- `Retry-After`: Número de segundos que o cliente deve esperar antes de fazer nova tentativa
  - Pode também ser uma data HTTP
- `X-RateLimit-Limit`: Como acima
- `X-RateLimit-Remaining`: Geralmente 0 quando limitado
- `X-RateLimit-Reset`: Como acima

### Exemplo de Resposta

```
HTTP/1.1 429 Too Many Requests
Content-Type: application/json
X-RateLimit-Limit: 100
X-RateLimit-Remaining: 0
X-RateLimit-Reset: 1625097600
Retry-After: 60

{
  "error": "rate_limit_exceeded",
  "message": "Too many requests, please try again later."
}
```

## Onde Aplicar Rate Limiting

### 1. Edge / CDN
- **Vantagens**: 
  - Bloqueia tráfego ruim antes que chegue à sua infraestrutura
  - Protege contra DDoS e absorve grande volume
  - Distribuído geograficamente
- **Desvantagens**:
  - Menos controle granular sobre políticas específicas por usuário ou serviço
  - Pode ser caro em volume muito alto
- **Quando usar**: Primeira linha de defesa contra abuso em larga escala, proteção de infraestrutura

### 2. API Gateway / Load Balancer
- **Vantagens**:
  - Centralizado, fácil de gerenciar políticas
  - Pode integrar com autenticação para limites por usuário/API key
  - Boa visibilidade e logging
- **Desvantagens**:
  - Pode se tornar gargalo se não escalar adequadamente
  - Adiciona latência adicional
- **Quando usar**: Controle de tráfego entrando no seu sistema, limites por cliente ou serviço

### 3. Serviço de Aplicação (In-process ou Local)
- **Vantagens**:
  - Muito granular - pode aplicar limites diferentes por endpoint, função, ou usuário
  - Baixa latência (se in-process)
  - Fácil de ajustar e testar
- **Desvantagens**:
  - Consome recursos do próprio serviço
  - Difícil de escalar horizontalmente sem armazenamento externo compartilhado
  - Inconsistência entre instâncias se não usar armazenamento compartilhado
- **Quando usar**: Limites específicos de lógica de negócio, proteção de recursos internos caros

### 4. Serviço Dedicatedo / Externo (Redis, Banco de Dados, Serviço Especializado)
- **Vantagens**:
  - Escalável e compartilhado entre múltiplas instâncias de serviço
  - Persistente e sobrevive a reinicializações de serviço
  - Pode ser otimizado para performance alta
- **Desvantagens**:
  - Adiciona dependência externa
  - Latência de rede para acessar armazenamento
  - Complexidade operacional adicional
- **Quando usar**: Sistemas escaláveis onde múltiplas instâncias precisam compartilhar estado de rate limiting

## Estratégias para Sistemas Escaláveis e Distribuídos

### 1. Armazenamento Centralizado (Redis, Memcached, etc.)
- **Como funciona**: Todas as instâncias de serviço leem e escrevem contadores em um armazenamento central
- **Considerações**:
  - Escolher armazenamento com operações atômicas (INCR, EXPIRE em Redis)
  - Lidar com falhas de armazenamento (failover, replicação)
  - Considerar latência de rede
- **Exemplo com Redis**:
  ```
  KEY = rate_limit:{identifier}:{endpoint}
  INCR KEY
  EXPIRE KEY window_size  # Define/expira a chave
  ```
- **Vantagens**: Simples, consistente, bom desempenho com rede rápida
- **Desvantagens**: Ponto único de falha (mitigado com clustering), latência de rede

### 2. Algoritmos Aproximados com Estruturas Probabilísticas
- **Como funciona**: Usar estruturas como Count-Min Sketch ou HyperLogLog para estimar contagens com menor memória
- **Vantagens**: Muito eficiente em memória, bom para cardinalidade alta
- **Desvantagens**: Aproximação (pode ter falsos positivos/negativos), mais complexo
- **Quando usar**: Quando se precisa lidar com milhões de identificadores diferentes e memória é limitada

### 3. Sharding por Identificador
- **Como funciona**: Distribuir identificadores entre múltiplos armazenamentos baseado em hash (ex: hash(identifier) % N shards)
- **Vantagens**: Escalabilidade horizontal, reduz carga por shard
- **Desvantagens**: Complexidade aumentada, rehashing necessário ao mudar número de shards
- **Quando usar**: Sistemas com muito alto volume de identificadores únicos

### 4. Janela Deslizante com Estrutura de Dados Eficiente
- **Como funciona**: Usar estruturas como circular buffers ou queues com timestamps para manter apenas dados relevantes
- **Vantagens**: Mais preciso que fixed window com menor memória que sliding window log puro
- **Desvantagens**: Ainda requer limpeza periódica, mais complexo de implementar
- **Quando usar**: Quando se quer bom equilíbrio entre precisão e recursos

### 5. Taxa Adaptativa Baseada em Load
- **Como funciona**: Ajustar limites dinamicamente baseado em carga atual do sistema (ex: aumentar limite quando sistema está ocioso, diminuir quando sobrecarregado)
- **Vantagens**: Melhor utilização de recursos, responde a condições reais
- **Desvantagens**: Mais complexo, pode confundir clientes se limites mudam frequentemente
- **Quando usar**: Sistemas onde se deseja maximizar utilização sem sobrecarregar

## Tratamento de Clientes quando Limitado

### 1. Hard Block (Bloqueio Direto)
- **Como funciona**: Retornar imediatamente erro 429 quando limite excedido
- **Vantagens**: Simples, protege recursos imediatamente
- **Desvantagens**: Pode causar experiência ruim se cliente não esperar adequadamente
- **Quando usar**: Quando se quer proteção imediata e forte

### 2. Throttling (Atraso)
- **Como funciona**: Em vez de bloquear, atrasar a resposta por um período proporcional ao excesso
- **Exemplo**: Se limite é 10/segundo e cliente fez 15, atrasar 500ms
- **Vantagens**: Mais suave, reduz possibilidade de thunder herd de retentativas
- **Desvantagens**: Aumenta latência para clientes que estão próximos do limite
- **Quando usar**: Quando se quer uma abordagem mais gentil para controle de tráfego

### 3. Servir Resposta Cacheada ou de Fallback
- **Como funciona**: Quando limitado, servir resposta cacheada antiga ou página de manutenção
- **Vantagens**: Mantém algum nível de serviço mesmo durante limite
- **Desvantagens**: Pode servir informação desatualizada, complexidade de implementação
- **Quando usar**: Quando se tem dados razoavelmente cacheáveis e estar ligeiramente desatualizado é aceitável

### 4. Priorização ou Classe de Serviço
- **Como funciona**: Diferentes tipos de usuários ou requisições têm diferentes limites ou prioridades
- **Exemplo**: Usuários premium têm limite mais alto, requisições críticas têm fila separada
- **Vantagens**: Permite diferenciar tratamento baseado em valor ou importância
- **Desvantagens**: Complexidade aumentada na definição e gerenciamento de classes
- **Quando usar**: Quando se tem clara hierarquia de usuários ou tipos de requisição

## Considerações de Implementação

### 1. Escolha do Identificador
- **IP Address**: Simples, mas pode ser compartilhado (NAT, proxies) ou mudar frequentemente
- **API Key / Token**: Bom para usuários autenticados, requer gerenciamento de chaves
- **User ID**: Precisa de autenticação, bom para limites pessoais
- **Combination**: Ex: API key + endpoint para limites mais granulares
- **Device ID / Cookie**: Para limitação por dispositivo ou navegador (menos confiável)

### 2. Granularidade do Limite
- **Global**: Mesmo limite para todo o serviço
- **Por Endpoint**: Limites diferentes para diferentes APIs (ex: mais permissivo para leitura, restritivo para escrita)
- **Por Tipo de Operação**: Limites baseado em custos computacionais (ex: busca complexa vs leitura simples)
- **Por Recurso**: Limites por usuário específico, tenant, ou grupo

### 3. Janela de Tempo
- **Curta** (segundos): Bom para proteger contra bursts imediatos
- **Média** (minutos): Equilibrio comum para APIs
- **Longa** (horas/dias): Para limites de uso diário ou mensal (ex: 1000 requisições por dia)

### 4. Tratamento de Cabeçalhos
- **Consistência**: Sempre retornar headers informativos mesmo quando não limitado
- **Precisão**: Garantir que os valores retornados sejam precisos e atualizados
- **Timezones**: Usar timestamps Unix epoch para evitar confusão de fuso horário
- **Segurança**: Não vazar informações sensíveis nos headers

### 5. Testes e Validação
- **Testes de Carga**: Simular tráfego em taxas acima, igual, e abaixo do limite
- **Testes de Burst**: Verificar comportamento com tráfego repentino alto
- **Testes de Recuperação**: Verificar que limites são resetados corretamente após janela
- **Testes de Distribuído**: Em sistemas com múltiplas instâncias, verificar consistência
- **Testes de Falha**: Verificar comportamento quando armazenamento de rate limiting está indisponível

## Perguntas de Entrevista Comuns

### Básicas
- "O que é rate limiting e por que ele é usado?"
- "Quais são as diferenças entre fixed window e sliding window rate limiting?"
- "Explique como funciona o algoritmo token bucket."

### Intermediárias
- "Como você implementaria rate limiting em um sistema escalável com múltiplas instâncias de serviço?"
- "Quais headers você retornaria em uma resposta de rate limiting e por quê?"
- "Como você lidaria com o problema de distribuir contadores de rate limiting em um cluster?"
- "Quais são as trade-offs entre diferentes algoritmos de rate limiting?"

### Avançadas
- "Como você projetaria um sistema de rate limiting que se adapte dinamicamente à carga do sistema?"
- "Discuta estratégias para rate limiting em arquiteturas de microserviços onde serviços chamam outros serviços."
- "Como você lidaria com rate limiting para usuários autenticados versus anônimos de forma justa?"
- "Explique como você implementaria um sistema de rate limiting que suporte diferentes limites por plano de serviço (free, premium, enterprise)."

### Follow-ups Típicos
- "E se precisássemos mudar nossa estratégia de rate limiting após o sistema estar em produção?"
- "Como você validaria que seu rate limiting está funcionando corretamente sob carga real?"
- "Qual seria sua estratégia para migrar de nenhum rate limiting para um sistema distribuído sem downtime?"
- "E se descobríssemos que nosso padrão de acesso tem características que tornam certos algoritmos ineficazes ou muito conservadores?"

## Checklist de Implementação de Rate Limiting

### Antes de Começar a Implementação
- [ ] Definir requisitos de limite (quantidade, janela de tempo, granularidade)
- [ ] Escolher identificador de cliente apropriado (IP, API key, user ID, etc.)
- [ ] Selecionar algoritmo de rate limiting baseado em trade-offs entre precisão, memória, e performance
- [ ] Determinar onde aplicar rate limiting (edge, gateway, serviço, armazenamento dedicado)
- [ ] Planejar estratégia de armazenamento e consistência para sistemas distribuídos
- [ ] Definir headers de resposta a serem retornados
- [ ] Planejar ação a ser tomada quando limite é excedido (429, throttling, etc.)
- [ ] Orçar recursos necessários (memória, armazenamento, largura de banda de rede)
- [ ] Planejar estratégia de teste e validação sob carga realista

### Durante a Implementação
- [ ] Implementar lógica de identificação do cliente
- [ ] Implementar algoritmo de rate limiting escolhido
- [ ] Configurar armazenamento de contadores (memória local, Redis, etc.)
- [ ] Implementar headers de resposta informativos (Limit, Remaining, Reset, etc.)
- [ ] Implementar resposta apropriada quando limite excedido (status 429, headers, corpo)
- [ ] Adicionar logging e métricas (contagem de requisições permitidas/bloqueadas, taxa de throttling)
- [ ] Implementar mecanismos de limpeza ou expiração de dados antigos (se aplicável)
- [ ] Testar extensivamente em ambiente de staging com diversos padrões de tráfego
- [ ] Validar comportamento em cenários de falha (armazenamento indisponível, rede lenta, etc.)

### Depois da Implementação e em Produção
- [ ] Monitorar métricas de taxa de requisições permitidas vs bloqueadas
- [ ] Alertar sobre aumentos súbitos em requisições bloqueadas (possível ataque ou bug)
- [ ] Rastrear eficácia do algoritmo escolhido (ajustar se muito conservador ou permissivo)
- [ ] Validar que janelas de tempo estão sendo resetadas corretamente
- [ ] Revisar periodicamente se limites ainda estão adequados ao padrões de uso observado
- [ ] Manter e atualizar documentação de limites para consumidores da API
- [ ] Aplicar patches de segurança e atualizações em dependências de armazenamento
- [ ] Coletar feedback de usuários legítimos que podem estar sendo afetados por limites muito baixos
- [ ] Planejar ajustes de limites baseado em crescimento observado e mudanças de padrão de uso

## RESUMO

Rate limiting é uma técnica essencial para proteger serviços, garantir uso justo de recursos, e melhorar resiliência contra abuso e sobrecarga:

**Princípios-chave:**
1. Rate limiting controla a taxa de requisições ou ações por cliente dentro de intervalos de tempo definidos
2. Diferentes algoritmos (fixed window, sliding window, token bucket, leaky bucket) oferecem trade-offs entre precisão, memória, e performance
3. A escolha de onde aplicar rate limiting (edge, gateway, serviço, armazenamento dedicado) afeta escalabilidade, latência, e granularidade de controle
4. Em sistemas distribuídos, armazenamento compartilhado e consistente é essencial para limites corretos entre múltiplas instâncias
5. Headers informativos e tratamento adequado de clientes limitados melhoram experiência e integração
6. Monitoramento, testes, e ajuste contínuo são necessários para manter eficácia conforme padrões de uso evoluem
- [ ] Lembre-se: Um sistema de rate limiting eficaz não é apenas sobre bloquear requisições em excesso - é sobre entender profundamente seus padrões de tráfego, características dos usuários, e requisitos de negócio para projetar uma solução que proteja seus serviços enquanto proporciona uma experiência justa e previsível para clientes legítimos.

