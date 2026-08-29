---
trilha: "AVANÇADA"
---
**Navegação:** [[MOC — TRILHA AVANÇADA]]
← [[PARTE 0 — MAPA DA documenta��o]] | #trilha/avancada | [[PARTE 22 — DISTRIBUTED TRANSACTIONS]] →

---
# PARTE 21 — TRANSACTIONS

> 🧠 **ESSENCIAL**
> Transações são unidades de trabalho indivisíveis que garantem integridade e consistência em sistemas de banco de dados, formando a base para operações confiáveis em aplicações críticas.

> 🎯 **ENTREVISTA — ALTA FREQUÊNCIA**
> Perguntas sobre propriedades ACID, níveis de isolamento, fenômenos de concorrência (dirty read, non-repeatable read, phantom read), deadlocks e estratégias de tratamento são extremamente comuns em entrevistas de arquitetura de software.

## O que são Transações?

**Transações** são sequências de operações executadas como uma unidade lógica única de trabalho, onde ou todas as operações são executadas com sucesso (commit) ou nenhuma é executada (rollback), mantendo o banco de dados em um estado consistente.

Uma transação deve seguir as propriedades ACID:
- **Atomicidade**: Todas as operações são tratadas como uma unidade indivisível
- **Consistência**: A transação leva o banco de um estado válido para outro estado válido
- **Isolamento**: Operações de transações concorrentes não se interferem
- **Durabilidade**: Uma vez confirmada, a transação persiste mesmo diante de falhas

## Por que existem?

À medida que aplicações evoluíram para lidar com operações complexas e múltiplos usuários simultâneos, surgiram desafios que transações resolvem:

- **Atualizações parciais**: Sem atomicidade, falhas podem deixar dados em estado inconsistente
- **Corrupção de dados**: Sem consistência, regras de negócio podem ser violadas
- **Interferência concorrente**: Sem isolamento, transações podem ler dados intermediários ou sobrescrever umas às outras
- **Perda de dados**: Sem durabilidade, confirmações podem ser perdidas em falhas de sistema
- **Complexidade de recuperação**: Sem transações, restaurar consistência após falhas é extremamente difícil

## Problema que resolve

Transações resolvem vários problemas críticos de integridade e concorrência:

1. **Estado inconsistente**: Garante que o banco nunca fique em estado intermediário inválido
2. **Violação de regras de negócio**: Assegura que todas as restrições sejam respeitadas
3. **Anomalias de concorrência**: Previne dirty reads, non-repeatable reads e phantom reads
4. **Perda de confirmação**: Assegura que transações confirmadas sobrevivam a falhas
5. **Complexidade de recuperação**: Fornece mecanismos claros para rollback e recuperação
6. **Confiança do usuário**: Usuários podem confiar que operações sejam completas ou não ocorram

## Como funciona internamente

Transações implementam diversos mecanismos para garantir as propriedades ACID:

### Mecanismos de Atomicidade e Durabilidade
- **Log de transações (WAL - Write-Ahead Logging)**: Todas as modificações são primeiro escritas em log antes de serem aplicadas ao banco
- **Checkpoint**: Periódicamente, dados modificados são escritos do buffer para disco
- **Recuperação**: Em caso de falha, o log é usado para redo (refazer) ou undo (desfazer) transações

### Mecanismos de Isolamento
- **Bloqueio (Locking)**: Recursos são bloqueados para impedir acesso concorrente conflitante
- **Versionamento (MVCC - Multi-Version Concurrency Control)**: Múltiplas versões de dados são mantidas para leituras não bloqueantes
- **Validação (Optimistic Concurrency Control)**: Conflitos são detectados no momento do commit

### Gerenciamento de Transações
- **Transaction Manager**: Coordena início, commit e rollback de transações
- **Resource Manager**: Gerencia acesso a recursos específicos (tabelas, índices)
- **Two-Phase Commit (2PC)**: Protocolo para transações distribuídas (coberto na PARTE 22)

## Propriedades ACID em Detalhe

### 1. Atomicidade (Atomicity)

**Definição**: Uma transação é tratada como uma unidade indivisível - ou todas as operações são executadas ou nenhuma é.

**Como funciona**:
- Todas as modificações são registradas no log antes de serem aplicadas
- Se qualquer operação falhar, o sistema usa o log para desfazer todas as alterações feitas até aquele ponto
- Rollback pode ser explícito (comando ROLLBACK) ou implícito (falha, timeout)

**Exemplo**:
```sql
BEGIN TRANSACTION;
UPDATE accounts SET balance = balance - 100 WHERE id = 1;
UPDATE accounts SET balance = balance + 100 WHERE id = 2;
-- Se o segundo UPDATE falhar, o primeiro é automaticamente desfeito
COMMIT;
```

**Quando usar**: Sempre que múltiplas operações devem ocorrer juntas ou nenhuma deve ocorrer.

### 2. Consistência (Consistency)

**Definição**: Uma transação preserva a consistência do banco de dados - ela leva o banco de um estado válido para outro estado válido conforme definido por restrições, gatilhos e regras de integridade.

**Como funciona**:
- Antes da transação: banco está em estado consistente (todas as restrições satisfeitas)
- Durante a transação: restrições podem ser temporariamente violadas
- Após a transação: se todas as operações foram bem-sucedidas, banco retorna ao estado consistente
- Se a transação for abortada: banco retorna ao estado consistente anterior

**Exemplos de restrições que garantem consistência**:
- Chaves primárias e estrangeiras
- Restrições UNIQUE, NOT NULL, CHECK
- Gatilhos que enforcem regras de negócio complexas

**Quando usar**: Sempre que houver regras de negócio que devem ser invariantes.

### 3. Isolamento (Isolation)

**Definição**: Operações de transações concorrentes são executadas como se fossem sequenciais - nenhuma transação vê os efeitos intermediários de outra transação concorrente.

**Níveis de Isolamento** (definidos pelo padrão SQL-92):
1. **READ UNCOMMITTED**: Menor isolamento, permite dirty reads
2. **READ COMMITTED**: Padrão em muitos bancos, evita dirty reads
3. **REPEATABLE READ**: Evita dirty e non-repeatable reads
4. **SERIALIZABLE**: Maior isolamento, evita todos os fenômenos de concorrência

**Fenômenos de Concorrência Prevenidos**:
- **Dirty Read**: Ler dados modificados por outra transação ainda não confirmada
- **Non-Repeatable Read**: Ler a mesma linha duas vezes e obter valores diferentes
- **Phantom Read**: Executar a mesma consulta duas vezes e obter conjuntos de linhas diferentes
- **Lost Update**: Duas transações leem, modificam e escrevem o mesmo dado, perdendo uma atualização

**Como funciona**:
- **Bloqueio Pessimista**: Recursos são bloqueados antes do acesso (shared/exclusive locks)
- **Versionamento Otimista**: Múltiplas versões são mantidas, conflitos verificados no commit
- **Escalonamento**: Sistema determina ordem segura de execução das transações

### 4. Durabilidade (Durability)

**Definição**: Uma vez que uma transação foi confirmada (commit), suas alterações persistirão mesmo diante de falhas de sistema, energia ou disco.

**Como funciona**:
- **Log de transações**: Todas as modificações são primeiro escritas em log durável antes de serem aplicadas aos dados
- **Força de log (force log)**: Commit só retorna após o log ser fisicamente escrito em disco
- **Recuperação após falha**: Sistema reexecuta transações confirmadas do log (redo) e desfaz transações não confirmadas (undo)

**Trade-off**: Durabilidade forte requer I/O síncrono no log, o que pode limitar throughput.

## Níveis de Isolamento em Detalhe

### READ UNCOMMITTED
- **Permite**: Dirty reads, non-repeatable reads, phantom reads
- **Bloqueios**: Nenhum bloqueio de leitura (apenas bloqueios de escrita para evitar lost updates)
- **Performance**: Maior concorrência, menor sobrecarga
- **Quando usar**: Aplicações onde leitura de dados potencialmente inconsistente é aceitável (estatísticas aproximadas, logs)
- **Quando evitar**: Qualquer situação onde consistência é importante

### READ COMMITTED
- **Permite**: Non-repeatable reads, phantom reads
- **Bloqueios**: Bloqueios de leitura liberados imediatamente após leitura (no nível de linha)
- **Performance**: Boa concorrência, evita o problema mais grave (dirty read)
- **Padrão**: Em muitos bancos como Oracle, SQL Server, PostgreSQL
- **Quando usar**: Aplicações gerais onde leitura de dados comprometidos não é tolerada
- **Exemplo clássico**: Ler saldo de conta enquanto transferência está em andamento - verá apenas o estado antes ou depois, nunca durante

### REPEATABLE READ
- **Permite**: Phantom reads
- **Bloqueios**: Bloqueios de leitura mantidos até fim da transação (evita non-repeatable reads)
- **Performance**: Menor concorrência que READ COMMITTED devido a bloqueios mais longos
- **Quando usar**: Quando é crítico que leituras repetidas retornem os mesmos valores
- **Exemplo**: Ler preço de produto duas vezes durante uma transação de compra para garantir consistência

### SERIALIZABLE
- **Permite**: Nenhum fenômeno de concorrência
- **Bloqueios**: Bloqueios de faixa (range locks) para evitar phantom reads
- **Performance**: Menor concorrência, maior chance de deadlocks
- **Quando usar**: Quando é necessário garantir execução serializável (como se transações ocorressem uma após a outra)
- **Exemplo**: Sistemas financeiros onde integridade absoluta é requerida

### Níveis de Isolamento Específicos de Bancos
- **Snapshot Isolation** (PostgreSQL, SQL Server): Versão do MVCC que fornece consistência semelhante a SERIALIZABLE para muitas cargas de trabalho
- **Read Committed Snapshot** (SQL Server): Versão otimista de READ COMMITTED
- **Serializable Snapshot Isolation** (PostgreSQL): Implementação verdadeiramente serializável usando MVCC

## Estratégias de Tratamento de Concorrência

### 1. Bloqueio Pessimista (Pessimistic Locking)
- **Como funciona**: Bloqueios são adquiridos antes de acessar recursos e mantidos até fim da transação
- **Tipos de bloqueios**:
  - **Shared (S)**: Permite leituras concorrentes, bloqueia escritas
  - **Exclusive (X)**: Bloqueia tanto leituras quanto escritas
  - **Update (U)**: Usado em atualizações para evitar deadlocks em padrão leitura-then-escrita
  - **Intent Locks**: Bloqueios de nível de tabela que indicam intenção de adquirir bloqueios de nível de linha
- **Protocolos**:
  - **Two-Phase Locking (2PL)**: Fase de crescimento (adquirir bloqueios) seguida de fase de encolhimento (liberar bloqueios)
  - **Strict 2PL**: Bloqueios exclusivos mantidos até commit (evita cascading aborts)
- **Vantagens**: Preveni conflitos antes que ocorram
- **Desvantagens**: Pode causar deadlocks, reduz concorrência, overhead de gerenciamento de bloqueios
- **Quando usar**: Altas taxas de conflito, transações longas, quando consistência é crítica

### 2. Controle de Concorrência Otimista (Optimistic Concurrency Control - OCC)
- **Como funciona**: Transações prosseguem sem bloqueios, verificando conflitos apenas no momento do commit
- **Processo**:
  1. **Leitura**: Transação lê dados e registra versão/timestamp
  2. **Modificação**: Transação trabalha com cópias locais
  3. **Validação**: No commit, verifica se dados lidos foram modificados por outras transações
  4. **Decisão**: Se nenhum conflito, commit; se conflito, rollback e retry
- **Mecanismos de validação**:
  - **Timestamps**: Cada transação tem timestamp; verifica ordem de timestamps
  - **Versões**: Cada linha tem número de versão; verifica se versão mudou
  - **Valores**: Compara valores reais lidos (mais caro, mas mais preciso)
- **Vantagens**: Melhor concorrência quando baixas taxas de conflito, evita bloqueios
- **Desvantagens**: Overhead de validação, waste de trabalho em caso de conflitos altos, possibilidade de starvation
- **Quando usar**: Baixas taxas de conflito, transações curtas, alto volume de leituras

### 3. Versionamento Multiversão (MVCC - Multi-Version Concurrency Control)
- **Como funciona**: Mantém múltiplas versões de dados; cada transação vê um snapshot consistente baseado no horário de início
- **Implementação**:
  - Cada modificação cria nova versão em vez de sobrescrever
  - Versões antigas são mantidas até que não sejam mais necessárias por nenhuma transação ativa
  - Limpeza (vacuum/garbage collection) remove versões obsoletas
- **Vantagens**: Leituras não bloqueantes, boa combinação de desempenho e consistência
- **Desvantagens**: Overhead de armazenamento (múltiplas versões), necessidade de limpeza
- **Quando usar**: Cargas de trabalho com muitas leituras e escritas moderadas (padrão em muitos sistemas web)
- **Bancos que usam**: PostgreSQL, MySQL/InnoDB, Oracle, SQL Server (com snapshot isolation)

## Deadlocks

**Definição**: Situação onde duas ou mais transações estão esperando uma pelas outras para liberar recursos, resultando em impasse.

**Condições necessárias** (Coffman conditions):
1. **Exclusão mútua**: Recursos não podem ser compartilhados
2. **Retenção e espera**: Transações seguram recursos enquanto esperam por outros
3. **Não preempção**: Recursos não podem serem retirados forçosamente
4. **Espera circular**: Cadeia de transações onde cada uma espera pela próxima

**Exemplo clássico**:
- Transação A: Bloqueia linha 1, espera linha 2
- Transação B: Bloqueia linha 2, espera linha 1

**Estratégias de tratamento**:
1. **Prevenção**: Projetar sistema para que uma das condições nunca ocorra
   - Ordenação de recursos: Sempre adquirir bloqueios na mesma ordem global
   - Timeouts: Limitar tempo de espera por bloqueios
   - Escalonamento: Evitar retenção e espera adquirindo todos os bloqueios de uma vez
2. **Detecção**: Permitir deadlocks ocorrerem, então detectar e recuperar
   - Algoritmos de detecção de grafos (wait-for graph)
   - Vítima escolhida com base em custo (menos trabalho feito, mais jovem, etc.)
   - Rollback da vítima e notificação à aplicação
3. **Ignorar**: Deixar aplicação lidar com timeouts e retry (menos comum em bancos)

**Como evitar na prática**:
- Sempre acessar tabelas/linhas na mesma ordem
- Manter transações o mais curtas possível
- Usar níveis de isolamento mais baixos quando aceitável
- Evitar bloqueios explícitos quando possível (confiar no isolamento do banco)
- Monitorar e alertar sobre frequência de deadlocks

## Pontos de Salvação (Savepoints)

**Definição**: Marcadores dentro de uma transação que permitem rollback parcial até aquele ponto sem abortar toda a transação.

**Como funciona**:
- `SAVEPOINT point_name`: Define um ponto de salvação
- `ROLLBACK TO point_name`: Desfaz operações desde o ponto de salvação
- `RELEASE SAVEPOINT point_name`: Remove ponto de salvação (libera recursos)
- Pode-se ter múltiplos pontos de salvação aninhados

**Exemplo**:
```sql
BEGIN TRANSACTION;
UPDATE accounts SET balance = balance - 100 WHERE id = 1;
SAVEPOINT after_debit;
UPDATE accounts SET balance = balance + 100 WHERE id = 2;
-- Se houver problema com crédito, podemos desfazer apenas o crédito
IF (something_wrong) THEN
    ROLLBACK TO after_debit;
    -- Tentar novamente ou tomar ação alternativa
END IF;
COMMIT;
```

**Quando usar**:
- Transações complexas com múltiplas etapas onde falha em etapas posteriores não deve desfazer trabalho já feito
- Quando é possível recuperar de falhas parciais sem abortar toda operação
- Em procedimentos armazenados complexos com lógica de recuperação

## Transações Distribuídas (Visão Geral - Detalhe na PARTE 22)

Transações que abrangem múltiplos recursos (bancos de dados, serviços, sistemas) requerem protocolos especiais para garantir atomicidade entre os recursos.

**Desafios**:
- Nenhum recurso único tem controle total sobre todos os participantes
- Falhas de comunicação podem ocorrer durante o processo de commit
- Coordenação introduz latency e complexidade

**Protocolos comuns** (cobertos em detalhe na PARTE 22):
- **Two-Phase Commit (2PC)**: Coordinador solicita prepare a todos, então commit ou abort
- **Three-Phase Commit (3PC)**: Adiciona fase de pré-commit para reduzir bloqueios
- **Saga**: Sequência de transações locais com compensações para rollback
- **Eventuali Consistency**: Aceita inconsistência temporária em favor de disponibilidade

## Impacto em Diferentes Aspectos

### Performance
- **Overhead de transações**: Log de transações, aquisição/liberação de bloqueios, gerenciamento de estado
- **Taxa de commit**: Transações curtas e frequentes têm overhead relativo maior
- **Nível de isolamento**: Níveis mais altos (SERIALIZABLE) reduzem concorrência e throughput
- **Contention**: Alta concorrência em mesmos recursos aumenta espera e possibilidade de deadlocks
- **Tamanho de transação**: Transações longas aumentam tempo de retenção de recursos e tamanho do log

### Escalabilidade
- **Escalabilidade de escrita**: Limitada pelo I/O do log de transações e capacidade de checkpoint
- **Escalabilidade de leitura**: Melhorada por MVCC e níveis de isolamento mais baixos
- **Escalabilidade horizontal**: Transações distribuídas são particularmente desafiadoras para escalar
- **Sharding**: Transações que cruzam shards requerem protocolos distribuídos complexos

### Disponibilidade
- **Bloqueios**: Transações esperando por bloqueios podem reduzir disponibilidade efetiva
- **Recuperação**: Tempo de recuperação após falha depende do tamanho do log e quantidade de trabalho a redo/undo
- **Manutenção**: Operações como vacuum (PostgreSQL) ou rebuild de índices podem afetar disponibilidade
- **Failover**: Sistemas de replicação devem garantir consistência de transações entre primário e réplicas

### Consistência
- **Forte**: Níveis de isolamento altos e atomicidade garantem consistência forte dentro do banco
- **Eventual**: Em sistemas distribuídos, pode-se optar por consistência eventual para melhor disponibilidade/performance
- **Constraints**: Transações garantem que constraints sejam sempre respeitadas no commit

### Segurança
- **Auditabilidade**: Log de transações fornece trilha de auditoria para todas as mudanças
- **Privilégios**: Controle de acesso ainda é necessário - transações não bypassam verificações de permissão
- **Injeção**: Transações não previnem SQL injection - ainda necessário usar prepared statements
- **Isolamento de dados**: Transações não isolam dados entre usuários diferentes - ainda necessário usar segurança em nível de linha se necessário

### Custo
- **I/O**: Escrita em log de transações é custo significativo para transações de escrita
- **Armazenamento**: Log de transações requer espaço; versões MVCC requerem espaço adicional
- **Processamento**: Gerenciamento de bloqueios, validação OCC, detecção de deadlocks consomem CPU
- **Oportunidade**: Bloqueios e esperas reduzem throughput efetivo do sistema

### Observabilidade
- **Métricas**: Taxa de commit/rollback, tempo médio de transação, frequência de deadlocks, tamanho do log
- **Logs**: Log de transações pode ser analisado para auditoria e depuração
- **Traces**: Distributed tracing pode mostrar propagação de transações entre serviços
- **Alertas**: Alta taxa de deadlocks, crescimento inesperado do log, transações longas

## Erros Comuns

### 1. Transações Muito Longas
- **Problema**: Bloqueios mantidos por muito tempo, aumentando chance de deadlocks e reduzindo concorrência
- **Solução**: Manter transações o mais curtas possível; fazer trabalho fora da transação quando possível

### 2. Esquecer de Tratar Erros
- **Problema**: Aplicação não verifica resultado de operações de banco, levando a commits de transações parciais
- **Solução**: Sempre verificar retorno de operações e fazer rollback em caso de erro

### 3. Uso Incorreto de Níveis de Isolamento
- **Problema**: Usar nível muito alto (desnecessário custo de desempenho) ou muito baixo (anomalias de dados)
- **Solução**: Entender requisitos de consistência da aplicação e escolher nível adequado

### 4. Não Fechar Resources Adequadamente
- **Problema**: Conexões, statements ou result sets não fechados levando a vazamento de resources
- **Solução**: Usar try-with-resources (Java), using (C#) ou garantir fechamento em finally

### 5. Dependência de Comportamento de Nível de Isolamento Específico do Banco
- **Problema**: Código assumindo comportamento que varia entre implementações de banco
- **Solução**: Testar em alvos de produção ou usar padrões portáveis quando possível

### 6. Ignorar Estatísticas e Plano de Execução
- **Problema**: Não perceber que índices estão sendo usados incorretamente ou que consultas dentro de transações são ineficientes
- **Solução**: Monitorar performance de consultas dentro de transações como faria com consultas isoladas

### 7. Transações em Loops
- **Problema**: Executar BEGIN/COMMIT dentro de loop causando overhead excessivo
- **Solução**: Agrupar operações em transações maiores quando possível, ou usar batch processing

### 8. Não Considerar Impacto de Triggers e Constraints
- **Problema**: Gatilhos ou constraints complexos causando comportamento inesperado ou performance ruim
- **Solução**: Entender o efeito completo de todas as operações dentro da transação

## Quando NÃO Usar Transações

### 1. Operações Idempotentes e Independentes
- **Exemplo**: Inserção de logs onde cada entrada é independente
- **Alternativa**: Escrita direta sem sobrecarga de transação

### 2. Cargas de Trabalho Puramente de Leitura
- **Exemplo**: Relatórios, dashboards
- **Alternativa**: Leitura consistente através de snapshots ou níveis de isolamento adequados

### 3. Quando Consistência Forte Não é Necessária
- **Exemplo**: Contadores de visualizações, curtidas em redes sociais
- **Alternativa**: Contadores eventualmente consistentes ou aproximados

### 4. Operações de Bulk Load ou Migração
- **Exemplo**: Importação inicial de grandes volumes de dados
- **Alternativa**: Desativar logging de transações temporariamente ou usar modos de carga rápida

### 5. Quando Overhead Supera Benefício
- **Exemplo**: Sistemas onde taxa de conflitos é extremamente baixa e perda ocasional de dados é aceitável
- **Alternativa**: Operações sem transação com detecção e correção posterior

## Exemplos de Implementação

### Exemplo Simples: Transferência Bancária
```java
public void transferir(int contaOrigem, int contaDestino, double valor) throws SQLException {
    Connection conn = dataSource.getConnection();
    try {
        conn.setAutoCommit(false); // Iniciar transação manualmente
        
        // Debitar conta origem
        String debitSql = "UPDATE contas SET saldo = saldo - ? WHERE id = ?";
        try (PreparedStatement stmt = conn.prepareStatement(debitSql)) {
            stmt.setDouble(1, valor);
            stmt.setInt(2, contaOrigem);
            int rows = stmt.executeUpdate();
            if (rows == 0) {
                throw new SQLException("Conta origem não encontrada");
            }
        }
        
        // Creditar conta destino
        String creditSql = "UPDATE contas SET saldo = saldo + ? WHERE id = ?";
        try (PreparedStatement stmt = conn.prepareStatement(creditSql)) {
            stmt.setDouble(1, valor);
            stmt.setInt(2, contaDestino);
            int rows = stmt.executeUpdate();
            if (rows == 0) {
                throw new SQLException("Conta destino não encontrada");
            }
        }
        
        conn.commit(); // Confirmar transação
    } catch (SQLException e) {
        conn.rollback(); // Desfazer transação em caso de erro
        throw e;
    } finally {
        conn.setAutoCommit(true); // Restaurar modo automático
        conn.close();
    }
}
```

### Exemplo Intermediário: Uso de Savepoints
```java
public void processarPedido(int pedidoId) throws SQLException {
    Connection conn = dataSource.getConnection();
    try {
        conn.setAutoCommit(false);
        
        // Validar estoque
        String checkSql = "SELECT quantidade FROM estoque WHERE produto_id = ?";
        try (PreparedStatement stmt = conn.prepareStatement(checkSql)) {
            stmt.setInt(1, produtoId);
            try (ResultSet rs = stmt.executeQuery()) {
                if (!rs.next() || rs.getInt("quantidade") < quantidade) {
                    throw new SQLException("Estoque insuficiente");
                }
            }
        }
        
        // Reservar estoque (ponto de salvação antes desta operação crítica)
        conn.setSavepoint("estoque_reservado");
        String reserveSql = "UPDATE estoque SET quantidade = quantidade - ? WHERE produto_id = ?";
        try (PreparedStatement stmt = conn.prepareStatement(reserveSql)) {
            stmt.setInt(1, quantidade);
            stmt.setInt(2, produtoId);
            stmt.executeUpdate();
        }
        
        // Processar pagamento (pode falhar)
        boolean pagamentoOk = processarPagamento(pedidoId, valor);
        if (!pagamentoOk) {
            // Rollback apenas da reserva de estoque
            conn.rollback("estoque_reservado");
            // Tentar novamente ou notificar usuário
            return;
        }
        
        // Registrar pedido
        String insertSql = "INSERT INTO pedidos (id, produto_id, quantidade, valor, status) VALUES (?, ?, ?, ?, 'CONFIRMADO')";
        try (PreparedStatement stmt = conn.prepareStatement(insertSql)) {
            stmt.setInt(1, pedidoId);
            stmt.setInt(2, produtoId);
            stmt.setInt(3, quantidade);
            stmt.setDouble(4, valor);
            stmt.executeUpdate();
        }
        
        conn.commit();
    } catch (SQLException e) {
        conn.rollback();
        throw e;
    } finally {
        conn.setAutoCommit(true);
        conn.close();
    }
}
```

### Exemplo Avançado: Análise de Isolamento em Concurrent Access
**Cenário**: Duas transações concorrentes atualizando o mesmo saldo

**Transação A**:
```sql
BEGIN;
SELECT saldo FROM contas WHERE id = 1; -- Lê 1000
UPDATE contas SET saldo = saldo - 100 WHERE id = 1; -- Saldo = 900
COMMIT;
```

**Transação B** (executando simultaneamente):
```sql
BEGIN;
SELECT saldo FROM contas WHERE id = 1; -- Depende do nível de isolamento
UPDATE contas SET saldo = saldo + 50 WHERE id = 1; -- Saldo = ? 
COMMIT;
```

**Resultados por nível de isolamento**:
- **READ UNCOMMITTED**: B pode ler 1000 (sujo) ou 900 (se A já escreveu) → saldo final pode ser 950 ou 1050
- **READ COMMITTED**: B espera A commitar antes de ler → vê 900 → saldo final = 950
- **REPEATABLE READ**: B vê o estado no início da transação (1000) mesmo após A commitar → saldo final = 1050
- **SERIALIZABLE**: Um dos transactions será abortado para manter serializabilidade

## Como isso aparece em System Design

### Decisões sobre Transações em Entrevistas de System Design

**Quando discutir transações**:
- Sempre que houver menção a atualizações de banco de dados
- Quando discutir consistência em operações críticas
- Antes de projetar operações que modificam múltiplas entidades
- Quando estimar latência de operações de escrita
- Ao escolher entre consistência forte e eventual

**Como justificar escolhas de transação**:
1. **Criticalidade da operação**: Quão importante é que a operação seja atômica?
2. **Requisitos de consistência**: O negócio tolera inconsistência temporária?
3. **Volume e concorrência**: Quantas operações por segundo e nível de concorrência esperada?
4. **Latência alvo**: Quão rápido a operação precisa ser confirmada?
5. **Falhas toleráveis**: Qual é o impacto de uma operação parcialmente executada?

**Exemplos de discussão em entrevistas**:
- "Para operações de transferência bancária, usaremos transações ACID com nível SERIALIZABLE para garantir que dinheiro não seja criado ou destruído"
- "No sistema de pedidos, usaremos transações com savepoints para permitir rollback parcial de reservas de estoque se o pagamento falhar"
- "Para atualização de contadores de visualização, podemos optar por atualizações assíncronas eventualmente consistentes para melhor performance"

### Perguntas de Entrevista Comuns

#### Básicas
- "O que são as propriedades ACID?"
- "Explique a diferença entre commit e rollback"
- "Para que serve o isolamento em transações?"

#### Intermediárias
- "Quais são os diferentes níveis de isolamento e o que cada um previne?"
- "Como você evitaria deadlocks em um sistema de alta concorrência?"
- "Explique como MVCC funciona e seus trade-offs"

#### Avançadas
- "Como você projetaria transações para um sistema com requisitos de consistência mistas?"
- "Discuta trade-offs entre usar transações distribuídas vs padrão Saga"
- "Como você lidaria com transações em sistemas de alta taxa de escrita?"

#### Follow-ups Típicos
- "E se precisássemos escalar além da capacidade de um único banco de dados?"
- "Como você validaria que suas transações estão mantendo consistência sob carga?"
- "Qual seria sua estratégia para lidar com transações longas que causam timeouts?"
- "E se o negócio exigisse que certas operações nunca pudessem ser perdidas, mesmo em falha catastrófica?"

## Checklist de Transações

### Antes de Implementar uma Transação
- [ ] Definir claramente o que deve ser atômico (todas ou nada)
- [ ] Determinar nível de isolamento necessário baseado em fenômenos toleráveis
- [ ] Estimar tamanho e duração esperada da transação
- [ ] Verificar se operações dentro da transação são eficientes (usar índices adequados)
- [ ] Planejar tratamento de erros e estratégia de rollback
- [ ] Considerar uso de savepoints para operações complexas
- [ ] Avaliar necessidade de timeout para evitar bloqueios indefinidos

### Durante Implementação
- [ ] Manter transações o mais curtas possível
- [ ] Acessar recursos na ordem consistente para evitar deadlocks
- [ ] Usar prepared statements para prevenir SQL injection
- [ ] Verificar retorno de todas operações de banco
- [ ] Implementar logging adequado para depuração
- [ ] Considerar impacto de triggers, constraints e cascata

### Após Implementação e em Produção
- [ ] Monitorar taxa de commit/rollback e tempo médio de transação
- [ ] Rastrear frequência e causas de deadlocks
- [ ] Verificar tamanho e crescimento do log de transações
- [ ] Medir impacto de diferentes níveis de isolamento em performance
- [ ] Alertar sobre transações que excedem thresholds de tempo ou tamanho
- [ ] Revisar periódicamente se nível de isolamento ainda é apropriado

## Resumo

Transações são fundamentais para integridade de dados em sistemas de software, mas requerem cuidadosa consideração de trade-offs entre consistência, performance e complexidade:

**Princípios-chave**:
1. Use transações para garantir atomicidade quando operações devem acontecer juntas ou nenhuma
2. Escolha o nível de isolamento mais baixo que satisfaça os requisitos de consistência da aplicação
3. Mantenha transações curtas para minimizar bloqueios e reduzir chance de deadlocks
4. Sempre trate erros adequadamente com rollback quando necessário
5. Monitore métricas de transações em produção para identificar problemas de performance ou consistência
6. Considere alternativas (eventual consistency, operações idempotentes) quando consistência forte não é necessária
- [ ] Lembre-se: a transação perfeita é aquela que garante consistência necessária com mínimo overhead e máxima disponibilidade
