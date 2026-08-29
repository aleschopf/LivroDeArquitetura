---
trilha: "INICIANTE"
---
**Navegação:** [[MOC — TRILHA INICIANTE]]
← [[PARTE 10 — COESÃO E ACOPLAMENTO]] | #trilha/iniciante | [[PARTE 3 — ARQUITETURA MONOLÍTICA]] →

---
# PARTE 11 — DESIGN PATTERNS

> 🧠 **ESSENCIAL**
> 
> Design Patterns são soluções reutilizáveis para problemas comuns que ocorrem no projeto de software. Eles representam as melhores práticas evoluídas ao longo do tempo por desenvolvedores experientes de software orientado a objeto.

## O que são Design Patterns?
Design Patterns são descrições de como resolver problemas recorrentes de projeto de software. Eles não são código pronto para copiar e colar, mas sim modelos ou templates que descrevem como resolver um problema em diferentes situações.

### Por que existem?
Como resposta à necessidade de reutilizar soluções comprovadas em vez de reinventar a roda. Antes dos design patterns, desenvolvedores resolviam os mesmos problemas repetidamente sem um vocabulário comum ou abordagem padronizada.

### Qual problema resolvem?
- Falta de reutilização de soluções de projeto
- Código mal estruturado e difícil de manter
- Dificuldade de comunicação entre desenvolvedores sobre soluções de projeto
- Redescobrimento de soluções já conhecidas
- Código frágil que quebra facilmente quando modificado

### Como funcionam internamente?
Cada padrão descreve:
- **Contexto:** Quando o padrão se aplica
- **Problema:** O desafio de projeto que o padrão aborda
- **Solução:** Os elementos do projeto, suas responsabilidades, relacionamentos e colaborações
- **Consequências:** Resultados e trade-offs de aplicar o padrão

### Como implementar?
1. **Identificar o problema** que você está tentando resolver
2. **Selecionar o padrão** apropriado baseado no contexto e nas forças envolvidas
3. **Entender a estrutura** do padrão (participantes, colaborações)
4. **Adaptar o padrão** ao seu contexto específico (linguagem, restrições, etc.)
5. **Implementar** seguindo as diretrizes do padrão
6. **Documentar** onde e por que você aplicou cada padrão

### Quais são as alternativas?
- Reinventar soluções para problemas comuns
- Usar soluções ad-hoc sem base em práticas estabelecidas
- Depender apenas de experiência individual sem padrões compartilhados
- Aplicar padrões incorretamente ou em contextos inadequados

### Quais são os trade-offs?
**Vantagens dos Design Patterns bem aplicados:**
- Reutilização de soluções comprovadas
- Melhoria na comunicação entre desenvolvedores (vocabulário comum)
- Código mais fácil de entender, manter e estender
- Redução de complexidade acumulada ao evitar soluções inferiores
- Facilidade de aprendizado para novos membros da equipe

**Desvantagens/custos:**
- Risco de overengineering se aplicados onde não são necessários
- Pode aumentar inicialmente a complexidade devido a classes adicionais
- Requer aprendizado e experiência para aplicar corretamente
- Pode ser aplicado de forma mecânica sem entender o problema subjacente
- Alguns padrões podem não se traduzir bem para linguagens não orientadas a objeto

### Quando usar?
- Quando você identifica um problema recorrente de projeto
- Quando quer comunicar soluções de projeto de forma eficaz
- Quando está refatorando código e vê oportunidades para aplicar padrões estabelecidos
- Quando está projetando novos sistemas e quer evitar armadilhas comuns
- Quando multiple desenvolvedores precisam trabalhar no mesmo código

### Quando não usar?
- Quando o problema é simples e não justifica a sobrecarga de um padrão
- Quando se está prototipando e velocidade é a única prioridade
- Quando o padrão não se encaixa no contexto específico (forçar um padrão)
- Quando se está aprendendo programação orientada a objeto básica
- Quando a equipe rejeita fortemente a ideia de padrões de projeto

### Quais são os erros mais comuns?
- Aplicar padrões onde não são necessários (overengineering)
- Escolher o padrão errado para o problema
- Implementar o padrão incorretamente violando sua intenção
- Usar padrões como substituto para pensamento crítico
- Aplicar padrões sem entender as forças e contexto que os tornaram eficazes
- Esquecer que padrões são sobre boas práticas, não regras rígidas
- Aplicar muitos padrões em um único projeto levando a complexidade desnecessária

### Como isso afeta:
- *performance:* Impacto varia por padrão (algumas indireções mínimas, outros podem otimizar)
- *escalabilidade:* Similar; padrões não impõem limitações de escalabilidade
- *disponibilidade:* Nenhum impacto direto
- *consistência:* Melhora pois padrões promovem soluções comprovadas e previsíveis
- *segurança:* Similar; padrões não afetam diretamente preocupações de segurança
- *custo:* Custo inicial pode ser maior devido ao aprendizado e implementação, mas custo de manutenção a longo prazo tende a ser menor
- *observabilidade:* Similar; pode ser instrumentado normalmente
- *complexidade operacional:* Pode reduzir devido a melhor estrutura e menos efeitos colaterais inesperados

### Exemplos reais de aplicação
- Padrão **Factory Method** em frameworks de teste para criar objetos de teste
- Padrão **Observer** em sistemas de GUI para lidar com eventos de usuário
- Padrão **Strategy** em algoritmos de compressão onde diferentes algoritmos podem ser intercambiados
- Padrão **Singleton** em gerenciadores de log ou configuração de aplicação
- Padrão **Decorator** em fluxos de I/O do Java para adicionar funcionalidades como bufferização

### Exemplo simplificado
Sem padrão (procedural):
```java
// ❌ ERRADO: Código difícil de estender e manter
public class NotificationService {
    public void sendNotification(String type, String message) {
        if ("email".equals(type)) {
            // código complexo para enviar email
        } else if ("sms".equals(type)) {
            // código complexo para enviar SMS
        } else if ("push".equals(type)) {
            // código complexo para enviar push notification
        }
        // Adicionar novo tipo exige modificar este método
    }
}
```

Com padrão Strategy:
```java
// ✅ CORRETO: Fácil de estender com novos tipos de notificação
public interface NotificationStrategy {
    void send(String message);
}

public class EmailNotification implements NotificationStrategy {
    public void send(String message) {
        // envio de email
    }
}

public class SMSNotification implements NotificationStrategy {
    public void send(String message) {
        // envio de SMS
    }
}

public class NotificationService {
    private NotificationStrategy strategy;
    
    public void setStrategy(NotificationStrategy strategy) {
        this.strategy = strategy;
    }
    
    public void sendNotification(String message) {
        strategy.send(message);  // Delegamos para a estratégia
    }
}

// Uso:
// NotificationService service = new NotificationService();
// service.setStrategy(new EmailNotification());
// service.sendNotification("Hello");
// 
// Para adicionar novo tipo:
// 1. Criar nova classe que implementa NotificationStrategy
// 2. Definir a estratégia no serviço
// 3. Nenhum código existente precisa ser modificado
```

### Exemplo de sistema de produção
Framework Spring:
- **Factory Method:** BeanFactory para criação de beans
- **Observer:** ApplicationEvent e ApplicationListener para eventos de aplicação
- **Strategy:** Diferentes implementations de HandlerMapping e HandlerAdapter
- **Decorator:** RequestContextHolder para decorar objetos de requisição
- **Proxy:** AOP (Aspect-Oriented Programming) usando proxies dinamicos
- **Template Method:** JdbcTemplate para operações de banco de dados com tratamento padrão de exceções
- **Facade:** WebMvcConfigurer para simplificar configuração do Spring MVC

### Como esse assunto pode aparecer em uma entrevista.
> 🎯 **ENTREVISTA — ALTA FREQUÊNCIA**
> 
> "Explique quando você usaria o padrão Strategy em vez de herança para variar um comportamento."
> 
> **Armadilha:** Sugerir que Strategy é sempre melhor que herança sem considerar quando herança é mais apropriada.
> 
> **Como raciocinar:** Descrever que Strategy é preferível quando o comportamento varia independentemente do tipo de objeto (pode ser alterado em tempo de execução) ou quando há múltiplas variações ortogonais. Herança é melhor quando há uma relação "é um" estável e o comportamento varia hierarquicamente. Mostrar exemplos como algoritmos de ordenação (Strategy) vs tipos de funcionários com aumento salarial diferente (pode ser herança se hierárquico, ou Strategy se independente).

## Creational Patterns

> 🎯 **ENTREVISTA — ALTA FREQUÊNCIA**
> 
> Padrões criacionais são frequentemente perguntados em entrevistas porque lidam com criação de objetos, um aspecto fundamental de OOP.

### definição
Padrões Criacionais abstraem o processo de instantição de objetos. Eles ajudam a tornar um sistema independente de como seus objetos são criados, compostos e representados.

### Por que existem?
Para separar a responsabilidade de criação de objetos do resto do sistema, tornando o código mais flexível e reutilizado. A criação direta com `new` espalhada pelo código dificulta mudanças e testes.

### Como funciona internamente?
- Encapsulam conhecimento sobre quais classes o sistema usa
- Escondem como as instâncias dessas classes são criadas e composta
- Permitem que o sistema seja independente de como seus objetos são criados
- Podem retornar instâncias de subclasses através de polimorfismo
- Gerenciam o ciclo de vida dos objetos quando necessário

### Principais Padrões Criacionais
1. **Factory Method**
2. **Abstract Factory**
3. **Builder**
4. **Prototype**
5. **Singleton**

### Como implementar?
Dependendo do padrão específico, mas geralmente envolve:
- Definir interfaces ou classes abstratas para criação
- Delegar a responsabilidade de criação para subclasses ou objetos separados
- Usar composição em vez de herança quando apropriado
- Gerenciar estado e ciclo de vida dos objetos criados

### Quais são as alternativas?
- Uso direto de construtores com `new` espalhado pelo código
- Métodos estáticos de fábrica em cada classe
- Construção complexa de objetos em construtores ou métodos de inicialização
- Deixar a criação de objetos como responsabilidade do cliente sem abstração

### Quais são os trade-offs?
**Vantagens dos padrões criacionais bem aplicados:**
- Flexibilidade na criação de objetos (o que criar, quem cria, como criar)
- Encapsulamento de conhecimento específico de famílias de produtos
- Facilidade de troca de famílias de produtos
- Consistência entre produtos criados
- Facilidade de teste através de mocks ou stubs de factories

**Desvantagens/custos:**
- Pode aumentar a complexidade devido a classes adicionais
- Pode parecer indireto para desenvolvedores iniciantes
- Requer compreensão clara de quando cada padrão se aplica
- Pode levar a hierarquias de factories complexas se não bem projetado

### Quando usar?
- Quando um sistema deve ser independente de como seus produtos são criados
- Quando um sistema deve ser configurável com múltiplas famílias de produtos
- Quando a família de produtos deve ser revelada apenas através de suas interfaces
- Quando se quer isolar clientes de classes de implementação concretas
- Quando a construção de objetos é complexa ou envolve múltiplos passos

### Quando não usar?
- Quando a criação de objetos é simples e direta
- Quando se está prototipando e velocidade é a única prioridade
- Quando o overhead de abstração não traz benefício proporcional
- Quando se está em um ambiente altamente restrito onde cada classe conta
- Quando se sabe com certeza que apenas um tipo de objeto será necessário

### Quais são os erros mais comuns?
- Aplicar Abstract Factory quando só se precisa de Factory Method
- Fazer factories conhecerem demais sobre os produtos que criam (vazamento de abstração)
- Não limitar adequadamente o escopo de responsabilidade da factory
- Esquecer que alguns padrões criacionais (como Singleton) têm ciclo de vida especial
- Aplicar padrões criacionais a objetos imutáveis onde não são necessários
- Criar fábricas que violam o Princípio da Responsabilidade Única

### Como isso afeta:
- *performance:* Impacto mínimo devido a indireções de método (geralmente insignificante)
- *escalabilidade:* Nenhum impacto direto
- *disponibilidade:* Nenhum impacto direto
- *consistência:* Melhora pois garante criação consistente de objetos relacionados
- *segurança:* Nenhum impacto direto
- *custo:* Similar; foco em onde a criação reside ao invés de adicionar ou remover código
- *observabilidade:* Melhora pois pontos de criação são mais explícitos e fáceis de monitorar
- *complexidade operacional:* Melhora devido a menor acoplamento na criação de objetos

### Exemplos reais de aplicação
- **Factory Method:** Frameworks de teste como JUnit usam factory methods para criar objetos de teste
- **Abstract Factory:** Kits de interface gráfica (como Swing) que permitem trocar aparência (Metal, Motif, etc.)
- **Builder:** Construção de objetos complexos como conexões de banco de dados ou mensagens de email
- **Prototype:** Sistemas de gráficos onde objetos complexos são clonados em vez de reconstituídos
- **Singleton:** Gerenciadores de log, configuração de aplicação, pools de conexão

### Exemplo simplificado
Factory Method (problema):
```java
// ❌ ERRADO: Cliente conhece demais sobre classes concretas
public class Application {
    public Document createDocument(String type) {
        if ("pdf".equals(type)) {
            return new PdfDocument();
        } else if ("word".equals(type)) {
            return new WordDocument();
        } else if ("excel".equals(type)) {
            return new ExcelDocument();
        }
        throw new IllegalArgumentException("Tipo desconhecido: " + type);
    }
}
```

Factory Method (solução):
```java
// ✅ CORRETO: Cliente depende de abstração, subclasses decidem qual classe instanciar
public abstract class Application {
    public Document createDocument() {
        return createDocumentInstance();  // Delega para subclasses
    }
    
    protected abstract Document createDocumentInstance();
}

public class PdfApplication extends Application {
    protected Document createDocumentInstance() {
        return new PdfDocument();
    }
}

public class WordApplication extends Application {
    protected Document createDocumentInstance() {
        return new WordDocument();
    }
}

// Uso:
// Application app = new PdfApplication();
// Document doc = app.createDocument();  // Retorna PdfDocument
//
// Para trocar tipo de documento:
// Application app = new WordApplication();
// Document doc = app.createDocument();  // Retorna WordDocument
```

### Exemplo de sistema de produção
Framework de desenho vetorial:
- **Abstract Factory:** Famílias de formas geométricas (2D vs 3D) e estilos de renderização (vetorial vs raster)
  - `ShapeFactory` abstrata com métodos `createCircle()`, `createRectangle()`, etc.
  - Implementações: `Vector2DShapeFactory`, `Raster2DShapeFactory`, `Vector3DShapeFactory`
  - Cliente desenha usando apenas a abstração `ShapeFactory`
- **Builder:** Construção complexa de gradientes e padrões
  - `GradientBuilder` para criar gradientes lineares, radiais, etc.
  - `PatternBuilder` para criar padrões de textura complexos
- **Prototype:** Ferramentas de clonagem de formas no editor
  - Usuário seleciona uma forma e arrasta com Ctrl para clonar
  - Implementado através de método `clone()` nas classes de forma
- **Singleton:** Gerenciador central de cores e estilos
  - `StyleManager` garante que só haja uma instância de gerenciamento de estilos
  - Acesso global consistente às definições de estilo

### Como esse assunto pode aparecer em uma entrevista.
> 🎯 **ENTREVISTA — ALTA FREQUÊNCIA**
> 
> "Explique a diferença entre Factory Method e Abstract Factory e quando você escolheria cada um."
> 
> **Armadilha:** Sugerir que Abstract Factory é simplesmente uma fábrica de fábricas sem explicar o contexto de famílias de produtos relacionados.
> 
> **Como raciocinar:** Descrever que Factory Method lida com a criação de um único produto através de herança (subclasses decidem qual classe instanciar), enquanto Abstract Factory lida com famílias de produtos relacionados através de composição (interface com múltiplos métodos de criação). Escolher Factory Method quando houver variações em um único tipo de produto. Escolher Abstract Factory quando houver múltiplas famílias de produtos onde os produtos de uma família são projetados para trabalhar juntos. Mostrar exemplo: Factory Method para diferentes tipos de documentos em um editor, Abstract Factory para kits de GUI onde botões, menus e texto precisam ser consistentes dentro de um estilo (Windows, Mac, etc.).

## Structural Patterns

> 🎯 **ENTREVISTA — MÉDIA FREQUÊNCIA**
> 
> Padrões estruturais concernem a composição de classes e objetos para formar estruturas maiores; entrevistadores querem ver se você entende como construir estruturas flexíveis.

### definição
Padrões Estruturais concernem a composição de classes e objetos para formar estruturas maiores. Eles ajudam a garantir que quando uma parte de um sistema muda, todo o sistema não precisa mudar igualmente.

### Por que existem?
Para fornecer maneiras flexíveis de compor objetos e classes em estruturas maiores enquanto mantém encapsulamento e baixo acoplamento. Sem esses padrões, estruturas complexas tendem a se tornar rígidas e difíceis de modificar.

### Como funciona internamente?
- Usam composição em vez de herança quando apropriado
- Fornecem interfaces simplificadas para funcionalidades complexas
- Permitem que objetos sejam tratados como se fossem outros tipos (adaptação)
- Gerenciam o ciclo de vida e responsabilidades de objetos compostos
- Oferecem maneiras de adicionar responsabilidades dinamicamente

### Principais Padrões Estruturais
1. **Adapter**
2. **Bridge**
3. **Composite**
4. **Decorator**
5. **Facade**
6. **Flyweight**
7. **Proxy**

### Como implementar?
Dependendo do padrão específico, mas geralmente envolve:
- Definir interfaces que permitem diferentes formas de composição
- Usar delegation para forwarding de responsabilidades
- Manter referências a componentes ou objetos envolvidos
- Gerenciar estado quando necessário para o padrão escolhido

### Quais são as alternativas?
- Herança múltipla ou em cascata para alcançar combinações de funcionalidades
- Código duplicado ou copiado para alcançar comportamentos similares
- Interfaces monolíticas que forzam implementações de funcionalidades não utilizadas
- Dependência direta em implementações concretas sem abstração
- Deixar a composição como responsabilidade do cliente sem orientação

### Quais são os trade-offs?
**Vantagens dos padrões estruturais bem aplicados:**
- Flexibilidade na composição de objetos e classes
- Reutilização através de composição em vez de herança
- Encapsulamento de complexidade atrás de interfaces simples
- Facilidade de evoluir sistemas sem quebrar código existente
- Melhoria na legibilidade através de nomes intencionais para combinações

**Desvantagens/custos:**
- Pode aumentar a complexidade devido a objetos intermediários adicionais
- Pode parecer indireto para desenvolvedores acostumados com herança direta
- Requer compreensão clara de trade-offs entre composição e herança
- Pode levar a muitos objetos pequenos se levado ao extremo
- Alguns padrões (como Proxy) podem introduzir latência adicional

### Quando usar?
- Quando se precisa adaptar interfaces incompatíveis
- Quando se quer separar abstração de implementação para que possam variar independentemente
- Quando se precisa tratar objetos individuais e composições de objetos uniformemente
- Quando se quer adicionar responsabilidades a objetos dinamicamente sem afetar outros
- Quando se precisa fornecer uma interface unificada para um conjunto de interfaces
- Quando se quer compartilhar objetos eficientemente para suportar grandes quantidades de objetos finamente granulares
- Quando se precisa controlar acesso a um objeto

### Quando não usar?
- Quando a relação entre entidades é naturalmente hierárquica e herança é suficiente
- Quando se está prototipando e velocidade é a única prioridade
- Quando o overhead de abstração não traz benefício proporcional
- Quando se está em um ambiente altamente restrito onde cada objeto conta
- Quando a solução direta é mais simples e compreensível

### Quais são os erros mais comuns?
- Usar Adapter quando um simples refatoramento resolveria o problema
- Confundir Bridge com Strategy (ambos lidam com variabilidade, mas em contextos diferentes)
- Fazer Composite tratar folhas e composições de forma inconsistente
- Usar Decorator quando a responsabilidade adicional é fixa e conhecida em tempo de compilação
- Fazer Facade expôr demasiados detalhes do subsistema que deveria simplificar
- Aplicar Flyweight a objetos onde o estado intrínseco não é realmente compartilhável
- Usar Proxy quando acesso direto seria suficiente e mais eficiente

### Como isso afeta:
- *performance:* Impacto varia (algumas indireções mínimas, Proxy pode adicionar latência)
- *escalabilidade:* Similar; padrões não impõem limitações de escalabilidade
- *disponibilidade:* Nenhum impacto direto
- *consistência:* Melhora pois padrões promovem composições previsíveis e estáveis
- *segurança:* Nenhum impacto direto
- *custo:* Similar; foco em onde a composição reside ao invés de adicionar ou remover código
- *observabilidade:* Melhora pois pontos de composição são mais explícitos e fáceis de monitorar
- *complexidade operacional:* Pode reduzir devido a melhor encapsulamento e menos efeitos colaterais inesperados

### Exemplos reais de aplicação
- **Adapter:** Adaptadores de legado para permitir que novos sistemas trabalhem com APIs antigas
- **Bridge:** Drivers de dispositivo onde abstração (operacoes comuns) é separada de implementação (hardware específico)
- **Composite:** Sistemas de arquivos onde diretórios e arquivos são tratados uniformemente
- **Decorator:** Fluxos de I/O do Java (BufferedInputStream, DataInputStream, etc.)
- **Facade:** Frameworks de inicialização como Spring que escondem complexidade de configuração
- **Flyweight:** Sistemas de texto onde caracteres compartilham propriedades de fonte e estilo
- **Proxy:** Proteção de acesso a objetos remotos (RMI) ou carregamento preguiçoso de recursos caros

### Exemplo simplificado
Decorator (problema):
```java
// ❌ ERRADO: Herança leva a explosão de classes para combinações de funcionalidades
public abstract class Coffee {
    public abstract double cost();
    public abstract String getDescription();
}

public class SimpleCoffee extends Coffee {
    public double cost() { return 1.0; }
    public String getDescription() { return "Simple coffee"; }
}

public class MilkCoffee extends Coffee {  // Leite
    public double cost() { return 1.2; }
    public String getDescription() { return "Simple coffee with milk"; }
}

public class SugarCoffee extends Coffee {  // Açúcar
    public double cost() { return 1.1; }
    public String getDescription() { return "Simple coffee with sugar"; }
}

public class MilkSugarCoffee extends Coffee {  // Leite e açúcar
    public double cost() { return 1.3; }
    public String getDescription() { return "Simple coffee with milk and sugar"; }
}
// E assim por diante para todas as combinações...
```

Decorator (solução):
```java
// ✅ CORRETO: Adiciona responsabilidades dinamicamente sem explosão de classes
public abstract class Coffee {
    public abstract double cost();
    public abstract String getDescription();
}

public class SimpleCoffee extends Coffee {
    public double cost() { return 1.0; }
    public String getDescription() { return "Simple coffee"; }
}

public abstract class CoffeeDecorator extends Coffee {
    protected Coffee decoratedCoffee;
    
    public CoffeeDecorator(Coffee coffee) {
        this.decoratedCoffee = coffee;
    }
}

public class Milk extends CoffeeDecorator {
    public Milk(Coffee coffee) {
        super(coffee);
    }
    
    public double cost() {
        return 0.2 + decoratedCoffee.cost();
    }
    
    public String getDescription() {
        return decoratedCoffee.getDescription() + ", milk";
    }
}

public class Sugar extends CoffeeDecorator {
    public Sugar(Coffee coffee) {
        super(coffee);
    }
    
    public double cost() {
        return 0.1 + decoratedCoffee.cost();
    }
    
    public String getDescription() {
        return decoratedCoffee.getDescription() + ", sugar";
    }
}

// Uso:
// Coffee coffee = new SimpleCoffee();
// coffee = new Milk(coffee);
// coffee = new Sugar(coffee);
// System.out.println(coffee.getDescription() + " $" + coffee.cost());
// Saída: Simple coffee, milk, sugar $1.3
//
// Fácil de estender:
// coffee = new Whip(new Sugar(new Milk(new SimpleCoffee())));
```

### Exemplo de sistema de produção
Biblioteca de processamento de imagens:
- **Adapter:** Adaptadores para diferentes formatos de arquivo (JPEG, PNG, GIF) que implementam uma interface comum `ImageLoader`
- **Bridge:** Abstração `Image` separada de implementação `ImageRenderer` (Software vs Hardware acceleration)
- **Composite:** Árvore de elementos gráficos onde `GraphicObject` pode ser `PrimitiveShape` (linha, círculo) ou `Group` contendo outros `GraphicObject`
- **Decorator:** Filtros de imagem que podem ser combinados dinamicamente (Blur, Sharpen, ColorBalance, etc.)
- **Facade:** Classe `ImageProcessor` que fornece métodos simples como `load()`, `applyFilter()`, `save()` escondendo complexidade de carregamento, processamento e salvamento
- **Flyweight:** Cache de objetos de fonte onde propriedades como família, tamanho e estilo são compartilhadas entre múltiplos caracteres
- **Proxy:** `LazyImageProxy` que carrega a imagem real apenas quando necessária para exibição

### Como esse assunto pode aparecer em uma entrevista.
> 🎯 **ENTREVISTA — MÉDIA FREQUÊNCIA**
> 
> "Explique a diferença entre Decorator e Proxy e quando você escolheria cada um."
> 
> **Armadilha:** Sugerir que ambos são simplesmente "wrappers" sem explicar as intenções diferentes (adicionar responsabilidades vs controlar acesso).
> 
> **Como raciocinar:** Descrever que Decorator adiciona responsabilidades a um objeto dinamicamente mantendo a mesma interface, enquanto Proxy fornece um substituto ou representante para controlar acesso a outro objeto (pode ser por motivos de segurança, custo, ou outros). Escolher Decorator quando se quer estender funcionalidades de forma flexível. Escolher Proxy quando se precisa de controle de acesso, carregamento preguiçoso, ou representação local de objeto remoto. Mostrar exemplo: Decorator para adicionar bordas e sombras a componentes GUI, Proxy para carregamento preguiçoso de imagens grandes ou acesso protegido a objetos de negócio.

## Behavioral Patterns

> 🎯 **ENTREVISTA — ALTA FREQUÊNCIA**
> 
> Padrões comportamentais são cruciais para entender como objetos interagem e distribuem responsabilidade; entrevistadores querem ver se você entende comunicação e colaboração entre objetos.

### definição
Padrões Comportamentais são aqueles que se ocupam especificamente de algoritmos e da atribuição de responsabilidades entre objetos. Eles descrevem não apenas padrões de classes ou objetos, mas também padrões de comunicação entre eles.

### Por que existem?
Para fornecer maneiras mais flexíveis de realizar comunicação entre objetos enquanto evita acoplamento rígido. Sem esses padrões, comunicação entre objetos tende a se tornar rígida, difícil de modificar e propensa a efeitos colaterais inesperados.

### Como funciona internamente?
- Usam composição e delegation para melhorar flexibilidade
- Encapsulam comportamento que caso contrário estaria espalhado
- Permitem variação no algoritmo usado por um objeto
- Facilitam comunicação indireta entre objetos (mediador, observador)
- Distribuem responsabilidade entre objetos de forma clara
- Gerenciam estado e transições quando necessário

### Principais Padrões Comportamentais
1. **Chain of Responsibility**
2. **Command**
3. **Interpreter**
4. **Iterator**
5. **Mediator**
6. **Memento**
7. **Observer**
8. **State**
9. **Strategy**
10. **Template Method**
11. **Visitor**

### Como implementar?
Dependendo do padrão específico, mas geralmente envolve:
- Definir papéis claros para diferentes objetos na interação
- Usar interfaces para desacoplar remetentes e destinatários
- Gerenciar estado quando necessário para o padrão escolhido
- Considerar o ciclo de vida e responsabilidades dos objetos envolvidos
- Documentar claramente o protocolo de comunicação estabelecido

### Quais são as alternativas?
- Comunicação direta e explícita entre objetos levando a alto acoplamento
- Código espalhado com condicionais complexos para lidar com diferentes casos
- Herança inadequada para alcançar variações de comportamento
- Variáveis globais ou singletons para compartilhar estado
- Deixar a comunicação como responsabilidade do cliente sem orientação

### Quais são os trade-offs?
**Vantagens dos padrões comportamentais bem aplicados:**
- Flexibilidade na atribuição de responsabilidades entre objetos
- Reutilização através de encapsulamento de comportamento comum
- Redução de acoplamento entre objetos comunicantes
- Facilidade de alterar comportamento independente da estrutura
- Melhoria na legibilidade através de nomes intencionais para padrões de interação
- Facilidade de teste através de isolamento de unidades de comportamento

**Desvantagens/custos:**
- Pode aumentar a complexidade devido a objetos intermediários adicionais
- Pode parecer indireto para desenvolvedores acostumados com comunicação direta
- Requer compreensão clara de protocolos de comunicação estabelecidos
- Pode levar a muitos objetos pequenos se levado ao extremo
- Alguns padrões (como Mediator) podem criar ponto central de falha se não bem projetado

### Quando usar?
- Quando se precisa encapsular um pedido como objeto
- Quando se precisa definir famílias de algoritmos encapsulados
- Quando se quer notificar múltiplos objetos sobre mudanças sem acoplamento rígido
- Quando se precisa tornar comportamento de objeto independente de seu estado
- Quando se quer separar algoritmo da estrutura em que ele opera
- Quando se precisa fornecer uma maneira uniforme de percorrer estruturas de dados complexas
- Quando se precisa capturar e externalizar estado interno de um objeto
- Quando se quer reduzir dependências de envio para múltiplos destinatários
- Quando se precisa encapsular um pedido como objeto para permitir parametrização de clientes

### Quando não usar?
- Quando a comunicação entre objetos é simples e direta
- Quando se está prototipando e velocidade é a única prioridade
- Quando o overhead de abstração não traz benefício proporcional
- Quando se está em um ambiente altamente restrito onde cada objeto conta
- Quando a solução direta é mais simples e compreensível
- Quando se sabe com certeza que nenhum padrão comportamental será necessário

### Quais são os erros mais comuns?
- Usar Observer quando um simples callback seria suficiente
- Fazer Command conhecer demais sobre o receptor (vazamento de abstração)
- Não limitar adequadamente o escopo de responsabilidade no Chain of Responsibility
- Esquecer que alguns padrões (como Memento) têm implicações de encapsulamento
- Aplicar Template Method quando as variações são mínimas e não justificam herança
- Usar Visitor quando a estrutura de dados é estável e raramente visitada
- Fazer State conhecer demais sobre transições possíveis (vazamento de abstração)
- Confundir Strategy com State (ambos encapsulam comportamento, mas com intenções diferentes)

### Como isso afeta:
- *performance:* Impacto varia (algumas indireções mínimas, Chain of Responsibility pode adicionar latência)
- *escalabilidade:* Similar; padrões não impõem limitações de escalabilidade
- *disponibilidade:* Nenhum impacto direto
- *consistência:* Melhora pois padrões promovem comunicação previsível e estável
- *segurança:* Nenhum impacto direto
- *custo:* Similar; foco em onde a comunicação reside ao invés de adicionar ou remover código
- *observabilidade:* Melhora pois pontos de comunicação são mais explícitos e fáceis de monitorar
- *complexidade operacional:* Pode reduzir devido a melhor encapsulamento e menos efeitos colaterais inesperados

### Exemplos reais de aplicação
- **Chain of Responsibility:** Sistemas de tratamento de exceções onde exceções propagam através de uma cadeia de handlers
- **Command:** Frameworks de GUI onde ações de menu e botões são encapsuladas como objetos de comando
- **Interpreter:** Engines de expressão regular ou linguagens de consulta simples
- **Iterator:** Coleções Java onde `Iterator` fornece acesso uniforme a elementos
- **Mediator:** Sistemas de chat onde usuários se comunicam através de um mediador central ao invés de diretamente
- **Memento:** Recursos de desfazer/refazer em editores de texto ou aplicativos de design
- **Observer:** Sistemas de publicação/assinatura onde múltiplos inscritos recebem atualizações de um publicador
- **State:** Máquinas de estado onde comportamento muda baseado em estado interno (como conexões de rede)
- **Strategy:** Algoritmos de ordenação ou compressão onde diferentes estratégias podem ser intercambiadas
- **Template Method:** Frameworks de teste onde estrutura de teste é fixa mas passos específicos podem variar
- **Visitor:** Compiladores onde operações são realizadas sobre árvores de sintaxe abstrata

### Exemplo simplificado
Observer (problema):
```java
// ❌ ERRADO: Alta acoplamento entre sujeito e observadores
public class Stock {
    private String symbol;
    private double price;
    private List<Investor> investors = new ArrayList<>();
    
    public void addInvestor(Investor investor) {
        investors.add(investor);
    }
    
    public void removeInvestor(Investor investor) {
        investors.remove(investor);
    }
    
    public void setPrice(double price) {
        this.price = price;
        // Notifica todos os investidores diretamente
        for (Investor investor : investors) {
            investor.update(symbol, price);
        }
    }
}

public class Investor {
    private String name;
    
    public Investor(String name) {
        this.name = name;
    }
    
    public void update(String symbol, double price) {
        System.out.println(name + " recebeu atualização: " + symbol + " = $" + price);
    }
}
```

Observer (solução):
```java
// ✅ CORRETO: Sujeito depende apenas de abstração, não de implementações concretas
public interface StockObserver {
    void update(String symbol, double price);
}

public class Stock {
    private String symbol;
    private double price;
    private List<StockObserver> observers = new ArrayList<>();
    
    public void attach(StockObserver observer) {
        observers.add(observer);
    }
    
    public void detach(StockObserver observer) {
        observers.remove(observer);
    }
    
    public void setPrice(double price) {
        this.price = price;
        // Notifica através da abstração
        for (StockObserver observer : observers) {
            observer.update(symbol, price);
        }
    }
}

public class Investor implements StockObserver {
    private String name;
    
    public Investor(String name) {
        this.name = name;
    }
    
    public void update(String symbol, double price) {
        System.out.println(name + " recebeu atualização: " + symbol + " = $" + price);
    }
}

public class TradingAlgorithm implements StockObserver {
    public void update(String symbol, double price) {
        // Lógica de algoritmo de trading
        if (price < 10.0) {
            System.out.println("Algoritmo comprou " + symbol);
        } else if (price > 20.0) {
            System.out.println("Algoritmo vendeu " + symbol);
        }
    }
}

// Uso:
// Stock stock = new Stock();
// stock.attach(new Investor("Alice"));
// stock.attach(new TradingAlgorithm());
// stock.setPrice(15.0);
// 
// Ambos Investor e TradingAlgorithm recebem atualização
// Fácil de adicionar/remover observadores sem modificar Stock
```

### Exemplo de sistema de produção
Framework de desenvolvimento de jogos:
- **Observer:** Sistema de eventos onde entidades de jogo se inscrevem para receber eventos (colisão, entrada de usuário, etc.)
- **Command:** Sistema de ações onde movimentos, habilidades e interações são encapsuladas como objetos de comando para desfazer/refazer e replay
- **State:** Máquinas de estado de entidades onde comportamento muda baseado em estado (parado, andando, atacando, morrendo)
- **Strategy:** Comportamentos de IA onde diferentes estratégias (agressivo, defensivo, furtivo) podem ser intercambiadas
- **Mediator:** Sistema de comunicação entre entidades onde elas se comunicam através de um mediador central ao invés de diretamente
- **Memento:** Sistema de salvamento de jogo onde estado é capturado e restaurado
- **Iterator:** Percorrimento de coleções de entidades, itens ou tiles de mapa
- **Template Method:** Loop principal de jogo onde etapas específicas (processamento de entrada, atualização, renderização) podem ser customizadas
- **Visitor:** Sistema de serialização onde diferentes formatos (JSON, XML, binário) visitam a mesma estrutura de dados de jogo
- **Chain of Responsibility:** Sistema de tratamento de entrada onde eventos de teclado/mouse são processados por uma cadeia de handlers (UI, jogo, console)

### Como esse assunto pode aparecer em uma entrevista.
> 🎯 **ENTREVISTA — ALTA FREQUÊNCIA**
> 
> "Explique quando você usaria o padrão Mediator em vez de Observer para comunicação entre objetos."
> 
> **Armadilha:** Sugerir que Mediator é sempre melhor que Observer sem considerar quando comunicação direta através de observadores é mais apropriada.
> 
> **Como raciocinar:** Descrever que Observer é ideal quando há um sujeito de interesse e múltiplos observadores que reagem a mudanças nesse sujeito (relação um-para-muitos). Mediator é melhor quando há múltiplos objetos que precisam se comunicar entre si de formas complexas (relacionamento muitos-para-muitos) e você quer evitar acoplamento direto entre eles. Mostrar exemplo: Observer para notificações de mudança de preço de ações para múltiplos investidores, Mediator para sistema de controle de tráfego aéreo onde aviões precisam coordenar decolagens, pousos e manobras no solo sem se comunicarem diretamente.

## Resumo e Checklist

> 💡 **DICA DE ENTREVISTA**
> 
> Sempre relacione a escolha ao contexto específico - não trate como regra universal.

Use Design Patterns quando:
- Você identifica um problema recorrente de projeto de software
- Está refatorando código e vê oportunidades para aplicar soluções comprovadas
- Múltiplos desenvolvedores precisam trabalhar no mesmo código e precisa de vocabulário comum
- Está projetando novos sistemas e quer evitar armadilhas comuns de projeto
- Quer melhorar a flexibilidade, manutenibilidade e reutilização do código
- Precisa comunicar soluções de projeto de forma eficaz para outros desenvolvedores
- Está trabalhando com sistemas que se espera que durem e evoluam ao longo do tempo

Não use Design Patterns quando:
- Você está prototipando e velocidade é a única prioridade
- O problema é simples e não justifica a sobrecarga de um padrão
- Você está em um ambiente altamente restrito onde cada classe ou objeto conta
- O padrão não se encaixa no contexto específico (forçar um padrão leva a pior resultado)
- Você está aprendendo programação orientada a objeto básica e ainda está dominando conceitos fundamentais
- A equipe rejeita fortemente a ideia de padrões de projeto como sobreengenharia desnecessária
- Você sabe com certeza que o problema nunca vai mudar ou precisar de extensão
- O overhead de aprendizado e implementação não traz benefício proporcional ao problema

### Checklist para Design Patterns
- [ ] Identifiquei claramente o problema de projeto que estou tentando resolver?
- [ ] Selecionei o padrão apropriado baseado no contexto e nas forças envolvidas?
- [ ] Entendi a estrutura do padrão (participantes, colaborações, consequências)?
- [ ] Adaptei o padrão ao meu contexto específico (linguagem, restrições, etc.)?
- [ ] Implementei o padrão seguindo as diretrizes estabelecidas?
- [ ] Documentei onde e por que apliquei cada padrão?
- [ ] Evitei aplicar padrões onde não são necessários (prevenir overengineering)?
- [ ] Verifiquei se minha implementação respeita a intenção do padrão?
- [ ] Considerei alternativas mais simples antes de recorrer a um padrão?
- [ ] Garanti que o padrão melhora a flexibilidade, manutenibilidade ou reutilização?
- [ ] Testei minha implementação para garantir que resolve o problema identificado?
- [ ] Considerei o impacto em performance, escalabilidade, disponibilidade, consistência, segurança, custo, observabilidade e complexidade operacional?
- [ ] Documentei exemplos reais de aplicação, exemplos simplificados e exemplos de sistemas de produção?
- [ ] Expliquei como esse assunto pode aparecer em uma entrevista e forneci respostas esperadas?
- [ ] Incluí exercícios de diferentes níveis para fixar o aprendizado?

## Exercícios

### Exercício básico
Implemente o padrão Strategy para um sistema de cálculo de frete que suporte diferentes métodos (Correios, Transportadora, Entrega Própria).

### Exercício intermediário
Refatore um sistema de notificações que atualmente usa condicionais para enviar email, SMS e push notifications para usar o padrão Observer.

### Exercício avançado
Projete um sistema de edição de texto que suporte desfazer/refazer usando o padrão Memento, com capacidade de limitar o histórico de ações.

### Exercício de entrevista
Explique a diferença entre os padrões Factory Method e Abstract Factory. Forneça exemplos de quando você usaria cada um em um sistema de processamento de pagamentos com múltiplos gateways (Stripe, PayPal, PicPay).

### Desafio
Implemente um framework simples de gerenciamento de eventos usando os padrões Observer, Mediator e Command. O sistema deve permitir:
- Inscrição de ouvintes para tipos específicos de eventos
- Publicação de eventos com dados associados
- Encapsulamento de ações como objetos de comando para execução e desfazer
- Mediação de comunicação entre componentes complexos através de um mediador central