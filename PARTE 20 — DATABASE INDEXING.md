---
trilha: "INTERMEDIÁRIA"
---
**Navegação:** [[MOC — TRILHA INTERMEDIÁRIA]]
← [[PARTE 19 — DATABASES]] | #trilha/intermediaria | [[MOC — TRILHA INTERMEDIÁRIA]] →

---
# PARTE 20 — DATABASE INDEXING

> 🧠 **ESSENCIAL**
> Índices são estruturas de dados especializadas que melhoram significativamente a velocidade de operações de recuperação em bancos de dados, tornando-se fundamentais para o desempenho de sistemas em escala.

> 🎯 **ENTREVISTA — ALTA FREQUÊNCIA**
> Perguntas sobre tipos de índices, quando usar cada tipo, como índices afetam performance de leitura vs escrita, estratégias de indexing composto e cobertura, e como analisar planos de consulta são extremamente comuns em entrevistas de arquitetura de software.

## O que são Índices de Banco de Dados?

**Índices de banco de dados** são estruturas de dados auxiliares que permitem localizar rapidamente registros em uma tabela sem precisar percorrer toda a tabela (full table scan). Eles funcionam similarmente ao índice de um livro, apontando diretamente para a localização dos dados desejados.

Índices são criados em uma ou mais colunas de uma tabela e contêm:
- Os valores das colunas indexadas
- Ponteiros para as linhas completas da tabela onde esses valores ocorrem

## Por que existem?

À medida que bancos de dados crescem em volume de dados, consultas sem índices tornam-se impraticáveis:

- **Consultas lentas**: Sem índices, o banco precisa percorrer toda a tabela linha por linha (O(n))
- **Escalabilidade ruim**: Performance degrada linearmente com o tamanho da tabela
- **Experiência do usuário pobre**: Tempos de resposta inaceitáveis para aplicações interativas
- **Custo computacional alto**: Consumo excessivo de CPU e I/O desnecessário
- **Bloqueios prolongados**: Consultas longas podem bloquear outras operações

## Problema que resolve

Índices resolvem vários problemas críticos de performance:

1. **Busca lenta**: Reduz complexidade de busca de O(n) para O(log n) ou O(1) dependendo do tipo
2. **Escalabilidade limitada**: Permite que consultas mantenham performance estável mesmo com crescimento de dados
3. **Sobrecarga de recursos**: Reduz I/O disco e consumo de CPU necessários para consultas
4. **Latência inaceitável**: Melhora tempos de resposta de segundos para milissegundos
5. **Bloqueios excessivos**: Consultas mais rápidas liberam recursos mais rapidamente

## Como funciona internamente

Índices implementam diversas estruturas de dados otimizadas para buscas rápidas:

### Estruturas Comuns de Índices

1. **B-Tree e B+Tree**
   - Estrutura balanceada de árvore
   - Todos os nós folha no mesmo nível
   - Busca, inserção e deleção em O(log n)
   - Suporta buscas de faixa eficientemente
   - Padrão na maioria dos bancos relacionais

2. **Hash Index**
   - Tabela hash que mapeia chaves para localizações
   - Busca exata em O(1) média
   - Não suporta buscas de faixa
   - Muito rápido para equalidade, inútil para comparações

3. **Bitmap Index**
   - Usa bits para representar presença/ausência de valores
   - Eficiente para colunas com baixa cardinalidade
   - Operações bit a bit são muito rápidas
   - Comum em data warehouses

4. **GiST, GIN, BRIN** (PostgreSQL)
   - Estruturas especializadas para tipos de dados específicos
   - GiST: Generalized Search Tree
   - GIN: Generalized Inverted Index
   - BRIN: Block Range INdex

### Componentes de um Índice

- **Root Node**: Nó inicial da árvore
- **Internal Nodes**: Nós intermediários que direcionam a busca
- **Leaf Nodes**: Nós finais que contêm os valores indexados e ponteiros (ROWIDs ou TIDs)
- **Pointers**: Referências para as linhas completas na tabela heap

## Tipos de Índices

### 1. B-Tree Index (Padrão)

**Como funciona**:
- Estrutura de árvore balanceada onde cada nó contém múltiplas chaves
- Chaves mantêm ordenação lexicográfica
- Busca percorre da raiz até o folha comparando chaves em cada nível

**Vantagens**:
- Suporta igualdade, desigualdade, buscas de faixa, ORDER BY
- Performance previsível O(log n)
- Funciona bem com alta cardinalidade
- Suporta índices únicos

**Desvantagens**:
- Overhead de manutenção em escritas frequentes
- Espaço adicional em disco
- Não ideal para baixa cardinalidade

**Quando usar**:
- Colunas frequentemente usadas em WHERE, JOIN, ORDER BY
- Chaves primárias e estrangeiras
- Colunas com alta cardinalidade
- Consultas que envolvem ranges (> , < , BETWEEN)

### 2. Hash Index

**Como funciona**:
- Função hash converte chave em bucket
- Colisões tratadas com encadeamento ou sondagem
- Busca direta para o bucket calculado

**Vantagens**:
- Busca exata extremamente rápida O(1)
- Ideal para consultas de igualdade pura
- Menor overhead em comparação com B-Tree para equalidade

**Desvantagens**:
- Não suporta buscas de faixa ou ordenação
- Performance degrada com muitas colisões
- Rehashing caro quando tabela cresce
- Útil apenas para operadores de igualdade

**Quando usar**:
- Consultas exclusivamente de igualdade (=)
- Chaves de busca em caches ou tabelas de lookup
- Situações onde apenas igualdade é necessária
- Colunas com distribuição uniforme de valores

### 3. Índice Composto (Composite Index)

**Como funciona**:
- Índice criado em múltiplas colunas na ordem especificada
- Estrutura B-Tree onde chave é tupla (col1, col2, col3, ...)
- Ordenação lexicográfica pela ordem das colunas

**Vantagens**:
- Pode atender múltiplas condições de filtro com um único índice
- Elimina necessidade de múltiplos índices separados
- Mais eficiente que índices separados para consultas compostas

**Desvantagens**:
- Ordem das colunas é crítica para efetividade
- Menos flexível para consultas que não usam prefixo
- Maior tamanho de índice

**Quando usar**:
- Consultas que filtram em múltiplas colunas consistentemente
- Quando a ordem de filtragem segue padrões previsíveis
- Para cobrir consultas específicas frequentes

### 4. Índice Cobertura (Covering Index)

**Como funciona**:
- Índice que contém todas as colunas necessárias para satisfazer uma consulta
- Também chamado de "índice que cobre a query"
- Evita necessidade de acessar a tabela heap (lookup adicional)

**Vantagens**:
- Elimina lookups na tabela principal
- Reduz I/O significativamente
- Pode transformar consulta lenta em extremamente rápida

**Desvantagens**:
- Tamanho maior do índice (armazena mais dados)
- Overhead maior em escritas
- Só útil para consultas específicas

**Quando usar**:
- Consultas frequentes e críticas de performance
- Quando o conjunto de colunas SELECT é pequeno e estável
- Para melhorar dramatically consultas específicas problemáticas

### 5. Índice Clustered vs Non-Clustered

**Clustered Index**:
- Determina a ordem física de armazenamento das linhas na tabela
- Só pode haver um índice clustered por tabela
- As linhas são armazenadas na ordem das chaves do índice
- Leitura sequencial eficiente quando acessado pelo índice clustered

**Non-Clustered Index**:
- Estrutura separada da tabela
- Ponteiros apontam para localizações na tabela (heap ou clustered)
- Pode haver múltiplos índices non-clustered por tabela
- Requer lookup adicional para obter dados completos

**Trade-offs**:
- Clustered: Melhor para ranges e scans sequenciais, pior para inserções aleatórias
- Non-Clustered: Mais flexível, permite múltiplos índices, overhead de lookup

## Seletividade de Índices (Index Selectivity)

**Seletividade** = Número de valores distintos / Número total de linhas

- Alta seletividade (próxima de 1): Poucas duplicatas, bom para índices
- Baixa seletividade (próxima de 0): Muitas duplicatas, menos eficaz para índices

**Exemplos**:
- Chave primária: Seletividade = 1.0 (ideal)
- Coluna de gênero (M/F): Seletividade ≈ 0.001 (péssima para índice isolado)
- Timestamp com alta granularidade: Seletividade alta
- Status de pedido (pendente, processado, entregue): Seletividade baixa

**Impacto na performance**:
- Índices com baixa seletividade podem ser ignorados pelo otimizador
- Índices compostos devem colocar colunas mais seletivas primeiro
- Estratégias como índices filtrados podem melhorar seletividade efetiva

## Planos de Consulta (Query Plans)

**Como ler um plano de consulta**:
1. **Operação**: Tipo de acesso (Index Scan, Seq Scan, Hash Join, etc.)
2. **Custo**: Estimativa de recursos necessários (inicial e total)
3. **Linhas**: Número estimado de linhas retornadas
4. **Largura**: Tamanho médio das linhas em bytes
5. **Filtros**: Condições aplicadas em cada etapa

**Elementos importantes em planos**:
- **Index Scan**: Usando índice para acessar dados
- **Bitmap Index Scan**: Combina múltiplos índices com operações bitmap
- **Index Only Scan**: Obtém todos os dados do índice (cobertura)
- **Seq Scan**: Varredura sequencial completa da tabela (geralmente ruim)
- **Nested Loop**: Join iterativo (bom para conjuntos pequenos)
- **Hash Join**: Join usando tabela hash (bom para médios/grandes)
- **Merge Join**: Join usando ordenação (bom quando já ordenado)

**Como obter planos**:
- PostgreSQL: `EXPLAIN ANALYZE SELECT ...`
- MySQL: `EXPLAIN FORMAT=TREE SELECT ...`
- SQL Server: `SHOWPLAN_TEXT` ou interface gráfica
- Oracle: `EXPLAIN PLAN FOR`

## Estratégias de Indexação

### 1. Indexação de Chave Primária
- Automática em maioria dos bancos
- Sempre clustered em SQL Server, opcional em outros
- Essencial para integridade referencial e JOINs eficientes

### 2. Indexação de Chaves Estrangeiras
- Critical para performance de JOINs
- Muitas vezes esquecida, causando full table scans
- Deve ser considerada em todo relacionamento FK

### 3. Indexação para WHERE e JOIN
- Colunas em cláusulas WHERE de igualdade, range, IN
- Colunas usadas em condições JOIN
- Priorizar igualdade antes de ranges em índices compostos

### 4. Indexação para ORDER BY e GROUP BY
- Evita operações de ordenação dispendiosas
- Índices podem fornecer ordenação prévia
- Combinar com WHERE quando possível

### 5. Indexação Cobrindo
- Incluir colunas SELECT no índice para evitar lookups
- Especialmente útil para consultas frequentes e críticas
- Avaliar trade-off tamanho do índice vs performance ganha

### 6. Indexação Parcial/Filtrada
- Índice que cobre apenas subseto de linhas (WHERE condição)
- Útil quando apenas subseto é consultado frequentemente
- Menor tamanho e melhor performance para casos específicos

### 7. Indexação Expressão/Funcional
- Índice baseado em expressão ou função (UPPER(col), col+1, etc.)
- Permite indexar resultados de transformações
- Necessário quando consulta usa funções na coluna

## Impacto em Diferentes Aspectos

### Performance
- **Leitura**: Melhoria dramática (10x-1000x+ common)
- **Escrita**: Overhead negativo (INSERT/UPDATE/DELETE mais lento)
- **Trade-off**: Mais índices = leituras mais rápidas, escritas mais lentas

### Escalabilidade
- Permite escalar leituras com crescimento de dados
- Sem índices, performance degrada linearmente com tamanho da tabela
- Estratégia de índice correta é essencial para escalabilidade horizontal

### Disponibilidade
- Índices não afetam diretamente disponibilidade
- Porém, consultas lentas podem consumir recursos e causar timeouts
- Manutenção de índices pode requerer bloqueios curtos

### Consistência
- Índices devem ser mantidos transactionalmente consistentes com dados
- Overhead mínimo em transações ACID
- Índices corrompidos podem causar resultados incorretos ou erros

### Segurança
- Índices em si não adicionam riscos de segurança
- Porém, podem expor padrões de acesso se analisados
- Criptografia de colunas pode impedir uso de alguns tipos de índice

### Custo
- **Custo de armazenamento**: Espaço adicional em disco (10%-50%+ típico)
- **Custo de performance**: Overhead em operações de escrita
- **Custo de complexidade**: Mais índices = mais complexidade de manutenção
- **ROI**: Geralmente positivo para índices bem escolhidos

### Observabilidade
- Estatísticas de uso de índice (scans, hits, misses)
- Monitoramento de fragmentação e rebuild necessário
- Alertas para índices não utilizados ou duplicados

## Erros Comuns

### 1. Over-Indexing
- Criar muitos índices "por via das dúvidas"
- Resultado: escritas lentas, espaço desperdiçado, manutenção complexa
- **Solução**: Monitorar uso real, remover índices não utilizados

### 2. Under-Indexing
- Esquecer índices críticos em colunas de busca frequente
- Resultado: consultas lentas, full table scans
- **Solução**: Analisar logs de query lenta, usar EXPLAIN regularmente

### 3. Índices na Ordem Errada
- Índice composto com colunas na ordem subótima
- Resultado: índice não utilizado apesar de existir
- **Solução**: Entender padrões de consulta, colocar colunas mais seletivas primeiro

### 4. Ignorar Seletividade
- Indexar colunas com baixa cardinalidade (status, gênero, etc.)
- Resultado: otimizador ignora o índice ou faz scans ineficientes
- **Solução**: Considerar índices compostos, filtrados ou outras abordagens

### 5. Não Atualizar Estatísticas
- Otimizador toma decisões baseadas em estatísticas desatualizadas
- Resultado: escolha de plano subótima mesmo com bons índices
- **Solução**: Agendar atualização automática de estatísticas

### 6. Indexar Colunas com Alta Atualização
- Índices em colunas que mudam muito frequentemente
- Resultado: alto custo de manutenção, fragmentação
- **Solução**: Avaliar se benefício de leitura justifica custo de escrita

### 7. Esquecer NULLs
- Comportamento de NULLs varia entre bancos em índices
- Resultado: consultas esperadas não usando índice como esperado
- **Solução**: Entender tratamento de NULLs específico do banco

## Quando NÃO Usar Índices

### 1. Tabelas Muito Pequenas
- Menos de algumas centenas de linhas
- Seq scan pode ser mais rápido que índice lookup
- Overhead do índice supera benefício

### 2. Colunas com Baixa Seletividade
- Poucos valores distintos repetidos muitas vezes
- Bitmap index pode ser melhor alternativa em data warehouses
- Considere particionamento em vez de índice

### 3. Escritas Extremamente Frequentes, Leituras Raras
- Log de auditoria, tabelas de evento append-only
- Overhead de índice em cada escrita supera benefício raro de leitura
- Considere arquivamento ou tabelas separadas

### 4. Colunas com Valores Muito Grandes
- TEXT, BLOB, JSON sem funções específicas
- Índices em colunas grandes são ineficientes
- Use índices em expressões ou colunas derivadas quando possível

### 5. Quando o Otimizador Escolhe Não Usar
- Estatísticas indicam que seq scan é mais eficiente
- Confie no otimizador, mas verifique com EXPLAIN
- Pode indicar necessidade de atualizar estatísticas ou rever consulta

## Alternativas aos Índices Tradicionais

### 1. Partições (Partitioning)
- Dividir tabela em partes físicas menores
- Pode eliminar necessidade de alguns índices
- Útil quando acesso segue padrões temporais ou categóricos

### 2. Materilized Views
- Pré-computar resultados de consultas complexas
- Pode ser mais eficiente que múltiplos índices para agregações
- Trade-off: dados não em tempo real, custo de manutenção

### 3. Caching
- Armazenar resultados frequentes em camada mais rápida (Redis, Memcached)
- Reduz carga no banco de dados
- Trade-off: consistência eventual, complexidade de invalidação

### 4. NoSQL com Modelos Específicos
- Algumas consultas podem ser melhor atendidas por modelos NoSQL
- Ex: grafos para relacionamentos complexos, colunarwide para analytics
- Trade-off: perda de funcionalidades relacional, consistência

### 5. Otimização de Consulta
- Reescrever consulta para ser mais eficiente
- Adicionar dicas ou hints quando necessário
- Revisar junções, subconsultas, funções no WHERE

## Exemplos de Consultas

### Exemplo Simples: Busca por ID
```sql
-- Sem índice: Seq Scan na tabela users
SELECT * FROM users WHERE id = 12345;

-- Com índice clustered em id: Index Scan usando índice primário
-- Performance: O(log n) vs O(n)
```

### Exemplo Intermediário: Filtro Composto
```sql
-- Consulta típica de e-commerce
SELECT * FROM orders 
WHERE customer_id = 1001 
  AND order_date >= '2026-01-01'
  AND status = 'shipped'
ORDER BY order_date DESC;

-- Índice ideal: (customer_id, order_date DESC, status)
-- Ou: (customer_id, order_date) incluindo status para cobertura
```

### Exemplo Avançado: Análise de Plano de Consulta
```sql
-- Consulta analítica
SELECT 
  c.category_name,
  SUM(oi.quantity * oi.price) as revenue
FROM orders o
JOIN order_items oi ON o.id = oi.order_id
JOIN products p ON oi.product_id = p.id
JOIN categories c ON p.category_id = c.id
WHERE o.order_date BETWEEN '2026-01-01' AND '2026-12-31'
GROUP BY c.category_name
ORDER BY revenue DESC;

-- Índices recomendados:
-- 1. orders(order_date) -- para filtro de data
-- 2. order_items(order_id) -- para JOIN
-- 3. products(id, category_id) -- cobrindo JOIN e filtro
-- 4. categories(id, category_name) -- cobrindo JOIN e GROUP BY
```

### Exemplo de Entrevista: Otimização de Consulta Lenta
**Pergunta**: "Você tem uma consulta que está levando 10 segundos para retornar resultados de uma tabela com 10 milhões de registros. Como você abordaria o problema?"

**Resposta esperada**:
1. **Reproduzir e medir**: Obter plano de consulta atual com EXPLAIN ANALYZE
2. **Identificar gargalo**: Verificar se está fazendo Seq Scan, qual etapa é mais cara
3. **Analisar WHERE e JOIN**: Colunas usadas em filtros e junções
4. **Propor índices**: Baseado nos padrões de acesso encontrados
5. **Considerar cobertura**: Se consulta retorna poucas colunas, índice cobrindo pode ajudar
6. **Testar e validar**: Criar índices propostos, medir melhoria
7. **Monitorar side effects**: Ver impacto em escritas e manutenção
8. **Iterar**: Ajustar com base em resultados reais

## Como isso aparece em System Design

### Decisões de Indexamento em Entrevistas de System Design

**Quando discutir índices**:
- Sempre que houver menção a consultas em banco de dados
- Quando discutir performance de leitura em camada de persistência
- Antes de otimizar consultas críticas no caminho crítico
- Quando estimar latência de operações de banco

**Como justificar escolhas de índice**:
1. **Padrões de acesso**: Quais consultas serão frequentes e críticas
2. **Trade-off leitura/escrita**: Sistema é read-heavy ou write-heavy?
3. **Volume de dados**: Quão grande a tabela vai ficar?
4. **Latência alvo**: Quão rápido as consultas precisam ser?
5. **Recursos disponíveis**: Espaço em disco, capacidade de escrita

**Exemplos de discussão em entrevistas**:
- "Para o feed de notícias, vamos precisar de índices compostos em (user_id, created_at) para buscar posts recentes de um usuário"
- "Na tabela de pedidos, um índice em status + created_at permitirá consultas eficientes de pedidos recentes por status"
- "Como esperamos apenas 1000 usuários ativos simultâneos, podemos começar sem índices complexos e adicionar conforme necessário"

### Perguntas de Entrevista Comuns

#### Básicas
- "Qual a diferença entre índice clustered e non-clustered?"
- "Como um índice melhora performance de consulta?"
- "Quando você NÃO criaria um índice em uma coluna?"

#### Intermediárias
- "Explique seletividade de índice e por que ela importa"
- "Qual seria sua estratégia de indexação para esta consulta específica?"
- "Como você identificaria se um índice está sendo usado ou não?"

#### Avançadas
- "Como você projetaria índices para uma tabela com padrões de acesso mistos?"
- "Discuta trade-offs entre vários índices simples vs poucos índices compostos"
- "Como você lidaria com indexação em colunas de alta atualização?"

#### Follow-ups Típicos
- "E se a distribuição de dados mudar com o tempo?"
- "Como você validaria que seu índice realmente melhorou performance?"
- "Qual seria seu plano de manutenção desses índices em produção?"
- "E se precisássemos suportar tanto consultas de igualdade quanto de faixa?"

## Checklist de Indexação

### Antes de Criar um Índice
- [ ] Analisar padrões de consulta reais (logs, slow query log)
- [ ] Verificar se coluna tem seletividade suficiente
- [ ] Considerar ordem ótima para índices compostos
- [ ] Avaliar se índice cobrindo seria benéfico
- [ ] Estimar tamanho adicional e overhead de escrita
- [ ] Verificar se índice semelhante já existe

### Após Criar um Índice
- [ ] Verificar se índice está sendo usado (EXPLAIN, estatísticas)
- [ ] Medir melhoria de performance em consultas alvo
- [ ] Monitorar impacto em operações de escrita
- [ ] Verificar necessidade de atualizar estatísticas
- [ ] Documentar motivo e expectativas do índice

### Manutenção Contínua
- [ ] Revisar uso de índices periodicamente (mensal/trimestral)
- [ ] Remover índices não utilizados ou duplicados
- [ ] Rebuild/reorganizar índices fragmentados
- [ ] Atualizar estatísticas conforme agendamento
- [ ] Ajustar estratégia com base em mudança de padrões de acesso

## Resumo

Índices são ferramentas essenciais para performance de bancos de dados, mas requerem cuidadosa consideração de trade-offs:

**Princípios-chave**:
1. Indexar com base em padrões de acesso reais, não em teoria
2. Balancear benefícios de leitura contra custos de escrita
3. Considerar seletividade, ordem e cobertura em projetos de índice
4. Monitorar uso e manutenção continuamente
- [ ] Sempre validar impacto com EXPLAIN e métricas reais
- [ ] Lembre-se: o melhor índice é aquele que é realmente usado e melhora performance