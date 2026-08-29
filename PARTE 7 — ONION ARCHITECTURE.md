---
trilha: "INTERMEDIÁRIA"
---
**Navegação:** [[MOC — TRILHA INTERMEDIÁRIA]]
← [[PARTE 0 — MAPA DA documenta��o]] | #trilha/intermediaria | [[PARTE 8 — DOMAIN-DRIVEN DESIGN]] →

---
# PARTE 7 — ONION ARCHITECTURE

> 🧠 **ESSENCIAL**
> 
> Onion Architecture organiza o sistema em camadas concêntricas onde quanto mais para o interior, mais alto é o nível de abstração e menos dependente de detalhes concretos, colocando o domínio de negócio no centro e fazendo todas as dependências apontarem para dentro.

## O que é Onion Architecture?
Onion Architecture é um padrão arquitetural proposto por Jeffrey Palermo que organiza o sistema em camadas semelhantes às de uma cebola, onde o núcleo contém o modelo de domínio de negócio e as camadas externas lidam com preocupações de infraestrutura, interface e testes. Todas as dependências apontam para dentro, ou seja, camadas externas dependem de camadas internas, mas nunca o inverso.

### Por que existe?
Como resposta à frustração com arquiteturas tradicionais em camadas onde as regras de negócio ficam dispersas ou acopladas com detalhes de infraestrutura, tornando difícil testar, manter e evoluir o sistema.

### Qual problema resolve?
- Acoplamento entre regras de negócio e detalhes de infraestrutura (banco de dados, framework, UI)
- Dificuldade de testar regras de negócio sem inicializar todo o sistema
- Dificuldade de mudar tecnologias sem afetar regras de negócio
- Regras de negócio difíceis de compreender devido à mistura com detalhes técnicos
- Baixa capacidade de reutilização de regras de negócio em diferentes contextos

### Como funciona internamente?
- O sistema é organizado em quatro camadas principais (do interior para o exterior):
  1. **Domain Layer** (Camada de Domínio) - Contém entidades de negócio, interfaces de repositório e serviços de domínio
  2. **Application Layer** (Camada de Aplicação) - Contém casos de uso, DTOs e interfaces de serviço
  3. **Infrastructure Layer** (Camada de Infraestrutura) - Contém implementações de repositórios, serviços externos e adaptadores de tecnologia
  4. **Presentation Layer** (Camada de Apresentação) - Contém controllers, views, apresentadores e ponto de entrada
- Pode existir uma quinta camada de **Testes** que depende apenas das camadas internas
- A **Dependency Rule** estabelece que dependências só podem apontar para dentro (camadas externas dependem de internas)
- Comunicação entre camadas ocorre através de interfaces que são implementadas por camadas externas

### Como implementar?
1. Definir entidades de negócio puro na camada de domínio (regras que não mudam com mudanças tecnológicas)
2. Definir interfaces de repositório na camada de domínio (contratos para persistência)
3. Definir serviços de domínio na camada de domínio (regras de negócio que não pertencem a uma entidade específica)
4. Definir casos de uso e DTOs na camada de aplicação (orquestração de domínio para requisitos específicos)
5. Implementar interfaces de repositório na camada de infraestrutura usando tecnologias específicas
6. Implementar outros serviços de infraestrutura (email, SMS, etc.) na camada de infraestrutura
7. Criar controllers e apresentadores na camada de apresentação que usam casos de uso
8. Garantir que nenhuma dependência atravesse para fora (respeitar a Dependency Rule)
9. Usar injeção de dependência para fornecer implementações concretas às interfaces

### Quais são as alternativas?
- arquitetura em camadas tradicional (Layered Architecture)
- Clean Architecture
- Hexagonal Architecture / Ports and Adapters
- arquitetura monolítica não estruturada
- Microservices
- arquitetura baseada em eventos

### Quais são os trade-offs?
**Vantagens da Onion Architecture:**
- Independência de infraestrutura: regras de negócio não dependem de bancos de dados, frameworks ou UI específicos
- Testabilidade: camadas de domínio e aplicação podem ser testadas sem infraestrutura
- Independência de UI: pode mudar interface sem mudar regras de negócio
- Independência de banco de dados: pode trocar de Oracle para MongoDB ou até para arquivos sem mudar regras de negócio
- Clareza de separação de preocupações: cada camada tem responsabilidade bem definida
- Fácil de entender e manter devido à estrutura clara e familiar de camadas
- Boa transição para equipes vindo de arquitetura em camadas tradicional

**Desvantagens da Onion Architecture:**
- Pode confundir com arquitetura em camadas tradicional se não houver aderência rígida à Dependency Rule
- Sobrehead inicial de criação de interfaces e camadas
- Requer disciplina da equipe para não violar a Dependency Rule
- Pode haver mais classes e interfaces do que em arquiteturas mais simples
- A curva de aprendizado pode existir para desenvolvedores unfamiliarizados com o conceito de dependências apenas para dentro
- Em alguns casos, pode parecer muito semelhante à Clean Architecture sem benefícios adicionais claros

### Quando usar?
- Sistemas onde a vida útil é esperada para ser longa (anos ou décadas)
- Sistemas onde regras de negócio são complexas e valiosas por si mesmas
- Quando se quer garantir que o núcleo do negócio seja facilmente testável
- Quando se antecipa necessidade de mudar tecnologias (framework, banco de dados, UI)
- Quando se quer maximizar a reutilização de regras de negócio em diferentes contextos
- Sistemas críticos onde falhas de regras de negócio seriam catastróficas
- Quando a equipe valoriza limpeza e manutenibilidade a longo prazo sobre velocidade inicial
- Quando se está migrando de arquitetura em camadas tradicional e quer melhor separação de preocupações

### Quando não usar?
- Protótipos ou provas de conceito onde velocidade de entrega é a única prioridade
- Aplicações muito simples onde o overhead não traz benefício proporcional
- Equipes que rejeitam fortemente a ideia de camadas e interfaces adicionais
- Quando se está em um ambiente altamente restrito onde cada classe conta (sistemas embarcados extremos)
- Quando se vai descartar o sistema após uso único ou muito limitado
- Quando se vai construir um sistema onde a tecnologia é fixa para sempre e nunca mudará

### Quais são os erros mais comuns?
- Violentar a Dependency Rule (ter entidades que dependem de infraestrutura)
- Colocar regras de negócio na camada de infraestrutura ou apresentação
- Fazer casos de uso saberem detalhes específicos de apresentação ou banco de dados
- Não definir claramente o que é domínio versus aplicação versus infraestrutura
- Usar entidades como estruturas de dados anêmicas (apenas getters/setters sem comportamento)
- Criar camadas desnecessariamente quando uma arquitetura mais simples seria suficiente
- Esquecer de injetar dependências e fazer instantiation direto nas camadas internas
- Acreditar que Onion Architecture significa "nenhuma infraestrutura" em vez de "não depender de detalhes de infraestrutura"

### Como isso afeta:
- *performance:* Ligeiramente pior devido a indireção e chamadas de interface (geralmente insignificante em aplicações de negócio)
- *escalabilidade:* Similar a outras arquiteturas; escalabilidade depende de implementação específica
- *disponibilidade:* Similar a outras arquiteturas; depende de como é deployada
- *consistência:* Excelente para manter consistência de regras de negócio devido ao isolamento
- *segurança:* Similar a outras arquiteturas; segurança é uma preocupação transversal
- *custo:* Custo inicial de desenvolvimento pode ser maior, mas custo de manutenção a longo prazo tende a ser menor
- *observabilidade:* Similar a outras arquiteturas; pode ser instrumentada normalmente
- *complexidade operacional:* Similar a outras arquiteturas bem estruturadas

### Exemplos reais de aplicação
- Sistemas de finanças onde regras de cálculo são críticas e duram décadas
- Sistemas de saúde onde regras de negócio clínico devem ser preservados independentemente de tecnologia
- Sistemas de controle industrial onde lógica de controle deve ser isolada de detalhes de hardware
- Muitos sistemas empresariais que evoluíram de arquiteturas em camadas para melhor separação de preocupações
- Aplicações onde houve necessidade de mudar de tecnologia (ex: de WebForms para MVC, de Swing para JavaFX) sem reescrever regras de negócio

### Exemplo simplificado
Estrutura básica de Onion Architecture para um sistema de gerenciamento de tarefas:
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
│   │               │   ├── repository/
│   │               │   │   └── TaskRepository.java (interface)
│   │               │   └── service/
│   │               │       └── TaskDomainService.java (regras de negócio de domínio)
│   │               ├── application/
│   │               │   ├── dto/
│   │               │   │   ├── TaskDto.java
│   │               │   │   ├── CreateTaskDto.java
│   │               │   │   └── UpdateTaskDto.java
│   │               │   ├── service/
│   │               │   │   ├── CreateTaskUseCase.java
│   │               │   │   ├── GetTaskUseCase.java
│   │               │   │   ├── UpdateTaskUseCase.java
│   │               │   │   └── DeleteTaskUseCase.java
│   │               │   └── exception/
│   │               │       └── TaskNotFoundException.java
│   │               ├── infrastructure/
│   │               │   ├── persistence/
│   │               │   │   ├── TaskRepositoryImpl.java (implementação JPA)
│   │               │   │   └── DatabaseConfig.java
│   │               │   ├── messaging/
│   │               │   │   └── NotificationServiceImpl.java (implementação email/SMS)
│   │               │   └── external/
│   │               │       └── PaymentGatewayImpl.java (integração com gateway de pagamento)
│   │               └── presentation/
│   │                   ├── web/
│   │                   │   ├── TaskController.java
│   │                   │   └── TaskPresenter.java
│   │                   ├── rest/
│   │                   │   └── TaskRestController.java
│   │                   └── cli/
│   │                       └── TaskCliCommand.java
│   └── resources/
│       └── application.properties
└── test/
    ├── domain/
    │   └── ... (testes de entidades e serviços de domínio)
    ├── application/
    │   └── ... (testes de casos de uso)
    └── infrastructure/
        └── ... (testes de implementações de infraestrutura)
```

### Exemplo de sistema de produção
Um sistema de processamento de seguros para seguradora:
- **Domain Layer:**
  - Entities: Apolice, Sinistro, Cliente, BemSegurado (com regras de negócio como cálculo de prêmio, validação de cobertura, avaliação de sinistro)
  - Repository Interfaces: ApoliceRepository, SinistroRepository, ClienteRepository
  - Domain Services: PremiumCalculationService (regras complexas de cálculo de prêmio), RiskAssessmentService (avaliação de risco)
- **Application Layer:**
  - DTOs: ApoliceDto, SinistroDto, CreateApoliceDto, ProcessSinistroDto
  - Use Cases: EmitirApoliceUseCase, RenovarApoliceUseCase, ProcessarSinistroUseCase, ConsultarApoliceUseCase
  - Application Services: NotificationService (interface para envio de comunicações)
- **Infrastructure Layer:**
  - Persistence: JPA implementations of repository interfaces using Hibernate with PostgreSQL
  - Messaging: Implementações de NotificationService usando SMTP, SMS gateway e push notification
  - External: Integração com serviços de crédito (Serasa, Boa Vista), serviços de vistoria externa
  - Security: Implementação de autenticação e autorização usando Spring Security ou JWT
- **Presentation Layer:**
  - Web: Controllers MVC para interface de seguradora e corretores
  - REST: Controllers para API consumida por aplicativos móveis e sistemas parceiros
  - CLI: Ferramentas de linha de comando para operações administrativas e relatórios
- Testes de domínio e aplicação rodam sem nenhum framework de infraestrutura
- Testes de infraestrutura testam as implementações específicas (JPA, HTTP clients, etc.)
- Deploy como aplicação web WAR ou múltiplos microservices dependendo do escopo

### Como esse assunto pode aparecer em uma entrevista.
> 🎯 **ENTREVISTA — ALTA FREQUÊNCIA**
> 
> "Explique como você aplicaria os princípios da Onion Architecture a um sistema de gestão escolar que precisa lidar com matrículas, notas, frequência e comunicação com pais."
> 
> **Armadilha:** Focar apenas na estrutura de pastas sem explicar como as dependências apontam apenas para dentro.
> 
> **Como raciocinar:** Descrever o domínio contendo entidades como Aluno, Turma, Disciplina, Nota com regras de negócio (validação de pré-requisitos, cálculo de média, políticas de frequência). Mostrar como interfaces de repositório ficam no domínio, enquanto implementações JPA ficam na infraestrutura. Explicar como casos de uso na aplicação orquestram entidades para operações como matricular aluno, lançar nota, calcular frequência. Demonstrar como controllers na apresentação usam apenas casos de uso, nunca acessando repositórios ou entidades diretamente.

## Domain Layer (Camada de Domínio)

> 🎯 **ENTREVISTA — ALTA FREQUÊNCIA**
> 
> A camada de domínio contém o coração do sistema - entidades de negócio e regras de negócio puro; entrevistadores querem ver se você entende o que pertence verdadeiramente ao domínio.

### definição
A Domain Layer contém entidades de negócio que representam conceitos fundamentais do negócio, interfaces de repositório que definem contratos de persistência, e serviços de domínio que encapsulam regras de negócio que não pertencem naturalmente a uma única entidade. Esta é a camada mais interna e mais abstrata do sistema.

### Por que existe?
Para isolar o que é verdadeiramente fundamental sobre o negócio dos detalhes específicos de como ele é implementado em uma aplicação particular, tornando as regras de negócio reutilizáveis, testáveis e independentes de tecnologia.

### Como funciona internamente?
- Entidades: objetos com identidade, estado e comportamento que representam conceitos de negócio
- Interfaces de Repositório: definem operações de persistência (salvar, buscar, deletar) sem especificar como são implementadas
- Serviços de Domínio: encapsulam regras de negócio que envolvem múltiplas entidades ou não pertencem naturalmente a uma entidade específica
- Objetos de Valor: objetos imutáveis que representam conceitos de negócio sem identidade (ex: Money, Endereço)
- Eventos de Domínio: representam algo que aconteceu no domínio que outras partes podem estar interessadas
- Nenhuma dependência em frameworks específicos, bancos de dados ou UI
- Todas as dependências apontam para dentro ou não existem (domínio depende apenas de si mesmo)

### Como implementar?
1. Identificar conceitos de negócio que são verdadeiros independentemente do contexto de aplicação
2. Definir entidades com identidade, estado e comportamento (regras de negócio intrínsecos)
3. Definir objetos de valor para conceitos imutáveis sem identidade
4. Definir eventos de domínio para representar ocorrências significativas
5. Definir interfaces de repositório que representam necessidades de persistência do ponto de vista do domínio
6. Definir serviços de domínio para regras de negócio que cruzam entidades ou são complexas demais para uma entidade
7. Manter foco no que o conceito de negócio é, não em como é armazenado, recuperado ou exibido
8. Nunca incluir anotações de framework ou dependências específicas de tecnologia
9. Garantir que entidades sejam responsáveis por validar seu próprio estado
10. Usar linguagem ubíqua do domínio nos nomes de classes, métodos e atributos

### Quais são as alternativas?
- Estruturas de dados anêmicas (apenas getters/setters sem comportamento)
- Entidades que são apenas mapeamentos diretos de tabelas de banco de dados
- Funções puras que operam em estruturas de dados
- Classes de serviço que contém toda a lógica (misturando domínio e aplicação)
- Tabelas de banco de dados ou estruturas de dados puras como fonte da verdade

### Quais são os trade-offs?
**Vantagens de domínio bem definido:**
- Regras de negócio ficam localizadas onde os dados residem (princípio do especialista em informações)
- Menos probabilidade de regras de negócio serem duplicadas ou inconsistentes
- Mais fácil de entender e manter regras de negócio relacionadas a um conceito
- Entidades podem ser reutilizadas em diferentes contextos de aplicação
- Testabilidade melhorada pois regras estão isoladas com os dados que as afetam
- Clareza sobre o que constitui verdadeiro negócio vs detalhes de aplicação

**Desvantagens/custos:**
- Pode parecer estranho para desenvolvedores acostumados com modelo anêmico ou de serviços
- Risco de entidades ficarem muito grandes se não houver bom limite de responsabilidade
- Pode haver tensão entre pureza de domínio e necessidade de eficiência em alguns casos
- Requer mudança de mentalidade para desenvolvedores acostumados com camadas tradicionais
- Pode haver debate sobre o que pertence ao domínio vs aplicação

### Quando usar?
- Sempre que houver um conceito de negócio com regras intrínsecas que não dependem de como é usado
- Quando se quer garantir que regras de negócio críticas não sejam espalhadas pelo código
- Quando múltiplas partes do sistema precisam usar o mesmo conceito de negócio com consistência
- Quando se quer maximizar a coesão e minimizar o acoplamento relacionado a conceitos de negócio
- Quando se quer facilitar o teste de regras de negócio isoladamente
- Quando se está construindo um sistema onde o negócio é o ativo mais valioso

### Quando não usar?
- Quando o conceito é puramente estrutural sem regras de negócio intrínsecas (ex: simples registro de log)
- Quando se está em um contexto onde desempenho crítico exige acesso direto a estruturas de dados
- Quando o conceito é tão simples que comportamento adicionaria complexidade desnecessária
- Quando se está construindo uma camada de apresentação pura onde o conceito é apenas para exibição
- Quando se está trabalhando com dados que são puramente de entrada/saída sem significado de negócio intrínseco
- Quando o overhead de modelagem de domínio não traz benefício proporcional ao valor do negócio

### Quais são os erros mais comuns?
- Fazer entidades anêmicas (apenas dados com getters/setters, toda lógica em serviços)
- Colocar regras de negócio de aplicação nas entidades (regras que são específicas de como é usado neste sistema)
- Fazer entidades dependentes de frameworks (anotações JPA, etc.)
- Expor estado interno demais através de getters/setters públicos
- Não encapsular validação de estado (permitir que entidades fiquem em estado inconsistente)
- Fazer entidades imutáveis quando precisam mudar estado como parte de suas regras de negócio
- Confundir entidades com tabelas de banco de dados ou estruturas de armazenamento
- Colocar lógica de aplicação nos serviços de domínio (deve ficar apenas regras de negócio puro)

### Como isso afeta:
- *performance:* Impacto mínimo; chamada de método vs acesso direto ao campo (geralmente insignificante)
- *escalabilidade:* Nenhum impacto direto
- *disponibilidade:* Nenhum impacto direto
- *consistência:* Melhora significativamente pois entidades garantem seu próprio estado válido
- *segurança:* Melhora pois entidades podem aplicar validação e controle de acesso intrínseco
- *custo:* Similar; foco em onde a lógica reside ao invés de adicionar ou remover código
- *observabilidade:* Melhora pois mudanças de estado ocorrem através de métodos bem definidos
- *complexidade operacional:* Similar; pode reduzir bugs devido a melhor encapsulamento

### Exemplos reais de aplicação
- Entidade Money em sistema financeiro que conhece sua moeda e pode realizar conversões e operações aritméticas seguras
- Entidade Range que representa um intervalo e sabe se contém um valor, se sobrepõe com outro range, etc.
- Entidade Account que conhece regras de saque, depósito e transferência com validação de limites
- Entidade que representa uma data e hora com fuso horário que sabe fazer conversões e operações de calendário
- Entidade que representa um endereço postal que sabe validar CEP, formatar para envelope, etc.
- Serviço de domínio que calcula juros compostos considerando diferentes períodos e taxas
- Serviço de domínio que valida se uma apólice de seguro está dentro das regras de subscrição

### Exemplo simplificado
Entidade anêmica (errada):
```java
// ❌ ERRADO: Apenas dados, toda lógica ficaria em serviços ou casos de uso
public class Order {
    private Long id;
    private String customerId;
    private BigDecimal amount;
    private LocalDateTime orderDate;
    private OrderStatus status;
    
    // Getters e setters públicos
    public Long getId() { return id; }
    public void setId(Long id) { this.id = id; }
    public String getCustomerId() { return customerId; }
    public void setCustomerId(String customerId) { this.customerId = customerId; }
    public BigDecimal getAmount() { return amount; }
    public void setAmount(BigDecimal amount) { this.amount = amount; }
    public LocalDateTime getOrderDate() { return orderDate; }
    public void setOrderDate(LocalDateTime orderDate) { this.orderDate = orderDate; }
    public OrderStatus getStatus() { return status; }
    public void setStatus(OrderStatus status) { this.status = status; }
}
```

Entidade com comportamento (correta):
```java
// ✅ CORRETO: Contém regras de negócio intrínsecas
public class Order {
    private Long id;
    private String customerId;
    private BigDecimal amount;
    private LocalDateTime orderDate;
    private OrderStatus status;
    private final List<OrderItem> items;
    
    public Order(Long id, String customerId, LocalDateTime orderDate) {
        this.id = id;
        this.customerId = customerId;
        this.orderDate = orderDate;
        this.status = OrderStatus.PENDING;
        this.items = new ArrayList<>();
        validate(); // Validação no construtor
    }
    
    // Métodos que representam regras de negócio
    public void addItem(OrderItem item) {
        if (status != OrderStatus.PENDING) {
            throw new IllegalStateException("Não é possível adicionar itens a ordem não pendente");
        }
        items.add(item);
        // Recalcula total automaticamente (regra de negócio)
    }
    
    public void removeItem(OrderItem item) {
        if (status != OrderStatus.PENDING) {
            throw new IllegalStateException("Não é possível remover itens de ordem não pendente");
        }
        items.remove(item);
    }
    
    public BigDecimal calculateTotal() {
        // Regra de negócio: total é soma dos itens mais impostos
        BigDecimal subtotal = items.stream()
                                  .map(OrderItem::getTotal)
                                  .reduce(BigDecimal.ZERO, BigDecimal::add);
        // Aplicar taxa de imposto (ex: 10%)
        return subtotal.multiply(new BigDecimal("1.10"));
    }
    
    public void confirm() {
        if (status != OrderStatus.PENDING) {
            throw new IllegalStateException("Somente ordens pendentes podem ser confirmadas");
        }
        if (items.isEmpty()) {
            throw new IllegalStateException("Ordem deve ter pelo menos um item");
        }
        this.status = OrderStatus.CONFIRMED;
    }
    
    public void cancel() {
        if (status == OrderStatus.DELIVERED) {
            throw new IllegalStateException("Não é possível cancelar ordem já entregue");
        }
        this.status = OrderStatus.CANCELLED;
    }
    
    // Validação intrínseca
    private void validate() {
        if (customerId == null || customerId.trim().isEmpty()) {
            throw new IllegalArgumentException("ID do cliente não pode ser vazio");
        }
        if (orderDate == null) {
            throw new IllegalArgumentException("Data da ordem não pode ser nula");
        }
        if (orderDate.isAfter(LocalDateTime.now().plusYears(1))) {
            throw new IllegalArgumentException("Data da ordem não pode ser mais de um ano no futuro");
        }
    }
    
    // Getters apenas (não setters públicos para preservar encapsulamento)
    public Long getId() { return id; }
    public String getCustomerId() { return customerId; }
    public LocalDateTime getOrderDate() { return orderDate; }
    public OrderStatus getStatus() { return status; }
    public List<OrderItem> getItems() { return Collections.unmodifiableList(items); }
    public BigDecimal getAmount() { return calculateTotal(); } // Derivado, não armazenado
}
```

Serviço de domínio com regras de negócio complexas:
```java
// ✅ CORRETO: Serviço de domínio com regras que envolvem múltiplas entidades
public class PricingDomainService {
    private final TaxRateProvider taxRateProvider; // Pode ser interface ou valor fixo
    
    public PricingDomainService(TaxRateProvider taxRateProvider) {
        this.taxRateProvider = taxRateProvider;
    }
    
    public BigDecimal calculateFinalPrice(Order order, Customer customer) {
        // Regra de negócio complexa que envolve ordem, cliente e taxas
        BigDecimal baseAmount = order.calculateTotal();
        
        // Desconto baseado no tipo de cliente
        BigDecimal discount = BigDecimal.ZERO;
        if (customer.isPremium()) {
            discount = baseAmount.multiply(new BigDecimal("0.15")); // 15% off para premium
        } else if (customer.getLifetimeValue().compareTo(new BigDecimal("10000")) > 0) {
            discount = baseAmount.multiply(new BigDecimal("0.08")); // 8% off para alto valor
        }
        
        BigDecimal discountedAmount = baseAmount.subtract(discount);
        
        // Aplicar taxa baseada na localização
        BigDecimal taxRate = taxRateProvider.getTaxRateFor(customer.getAddress());
        BigDecimal taxAmount = discountedAmount.multiply(taxRate);
        
        return discountedAmount.add(taxAmount);
    }
}
```

### Exemplo de sistema de produção
Sistema de gestão de clínica médica:
- **Entidades de Domínio:**
  - Paciente: contém regras como validação de CPF, cálculo de idade, verificação de convenio válido
  - Médico: contém regras como validação de CRM, especialidades válidas, carga horária máxima
  - Consulta: contém regras como validação de horário, duração baseada no tipo, conflito de agendas
  - Prontuário: contém regras como segredo médico, retenção por tempo mínimo, acesso restrito
  - Exame: contém regras como preparo necessário, janela de validade do resultado, interpretação
- **Interfaces de Repositório:**
  - PacienteRepository: operações de buscar, salvar, atualizar paciente
  - MedicoRepository: operações para gerenciar profissionais
  - ConsultaRepository: operações para agendar, cancelar, buscar consultas
  - ProntuarioRepository: operações para gerenciar registros clínicos
- **Serviços de Domínio:**
  - ValidaçãoAgendaService: verifica conflitos de horário, considerando tipo de consulta e disponibilidade do médico
  - CodificacaoProcedimentoService: aplica regras da tabela SUS ou demais para gerar códigos de procedimento
  - InterpretacaoResultadoService: interpreta resultados de exames considerando referência, idade, sexo
  - SegurancaInformacaoService: aplica regras de LGPD e sigilo médico ao acesso a informações
- **Objetos de Valor:**
  - Cpf: validação e formatação de CPF brasileiro
  - Crm: validação e formatação de CRM médico por estado
  - Dinheiro: valor com moeda, operações aritméticas seguras
  - Periodo: intervalo de data/hora com operações de interseção e união

### Como esse assunto pode aparecer em uma entrevista.
> 🎯 **ENTREVISTA — ALTA FREQUÊNCIA**
> 
> "Descreva como você modelaria o conceito de 'Política de Seguro' como uma entidade de domínio em um sistema de gestão de seguradora."
> 
> **Armadilha:** Fazer uma classe apenas com campos como numero, valor, segurado e deixar toda a lógica de cálculo de prêmio, verificação de cobertura e processamento de sinistro em serviços de aplicação ou infraestrutura.
> 
> **Como raciocinar:** Definir a classe Apolice com métodos que representam regras de negócio intrínsecos: `calcularPremio()` que considera fatores de risco, idade, histórico; `verificarCobertura(Sinistro sinistro)` que verifica se o sinistro está dentro dos termos da apólice; `processarRenovacao()` que aplica regras de reajuste e verifica condições para renovação; `estaVigente(LocalDate data)` que verifica se a apólice está ativa em uma data específica. Incluir validação no construtor e métodos controlados para garantir que a entidade sempre esteja em estado válido conforme regras de negócio do seguro.

## Application Layer (Camada de Aplicação)

> 🎯 **ENTREVISTA — ALTA FREQUÊNCIA**
> 
> A camada de aplicação contém casos de uso que orquestram o domínio para atender aos requisitos funcionais específicos da aplicação; entrevistadores querem ver se você entende a diferença entre domínio (regras gerais) e aplicação (regras específicas deste sistema).

### definição
A Application Layer contém casos de uso (ou interators) que implementam os requisitos funcionais específicos desta aplicação, orquestrando entidades e serviços de domínio para atingir objetivos de negócio específicos. Também contém DTOs (Data Transfer Objects) para transferir dados entre camadas e interfaces de serviço que definem contratos para operações que o domínio precisa do mundo exterior (como notificação, geração de relatórios, etc.).

### Por que existe?
Para separar o que é específico de como este sistema particular usa o domínio das regras que são verdadeiras independentemente do contexto. Esta camada contém o "como" nós fazemos as coisas neste sistema específico, enquanto o domínio contém o "o que" é verdadeiramente fundamental sobre o negócio.

### Como funciona internamente?
- Casos de Uso: orquestram chamadas entre entidades e serviços de domínio para alcançar um objetivo de negócio específico desta aplicação
- DTOs: objetos simples usados para transferir dados entre camadas, normalmente sem comportamento
- Interfaces de Serviço: definem contratos para operações que o domínio precisa do mundo exterior (ex: EnviarEmailService, GerarRelatorioService)
- Orquestram entidades e serviços de domínio aplicando regras de negócio de aplicação
- Não contêm regras de negócio de domínio (isso fica no domínio)
- Não contêm detalhes de apresentação ou infraestrutura (isso fica nas camadas externas)
- Geralmente têm uma entrada (request) e uma saída (response) bem definidas
- Dependem apenas de domínio e de interfaces (repositórios, serviços de domínio, serviços de aplicação)

### Como implementar?
1. Identificar os principais objetivos de negócio que este sistema específico precisa atender (casos de uso)
2. Para cada caso de uso, definir uma classe com método execute() que implementa a lógica
3. Definir claramente os dados de entrada (request) e saída (response) que o caso de uso consome e produz (usualmente DTOs)
4. Dependem apenas de entidades, serviços de domínio e interfaces da camada de domínio
5. Orquestram domínio para aplicar regras de negócio de aplicação e alcançar o objetivo
6. Tratam erros e exceções de acordo com regras de negócio de aplicação
7. Não sabem como os dados serão apresentados ou armazenados (isso é responsabilidade das camadas externas)
8. Podem ser implementados como objetos imutáveis com dependências injetadas via construtor
9. Nunca contêm lógica de domínio puro (deve ficar no domínio)
10. Nunca contêm detalhes de tecnologia específicos de apresentação ou infraestrutura

### Quais são as alternativas?
- Colocar toda a lógica nos controladores ou apresentadores (misturando aplicação e apresentação)
- Fazer domínio responsável por toda a lógica (overcarregando domínio com regras específicas da aplicação)
- Colocar lógica em serviços genéricos sem clara associação a objetivos de negócio
- Usar scripts ou procedimentos sem estrutura orientada a objetos
- Misturar lógica de aplicação com detalhes de infraestrutura

### Quais são os trade-offs?
**Vantagens de aplicação bem definida:**
- Clareza sobre o que este sistema específico faz em termos de comportamento de negócio
- Facilidade de testar regras de negócio de aplicação em isolamento do domínio
- Independência de detalhes de apresentação e infraestrutura
- Facilidade de reutilizar em diferentes contextos se interfaces forem estáveis (web, mobile, linha de comando)
- Melhor compreensão do fluxo de negócio através da nomeação clara dos casos de uso
- Facilidade de mudar como algo é feito sem mudar o que é feito (desde que entrada/saída permaneçam semelhantes)
- Separação clara entre regras de negócio gerais (domínio) e específicas desta aplicação

**Desvantagens/custos:**
- Sobrehead de criação de classes adicionais
- Pode parecer indireto para fluxos muito simples
- Requer disciplina para não vazar detalhes de domínio ou externos nos casos de uso
- Pode haver sobreposição ou lacunas se não forem bem delimitados
- Necessidade de definir claramente entrada e saída para cada caso de uso
- Risco de ficar muito parecido com serviço de aplicação genérico se não houver foco claro em objetivos de negócio

### Quando usar?
- Sempre que houver um objetivo de negócio claro que envolve múltiplas entidades ou regras de coordenação específicas desta aplicação
- Quando se quer separar claramente regras de negócio de aplicação das regras de negócio geral (domínio)
- Quando se quer garantir que o núcleo do sistema seja testável sem UI ou infraestrutura
- Quando múltiplas interfaces (web, API, linha de comando) precisam acessar a mesma lógica de negócio de aplicação
- Quando se quer facilitar a compreensão e manutenção do sistema através de nomes claros de funcionalidades específicas

### Quando não usar?
- Quando o sistema é tão simples que cada ação é uma operação direta em uma entidade ou serviço de domínio
- Quando se está construindo uma camada de apresentação pura onde não há lógica de negócio de aplicação
- Quando o overhead de classes e interfaces não traz benefício proporcional ao tamanho do sistema
- Quando se está em um contexto onde desempenho crítico exige código inline sem abstração
- Quando se está prototipando e velocidade é a única prioridade

### Quais são os erros mais comuns?
- Casos de uso que conhecem detalhes de apresentação (ex: uso de HttpServletRequest, ModelAndView)
- Casos de uso que conhecem detalhes de infraestrutura (ex: uso direto de EntityManager ou JDBC)
- Casos de uso que contêm lógica de apresentação (formatação de dados para exibição)
- Falta de clara delimitação entre o que é caso de uso e o que é domínio
- Casos de uso que são muito grandes e fazem demasiado (violando princípio da responsabilidade única)
- Casos de uso que dependem de implementações concretas em vez de interfaces
- Não tratar adequadamente casos de erro ou borda de acordo com regras de negócio de aplicação
- Colocar regras de negócio de domínio nos casos de uso (deve ficar apenas no domínio)

### Como isso afeta:
- *performance:* Impacto mínimogeralmente <1% devido a chamada de método indireto
- *escalabilidade:* Nenhum impacto direto
- *disponibilidade:* Nenhum impacto direto
- *consistência:* Melhora pois regras de negócio de aplicação ficam localizadas e menos propensas a inconsistência
- *segurança:* Similar; casos de uso podem aplicar validações e controle de acesso de negócio de aplicação
- *custo:* Similar; foco em onde a lógica de aplicação reside
- *observabilidade:* Melhora pois pontos claros de lógica de negócio facilitam logging e tracing
- *complexidade operacional:* Similar; pode reduzir bugs devido a melhor separação de responsabilidades

### Exemplos reais de aplicação
- Caso de uso "Processar Reembolso" em sistema de saúde que:
  - Valida documentação necessária
  - Verifica limite de reembolso baseado no plano
  - Calcula valor com base na tabela de procedimentos
  - Aplica regras de franquia e coparticipação
  - Gera ordem de pagamento
  - Notifica beneficiário
- Caso de uso "Gerar Boletim" em sistema escolar que:
  - Busca frequência e notas do aluno
  - Aplica regras de aprovação e recuperação específica da instituição
  - Calcula média ponderada se necessário
  - Determina situação final
  - Formata dados para impressão ou envio digital
- Caso de uso "Aprovar Empréstimo" em sistema financeiro que:
  - Verifica score de crédito mínimo
  - Calcula capacidade de pagamento baseado em renda e despesas
  - Aplica políticas de limite baseado no relacionamento
  - Estrutura parcelas e taxas
  - Gera contrato
  - Inicia processo de desembolso

### Exemplo simplificado
Caso de uso sem independência (errado):
```java
// ❌ ERRADO: Caso de uso conhece detalhes de apresentação e infraestrutura
@Service
public class OrderService {
    @PersistenceContext
    private EntityManager em;
    
    @Transactional
    public OrderResponse placeOrder(HttpServletRequest request, Model model) {
        String customerId = request.getParameter("customerId");
        String itemsJson = request.getParameter("items");
        
        // Lógica de apresentação misturada com negócio
        if (customerId == null || customerId.isEmpty()) {
            model.addAttribute("error", "Cliente é obrigatório");
            return null;
        }
        
        // Parse manual de JSON - lógica que poderia ser em DTO ou domínio
        List<OrderItemDto> items = parseItemsJson(itemsJson);
        
        Customer customer = em.find(Customer.class, Long.parseLong(customerId));
        if (customer == null) {
            model.addAttribute("error", "Cliente não encontrado");
            return null;
        }
        
        Order order = new Order(null, customerId, LocalDateTime.now());
        for (OrderItemDto itemDto : items) {
            OrderItem item = new OrderItem(
                itemDto.getProductId(),
                itemDto.getQuantity(),
                itemDto.getUnitPrice()
            );
            order.addItem(item);
        }
        
        em.persist(order);
        
        // Lógica de navegação misturada
        model.addAttribute("order", order);
        
        // Retornando entidade diretamente (vazando detalhes de infraestrutura)
        return new OrderResponse(
            order.getId(),
            order.getCustomerId(),
            order.getOrderDate(),
            order.getStatus(),
            order.getItems().stream()
                           .map(i -> new OrderItemDto(
                               i.getProductId(),
                               i.getQuantity(),
                               i.getUnitPrice()
                           ))
                           .collect(Collectors.toList())
        );
    }
}
```

Caso de uso com independência (correto):
```java
// ✅ CORRETO: Caso de uso depende apenas de domínio e interfaces
@Service
@RequiredArgsConstructor
public class PlaceOrderUseCase {
    private final CustomerRepository customerRepository;
    private final OrderRepository orderRepository;
    private final EmailService emailService; // Interface de serviço de aplicação
    
    public OrderResponse execute(PlaceOrderCommand command) {
        // Validação de negócio de aplicação
        Customer customer = customerRepository.findById(command.getCustomerId())
            .orElseThrow(() -> new IllegalArgumentException("Cliente não encontrado"));
        
        if (command.getItems() == null || command.getItems().isEmpty()) {
            throw new IllegalArgumentException("Ordem deve ter pelo menos um item");
        }
        
        // Orquestração de domínio usando apenas regras de negócio
        Order order = new Order(null, customer.getId(), LocalDateTime.now());
        for (OrderItemDto itemDto : command.getItems()) {
            OrderItem item = new OrderItem(
                itemDto.getProductId(),
                itemDto.getQuantity(),
                itemDto.getUnitPrice()
            );
            order.addItem(item); // Aplica regras de negócio de domínio
        }
        
        Order savedOrder = orderRepository.save(order);
        
        // Aplica regras de negócio de aplicação (ex: enviar confirmação)
        emailService.sendOrderConfirmation(savedOrder);
        
        // Retorna apenas dados de negócio, não detalhes de apresentação ou infraestrutura
        return new OrderResponse(
            savedOrder.getId(),
            savedOrder.getCustomerId(),
            savedOrder.getOrderDate(),
            savedOrder.getStatus(),
            savedOrder.getItems().stream()
                                  .map(item -> new OrderItemDto(
                                      item.getProductId(),
                                      item.getQuantity(),
                                      item.getUnitPrice()
                                  ))
                                  .collect(Collectors.toList())
        );
    }
}

// Comandos e DTOs específicos de aplicação (contêm apenas dados)
record PlaceOrderCommand(String customerId, List<OrderItemDto> items) {
    public PlaceOrderCommand {
        if (customerId == null || customerId.trim().isEmpty()) {
            throw new IllegalArgumentException("ID do cliente não pode ser vazio");
        }
        if (items == null || items.isEmpty()) {
            throw new IllegalArgumentException("Lista de itens não pode ser vazia ou nula");
        }
        for (OrderItemDto item : items) {
            if (item.getQuantity() <= 0) {
                throw new IllegalArgumentException("Quantidade deve ser positiva");
            }
            if (item.getUnitPrice().compareTo(BigDecimal.ZERO) < 0) {
                throw new IllegalArgumentException("Preço unitário não pode ser negativo");
            }
        }
    }
}

record OrderItemDto(Long productId, int quantity, BigDecimal unitPrice) {
    public OrderItemDto {
        if (productId == null || productId <= 0) {
            throw new IllegalArgumentException("ID do produto deve ser válido");
        }
        if (quantity <= 0) {
            throw new IllegalArgumentException("Quantidade deve ser positiva");
        }
        if (unitPrice.compareTo(BigDecimal.ZERO) < 0) {
            throw new IllegalArgumentException("Preço unitário não pode ser negativo");
        }
    }
}

record OrderResponse(Long id, String customerId, LocalDateTime orderDate, OrderStatus status, List<OrderItemDto> items) {}
```

Serviço de aplicação (interface no domínio, implementação na infraestrutura):
```java
// Interface na camada de domínio - define o que o domínio precisa
public interface EmailService {
    void sendOrderConfirmation(Order order);
    void sendShippingUpdate(Order order, ShippingUpdate update);
    void sendPaymentReceipt(Order order, PaymentReceipt receipt);
}

// Implementação na camada de infraestrutura - como fazer com tecnologia específica
@Service
@RequiredArgsConstructor
public class EmailServiceImpl implements EmailService {
    private final JavaMailSender mailSender;
    private final TemplateEngine templateEngine;
    
    @Override
    public void sendOrderConfirmation(Order order) {
        // Detalhes específicos de tecnologia JavaMail e Thymeleaf
        Context context = new Context();
        context.setVariable("order", order);
        String htmlContent = templateEngine.process("order-confirmation", context);
        
        MimeMessage message = mailSender.createMimeMessage();
        try {
            MimeMessageHelper helper = new MimeMessageHelper(message, true, "UTF-8");
            helper.setTo(order.getCustomer().getEmail());
            helper.setSubject("Confirmação de Pedido #" + order.getId());
            helper.setText(htmlContent, true);
            mailSender.send(message);
        } catch (MessagingException e) {
            throw new RuntimeException("Falha ao enviar email de confirmação", e);
        }
    }
}
```

### Exemplo de sistema de produção
Sistema de gestão de clínica médica:
- **Casos de Uso na Camada de Aplicação:**
  - AgendarConsultaUseCase: orquestra validação de horário, disponibilidade do médico, criação de consulta
  - DarAltaPacienteUseCase: verifica regras de alta, gera receitas, atualiza status do paciente
  - ProcessarPagamentoConsultaUseCase: aplica regras de coparticipação, processa pagamento, emite recibo
  - GerarProntuarioUseCase: coleta dados da consulta, aplica regras de segredo médico, formata para entrega
  - SolicitarExameUseCase: verifica necessidade clínica, escolhe tipo de exame, instrui preparo do paciente
- **DTOs:**
  - ConsultaDto: dados essenciais para transferência entre camadas
  - PacienteDto: informações básicas do paciente para exibição ou integração
  - MedicoDto: dados do médico para escalonamento ou consulta
  - FichaConsultaDto: dados específicos para preenchimento durante consulta
- **Interfaces de Serviço:**
  - ProntuarioService: interface para gerar, armazenar e recuperar prontuários
  - ComunicacaoService: interface para enviar lembretes, confirmações e alertas
  - PagamentoService: interface para processar diferentes formas de pagamento
  - IntegracaoLaboratorioService: interface para solicitar exames e receber resultados de laboratórios externos
- **Implementações de Serviço (na Infraestrutura):**
  - ProntuarioServiceImpl: usa sistema de gestão de documentos ou banco de dados específico
  - ComunicacaoServiceImpl: integra com serviços de SMS, email e push notification
  - PagamentoServiceImpl: integra com gateways de pagamento como PagSeguro, Stripe ou boleto bancário
  - IntegracaoLaboratorioServiceImpl: usa HL7 ou APIs específicas de laboratórios parceiros

### Como esse assunto pode aparecer em uma entrevista.
> 🎯 **ENTREVISTA — ALTA FREQUÊNCIA**
> 
> "Explique a diferença entre uma entidade de domínio e um caso de uso de aplicação no contexto de um sistema de reservas de hotéis."
> 
> **Armadilha:** Confundir os dois ou sugerir que casos de uso são apenas métodos nas entidades.
> 
> **Como raciocinar:** Entidades representam conceitos de negócio geral com regras intrínsecas (ex: Reserva sabe validar datas, calcular total, aplicar políticas de cancelamento). Casos de uso representam fluxos específicos desta aplicação (ex: FazerReserva orquestra verificação de disponibilidade, criação de reserva, processamento de pagamento, envio de confirmação). Entidades são reutilizáveis em diferentes contextos; casos de uso são específicos de como este sistema particular usa as entidades para atender aos seus requisitos funcionais específicos.

## Infrastructure Layer (Camada de Infraestrutura)

> 💡 **DICA DE ENTREVISTA**
> 
> Esta camada contém os detalhes específicos de tecnologia; entrevistadores querem ver se você entende que ela deve ser focada em implementação e não conter lógica de negócio.

### definição
A Infrastructure Layer contém implementações técnicas específicas que realizam as operações definidas pelas interfaces das camadas internas. Esta é onde ficam os detalhes de banco de dados, frameworks web, serviços externos, mensageria e outras preocupações de tecnologia. Todas as dependências desta camada apontam para dentro (para as camadas de domínio e aplicação), mas nunca para fora.

### Por que existe?
Para isolar os detalhes específicos de tecnologia das regras de negócio puro, permitindo que as camadas de domínio e aplicação permaneçam completamente alheias a como as operações são realmente implementadas.

### Como funciona internamente?
- Implementações de Repositório: concretizam as interfaces de repositório definidas no domínio usando tecnologias específicas (JPA, Hibernate, MongoDB, etc.)
- Implementações de Serviço: concretizam as interfaces de serviço definidas na aplicação usando tecnologias específicas (JavaMail, Twilio, APIs REST, etc.)
- Adaptadores de Tecnologia: lidam com preocupações específicas como mensageria (Kafka, RabbitMQ), caching (Redis, Caffeine), ou integração com sistemas legados
- Configuração de Tecnologia: configura específicos de framework, conexão de banco de segurança, etc.
- Nenhuma lógica de negócio (isso fica nas camadas de domínio e aplicação)
- Dependências apontam apenas para dentro (usam entidades, casos de uso, interfaces de domínio)
- Comunicação com camadas internas ocorre exclusivamente através de implementação de interfaces

### Como implementar?
1. Identificar todas as interfaces necessárias definidas nas camadas de domínio e aplicação
2. Para cada interface, criar uma ou mais implementações concretas usando tecnologias específicas
3. Implementações de Repositório:
   - Escolher tecnologia de persistência (SQL, NoSQL, arquivo, etc.)
   - Mapear entidades de domínio para estruturas de tecnologia
   - Implementar operações de CRUD usando a tecnologia escolhida
   - Nunca colocar lógica de negócio (deve ficar apenas nas camadas internas)
4. Implementações de Serviço:
   - Escolher tecnologia específica (email, SMS, pagamento, etc.)
   - Implementar operações usando APIs específicas da tecnologia
   - Tratar preocupações específicas como autenticação, rate limiting, retry, etc.
   - Nunca colocar lógica de negócio
5. Nunca colocar lógica de negócio nesta camada
6. Garantir que dependências somente apontem para dentro (usem domínio e aplicação, não sejam usados por eles)
7. Usar padrões como repositório, fábrica, estratégia ou adapter para facilitar implementação
8. Configurar o sistema para conectar implementações às interfaces apropriadas (injeção de dependência, etc.)
9. Manter preocupações de tecnologia específicas aqui (conexões, sessões, parsing, etc.)

### Quais são as alternativas?
- Misturar lógica de negócio com detalhes técnicos (violando separação de preocupações)
- Fazer domínio ou aplicação conhecerem diretamente detalhes de tecnologia
- Deixar interfaces responsáveis por implementação
- Usar camadas de serviço genéricas que misturam aplicação e tecnologia
- Não ter camada de infraestrutura e deixar domínio ou aplicação lidarem diretamente com detalhes técnicos

### Quais são os trade-offs?
**Vantagens de infraestrutura bem definida:**
- Isolamento completo de regras de negócio de detalhes técnicos
- Facilidade de mudar tecnologia sem afetar regras de negócio
- Testabilidade melhorada pois domínio e aplicação podem ser testados com mocks ou implementações em memória
- Clareza sobre onde ficam os detalhes específicos de cada tecnologia
- Facilidade de suportar múltiplas tecnologias simultaneamente se necessário
- Menos risco de vazamento de detalhes técnicos para regras de negócio

**Desvantagens/custos:**
- Sobrehead de criação de classes de implementação
- Pode parecer que esta camada faz "pouco" em comparação com o resto
- Requer disciplina para manter limpa (nenhuma lógica de negócio)
- Pode haver alguma indireção na chamada de tecnologia para regra de negócio
- Necessidade de entender bem onde cada pertencimento vai
- Em sistemas muito simples, pode parecer excesso de formalismo

### Quando usar?
- Sempre que se usar qualquer tecnologia específica de persistência, comunicação ou apresentação
- Quando se quiser garantir que escolha de tecnologia não aprisione regras de negócio
- Quando se antecipa necessidade de mudar tecnologia no futuro
- Quando se quer maximizar testabilidade e manutenibilidade a longo prazo
- Quando se quer evitar o problema de "framework lock-in" onde mudar tecnologia exige reescrita massiva
- Quando se precisa integrar com sistemas legados usando tecnologias antigas ou proprietárias

### Quando não usar?
- Quando se está construindo um sistema tão simples que usar qualquer tecnologia seria overkill
- Quando se está em um ambiente onde apenas código puro é permitido (sistemas embarcados ultra-restritos)
- Quando se está prototipando e velocidade é a única prioridade absoluta
- Quando se está em um contexto onde o overhead de camadas não traz benefício proporcional
- Quando se está construindo um sistema onde a tecnologia é fixa para sempre e nunca mudará
- Quando o overhead de abstração não traz benefício proporcional ao valor da tecnologia utilizada

### Quais são os erros mais comuns?
- Colocar regras de negócio em classes de infraestrutura (ex: validação de negócio em um repositório JPA)
- Fazer classes de infraestrutura dependerem de camadas externas de forma inadequada (violando Dependency Rule ao contrário)
- Tratar esta camada como local genérico para qualquer código difícil de colocar em outro lugar
- Esquecer de que esta camada deve ser apenas uma implementação e acabar colocando lógica significativa aqui
- Anotar diretamente entidades de domínio com especificações de tecnologia (fazendo-as dependentes de detalhes técnicos)
- Fazer pontos de integração conhecerem demais sobre regras de negócio (devem ser fins delegadores)

### Como isso afeta:
- *performance:* Impacto depende da tecnologia específica, mas a camada em si adiciona pouco overhead além da tecnologia em si
- *escalabilidade:* Similar a como a tecnologia específica escalaria; a camada de infraestrutura em si não adiciona muito
- *disponibilidade:* Similar a tecnologia específica; problemas aqui afetam acesso ao sistema mas não necessariamente regras de negócio interno
- *consistência:* Similar; depende de como implementações lidam com transições e erros
- *segurança:* Muita segurança de entrada e saída acontece aqui (validação, sanitização, autenticação, autorização)
- *custo:* Similar a tecnologia específica; custos de licença, desempenho, etc. vêm da tecnologia escolhida
- *observabilidade:* Melhora pois pontos claros de entrada/saída facilitam instrumentação de tecnologia específica
- *complexidade operacional:* Similar a tecnologia específica; gerenciamento de tecnologia acontece aqui

### Exemplos reais de aplicação
- Camada de Persistência JPA/Hibernate que:
  - Contém apenas classes que implementam interfaces de repositório do domínio
  - Contém apenas classes de configuração de conexão e mapeamento objeto-relacional
  - Nunca contém lógica como cálculo de totais, validação de descontos ou aplicação de regras de negócio
  - Nota: Mesmo entidades JPA podem ser polêmicas se colocadas aqui; melhor manter entidades puras no domínio e usar apenas mapeamento separado
- Camada de Serviços de Email que:
  - Contém apenas classes que implementam interface de serviço de email da aplicação
  - Contém apenas classes de configuração de servidor SMTP, credenciais e templates
  - Nunca contém lógica como determinação de quando enviar ou conteúdo da mensagem além do básico de formatação
- Camada de Integração com Sistemas Externos que:
  - Contém apenas classes que implementam interfaces de serviço da aplicação
  - Contém apenas código específico para falar com APIs externas usando tecnologia escolhida
  - Nunca contém lógica como regras de negócio para processamento de resposta ou aplicação de descontos
- Camada de Mensageria que:
  - Contém apenas classes que implementam interfaces de serviço da aplicação
  - Contém apenas código específico para produzir/consumir mensagens usando tecnologia escolhida
  - Nunca contém lógica como processamento de lote específico ou aplicação de regras de negócio

### Exemplo simplificado
Infraestrutura violando independência (errada):
```java
// ❌ ERRADO: Repositório contém lógica de negócio
@Repository
public class OrderRepositoryImpl implements OrderRepository {
    @PersistenceContext
    private EntityManager em;
    
    @Override
    public Order save(Order order) {
        // Lógica de negócio inadequadamente colocada aqui
        if (order.getItems().isEmpty()) {
            throw new IllegalStateException("Ordem deve ter pelo menos um item");
        }
        
        // Mais lógica de negócio aqui - aplicar descontos baseado no valor
        BigDecimal total = order.calculateTotal();
        if (total.compareTo(new BigDecimal("1000")) > 0) {
            // Aplicar desconto de 10% - ISSO É LÓGICA DE NEGÓCIO!
            order.applyDiscount(total.multiply(new BigDecimal("0.10")));
        }
        
        // Operação de persistência
        if (order.getId() == null) {
            em.persist(order);
            return order;
        } else {
            return em.merge(order);
        }
    }
    
    @Override
    public Optional<Order> findById(Long id) {
        // Mesmo aqui - lógica de negócio indevida
        Optional<Order> orderOptional = Optional.ofNullable(em.find(Order.class, id));
        orderOptional.ifPresent(order -> {
            // Verificar se ordem está expirada - LÓGICA DE NEGÓCIO NO REPOSITÓRIO!
            if (order.getOrderDate().isBefore(LocalDateTime.now().minusDays(30))) {
                order.setStatus(OrderStatus.EXPIRED);
            }
        });
        return orderOptional;
    }
}
```

Infraestrutura respeitando independência (correta):
```java
// ✅ CORRETO: Repositório apenas implementa interface de domínio com tecnologia específica
@Repository
@RequiredArgsConstructor
public class OrderRepositoryImpl implements OrderRepository {
    private final EntityManager entityManager;
    
    @Override
    public Order save(Order order) {
        // Apenas traduz entre domínio e tecnologia - NADA de lógica de negócio
        if (order.getId() == null) {
            entityManager.persist(order);
            return order;
        } else {
            return entityManager.merge(order);
        }
    }
    
    @Override
    public Optional<Order> findById(Long id) {
        // Apenas busca e retorna - NADA de lógica de negócio
        return Optional.ofNullable(entityManager.find(Order.class, id));
    }
    
    @Override
    public List<Order> findAll() {
        // Apenas busca todos - NADA de lógica de negócio
        return entityManager.createQuery("SELECT o FROM Order o", Order.class)
                           .getResultList();
    }
    
    @Override
    public void deleteById(Long id) {
        // Apenas deleta - NADA de lógica de negócio
        Order order = entityManager.find(Order.class, id);
        if (order != null) {
            entityManager.remove(order);
        }
    }
}
```

Serviço de infraestrutura respeitando independência (correto):
```java
// ✅ CORRETO: Serviço de email apenas implementa interface de aplicação com tecnologia específica
@Service
@RequiredArgsConstructor
public class EmailServiceImpl implements EmailService {
    private final JavaMailSender mailSender;
    private final TemplateEngine templateEngine;
    
    @Override
    public void sendOrderConfirmation(Order order) {
        // Apenas detalhes específicos de tecnologia JavaMail e Thymeleaf
        // NADA de lógica de negócio como quando enviar ou o que incluir
        
        Context context = new Context();
        context.setVariable("order", order);
        String htmlContent = templateEngine.process("order-confirmation", context);
        
        MimeMessage message = mailSender.createMimeMessage();
        try {
            MimeMessageHelper helper = new MimeMessageHelper(message, true, "UTF-8");
            helper.setTo(order.getCustomer().getEmail());
            helper.setSubject("Confirmação de Pedido #" + order.getId());
            helper.setText(htmlContent, true);
            mailSender.send(message);
        } catch (MessagingException e) {
            throw new RuntimeException("Falha ao enviar email de confirmação", e);
        }
    }
    
    @Override
    public void sendShippingUpdate(Order order, ShippingUpdate update) {
        // Similar - apenas tecnologia, nenhuma lógica de negócio
    }
    
    @Override
    public void sendPaymentReceipt(Order order, PaymentReceipt receipt) {
        // Similar - apenas tecnologia, nenhuma lógica de negócio
    }
}
```

Configuração de infraestrutura respeitando independência:
```java
// ✅ CORRETO: Configuração apenas conecta componentes - NADA de lógica de negócio
@Configuration
public class InfrastructureConfig {
    
    @Bean
    public OrderRepository orderRepository(EntityManager entityManager) {
        return new OrderRepositoryImpl(entityManager);
    }
    
    @Bean
    public EmailService emailService(JavaMailSender mailSender, TemplateEngine templateEngine) {
        return new EmailServiceImpl(mailSender, templateEngine);
    }
    
    @Bean
    public PaymentService paymentService(PaymentGatewayConfig paymentGatewayConfig) {
        return new PaymentServiceImpl(paymentGatewayConfig);
    }
    
    // Outros beans para repositórios, serviços, etc.
}
```

### Exemplo de sistema de produção
Sistema de processamento de seguros para seguradora:
- **Implementações de Repositório:**
  - ApoliceRepositoryImpl: implementa ApoliceRepository usando JPA/Hibernate com PostgreSQL
  - SinistroRepositoryImpl: implementa SinistroRepository usando JPA/Hibernate com PostgreSQL
  - ClienteRepositoryImpl: implementa ClienteRepository usando JPA/Hibernate com PostgreSQL
  - Nenhuma contém lógica como cálculo de prêmio, avaliação de cobertura ou processamento de sinistro
- **Implementações de Serviço:**
  - NotificationServiceImpl: implementa NotificationService usando JavaMail para email, Twilio para SMS
  - PaymentServiceImpl: implementa PaymentService integrado com gateways como Stripe, PayPal ou boleto bancário
  - DocumentGenerationServiceImpl: implementa interface para gerar apólices, recibos e cartas usando biblioteca de geração de PDF
  - Nenhuma contém lógica como regras de negócio para determinação de cobertura ou cálculo de valores
- **Adaptadores de Tecnologia:**
  - LegacySystemAdapter: implementa interface para integração com sistema legado de apólias usando arquivos fixos ou banco de dados antigo
  - ExternalCreditServiceAdapter: implementa interface para consulta de serviços de crédito como Serasa ou Boa Vista
  - WeatherServiceAdapter: implementa interface para obter dados climáticos para avaliação de risco agrícola
  - Nenhuma contém lógica como regras de negócio para processamento de dados ou tomada de decisão
- **Configuração de Tecnologia:**
  - DatabaseConfig: configuração de conexão com PostgreSQL, pool de conexões, dialecto Hibernate
  - MailConfig: configuração de servidor SMTP, credenciais, política de tentativa de envio
  - PaymentGatewayConfig: configuração de credenciais e endpoints para diferentes gateways de pagamento
  - Nenhuma contém lógica como regras de negócio para escolha de tecnologia ou processamento de resultados

### Como esse assunto pode aparecer em uma entrevista.
> 🎯 **ENTREVISTA — ALTA FREQUÊNCIA**
> 
> "Descreva como você organizaria a camada de infraestrutura em um sistema de leilão online para garantir que ela não contenha lógica de negócio."
> 
> **Armadilha:** Sugerir que é aceitável colocar alguma lógica de negócio aqui desde que seja "pouca" ou "relacionada à tecnologia".
> 
> **Como raciocinar:** Explicar que esta camada deve conter apenas:
> - Implementações de interfaces de repositório do domínio usando tecnologias específicas de persistência
> - Implementações de interfaces de serviço da aplicação usando tecnologias específicas (email, pagamento, etc.)
> - Código específico de tecnologia necessário para fazer o sistema funcionar (conexões, sessões, adaptadores, etc.)
> - Configuração específica de tecnologia
> - Nenhuma regra de negócio como cálculo de lances, validação de usuários ou aplicação de regras de leilão
> Mostrar como qualquer tentativa de colocar lógica de negócio aqui iria violar a Dependency Rule e prejudicar testabilidade e independência tecnológica.

## Presentation Layer (Camada de Apresentação)

> 🎯 **ENTREVISTA — ALTA FREQUÊNCIA**
> 
> Esta é a camada mais externa onde ficam os pontos de entrada do sistema; entrevistadores querem ver se você entende que ela deve ser fina e focada em recepção de entrada e apresentação de saída.

### definição
A Presentation Layer contém os pontos de entrada do sistema como controllers, views, apresentadores e pontos de acesso linha de comando. Esta é a camada mais externa onde ficam todos os detalhes específicos de como os usuários ou sistemas externos interagem com a aplicação. Todas as dependências desta camada apontam para dentro (para as camadas de aplicação e domínio), mas nunca para fora.

### Por que existe?
Para isolar os detalhes específicos de apresentação e interação das regras de negócio puro, permitindo que as camadas internas permaneçam completamente alheias a como os dados são recebidos ou exibidos.

### Como funciona internamente?
- Controllers: recebem entradas de tecnologia específica (requisições HTTP, mensagens de fila, comandos de linha de comando) e delegam para casos de uso
- Views/Apresentadores: formatam dados de casos de uso para apresentação específica (HTML, JSON, XML, telas de console)
- Point of Entrada: main method, aplicação web, consumidor de fila, etc. que inicia o sistema
- Validação de Entrada: geralmente ocorre aqui para verificar formato básico antes de passar para casos de uso
- Nenhuma lógica de negócio (isso fica nas camadas de domínio e aplicação)
- Nenhuma dependência em tecnologia específica além do necessário para fazer a camada funcionar
- Dependências apontam apenas para dentro (usam casos de uso, DTOs, serviços de aplicação)
- Comunicação com camadas internas ocorre exclusivamente através de chamada de casos de uso ou serviços

### Como implementar?
1. Identificar todos os pontos de entrada necessários (web, mobile, API, linha de comando, batch, etc.)
2. Para cada ponto de entrada, criar componentes que recebam entrada específica de tecnologia
3. Validar entrada básica de formato (geralmente faz parte da responsabilidade da camada de apresentação)
4. Converter entrada para DTOs ou comandos de aplicação
5. Delegar para casos de uso apropriados na camada de aplicação
6. Receber resultados de casos de uso e converter para formato de saída específico de tecnologia
7. Nunca colocar lógica de negócio (deve ficar apenas nas camadas de domínio e aplicação)
8. Nunca colocar detalhes de infraestrutura específicos além do necessário para apresentação
9. Garantir que dependências somente apontem para dentro (usem aplicação e domínio, não sejam usados por eles)
10. Usar padrões como MVC, MVVM, apresentador ou controlador para facilitar apresentação
11. Configurar o sistema para conectar pontos de entrada às implementações apropriadas

### Quais são as alternativas?
- Misturar lógica de negócio com detalhes de apresentação (violando separação de preocupações)
- Fazer domínio ou aplicação conhecerem diretamente detalhes de apresentação
- Deixar casos de uso responsáveis por recepção de entrada e formatação de saída
- Usar camadas de serviço genéricas que misturam aplicação e apresentação
- Não ter camada de apresentação e deixar domínio ou aplicação lidarem diretamente com entrada/saída

### Quais são os trade-offs?
**Vantagens de apresentação bem definida:**
- Isolamento completo de regras de negócio de detalhes de apresentação
- Facilidade de mudar tecnologia de apresentação sem afetar regras de negócio
- Testabilidade melhorada pois domínio e aplicação podem ser testados sem UI ou pontos de entrada
- Clareza sobre onde ficam os detalhes específicos de cada tecnologia de apresentação
- Facilidade de suportar múltiplas tecnologias de apresentação simultaneamente
- Menos risco de vazamento de detalhes de apresentação para regras de negócio

**Desvantagens/custos:**
- Sobrehead de criação de componentes de apresentação
- Pode parecer que esta camada faz "pouco" em comparação com o resto
- Requer disciplina para manter limpa (nenhuma lógica de negócio)
- Pode haver alguma indireção na chamada de apresentação para regra de negócio
- Necessidade de entender bem onde cada pertencimento vai
- Em sistemas muito simples, pode parecer excesso de formalismo

### Quando usar?
- Sempre que se precisar de pontos de entrada para usuários ou sistemas externos interagirem com o sistema
- Quando se quiser garantir que escolha de tecnologia de apresentação não aprisione regras de negócio
- Quando se antecipa necessidade de mudar tecnologia de apresentação no futuro
- Quando se quer maximizar testabilidade e manutenibilidade a longo prazo
- Quando se quer evitar o problema de "framework lock-in" onde mudar tecnologia de apresentação exige reescrita massiva
- Quando se precisa suportar diferentes tipos de clientes (web app, app móvel, API de terceiros, linha de comando, batch)

### Quando não usar?
- Quando se está construindo um sistema tão simples que entrada e saída são diretas e simples
- Quando se está em um ambiente onde apenas código puro é permitido (sistemas embarcados ultra-restritos)
- Quando se está prototipando e velocidade é a única prioridade absoluta
- Quando se está em um contexto onde o overhead de camadas não traz benefício proporcional
- Quando se está construindo um sistema onde a tecnologia de apresentação é fixa para sempre e nunca mudará
- Quando o overhead de abstração não traz benefício proporcional ao valor da apresentação fornecida

### Quais são os erros mais comuns?
- Colocar regras de negócio em componentes de apresentação (ex: validação de negócio em um controller)
- Fazer componentes de apresentação conhecerem demais sobre domínio ou aplicação (acoplamento reverso)
- Tratar esta camada como local genérico para qualquer código difícil de colocar em outro lugar
- Esquecer de que esta camada deve ser apenas uma camada fina de entrada/saída e acabar colocando lógica significativa aqui
- Fazer pontos de entrada conhecerem demais sobre regras de negócio (devem ser fins delegadores)
- Colocar lógica de apresentação nos casos de uso ou domínio (deve ficar apenas na apresentação)

### Como isso afeta:
- *performance:* Impacto depende da tecnologia específica, mas a camada em si adiciona pouco overhead além da tecnologia em si
- *escalabilidade:* Similar a como a tecnologia específica de apresentação escalaria; a camada de apresentação em si não adiciona muito
- *disponibilidade:* Similar a tecnologia específica de apresentação; problemas aqui afetam entrada/saída mas não necessariamente regras de negócio interno
- *consistência:* Similar; depende de como componentes de apresentação lidam com transições e erros
- *segurança:* Muita segurança de entrada acontece aqui (validação, sanitização, autenticação de nível de entrada)
- *custo:* Similar a tecnologia específica de apresentação; custos de licença, desempenho, etc. vêm da tecnologia escolhida
- *observabilidade:* Melhora pois pontos claros de entrada/saída facilitam logging e monitoring de tecnologia específica
- *complexidade operacional:* Similar a tecnologia específica de apresentação; gerenciamento de apresentação acontece aqui

### Exemplos reais de aplicação
- Camada Web MVC que:
  - Contém apenas controllers que recebem requisições HTTP e delegam para casos de uso
  - Contém apenas views ou apresentadores que formatam dados de casos de uso para HTML
  - Contém apenas middleware básico para logging, autenticação e tratamento de erros
  - Nunca contém lógica como cálculo de totais, validação de descontos ou aplicação de regras de negócio
- Camada de API REST que:
  - Contém apenas controllers que recebem requisições HTTP/JSON e delegam para casos de uso
  - Contém apenas serializadores ou apresentadores que formatam dados de casos de uso para JSON/XML
  - Contém apenas tratamento básico de códigos de status e cabeçalhos HTTP
  - Nunca contém lógica como determinação de quando processar ou o que incluir na resposta além do básico
- Camada de Linha de Comando que:
  - Contém apenas parsers de argumentos que convertem entrada para comandos de aplicação
  - Contém apenas formatadores de saída que convertem dados de casos de uso para texto formatado
  - Contém apenas tratamento básico de códigos de saída e mensagens de erro
  - Nunca contém lógica como quando executar ou o que fazer com o resultado além do básico de chamada
- Camada de Mensageria/Fila que:
  - Contém apenas consumidores que recebem mensagens de tecnologia específica e delegam para casos de uso
  - Contém apenas produtores que convertem dados de casos de uso para mensagens de tecnologia específica
  - Contém apenas tratamento básico de acknowlegment, retry e dead letter queues
  - Nunca contém lógica como processamento de lote específico ou aplicação de regras de negócio

### Exemplo simplificado
Apresentação violando independência (errada):
```java
// ❌ ERRADO: Controller sabe demais sobre caso de uso e contém lógica de negócio
@RestController
public class OrderController {
    @Autowired
    private PlaceOrderUseCase placeOrderUseCase;
    
    @PostMapping("/orders")
    public ResponseEntity<?> placeOrder(@RequestBody OrderRequest request) {
        // Lógica de negócio inadequadamente colocada aqui (validação além do básico)
        if (request.getCustomerId() == null || request.getCustomerId().isEmpty()) {
            return ResponseEntity.badRequest().body("Cliente é obrigatório");
        }
        
        // Mais lógica de negócio aqui - verificar se cliente está ativo, etc.
        
        // Chamada ao caso de uso
        OrderResponse response = placeOrderUseCase.execute(new PlaceOrderCommand(
            request.getCustomerId(),
            request.getItems()
        ));
        
        // Mais lógica de negócio aqui - modificar resposta baseado em regras de negócio
        
        return ResponseEntity.ok(response);
    }
    
    @GetMapping("/orders/{id}")
    public ResponseEntity<?> getOrder(@PathVariable Long id) {
        // Lógica de negócio inadequadamente colocada aqui
        OrderResponse response = orderUseCase.getOrder(id);
        
        // Mais lógica de negócio aqui - aplicar formatação baseado no status
        
        return ResponseEntity.ok(response);
    }
}
```

Apresentação respeitando independência (correta):
```java
// ✅ CORRETO: Controller apenas traduz entre HTTP e caso de uso
@RestController
@RequestMapping("/api/orders")
@RequiredArgsConstructor
public class OrderController {
    private final PlaceOrderUseCase placeOrderUseCase;
    private final GetOrderUseCase getOrderUseCase;
    private final ListOrdersUseCase listOrdersUseCase;
    
    @PostMapping
    public ResponseEntity<OrderResponse> placeOrder(@RequestBody PlaceOrderApiRequest request) {
        // Apenas traduz entre HTTP e caso de uso
        PlaceOrderCommand command = new PlaceOrderCommand(
            request.getCustomerId(),
            request.getItems()
        );
        
        // Chamada pura para caso de uso
        OrderResponse response = placeOrderUseCase.execute(command);
        
        // Apenas traduz para formato de saída HTTP
        return ResponseEntity.ok(new OrderApiResponse(
            response.getId(),
            response.getCustomerId(),
            response.getOrderDate(),
            response.getStatus(),
            response.getItems().stream()
                               .map(item -> new OrderItemApiResponse(
                                   item.getProductId(),
                                   item.getQuantity(),
                                   item.getUnitPrice()
                               ))
                               .collect(Collectors.toList())
        ));
    }
    
    @GetMapping("/{id}")
    public ResponseEntity<OrderResponse> getOrder(@PathVariable Long id) {
        // Apenas traduz entre HTTP e caso de uso
        OrderResponse response = getOrderUseCase.execute(new GetOrderCommand(id));
        
        // Apenas traduz para formato de saída HTTP
        return ResponseEntity.ok(new OrderApiResponse(
            response.getId(),
            response.getCustomerId(),
            response.getOrderDate(),
            response.getStatus(),
            response.getItems().stream()
                               .map(item -> new OrderItemApiResponse(
                                   item.getProductId(),
                                   item.getQuantity(),
                                   item.getUnitPrice()
                               ))
                               .collect(Collectors.toList())
        ));
    }
}

// Objetos específicos de tecnologia (API) - contêm apenas dados, nenhuma lógica de negócio
record PlaceOrderApiRequest(String customerId, List<OrderItemApiRequest> items) {}
record OrderItemApiRequest(Long productId, int quantity, BigDecimal unitPrice) {}
record OrderApiResponse(Long id, String customerId, LocalDateTime orderDate, OrderStatus status, List<OrderItemApiResponse> items) {}
record OrderItemApiResponse(Long productId, int quantity, BigDecimal unitPrice) {}
```

### Exemplo de sistema de produção
Sistema de gestão de clínica médica:
- **Controllers Web (REST):**
  - ConsultaController: recebe requisições HTTP/JSON, valida formato básico, converte para AgendarConsultaCommand, delega para agendarConsultaUseCase.execute(command)
  - PacienteController: similar para operações de paciente
  - ProntuarioController: similar para operações de prontuário
  - Nenhuma contém lógica como verificação de disponibilidade, aplicação de regras de negócio ou formatação de resposta além do básico
- **Controllers Web (MVC):**
  - ConsultaWebController: semelhante mas retorna views HTML em vez de JSON
  - PacienteWebController: similar
  - Nenhuma contém lógica de negócio
- **Apresentadores de Linha de Comando:**
  - ConsultaCliCommand: converte argumentos de linha de comando para AgendarConsultaCommand, delega para caso de uso
  - RelatorioCliCommand: converte argumentos para GerarRelatorioCommand, delega para caso de uso
  - Nenhuma contém lógica de negócio
- **Consumidores de Mensageria:**
  - NotificacaoListener: recebe mensagens de fila sobre eventos, converte para NotificarPacienteCommand, delega para caso de uso
  - PagamentoListener: recebe confirmações de pagamento, converte para ProcessarPagamentoCommand, delega para caso de uso
  - Nenhuma contém lógica de negócio
- **Formatadores de Saída:**
  - HtmlConsultaPresenter: formata dados de consulta para exibição em HTML
  - JsonConsultaPresenter: formata dados de consulta para resposta JSON
  - TxtConsultaPresenter: formata dados de consulta para exibição em texto simples
  - Nenhuma contém lógica de negócio na formatação - apenas conversão de formato
- **Points of Entrada:**
  - Classe principal que inicializa o framework (Spring Boot, etc.)
  - Configuração de rotas HTTP
  - Configuração de fila de mensageria
  - Nenhuma contém lógica de negócio

### Como esse assunto pode aparecer em uma entrevista.
> 🎯 **ENTREVISTA — ALTA FREQUÊNCIA**
> 
> "Descreva como você organizaria a camada de apresentação em um sistema de reservas de hotéis para garantir que ela não contenha lógica de negócio."
> 
> **Armadilha:** Sugerir que é aceitável colocar alguma lógica de negócio aqui desde que seja "pouca" ou "relacionada à apresentação".
> 
> **Como raciocinar:** Explicar que esta camada deve conter apenas:
> - Controllers que recebem entrada específica de tecnologia (HTTP, CLI, mensagem) e delegam para casos de uso
> - Validação básica de formato de entrada (ex: JSON válido, campos obrigatórios presentes)
> - Conversão entre formato específico de tecnologia e DTOs/comandos de aplicação
> - Delegação pura para casos de uso na camada de aplicação
> - Conversão de resultados de casos de uso para formato específico de saída
> - Nenhuma regra de negócio como cálculo de total, validação de disponibilidade ou aplicação de políticas de cancelamento
> Mostrar como qualquer tentativa de colocar lógica de negócio aqui iria violar a Dependency Rule e prejudicar testabilidade e independência de apresentação.

## Dependency Rule (Regra de Dependência) na Onion Architecture

> 🎯 **ENTREVISTA — ALTA FREQUÊNCIA**
> 
> A Dependency Rule é fundamental para a Onion Architecture e frequentemente perguntada em entrevistas para testar entendimento de acoplamento e independência.

### definição
A Dependency Rule na Onion Architecture estabelece que dependências só podem apontar para dentro: camadas externas dependem de camadas internas, mas camadas internas nunca dependem de camadas externas. Ou seja, a camada de domínio não depende de aplicação, infraestrutura ou apresentação; a camada de aplicação não depende de infraestrutura ou apresentação; apenas a camada de apresentação pode depender de aplicação, infraestrutura e domínio.

### Por que existe?
Para garantir que as regras de negócio puro no domínio permaneçam completamente independentes de detalhes de aplicação, infraestrutura e apresentação, tornando-as testáveis, reutilizáveis e independentes de tecnologia.

### Como funciona internamente?
- Camada de Domínio:
  - Não depende de nenhuma outra camada (ou apenas de si mesma)
  - Contém entidades de negócio, interfaces de repositório e serviços de domínio
  - É o núcleo mais abstrato e estável do sistema
- Camada de Aplicação:
  - Depende apenas da camada de domínio
  - Contém casos de uso, DTOs e interfaces de serviço
  - Orquestra domínio para atender aos requisitos funcionais específicos desta aplicação
- Camada de Infraestrutura:
  - Depende de domínio e aplicação (mas não de apresentação)
  - Contém implementações de repositórios, serviços e adaptadores de tecnologia
  - Fornece implementações concretas das interfaces definidas internas
- Camada de Apresentação:
  - Depende de domínio, aplicação e infraestrutura (mas apenas do necessário)
  - Contém controllers, views, apresentadores e ponto de entrada
  - Recebe entrada específica de tecnologia e delega para casos de uso
- Nenhuma dependência aponta para fora (de interna para externa)
- Comunicação entre camadas ocorre exclusivamente através de implementação de interfaces definidas em camadas mais internas

### Como implementar?
1. Manter camada de domínio pura - nenhuma dependência em aplicação, infraestrutura ou apresentação
2. Permitir que camada de aplicação dependa apenas de domínio
3. Permitir que camada de infraestrutura dependa de domínio e aplicação
4. Permitir que camada de apresentação dependa de domínio, aplicação e infraestrutura (limitado ao necessário)
5. Usar injeção de dependência para fornecer implementações concretas às interfaces definidas internas
6. Nunca permitir que uma camada interna faça importação de pacotes específicos de uma camada externa
7. Em revisões de código, verificar que nenhuma dependência aponta para fora
8. Usar ferramentas de arquitetura (como ArchUnit) para testar que a Dependency Rule não é violada
9. Em testes, garantir que testes de domínio dependam apenas de domínio
10. Em testes, garantir que testes de aplicação dependam apenas de domínio e aplicação

### Quais são as alternativas?
- Permitir dependências livres (arquitetura tradicional sem restrições)
- Dependências apenas em uma direção como em arquitetura em camadas tradicional
- Nenhuma restrição de dependência (tudo pode depender de tudo)
- Dependências bidirecionais controladas (como em algumas arquiteturas de plugins)

### Quais são os trade-offs?
**Vantagens de respeitar a Dependency Rule:**
- Domínio totalmente independente de aplicação, infraestrutura e apresentação
- Excelente testabilidade: domínio pode ser testado isoladamente, aplicação pode ser testada com mocks de domínio
- Independência tecnológica: regras de negócio não sabem nada sobre banco de dados, framework ou UI usados
- Clareza sobre o que o sistema realmente precisa (as interfaces definidas no domínio)
- Facilidade de mudar tecnologia sem afetar regras de negócio
- Facilidade de suportar múltiplas tecnologias simultaneamente
- Menos risco de efeito colateral ao mudar detalhes técnicos

**Desvantagens/custos:**
- Sobrehead de definição de interfaces
- Pode parecer indireto para desenvolvedores acostumados com acesso direto
- Requer disciplina para manter as camadas puras (nenhum vazamento de dependência)
- Necessidade de entender bem onde cada pertencimento vai
- Em casos muito simples, pode parecer excesso de formalismo

### Quando usar?
- Sempre que se quiser garantir independência de regras de negócio em relação a detalhes de aplicação, infraestrutura e apresentação
- Sistemas onde regras de negócio são o ativo mais valioso e devem ser preservados
- Quando se antecipa necessidade de mudar tecnologia (aplicação, infraestrutura ou apresentação)
- Sistemas que devem ter longa vida útil e passar por múltiplas evoluções tecnológicas
- Quando se quer maximizar testabilidade e reutilização de regras de negócio

### Quando não usar?
- Quando se está construindo um protótipo descartável onde velocidade é a única prioridade
- Quando se está em um contexto onde desempenho crítico exige acesso direto e nenhuma indireção é tolerável
- Quando se está prototipando e velocidade é a única prioridade
- Quando se está construindo um sistema tão simples que o overhead não traz benefício
- Quando se está em um ambiente altamente restrito onde cada byte conta e indireção é proibida

### Quais são os erros mais comuns?
- Domínio que depende de aplicação (ex: entidade que conhece caso de uso específico)
- Domínio que depende de infraestrutura (ex: entidade com anotações JPA ou conhece detalhes de banco de dados)
- Domínio que depende de apresentação (ex: entidade que conhece detalhes de HTML ou JSON)
- Aplicação que depende de infraestrutura (ex: caso de uso que conhece detalhes de JPA ou HTTP)
- Aplicação que depende de apresentação (ex: caso de uso que conhece detalhes de Servlet ou MVC)
- Infraestrutura que depende de apresentação (ex: implementação que conhece detalhes de servlet ou JSP)
- Uso de injeção de dependência incorreta (criando instâncias diretamente em vez de injetar)
- Acreditar que usar uma interface automaticamente respeita a Dependency Rule (quando a implementação ainda é referenciada incorretamente)

### Como isso afeta:
- *performance:* Impacto mínimogeralmente <1% devido a chamada de interface indireta vs direta
- *escalabilidade:* Nenhum impacto direto
- *disponibilidade:* Nenhum impacto direto
- *consistência:* Melhora pois regras de negócio ficam isoladas e menos propensas a inconsistência
- *segurança:* Similar; regras de negócio puro são mais fáceis de auditar
- *custo:* Custo inicial de desenvolvimento ligeiramente maior, mas custo de manutenção a longo prazo menor
- *observabilidade:* Melhora pois pontos claros de dependência facilitam mocking e testing
- *complexidade operacional:* Similar; pode reduzir bugs devido a melhor isolamento e testabilidade

### Exemplos reais de aplicação
- Sistema de negociação de ações onde:
  - Domínio contém entidades como Ordem, Acao, Carteira com regras de negócio de validação e cálculo
  - Domínio contém interfaces como OrdemRepository, AcaoService
  - Aplicação contém casos de uso como ExecutarOrdemUseCase, CancelarOrdemUseCase
  - Infraestrutura contém implementações JPA de repositórios e integração com APIs de corretora
  - Apresentação contém controllers REST que recebem ordens e delegam para casos de uso
  - Nenhum domínio conhece JPA, HTTP ou detalhes de corretora específica
- Sistema de prontuário eletrônico onde:
  - Domínio contém entidades como Paciente, Medico, Consulta, Prontuário com regras de negócio clínico
  - Domínio contém interfaces como PacienteRepository, ConsultaService
  - Aplicação contém casos de uso como AgendarConsultaUseCase, EmitirProntuarioUseCase
  - Infraestrutura contém implementações JPA e integração com sistemas de laboratório e farmácia
  - Apresentação contém controllers web e mobile que recebem solicitações e delegam para casos de uso
  - Nenhum domínio conhece detalhes de banco de dados, web ou mobile específico

### Exemplo simplificado
Violando a Dependency Rule (errado):
```java
// ❌ ERRADO: Entidade de domínio conhece detalhes de aplicação (caso de uso)
@Entity
public class Order {
    @Id @GeneratedValue
    private Long id;
    
    private String customerId;
    private LocalDateTime orderDate;
    private OrderStatus status;
    
    // ❌ PROBLEMA: Conhece caso de uso específico de aplicação
    private PlaceOrderUseCase placeOrderUseCase; // DEVERIA ficar apenas na aplicação
    
    public void placeOrder(String customerId, List<OrderItem> items) {
        // ❌ PROBLEMA: Lógica de aplicação no domínio
        if (customerId == null || customerId.isEmpty()) {
            throw new IllegalArgumentException("Cliente é obrigatório");
        }
        this.customerId = customerId;
        this.orderDate = LocalDateTime.now();
        this.status = OrderStatus.PENDING;
        // ... lógica de criação de ordem
        
        // ❌ PROBLEMA: Chamando caso de uso do domínio
        placeOrderUseCase.execute(new PlaceOrderCommand(customerId, items));
    }
    
    // Getters e setters
}
```

Respeitando a Dependency Rule (correto):
```java
// ✅ CORRETO: Entidade de domínio pura - nenhuma dependência em aplicação
public class Order {
    private Long id;
    private String customerId;
    private LocalDateTime orderDate;
    private OrderStatus status;
    private final List<OrderItem> items;
    
    public Order(Long id, String customerId, LocalDateTime orderDate) {
        this.id = id;
        this.customerId = customerId;
        this.orderDate = orderDate;
        this.status = OrderStatus.PENDING;
        this.items = new ArrayList<>();
    }
    
    // Métodos de negócio de domínio puro
    public void addItem(OrderItem item) {
        if (status != OrderStatus.PENDING) {
            throw new IllegalStateException("Não é possível adicionar itens a ordem não pendente");
        }
        items.add(item);
    }
    
    public BigDecimal calculateTotal() {
        return items.stream()
                   .map(OrderItem::getTotal)
                   .reduce(BigDecimal.ZERO, BigDecimal::add);
    }
    
    public void confirm() {
        if (status != OrderStatus.PENDING) {
            throw new IllegalStateException("Somente ordens pendentes podem ser confirmadas");
        }
        if (items.isEmpty()) {
            throw new IllegalStateException("Ordem deve ter pelo menos um item");
        }
        this.status = OrderStatus.CONFIRMED;
    }
    
    // Getters apenas
    public Long getId() { return id; }
    public String getCustomerId() { return customerId; }
    public LocalDateTime getOrderDate() { return orderDate; }
    public OrderStatus getStatus() { return status; }
    public List<OrderItem> getItems() { return Collections.unmodifiableList(items); }
}
```

Caso de uso na aplicação (depende apenas de domínio):
```java
// ✅ CORRETO: Caso de uso depende apenas de domínio
@Service
@RequiredArgsConstructor
public class PlaceOrderUseCase {
    private final OrderRepository orderRepository; // Interface de domínio
    
    public OrderResponse execute(PlaceOrderCommand command) {
        // Validação de negócio de aplicação
        if (command.getCustomerId() == null || command.getCustomerId().isEmpty()) {
            throw new IllegalArgumentException("Cliente é obrigatório");
        }
        
        if (command.getItems() == null || command.getItems().isEmpty()) {
            throw new IllegalArgumentException("Ordem deve ter pelo menos um item");
        }
        
        // Orquestração de domínio usando apenas regras de negócio de domínio
        Order order = new Order(null, command.getCustomerId(), LocalDateTime.now());
        for (OrderItemDto itemDto : command.getItems()) {
            OrderItem item = new OrderItem(
                itemDto.getProductId(),
                itemDto.getQuantity(),
                itemDto.getUnitPrice()
            );
            order.addItem(item); // Apenas domínio
        }
        
        Order savedOrder = orderRepository.save(order); // Apenas domínio
        
        // Retorna apenas dados de negócio
        return new OrderResponse(
            savedOrder.getId(),
            savedOrder.getCustomerId(),
            savedOrder.getOrderDate(),
            savedOrder.getStatus(),
            savedOrder.getItems().stream()
                                  .map(item -> new OrderItemDto(
                                      item.getProductId(),
                                      item.getQuantity(),
                                      item.getUnitPrice()
                                  ))
                                  .collect(Collectors.toList())
        );
    }
}
```

### Exemplo de sistema de produção
Sistema de gestão de clínica médica:
- **Domínio (puro):**
  - Entidades: Paciente, Medico, Consulta, Prontuario (nenhuma conhece JPA, HTTP ou detalhes específicos)
  - Interfaces de Repositório: PacienteRepository, MedicoRepository, ConsultaRepository, ProntuarioRepository
  - Serviços de Domínio: ValidaçãoAgendaService, CodificacaoProcedimentoService (nenhuma conhece detalhes de aplicação)
- **Aplicação:**
  - Casos de Uso: AgendarConsultaUseCase, DarAltaPacienteUseCase, ProcessarPagamentoConsultaUseCase (dependem apenas de domínio)
  - DTOs: ConsultaDto, PacienteDto, MedicoDto (apenas dados de transferência)
  - Interfaces de Serviço: EmailService, NotificationService, PagamentoService (apenas contratos)
- **Infraestrutura:**
  - Implementações de Repositório: PacienteRepositoryImpl (JPA), MedicoRepositoryImpl (JPA), etc. (dependem de domínio)
  - Implementações de Serviço: EmailServiceImpl (JavaMail), NotificationServiceImpl (Twilio/SMS), etc. (dependem de aplicação e domínio)
- **Apresentação:**
  - Controllers: ConsultaController (HTTP), PacienteController (HTTP), etc. (dependem de aplicação)
  - Views: HTML, JSON, XML formatadores (dependem de aplicação)
  - Nenhuma camada interna conhece detalhes específicos de tecnologia externa

### Como esse assunto pode aparecer em uma entrevista.
> 🎯 **ENTREVISTA — ALTA FREQUÊNCIA**
> 
> "Explique como você garantiria que as regras de negócio de um sistema de processamento de pagamentos sejam independentes da camada de apresentação (web, mobile, API) e da camada de infraestrutura (banco de dados, gateway de pagamento)."
> 
> **Armadilha:** Sugerir apenas usar camadas sem verificar se dependências estão apontando na direção correta.
> 
> **Como raciocinar:** Definir claramente que:
> - Camada de domínio contém entidades como Pagamento, Cartao, Fatura com regras de negócio puro (validação de número, cálculo de juros, aplicação de taxas)
> - Camada de domínio contém interfaces como PagamentoRepository, NotificacaoService
> - Camada de aplicação contém casos de uso como ProcessarPagamentoUseCase, EstornarPagamentoUseCase (dependem apenas de domínio)
> - Camada de aplicação contém DTOs e interfaces de serviço necessárias
> - Camada de infraestrutura contém implementações como PagamentoRepositoryImpl (JPA/Hibernate), NotificacaoServiceImpl (Twilio/email)
> - Camada de apresentação contém controllers que recebem requisições HTTP/JSON e delegam para casos de uso
> - Verificar que nenhuma dependência aponta para fora (domínio não conhece JPA, HTTP, Twilio; aplicação não conhece detalhes específicos de banco de dados ou gateway)
> - Mostrar como teste de domínio roda sem nenhum framework, teste de aplicação roda com mocks de domínio, teste de infraestrutura testa as implementações específicas

## Quando usar Onion Architecture

> 💡 **DICA DE ENTREVISTA**
> 
> Sempre relacione a escolha ao contexto específico - não trate como regra universal.

Use Onion Architecture quando:
- O sistema é esperado para ter longa vida útil (anos ou décadas)
- As regras de negócio são complexas e representam o ativo mais valioso do sistema
- Se antecipa necessidade de mudar tecnologias (aplicação, infraestrutura ou apresentação) durante a vida do sistema
- Múltiplas interfaces externas (web, mobile, API, linha de comando) precisam acessar a mesma lógica de negócio
- Se quer garantir que o núcleo do sistema seja altamente testável sem depender de frameworks completos
- Se quer evitar o problema de "framework lock-in" onde mudar tecnologia exige reescrita massiva
- A equipe valoriza limpeza, manutenibilidade e testabilidade a longo prazo sobre velocidade inicial de desenvolvimento
- Se está construindo um sistema onde falhas de regras de negócio seriam particularmente custosas
- Se quer maximizar a reutilização de regras de negócio em diferentes contextos ou aplicações
- Se está migrando de arquitetura em camadas tradicional e quer melhor separação de preocupações com aderência rígida à Dependency Rule

Não use Onion Architecture quando:
- Está construindo um protótipo descartável ou prova de conceito onde velocidade é a única prioridade
- O sistema é tão simples que o overhead de camadas e interfaces não traz benefício proporcional
- A equipe rejeita fortemente a ideia de camadas e interfaces adicionais
- Está em um ambiente altamente restrito onde cada classe ou byte conta (sistemas embarcados ultra-restritos)
- Se está prototipando e velocidade é a prioridade absoluta
- Se vai descartar o sistema após uso único ou muito limitado
- O domínio é tão simples que não há regras de negócio significativas para isolar
- Se está em um contexto onde desempenho crítico exige acesso direto e nenhuma indireção é tolerável
- Quando o overhead de abstração não traz benefício proporcional ao valor adicional obtido
- Quando se está construindo um sistema onde a tecnologia é fixa para sempre e nunca mudará, tornando o isolamento desnecessário

## Comparação com outras arquiteturas

> 🎯 **ENTREVISTA — ALTA FREQUÊNCIA**
> 
> Comparar arquiteturas é comum em entrevistas para testar entendimento de trade-offs e contexto.

| Aspecto | Onion Architecture | Clean Architecture |
|---------|-------------------|-------------------|
| **Visão Central** | Camadas concêntricas com domínio no centro | Círculos concêntricos com entidades no centro |
| **Terminologia** | Domínio, Aplicação, Infraestrutura, Apresentação | Entities, Use Cases, Interface Adapters, Frameworks & Drivers |
| **Foco Principal** | Separação por tipo de responsabilidade (domínio vs aplicação vs infra) | Separação por nível de abstração (negócio vs mecanismo) |
| **Camada de Aplicação** | Explícita: casos de uso e DTOs | Implícita: casos de uso ficam no segundo círculo |
| **Camada de Domínio** | Contém entidades, interfaces de repositório e serviços de domínio | Contém entidades e, às vezes, serviços de domínio |
| **Quando Usar** | Quando se foca em separação clara de responsabilidades por tipo | Quando se foca em independência de regras de negócio em relação a detalhes técnicos |
| **Sobreposição** | Muito overlap; podem ser vistas como complementares ou variações do mesmo conceito | Muito overlap; clean architecture pode ser vista como uma especialização da onion |
| **Exemplo de Uso** | Sistema onde se quer separar claramente domínio, aplicação e infraestrutura | Sistema onde regras de negócio são o ativo mais valioso e devem ser preservados independentemente de tecnologia |

| Aspecto | Onion Architecture | Hexagonal Architecture |
|---------|-------------------|-----------------------|
| **Visão Central** | Camadas concêntricas | Hexágono com portas no centro |
| **Terminologia** | Domínio, Aplicação, Infraestrutura, Apresentação | Portas (entrada/saída), Adaptadores, Núcleo (Domínio) |
| **Foco Principal** | Separação por tipo de responsabilidade | Isolamento através de portas que definem pontos de interação |
| **Abordagem de Camadas** | Mais tradicional e familiar para equipes vindo de camadas | Mais focada em pontos de interação específicos |
| **Testabilidade** | Muito alta: domínio e aplicação testáveis sem infraestrutura | Muito alta: domínio testável com adaptadores em memória |
| **Flexibilidade de Entrada/Saída** | Alta: suporta múltiplas interfaces através de adaptadores | Muito alta: projetada especificamente para múltiplas entradas/saídas |
| **Quando Usar** | Quando se foca em separação clara de responsabilidades | Quando se foca em flexibilidade de entrada/saída |
| **Sobreposição** | Muito overlap; onion pode ser vista como uma variação da hexagonal com foco diferente | Muito overlap; hexagonal pode ser vista como uma variação da onion com foco diferente |

## Exercícios

### Exercício básico
Explique a Dependency Rule da Onion Architecture usando um exemplo de sistema de lista de tarefas.

### Exercício intermediário
Dado um cenário de sistema bancário com funcionalidades de conta corrente, poupança e investimento, analise:
- Como as entidades seriam modeladas na camada de domínio (focando em regras de negócio intrínseco)
- Como as interfaces de repositório seriam definidas no domínio
- Como os casos de uso seriam estruturados na camada de aplicação (fluxos específicos da aplicação)
- Como os DTOs seriam usados para transferir dados entre camadas
- Como os adaptadores de infraestrutura lidariam com JPA, Hibernate e serviços externos
- Como a apresentação seria mantida fina usando apenas casos de uso
- Como você testaria cada camada isoladamente

### Exercício avançado
Analise um sistema que você conhece que usa ou poderia se beneficiar da Onion Architecture:
1. Documente como as responsabilidades seriam distribuídas entre as quatro camadas
2. Mostre como as regras de negócio ficam isoladas dos detalhes técnicos
3. Avalie se a arquitetura segue corretamente os princípios de isolamento e testabilidade
4. Identifique oportunidades de melhoria na aplicação dos princípios
5. Descreva como você migraria um sistema existente para essa arquitetura com risco mínimo

### Exercício de entrevista
> 🎯 **ENTREVISTA — ALTA FREQUÊNCIA**
> 
> "Você herda um sistema onde as regras de negócio estão espalhadas entre controllers, entidades e serviços. Descreva sua abordagem para refatorá-lo para Onion Architecture sem riscos desnecessários."
> 
> Forneça a resposta esperada e explique o que torna ela eficaz.

### Desafio
Crie uma matriz de decisão que ajude a determinar quando usar Onion Architecture, quando evoluir de uma arquitetura em camadas tradicional, e quando considerar alternativas como Clean ou Hexagonal Architecture. Inclua fatores como: vida útil esperada do sistema, valor das regras de negócio, necessidade de independência tecnológica, múltiplas interfaces externas, maturidade da equipe, e requisitos de testabilidade.
