# PARTE 49 — EVENT SOURCING

## 🧠 **ESSENCIAL**
Event Sourcing é um padrão arquitetural onde as mudanças no estado de uma aplicação são armazenadas como uma sequência de eventos. Em vez de salvar apenas o estado atual, cada modificação é registrada como um evento que descreve o que aconteceu, permitindo reconstruir o estado em qualquer ponto do tempo.

## 🎯 **ENTREVISTA — ALTA FREQUÊNCIA**
- O que é Event Sourcing e como ele funciona?
- Quais são os benefícios e desafios do Event Sourcing?
- Como Event Sourcing difere de abordagens tradicionais de persistência?
- Quando usar e quando evitar Event Sourcing?
- Como implementar Event Sourcing na prática?

---

### Fundamentos do Event Sourcing

No Event Sourcing, o estado de uma entidade não é armazenado diretamente. Em vez disso, cada mudança de estado é capturada como um evento imutável. O estado atual é reconstruído reprocessando todos os eventos desde o início.

**Conceitos-chave:**

1. **Eventos**: Fatos que descrevem algo que aconteceu no passado (ex: `OrderCreated`, `ItemAddedToOrder`, `OrderShipped`)
2. **Event Store**: Armazenamento persistente de eventos em ordem sequencial
3. **Replay/Reprocessamento**: Reconstruir o estado aplicando eventos em ordem
4. **Snapshots**: Otimização para evitar reprocessar todos os eventos do zero
5. **Idempotência**: Eventos podem ser processados múltiplas vezes sem efeito colateral

### Como Funciona o Event Sourcing

Em vez de:
```sql
UPDATE Orders SET Status = 'Shipped' WHERE OrderId = 123;
```

Com Event Sourcing:
```csharp
// Armazenar evento
await eventStore.AppendAsync(new OrderShippedEvent(
    orderId: 123,
    shippedAt: DateTime.UtcNow,
    trackingNumber: "TRK123456"
));

// Reconstruir estado
var orderState = ReplayEventsForOrder(123);
```

O processo de replay:
1. Lê todos os eventos relacionados ao agregado (ex: OrderId = 123)
2. Aplica cada evento em ordem cronológica
3. Resultado: estado atual do agregado

### Estrutura de um Evento

Eventos são geralmente simples, imutáveis e descrevem fatos do passado:
```csharp
public class OrderCreated : IEvent
{
    public Guid OrderId { get; }
    public Guid CustomerId { get; }
    public DateTime OrderDate { get; }
    public List<OrderItem> Items { get; }
    
    public OrderCreated(Guid orderId, Guid customerId, DateTime orderDate, List<OrderItem> items)
    {
        OrderId = orderId;
        CustomerId = customerId;
        OrderDate = orderDate;
        Items = items;
    }
}
```

Características importantes:
- **Imutabilidade**: Depois criado, nunca muda
- **Nomeação no passado**: Descrevem algo que já aconteceu
- **Auto-descritivos**: Contêm todas as informações necessárias
- **Versionáveis**: Podem evoluir mantendo compatibilidade

### Event Store

O Event Store é o coração do padrão. Características essenciais:

1. **Append-only**: Só permite adicionar novos eventos (nunca update/delete)
2. **Ordenação garantida**: Eventos são armazenados na ordem em que ocorreram
3. **Consistência transacional**: Operações de append são atômicas
4. **Indexação por agregado**: Eficiente recuperação de eventos por ID do agregado
5. **Durabilidade**: Garantia de que eventos não serão perdidos

Implementações populares:
- EventStoreDB (produto dedicado)
- Tabelas em bancos relacionais (com cuidado)
- Apache Kafka (como log de eventos)
- Amazon DynamoDB + Lambda
- Azure Cosmos DB

### CQRS e Event Sourcing

Event Sourcing é frequentemente combinado com CQRS, onde:
- **Lado de escrita**: Usa Event Sourcing para persistir comandos como eventos
- **Lado de leitura**: Projeções atualizam modelos de leitura baseado nos eventos

Fluxo típico:
1. Command chega (ex: `CreateOrderCommand`)
2. Handler valida e cria um ou mais eventos (ex: `OrderCreated`)
3. Eventos são salvos no Event Store
4. Projeções (event handlers) atualizam modelos de leitura
5. Consulta lê do modelo de leitura otimizado

### Benefícios do Event Sourcing

1. **Audit Trail Completo**: Histórico total de todas as mudanças
2. **Reconstrução de Estado**: Pode reconstruir estado em qualquer ponto do tempo
3. **Depuração e Análise**: Fácil entender como um estado foi alcançado
4. **Forense e Conformidade**: Atende requisitos de auditoria e regulatórios
5. **Reprocessamento**: Pode remodelar modelos de leitura reprocessando eventos
6. **Integração**: Outros sistemas podem consumir eventos diretamente
7. **Desacoplamento**: Escritores e leitores podem evoluir independentemente
8. **Escalabilidade**: Leitura e escrita podem ser escalados separadamente
9. **Tempo de viagem**: Consultas "como estava ontem às 3 PM" são naturais

### Desafios e Considerações

1. **Complexidade Aumentada**: Novo paradigma para aprender
2. **Volume de Dados**: Eventos acumulam rapidamente (mas geralmente barato de armazenar)
3. **Consistência Eventual**: Entre escrita e leitura pode haver atraso
4. **Migração de Esquema**: Evoluir eventos pode ser complexo
5. **Performance de Leitura**: Reprocessar muitos eventos pode ser lento (resolvido com snapshots)
6. **Consultas Ad-hoc**: Difícil fazer consultas complexas diretamente nos eventos
7. **Exclusão de Dados**: "Direito ao esquecimento" (GDPR) é desafiador
8. **Curva de Aprendizado**: Equipe precisa internalizar o pensamento baseado em eventos

### Snapshots: Otimizando a Leitura

Para evitar reprocessar milhares de eventos a cada carregamento, usamos snapshots:

```csharp
// Salvar snapshot a cada N eventos ou período de tempo
if (eventsSinceLastSnapshot >= SNAPSHOT_THRESHOLD)
{
    var snapshot = new OrderSnapshot
    {
        OrderId = order.Id,
        Status = order.Status,
        TotalAmount = order.CalculateTotal(),
        LastUpdated = DateTime.UtcNow,
        Version = events.Count
    };
    
    await snapshotStore.SaveAsync(snapshot);
}

// Carregar estado: snapshot + eventos posteriores
var snapshot = await snapshotStore.LoadLatestAsync(orderId);
var recentEvents = await eventStore.LoadEventsSinceAsync(orderId, snapshot.Version);
var orderState = ApplyEventsToSnapshot(snapshot, recentEvents);
```

### Versionamento de Eventos

Eventos precisam evoluir com o tempo. Estratégias:

1. **Adicionar Campos**: Novos campos com valores padrão
2. **Event Upgraders**: Processadores que convertem eventos antigos para novos
3. **Versionamento Explícito**: Incluir versão no evento e ter handlers por versão
4. **Upcasters no Event Store**: Transformar eventos na leitura

Exemplo de upgrader:
```csharp
public class OrderCreatedV2Upgrader : IEventUpgrader
{
    public IEnumerable<IEvent> Upgrade(OrderCreatedV1 @event)
    {
        // Converter V1 para V2
        yield return new OrderCreatedV2(
            @event.OrderId,
            @event.CustomerId,
            @event.OrderDate,
            @event.Items,
            Currency.USD // Novo campo com valor padrão
        );
    }
}
```

### Arquitetura com Event Sourcing

Componentes típicos:

1. **Agregados**: Entidades que mantêm consistência transacional
2. **Comandos**: Intenções de mudar estado (ex: `AddItemToOrder`)
3. **Eventos**: Fatos que já aconteceram (ex: `ItemAddedToOrder`)
4. **Handlers de Comando**: Validam comandos e produzem eventos
5. **Event Store**: Persiste eventos
6. **Projeções**: Atualizam modelos de leitura baseado em eventos
7. **Modelos de Leitura**: Otimizados para consultas
8. **Snapshots**: Otimização de performance
9. **Consumidores Externos**: Outros sistemas que reagem aos eventos

### Implementação Prática

Exemplo completo simplificado:

```csharp
// Agregado
public class Order : AggregateRoot
{
    public Guid CustomerId { get; private set; }
    public OrderStatus Status { get; private set; }
    public List<OrderLine> Lines { get; private set; } = new();
    
    public Order(Guid id, Guid customerId)
    {
        Apply(new OrderCreated { Id = id, CustomerId = customerId });
    }
    
    public void AddItem(Product product, int quantity)
    {
        if (Status != OrderStatus.Draft)
            throw new InvalidOperationException("Cannot modify non-draft order");
            
        Apply(new ItemAddedToOrder {
            OrderId = Id,
            ProductId = product.Id,
            ProductName = product.Name,
            Quantity = quantity,
            UnitPrice = product.Price
        });
    }
    
    // Event handlers (aplicam estado)
    public void Apply(OrderCreated @event)
    {
        Id = @event.Id;
        CustomerId = @event.CustomerId;
        Status = OrderStatus.Draft;
    }
    
    public void Apply(ItemAddedToOrder @event)
    {
        Lines.Add(new OrderLine {
            ProductId = @event.ProductId,
            ProductName = @event.ProductName,
            Quantity = @event.Quantity,
            UnitPrice = @event.UnitPrice
        });
    }
    
    // Outros Apply methods...
}

// Command Handler
public class AddItemToOrderCommandHandler
{
    private readonly IRepository<Order> _repository;
    
    public async Task Handle(AddItemToOrderCommand command)
    {
        var order = await _repository.GetByIdAsync<Order>(orderId: command.OrderId);
        order.AddItem(command.Product, command.Quantity);
        await _repository.SaveAsync(order);
    }
}

// Projection (atualiza modelo de leitura)
public class OrderDetailsProjection : IEventHandler<OrderCreated>,
                                     IEventHandler<ItemAddedToOrder>,
                                     IEventHandler<OrderShipped>
{
    private readonly IOrderReadRepository _readRepository;
    
    public Task Handle(OrderCreated @event) => 
        _readRepository.CreateAsync(new OrderDetailsDto {
            OrderId = @event.OrderId,
            CustomerId = @event.CustomerId,
            Status = OrderStatus.Draft,
            Lines = new List<OrderLineDto>()
        });
    
    public Task Handle(ItemAddedToOrder @event) =>
        _readRepository.AddOrderLineAsync(@event.OrderId, 
            new OrderLineDto {
                ProductId = @event.ProductId,
                ProductName = @event.ProductName,
                Quantity = @event.Quantity,
                UnitPrice = @event.UnitPrice
            });
    
    // Outros handlers...
}
```

### Quando Usar Event Sourcing

Event Sourcing brilha quando:

1. **Audit Trail é Essencial**: Sistemas financeiros, saúde, jurídico
2. **Requisitos de Conformidade**: Necessidade de provar como estado foi alcançado
3. **Análise e Business Intelligence**: Querer analisar comportamentos ao longo do tempo
4. **Colaboração e Trabalho em Tempo Real**: Sistemas como Google Docs
5. **Integração de Sistemas**: Outros sistemas precisam consumir mudanças em tempo real
6. **Domínios Complexos com Muitas Regras de Negócio**: Rastrear como decisões foram tomadas
7. **Sistemas com Múltiplas Visões dos Mesmos Dados**: Diferentes usuários veem o mesmo dado de formas diferentes
8. **Recuperação de Erros**: Capacidade de "desfazer" ou reprocessar após correção de bugs

### Quando Evitar Event Sourcing

Considere alternativas quando:

1. **Simplicidade é Prioridade**: Aplicações CRUD simples
2. **Volume Extremamente Alto de Escritas**: Pode tornar impraticável (embora seja raro)
3. **Consistência Imediata Absolutamente Necessária**: Embora seja possível, adiciona complexidade
4. **Equipe Sem Experiência**: Curva de aprendizado significativa
5. **Consultas Ad-hoc Complexas Diretas nos Dados**: Melhor usar data warehouse para isso
6. **Requisitos de Exclusão Estrita de Dados**: GDPR "right to be forgotten" é desafiador
7. **Prototipagem ou MVPs**: Overhead pode não valer a pena inicialmente
8. **Quando o Estado Atual é Suficiente**: Não há necessidade de histórico ou replay

### Tecnologias e Frameworks

**Frameworks dedicados:**
- **EventStoreDB**: Banco de dados otimizado para event sourcing
- **Axon Framework** (Java/JVM): Suporte completo a CQRS e Event Sourcing
- **NServiceBus** (.NET): Com suporte a sagas e persistência
- **MediatR + EventStoreDB** (.NET): Combinação popular
- **Spring Cloud Stream** (Java): Com suporte a event sourcing

**Infraestrutura de apoio:**
- **Apache Kafka**: Excelente para streaming de eventos em escala
- **Amazon Kinesis**: Alternativa AWS para streaming
- **Google Cloud Pub/Sub**: Para arquiteturas nativas cloud
- **Redis**: Para caches e snapshots rápidos
- **PostgreSQL/MongoDB**: Pode ser usado como event store simples (com limitações)

### Padrões Relacionados

Event Sourcing trabalha bem com:

1. **CQRS**: Separação natural entre escrita (eventos) e leitura (projeções)
2. **Saga Pattern**: Gerenciamento de transações distribuídas usando eventos
3. **Event-Driven Architecture**: Comunicação assíncrona baseada em eventos
4. **Microserviços**: Serviços podem se comunicar através de eventos
5. **Domain-Driven Design**: Agregados e eventos naturalmente modelam o domínio
6. **Materialized Views**: Projeções são essencialmente views materializadas
7. **Event Notification**: Outros sistemas se inscrevem em eventos relevantes

### Considerações de Operação e Monitoramento

Em produção com Event Sourcing, monitore:

1. **Throughput de Eventos**: Eventos por segundo sendo escritos
2. **Latência de Persistência**: Tempo para salvar evento
3. **Tamanho do Event Store**: Crescimento de armazenamento ao longo do tempo
4. **Lag de Projeções**: Atrás que as projeções estão do event store
5. **Taxa de Falhas**: Eventos que falham ao serem processados
6. **Uso de Snapshots**: Efetividade da estratégia de snapshots
7. **Throughput de Leitura**: Performance das consultas nos modelos de leitura
8. **Dead Letter Queues**: Eventos que repetidamente falham no processamento

### Estudos de Caso

**Martin Fowler**: Popularizou o conceito através de seus escritos, citando uso em sistemas de trading financeiro onde cada transação precisa ser auditável.

**LMAX Exchange**: Usa arquitetura baseada em eventos para seu sistema de trading de alta performance, processando milhões de transações por dia com rastreabilidade completa.

**Github**: Utiliza variações de event sourcing para seu sistema de eventos (Pull Requests, Issues, Commits) permitindo reconstrução completa do estado do repositório em qualquer momento.

**Uber**: Em seus sistemas de pagamento e corridas, usa event sourcing para manter audit trail completo e permitir reconciliação financeira precisa.

### Resumo

Event Sourcing é um padrão poderoso que transforma como pensamos sobre persistência de dados. Ao armazenar eventos em vez de estado atual, ganhamos capacidade de auditoria, reprocessamento e integração que são difíceis de alcançar com abordagens tradicionais.

Embora introduza complexidade adicional em termos de aprendizado de novos paradigmas e gerenciamento de volume de eventos, os benefícios em termos de rastreabilidade, flexibilidade e capacidades de análise fazem valer a pena para muitos domínios de negócio.

A chave para o sucesso com Event Sourcing está em entender seus trade-offs e aplicá-lo onde seus benefícios - particularmente audit trail, capacidade de reprocessamento e integração em tempo real - proporcionam vantagens claras sobre abordagens mais simples. Quando combinado com CQRS e práticas de microsserviços, Event Sourcing pode formar a base para sistemas altamente escaláveis, auditáveis e responsivos.