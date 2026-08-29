# PARTE 48 — CQRS

## 🧠 **ESSENCIAL**
CQRS (Command Query Responsibility Segregation) é um padrão arquitetural que separa as operações de leitura (queries) das operações de escrita (commands) em um sistema, permitindo que cada lado seja otimizado independentemente para melhor desempenho, escalabilidade e manutenibilidade.

## 🎯 **ENTREVISTA — ALTA FREQUÊNCIA**
- O que é CQRS e quando devo usá-lo?
- Como o CQRS difere de arquiteturas tradicionais?
- Quais são os benefícios e desafios do CQRS?
- Como implementar CQRS com event sourcing?
- Quando não usar CQRS?

---

### Fundamentos do CQRS

O padrão CQRS surge da observação de que, na maioria dos sistemas, as operações de leitura e escrita têm requisitos diferentes:

**Operações de Escrita (Commands):**
- Modificam o estado do sistema
- Precisam ser consistentes e transacionais
- Geralmente têm menos volume, mas maior complexidade de negócio
- Focam em validações e regras de negócio

**Operações de Leitura (Queries):**
- Apenas leem o estado do sistema
- Não modificam dados
- Podem ter volume muito maior
- Focam em performance e flexibilidade de apresentação

Em sistemas tradicionais, um único modelo de dados tenta atender ambos os lados, resultando em compensações. O CQRS propõe dividir essa responsabilidade.

### Modelo de Comandos (Command Model)

O lado de comandos lida com:
- Validação de entrada
- Aplicação de regras de negócio
- Atualização do estado
- Publicação de eventos (quando combinado com Event Sourcing)

Commands são objetos que representam uma intenção de mudança no sistema:
```csharp
public class CreateOrderCommand : ICommand
{
    public Guid CustomerId { get; set; }
    public List<OrderItemDto> Items { get; set; }
    public DateTime OrderDate { get; set; }
}
```

Handlers processam esses commands:
```csharp
public class CreateOrderCommandHandler : ICommandHandler<CreateOrderCommand>
{
    private readonly IOrderRepository _repository;
    
    public async Task Handle(CreateOrderCommand command)
    {
        // Validações de negócio
        var order = new Order(command.CustomerId, command.Items, command.OrderDate);
        await _repository.AddAsync(order);
        // Possivelmente publicar eventos
    }
}
```

### Modelo de Consulta (Query Model)

O lado de consulta é otimizado para:
- Performance de leitura
- Flexibilidade na estrutura de dados retornada
- Escalabilidade horizontal
- Uso de tecnologias específicas para leitura (caches, read replicas, etc.)

Queries são objetos que representam uma solicitação de leitura:
```csharp
public class GetOrderDetailsQuery : IQuery<OrderDetailsDto>
{
    public Guid OrderId { get; set; }
}
```

Query handlers retornam DTOs otimizados para consumo:
```csharp
public class GetOrderDetailsQueryHandler : IQueryHandler<GetOrderDetailsQuery, OrderDetailsDto>
{
    private readonly IOrderReadRepository _readRepository;
    
    public async Task<OrderDetailsDto> Handle(GetOrderDetailsQuery query)
    {
        var order = await _readRepository.GetOrderDetailsAsync(query.OrderId);
        return new OrderDetailsDto
        {
            OrderId = order.Id,
            CustomerName = order.Customer.Name,
            Items = order.Items.Select(i => new OrderItemDto
            {
                ProductName = i.Product.Name,
                Quantity = i.Quantity,
                Price = i.Price
            }).ToList(),
            Total = order.Items.Sum(i => i.Quantity * i.Price)
        };
    }
}
```

### Benefícios do CQRS

1. **Separação de Preocupações**: Modelos de leitura e escrita podem evoluir independentemente
2. **Otimização de Performance**: Cada lado pode ser otimizado para seu caso de uso específico
3. **Escalabilidade**: Lados de leitura e escrita podem ser escalados independentemente
4. **Flexibilidade**: Modelos de leitura podem ser adaptados para diferentes interfaces de usuário
5. **Manutenibilidade**: Código mais simples e focado em cada lado
6. **Compatibilidade com DDD**: Naturalmente se integra com conceitos de Domain-Driven Design

### Desafios e Complexidades

1. **Eventual Consistência**: Entre os modelos de leitura e escrita pode haver atraso na sincronização
2. **Complexidade Aumentada**: Mais componentes para manter e monitorar
3. **Duplicação de Dados**: Mesmo dado pode existir em múltiplas representações
4. **Curva de Aprendizado**: Equipe precisa entender o padrão e suas implicações
5. **Depuração Mais Complexa**: Rastrear mudanças pode envolver múltiplos sistemas

### CQRS com Event Sourcing

CQRS é frequentemente combinado com Event Sourcing, onde o lado de escrita armazena eventos que representam mudanças de estado, em vez do estado atual:

```csharp
// Command Handler com Event Sourcing
public class CreateOrderCommandHandler : ICommandHandler<CreateOrderCommand>
{
    private readonly IEventStore _eventStore;
    
    public async Task Handle(CreateOrderCommand command)
    {
        // Validações
        var orderCreated = new OrderCreatedEvent(
            Guid.NewGuid(), 
            command.CustomerId, 
            command.Items, 
            command.OrderDate);
        
        await _eventStore.SaveEventsAsync(orderCreated);
        // O modelo de leitura será atualizado pelos eventos
    }
}
```

O modelo de leitura é então construído através de um processo de replay dos eventos:
```csharp
// Projection que atualiza o modelo de leitura
public class OrderProjection : IEventHandler<OrderCreated>
{
    private readonly IOrderReadRepository _readRepository;
    
    public Task Handle(OrderCreated @event)
    {
        var orderDto = new OrderDetailsDto
        {
            OrderId = @event.OrderId,
            CustomerId = @event.CustomerId,
            OrderDate = @event.OrderDate,
            Items = @event.Items.Select(i => new OrderItemDto
            {
                ProductId = i.ProductId,
                Quantity = i.Quantity,
                Price = i.Price
            }).ToList()
        };
        
        return _readRepository.SaveAsync(orderDto);
    }
}
```

### Quando Usar CQRS

CQRS é particularmente útil quando:
- O sistema tem alto volume de leituras comparado a escritas
- Modelos de leitura e escrita têm requisitos muito diferentes
- Há necessidade de múltiplas representações dos mesmos dados para diferentes usuários
- Performance de leitura é crítica e precisa de otimizações específicas
- O domínio é complexo e beneficia-se da separação clara de responsabilidades
- Planeja-se usar microsserviços ou arquiteturas baseadas em eventos

### Quando Não Usar CQRS

Evite CQRS quando:
- O sistema é simples com baixo volume de dados
- As operações de leitura e escrita são semelhantes em complexidade e volume
- A equipe não tem experiência com padrões avançados
- A consistência imediata é absolutamente necessária
- O overhead de complexidade não justifica os benefícios
- Estou prototipando ou construindo um MVP

### Tecnologias e Frameworks

Várias tecnologias facilitam a implementação de CQRS:

**.NET:**
- MediatR (para comandos e queries)
- EventStoreDB (para event sourcing)
- NServiceBus (para mensageria)

**Java:**
- Axon Framework
- Spring Cloud Stream
- LMAX Disruptor

**Node.js:**
- NestJS CQRS module
- MediatR equivalent libraries
- Redis para caches de leitura

**Infraestrutura:**
- Message brokers (RabbitMQ, Apache Kafka, AWS SQS/SNS)
- Caches distribuídos (Redis, Memcached)
- Data warehouses para análises
- Read replicas de bancos de dados

### Padrões Relacionados

CQRS frequentemente trabalha junto com outros padrões:

1. **Event Sourcing**: Armazenamento de eventos em vez de estado atual
2. **Event-Driven Architecture**: Comunicação assíncrona através de eventos
3. **Microserviços**: Serviços independentes com responsabilidade única
4. **Domain-Driven Design**: Foco no domínio de negócio e modelos ricos
5. **Materialized Views**: Pré-computação de dados para consultas rápidas
6. **Saga Pattern**: Gerenciamento de transações distribuídas

### Considerações de Implementação

Ao implementar CQRS, considere:

**Modelo de Comandos:**
- Validação rigorosa na borda do sistema
- Commands devem ser imutáveis
- Use handlers transacionais
- Considere validação em duas camadas (borda e domínio)

**Modelo de Consultas:**
- Projete DTOs específicos para cada uso
- Use tecnologias otimizadas para leitura
- Implemente caching estratégico
- Considere denormalização controlada

**Integração entre Lados:**
- Mecanismo de propagação de mudanças (eventos, triggers, etc.)
- Tratamento de eventuais inconsistências
- Monitoramento de lag entre modelos
- Estratégias de recuperação de falhas

### Segurança em CQRS

A segurança precisa ser considerada em ambos os lados:

**Comandos:**
- Autenticação e autorização de quem pode executar comandos
- Validação de dados de entrada
- Proteção contra comandos maliciosos
- Auditoria de mudanças

**Consultas:**
- Controle de acesso aos dados retornados
- Filtragem de informações sensíveis
- Proteção contra vazamento de dados
- Rate limiting para evitar abusos

### Testando CQRS

Teste ambos os lados independentemente:

**Testes de Comandos:**
- Testar validações de negócio
- Verificar mudanças de estado corretas
- Testar publicação de eventos
- Simular falhas de validação

**Testes de Consultas:**
- Verificar precisão dos dados retornados
- Testar performance sob carga
- Validar mapeamentos de DTO
- Testar cenários de dados ausentes

### Monitoramento e Observabilidade

Em sistemas CQRS, monitore:

**Lado de Escrita:**
- Taxa de processamento de comandos
- Tempo de validação e processamento
- Taxa de falhas de validação
- Latência de publicação de eventos

**Lado de Leitura:**
- Latência de resposta das consultas
- Taxa de acerto/falha do cache
- Lag entre modelos de leitura e escrita
- Throughput de consultas por segundo

**Integração:**
- Atraso na propagação de eventos
- Falhas no processamento de eventos
- Consistencia entre modelos
- Uso de recursos em cada lado

### Estudos de Caso

**Amazon:** Usa variações de CQRS em seus sistemas de recomendação, onde o modelo de leitura é otimizado para acesso rápido a produtos, enquanto o modelo de escrita lida com atualizações de estoque e preços.

**Netflix:** Implementa CQRS em seus sistemas de conteúdo, separando as operações de catastrofe (leitura intensiva) das operações de gerenciamento de conteúdo (escrita com regras complexas).

**Uber:** Utiliza CQRS em seus sistemas de corrida, onde o modelo de leitura fornece informações em tempo real para usuários e motoristas, enquanto o modelo de escrita processa solicitações de corrida e pagamentos.

### Resumo

CQRS é um padrão poderoso que separa as responsabilidades de leitura e escrita em um sistema, permitindo que cada lado seja otimizado independentemente. Embora introduza complexidade adicional, oferece benefícios significativos em termos de desempenho, escalabilidade e manutenibilidade para sistemas com requisitos diferentes entre leituras e escritas.

A chave para o sucesso com CQRS está em entender quando aplicá-lo - ele não é uma solução universal, mas sim uma ferramenta valiosa para cenários específicos onde a separação de preocupações traz vantagens claras. Quando combinado com padrões como Event Sourcing e arquiteturas orientadas a eventos, CQRS pode formar a base para sistemas altamente escaláveis e responsivos.

