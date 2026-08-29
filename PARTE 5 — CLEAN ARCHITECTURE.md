---
trilha: "INICIANTE"
---
**Navegação:** [[MOC — TRILHA INICIANTE]]
← [[PARTE 4 — ARQUITETURA EM CAMADAS]] | #trilha/iniciante | [[PARTE 6 — HEXAGONAL ou PORTS AND ADAPTERS]] →

---
# PARTE 5 — CLEAN ARCHITECTURE

> 🧠 **ESSENCIAL**
> 
> Clean Architecture propõe uma separação clara de preocupações onde as regras de negócio são independentes de frameworks, bancos de dados e interfaces externas, permitindo que o sistema seja fácil de manter, testar e evoluir.

## O que é Clean Architecture?
Clean Architecture é um estilo de arquitetura de software proposto por Robert C. Martin (Uncle Bob) que organiza o sistema em círculos concêntricos, onde quanto mais para o interior, mais alto é o nível de abstração e menos dependente de detalhes concretos. O núcleo contém as regras de negócio puro, enquanto os círculos externos lidam com mecanismos como frameworks, bancos de dados e interfaces de usuário.

### Por que existe?
Como resposta à frustração com arquiteturas onde as regras de negócio estão entrelaçadas com detalhes técnicos específicos, tornando o sistema difícil de testar, manter e adaptar a mudanças tecnológicas ou de negócio.

### Qual problema resolve?
- Alto acoplamento entre regras de negócio e frameworks/bibliotecas
- Dificuldade de testar regras de negócio sem inicializar todo o framework
- Dificuldade de mudar de banco de dados ou framework sem reescrever grande parte do código
- Regras de negócio difíceis de entender devido à mistura com detalhes técnicos
- Baixa capacidade de reutilização de regras de negócio em diferentes contextos

### Como funciona internamente?
- O sistema é organizado em quatro camadas principais (do interior para o exterior):
  1. **Entities** (Entidades) - Regras de negócio crítico e de alto nível
  2. **Use Cases** (Casos de Uso) - Regras de negócio específico da aplicação
  3. **Interface Adapters** (Adaptadores de Interface) - Converte dados entre formatos convenientes para casos de uso e para entidades externas
  4. **Frameworks & Drivers** (Frameworks e Drivers) - Detalhes específicos como frameworks web, bancos de dados, dispositivos
- A **Dependency Rule** estabelece que dependências só podem apontar para dentro (camadas internas não dependem de camadas externas)
- Comunicação entre camadas ocorre através de interfaces que são implementadas por camadas externas

### Como implementar?
1. Definir entidades de negócio puro (regras que não mudam com mudanças tecnológicas)
2. Definir casos de uso que orquestram entidades para atender aos requisitos funcionais
3. Criar interfaces nos casos de uso para dependências externas (repositórios, apresentadores)
4. Implementar essas interfaces nos adaptadores de interface usando tecnologias específicas
5. Manter os frameworks e drivers apenas na camada mais externa
6. Garantir que nenhuma dependência atravesse para fora (respeitar a Dependency Rule)
7. Usar injeção de dependência para fornecer implementações concretas às interfaces

### Quais são as alternativas?
- Arquitetura em camadas tradicional (Layered Architecture)
- Hexagonal Architecture / Ports and Adapters
- Onion Architecture
- Arquitetura monolítica não estruturada
- Microservices
- Arquitetura baseada em eventos

### Quais são os trade-offs?
**Vantagens da Clean Architecture:**
- Independência de frameworks: regras de negócio não dependem de bibliotecas específicas
- Testabilidade: regras de negócio podem ser testadas sem web server, banco de dados ou qualquer framework
- Independência de UI: pode mudar interface sem mudar regras de negócio
- Independência de banco de dados: pode trocar de Oracle para MongoDB ou até para arquivos sem mudar regras de negócio
- Independência de agências externas: regras de negócio não sabem nada sobre o mundo exterior
- Alta coesão e baixo acoplamento entre componentes
- Fácil de entender e manter devido à separação clara de responsabilidades

**Desvantagens da Clean Architecture:**
- Sobrehead inicial de criação de interfaces e camadas
- Pode parecer excessivamente complexo para aplicações muito simples
- Requer disciplina da equipe para não violar a Dependency Rule
- Pode haver mais classes e interfaces do que em arquiteturas mais simples
- A curva de aprendizado pode ser íngreme para desenvolvedores unfamiliarizados com o conceito
- Em alguns casos, pode levar a indireção excessiva onde benefícios são mínimos

### Quando usar?
- Sistemas onde a vida útil é esperada para ser longa (anos ou décadas)
- Sistemas onde regras de negócio são complexas e valiosas por si mesmas
- Quando se quer garantir que o núcleo do negócio seja facilmente testável
- Quando se antecipa necessidade de mudar tecnologias (framework, banco de dados, UI)
- Quando se quer maximizar a reutilização de regras de negócio em diferentes contextos
- Sistemas críticos onde falhas de regras de negócio seriam catastróficas
- Quando a equipe valoriza limpeza e manutenibilidade a longo prazo sobre velocidade inicial

### Quando não usar?
- Protótipos ou provas de conceito onde velocidade de entrega é a única prioridade
- Aplicações muito simples onde o overhead não traz benefício proporcional
- Equipes que rejeitam fortemente a ideia de camadas e interfaces adicionais
- Quando se está em um ambiente altamente restrito onde cada classe conta (sistemas embarcados extremos)
- Quando se vai descartar o sistema após uso único ou muito limitado

### Quais são os erros mais comuns?
- Violentar a Dependency Rule (ter entidades que dependem de frameworks)
- Colocar regras de negócio nos adaptadores de interface ou frameworks
- Fazer casos de uso saberem detalhes específicos de apresentação ou banco de dados
- Não definir claramente o que é uma entidade versus um caso de uso
- Usar entidades como estruturas de dados anêmicas (apenas getters/setters sem comportamento)
- Criar camadas desnecessariamente quando uma arquitetura mais simples seria suficiente
- Esquecer de injetar dependências e fazer instantiation direto nas camadas internas
- Acreditar que Clean Architecture significa "nenhum framework" em vez de "não depender de detalhes de frameworks"

### Como isso afeta:
- *performance:* Ligeiramente pior devido a indireção e chamadas de interface (geralmente insignificante em aplicações de negócio)
- *escalabilidade:* Similar a outras arquiteturas monolíticas; escalabilidade depende de implementação específica
- *disponibilidade:* Similar a outras arquiteturas; depende de como é deployada
- *consistência:* Excelente para manter consistência de regras de negócio devido à isolamento
- *segurança:* Similar a outras arquiteturas; segurança é uma preocupação transversal
- *custo:* Custo inicial de desenvolvimento pode ser maior, mas custo de manutenção a longo prazo tende a ser menor
- *observabilidade:* Similar a outras arquiteturas; pode ser instrumentada normalmente
- *complexidade operacional:* Similar a outras arquiteturas monolíticas

### Exemplos reais de aplicação
- Sistemas de finanças onde regras de cálculo são críticas e duram décadas
- Sistemas de saúde onde regras de negócio clínico devem ser preservados independentemente de tecnologia
- Sistemas de controle industrial onde lógica de controle deve ser isolada de detalhes de hardware
- Muitos sistemas empresariais que evoluíram de arquiteturas em camadas para melhor separação de preocupações
- Aplicações onde houve necessidade de mudar de tecnologia (ex: de WebForms para MVC, de Swing para JavaFX) sem reescrever regras de negócio

### Exemplo simplificado
Estrutura básica de Clean Architecture para um sistema de gerenciamento de tarefas:
```
src/
├── main/
│   ├── java/
│   │   └── com/
│   │       └── example/
│   │           └── taskmanager/
│   │               ├── TaskManagerApplication.java
│   │               ├── domain/
│   │               │   ├── entities/
│   │               │   │   └── Task.java
│   │               │   └── usecases/
│   │               │       ├── AddTaskUseCase.java
│   │               │       ├── GetTaskUseCase.java
│   │               │       ├── UpdateTaskUseCase.java
│   │               │       └── DeleteTaskUseCase.java
│   │               ├── interfaceadapters/
│   │               │   ├── controllers/
│   │               │   │   ├── TaskController.java
│   │               │   │   └── TaskPresenter.java
│   │               │   ├── gateways/
│   │               │   │   ├── TaskRepository.java (interface)
│   │               │   │   └── TaskPresenter.java (interface)
│   │               │   └── mappers/
│   │               │       └── TaskMapper.java
│   │               └── frameworksdrivers/
│   │                   ├── web/
│   │                   │   ├── TaskRestController.java
│   │                   │   └── TaskWebController.java
│   │                   ├── persistence/
│   │                   │   ├── TaskRepositoryImpl.java (implementação JPA)
│   │                   │   └── DatabaseConfig.java
│   │                   └── webconfig/
│   │                       ├── SecurityConfig.java
│   │                   └── TaskRestApplication.java
│   └── resources/
│       └── application.properties
└── test/
    └── ... (testes unitários de use cases e entidades sem framework)
```

### Exemplo de sistema de produção
Um sistema de processamento de pedidos para e-commerce:
- **Entities:** Pedido, ItemPedido, Cliente, Pagamento (com regras de negócio como cálculo de total, validação de desconto, transição de status)
- **Use Cases:** CriarPedido, AdicionarItemPedido, ProcessarPagamento, CancelarPedido, ObterDetalhesPedido
- **Interface Adapters:** 
  - Controllers REST que recebem requisições HTTP
  - Presenters que formatam respostas para JSON/XML
  - Gateways de repositório (interfaces para PedidoRepository, ClienteRepository)
  - Mappers entre entidades e DTOs
- **Frameworks & Drivers:**
  - Camada web: Spring Boot MVC para endpoints REST
  - Camada de persistência: Hibernate/JPA com PostgreSQL
  - Camada de mensageria: integração com Apache Kafka para eventos de pedido
  - Camada de autenticação: Spring Security com JWT
  - Camada de entrada: Tomcat/Jetty como servidor web
- Testes de unidade para entities e use cases rodam sem nenhum framework
- Testes de integração testam a camada de frameworks com banco de dados em memória
- Deploy como único arquivo JAR ou container Docker

### Como esse assunto pode aparecer em uma entrevista.
> 🎯 **ENTREVISTA — ALTA FREQUÊNCIA**
> 
> "Explique como você aplicaria os princípios da Clean Architecture a um sistema de reservas de restaurantes."
> 
> **Armadilha:** Focar apenas na estrutura de pastas sem explicar como as regras de negócio ficam independentes.
> 
> **Como raciocinar:** Descrever entidades como Reserva, Cliente, Mesa com regras de negócio (validação de horário, conflito de mesas, políticas de cancelamento). Explicar casos de uso como FazerReserva, CancelarReserva, ConsultarDisponibilidade. Mostrar como os adaptadores de interface lidam com HTTP, banco de dados e formatação de resposta, enquanto os use cases permanecem puramente focados nas regras de negócio sem saber se estão talking to um web app ou app móvel.

## Dependency Rule (Regra de Dependência)

> 🎯 **ENTREVISTA — ALTA FREQUÊNCIA**
> 
> A Dependency Rule é o cerne da Clean Architecture e frequentemente perguntada em entrevistas para testar entendimento de acoplamento e independência.

### definição
A Dependency Rule estabelece que o código nos círculos internos (entidades e casos de uso) nunca pode depender de código nos círculos externos (adaptadores de interface, frameworks e drivers). Dependências só podem apontar para dentro, nunca para fora.

### Por que existe?
Para garantir que as regras de negócio puro permaneçam independentes de detalhes técnicos específicos, tornando-as testáveis, manteníveis e reutilizáveis.

### Como funciona internamente?
- Entidades (círculo mais interno) não dependem de nada além de talvez outras entidades
- Casos de uso dependem apenas de entidades e de interfaces definidas nos próprios casos de uso
- Adaptadores de interface dependem de casos de uso e de entidades, mas implementam interfaces definidas nos casos de uso
- Frameworks e drivers dependem de tudo o que está interno, mas não são dependidos por nada interno
- Comunicação entre camadas ocorre através de injeção de dependência de implementações concretas para interfaces abstratas definidas em círculos internos

### Como implementar?
1. Definir todas as interfaces necessárias nos círculos internos (casos de uso ou entidades)
2. Nunca permitir que classes em círculos internos tenham importações de pacotes externos
3. Usar injeção de dependência (construtor, setter ou método) para fornecer implementações concretas
4. Manter os pacotes externos (frameworks, bibliotecas específicas) apenas nos círculos externos
5. Usar ferramentas de arquitetura (como ArchUnit) para testar que a Dependency Rule não é violada
6. Em revisões de código, verificar que nenhuma classe interna referencia pacotes externos

### Quais são as alternativas?
- Permitir dependências livres (arquitetura tradicional sem restrições)
- Dependências apenas em uma direção (como em arquitetura em camadas tradicional)
- Dependências bidirecionais controladas (como em algumas arquiteturas de plugins)
- Nenhuma dependência explícita (sistema totalmente acoplado - "big ball of mud")

### Quais são os trade-offs?
**Vantagens de respeitar a Dependency Rule:**
- Entidades e casos de uso são totalmente testáveis sem mocks pesados ou inicialização de frameworks
- Regras de negócio podem ser reutilizadas em diferentes contextos (web app, app móvel, linha de comando)
- Facilidade de mudar tecnologias sem afetar regras de negócio
- Clareza na compreensão do que constitui regra de negócio puro
- Redução do risco de efeito colateral ao mudar detalhes técnicos

**Desvantagens/custos:**
- Sobrehead inicial de definição de interfaces
- Necessidade de camadas adicionais de adaptadores
- Pode parecer indireto para desenvolvedores acostumados com acesso direto
- Requer disciplina da equipe para manter a regra
- Em casos muito simples, pode parecer excesso de formalismo

### Quando usar?
- Sempre que se quiser garantir independência de regras de negócio em relação a detalhes técnicos
- Sistemas onde regras de negócio são o ativo mais valioso e devem ser preservados
- Quando se antecipa mudança de tecnologia (framework, banco de dados, UI)
- Sistemas que devem ter longa vida útil e passar por múltiplas evoluções tecnológicas
- Quando se quer maximizar testabilidade das regras de negócio

### Quando não usar?
- Protótipos descartáveis onde velocidade é a única preocupação
- Sistemas extremamente simples onde o overhead não traz benefício
- Quando a equipe não está disposta a seguir a disciplina necessária
- Em ambientes altamente restritos onde cada byte conta (sistemas embarcados ultra-restritos)

### Quais são os erros mais comuns?
- Entidades que importam pacotes de framework (ex: Entity com anotações JPA diretamente na classe)
- Casos de uso que conhecem detalhes de apresentação (ex: uso de HttpServletRequest)
- Adaptadores de interface que vazam detalhes externos para casos de uso (ex: retornando entidades específicas de ORM)
- Falta de interfaces nos casos de uso para dependências externas
- Injeção de dependência feita incorretamente (criando instâncias diretamente em vez de injetar)
- Acreditar que usar interfaces automaticamente respeita a Dependency Rule (quando as implementações ainda são referenciadas incorretamente)

### Como isso afeta:
- *performance:* Impacto mínimogeralmente <1% devido a chamada de interface indireta
- *escalabilidade:* Nenhum impacto direto; depende de como o sistema é implementado e deployado
- *disponibilidade:* Nenhum impacto direto
- *consistência:* Melhora consistência de regras de negócio devido ao isolamento
- *segurança:* Similar; regras de negócio puro são mais fáceis de auditar
- *custo:* Custo inicial de desenvolvimento ligeiramente maior, mas custo de manutenção a longo prazo menor
- *observabilidade:* Pode ser instrumentada normalmente; regras de negócio puro são pontos claros para logging de negócio
- *complexidade operacional:* Similar a outras arquiteturas bem estruturadas

### Exemplos reais de aplicação
- Sistema de negociação de ações onde regras de cálculo de lucro/prejuízo devem ser independentes de corretora ou plataforma de trading
- Sistema de prontuário eletrônico onde regras clínicas devem ser preservadas independentemente do sistema de saúde ou país
- Sistema de controle de tráfego aéreo onde lógica de separação de aeronaves deve ser independente do radar ou sistema de comunicação específico

### Exemplo simplificado
Violando a Dependency Rule:
```java
// ❌ ERRADO: Entidade depende de framework (JPA)
@Entity
public class Task {
    @Id @GeneratedValue
    private Long id;
    
    private String title;
    private boolean completed;
    
    // Getters e setters
}
```

Respeitando a Dependency Rule:
```java
// ✅ CORRETO: Entidade pura sem dependências de framework
public class Task {
    private Long id;
    private String title;
    private boolean completed;
    
    // Construtor, getters, métodos de negócio
    public Task(Long id, String title, boolean completed) {
        this.id = id;
        this.title = title;
        this.completed = completed;
    }
    
    public void markAsCompleted() {
        this.completed = true;
    }
    
    // Getters apenas se necessário para casos de uso
    public Long getId() { return id; }
    public String getTitle() { return title; }
    public boolean isCompleted() { return completed; }
}
```

Casos de uso dependendo apenas de interfaces:
```java
// ✅ CORRETO: Caso de uso depende apenas de entidades e interfaces
public class AddTaskUseCase {
    private final TaskRepository taskRepository; // Interface definida neste pacote
    
    public AddTaskUseCase(TaskRepository taskRepository) {
        this.taskRepository = taskRepository;
    }
    
    public Task execute(String title) {
        Task task = new Task(null, title, false);
        return taskRepository.save(task);
    }
}
```

### Exemplo de sistema de produção
Plataforma de reservas de hotéis com regras de negócio complexas:
- Entidade Reserva com regras de negócio como:
  - Não permitir reserva para datas passadas
  - Validar que horário de check-out é após check-in
  - Aplicar taxas de cancelamento baseado em antecedência
  - Calcular total incluindo impostos e taxas de serviço
- Caso de uso FazerReserva que:
  - Valida disponibilidade através de interface de repositório
  - Cria entidade Reserva aplicando todas as regras de negócio
  - Salva através do repositório
  - Não sabe se está talking to um banco de dados SQL, NoSQL ou serviço externo
- Adaptadores de interface que:
  - Implementam TaskRepository usando Hibernate com PostgreSQL
  - Ou implementam usando cliente de serviço REST para microservice de reservas
  - Ou implementam usando arquivo CSV para modo de teste
- Frameworks que:
  - Spring Boot MVC expõe endpoints REST
  - Hibernate implementa a interface de repositório
  - Nunca são referenciados diretamente pelos casos de uso ou entidades

### Como esse assunto pode aparecer em uma entrevista.
> 🎯 **ENTREVISTA — ALTA FREQUÊNCIA**
> 
> "Como você garantiria que as regras de negócio de um sistema de pagamento sejam independentes do banco de dados utilizado?"
> 
> **Armadilha:** Sugerir apenas usar um ORM sem considerar que anotações ou chamadas específicas ainda criam acoplamento.
> 
> **Como raciocinar:** Definir entidades de pagamento pura (sem anotações JPA ou similares), criar casos de uso que dependem apenas de interfaces de repositório, implementar essas interfaces nos adaptadores usando a tecnologia escolhida (JPA, MongoDB driver, etc.), garantir que nenhuma regra de negócio esteja nos adaptadores ou frameworks, testar casos de uso em isolamento com mocks ou implementações em memória.

## Entities (Entidades)

> 🎯 **ENTREVISTA — FREQUENTE**
> 
> Entidades são frequentemente confundidas com simples estruturas de dados; entrevistadores querem ver se você entende que elas devem conter comportamento de negócio.

### definição
Entities são objetos que encapsulam regras de negócio crítico e de alto nível que são verdadeiros independentemente de qualquer aplicação específica. Elas representam conceitos de negócio que seriam os mesmos mesmo se o sistema fosse usado de uma maneira completamente diferente.

### Por que existe?
Para separar o que é verdadeiramente fundamental sobre um conceito de negócio dos detalhes específicos de como ele é usado em uma aplicação particular.

### Como funciona internamente?
- Contêm dados que representam estado de negócio
- Incluem métodos que implementam regras de negócio intrínsecos ao conceito
- São responsáveis por si mesmos (encapsulamento): sabem como validar seu próprio estado
- Não dependem de nenhum framework específico ou detalhe técnico
- Pode ser compartilhada entre múltiplas aplicações ou usos do mesmo conceito de negócio
- Geralmente são poucas em número comparado a casos de uso
- Mudam menos frequentemente que casos de uso ou adaptadores de interface

### Como implementar?
1. Identificar conceitos de negócio que são verdadeiros independentemente do contexto de aplicação
2. Definir atributos que representam o estado essencial desses conceitos
3. Implementar métodos que representam regras de negócio intrínsecos (validação, transições de estado, cálculos)
4. Garantir que a entidade seja responsável por validar seu próprio estado
5. Evitar getters/setters públicos que expõem estado interno sem controle (preferir imutabilidade ou métodos com semântica clara)
6. Não incluir anotações de framework ou dependências específicas
7. Manter foco no que o conceito de negócio é, não em como é armazenado ou exibido

### Quais são as alternativas?
- Estruturas de dados anêmicas (apenas getters/setters sem comportamento)
- Objetos que são apenas DTOs (Data Transfer Objects) para transferir dados entre camadas
- Tabelas de banco de dados ou estruturas de dados puras
- Funções puras que operam em estruturas de dados
- Classes de serviço que contém a lógica (misturando aplicação e domínio)

### Quais são os trade-offs?
**Vantagens de entidades com comportamento:**
- Regras de negócio ficam localizadas onde os dados residem (princípio do especialista em informações)
- Menos probabilidade de regras de negócio serem duplicadas ou inconsistentes
- Mais fácil de entender e manter regras de negócio relacionadas a um conceito
- Entidades podem ser reutilizadas em diferentes contextos de aplicação
- Testabilidade melhorada pois regras estão isoladas com os dados que as afetam

**Desvantagens/custos:**
- Pode parecer estranho para desenvolvedores acostumados com modelo anêmico ou de serviços
- Risco de entidades ficarem muito grandes se não houver bom limite de responsabilidade
- Pode haver tensão entre pureza de entidade e necessidade de eficiência em alguns casos
- Requer mudança de mentalidade para desenvolvedores acostumados com camadas tradicionais

### Quando usar?
- Sempre que houver um conceito de negócio com regras intrínsecas que não dependem de como é usado
- Quando se quer garantir que regras de negócio críticas não sejam espalhadas pelo código
- Quando múltiplas partes do sistema precisam usar o mesmo conceito de negócio com consistência
- Quando se quer maximizar a coesão e minimizar o acoplamento relacionado a conceitos de negócio
- Quando se quer facilitar o teste de regras de negócio isoladamente

### Quando não usar?
- Quando o conceito é puramente estrutural sem regras de negócio intrínsecas (ex: simples registro de log)
- Quando se está em um contexto onde desempenho crítico exige acesso direto a estruturas de dados
- Quando o conceito é tão simples que comportamento adicionaria complexidade desnecessária
- Quando se está construindo uma camada de apresentação pura onde o conceito é apenas para exibição
- Quando se está trabalhando com dados que são puramente de entrada/saída sem significado de negócio intrínseco

### Quais são os erros mais comuns?
- Fazer entidades anêmicas (apenas dados com getters/setters, toda lógica em serviços)
- Colocar regras de negócio de aplicação nas entidades (regras que são específicas de como é usado neste sistema)
- Fazer entidades dependentes de frameworks (anotações JPA, etc.)
- Expor estado interno demais através de getters/setters públicos
- Não encapsular validação de estado (permitir que entidades fiquem em estado inconsistente)
- Fazer entidades imutáveis quando precisam mudar estado como parte de suas regras de negócio
- Confundir entidades com tabelas de banco de dados ou estruturas de armazenamento

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

### Exemplo simplificado
Entidade anêmica (errada):
```java
// ❌ ERRADO: Apenas dados, toda lógica ficaria em serviços ou casos de uso
public class Task {
    private Long id;
    private String title;
    private boolean completed;
    
    // Getters e setters públicos
    public Long getId() { return id; }
    public void setId(Long id) { this.id = id; }
    public String getTitle() { return title; }
    public void setTitle(String title) { this.title = title; }
    public boolean isCompleted() { return completed; }
    public void setCompleted(boolean completed) { this.completed = completed; }
}
```

Entidade com comportamento (correta):
```java
// ✅ CORRETO: Contém regras de negócio intrínsecas
public class Task {
    private Long id;
    private String title;
    private boolean completed;
    
    public Task(Long id, String title, boolean completed) {
        this.id = id;
        this.title = title;
        this.completed = completed;
        validate(); // Validação no construtor
    }
    
    // Métodos que representam regras de negócio
    public void markAsCompleted() {
        if (completed) {
            throw new IllegalStateException("Task já está concluída");
        }
        this.completed = true;
    }
    
    public void reopen() {
        if (!completed) {
            throw new IllegalStateException("Task não está concluída para ser reaberta");
        }
        this.completed = false;
    }
    
    public boolean canBeEdited() {
        // Regra de negócio: tarefas concluídas não podem ser editadas
        return !completed;
    }
    
    // Validação intrínseca
    private void validate() {
        if (title == null || title.trim().isEmpty()) {
            throw new IllegalArgumentException("Título não pode ser vazio");
        }
    }
    
    // Getters apenas (não setters públicos para preservar encapsulamento)
    public Long getId() { return id; }
    public String getTitle() { return title; }
    public boolean isCompleted() { return completed; }
}
```

### Exemplo de sistema de produção
Sistema de gestão hospitalar:
- **Entidade Paciente:**
  - Atributos: id, nome, dataDeNascimento, sexo, prontuario
  - Métodos de negócio:
    - `getIdade()`: calcula idade baseado na data de nascimento e data atual
    - `getTipoSanguineo()`: retorna tipo sanguíneo com validação
    - `podeFazerExame(TipoExame exame)`: verifica restrições baseado em histórico e condições
    - `getContatoEmergencia()`: retorna informações de contato para emergências
  - Validação intrínseca:
    - Nome não pode ser vazio
    - Data de nascimento não pode ser no futuro
    - Sexo deve ser um valor válido
    - Prontuário deve seguir formato específico
- **Entidade Consulta:**
  - Atributos: id, paciente, medico, dataHora, tipo, status
  - Métodos de negócio:
    - `getDuracaoEstimada()`: retorna duração baseada no tipo de consulta
    - `podeSerRemarcada()`: verifica políticas de remarcagem baseado em antecedência e tipo
    - `getValor()`: calcula valor baseado no tipo, convênio e coparticipação
    - `cancelar()`: aplica regras de cancelamento incluindo taxas e notificações
  - Validação intrínseca:
    - Data/hora não pode ser no passado (exceto para consultas já realizadas)
    - Paciente e médico devem ser válidos e ativos
    - Tipo de consulta deve estar na lista permitida para aquele médico

### Como esse assunto pode aparecer em uma entrevista.
> 🎯 **ENTREVISTA — FREQUENTE**
> 
> "Descreva como você modelaria o conceito de 'Conta Bancária' como uma entidade em um sistema financeiro."
> 
> **Armadilha:** Fazer uma classe apenas com campos como numero, saldo, titular e deixar toda a lógica de saque, depósito e transferência em serviços.
> 
> **Como raciocinar:** Definir a classe ContaBancária com métodos que representam regras de negócio intrínsecos: `sacar(valor)` que verifica limite e saldo, `depositar(valor)` que valida valor positivo, `transferirPara(ContaBancaria destino, valor)` que verifica se é a mesma conta ou conta válida, `getSaldoDisponivel()` que considera limites e bloqueios. Incluir validação no construtor e setters controlados para garantir que a entidade sempre esteja em estado válido.

## Use Cases (Casos de Uso)

> 🎯 **ENTREVISTA — ALTA FREQUÊNCIA**
> 
> Casos de uso são onde as regras de negócio específico da aplicação são implementadas; entrevistadores querem ver se você entende a diferença entre entidades (regras gerais) e casos de uso (regras específicas da aplicação).

### definição
Use Cases (ou Interactors) contêm as regras de negócio específico da aplicação que orquestram entidades para atender aos requisitos funcionais. Eles representam o que o sistema faz em termos de comportamento de negócio que é específico deste contexto de aplicação.

### Por que existe?
Para separar o que é específico de como este sistema particular usa as entidades das regras que são verdadeiras independentemente do contexto.

### Como funciona internamente?
- Orquestram chamadas entre entidades para alcançar um objetivo de negócio específico
- Contêm regras de negócio que variam com como o sistema é usado (não são verdadeiras para todas as aplicações das mesmas entidades)
- Dependem apenas de entidades e de interfaces definidas nos próprios casos de uso (para dependências externas)
- Não conhecem detalhes de apresentação, banco de dados ou frameworks específicos
- Geralmente têm uma entrada (request) e uma saída (response) bem definidas
- São responsáveis por aplicar regras de negócio de aplicação como validações, transações e fluxos de trabalho
- Não contêm lógica de apresentação ou detalhes de armazenamento

### Como implementar?
1. Identificar os principais objetivos de negócio que o sistema precisa atender (casos de uso)
2. Para cada caso de uso, definir uma classe com método execute() que implementa a lógica
3. Definir claramente os dados de entrada (request) e saída (response) que o caso de uso consome e produz
4. Dependem apenas de entidades e de interfaces (repositórios, apresentadores, serviços externos) definidas no mesmo pacote
5. Orquestram entidades para aplicar regras de negócio e alcançar o objetivo
6. Tratam erros e exceções de acordo com regras de negócio de aplicação
7. Não sabem como os dados serão apresentados ou armazenados (isso é responsabilidade dos adaptadores)
8. Podem ser implementados como objetos imutáveis com dependências injetadas via construtor

### Quais são as alternativas?
- Colocar toda a lógica nos controladores ou apresentadores (misturando aplicação e apresentação)
- Fazer entidades responsáveis por toda a lógica (overcarregando entidades com regras específicas da aplicação)
- Colocar lógica em serviços genéricos sem clara associação a objetivos de negócio
- Usar scripts ou procedimentos sem estrutura orientada a objetos
- Misturar lógica de aplicação com detalhes de infraestrutura

### Quais são os trade-offs?
**Vantagens de casos de uso bem definidos:**
- Clareza sobre o que o sistema faz em termos de comportamento de negócio
- Facilidade de testar regras de negócio de aplicação em isolamento
- Independência de detalhes de apresentação e armazenamento
- Facilidade de reutilizar em diferentes contextos (web, mobile, linha de comando) se interfaces forem estáveis
- Melhor compreensão do fluxo de negócio através da nomeação clara dos casos de uso
- Facilidade de mudar como algo é feito sem mudar o que é feito (desde que entrada/saída permaneçam semelhantes)

**Desvantagens/custos:**
- Sobrehead de criação de classes adicionais
- Pode parecer indireto para fluxos muito simples
- Requer disciplina para não vazar detalhes externos nos casos de uso
- Pode haver sobreposição ou lacunas se não forem bem delimitados
- Necessidade de definir claramente entrada e saída para cada caso de uso

### Quando usar?
- Sempre que houver um objetivo de negócio claro que envolve múltiplas entidades ou regras de coordenação
- Quando se quer separar claramente regras de negócio de aplicação das regras de negócio geral (entidades)
- Quando se quer garantir que o núcleo do sistema seja testável sem UI ou banco de dados
- Quando múltiplas interfaces (web, API, linha de comando) precisam acessar a mesma lógica de negócio
- Quando se quer facilitar a compreensão e manutenção do sistema através de nomes claros de funcionalidades

### Quando não usar?
- Quando o sistema é tão simples que cada ação é uma operação direta em uma entidade
- Quando se está construindo uma camada de apresentação pura onde não há lógica de negócio de aplicação
- Quando o overhead de classes e interfaces não traz benefício proporcional ao tamanho do sistema
- Quando se está em um contexto onde desempenho crítico exige código inline sem abstração
- Quando se está prototipando e velocidade é a única preocupação

### Quais são os erros mais comuns?
- Casos de uso que conhecem detalhes de apresentação (ex: uso de HttpServletRequest, ModelAndView)
- Casos de uso que conhecem detalhes de armazenamento (ex: uso direto de EntityManager ou JDBC)
- Casos de uso que contêm lógica de apresentação (formatação de dados para exibição)
- Falta de clara delimitação entre o que é caso de uso e o que é entidade
- Casos de uso que são muito grandes e fazem demasiado (violando princípio da responsabilidade única)
- Casos de uso que dependem de implementações concretas em vez de interfaces
- Não tratar adequadamente casos de erro ou borda de acordo com regras de negócio

### Como isso afeta:
- *performance:* Impacto mínimogeralmente <1% devido a chamada de método indireto
- *escalabilidade:* Nenhum impacto direto
- *disponibilidade:* Nenhum impacto direto
- *consistência:* Melhora pois regras de negócio de aplicação ficam localizadas e menos propensas a inconsistência
- *segurança:* Similar; casos de uso podem aplicar validações e controle de acesso de negócio
- *custo:* Similar; foco em onde a lógica de aplicação reside
- *observabilidade:* Melhora pois pontos claros de lógica de negócio facilitam logging e tracing
- *complexidade operacional:* Similar; pode reduzir bugs devido a melhor separação de responsabilidades

### Exemplos reais de aplicação
- Caso de uso "Processar Pagamento" em sistema de e-commerce que:
  - Valida detalhes do cartão
  - Verifica limite disponível
  - Chama gateway de pagamento
  - Atualiza status do pedido
  - Gera recibo
- Caso de uso "Reservar Voo" em sistema de companhia aérea que:
  - Verifica disponibilidade de assento
  - Aplica regras de tarifas e descontos
  - Cria reserva com passageiros
  - Processa pagamento se necessário
  - Envia confirmação
- Caso de uso "Autenticar Usuário" que:
  - Valida credenciais
  - Verifica se conta está ativa e não bloqueada
  - Aplica políticas de tentativas e bloqueio
  - Gera token de sessão
  - Atualiza último login

### Exemplo simplificado
Caso de uso sem independência (errado):
```java
// ❌ ERRADO: Caso de uso conhece detalhes de apresentação e armazenamento
@Stateless
public class TaskService {
    @PersistenceContext
    private EntityManager em;
    
    public void addTask(HttpServletRequest request, Model model) {
        String title = request.getParameter("title");
        // Lógica de apresentação misturada com negócio
        if (title == null || title.isEmpty()) {
            model.addAttribute("error", "Título é obrigatório");
            return;
        }
        
        Task task = new Task();
        task.setTitle(title);
        task.setCompleted(false);
        em.persist(task); // Dependência direta em JPA
        
        // Lógica de navegação misturada
        model.addAttribute("task", task);
        // Redirecionamento ou forward implícito
    }
}
```

Caso de uso com independência (correto):
```java
// ✅ CORRETO: Caso de uso depende apenas de entidades e interfaces
public class AddTaskUseCase {
    private final TaskRepository taskRepository; // Interface definida neste pacote
    
    public AddTaskUseCase(TaskRepository taskRepository) {
        this.taskRepository = taskRepository;
    }
    
    // Entrada e saída bem definidas
    public TaskResponse execute(TaskRequest request) {
        // Validação de negócio de aplicação
        if (request.getTitle() == null || request.getTitle().trim().isEmpty()) {
            throw new IllegalArgumentException("Título é obrigatório");
        }
        
        // Orquestração de entidades usando apenas regras de negócio
        Task task = new Task(null, request.getTitle().trim(), false);
        Task savedTask = taskRepository.save(task);
        
        // Retorna apenas dados de negócio, não detalhes de apresentação
        return new TaskResponse(
            savedTask.getId(),
            savedTask.getTitle(),
            savedTask.isCompleted()
        );
    }
}
```

### Exemplo de sistema de produção
Sistema de gestão de bibliotecas:
- **Caso de uso EmprestarLivro:**
  - Entrada: EmprestarLivroRequest (ID do livro, ID do usuário, data de empréstimo)
  - Saída: EmprestarLivroResponse (ID do empréstimo, data de devolução prevista, multa prevista se houver)
  - Lógica:
    - Valida que livro existe e está disponível
    - Valida que usuário existe e não tem bloqueios
    - Calcula data de devolução prevista baseado em tipo de livro e políticas
    - Verifica se usuário já não tem máximo de empréstimos permitido
    - Cria entidade Empréstimo com os dados
    - Salva através do repositório de empréstimos
    - Atualiza status do livro para "emprestado"
    - Retorna dados de saída sem detalhes de como serão apresentados
- **Caso de uso DevolverLivro:**
  - Entrada: DevolverLivroRequest (ID do empréstimo)
  - Saída: DevolverLivroResponse (multa aplicada, se houver)
  - Lógica:
    - Valida que empréstimo existe e está ativo
    - Calcula multa baseado em data de devolução real vs prevista
    - Atualiza status do empréstimo para "devolvido"
    - Atualiza status do livro para "disponível"
    - Notifica usuários em fila de reserva se aplicável
    - Retorna resultado sem detalhes de apresentação

### Como esse assunto pode aparecer em uma entrevista.
> 🎯 **ENTREVISTA — ALTA FREQUÊNCIA**
> 
> "Explique a diferença entre uma entidade e um caso de uso no contexto de um sistema de reservas de hotéis."
> 
> **Armadilha:** Confundir os dois ou sugerir que casos de uso são apenas métodos nas entidades.
> 
> **Como raciocinar:** Entidades representam conceitos de negócio geral com regras intrínsecas (ex: Reserva sabe validar datas, calcular total, aplicar políticas de cancelamento). Casos de uso representam fluxos específicos da aplicação (ex: FazerReserva orquestra verificação de disponibilidade, criação de reserva, processamento de pagamento, envio de confirmação). Entidades são reutilizáveis em diferentes contextos; casos de uso são específicos de como este sistema particular usa as entidades.

## Interface Adapters (Adaptadores de Interface)

> 💡 **DICA DE ENTREVISTA**
> 
> Adaptadores de interface são onde a Clean Architecture se conecta ao mundo exterior; entrevistadores querem ver se você entende seu papel na conversão de dados e isolamento de detalhes técnicos.

### definição
Interface Adapters convertem dados entre o formato mais conveniente para casos de uso e entidades e o formato mais conveniente para bancos de dados, web, ou outros agentes externos. Eles são o colante que une a arquitetura limpa ao mundo dos detalhes técnicos sem deixar que esses detalhes vazem para os círculos internos.

### Por que existe?
Para traduzir entre o mundo interno limpo (onde regras de negógio são puras) e o mundo externo bagunçado (onde temos SQL, HTTP, JSON, UI, etc.) sem comprometer a independência dos círculos internos.

### Como funciona internamente?
- Implementam interfaces definidas nos casos de uso ou entidades (repositórios, apresentadores, gateways)
- Recebem dados de agentes externos (requisições HTTP, resultados de banco de dados, mensagens) e os convertem para formato útil para casos de uso
- Recebem dados de casos de uso e os convertem para formato útil para agentes externos (respostas HTTP, comandos de banco de dados, mensagens para envio)
- Contêm os detalhes específicos de tecnologia (frameworks web, drivers de banco de dados, bibliotecas de mensageria)
- Nunca contêm regras de negócio (isso ficaria nos casos de uso ou entidades)
- São responsáveis por preocupações como mapeamento objeto-relacional, serialização JSON, validação de entrada de formulário, etc.

### Como implementar?
1. Identificar as interfaces necessárias definidas nos casos de uso (repositórios, apresentadores, serviços externos)
2. Criar implementações concretas dessas interfaces usando tecnologias específicas
3. Converter dados entre formato de caso de uso e formato externo (DTOs para entidades, entidades para DTOs, etc.)
4. Tratar preocupações específicas da tecnologia (conexões de banco de dados, sessões web, parsing de JSON, etc.)
5. Nunca colocar regras de negócio nestas classes
6. Garantir que dependências somente apontem para dentro (usem entidades e casos de uso, não sejam usados por eles)
7. Usar padrões como mappers, converters ou builders para facilitar a conversão de dados

### Quais são as alternativas?
- Misturar lógica de negócio com detalhes técnicos (violando separação de preocupações)
- Fazer casos de uso conhecerem diretamente detalhes de tecnologia
- Deixar entidades responsáveis por conversão de formato
- Usar camadas de serviço genéricas que misturam aplicação e tecnologia
- Não ter camada de adaptação e deixar casos de uso lidarem diretamente com detalhes técnicos

### Quais são os trade-offs?
**Vantagens de adaptadores de interface bem definidos:**
- Isolamento completo de regras de negócio de detalhes técnicos
- Facilidade de mudar tecnologias sem afetar regras de negócio
- Testabilidade melhorada pois casos de uso podem ser testados com mocks ou implementações em memória
- Clareza sobre onde ficam os detalhes específicos de cada tecnologia
- Facilidade de suportar múltiplos tipos de entrada/saída (web, mobile, linha de comando, batch) com mesma lógica de negócio
- Facilidade de testar adaptações específicas isoladamente

**Desvantagens/custos:**
- Sobrehead de criação de classes de adaptação e conversão
- Pode parecer indireto para desenvolvedores acostumados com acesso direto
- Requer disciplina para manter a camada pura (nenhuma regra de negócio)
- Pode haver sobrehead de conversão de dados entre formatos
- Necessidade de gerenciar múltiplas implementações quando se suporta múltiplas tecnologias

### Quando usar?
- Sempre que se quiser manter regras de negócio independentes de detalhes de apresentação, armazenamento ou comunicação
- Quando múltiplas interfaces externas precisam acessar a mesma lógica de negócio
- Quando se antecipa necessidade de mudar tecnologia de apresentação ou armazenamento
- Quando se quer garantir que casos de uso sejam testáveis sem inicializar frameworks completos
- Quando se quer suportar diferentes tipos de clientes (web app, app móvel, API de terceiros) com mesma lógica de negócio

### Quando não usar?
- Quando o sistema é tão simples que o overhead não traz benefício proporcional
- Quando se está construindo um protótipo descartável onde velocidade é a única prioridade
- Quando se está em um contexto onde acesso direto a tecnologia é considerado aceitável ou até preferível
- Quando se está construindo uma camada de apresentação pura onde não há necessidade de lógica de negócio
- Quando se está em um ambiente altamente restrito onde cada classe conta e conversão de dados é proibida

### Quais são os erros mais comuns?
- Colocar regras de negócio nos adaptadores de interface (ex: validação de negócio em um controller)
- Fazer adaptadores vazarem detalhes externos para casos de uso (ex: retornar entidades específicas de ORM em vez de DTOs estáveis)
- Não definir claramente as interfaces nos casos de uso, levando a adaptadores que não correspondem a nada interno
- Fazer adaptações conhecerem demais sobre o contexto interno (acoplamento reverso)
- Tratar adaptadores como locais genéricos para qualquer código que não se encaixe em outro lugar
- Esquecer de injetar dependências e fazer instantiation direto de tecnologias específicas

### Como isso afeta:
- *performance:* Impacto moderado devido a conversão de dados e chamadas de framework (pode ser significativo em alto volume)
- *escalabilidade:* Similar a outras arquiteturas; depende de implementação específica de adaptadores
- *disponibilidade:* Similar; falhas em adaptadores afetam camada externa mas não necessariamente regras de negócio interno
- *consistência:* Similar; depende de como adaptadores lidam com transições e erros
- *segurança:* Adaptadores são onde muita segurança de entrada acontece (validação, sanitização)
- *custo:* Custo inicial maior devido a classes adicionais, mas custo de mudança tecnológica menor
- *observabilidade:* Melhora pois pontos claros de entrada/saída facilitam logging de monitoramento
- *complexidade operacional:* Similar; pode aumentar devido a mais componentes para gerenciar, mas reduz devido a melhor isolamento

### Exemplos reais de aplicação
- Adaptadores REST que:
  - Recebem requisições HTTP e JSON
  - Convertem para objetos de caso de uso (TaskRequest)
  - Chamam casos de uso
  - Convertem resultados de caso de uso para JSON de resposta
  - Nunca contêm lógica de negócio como cálculo de taxas ou validação de regras de domínio
- Adaptadores de banco de dados que:
  - Recebem entidades de caso de uso
  - Converte para comandos SQL ou operações de NoSQL
  - Executam operações de banco de dados
  - Converte resultados de banco de volta para entidades de caso de uso
  - Nunca contêm lógica de negócio como cálculo de totais ou aplicação de descontos
- Adaptadores de mensagem que:
  - Recebem mensagens de fila ou tópico
  - Converte para objetos de caso de uso
  - Chamam casos de uso
  - Converte resultados para mensagens de resposta
  - Nunca contêm lógica de negócio como processamento de lote ou aplicação de regras

### Exemplo simplificado
Adaptador violando independência (errado):
```java
// ❌ ERRADO: Controller sabe demais sobre caso de uso e contém lógica de negócio
@RestController
public class TaskController {
    @Autowired
    private AddTaskUseCase addTaskUseCase;
    
    @PostMapping("/tasks")
    public ResponseEntity<?> addTask(@RequestBody TaskRequest request) {
        // Lógica de negócio inadequadamente colocada aqui
        if (request.getTitle() == null || request.getTitle().isEmpty()) {
            return ResponseEntity.badRequest().body("Título é obrigatório");
        }
        
        // Chamada ao caso de uso
        TaskResponse response = addTaskUseCase.execute(request);
        
        // Mais lógica de negócio aqui
        if (response.isCompleted()) {
            // Faz algo específico baseado no resultado
        }
        
        return ResponseEntity.ok(response);
    }
}
```

Adaptador respeitando independência (correto):
```java
// ✅ CORRETO: Controller apenas traduz entre HTTP e caso de uso
@RestController
@RequestMapping("/tasks")
public class TaskController {
    private final AddTaskUseCase addTaskUseCase;
    private final GetTaskUseCase getTaskUseCase;
    
    public TaskController(AddTaskUseCase addTaskUseCase, GetTaskUseCase getTaskUseCase) {
        this.addTaskUseCase = addTaskUseCase;
        this.getTaskUseCase = getTaskUseCase;
    }
    
    @PostMapping
    public ResponseEntity<TaskResponse> addTask(@RequestBody TaskRequest request) {
        // Apenas traduz entre HTTP e caso de uso
        TaskResponse response = addTaskUseCase.execute(request);
        return ResponseEntity.ok(response);
    }
    
    @GetMapping("/{id}")
    public ResponseEntity<TaskResponse> getTask(@PathVariable Long id) {
        TaskResponse response = getTaskUseCase.execute(new GetTaskRequest(id));
        return ResponseEntity.ok(response);
    }
}
```

Mappers para conversão de dados:
```java
// ✅ CORRETO: Mapper apenas converte entre formato de caso de uso e entidade
public class TaskMapper {
    public static Task toEntity(TaskRequest request) {
        return new Task(null, request.getTitle().trim(), false);
    }
    
    public static TaskResponse toResponse(Task task) {
        return new TaskResponse(
            task.getId(),
            task.getTitle(),
            task.isCompleted()
        );
    }
    
    public static TaskRequest toRequest(TaskResponse response) {
        return new TaskRequest(response.getTitle());
    }
}
```

### Exemplo de sistema de produção
Sistema de processamento de pedidos com múltiplas interfaces:
- **Adaptadores Web (REST):**
  - PedidoController: recebe requisições HTTP/JSON, chama casos de uso como CriarPedido, ProcessarPagamento
  - PedidoPresenter: formata respostas JSON para clientes web e mobile
  - Nunca contém lógica como cálculo de frete ou validação de cupom (isso fica nos casos de uso)
- **Adaptadores de Banco de Dados:**
  - PedidoRepositoryImpl: implementa interface PedidoRepository usando Hibernate
  - Converte entidades de caso de uso (Pedido, ItemPedido) para operações SQL
  - Nunca contém lógica como cálculo de total ou aplicação de descontos
  - Usa apenas mapeamento objeto-relacional e controle de transação
- **Adaptadores de Mensageria:**
  - PedidoEventListener: recebe mensagens de Kafka sobre eventos de pedido
  - Converte para objetos de caso de uso como AtualizarStatusPedidoUseCase
  - Nunca contém lógica como regras de negócio para processamento de eventos
  - Apenas traduz entre formato de mensagem e formato de caso de uso
- **Adaptadores de Entrada em Lote:**
  - BatchPedidoProcessor: lê arquivos CSV ou XML de pedidos
  - Converte para objetos de caso de uso como ProcessarLotePedidoUseCase
  - Nunca contém lógica como validação de dados do lote ou aplicação de regras de negócio
  - Apenas traduz entre formato de arquivo e formato de caso de uso

### Como esse assunto pode aparecer em uma entrevista.
> 🎯 **ENTREVISTA — FREQUENTE**
> 
> "Explique como você estruturaria os adaptadores de interface para um sistema que precisa suportar tanto uma interface web quanto uma API móvel compartilhando a mesma lógica de negócio."
> 
> **Armadilha:** Sugerir duplicar lógica de negócio ou fazer casos de uso conhecerem detalhes específicos de web ou mobile.
> 
> **Como raciocinar:** Definir casos de uso pura que não sabem nada sobre como são acessados. Criar adaptadores separados para web e mobile que ambos implementam as mesmas interfaces definidas nos casos de uso (repositórios, apresentadores). Cada adaptador lida com sua tecnologia específica (Spring MVC para web, talvez um framework diferente para mobile) mas ambos chamam os mesmos casos de uso e recebem os mesmos tipos de dados de saída para formatar adequadamente para seu público-alvo.

## Frameworks & Drivers (Frameworks e Drivers)

> 💡 **DICA DE ENTREVISTA**
> 
> Esta é a camada mais externa onde ficam os detalhes específicos de tecnologia; entrevistadores querem ver se você entende que ela deve ser fina e não conter lógica de negócio.

### definição
Frameworks & Drivers são os detalhes específicos como frameworks web, bancos de dados, dispositivos externos, e interfaces de usuário. Esta é a camada mais externa onde ficam todos os detalhes que são específicos de tecnologia e que provavelmente mudarão ao longo da vida do sistema.

### Por que existe?
Para conter todos os detalhes específicos de tecnologia em um lugar onde eles não podem contaminar as regras de negócio puro nos círculos internos. Esta é onde "o detalhe malvado" vive, longe do coração limpo do sistema.

### Como funciona internamente?
- Contém frameworks específicos (Spring, Django, React, etc.)
- Contém drivers de banco de dados (Hibernate, JDBC, MongoDB driver, etc.)
- Contém interfaces de usuário específicos (Servlets, Controllers, Views, etc.)
- Contém configuração específica de tecnologia
- Pode conter adaptadores que implementam interfaces definidas nos círculos internos
- Nunca contém regras de negócio (isso ficaria nos casos de uso ou entidades)
- É responsável por preocupações como inicialização de framework, configuração de banco de dados, roteamento de URLs, etc.
- É onde o ponto de entrada do sistema geralmente reside (main method, aplicação web, etc.)

### Como implementar?
1. Manter esta camada o mais fina possível - apenas o necessário para fazer o sistema funcionar com as tecnologias escolhidas
2. Nunca colocar regras de negócio aqui
3. Usar esta camada apenas para:
   - Configurar e inicializar frameworks
   - Fornecer adaptadores que implementam interfaces dos círculos internos
   - Tratar preocupações específicas de tecnologia (conexões, sessões, parsing, etc.)
   - Fornecer ponto de entrada do sistema
4. Garantir que dependências somente apontem para dentro (dependam de casos de uso, entidades, adaptadores de interface)
5. Nunca permitir que esta camada seja dependida por círculos internos (isso violaria a Dependency Rule)
6. Usar injeção de dependência para fornecer implementações concretas de interfaces dos círculos internos

### Quais são as alternativas?
- Misturar frameworks com lógica de negócio em todo o código (arquitetura tradicional)
- Fazer entidades conhecerem diretamente detalhes de framework (anotações JPA em entidades)
- Deixar casos de uso dependerem de frameworks específicos
- Não ter distinção clara e deixar detalhes técnicos espalhados pelo código
- Fazer tudo em um único blob de código sem separação de preocupações

### Quais são os trade-offs?
**Vantagens de manter frameworks e drivers na camada externa:**
- Regras de negócio permanecem independentes de escolhas tecnológicas específicas
- Facilidade de mudar tecnologia sem afetar o núcleo do sistema
- Testabilidade melhorada pois núcleo pode ser testado sem frameworks
- Clareza sobre onde ficam os detalhes específicos de cada tecnologia
- Facilidade de suportar múltiplas tecnologias simultaneamente se necessário
- Menos risco de vazamento de detalhes técnicos para regras de negócio

**Desvantagens/custos:**
- Pode parecer que esta camada faz "pouco" em comparação com o resto
- Requer disciplina para manter limpa (nenhuma regra de negócio)
- Pode haver alguma indireção na chamada de framework para regra de negócio
- Necessidade de entender bem onde cada pertencimento vai
- Em sistemas muito simples, pode parecer excesso de formalismo

### Quando usar?
- Sempre que se usar qualquer framework, biblioteca ou tecnologia específica
- Quando se quiser garantir que escolha de tecnologia não aprisione regras de negócio
- Quando se antecipa necessidade de mudar tecnologia no futuro
- Quando se quer maximizar testabilidade e manutenibilidade a longo prazo
- Quando se quer evitar o problema de "framework lock-in" onde mudar tecnologia exige reescrita massiva

### Quando não usar?
- Quando se está construindo um sistema tão simples que usar qualquer framework seria overkill
- Quando se está em um ambiente onde apenas código puro é permitido (sistemas embarcados ultra-restritos)
- Quando se está prototipando e velocidade é a única prioridade absoluta
- Quando se está em um contexto onde o overhead de camadas não traz benefício proporcional
- Quando se está construindo um sistema onde a tecnologia é fixa para sempre e nunca mudará

### Quais são os erros mais comuns?
- Colocar regras de negócio em classes de framework (ex: entidades com anotações JPA, controllers com lógica de negócio)
- Fazer classes de framework dependerem de círculos internos de forma inadequada (violando Dependency Rule ao contrário)
- Tratar esta camada como local genérico para qualquer código difícil de colocar em outro lugar
- Esquecer de que esta camada deve ser apenas um adapter fino e acabar colocando lógica significativa aqui
- Anotar entidades diretamente com especificações de framework (fazendo-as dependentes de detalhes técnicos)
- Fazer pontos de entrada do sistema conhecerem demais sobre regras de negócio (devem ser finos delegadores)

### Como isso afeta:
- *performance:* Impacto depende da tecnologia específica, mas a camada em si adiciona pouco overhead além do framework em si
- *escalabilidade:* Similar a como a tecnologia específica escalaria; a camada de adaptação em si não adiciona muito
- *disponibilidade:* Similar a tecnologia específica; problemas aqui afetam acesso ao sistema mas não necessariamente regras de negócio interno
- *consistência:* Nenhum impacto direto
- *segurança:* Muita segurança de entrada acontece aqui (validação de entrada, autenticação, autorização de nível de framework)
- *custo:* Similar a tecnologia específica; custos de licença, desempenho, etc. vêm da tecnologia escolhida
- *observabilidade:* Melhora pois pontos claros de entrada/saída facilitam instrumentação de framework
- *complexidade operacional:* Similar a tecnologia específica; gerenciamento de framework acontece aqui

### Exemplos reais de aplicação
- Camada Spring Boot que:
  - Contém apenas classes de configuração (@Configuration)
  - Contém apenas controladores que delegam para casos de uso (@RestController)
  - Contém apenas classes que implementam interfaces de repositório (@Repository)
  - Nunca contém lógica como cálculo de totais, validação de descontos ou aplicação de regras de negócio
- Camada Hibernate que:
  - Contém apenas classes de mapeamento objeto-relacional (@Entity - mas atenção: isso é polêmico!)
  - Contém apenas repositórios que implementam interfaces definidas nos casos de uso
  - Nunca contém lógica de negócio como cálculo de idade ou aplicação de taxas
  - Nota: Mesmo @Entity pode ser considerado uma violação se a entidade tiver regras de negócio, por isso muitas vezes se usa entidades puras e mapeamento separado
- Camada de servidor web que:
  - Contém apenas código de inicialização e configuração do servidor
  - Contém apenas adaptadores que recebem requisições e chamam casos de uso
  - Nunca contém lógica como processamento de lote ou aplicação de regras de negócio

### Exemplo simplificado
Framework violando independência (errado):
```java
// ❌ ERRADO: Entidade com anotações JPA - depende de detalhes de framework
@Entity
@Table(name = "tasks")
public class Task {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    @Column(nullable = false)
    private String title;
    
    @Column(nullable = false)
    private boolean completed;
    
    // Getters e setters
    // ... (nenhuma lógica de negócio aqui, mas ainda depende de JPA)
}
```

Framework respeitando independência (correto):
```java
// ✅ CORRETO: Entidade pura sem qualquer dependência de framework
public class Task {
    private Long id;
    private String title;
    private boolean completed;
    
    public Task(Long id, String title, boolean completed) {
        this.id = id;
        this.title = title;
        this.completed = completed;
    }
    
    // Métodos de negócio puro
    public void markAsCompleted() {
        this.completed = true;
    }
    
    // Getters apenas
    public Long getId() { return id; }
    public String getTitle() { return title; }
    public boolean isCompleted() { return completed; }
}
```

Implementação de repositório na camada de frameworks:
```java
// ✅ CORRETO: Implementação que depende apenas de interfaces internas
@Repository
public class TaskRepositoryImpl implements TaskRepository {
    private final EntityManager entityManager; // Dependência em framework, mas OK aqui
    
    public TaskRepositoryImpl(EntityManager entityManager) {
        this.entityManager = entityManager;
    }
    
    @Override
    public Task save(Task task) {
        if (task.getId() == null) {
            entityManager.persist(task);
            return task;
        } else {
            return entityManager.merge(task);
        }
    }
    
    @Override
    public Optional<Task> findById(Long id) {
        return Optional.ofNullable(entityManager.find(Task.class, id));
    }
}
```

Ponto de entrada da aplicação:
```java
// ✅ CORRETO: Apenas configura e inicia framework, delega tudo para casos de uso
@SpringBootApplication
public class TaskManagerApplication {
    public static void main(String[] args) {
        SpringApplication.run(TaskManagerApplication.class, args);
    }
}

// Configuração que apenas conecta componentes
@Configuration
public class AppConfig {
    @Bean
    public TaskRepository taskRepository(EntityManager entityManager) {
        return new TaskRepositoryImpl(taskRepository);
    }
    
    @Bean
    public AddTaskUseCase addTaskUseCase(TaskRepository taskRepository) {
        return new AddTaskUseCase(taskRepository);
    }
    
    // Outros beans para casos de uso, adaptadores, etc.
}
```

### Exemplo de sistema de produção
Plataforma de streaming de vídeo:
- **Frameworks & Drivers Web:**
  - Spring Boot configuração mínima para expor endpoints REST
  - Controladores que apenas recebem requisições HTTP e delegam para casos de uso como IniciarReproducaoUseCase, PausarReproducaoUseCase
  - Nunca contém lógica como cálculo de bitrate, seleção de servidor de conteúdo ou aplicação de regras de DRM
- **Frameworks & Drivers de Persistência:**
  - Implementações de repositório usando Cassandra para dados de usuário e MongoDB para metadados de conteúdo
  - Apenas mapeamento objeto-documento e controle de conexão
  - Nunca contém lógica como cálculo de recomendações ou aplicação de políticas de licenciamento
- **Frameworks & Drivers de Mensageria:**
  - Consumidores e produtores Apache Kafka que apenas traduzem entre mensagens e objetos de caso de uso
  - Nunca contém lógica como processamento de eventos de usuário ou atualização de métricas
- **Frameworks & Drivers de Autenticação:**
  - Integração com OAuth2/OpenID Connect que apenas lida com troca de tokens e validação de assinatura
  - Nunca contém lógica como regras de negócio para concessão de acesso ou detecção de fraude
- **Ponto de entrada:**
  - Classe principal que apenas inicializa o Spring Boot
  - Toda a lógica de negócio está nos casos de uso e entidades, completamente isolada dos detalhes de streaming, codificação, entrega de conteúdo, etc.

### Como esse assunto pode aparecer em uma entrevista.
> 🎯 **ENTREVISTA — FREQUENTE**
> 
> "Descreva como você organizaria a camada de frameworks e drivers em um sistema de leilão online para garantir que ela não contenha lógica de negócio."
> 
> **Armadilha:** Sugerir que é aceitável colocar alguma lógica de negócio aqui desde que seja "pouca" ou "relacionada à tecnologia".
> 
> **Como raciocinar:** Explicar que esta camada deve conter apenas:
> - Configuração de inicialização do framework (Spring, etc.)
> - Adaptadores que implementam interfaces definidas nos círculos internos (controladores que chamam casos de uso, repositórios que implementam interfaces de repositório)
> - Código específico de tecnologia necessário para fazer o sistema funcionar (conexões de banco de dados, sessões web, etc.)
> - Ponto de entrada do sistema
> - Nenhuma regra de negócio como cálculo de lances, validação de usuários ou aplicação de regras de leilão
> Mostrar como qualquer tentativa de colocar lógica de negócio aqui iria violar a Dependency Rule e prejudicar testabilidade e independência tecnológica.

## Dependency Inversion (Inversão de Dependência)

> 🎯 **ENTREVISTA — ALTA FREQUÊNCIA**
> 
> A Inversão de Dependência é um princípio SOLID que é fundamental para a Clean Architecture; entrevistadores frequentemente testam entendimento desse conceito.

### definição
A Inversão de Dependência tem duas partes:
1. Módulos de alto nível não devem depender de módulos de baixo nível. Ambos devem depender de abstrações.
2. Abstrações não devem depender de detalhes. Detalhes devem depender de abstrações.

No contexto da Clean Architecture, isso significa que os círculos internos (altos nível: entidades, casos de uso) dependem de abstrações (interfaces), e os círculos externos (baixo nível: frameworks, drivers) implementam essas abstrações.

### Por que existe?
Para eliminar dependências diretas que tornam o código rígido, frágil e imutável. Sem inversão de dependência, mudar detalhes técnicos exigiria mudar regras de negócio, e reutilizar regras de negócio em diferentes contextos seria difícil ou impossível.

### Como funciona internamente?
- Círculos internos definem interfaces que representam serviços de que precisam (repositórios, apresentadores, gateways)
- Círculos internos dependem apenas dessas interfaces, não de implementações concretas
- Círculos externos fornecem implementações concretas dessas interfaces
- Comunicação ocorre através de injeção de dependência: implementações concretas são fornecidas aos círculos internos
- Nenhum círculo interno sabe quais implementações concretas estão sendo usadas
- Mudar implementação concreta não requer mudança em código interno

### Como implementar?
1. Definir todas as dependências necessárias como interfaces nos círculos internos (casos de uso ou entidades)
2. Nunca permitir que círculos internos tenham referências a classes concretas de círculos externos
3. Usar injeção de dependência (geralmente via construtor) para fornecer implementações concretas
4. Garantir que círculos externos implementem as interfaces definidas internamente
5. Usar containers de injeção de dependência (Spring, Guice, etc.) ou fazer manualmente
6. Em testes, fornecer implementações mock ou em memória das interfaces
7. Nunca permitir que detalhes vazem para dentro através das interfaces (ex: retornar tipos específicos de ORM)

### Quais são as alternativas?
- Dependência direta em classes concretas (alta acoplamento, difícil de mudar ou testar)
- Dependência em abstrações que ainda vazam detalhes (interfaces que retornam tipos específicos de framework)
- Nenhuma abstração (tudo acoplado diretamente)
- Dependência apenas em uma direção sem inversão verdadeira

### Quais são os trade-offs?
**Vantagens da Inversão de Dependência:**
- Altamente testável: círculos internos podem ser testados com mocks ou implementações simples
- Altamente substituível: implementações concretas podem ser trocadas sem afetar círculos internos
- Independência tecnológica: regras de negócio não sabem nada sobre banco de dados, framework ou UI usados
- Clareza sobre o que o sistema realmente precisa (as interfaces definidas internamente)
- Facilidade de suportar múltiplas implementações simultaneamente (ex: múltiplos bancos de dados para diferentes ambientes)
- Menos risco de efeito colateral ao mudar detalhes técnicos

**Desvantagens/custos:**
- Sobrehead de definição de interfaces
- Pode parecer indireto para desenvolvedores acostumados com acesso direto
- Requer disciplina para manter asInterfaces puras (nenhum vazamento de detalhes)
- Necessidade de entender bem onde cada pertencimento vai
- Em casos muito simples, pode parecer excesso de formalismo

### Quando usar?
- Sempre que houver dependência de algo que pode mudar (tecnologia, implementação, ambiente)
- Quando se quer garantir testabilidade de módulos de alto nível
- Quando se antecipa necessidade de mudar implementação sem afetar uso
- Quando se quer reutilizar módulos de alto nível em diferentes contextos
- Quando se quer evitar o problema de "quebrar mudanças" ao atualizar dependências
- Quando se quer apoiar princípios como teste驱动开发 (TDD) onde se testa primeiro com implementações falsas

### Quando não usar?
- Quando a dependência é em algo que nunca mudará e é extremamente simples
- Quando se está em um contexto onde desempenho crítico exige acesso direto e nenhuma indireção é tolerável
- Quando se está prototipando e velocidade é a única prioridade
- Quando se está construindo um sistema tão simples que o overhead não traz benefício
- Quando se está em um ambiente altamente restrito onde cada byte conta e indireção é proibida

### Quais são os erros mais comuns?
- Interfaces que retornam tipos específicos de framework (ex: List<Entity> onde Entity é uma classe JPA anotada)
- Classes internas que ainda fazem importações de pacotes externos apesar de usar interfaces
- Injeção de dependência feita incorretamente (criando instâncias diretamente em vez de injetar)
- Acreditar que usar uma interface automaticamente inverte dependência (quando a implementação ainda é conhecida ou especificada em detalhes internos)
- Interfaces que são tão específicas que funcionam apenas com uma implementação concreta
- Esquecer de injetar dependências e ter dependências implícitas ou globais

### Como isso afeta:
- *performance:* Impacto mínimogeralmente <1% devido a chamada de interface indireta vs direta
- *escalabilidade:* Nenhum impacto direto
- *disponibilidade:* Nenhum impacto direto
- *consistência:* Similar; depende de implementação das abstrações
- *segurança:* Similar; abstrações podem incluir preocupações de segurança como validação de entrada
- *custo:* Similar; foco em onde a concretude reside ao invés de adicionar ou remover código
- *observabilidade:* Melhora pois pontos claros de dependência facilitam mocking e testing
- *complexidade operacional:* Similar; pode reduzir bugs devido a melhor isolamento e testabilidade

### Exemplos reais de aplicação
- Repositório que retorna List<Task> onde Task é uma entidade pura, não uma classe JPA ou Hibernate específica
- Gateway de pagamento que implementa interface PaymentProcessor definida nos casos de uso, podendo ser implementado como StripeProcessor, PayPalProcessor ou MockProcessor para testes
- Servidor de email que implementa interface EmailSender definida nos casos de uso, podendo ser implementado como SMTPLaser, SendGridProvider ou MockSender
- Serviço de autenticação que implementa interface AuthProvider definida nos casos de uso, podendo ser implementado como LDAPProvider, OAuth2Provider ou MockProvider
- Cache que implementa interface CacheProvider definida nos casos de uso, podendo ser implementado como RedisCache, CaffeineCache ou NullCache

### Exemplo simplificado
Violando Inversão de Dependência:
```java
// ❌ ERRADO: Caso de uso depende diretamente de classe concreta de framework
public class AddTaskUseCase {
    private final TaskRepositoryImpl taskRepository; // Dependência em concretização
    
    public AddTaskUseCase(TaskRepositoryImpl taskRepository) {
        this.taskRepository = taskRepository;
    }
    
    public void execute(String title) {
        Task task = new Task(null, title, false);
        taskRepository.save(task); // Presume implementação específica
    }
}
```

Respeitando Inversão de Dependência:
```java
// ✅ CORRETO: Caso de uso depende apenas de interface
public interface TaskRepository {
    Task save(Task task);
    Optional<Task> findById(Long id);
    void deleteById(Long id);
}

public class AddTaskUseCase {
    private final TaskRepository taskRepository; // Dependência apenas em abstração
    
    public AddTaskUseCase(TaskRepository taskRepository) {
        this.taskRepository = taskRepository;
    }
    
    public void execute(String title) {
        Task task = new Task(null, title, false);
        taskRepository.save(task); // Funciona com qualquer implementação da interface
    }
}

// Implementação concreta na camada de frameworks
@Repository
public class TaskRepositoryImpl implements TaskRepository {
    private final EntityManager entityManager;
    
    public TaskRepositoryImpl(EntityManager entityManager) {
        this.entityManager = entityManager;
    }
    
    @Override
    public Task save(Task task) {
        // Implementação específica aqui - OK porque está na camada externa
        if (task.getId() == null) {
            entityManager.persist(task);
            return task;
        } else {
            return entityManager.merge(task);
        }
    }
}
```

### Exemplo de sistema de produção
Sistema de reservas de viagens com múltiplas implementações:
- **Interface de Pagamento PaymentProcessor** (definida nos casos de uso):
  ```java
  public interface PaymentProcessor {
      PaymentResult processPayment(PaymentRequest request);
      PaymentResult refundPayment(String paymentId, BigDecimal amount);
  }
  ```
- **Implementações Concretas** (na camada de frameworks):
  - StripePaymentProcessor: integra com API do Stripe
  - PayPalPaymentProcessor: integra com API do PayPal
  - BogusPaymentProcessor: implementação simples para testes e desenvolvimento
  - OfflinePaymentProcessor: para pagamentos via boleto ou transferência bancária
- **Casos de Uso** (processar pagamento, reembolsar):
  - Dependem apenas da interface PaymentProcessor
  - Nunca sabem se estão talking to Stripe, PayPal ou outro método
  - Testados com BogusPaymentProcessor em memória
- **Configuração:**
  - Em produção: usa StripePaymentProcessor
  - Em staging: pode usar PayPalPaymentProcessor para testes de integração
  - Em desenvolvimento: usa BogusPaymentProcessor
  - Para pagamentos offline: pode ser configurado por tipo de pagamento ou região

### Como esse assunto pode aparecer em uma entrevista.
> 🎯 **ENTREVISTA — ALTA FREQUÊNCIA**
> 
> "Explique como você aplicaria o princípio da Inversão de Dependência a um sistema que precisa enviar notificações por email, SMS e push notification."
> 
> **Armadilha:** Sugerir que basta criar uma classe NotificationService com métodos para cada tipo, ou fazer casos de uso dependerem de classes concretas de cada provedor.
> 
> **Como raciocinar:** Definir interface NotificationSender nos casos de uso com método send(NotificationRequest request). Criar implementações concretas EmailSenderImpl, SmsSenderImpl, PushNotificationSenderImpl na camada de frameworks, cada uma lidando com seu provedor específico (SES, Twilio, Firebase Cloud Messaging). Casos de uso dependem apenas da interface NotificationSender e são testados com mocks ou implementações em memória. Mostrar como adicionar um novo provedor (ex: WhatsApp Business) apenas requer nova implementação da interface sem tocar nos casos de uso.

## Comparação com outras arquiteturas

> 🎯 **ENTREVISTA — ALTA FREQUÊNCIA**
> 
> Comparar arquiteturas é comum em entrevistas para testar entendimento de trade-offs e contexto.

### Clean Architecture vs Arquitetura em Camadas (Layered)

| Aspecto | Clean Architecture | Arquitetura em Camadas Tradicional |
|---------|-------------------|-----------------------------------|
| **Direção de Dependência** | Para dentro apenas (Dependency Rule estrita) | Geralmente para baixo, mas pode ter exceções |
| **Independência de Frameworks** | Alta: regras de negócio não conhecem detalhes de framework | Baixa a média: entidades frequentemente têm anotações de framework |
| **Testabilidade** | Alta: casos de uso e entidades testáveis sem framework | Média a baixa: frequentemente requer inicialização de framework |
| **Independência de UI** | Alta: pode mudar de web para mobile sem mudar regras de negócio | Baixa a média: lógica de negócio frequentemente misturada com presentación |
| **Independência de Banco de Dados** | Alta: pode trocar de SQL para NoSQL sem mudar regras de negócio | Baixa a média: entidades frequentemente dependem de detalhes de ORM |
| **Complexidade Inicial** | Mais elevada devido a interfaces e camadas adicionais | Menor devido a estrutura mais direta |
| **Sobrehead de Indireção** | Presente devido a chamadas de interface | Menor devido a acesso mais direto em muitos casos |
| **Clareza de Regras de Negócio** | Muito alta: núcleos limpos e bem definidos | Média a baixa: regras frequentemente espalhadas ou misturadas |
| **Facilidade de Mudança Tecnológica** | Alta: núcleo permanece mesmo mudando stack completa | Baixa a média: frequentemente requer mudanças significativas no código |
| **Quando Usar** | Sistemas de longa vida, regras de negócio valiosas, necessidade de independência tecnológica | Sistemas simples, protótipos, onde velocidade inicial é crítica |

### Clean Architecture vs Hexagonal Architecture / Ports and Adapters

| Aspecto | Clean Architecture | Hexagonal Architecture |
|---------|-------------------|------------------------|
| **Visão Central** | Círculos concêntricos com entidades no centro | Hexágono com porta de domínio no centro |
| **Terminologia** | Entities, Use Cases, Interface Adapters, Frameworks & Drivers | Ports (entrada/saída), Adapters, Domain |
| **Foco** | Separação por nível de abstração (negócio vs mecanismo) | Separação por preocupação de entrada/saída |
| **Dependência Rule** | Dependências apenas para dentro | Portas definem dependências; adaptadores implementam portas |
| **Testabilidade** | Muito alta: núcleo testável sem adapters | Alta: domínio testável sem adapters |
| **Flexibilidade de Entrada/Saída** | Alta: múltiplos tipos de adaptadores suportados | Muito alta: projetada especificamente para múltiplas entradas/saídas |
| **Quando Usar** | Quando se foca em independência de regras de negócio | Quando se foca em flexibilidade de entrada/saída (múltiplas UIs, múltiplos bancos de dados, etc.) |
| **Sobreposição** | Muito overlap; podem ser vistas como complementares | Muito overlap; hexagona é frequentemente uma visão da clean architecture |
| **Exemplo de Uso** | Sistema onde regras de negócio são o ativo mais valioso | Sistema onde se quer suportar web, mobile, API de terceiros, linha de comando com mesma lógica de negócio |

### Clean Architecture vs Onion Architecture

| Aspecto | Clean Architecture | Onion Architecture |
|---------|-------------------|-------------------|
| **Origem** | Robert C. Martin (Uncle Bob) | Jeffrey Palermo |
| **Visão** | Círculos concêntricos | Camadas semelhantes a cebola |
| **Terminologia** | Similar (Entities, Use Cases, etc.) | Similar com algumas variações |
| **Foco Principal** | Independência de frameworks e detalhes técnicos | Separação de preocupações com foco em domínio no centro |
| **Dependency Rule** | Mesma princípio: dependências apenas para dentro | Mesma princípio |
| **Complexidade** | Similar | Similar |
| **Quando Usar** | Quando se quer enfatizar independência tecnológica | Quando se quer enfatizar isolamento de domínio |
| **Relação** | Frequentemente consideradas variações da mesma ideia | Frequentemente consideradas variações da mesma ideia |
| **Exemplo de Uso** | Ambas aplicáveis aos mesmos cenários; escolha frequentemente baseada em terminologia preferida da equipe |

## Quando usar Clean Architecture

> 💡 **DICA DE ENTREVISTA**
> 
> Sempre relacione a escolha ao contexto específico - não trate como regra universal.

Use Clean Architecture quando:
- O sistema é esperado para ter longa vida útil (anos ou décadas)
- As regras de negócio são complexas e representam o ativo mais valioso do sistema
- Se antecipa necessidade de mudar tecnologias (framework, banco de dados, UI) durante a vida do sistema
- Múltiplas interfaces externas (web, mobile, API, linha de comando) precisam acessar a mesma lógica de negócio
- Se quer garantir que o núcleo do sistema seja altamente testável sem depender de frameworks completos
- Se quer evitar o problema de "framework lock-in" onde mudar tecnologia exige reescrita massiva
- A equipe valoriza limpeza, manutenibilidade e testabilidade a longo prazo sobre velocidade inicial de desenvolvimento
- Se está construindo um sistema onde falhas de regras de negócio seriam particularmente custosas
- Se quer maximizar a reutilização de regras de negócio em diferentes contextos ou aplicações

Não use Clean Architecture quando:
- Está construindo um protótipo descartável ou prova de conceito onde velocidade é a única prioridade
- O sistema é tão simples que o overhead de interfaces e camadas não traz benefício proporcional
- A equipe rejeita fortemente a ideia de camadas adicionais e indireção
- Está em um ambiente altamente restrito onde cada classe ou byte conta (sistemas embarcados ultra-restritos)
- Se está prototipando e velocidade é a prioridade absoluta
- Se vai descartar o sistema após uso único ou muito limitado
- O domínio é tão simples que não há regras de negócio significativas para isolar
- Se está em um contexto onde desempenho crítico exige acesso direto e nenhuma indireção é tolerável

## Exercícios

### Exercício básico
Explique a Dependency Rule da Clean Architecture usando um exemplo de sistema de lista de tarefas.

### Exercício intermediário
Dado um cenário de sistema bancário com funcionalidades de conta corrente, poupança e investimento, analise:
- Como as entidades seriam modeladas (focando em regras de negócio intrínseco)
- Como os casos de uso seriam estruturados (fluxos específicos da aplicação)
- Como os adaptadores de interface lidariam com web, mobile e linha de comando
- Como os frameworks e drivers seriam mantidos puros
- Como a Inversão de Dependência seria aplicada para repositórios e serviços externos
- Como você testaria o núcleo do sistema sem nenhum framework

### Exercício avançado
Analise um sistema que você conhece que usa ou poderia se beneficiar da Clean Architecture:
1. Documente como as responsabilidades seriam distribuídas entre as quatro camadas
2. Mostre como as regras de negócio ficam isoladas dos detalhes técnicos
3. Avalie se a arquitetura segue corretamente os princípios de independência e testabilidade
4. Identifique oportunidades de melhoria na aplicação dos princípios
5. Descreva como você migraria um sistema existente para essa arquitetura com risco mínimo

### Exercício de entrevista
> 🎯 **ENTREVISTA — ALTA FREQUÊNCIA**
> 
> "Você herda um sistema onde as regras de negócio estão espalhadas entre controllers, entidades e serviços. Descreva sua abordagem para refatorá-lo para Clean Architecture sem riscos desnecessários."
> 
> Forneça a resposta esperada e explique o que torna ela eficaz.

### Desafio
Crie uma matriz de decisão que ajude a determinar quando usar Clean Architecture, quando evoluir de uma arquitetura em camadas tradicional, e quando considerar alternativas como Hexagonal ou Onion Architecture. Inclua fatores como: vida útil esperada do sistema, valor das regras de negócio, necessidade de independência tecnológica, múltiplas interfaces externas, maturidade da equipe, e requisitos de testabilidade.