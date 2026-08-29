---
trilha: "PARA ENTREVISTAS"
---
**Navegação:** [[MOC — TRILHA PARA ENTREVISTAS]]
← [[PARTE 75 — CHEAT SHEETS]] | #trilha/entrevistas | [[PARTE 77 — GLOSSÁRIO]] →

---
# PARTE 76 — GLOSSÁRIO

## Fundamentos

Este glossário fornece definições claras e concisas para termos-chave em arquitetura de software, padrões de design, conceitos de sistemas distribuídos e tecnologias relacionadas. Ter um vocabulário compartilhado é essencial para comunicação eficaz entre arquitetos, desenvolvedores, gerentes de projeto e stakeholders de negócio.

As definições aqui apresentadas seguem padrões da indústria quando aplicáveis e são formuladas para serem tanto precisas quanto acessíveis a profissionais em diferentes níveis de experiência.

> **Nota**: Este glossário é um documento vivo. À medida que novas tecnologias e práticas emergem, ele deve ser atualizado para permanecer relevante e útil.

## A

### API (Application Programming Interface)
Um conjunto de definições e protocolos para construção e integração de aplicações de software. Uma API especifica como os componentes de software devem interagir, incluindo quais tipos de chamadas podem ser feitos, como fazer essas chamadas, quais formatos de dados usar e quais convenções seguir.

### ACID
Acronimo para Atomicidade, Consistência, Isolamento e Durabilidade. Um conjunto de propriedades que garantem que transações de banco de dados sejam processadas de forma confiável, mesmo em caso de erros, falhas de energia ou outras interrupções.

### Arquitetura em Camadas (Layered Architecture)
Um estilo arquitetural onde os componentes são organizados em camadas horizontais, cada uma com uma responsabilidade específica. As camadas típicas incluem apresentação, aplicação, negócio e persistência, com dependências fluindo de cima para baixo.

### Arquitetura de Microserviços
Um estilo arquitetural que estrutura uma aplicação como uma coleção de serviços pequenos, autônomos e fracamente acoplados. Cada serviço implementa uma capacidade de negócio específica e pode ser desenvolvido, implantado e escalado independentemente.

### Arquitetura Orientada a Eventos (Event-Driven Architecture - EDA)
Um estilo arquitetural onde a produção, detecção, consumo e reação a eventos são o principal mecanismo de estruturação da aplicação. Componentes se comunicam através de eventos produzidos e consumidos de forma assíncrona.

### Alta Disponibilidade
A capacidade de um sistema ou componente de permanecer operacional e acessível por um alto percentual de tempo, geralmente medido em "noves" (por exemplo, 99,9% de disponibilidade = "três noves").

## B

### Balanceador de Carga (Load Balancer)
Um dispositivo ou software que distribui o tráfego de rede ou de aplicações entre múltiplos servidores para garantir que nenhum único servidor fique sobrecarregado, melhorando assim a capacidade e a confiabilidade das aplicações.

### Banco de Dados NoSQL
Bancos de dados não relacionais que fornecem mecanismos para armazenamento e recuperação de dados modelados de maneiras diferentes das relações tabulares usadas em bancos de dados relacionais. Tipos incluem documento, chave-valor, wide-column e grafos.

### Banco de Dados Relacional (RDBMS)
Um banco de dados baseado no modelo relacional de dados, onde os dados são estruturados em tabelas (relações) composto por linhas e colunas, com relacionamentos estabelecidos através de chaves primárias e estrangeiras.

### Broker de Mensagens
Um intermediário de software que traduz mensagens do protocolo de mensagens do remetente para o protocolo de mensagens do destinatário. Permite comunicação assíncrona e desacoplada entre aplicações ou componentes de sistema.

## C

### Cache
Camada de armazenamento de dados que armazena cópias de dados frequentemente acessados para reduzir o tempo de acesso e a carga no sistema de armazenamento primário. Pode ser local (in-process) ou distribuído.

### CAP Theorem
Teorema que afirma que em um sistema de dados distribuído, é possível garantir apenas duas das três propriedades seguintes simultaneamente: Consistência, Disponibilidade e Tolerância à Partição.

### Caching em Camadas Múltiplas (Multilevel Cache)
Estratégia que combina diferentes tipos de cache (por exemplo, cache local rápido e cache distribuído maior) para obter o melhor desempenho possível, equilibrando latência baixa e capacidade maior.

### Circuit Breaker
Padrão de design que detecta falhas e encapsula a lógica de prevenção de propagação de falhas em um sistema distribuído. Funciona como um disjuntor elétrico, abrindo o circuito quando as falhas ultrapassam um limiar e fechando-o após um tempo de recuperação.

### Consistência Eventual
Modelo de consistência onde, se nenhuma nova atualização for feita em um dado eventualmente, todas as leituras retornarão o último valor atualizado. Não garante quando a consistência será alcançada, apenas que eventualmente será.

### Containers
Unidades leves e portáteis de software que empacotam o código e todas as suas dependências para que o aplicativo seja executado de forma rápida e confiável de um ambiente de computação para outro. Docker é a plataforma de container mais popular.

### CDN (Content Delivery Network)
Rede geograficamente distribuída de servidores proxy e seus data centers. O objetivo é distribuir o serviço espacialmente em relação aos usuários finais para fornecer alta disponibilidade e alta performance ao distribuir conteúdo espacialmente.

## D

### Desnormalização
Processo de tentar otimizar o desempenho de leitura de um banco de dados adicionando dados redundantes ou agrupando dados. É o oposto da normalização e geralmente envolve a duplicação de dados em múltiplas tabelas para evitar joins caros.

### Desenvolvimento Orientado a Testes (TDD - Test-Driven Development)
Metodologia de desenvolvimento de software onde os testes são escritos antes do código que deve passar neles. O ciclo é: escrever um teste que falha, escrever o mínimo de código para fazer o teste passar, e refatorar o código.

### Deploy Azul/Verde (Blue/Green Deployment)
Técnica de lançamento de software onde dois ambientes de produção idênticos são mantidos: um (azul) está atualmente em uso, enquanto o outro (verde) está ocioso. Uma nova versão é implantada no ambiente ocioso e, após teste, o tráfego é switches para ele.

### Design por Contrato
Metodologia de desenvolvimento de software onde os desenvolvedores definem interfaces de software com especificações formais, precondições, postcondições e invariantes. O termo foi popularizado por Bertrand Meyer no contexto da linguagem Eiffel.

### Debouncing
Técnica de programação usada para garantir que funções acionadas por eventos não sejam executadas repetidamente em rápida sucessão. Em vez disso, a função é executada apenas depois de um certo período de tempo ter se passado desde a última vez que foi chamada.

## E

### Escalabilidade Horizontal (Scale Out)
Adicionando mais máquinas ou nós a um sistema para lidar com aumento de carga. Contrasta com escalabilidade vertical (scale up), que envolve adicionar mais recursos (CPU, RAM) a uma máquina existente.

### Escalabilidade Vertical (Scale Up)
Aumentando os recursos de um único nó no sistema (como CPU, memória ou armazenamento) para lidar com aumento de carga. Contrasta com escalabilidade horizontal, que adiciona mais nós ao sistema.

### Event Sourcing
Padrão de design onde as mudanças no estado de uma aplicação são armazenadas como uma sequência de eventos. Em vez de armazenar apenas o estado atual, cada mudança de estado é persistida como um evento, permitindo reconstruir o estado em qualquer momento no passado.

### Exponencial Backoff
Estratégia onde o tempo entre tentativas repetidas aumenta exponencialmente após cada falha. Geralmente combinado com jitter para evitar que muitas tentativas ocorram ao mesmo tempo (thundering herd).

## F

### Failover
Capacidade de um sistema de mudar automaticamente para um componente ou sistema de reserva quando detecta uma falha ou anomalia no componente ativo. É um aspecto crítico de sistemas de alta disponibilidade.

### Ferramenta de Infraestrutura como Código (IaC)
Ferramenta que permite gerenciar e provisionar infraestrutura de computação através de arquivos de definição legíveis por máquina, em vez de configuração de hardware interativa ou ferramentas de configuração de linha de comando. Exemplos incluem Terraform, AWS CloudFormation e Ansible.

### Fila de Mensagens
Estrutura de dados que segue o princípio FIFO (First-In, First-Out) onde elementos são adicionados ao final e removidos do início. No contexto de sistemas distribuídos, refere-se a mecanismos de comunicação assíncrona onde mensagens são armazenadas até que possam ser processadas.

### Framing (Enquadramento)
No contexto de protocolos de comunicação, refere-se à maneira como os dados são estruturados em unidades de transmissão (frames) para que o remetente e o destinatário possam identificar onde uma mensagem começa e termina.

## G

### Gatekeeper
Padrão arquitetural onde um componente central controla o acesso a recursos ou serviços. Pode ser usado para autenticação, autorização, rate limiting ou outras funções de controle.

### Garbage Collection (Coleta de Lixo)
Forma automática de gerenciamento de memória onde o ambiente de execução identifica e liberta memória que não está mais sendo usada pelo programa. Comum em linguagens como Java, C#, Go e Python.

### GitOps
Conjunto de práticas que usa o Git como fonte única da verdade para declarações de infraestrutura e aplicações. Usando o Git pull requests para gerenciar alterações na infraestrutura e nas aplicações.

### Grafana
Plataforma de código aberto para monitoramento e observabilidade. Permite criar, explorar e compartilhar painéis de dados e é frequentemente usada junto com fontes de dados como Prometheus, Elasticsearch e InfluxDB.

## H

### High Availability (HA)
Capacidade de um sistema ou componente de permanecer operacional e acessível por um alto percentual de tempo, geralmente medido em "noves" (por exemplo, 99,99% de disponibilidade = "quatro noves").

### Heartbeat (Batimento Cardíaco)
Sinal periódico enviado entre componentes de um sistema para indicar que estão operacionais. Se um componente parar de receber heartbeats de outro dentro de um certo período, ele pode assumir que o outro componente falhou.

### Hexagonal Architecture (Ports and Adapters)
Estilo arquitetural também conhecido como arquitetura de portas e adaptadores. O núcleo da aplicação é independente de preocupações externas como bancos de dados, interfaces de usuário ou sistemas externos, que se conectam através de portas e adaptadores.

### Hot Standby
Configuração onde um sistema secundário está em execução simultaneamente com o primário, replicando seus dados em tempo real, pronto para assumir imediatamente caso o primário falhe.

## I

### Idempotência
Propriedade de certas operações em matemática e ciência da computação onde elas podem ser aplicadas múltiplas vezes sem mudar o resultado além da aplicação inicial. Importante em sistemas distribuídos para tratar duplicate requests.

### Imutabilidade
Propriedade de um objeto cujo estado não pode ser modificado após sua criação. Objetos imutáveis são inerentemente seguros para uso em concorrência poiché não podem ser alterados por múltiplas threads simultaneamente.

### Infraestrutura como Código (IaC)
Prática de gerenciar e provisionar infraestrutura de computação através de arquivos de definição legíveis por máquina, em vez de configuração de hardware interativa ou ferramentas de configuração de linha de comando.

### Injeção de Dependência
Técnica de design de software na qual um objeto recebe outras objetos dos quais depende. Esses outros objetos são chamados de dependências. No sentido típico, o objeto recebido é chamado de serviço e o objeto que o recebe é chamado de cliente.

### Integração Contínua (CI)
Prática de desenvolvimento de software onde os desenvolvedores integram código em um repositório compartilhado várias vezes ao dia. Cada integração é verificada por um build automatizado para detectar erros de integração o mais rápido possível.

## J

### Jitter
Variação aleatória introduzida em um intervalo de tempo para evitar que múltiplos eventos ocorram simultaneamente. Comumente usado em algoritmos de backoff para evitar o problema do "thundering herd".

### JSON (JavaScript Object Notation)
Formato leve de troca de dados que é fácil para humanos lerem e escreverem e fácil para máquinas serem analisadas e geradas. Baseado em um subconjunto da linguagem de programação JavaScript.

### JVM (Java Virtual Machine)
Máquina virtual que permite que um computador execute programas Java bem como aqueles escritos em outras linguagens que também são compilados para bytecode Java. O JVM fornece um ambiente de execução que traduz bytecode Java para ações específicas do sistema operacional.

## K

### Kafka
Plataforma de streaming de eventos distribuída originalmente desenvolvida pela LinkedIn e doada à Apache Software Foundation. Usada para construir pipelines de dados em tempo real e aplicações de streaming.

### Kubernetes
Sistema de código aberto para automatizar a implantação, o dimensionamento e o gerenciamento de aplicações em contêineres. Originalmente projetado pelo Google e agora mantido pela Cloud Native Computing Foundation.

### Key-Value Store
Tipo de banco de dados NoSQL que armazena dados como uma coleção de pares chave-valor. É simples, rápido e altamente escalável, adequado para caching, armazenamento de sessões e outros usos onde o acesso primário é por chave.

## L

### Latência
Tempo decorrido entre a origem de um pacote de dados e seu destino. Em sistemas de computação, refere-se ao atraso antes de uma transferência de dados começar após uma instrução para sua transferência.

### Load Shedding
Estratégia onde um sistema rejeita intencionalmente algumas solicitações quando sobrecarregado para evitar falha total e manter o serviço para solicitações críticas. Pode ser baseado em prioridade, aleatório ou outros critérios.

### Log Aggregation
Prática de coletar logs de múltiplos fontes e sistemas em um local central para armazenamento, indexação e análise. Essencial para observabilidade em sistemas distribuídos.

### LRU (Least Recently Used)
Política de substituição de cache onde o item que não foi acessado há mais tempo é o primeiro a ser removido quando o cache está cheio e precisa fazer espaço para novos itens.

## M

### Microserviços
Estilo arquitetural que estrutura uma aplicação como uma coleção de serviços pequenos, autônomos e fracamente acoplados. Cada serviço implementa uma capacidade de negócio específica e pode ser desenvolvido, implantado e escalado independentemente.

### Middleware
Software que fica entre um sistema operacional e as aplicações executadas nele. No contexto de aplicações web, refere-se a funções que têm acesso ao objeto de solicitação e ao objeto de resposta.

### Monolitico
Estilo arquitetural onde uma aplicação é construída como uma única unidade indivisível. Normalmente inclui interface de usuário e código de acesso a dados em um único programa único de uma única plataforma.

### Message Queue
Forma assíncrona de comunicação de serviço a serviço utilizada em arquiteturas de computação sem servidor e em microserviços. As mensagens são entregues mediante fila e fornecem armazenamento temporário quando o destino está ocupado ou inativo.

### Migração de Banco de Dados
Processo de mover dados de um ou mais bancos de dados fonte para um ou mais bancos de dados destino usando um conjunto de procedimentos para garantir que os dados sejam transferidos com precisão e completude.

## N

### NATS
Sistema de mensagens de código alto desempenho para microsserviços, IoT e sistemas de computação em nuvem. Funciona como um sistema de mensagens leve e de alto desempenho.

### Normalização
Processo de organizar os campos e tabelas de um banco de dados relacional para minimizar a redundância e melhorar a integridade dos dados. Geralmente envolve dividir grandes tabelas em tabelas menores e menos redundantes e definir relacionamentos entre elas.

### Nó (Node)
Em computação distribuída, refere-se a uma única máquina física ou virtual que faz parte de um cluster maior. Em estruturas de dados, refere-se a um elemento individual em uma lista encadeada, árvore ou grafo.

### Não Blocking (Não Bloqueante)
Descrição de operações que não bloqueiam a execução de outros processos enquanto aguardam sua conclusão. Em contrapartida às operações bloqueantes, que impedem a execução adicional até que elas terminem.

## O

### Observabilidade
Medida de quão bem os estados internos de um sistema podem ser inferidos a partir de seus conhecidos externos. Em sistemas de software, refere-se à capacidade de entender o que está acontecendo dentro de um sistema com base em suas saídas (logs, métricas, traces).

### Orquestração de Containers
Automatização do provisionamento, implantação, escalonamento, gerenciamento de rede e disponibilidade de contêineres. Plataformas como Kubernetes fornecem orquestração de containers.

### OpenAPI Specification (OAS)
Formato de descrição de definição de interface para APIs RESTful que é independente de linguagem. Permite que humanos e computadores descubram e compreendam as capacidades de um serviço sem acesso ao código-fonte, documentação adicional ou inspeção de tráfego de rede.

### ORM (Object-Relational Mapping)
Técnica de programação para converter dados entre sistemas de tipo incompatíveis usando linguagens de programação orientadas a objetos. Cria um "banco de dados virtual de objeto" que pode ser usado dentro da linguagem de programação.

### Otimização Prematura
Ato de otimizar algo antes de saber se realmente precisa ser otimizado. Geralmente considerado um antipadrão porque pode levar a código mais complexo sem benefício de desempenho proporcional.

## P

### Particionamento (Sharding)
Método de distribuir dados em múltiplas máquinas. Um banco de dados particionado ou particionado horizontalmente é um banco de dados cujo esquema é particionado ou dividido em múltiplas tabelas distintas, cada uma armazenada em uma estrutura de banco de dados separada.

### Plataforma como Serviço (PaaS)
Categoria de serviços de computação em nuvem que fornece uma plataforma permitindo que os clientes desenvolvam, executem e gerenciem aplicações sem a complexidade de construir e manter a infraestrutura normalmente associada ao desenvolvimento e lançamento de um aplicativo.

### POLA (Principle of Least Astonishment)
Princípio de design de software e ergonomia que afirma que um componente de um sistema deve se comportar de maneira que os usuários esperem; ele não deve surpreendê-los.

### Polling
Técnica onde um cliente verifica repetidamente o status de um recurso ou condição. Pode ser ineficiente se feito com muita frequência, mas útil quando webhooks ou outras formas de notificação push não estão disponíveis.

### Propriedade ACID
Conjunto de propriedades de transações de banco de dados que garante que as transações sejam processadas de forma confiável. ACID significa Atomicidade, Consistência, Isolamento e Durabilidade.

### Pub/Sub (Publish/Subscribe)
Padrão de mensagens onde os remetentes (publicadores) de mensagens não programam as mensagens diretamente para serem recebidas por destinatários específicos (assinantes). Em vez disso, as mensagens são publicadas sem saber quem, se houver, vai recebê-las.

## Q

### Qualidade de Serviço (QoS)
Medida do desempenho geral de um serviço, como telefone ou rede de computadores ou computação em nuvem, particularmente o desempenho visto pelos usuários da rede. Pode incluir fatores como disponibilidade, latência e taxa de erro.

### Query Language
Linguagem de programação usada para fazer consultas em bancos de dados e sistemas de informação. Exemplos incluem SQL (Structured Query Language) para bancos de dados relacionais e várias linguagens para bancos de dados NoSQL.

### Quorum
Número mínimo de membros de um grupo necessário para estar presente para conduzir negócios do grupo. Em sistemas distribuídos, frequentemente usado em algoritmos de consenso para garantir que decisões sejam tomadas apenas quando um número suficiente de nós concordam.

## R

### Rate Limiting
Técnica para controlar a taxa de tráfego enviada ou recebida por uma interface de rede. Geralmente usado para prevenir abuso de APIs, proteger recursos de sobrecarga e garantir uso justo entre múltiplos consumidores.

### Replication
Processo de compartilhamento de informações para garantir consistência entre recursos redundantes, como software ou componentes de hardware, para melhorar a confiabilidade, tolerância a falhas ou acessibilidade.

### REST (Representational State Transfer)
Estilo arquitetural para sistemas distribuídos hipermídia que depende de um protocolo stateless, geralmente HTTP, para interagir com recursos conhecidos pelo identificador uniforme de recurso (URI).

### Resiliência
Capacidade de um sistema de se recuperar de falhas e continuar funcionando. Não é apenas sobre evitar falhas, mas também sobre como o sistema responde quando as falhas ocorrem.

### Reversibilidade
Propriedade de uma decisão arquitetural que indica quão fácil ou difícil seria mudar essa decisão no futuro. Decisões altamente reversíveis têm baixo custo de mudança, enquanto decisões irreversíveis têm alto custo de mudança.

### RPC (Remote Procedure Call)
Protocolo que um programa de computador pode usar para solicitar um serviço de um programa localizado em outro computador em uma rede sem precisar entender os detalhes da rede.

## S

### SaaS (Software como Serviço)
Modelo de licenciamento e entrega de software no qual o software é licenciado por assinatura e está hospedado centralmente. Também é conhecido como "software sob demanda".

### Sagas
Padrão para gerenciar transações distribuídas. Uma saga é uma sequência de transações locais onde cada transação atualiza o banco de dados e publica uma mensagem ou evento para acionar a próxima transação local na saga.

### Escalabilidade
Capacidade de um sistema, processo ou rede de lidar com crescentes quantidades de trabalho ou seu potencial para ser ampliado para acomodar esse crescimento. Pode ser medido em termos de tamanho, volume ou número de usuários.

### Segurança de Cadeia de Suprimentos
Esforços para garantir a segurança de cada componente da cadeia de suprimentos de software, desde o código-fonte até o produto entregue. Inclui práticas como verificação de dependências, build seguro e distribuição segura.

### Service Mesh
Camada de infraestrutura dedicada para facilitar a comunicação entre serviços. Implementado como um array de proxies de rede lado a lado junto com lógica de gerenciamento, configuração e telemetria.

### Sharding
Método de particionar dados horizontalmente em múltiplas máquinas. Também conhecido como particionamento horizontal, envolve a divisão de um conjunto de dados em partes menores e mais gerenciáveis chamados fragmentos (shards).

### Single Source of Truth (SSOT)
Prática de estruturar modelos de informação e esquemas de dados associados de modo que cada elemento de dado seja armazenado exatamente uma vez. Qualquer ligação a esse dado é feita por referência.

### SOA (Arquitetura Orientada a Serviços)
Estilo arquitetural no qual os componentes de fornecimento de serviços são fornecidos aos outros componentes por meio de um protocolo de comunicação sobre uma rede. Os princípios de SOA são independentes de qualquer produto, fornecedor ou tecnologia.

### Stateless
Propriedade de um protocolo, aplicação ou serviço onde nenhuma informação de sessão é retida pelo remetente ou receptor. Cada solicitação de comunicação é tratada como uma transação independente, não relacionada a solicitações anteriores.

### Stream Processing
Paradigma de processamento de dados onde operações são executadas em fluxos de dados em tempo real. Os dados fluem continuamente de uma fonte para um destino enquanto são processados por várias operações.

## T

### Taxa de Transferência (Throughput)
Quantidade de material ou itens que passam por um sistema ou processo. Em redes de computadores, refere-se à quantidade de dados transferidos de um lugar para outro em um determinado período de tempo.

### Técnica do Estrangulador (Strangler Fig Padrão)
Estratégia para migrar um sistema legião substituindo gradualmente funcionalidades específicas por novos serviços e aplicações. À medida que a nova substituta assume cada vez mais funcionalidade, o sistema legião eventualmente é estrangulado e deixa de existir.

### Tela de Divisão (Split Brain)
Situação em um cluster de computação onde o cluster é dividido em duas ou mais partes que não conseguem se comunicar entre si, e cada parte acredita ser a única parte legítima do cluster.

### Tolerância a Falhas
Capacidade de um sistema de continuar operando adequadamente mesmo na ocorrência de falhas (esperadas ou inesperadas) em alguns de seus componentes. É uma propriedade particularmente importante de sistemas de alta disponibilidade e sistemas críticos para missões.

### Topologia
Arranjo dos vários elementos (links, nós, etc.) de uma rede de computadores. Essencialmente, é a estrutura topológica de uma rede e pode ser representado fisicamente ou logicamente.

### TTL (Time To Live)
Mecanismo que limita a vida útil ou a permanência de dados em um computador ou rede. Pode ser implementado como um contador ou timestamp anexado ou embutido nos dados. Quando o TTL atingido, os dados são descartados.

## U

### UI (Interface do Usuário)
Espaço onde as interações entre humanos e máquinas ocorrem. O objetivo de uma interface do usuário efetiva é tornar a experiência do usuário fácil e eficiente, significando que o usuário consegue alcançar o resultado desejado com o mínimo de esforço.

### Uptime
Quantidade de tempo que uma máquina, geralmente um computador, tem estado ligado e disponível. É o oposto do downtime e frequentemente usado como medida de confiabilidade ou estabilidade.

### URL (Uniform Resource Locator)
Referência a um recurso da web que especifica sua localização na rede de computadores e um mecanismo para recuperá-lo. Um tipo específico de Identificador de Recurso Uniforme (URI), embora muitos termos técnicos e protocolos usem os dois termos de forma intercambiável.

### Usabilidade
Facilidade de acesso e/ou uso de um produto ou serviço. Um produto com alta usabilidade é fácil de aprender, eficiente de usar, agradável ao interagir com ele e tolera erros do usuário.

## V

### Virtualização
Criação de uma versão virtual (em vez de real) de algo, como uma plataforma de hardware, sistema operacional, dispositivo de armazenamento ou recurso de rede. A versão virtual se comporta como se fosse a versão real.

### VPC (Virtual Private Cloud)
Rede lógica isolada dentro de uma nuvem pública dedicada a uma conta do cliente. O cliente lançará recursos da AWS, como instâncias do Amazon EC2, em sua VPC.

### Versionamento Semântico (SemVer)
Esquema de versionamento que consiste em três números: MAJOR.MINOR.PATCH. Incrementa o MAJOR quando fizer mudanças incompatíveis com a API, o MINOR quando adicionar funcionalidade de forma compatível com a versão anterior e o PATCH quando fizer correções de bugs compatíveis com a versão anterior.

### VM (Máquina Virtual)
Emulação de um sistema de computação. Máquinas virtuais são baseadas em arquiteturas de computador e fornecem a funcionalidade de um computador físico. Suas implementações podem envolver hardware especializado, software ou uma combinação.

## W

### Webhook
Método de augmentar ou alterar o comportamento de uma página da web ou aplicação web com callbacks personalizados. Esses callbacks podem ser mantidos, modificados e gerenciados por terceiros usuários e desenvolvedores que talvez não estejam afiliados ao site de origem originalmente.

### Worker Node
Em um cluster de computação (como Kubernetes), um nó de trabalho é uma máquina virtual ou física que executa as aplicações e cargas de trabalho reais. Contrasta com o nó de controle (master node) que gerencia o cluster.

### Workload
Quantidade de trabalho que um sistema ou componente foi projetado para realizar ou está atualmente realizando. Em computação em nuvem, refere-se à quantidade de recursos de computação que uma aplicação está usando.

### WAL (Write-Ahead Logging)
Família de técnicas para fornecer atomicidade e durabilidade entre duas ou mais operações de armazenamento. Em sistemas de banco de dados, o WAL é um conjunto de regras que garante que as atualizações de dados ocorram na ordem correta.

## X

### XML (eXtensible Markup Language)
Linguagem de marcação que define um conjunto de regras para codificar documentos em um formato que é tanto legível por humanos quanto legível por máquinas. É um padrão do World Wide Web Consortium (W3C).

### XA Transactions
Padrão para processamento de transações distribuídas que permite que um gerenciador de transações coordene transações em múltiplos gerenciadores de recursos (por exemplo, bancos de dados). Parte do padrão X/Open Distributed Transaction Processing (DTP).

## Y

### YAML (YAML Ain't Markup Language)
Linguagem de serialização de dados legível por humans que é comumente usada para arquivos de configuração e em aplicações onde os dados estão sendo armazenados ou transmitidos. Projetado para ser fácil de ler e escrever.

### You Only Look Once (YOLO)
Sistema de identificação de objetos em tempo real que processa imagens apenas uma vez, usando uma única rede neural para prever caixas delimitadoras e probabilidades de classe diretamente a partir de imagens completas em uma única avaliação.

## Z

### Zero Trust
Modelo de segurança centrado na crença de que as organizações não devem confiar automaticamente em nada dentro ou fora de seus perímetros e, em vez disso, devem verificar tudo o que tenta se conectar aos seus sistemas antes de conceder acesso.

### ZooKeeper
Serviço de coordenação de código aberto para aplicações distribuídas. Oferece um conjunto simples de primitivos que as aplicações distribuídas podem construir sobre eles para implementar sincronização, manutenção de grupos de nós e presença de grupo.

### Zombie Process
Processo que terminou sua execução, mas ainda tem uma entrada na tabela de processos para permitir que seu processo pai leia seu status de saída. Também conhecido como processo defunto.

## Conclusão

Este glossário fornece uma base sólida de terminologia em arquitetura de software e tópicos relacionados. Entender esses termos é essencial para comunicação eficaz, tomada de decisão informada e aprendizado contínuo no campo da arquitetura de software.

À medida que você encontrar novos termos ou conceitos em seu trabalho, considere adicioná-los ao seu glossário pessoal ou organizacional para manter um vocabulário compartilhado e atualizado.

> **Próxima Parte**: PARTE 77 — PLANO DE ESTUDOS - Estruturas e recomendações para aprendizado sistemático em arquitetura de software, incluindo recursos, cronogramas e caminhos de especialização.
