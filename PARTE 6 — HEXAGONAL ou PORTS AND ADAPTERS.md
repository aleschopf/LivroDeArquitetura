---
trilha: "INICIANTE"
---
**Navegação:** [[MOC — TRILHA INICIANTE]]
← [[PARTE 5 — CLEAN ARCHITECTURE]] | #trilha/iniciante | [[MOC — TRILHA INICIANTE]] →

---
# PARTE 6 — HEXAGONAL ARCHITECTURE / PORTS AND ADAPTERS

> 🧠 **ESSENCIAL**
> 
> Hexagonal Architecture (também conhecida como Ports and Adapters) propõe isolar o núcleo de negócio do mundo exterior através de portas (interfaces) que definem como o núcleo interage com o exterior, e adaptadores que implementam essas portas para tecnologias específicas.

## O que é Hexagonal Architecture?
Hexagonal Architecture é um padrão arquitetural proposto por Alistair Cockburn que organiza o sistema como um hexágono (ou polígono com N lados) onde cada lado representa um tipo diferente de interação com o mundo exterior (portas), e adaptadores convertem essas interações para tecnologias específicas. O núcleo contém a lógica de negócio puro e é completamente isolado de preocupações externas.

### Por que existe?
Como resposta à dificuldade de testar e manter sistemas onde a lógica de negócio está entrelaçada com detalhes de apresentação, banco de dados e outros aspectos técnicos, tornando difícil mudar tecnologias ou suportar múltiplos tipos de entrada/saída.

### Qual problema resolve?
- Dificuldade de testar lógica de negócio sem inicializar bancos de dados, servidores web ou outros componentes externos
- Dificuldade de suportar múltiplos tipos de entrada/saída (web, mobile, linha de comando, batch, etc.) com mesma lógica de negócio
- Dificuldade de mudar tecnologia de banco de dados ou framework sem afetar lógica de negócio
- Lógica de negócio espalhada por múltiplas camadas dificultando compreensão e manutenção
- Acoplamento entre regras de negócio e detalhes técnicos específicos

### Como funciona internamente?
- O sistema é dividido em duas áreas principais:
  1. **Núcleo (Domain/Core)** - Contém entidades de negócio, casos de uso e regras de negócio puro
  2. **Mundo Exterior** - Contém tudo que interage com o sistema (usuários, sistemas externos, bancos de dispositivos)
- Entre estas áreas existem **Portas** (Ports) - interfaces que definem como o núcleo interage com o mundo exterior
- Do mundo exterior para o núcleo: **Portas de Entrada** (Driving Ports / Primary Ports) - recebem comandos do mundo exterior
- Do núcleo para o mundo exterior: **Portas de Saída** (Driven Ports / Secondary Ports) - enviam comandos para o mundo exterior
- **Adaptadores** (Adapters) convertem entre tecnologia específica e as portas:
  - Adaptadores de Entrada: convertem de tecnologia específica (HTTP, CLI, etc.) para porta de entrada
  - Adaptadores de Saída: convertem de porta de saída para tecnologia específica (banco de dados, mensagem, etc.)
- Dependências apontam apenas para dentro: o núcleo não conhece adaptadores, apenas portas

### Como implementar?
1. Identificar as diferentes formas como o sistema interage com o mundo exterior (entradas e saídas)
2. Definir portas de entrada como interfaces no núcleo que representam comandos que podem ser recebidos
3. Definir portas de saída como interfaces no núcleo que representam serviços que o núcleo precisa
4. Implementar lógica de negócio no núcleo usando apenas essas portas (sem conhecer implementações)
5. Criar adaptadores de entrada que convertem de tecnologia específica para chamar portas de entrada
6. Criar adaptadores de saída que implementam portas de saída usando tecnologia específica
7. Configurar o sistema para conectar adaptadores às portas apropriadas
8. Garantir que nenhuma dependência atravesse do núcleo para fora (o núcleo depende apenas de portas)

### Quais são as alternativas?
- Arquitetura em camadas tradicional (Layered Architecture)
- Clean Architecture
- Onion Architecture
- Arquitetura monolítica não estruturada
- Microservices
- Arquitetura baseada em eventos

### Quais são os trade-offs?
**Vantagens da Hexagonal Architecture:**
- Excelente testabilidade: núcleo pode ser testado com adaptadores em memória ou mocks
- Flexibilidade de entrada/saída: fácil de adicionar novos tipos de adaptadores (web, mobile, linha de comando, etc.)
- Independência tecnológica: lógica de negócio não depende de banco de dados, framework ou UI específicos
- Clareza de fronteiras: óbvio onde fica lógica de negócio vs detalhes técnicos
- Facilidade de mudança tecnológica: troque adaptadores sem tocar no núcleo
- Suporte natural a múltiplas interfaces simultâneas

**Desvantagens da Hexagonal Architecture:**
- Sobrehead inicial de definição de portas e adaptadores
- Pode parecer excessivamente formal para aplicações muito simples
- Requer disciplina para manter o núcleo puro (nenhum vazamento de detalhes técnicos)
- Pode haver sobrehead de conversão entre formatos nas portas
- Necessidade de gerenciar múltiplos adaptadores quando se suporta múltiplas tecnologias

### Quando usar?
- Sistemas que precisam suportar múltiplos tipos de entrada/saída (web, API, mobile, linha de comando, batch)
- Quando se antecipa necessidade de mudar tecnologia de apresentação ou armazenamento
- Quando se quer garantir testabilidade elevada da lógica de negócio
- Sistemas de longa vida onde independência tecnológica é valiosa
- Quando múltiplas equipes ou sistemas externos precisam interagir com o mesmo núcleo de negócio
- Quando se quer evitar acoplamento entre regras de negócio e detalhes específicos de tecnologia

### Quando não usar?
- Protótipos descartáveis onde velocidade é a única prioridade
- Aplicações muito simples onde o overhead não traz benefício proporcional
- Equipes que rejeitam fortemente a ideia de portas e adaptadores adicionais
- Quando se está em um ambiente altamente restrito onde cada classe conta (sistemas embarcados extremos)
- Quando se vai descartar o sistema após uso único ou muito limitado
- Quando o domínio é tão simples que não há benefício claro no isolamento

### Quais são os erros mais comuns?
- Vazar detalhes técnicos para o núcleo através das portas (ex: porta que retorna tipos específicos de ORM)
- Fazer o núcleo conhecer adaptadores específicos (violando o princípio de dependência para dentro)
- Colocar lógica de negócio nos adaptadores (deve ficar apenas no núcleo)
- Definir portas muito específicas que funcionam apenas com uma tecnologia
- Esquecer de tornar adaptadores fins - colocando muita lógica neles
- Não separar claramente portas de entrada e saída

### Como isso afeta:
- *performance:* Impacto mínimo devido a indireção de interface (geralmente insignificante em aplicações de negócio)
- *escalabilidade:* Similar a outras arquiteturas; depende de implementação específica
- *disponibilidade:* Similar; problemas em adaptadores afetam respective entrada/saída mas não necessariamente núcleo
- *consistência:* Excelente para manter consistência de regras de negócio devido ao isolamento
- *segurança:* Similar; segurança de entrada geralmente acontece nos adaptadores de entrada
- *custo:* Custo inicial pode ser maior, mas custo de mudança tecnológica e manutenção a longo prazo tende a ser menor
- *observabilidade:* Excelente pois pontos claros de entrada/saída facilitam monitoring e logging
- *complexidade operacional:* Similar; pode aumentar devido a mais componentes, mas reduz devido a melhor isolamento

### Exemplos reais de aplicação
- Sistemas de pagamento que precisam aceitar cartão de crédito, boleto, transferência e Pix através de diferentes adaptadores de entrada, mas usar o mesmo núcleo para validação e processamento
- Sistemas de jogo que precisam suportar web, mobile e console com mesma lógica de jogo
- Sistemas de comércio eletrônico que recebem pedidos de website, app móvel, call center e integrações de parceiros
- Sistemas financeiros que precisam integrar com múltiplos bancos, corretoras e sistemas legados através de diferentes adaptadores de saída
- Aplicações onde houve necessidade de mudar de tecnologia (ex: de SOAP para REST, de um banco SQL para NoSQL) sem reescrever lógica de negócio

### Exemplo simplificado
Estrutura básica de Hexagonal Architecture para um sistema de gerenciamento de tarefas:
```
src/
├── main/
│   ├── java/
│   │   └── com/
│   │       └── example/
│   │           └── taskmanager/
│   │               ├── TaskManagerApplication.java
│   │               ├── domain/
│   │               │   ├── model/
│   │               │   │   └── Task.java
│   │               │   ├── ports/
│   │               │   │   ├── in/
│   │               │   │   │   ├── AddTaskUseCase.java (porta de entrada)
│   │               │   │   │   ├── GetTaskUseCase.java (porta de entrada)
│   │               │   │   │   ├── UpdateTaskUseCase.java (porta de entrada)
│   │               │   │   │   └── DeleteTaskUseCase.java (porta de entrada)
│   │               │   │   └── out/
│   │               │   │       ├── TaskRepository.java (porta de saída)
│   │               │   │       └── NotificationService.java (porta de saída)
│   │               │   └── service/
│   │               │       ├── AddTaskService.java (implementa porta de entrada)
│   │               │       ├── GetTaskService.java (implementa porta de entrada)
│   │               │       ├── UpdateTaskService.java (implementa porta de entrada)
│   │               │       └── DeleteTaskService.java (implementa porta de entrada)
│   │               ├── adapters/
│   │               │   ├── in/
│   │               │   │   ├── web/
│   │               │   │   │   ├── TaskRestController.java (adaptador de entrada HTTP)
│   │               │   │   │   └── TaskWebController.java (adaptador de entrada web tradicional)
│   │               │   │   ├── cli/
│   │               │   │   │   └── TaskCliCommand.java (adaptador de entrada linha de comando)
│   │               │   │   └── event/
│   │               │   │       └── TaskEventListener.java (adaptador de entrada de eventos)
│   │               │   └── out/
│   │               │       ├── persistence/
│   │               │       │   ├── TaskRepositoryJpa.java (adaptador de saída JPA)
│   │               │       │   ├── TaskRepositoryMongo.java (adaptador de saída MongoDB)
│   │               │       │   └── TaskRepositoryFile.java (adaptador de saída arquivo)
│   │               │       ├── messaging/
│   │               │       │   ├── NotificationServiceEmail.java (adaptador de saída email)
│   │               │       │   ├── NotificationServiceSms.java (adaptador de saída SMS)
│   │               │       │   └── NotificationServicePush.java (adaptador de saída push)
│   │               │       └── caching/
│   │               │           ├── TaskCacheRedis.java (adaptador de saída Redis)
│   │               │           └── TaskCacheCaffeine.java (adaptador de saída Caffeine)
│   │               └── config/
│   │                   ├── HexagonalConfig.java
│   │                   └── AdapterFactory.java
│   └── resources/
│       └── application.properties
└── test/
    └── ... (testes usando adaptadores em memória)
```

### Exemplo de sistema de produção
Um sistema de processamento de pedidos para e-commerce com múltiplas interfaces:
- **Núcleo (Domain):**
  - Entidades: Pedido, ItemPedido, Cliente, Pagamento (regras de negócio puro)
  - Portas de Entrada: 
    - CriarPedidoUseCase (recebe comando para criar pedido)
    - AdicionarItemPedidoUseCase (recebe comando para adicionar item)
    - ProcessarPagamentoUseCase (recebe comando para processar pagamento)
    - CancelarPedidoUseCase (recebe comando para cancelar pedido)
    - ObterDetalhesPedidoUseCase (recebe comando para obter detalhes)
  - Portas de Saída:
    - PedidoRepository (persiste e busca pedidos)
    - EstoqueService (verifica e atualiza estoque)
    - PagamentoGateway (processa pagamentos externos)
    - EmailService (envia emails de notificação)
    - EventPublisher (publica eventos para outros sistemas)
- **Adaptadores de Entrada:**
  - Web: RestController que recebe requisições HTTP/JSON e chama portas de entrada
  - Mobile: Interface semelhante adaptada para consumo de app móvel
  - Call Center: Interface para operadores atenderem pedidos por telefone
  - Parceiros: Adaptadores para XML/EDI de sistemas de parceiros
  - Batch: Processador de arquivos CSV/XML para importação em lote
- **Adaptadores de Saída:**
  - Persistência: Implementações JPA, MongoDB, Cassandra para PedidoRepository
  - Estoque: Adaptadores para sistema de estoque interno ou serviço externo
  - Pagamento: Adaptadores para gateways como Stripe, PayPal, PagSeguro
  - Email: Adaptadores para SES, SendGrid, SMTP
  - Eventos: Adaptadores para Kafka, RabbitMQ, AWS SNS
- Testes do núcleo usam adaptadores em memória para repositório, estoque, pagamento, etc.
- Deploy como múltiplos adaptadores conectados ao mesmo núcleo (pode ser um único deploy ou múltiplos serviços)

### Como esse assunto pode aparecer em uma entrevista.
> 🎯 **ENTREVISTA — ALTA FREQUÊNCIA**
> 
> "Explique como você aplicaria os princípios da Hexagonal Architecture a um sistema de reservas de hotéis que precisa suportar website, app móvel, agente de viagens externos e sistema interno de recepção."
> 
> **Armadilha:** Focar apenas na estrutura de pastas sem explicar como as portas isolam o núcleo de negócio.
> 
> **Como raciocinar:** Descrever o núcleo contendo entidades como Reserva, Quarto, Hóspede com regras de negócio (validação de datas, cálculo de tarifas, políticas de cancelamento). Definir portas de entrada como FazerReservaUseCase, CancelarReservaUseCase, CheckInUseCase, etc. Definir portas de saída como QuartoRepository, PagamentoGateway, NotificationService, etc. Mostrar como adaptadores de entrada para website, app móvel, agente de viagens e recepção interna todos chamam as mesmas portas de entrada, enquanto adaptadores de saída lidam com diferentes tecnologias de banco de dados, pagamento e notificação sem afetar o núcleo.

## Portas (Ports)

> 🎯 **ENTREVISTA — ALTA FREQUÊNCIA**
> 
> Portas são o cerne da Hexagonal Architecture e frequentemente perguntadas em entrevistas para testar entendimento de interfaces e isolamento.

### definição
Portas são interfaces definidas no núcleo que representam como o núcleo interage com o mundo exterior. Elas definem os contratos de comunicação sem especificar como essa comunicação é implementada. Existem dois tipos principais: portas de entrada (que recebem comandos do mundo exterior) e portas de saída (que enviam comandos para o mundo exterior).

### Por que existe?
Para definir claramente os pontos de interação entre o núcleo de negócio puro e o mundo exterior de detalhes técnicos, permitindo que o núcleo permaneça completamente alheio a como as interações são realizadas.

### Como funciona internamente?
- Portas de Entrada (Driving Ports / Primary Ports):
  - Definem comandos que o mundo exterior pode dar ao núcleo
  - São implementadas por casos de uso ou serviços de aplicação no núcleo
  - Adaptadores de entrada chamam essas portas para entregar comandos do mundo exterior
  - Exemplo: `UserService` (porta de entrada) com métodos como `registerUser(UserCommand cmd)`
- Portas de Saída (Driven Ports / Secondary Ports):
  - Definem serviços que o núcleo precisa do mundo exterior
  - São implementadas por adaptadores de saída no mundo exterior
  - Núcleo chama essas portas para acessar funcionalidades externas
  - Exemplo: `UserRepository` (porta de saída) com métodos como `save(User user)` e `findById(String id)`
- O núcleo depende apenas dessas interfaces, não de implementações concretas
- Nenhum detalhe de tecnologia vaza para o núcleo através das portas
- Comunicação é sempre síncrona através de chamadas de método (assíncrono pode ser modelado através de retornos como CompletableFuture ou eventos)

### Como implementar?
1. Identificar todas as formas como o núcleo precisa interagir com o mundo exterior
2. Para cada tipo de interação, definir uma interface no núcleo:
   - Se o mundo exterior dá comandos ao núcleo → porta de entrada
   - Se o núcleo precisa de serviços do mundo exterior → porta de saída
3. Nomear portas de forma que reflitam a intenção de negócio, não detalhes técnicos
4. Manter interfaces focadas e coesas (princípio da segregação de interface)
5. Evitar parâmetros ou retornos que vazem detalhes específicos de tecnologia
6. Usar tipos de domínio puro nos parâmetros e retornos (entidades, objetos de valor, etc.)
7. Documentar claramente o propósito e contrato de cada porta
8. Garantir que portas sejam estáveis e mudem raramente (representam necessidades de negócio, não detalhes técnicos)

### Quais são as alternativas?
- Nenhuma interface explícita (núcleo conhece diretamente detalhes de tecnologia)
- Interfaces que vazam detalhes de tecnologia (ex: retornando entidades específicas de ORM)
- Interfaces específicas de tecnologia (ex: usando diretamente Hibernate Session ou ServletRequest)
- Callbacks ou eventos em vez de interfaces síncronas
- Dependência direta em classes concretas de tecnologia

### Quais são os trade-offs?
**Vantagens de portas bem definidas:**
- Núcleo totalmente isolado de detalhes técnicos
- Excelente testabilidade: pode-se usar mocks ou implementações em memória para testar núcleo
- Flexibilidade para mudar tecnologia sem afetar núcleo
- Clareza sobre o que o núcleo realmente precisa do mundo exterior
- Facilidade de suportar múltiplas tecnologias simultaneamente para o mesmo tipo de interação
- Contratos estáveis que evoluem lentamente baseado em necessidades de negócio

**Desvantagens/custos:**
- Sobrehead de definição e manutenção de interfaces
- Pode parecer indireto para desenvolvedores acostumados com acesso direto
- Requer disciplina para manter portas puras (nenhum vazamento de detalhes)
- Necessidade de pensar cuidadosamente sobre o que pertence ao núcleo vs mundo exterior
- Em casos muito simples, pode parecer excesso de formalismo

### Quando usar?
- Sempre que o núcleo precisar interagir com o mundo exterior (praticamente todo sistema não trivial)
- Quando se quer garantir isolamento de regras de negócio de detalhes técnicos
- Quando se antecipa necessidade de mudar tecnologia de interação
- Quando múltiplas tecnologias precisam interagir com o mesmo núcleo
- Quando se quer maximizar testabilidade do núcleo

### Quando não usar?
- Quando se está construindo um sistema tão simples que interação com mundo exterior é direta e simples
- Quando se está em um contexto onde acesso direto a tecnologia é considerado aceitável ou até preferível
- Quando se está prototipando e velocidade é a única prioridade
- Quando se está em um ambiente altamente restrito onde cada byte conta e indireção é proibida
- Quando o tipo de interação é tão simples e estável que não justifica abstração

### Quais são os erros mais comuns?
- Portas que retornam tipos específicos de tecnologia (ex: List<Entity> onde Entity é classe JPA anotada)
- Parâmetros de porta que expõem detalhes de tecnologia (ex: HttpServletRequest, EntityManager)
- Portas que são tão específicas que funcionam apenas com uma implementação concreta
- Misturar comandos e consultas na mesma porta violando CQS (Command Query Separation) quando não apropriado
- Fazer portas conhecerem demais sobre o mundo exterior (acoplamento reverso)
- Não separar claramente portas de entrada e saída
- Fazer portas genéricas demais que perdem a intenção de negócio

### Como isso afeta:
- *performance:* Impacto mínimogeralmente <1% devido a chamada de interface indireta vs direta
- *escalabilidade:* Nenhum impacto direto
- *disponibilidade:* Nenhum impacto direto
- *consistência:* Melhora pois contratos claros reduzem mal-entendidos sobre responsabilidades
- *segurança:* Similar; portas podem incluir preocupações de segurança como validação de entrada de negócio
- *custo:* Similar; foco em onde o contrato reside ao invés de adicionar ou remover código
- *observabilidade:* Melhora pois pontos claros de interação facilitam logging e tracing
- *complexidade operacional:* Similar; pode reduzir bugs devido a melhor separação de responsabilidades

### Exemplos reais de aplicação
- Porta de entrada `AccountService` em sistema bancário com métodos como `openAccount(OpenAccountCommand)`, `closeAccount(CloseAccountCommand)`, `transferFunds(TransferFundsCommand)`
- Porta de saída `MarketDataService` em sistema de trading com métodos como `getQuote(String symbol)`, `subscribeToPriceUpdates(String symbol, PriceUpdateListener)`
- Porta de entrada `GameService` em sistema de jogo com métodos como `startGame(StartGameCommand)`, `makeMove(MoveCommand)`, `endGame()`
- Porta de saída `NotificationService` em sistema de e-commerce com métodos como `sendOrderConfirmation(Order order)`, `sendShippingUpdate(ShippingUpdate update)`
- Porta de saída `ExternalPaymentGateway` em sistema de pagamento com métodos como `processPayment(PaymentRequest)`, `refundPayment(String paymentId, BigDecimal amount)`

### Exemplo simplificado
Porta violando isolamento (errada):
```java
// ❌ ERRADO: Porta de saída conhece detalhes específicos de ORM (Hibernate)
public interface TaskRepository {
    List<Task> findAll(); // Retorna tipo específico se Task for entidade JPA
    Task findById(Long id); // Pode retornar proxy ou lazy loading específico
    void save(Task task); // Pode causar flush imediato ou comportamento específico
    void deleteById(Long id);
    
    // Métodos que expõem detalhes específicos de tecnologia
    EntityManager getEntityManager(); // Acesso direto ao persistence context
    Session getSession(); // Acesso direto ao session do Hibernate
}
```

Porta respeitando isolamento (correta):
```java
// ✅ CORRETO: Porta de saída usa apenas tipos de domínio puro
public interface TaskRepository {
    List<Task> findAll(); // Task é entidade pura de domínio
    Optional<Task> findById(Long id);
    Task save(Task task);
    void deleteById(Long id);
    long count();
    boolean existsById(Long id);
    
    // Métodos de negócio específicos se fizer sentido no contexto
    List<Task> findByTitleContaining(String titleFilter);
    List<Task> findByCompleted(boolean completed);
    List<Task> findByCreatedAfter(LocalDateTime date);
}
```

Porta de entrada com comando claro:
```java
// ✅ CORRETO: Porta de entrada usa comandos de negócio puro
public interface TaskService {
    TaskResponse addTask(AddTaskCommand command);
    TaskResponse getTask(GetTaskCommand command);
    TaskResponse updateTask(UpdateTaskCommand command);
    void deleteTask(DeleteTaskCommand command);
    List<TaskResponse> listTasks(ListTasksCommand command);
}
```

Comandos de entrada (tipos de domínio puro):
```java
// ✅ CORRETO: Comandos contêm apenas dados de negócio, nenhum detalhe de tecnologia
public record AddTaskCommand(String title) {
    public AddTaskCommand {
        if (title == null || title.trim().isEmpty()) {
            throw new IllegalArgumentException("Título não pode ser vazio");
        }
    }
}

public record GetTaskCommand(Long id) {
    public GetTaskCommand {
        if (id == null || id <= 0) {
            throw new IllegalArgumentException("ID deve ser positivo");
        }
    }
}

// ... outros comandos semelhantes
```

### Exemplo de sistema de produção
Sistema de gestão hospitalar com múltiplas interfaces:
- **Portas de Entrada (no núcleo):**
  - `PatientService`: métodos como `registerPatient(RegisterPatientCommand)`, `updatePatientInfo(UpdatePatientCommand)`
  - `AppointmentService`: métodos como `scheduleAppointment(ScheduleAppointmentCommand)`, `cancelAppointment(CancelAppointmentCommand)`
  - `MedicalRecordService`: métodos como `addMedicalRecord(AddMedicalRecordCommand)`, `getMedicalHistory(GetMedicalHistoryCommand)`
  - `BillingService`: métodos como `generateInvoice(GenerateInvoiceCommand)`, `processPayment(ProcessPaymentCommand)`
- **Portas de Saída (no núcleo):**
  - `PatientRepository`: métodos para persistir e buscar pacientes
  - `AppointmentRepository`: métodos para gerenciar agendamentos
  - `MedicalRecordRepository`: métodos para gerenciar prontuários
  - `NotificationService`: métodos para enviar lembretes, confirmações, alertas
  - `InsuranceService`: métodos para verificar cobertura e processar reembolsos
  - `LaboratoryService`: métodos para solicitar exames e receber resultados
  - `PharmacyService`: métodos para prescrever medicamentos e verificar interações
- **Adaptadores de Entrada:**
  - Web: Controllers REST que recebem requisições de portal do paciente e staff
  - Mobile: API para app móvel de pacientes e profissionais
  - Telefone: Sistema IVR para agendamento e consulta de informações básicas
  - Terminal: Interface para atendimento em balcão de hospital
  - Integração: Adaptadores para sistemas externos de laboratório, farmácia e convênios
- **Adaptadores de Saída:**
  - Persistência: Implementações usando PostgreSQL para repositórios
  - Notificação: Adaptadores para email, SMS e push notification
  - Seguros: Adaptadores para sistemas de convênios e seguradoras via HL7 ou APIs proprietárias
  - Laboratório: Adaptadores para sistemas de equipamentos médicos e laboratórios externos
  - Farmácia: Adaptadores para sistemas de controle de estoque e dispensação de medicamentos

### Como esse assunto pode aparecer em uma entrevista.
> 🎯 **ENTREVISTA — ALTA FREQUÊNCIA**
> 
> "Descreva como você definiria as portas para um sistema de negociação de ações que precisa receber ordens de traders através de diferentes interfaces (desktop, mobile, API de terceiros) e enviar confirmações para exchanges, custodians e sistemas de risco."
> 
> **Armadilha:** Sugerir portas que expõem detalhes específicos de protocolos de exchange ou tipos de mensagem proprietários.
> 
> **Como raciocinar:** Definir portas de entrada como `TradingService` com métodos como `placeOrder(PlaceOrderCommand)`, `cancelOrder(CancelOrderCommand)`, `amendOrder(AmendOrderCommand)` onde comandos contêm apenas dados de negócio (símbolo, quantidade, tipo de ordem, preço, etc.). Definir portas de saída como `ExchangeGateway` com métodos como `sendOrderToExchange(Order order)`, `MarketDataService` com métodos como `subscribeToMarketData(String symbol, MarketDataListener)`, `RiskService` com métodos como `checkRisk(RiskCheckRequest)`, e `CustodianService` com métodos como `settleTrade(SettlementRequest)`. Mostrar como adaptadores de entrada para diferentes interfaces (web desktop, app móvel, FIX protocol para terceiros) todos chamam as mesmas portas de entrada, enquanto adaptadores de saída lidam com diferentes protocolos de exchange, sistemas de custodian e verificação de risco sem afetar o núcleo de negócio de trading.

## Adaptadores (Adapters)

> 🎯 **ENTREVISTA — FREQUENTE**
> 
> Adaptadores são onde a Hexagonal Architecture se conecta ao mundo exterior; entrevistadores querem ver se você entende seu papel na conversão entre tecnologia específica e portas puras.

### definição
Adaptadores convertem entre o formato específico de tecnologia (HTTP, SQL, mensagem, etc.) e o formato definido pelas portas do núcleo. Adaptadores de entrada recebem comandos do mundo exterior e os traduzem para chamadas de porta de entrada. Adaptadores de saída recebem chamadas de porta de saída e os traduzem para ações específicas de tecnologia. Eles contêm todos os detalhes específicos de tecnologia mas nenhuma lógica de negócio.

### Por que existe?
Para traduzir entre o mundo exterior bagunçado (onde temos HTTP, SQL, JSON, protocolos proprietários, etc.) e o mundo interno limpo (onde temos portas com contratos de negócio puro) sem deixar que detalhes técnicos vazem para o núcleo ou que lógica de negócio contamine o mundo exterior.

### Como funciona internamente?
- Adaptadores de Entrada:
  - Recebem entrada de tecnologia específica (requisição HTTP, mensagem de fila, comando de linha de comando, etc.)
  - Validam e convertem entrada para objetos de comando puro de domínio
  - Chamam a porta de entrada apropriada no núcleo com esses objetos
  - Recebem resultado da porta de entrada e o convertem para formato de saída específico de tecnologia
  - Nunca contêm lógica de negócio (isso fica no núcleo)
  - Exemplos: REST controller que converte JSON para AddTaskCommand e chama taskService.addTask(command)
- Adaptadores de Saída:
  - Recebem chamadas de porta de saída do núcleo (ex: taskRepository.save(task))
  - Convertem objetos de domínio para formato específico de tecnologia (entidade JPA, documento MongoDB, comando SQL, etc.)
  - Executam a operação específica de tecnologia
  - Converte resultado de volta para objeto de domínio se necessário
  - Nunca contêm lógica de negócio (isso fica no núcleo)
  - Exemplos: repositório JPA que converte Task entidade de domínio para TaskEntity JPA e persiste
- Adaptadores dependem apenas do núcleo (por meio das portas) e de bibliotecas/frameworks específicos
- Núcleo não conhece adaptadores - apenas sabe que portas serão implementadas por alguém
- Comunicação entre adaptadores e núcleo ocorre exclusivamente através de chamadas de interface de porta

### Como implementar?
1. Identificar todas as tecnologias específicas com as quais o sistema precisa interagir
2. Para cada tecnologia e tipo de interação (entrada/saída), criar um adaptador
3. Adaptadores de Entrada:
   - Receber entrada específica de tecnologia
   - Validar entrada (geralmente faz parte da responsabilidade do adaptador)
   - Converter para objetos de comando/puro de domínio
   - Chamar porta de entrada apropriada no núcleo
   - Converter resultado para saída específica de tecnologia
   - Tratar erros de acordo com protocolo específico de tecnologia
4. Adaptadores de Saída:
   - Receber chamadas de porta de saída do núcleo
   - Converter objetos de domínio para formato específico de tecnologia
   - Executar operação específica de tecnologia
   - Lidar com resultados específicos de tecnologia (linhas afetadas, IDs gerados, etc.)
   - Converter resultado de volta para objeto de domínio se necessário pela interface da porta
5. Nunca colocar lógica de negócio nos adaptadores
6. Garantir que adaptadores sejam fins e focados em tradução, não em lógica de negócio
7. Usar padrões como mappers, converters ou builders para facilitar conversão de dados
8. Configurar o sistema para conectar adaptadores às portas apropriadas (injeção de dependência, configuração, etc.)

### Quais são as alternativas?
- Misturar lógica de negócio com detalhes técnicos (violando separação de preocupações)
- Fazer núcleo conhecer diretamente detalhes de tecnologia
- Deixar portas responsáveis por conversão de formato
- Usar camadas de serviço genéricas que misturam aplicação e tecnologia
- Não ter camada de adaptação e deixar núcleo lidar diretamente com detalhes técnicos

### Quais são os trade-offs?
**Vantagens de adaptadores bem definidos:**
- Isolamento completo de regras de negócio de detalhes técnicos
- Flexibilidade para suportar múltiplas tecnologias simultaneamente (web, mobile, linha de comando, etc.)
- Testabilidade melhorada pois núcleo pode ser testado com adaptadores em memória ou mocks
- Clareza sobre onde ficam os detalhes específicos de cada tecnologia
- Facilidade de mudar tecnologia sem afetar regras de negócio
- Facilidade de suportar tecnologias legadas juntamente com modernas
- Testabilidade dos próprios adaptadores isoladamente

**Desvantagens/custos:**
- Sobrehead de criação de classes de adaptação e conversão
- Pode parecer indireto para desenvolvedores acostumados com acesso direto
- Requer disciplina para manter a camada pura (nenhuma regra de negócio)
- Pode haver sobrehead de conversão de dados entre formatos
- Necessidade de gerenciar múltiplas implementações quando se suporta múltiplas tecnologias
- Risco de adaptadores ficarem muito grandes se não houver bom limite de responsabilidade

### Quando usar?
- Sempre que se quiser manter regras de negócio independentes de detalhes de apresentação, armazenamento ou comunicação
- Quando múltiplas tecnologias externas precisam acessar a mesma lógica de negócio
- Quando se antecipa necessidade de mudar tecnologia de apresentação ou armazenamento
- Quando se quer garantir que núcleo seja testável sem inicializar frameworks completos
- Quando se quer suportar diferentes tipos de clientes (web app, app móvel, API de terceiros, linha de comando, batch) com mesma lógica de negócio
- Quando se precisa integrar com sistemas legados usando tecnologias antigas ou proprietárias

### Quando não usar?
- Quando o sistema é tão simples que o overhead não traz benefício proporcional
- Quando se está construindo um protótipo descartável onde velocidade é a única prioridade
- Quando se está em um contexto onde acesso direto a tecnologia é considerado aceitável ou até preferível
- Quando se está construindo uma camada de apresentação pura onde não há necessidade de lógica de negócio
- Quando se está em um ambiente altamente restrito onde cada classe conta e conversão de dados é proibida

### Quais são os erros mais comuns?
- Colocar regras de negócio nos adaptadores de entrada ou saída
- Fazer adaptadores vazarem detalhes externos para o núcleo através de parâmetros ou retornos das portas
- Adaptadores conhecerem demais sobre o núcleo (acoplamento reverso)
- Tratar adaptadores como locais genéricos para qualquer código que não se encaixe em outro lugar
- Esquecer de validar entrada adequadamente nos adaptadores de entrada
- Fazer adaptadores de saída fazerem suposições sobre uso ou chamadas futuras do núcleo
- Não separar claramente adaptadores de entrada e saída
- Fazer um único adaptador fazer tanto entrada quanto saída quando seria melhor separar

### Como isso afeta:
- *performance:* Impacto moderado devido a conversão de dados e chamadas de framework (pode ser significativo em alto volume)
- *escalabilidade:* Similar a outras arquiteturas; depende de implementação específica de adaptadores
- *disponibilidade:* Similar; falhas em adaptadores afetam respective entrada/saída mas não necessariamente núcleo
- *consistência:* Similar; depende de como adaptadores lidam com transições e erros
- *segurança:* Adaptadores são onde muita segurança de entrada acontece (validação, sanitização, autenticação de nível de entrada)
- *custo:* Custo inicial maior devido a classes adicionais, mas custo de mudança tecnológica menor
- *observabilidade:* Melhora pois pontos claros de entrada/saída facilitam logging de monitoramento e métricas de tecnologia específica
- *complexidade operacional:* Similar; pode aumentar devido a mais componentes para gerenciar, mas reduz devido a melhor isolamento e testabilidade

### Exemplos reais de aplicação
- Adaptadores REST que:
  - Recebem requisições HTTP e JSON ou XML
  - Validam dados de entrada (geralmente usando bean validation ou similar)
  - Convertem para objetos de comando de domínio (AddTaskCommand, etc.)
  - Chamam porta de entrada apropriada (taskService.addTask(command))
  - Convertem resultado de porta de entrada para JSON ou XML de resposta
  - Tratam códigos de status HTTP e cabeçalhos apropriadamente
  - Nunca contêm lógica como cálculo de taxas, aplicação de descontos ou validação de regras de domínio
- Adaptadores de Banco de Dados que:
  - Recebem chamadas de porta de saída (taskRepository.save(task))
  - Convertem Task entidade de domínio para tecnologia específica (JPA Entity, MongoDB Document, etc.)
  - Executam operação de persistência (INSERT, UPDATE, save, etc.)
  - Lidam com resultado específico (ID gerado, linhas afetadas, etc.)
  - Converte resultado de volta para entidade de domínio se necessário
  - Nunca contêm lógica como cálculo de totais, aplicação de descontos ou regras de negócio
- Adaptadores de Mensagem/Fila que:
  - Recebem mensagens de tecnologia específica (JMS, AMQP, Kafka, etc.)
  - Convertem payload para objetos de comando de domínio
  - Chamam porta de entrada apropriada
  - Converte resultado de volta para mensagem de resposta se necessário pelo protocolo
  - Nunca contêm lógica como processamento de lote específico ou aplicação de regras de negócio
- Adaptadores de Arquivo que:
  - Leem arquivos de formato específico (CSV, XML, JSON, fixed width, etc.)
  - Convertem linhas/registros para objetos de comando de domínio
  - Chamam porta de entrada apropriada (usualmente em lote ou batch)
  - Nunca contêm lógica como validação de dados do lote além do básico de formato ou aplicação de regras de negócio

### Exemplo simplificado
Adaptador de entrada violando independência (errado):
```java
// ❌ ERRADO: REST controller sabe demais sobre caso de uso e contém lógica de negócio
@RestController
public class TaskController {
    @Autowired
    private TaskService taskService;
    
    @PostMapping("/tasks")
    public ResponseEntity<?> addTask(@RequestBody TaskRequest request) {
        // Lógica de negócio inadequadamente colocada aqui (validação além do básico)
        if (request.getTitle() == null || request.getTitle().isEmpty()) {
            return ResponseEntity.badRequest().body("Título é obrigatório");
        }
        // Mais lógica de negócio aqui - verificar se título já existe, etc.
        
        // Chamada ao serviço (porta de entrada)
        TaskResponse response = taskService.addTask(new AddTaskCommand(request.getTitle()));
        
        // Mais lógica de negócio aqui - modificar resposta baseado em regras de negócio
        
        return ResponseEntity.ok(response);
    }
}
```

Adaptador de entrada respeitando independência (correto):
```java
// ✅ CORRETO: Controller apenas traduz entre HTTP e porta de entrada
@RestController
@RequestMapping("/api/tasks")
public class TaskController {
    private final TaskService taskService; // Porta de entrada
    
    public TaskController(TaskService taskService) {
        this.taskService = taskService;
    }
    
    @PostMapping
    public ResponseEntity<TaskResponse> addTask(@RequestBody AddTaskApiRequest request) {
        // Apenas validação básica de formato e conversão
        AddTaskCommand command = new AddTaskCommand(request.getTitle());
        
        // Chamada pura para porta de entrada
        TaskResponse response = taskService.addTask(command);
        
        // Apenas conversão para formato de saída
        return ResponseEntity.ok(new TaskApiResponse(response.getId(), response.getTitle(), response.getCompleted()));
    }
    
    // Outros endpoints semelhantes para get, update, delete, list
}

// Objetos específicos de tecnologia (API) - contêm apenas dados, nenhuma lógica de negócio
record AddTaskApiRequest(String title) {}
record TaskApiResponse(Long id, String title, boolean completed) {}
```

Adaptador de saída respeitando independência (correto):
```java
// ✅ CORRETO: Adaptador JPA apenas traduz entre porta de saída e tecnologia específica
@Repository
public class TaskRepositoryJpa implements TaskRepository { // Implementa porta de saída
    private final EntityManager entityManager;
    
    public TaskRepositoryJpa(EntityManager entityManager) {
        this.entityManager = entityManager;
    }
    
    @Override
    public List<Task> findAll() {
        // Apenas traduz entre tecnologia e domínio
        List<TaskEntity> taskEntities = entityManager.createQuery("SELECT t FROM TaskEntity t", TaskEntity.class)
                                                    .getResultList();
        return taskEntities.stream()
                          .map(TaskEntity::toDomain) // Converte entidade JPA para domínio puro
                          .collect(Collectors.toList());
    }
    
    @Override
    public Optional<Task> findById(Long id) {
        TaskEntity taskEntity = entityManager.find(TaskEntity.class, id);
        return Optional.ofNullable(taskEntity).map(TaskEntity::toDomain);
    }
    
    @Override
    public Task save(Task task) {
        TaskEntity taskEntity;
        if (task.getId() == null) {
            taskEntity = new TaskEntity(task); // Converte domínio para entidade JPA
            entityManager.persist(taskEntity);
        } else {
            taskEntity = entityManager.find(TaskEntity.class, task.getId());
            if (taskEntity != null) {
                taskEntity.updateFromDomain(task); // Atualiza entidade JPA com dados de domínio
            } else {
                taskEntity = new TaskEntity(task);
                entityManager.persist(taskEntity);
            }
        }
        // Apenas traduz de volta para domínio
        return taskEntity.toDomain();
    }
    
    @Override
    public void deleteById(Long id) {
        TaskEntity taskEntity = entityManager.find(TaskEntity.class, id);
        if (taskEntity != null) {
            entityManager.remove(taskEntity);
        }
    }
}

// Entidade JPA específica de tecnologia - contém apenas mapeamento, nenhuma lógica de negócio
@Entity
@Table(name = "tasks")
class TaskEntity {
    @Id @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    @Column(nullable = false)
    private String title;
    
    @Column(nullable = false)
    private boolean completed;
    
    // Construtores, getters, setters
    
    // Métodos de conversão - apenas tradução, nenhuma lógica de negócio
    public TaskEntity(Task domain) {
        this.title = domain.getTitle();
        this.completed = domain.isCompleted();
        // ID é gerado pelo banco ou definido se existente
    }
    
    public Task toDomain() {
        return new Task(this.id, this.title, this.completed);
    }
    
    public void updateFromDomain(Task domain) {
        this.title = domain.getTitle();
        this.completed = domain.isCompleted();
        // ID não muda
    }
}
```

Mappers para conversão de dados:
```java
// ✅ CORRETO: Mapper apenas converte entre formato de domínio e tecnologia específica
public class TaskMapper {
    public static Task toDomain(TaskEntity entity) {
        return new Task(entity.getId(), entity.getTitle(), entity.isCompleted());
    }
    
    public static TaskEntity toEntity(Task domain) {
        return new TaskEntity(null, domain.getTitle(), domain.isCompleted());
    }
    
    // Similar para outros tipos de domínio e tecnologia
}
```

### Exemplo de sistema de produção
Sistema de processamento de pedidos com múltiplas tecnologias:
- **Adaptadores de Entrada Web:**
  - PedidoController: recebe requisições HTTP/JSON, valida formato básico, converte para CriarPedidoCommand, chama pedidoService.criarPedido(command)
  - Nenhuma lógica como cálculo de frete, validação de cupom ou aplicação de descontos (isso fica nos casos de uso/núcleo)
  - Tratamento adequado de códigos HTTP (201 Created, 400 Bad Request, etc.)
- **Adaptadores de Entrada Mobile:**
  - Similar ao web mas possivelmente usando protobuf ou formato otimizado para mobile
  - Mesma conversão para comandos de domínio, mesma chamada aos serviços de domínio
- **Adaptadores de Entrada de Parceiros (XML/EDI):**
  - Recebem arquivos XML ou mensagens EDI de parceiros comerciais
  - Convertem para comandos de domínio usando parsers específicos de formato
  - Chamam mesmos serviços de domínio
  - Nunca contêm lógica como validação de crédito do parceiro ou aplicação de regras de negócio específicas
- **Adaptadores de Saída de Persistência:**
  - PedidoRepositoryJpa: converte Pedido domínio para PedidoEntity JPA, persiste, converte resultado de volta
  - PedidoRepositoryMongo: converte Pedido domínio para documento MongoDB, salva, converte resultado de volta
  - Nenhuma lógica como cálculo de total ou aplicação de descontos
- **Adaptadores de Saída de Pagamento:**
  - PaymentGatewayStripe: converte PaymentRequest domínio para formato Stripe, chama API, converte resposta de volta
  - PaymentGatewayPayPal: semelhante para PayPal
  - PaymentGatewayBoleto: gera boleto bancário, retorna informações para pagamento
  - Nenhuma lógica como validação de limite de crédito ou aplicação de taxas de juros
- **Adaptadores de Saída de Notificação:**
  - NotificationServiceEmail: converte EmailNotification domínio para formato de email, envia via SMTP ou serviço de email
  - NotificationServiceSms: converte SMS domínio para formato de operadora, envia via gateway de SMS
  - NotificationServicePush: converte PushNotification domínio para formato de serviço de push, envia via FCM/APNS
  - Nenhuma lógica como determinação de quando enviar ou conteúdo da mensagem além do básico de formatação

### Como esse assunto pode aparecer em uma entrevista.
> 🎯 **ENTREVISTA — FREQUENTE**
> 
> "Explique como você estruturaria os adaptadores para um sistema que precisa ler transações de arquivos CSV, validar contra regras de negócio, e salvar em både um banco de dados relacional quanto em um data lake para análise."
> 
> **Armadilha:** Sugerir fazer validação de regras de negócio ou transformação de dados nos adaptadores de entrada ou saída.
> 
> **Como raciocinar:** Explicar que adaptadores de entrada para CSV apenas fazem:
> - Leitura e parsing básico do formato CSV
> - Conversão de cada linha para objeto de comando de domínio (ProcessTransactionCommand)
> - Validação básica de formato (número de colunas, tipos de dados básicos)
> - Chamada ao serviço de domínio apropriado (processTransactionService.process(command))
> - Nenhuma lógica de validação de regras de negócio (como valores válidos, regras de negócio específicas)
> - Nenhuma lógica de transformação de dados além do básico de tipo
> 
> Mostrar como adaptadores de saída então fazem:
> - Para banco de dados relacional: converte domínio para entidades JPA, persiste
> - Para data lake: converte domínio para formato Parquet/Avro, grava em particionado por data
> - Nenhuma lógica de negócio em nenhum dos adaptadores
> - Toda a lógica de validação de regras de negócio, cálculo de taxas, aplicação de regras específicas fica nos casos de uso ou entidades do núcleo

## Comparação com Clean Architecture

> 🎯 **ENTREVISTA — ALTA FREQUÊNCIA**
> 
> Comparar arquiteturas é comum em entrevistas para testar entendimento de trade-offs e contexto.

| Aspecto | Hexagonal Architecture | Clean Architecture |
|---------|------------------------|-------------------|
| **Visão Central** | Hexágono com portas no centro, adaptadores nos lados | Círculos concêntricos com entidades no centro |
| **Terminologia** | Portas (entrada/saída), Adaptadores, Núcleo (Domínio) | Entities, Use Cases, Interface Adapters, Frameworks & Drivers |
| **Foco Principal** | Isolamento através de portas que definem pontos de interação | Isolamento através de níveis de abstração (negócio vs mecanismo) |
| **Portas vs Camadas** | Portas definem contratos específicos de interação | Camadas representam níveis de abstração geral |
| **Adaptadores vs Interface Adapters** | Similar conceito: tradução entre tecnologia e contrato | Similar conceito: tradução entre tecnologia e uso |
| **Testabilidade** | Muito alta: núcleo testável com adaptadores em memória | Muito alta: núcleo testável sem frameworks |
| **Flexibilidade de Entrada/Saída** | Muito alta: projetada especificamente para múltiplas entradas/saídas | Alta: suporta múltiplas interfaces através de adaptadores |
| **Independência Tecnológica** | Alta: núcleo não conhece detalhes de tecnologia | Alta: núcleo não conhece detalhes de tecnologia |
| **Quando Usar** | Quando se foca em flexibilidade de entrada/saída (múltiplos tipos de UI, múltiplos bancos de dados, etc.) | Quando se foca em independência de regras de negócio em relação a detalhes técnicos |
| **Sobreposição** | Muito overlap; podem ser vistas como complementares ou variações do mesmo conceito | Muito overlap; clean architecture pode ser vista como uma especialização da hexagonal |
| **Exemplo de Uso** | Sistema onde se quer suportar web, mobile, API de terceiros, linha de comando com mesma lógica de negócio | Sistema onde regras de negócio são o ativo mais valioso e devem ser preservados independentemente de tecnologia |

## Quando usar Hexagonal Architecture

> 💡 **DICA DE ENTREVISTA**
> 
> Sempre relacione a escolha ao contexto específico - não trate como regra universal.

Use Hexagonal Architecture quando:
- O sistema precisa suportar múltiplos tipos de entrada/saída (web, mobile, API, linha de comando, batch, etc.)
- Se antecipa necessidade de mudar tecnologia de apresentação ou armazenamento durante a vida do sistema
- Se quer garantir que o núcleo do sistema seja altamente testável sem depender de frameworks completos
- Múltiplas interfaces externas (web, mobile, API de terceiros, sistemas legados) precisam acessar a mesma lógica de negócio
- Se quer evitar acoplamento entre regras de negócio e detalhes específicos de tecnologia de entrada ou saída
- Se está construindo um sistema onde diferentes equipes ou sistemas externos precisam integrar com o mesmo núcleo de negócio
- Se valoriza flexibilidade e capacidade de evolução tecnológica sem reescrever lógica de negócio
- Se quer suportar tecnologias legadas juntamente com modernas na mesma aplicação
- Se está construindo um sistema onde o ponto de entrada ou saída pode mudar frequentemente (ex: adicionar novos tipos de dispositivos, protocolos de comunicação, etc.)

Não use Hexagonal Architecture quando:
- Está construindo um protótipo descartável ou prova de conceito onde velocidade é a única prioridade
- O sistema é tão simples que o overhead de portas e adaptadores não traz benefício proporcional
- A equipe rejeita fortemente a ideia de portas e adaptadores adicionais
- Está em um ambiente altamente restrito onde cada classe ou byte conta (sistemas embarcados ultra-restritos)
- Se está prototipando e velocidade é a prioridade absoluta
- Se vai descartar o sistema após uso único ou muito limitado
- O domínio é tão simples que não há pontos de interação significativos para isolar
- Se está em um contexto onde desempenho crítico exige acesso direto e nenhuma indireção é tolerável

## Exercícios

### Exercício básico
Explique a diferença entre portas de entrada e portas de saída na Hexagonal Architecture usando um exemplo de sistema de reserva de hotéis.

### Exercício intermediário
Dado um cenário de sistema de processamento de pagamentos que precisa aceitar cartão de crédito, boleto, PIX e transferência bancária através de diferentes interfaces (website, app móvel, caixa eletrônico, API de parceiros) e enviar confirmações para bancos, sistemas antifraude e serviços de notificação, analise:
- Como as portas de entrada seriam modeladas (comandos que podem ser recebidos do mundo exterior)
- Como as portas de saída seriam modeladas (serviços que o núcleo precisa do mundo exterior)
- Como os adaptadores de entrada lidariam com website, app móvel, caixa eletrônico e API de parceiros
- Como os adaptadores de saída lidariam com bancos, sistemas antifraude e serviços de notificação
- Como o núcleo de negócio permaneceria isolado usando apenas as portas
- Como você testaria o núcleo do sistema sem nenhum framework ou tecnologia específica

### Exercício avançado
Analise um sistema que você conhece que usa ou poderia se beneficiar da Hexagonal Architecture:
1. Documente como as responsabilidades seriam distribuídas entre núcleo, portas e adaptadores
2. Mostre como as regras de negócio ficam isoladas dos detalhes técnicos
3. Avalie se a arquitetura segue corretamente os princípios de isolamento e testabilidade
4. Identifique oportunidades de melhoria na aplicação dos princípios
5. Descreva como você migraria um sistema existente para essa arquitetura com risco mínimo

### Exercício de entrevista
> 🎯 **ENTREVISTA — ALTA FREQUÊNCIA**
> 
> "Você está projetando um sistema de controle de acesso físico que precisa ler credenciais de diferentes tipos de leitores (RFID, biométrico, facial) e controlar diferentes tipos de trancas (eletromecânica, catraca, girafa) enquanto se comunica com sistemas de HR e de visitantes. Descreva como você aplicaria os princípios da Hexagonal Architecture."
> 
> Forneça a resposta esperada e explique o que torna ela eficaz.

### Desafio
Crie uma matriz de decisão que ajude a determinar quando usar Hexagonal Architecture, quando evoluir de uma arquitetura em camadas tradicional, e quando considerar alternativas como Clean ou Onion Architecture. Inclua fatores como: vida útil esperada do sistema, número e variedade de interfaces externas, necessidade de independência tecnológica, múltiplos tipos de entrada/saída, maturidade da equipe, e requisitos de testabilidade.