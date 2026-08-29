---
trilha: "INICIANTE"
---
**Navegação:** [[MOC — TRILHA INICIANTE]]
← [[PARTE 1 — FUNDAMENTOS DE ARQUITETURA DE SOFTWARE]] | #trilha/iniciante | [[PARTE 9 — SOLID]] →

---
# PARTE 2 — REQUISITOS E DECISOES ARQUITETURAIS

## Functional Requirements

> 🧠 **ESSENCIAL**
> 
> Requisitos funcionais descrevem o que o sistema deve fazer - suas características, funções e comportamentos específicos que atendem diretamente às necessidades de negócio.

### definição
Requisitos funcionais são declarações claras e verificáveis de serviços que o sistema deve fornecer, como ele deve se comportar diante de determinados inputs e quais outputs deve produzir. Eles descrevem as funções específicas que o sistema deve realizar para atender aos objetivos de negócio.

### características
- **Verificáveis**: Podem ser testados e validados
- **Claros**: Descrevem comportamento específico sem ambiguidade
- **Completos**: Cobrem todas as funções necessárias do sistema
- **Consistentes**: Não se contradizem entre si
- **Traçáveis**: Podem ser ligados a objetivos de negócio e stakeholders específicos

### exemplos
- "O sistema permitirá que usuários criem contas com email e senha"
- "O sistema calculará o frete baseado no peso, dimensões e destino do pacote"
- "O sistema gerará relatórios de vendas mensais em formato PDF"
- "O sistema enviará notificações por push quando houver atualizações importantes"
- "O sistema validará CPF/CNPJ conforme regras da Receita Federal"

### como coletar
- Entrevistas com stakeholders
- Workshops de requisitos
- Análise de documentos existentes (lei, regulamentos, manuais)
- Observação de processos atuais
- Protótipos e maquettes
- User stories e casos de uso

### desafios comuns
- Requisitos vagos ou ambíguos ("o sistema deve ser rápido")
- Requisitos conflitantes entre diferentes stakeholders
- Requisitos impossíveis de implementar com tecnologia disponível
- Falta de envolvimento do usuário final no processo
- Mudanças frequentes de requisitos durante desenvolvimento

## Non-Functional Requirements

> 🎯 **ENTREVISTA — ALTA FREQUÊNCIA**
> 
> Requisitos não-funcionais são cruciais em entrevistas porque revelam como você pensa sobre qualidade, trade-offs e restrições reais do sistema.

### definição
Requisitos não-funcionais (também chamados de quality attributes ou constraints) especificam critérios que julham a operação de um sistema, ao invés de seus comportamentos específicos. Eles descrevem como o sistema deve ser, ao invés de o que o sistema deve fazer.

### categorias principais

#### 1. Qualidade de produto (Product Qualities)
- **Performance**: Tempo de resposta, throughput, utilização de recursos
- **Segurança**: Confidencialidade, integridade, disponibilidade, autenticação, autorização
- **Disponibilidade**: Uptime, MTBF, MTTR, failover
- **Escalabilidade**: Capacidade de lidar com aumento de carga
- **Manutenibilidade**: Facilidade de correção, adaptação e aprimoramento
- **Portabilidade**: Facilidade de transferência entre ambientes
- **Reusabilidade**: Extent to which software can be reused
- **Interoperabilidade**: Capacidade de trocar informações com outros sistemas
- **Corretude**: Conformidade com especificações e regras de negócio
- **Fiabilidade**: Probabilidade de operação sem falha
- **Segurança**: Proteção contra acessos não autorizados e danos
- **Precisão**: Exatidão e liberdade de erros
- **Robustez**: Capacidade de lidar com condições adversas
- **Integridade**: Prevenção de acesso não autorizado a dados

#### 2. Qualidade de desenvolvimento (Development Qualities)
- **Testabilidade**: Facilidade de criação de testes eficazes
- **Entendibilidade**: Clareza e facilidade de compreensão do código
- **Modificabilidade**: Facilidade de fazer mudanças sem introduzir defeitos
- **Estabilidade**: Resistência a mudanças causando efeitos colaterais indesejados
- **Trabalho em equipe**: Facilidade de múltiplos desenvolvedores trabalharem simultaneamente

#### 3. Qualidade de produto em evolução (Product Evolution Qualities)
- **Flexibilidade**: Facilidade de mudar para atender novos requisitos
- **Escalabilidade**: Capacidade de aumentar funcionalidade ou capacidade
- **Reusabilidade**: Extent to which software can be reused in outros sistemas
- **Interoperabilidade**: Capacidade de trabalhar com outros sistemas
- **Portabilidade**: Facilidade de executar em diferentes ambientes

### exemplos
- "O sistema deve responder a 95% das requisições em menos de 2 segundos"
- "O sistema deve estar disponível 99.9% do tempo mensalmente"
- "O sistema deve suportar até 10.000 usuários simultâneos"
- "Todos os dados pessoais devem ser criptografados em repouso usando AES-256"
- "O sistema deve ser capaz de recuperar de um desastre em menos de 4 horas"
- "O código deve ter cobertura de teste unitário mínima de 80%"
- "O sistema deve ser compatível com navegadores Chrome, Firefox, Safari e Edge versões recentes"
- "O sistema deve processar pagamentos compatível com PCI-DSS nível 1"

### como documentar
Usar o formato: "O sistema shall [verbo] [objeto] [condição]" ou "O sistema deve [verbo] [objeto] [condição]"
Exemplos:
- "O sistema shall responder a requisições de busca em menos de 1 segundo para 90% dos casos"
- "O sistema deve manter logs de auditoria por mínimo de 7 anos"
- "O sistema shall criptografar todas as senhas usando bcrypt com fator de custo 12"

### desafios comuns
- Confundir requisitos não-funcionais com objetivos ou metas vagas
- Especificar requisitos impossíveis ou extremamente caros de atender
- Especificar requisitos sobrepostos ou conflitantes
- Falta de mensurabilidade ("o sistema deve ser fácil de usar")
- Especificar soluções em vez de requisitos ("o sistema deve usar MongoDB")

## Constraints

> 💡 **DICA DE ENTREVISTA**
> 
> Restrições são frequentemente subestimadas em entrevistas, mas são críticas para demonstrar pensamento prático e consciência de limites reais.

### definição
Restrições são limitações impostas ao sistema ou ao seu desenvolvimento que limitam as opções de solução disponíveis. Elas definem o que não pode ser feito ou limita as escolhas de tecnologia, prazo, recursos, etc.

### tipos de restrições

#### 1. Restrições de negócio
- **Orçamento**: Limite financeiro para desenvolvimento e operação
- **Prazo**: Data limite para entrega ou marcos intermediários
- **Recursos humanos**: Número e qualificação de desenvolvedores disponíveis
- **Recursos tecnológicos**: Infraestrutura existente que deve ser utilizada
- **Políticas corporativas**: Padrões de tecnologia aprovados pela empresa
- **Contratos com terceiros**: Limitações impostas por fornecedores ou parceiros

#### 2. Restrições técnicas
- **Tecnologia obrigatória**: Linguagens, frameworks ou plataformas que devem ser usadas
- **Tecnologia proibida**: Linguagens, frameworks ou plataformas que não podem ser usadas
- **Integração com sistemas legados**: Necessidade de se comunicar com sistemas existentes
- **Padrões da indústria**: Necessidade de cumprir padrões regulatórios ou de mercado
- **Limitações de hardware**: Capacidade física dos equipamentos disponíveis
- **Limitações de software**: Versões mínimas/máximas de softwares necessários

#### 3. Restrições legais e regulatórias
- **Lei de proteção de dados** (LGPD, GDPR, CCPA)
- **Regulamentações setoriais** (bancária, saúde, telecomunicações)
- **Direitos autorais e propriedade intelectual**
- **Leis de acessibilidade** (WCAG, lei brasileira de inclusão)
- **Normas de segurança** (ISO 27001, NIST, PCI-DSS)

#### 4. Restrições operacionais
- **Horários de operação**: Períodos em que manutenção pode ser realizada
- **Janela de deploy**: Períodos permitidos para releases em produção
- **Capacidade de suporte**: Equipe disponível para operação e manutenção
- **Procedimentos de mudança**: Processos aprovados para alterações em produção
- **Requisitos de documentação**: Nível de documentação exigido para operação

### exemplos
- "O sistema deve ser desenvolvido usando Java 17 e Spring Boot 3 devido a política corporativa"
- "O prazo máximo para entrega é 6 meses devido a contrato com cliente"
- "O orçamento máximo para infraestrutura é R$ 5.000,00 por mês"
- "O sistema deve ser compatível com navegadores IE11 devido a requisitos de clientes corporativos"
- "Todos os dados devem permanecer dentro do território nacional devido a leis de soberania de dados"
- "O sistema deve integrar com o sistema legado de folha de pagamento usando apenas arquivos CSV"
- "A equipe de desenvolvimento é limitada a 2 desenvolvedores backend e 1 frontend"
- "O sistema deve operar em servidores com no máximo 4GB de RAM devido a limitações de hospedagem atual"

### como identificar
- Perguntar explícitamente: "Quais são as limitações que temos para este projeto?"
- Revisar contratos, políticas corporativas e acordos de nível de serviço
- Analisar sistemas existentes que precisam ser integrados
- Consultar equipes de operações, segurança e compliance
- Revisar regulamentações aplicáveis ao domínio do sistema

### impactos na arquitetura
- Limitações de tecnologia podem impedir uso de certos padrões arquiteturais
- Restrições de prazo podem favorecer soluções mais simples ou uso de plataformas prontas
- Orçamento limitado pode impedir soluções de alta disponibilidade ou escalabilidade avançada
- Restrições de integração podem forçar acoplamento indesejado com sistemas legados
- Limitações de equipe podem impedir adoção de tecnologias com curva de aprendizado íngreme

## Assumptions

> 🎯 **ENTREVISTA — MODERADO**
> 
> Suposições são importantes em entrevistas porque mostram sua capacidade de identificar incertezas e gerenciar riscos através de validação e mitigação.

### definição
Suposições são fatores que são considerados verdadeiros, reais ou certos sem prova ou demonstração, e que são necessários para o planejamento e estimativa do projeto. Elas representam crenças sobre o ambiente, tecnologia, comportamento dos usuários ou outros fatores que afetam o projeto.

### características
- **Não verificadas**: Ainda não foram validadas por evidências
- **Necessárias para planejamento**: Sem elas, não seria possível fazer estimativas ou tomar decisões
- **Arriscadas**: Se estiverem incorretas, podem impactar significativamente o projeto
- **Documentáveis**: Devem ser registradas para revisão futura
- **Revisáveis**: Devem ser reavaliadas periodicamente conforme novo informação fica disponível

### tipos comuns de suposições

#### 1. Sobre o ambiente
- "A infraestrutura de nuvem estará disponível com 99.9% de uptime"
- "A largura de banda da rede será suficiente para o tráfego esperado"
- "Os servidores de banco de dados terão desempenho adequado para a carga esperada"

#### 2. Sobre a tecnologia
- "A versão X da biblioteca Y será compatível com nossos requisitos"
- "O framework Z terá suporte adequado durante o ciclo de vida do projeto"
- "As APIs de terceiros que pretendemos usar estarão disponíveis e estáveis"

#### 3. Sobre os usuários
- "Os usuários terão acesso a internet banda larga"
- "Os usuários estarão dispostos a pagar pelo serviço"
- "Os usuários entenderão como usar a interface sem treinamento extensivo"
- "A taxa de adoção será X% dentro dos primeiros 6 meses"

#### 4. Sobre o mercado
- "Não haverá mudanças regulatórias significativas durante o período do projeto"
- "Os concorrentes não lançarão recursos similares que afetem nossa proposta de valor"
- "Os preços dos insumos permanecerão estáveis"

#### 5. Sobre a equipe
- "A equipe terá as habilidades necessárias para trabalhar com as tecnologias escolhidas"
- "Não haverá rotatividade significativa da equipe durante o projeto"
- "O nível de comprometimento da equipe será adequado para cumprir os prazos"

### como documentar
Formato: "Supomos que [afirmação]" ou "Assume-se que [afirmação]"
Exemplos:
- "Supomos que o serviço de email externo terá latência média de menos de 200ms"
- "Assume-se que 70% dos usuários acessarão o sistema principalmente através de dispositivos móveis"
- "Supomos que o provedor de pagamento escolhido manterá suas taxas estáveis pelos próximos 12 meses"
- "Assume-se que a equipe de desenvolvimento estará familiarizada com React após 2 semanas de treinamento"

### como validar
- **Prototipagem**: Construir provas de conceito para testar suposições técnicas
- **Pesquisa de mercado**: Validar suposições sobre usuários e mercado através de pesquisas
- **Provas de conceito com tecnologia**: Testar integrações e desempenho antes do compromisso total
- **Consultoria com especialistas**: Obter opiniões de especialistas em domínio ou tecnologia
- **Análise de dados históricos**: Usar dados de sistemas similares ou anteriores
- **Experimentação controlada**: Testar hipóteses em ambiente controlado antes de implementação total

### estratégias de mitigação
- **Planos de contingência**: Definir ações alternativas caso a suposição se prove falsa
- **Abordagens incremental**: Implementar em fases para validar suposições cedo
- **Monitoramento contínuo**: Acompanhar indicadores que validam ou invalida a suposição
- **Design flexível**: Arquitetura que permite mudanças caso suposições falhem
- **Contractos com cláusulas de saída**: Acordos com fornecedores que permitem mudança se necessário
- **Buffer de tempo e recurso**: Reservar margem para lidar com impacto de suposições incorretas

### exemplos de impactos quando incorretas
- Suposição de desempenho de banco de dados incorreta → sistema lento, necessidade de rearquitetura
- Suposição de disponibilidade de serviço de terceiros incorreta → falhas em cascata, necessidade de circuit breaker ou fallback
- Suposição de habilidade da equipe incorreta → atrasos, qualidade inferior, necessidade de treinamento ou contratação
- Suposição de comportamento do usuário incorreta → baixa adoção, necessidade de redesign significativo

## Business Requirements

> 💡 **DICA DE ENTREVISTA**
> 
> Requisitos de negócio conectam tecnologia com valor real e são essenciais para demonstrar pensamento de arquiteto que entende o contexto maior.

### definição
Requisitos de negócio são declarações de objetivos, metas ou necessidades da organização que o sistema deve atender. Eles descrevem o "por quê" por trás do projeto, focando nos resultados de negócio que se espera alcançar, piuttosto que nas especificações técnicas de como alcançá-los.

### características
- **Orientados para resultados**: Focam em o que se espera alcançar, não como
- **Alinhados com estratégia**: Devem apoiar os objetivos estratégicos da organização
- **Mensuráveis**: Devem poder ser quantificados para avaliar sucesso
- **Prioritários**: Alguns são mais críticos que outros para o sucesso do negócio
- **Valiosos**: Devem trazer benefício tangível para a organização
- **Estáveis**: Tendem a mudar menos frequentemente que requisitos funcionais

### exemplos
- "Aumentar a retenção de clientes em 15% dentro de 12 meses após o lançamento"
- "Reduzir o custo de processamento de pedidos em 30% através da automação"
- "Expandir para novos mercados geográficos através de suporte a múltiplos idiomas e moedas"
- "Melhorar a satisfação do cliente medida pelo NPS de 65 para 80 pontos"
- "Aumentar a taxa de conversão de visitas para vendas de 2% para 3,5%"
- "Reduzir o tempo médio de atendimento ao cliente de 10 minutos para 3 minutos"
- "Cumprir com regulamentação X para evitar multas e sanções legais"
- "Posicionar a empresa como líder em inovação no setor Y"

### como coletar
- Entrevistas com executivos e patrocinadores do projeto
- Análise de documentos estratégicos (plano de negócio, visão, missão)
- Workshops de alinhamento com liderança
- Análise de indicadores-chave de performance (KPIs) atuais
- Estudos de mercado e análise de concorrência
- Feedback de clientes e usuários atuais
- Análise de tendências do setor e tecnológicas

### relação com outros tipos de requisitos
- **Business Requirements → Functional Requirements**: O "por quê" leva ao "o quê"
- **Business Requirements → Non-Functional Requirements**: Objetivos de negócio podem gerar restrições de qualidade (ex: necessidade de alta disponibilidade para manter receita)
- **Business Requirements → Constraints**: Objetivos de negócio podem gerar restrições (ex: orçamento limitado baseado em ROI esperado)
- **Business Requirements → Assumptions**: Metas de negócio geram suposições (ex: supomos que mercado vai crescer X%)

### exemplos de má formulação
- Muito vagos: "Melhorar o sistema" (como? em quê? quanto?)
- Muito técnicos: "Implementar microserviços usando Docker e Kubernetes" (isso é solução, não requisito de negócio)
- Não mensuráveis: "Tornar o sistema mais amigável" (como medir amigabilidade?)
- Não alinhados com estratégia: Um requisito que não apoia nenhum objetivo da organização

## Technical Requirements

> 🎯 **ENTREVISTA — FREQUENTE**
> 
> Requisitos técnicos são importantes em entrevistas porque mostram sua capacidade de traduzir necessidades de negócio em soluções viáveis.

### definição
Requisitos técnicos são especificações detalhadas das características tecnológicas que o sistema deve possuir para atender aos requisitos funcionais, não-funcionais e de negócio. Eles descrevem as soluções tecnológicas específicas, padrões, arquiteturas e tecnologias que devem ser usadas.

### características
- **Específicos**: Descrevem tecnologias, padrões ou abordagens concretas
- **Derivados**: São consequência lógica dos outros tipos de requisitos
- **Viáveis**: Devem ser possíveis de implementar com recursos disponíveis
- **Consistentes**: Não devem se contradizer ou criar incompatibilidades
- **Verificáveis**: Devem poder ser testados e validados
- **Documentados**: Devem estar claramente especificados para a equipe de desenvolvimento

### exemplos
- "O sistema deve usar uma arquitetura de microsserviços com comunicação assíncrona via Apache Kafka"
- "O banco de dados principal deve ser PostgreSQL 15 com réplicas de leitura para escalar consultas"
- "O sistema deve implementar autenticação usando OAuth 2.0 com OpenID Connect e JWT"
- "Todos os dados pessoais devem ser criptografados em repouso usando AES-256-GCM"
- "O sistema deve usar containers Docker orquestrados por Kubernetes para deploy e escalabilidade"
- "O frontend deve ser desenvolvido usando React 18 com TypeScript e Redux Toolkit"
- "O sistema deve implementar caching distribuído usando Redis com estratégia cache-aside"
- "A comunicação entre serviços deve usar gRPC com Protobuf para serialização eficiente"
- "O sistema deve aplicar o princípio do menor privilégio usando IAM da AWS com papéis bem definidos"
- "Todos os logs devem be structured em JSON and sent to the ELK stack for analysis"

### como derivar
1. **Analisar requisitos funcionais**: Que tecnologias são necessárias para implementar cada função?
2. **Analisar requisitos não-funcionais**: Que soluções técnicas atendem aos requisitos de performance, segurança, etc.?
3. **Analisar requisitos de negócio**: Que tecnologias apoiam os objetivos estratégicos?
4. **Considerar restrições**: Que tecnologias são obrigatórias ou proibidas?
5. **Validar suposições**: Que tecnologias são compatíveis com nossas suposições sobre ambiente e equipe?
6. **Balancear trade-offs**: Que solução oferece melhor custo-benefício considerando todos os fatores?

### exemplos de derivação
- **Funcional**: "Sistema deve permitir busca em tempo real por produtos"
  + **Não-funcional**: "Resposta em menos de 200ms para 95% das consultas"
  + **Técnico**: "Usar Elasticsearch como engine de busca com índices otimizados e cache de resultados frequentes"

- **Funcional**: "Sistema deve processar pagamentos de cartão de crédito"
  + **Não-funcional**: "Deve ser compatível com PCI-DSS nível 1 e garantir confidencialidade dos dados"
  + **Técnico**: "Usar tokenização de cartão, nunca armazenar PAN completo, integrar com gateway certificado PCI-DSS, criptografar dados sensíveis em repouso"

- **Funcional**: "Sistema deve enviar notificações para usuários"
  + **Não-funcional**: "Deve entregar 99% das notificações em menos de 5 segundos"
  + **Técnico**: "Usar serviço de push notifications (Firebase Cloud Messaging / Apple Push Notification Service) com fila de retry e dead letter queue para falhas"

### armadilhas comuns
- **Prematura otimização**: Especificar soluções tecnológicas complexas demais para o estágio atual
- **Vender solução**: Especificar tecnologia específica sem considerar alternativas melhores
- **Ignorar restrições**: Propor soluções que violam restrições de orçamento, prazo ou equipe
- **Sobre-especificação**: Detalhar demais soluções que deveriam ser decididas pela equipe de desenvolvimento
- **Under-especificação**: Ser muito vago, deixando decisões críticas para acontecerem por acaso
- **Ignorar operacionalidade**: Especificar soluções que a equipe de operações não consegue suportar

### relação com decisões arquiteturais
Requisitos técnicos são o elo entre requisitos de negócio/funcionais e decisões arquiteturais. Eles:
- Traduzem "o que precisamos" em "como vamos fazer"
- Limitam o espaço de soluções baseado em restrições e suppositions
- Fornecem base para avaliar trade-offs entre alternativas
- Servem como entrada para o processo de tomada de decisão arquitetural
- Devem ser revisados à medida que se aprende mais durante o projeto

## Processo de Transformação de Requisitos em Arquitetura

> 🎯 **ENTREVISTA — ALTA FREQUÊNCIA**
> 
> Este processo é crucial em entrevistas porque mostra seu pensamento sistemático e capacidade de lidar com complexidade real.

### visão geral
O processo de transformar requisitos em arquitetura é iterativo e colaborativo, envolvendo múltiplas etapas de análise, síntese, validação e refinamento. Não é uma sequência linear rígida, mas sim um ciclo de aprendizado e adaptação.

```text
Requisitos de Negócio
        ↓
Requisitos Funcionais + Não-Funcionais
        ↓
Análise de Restrições e Suposições
        ↓
Derivação de Requisitos Técnicos
        ↓
Exploração de Alternativas Arquiteturais
        ↓
Avaliação de Trade-offs
        ↓
Tomada de Decisão Arquitetural
        ↓
Validação com Stakeholders
        ↓
Refinamento e Detalhamento
```

### etapa 1: compreensão profunda dos requisitos de negócio
**Objetivo**: Entender o "por trás" do projeto e o valor que se espera criar
**Atividades**:
- Entrevistar patrocinadores e stakeholders de negócio
- Analisar documentos estratégicos e de visão
- Entender métricas de sucesso atuais e desejadas
- Mapear processos de negócio afetados
- Identificar dores e oportunidades
**Resultado**: Declaração clara de objetivos de negócio e métricas de sucesso

### etapa 2: elicitação e análise de requisitos funcionais
**Objetivo**: Definir claramente o que o sistema deve fazer
**Atividades**:
- Workshops com usuários e especialistas de domínio
- Criação de user stories e casos de uso
- Modelagem de processos de negócio (BPMN, fluxogramas)
- Priorização de funcionalidades (MoSCoW, value vs effort)
- Validação com stakeholders através de protótipos ou mockups
**Resultado**: Backlog de requisitos funcionais verificáveis e priorizados

### etapa 3: identificação de requisitos não-funcionais e atributos de qualidade
**Objetivo**: Definir como o sistema deve ser em termos de performance, segurança, etc.
**Atividades**:
- Entrevistar equipes de operações, segurança e compliance
- Analisar SLAs existentes e expectativas de disponibilidade
- Revisar regulamentações aplicáveis ao domínio
- Considerar expectativas de crescimento e escalabilidade futura
- Identificar restrições de performance, segurança, usabilidade, etc.
**Resultado**: Lista de requisitos não-funcionais mensuráveis e priorizados

### etapa 4: documentação de restrições e suposições
**Objetivo**: Entender os limites e incertezas que afetam as decisões
**Atividades**:
- Revisar contratos, políticas corporativas e acordos de nível de serviço
- Consultar equipes de finanças, jurídico e operações
- Analisar arquitetura e tecnologias existentes que precisam ser integradas
- Identificar limitações de orçamento, prazo, recursos humanos e tecnológicos
- Documentar crenças sobre ambiente, tecnologia e comportamento do usuário
**Resultado**: Catálogo claro de restrições (o que não podemos fazer) e suppositions (o que acreditamos ser verdade)

### etapa 5: derivação de requisitos técnicos
**Objetivo**: Traduzir requisitos em especificações tecnológicas viáveis
**Atividades**:
- Analisar cada requisito funcional para determinar necessidades tecnológicas
- Mapear requisitos não-funcionais para soluções técnicas específicas
- Considerar restrições ao eliminar opções tecnológicas inviáveis
- Validar suppositions através de provas de conceito quando necessário
- Balancear trade-offs entre diferentes opções tecnológicas
- Consultar especialistas em tecnologia quando necessário
**Resultado**: Especificação técnica detalhada que guia decisões arquiteturais

### etapa 6: exploração de alternativas arquiteturais
**Objetivo**: Identificar diferentes formas de atender aos requisitos técnicos
**Atividades**:
- Brainstorming de abordagens arquiteturais possíveis
- Pesquisa de padrões e estilos arquiteturais relevantes
- Análise de sistemas similares e lições aprendidas
- Criação de protótipos arquiteturais de baixa fidelidade
- Consulta com especialistas em arquitetura e tecnologia
- Consideração de opções de make vs buy, cloud vs on-premises, etc.
**Resultado**: Conjunto de alternativas arquiteturais viáveis com prós e contras iniciais

### etapa 7: avaliação de trade-offs
**Objetivo**: Selecionar a melhor arquitetura considerando múltiplos critérios
**Atividades**:
- Criar matrizes de decisão com critérios ponderados
- Avaliar cada alternativa contra requisitos funcionais e não-funcionais
- Considerar impacto nas restrições (orçamento, prazo, recursos)
- Avaliar riscos associados a cada alternativa
- Considerar implicações de longo prazo (manutenibilidade, evolvibilidade)
- Realizar revisões de arquitetura com pares especialistas
- Quando possível, criar provas de conceito para validar hipóteses críticas
**Resultado**: Seleção da arquitetura candidate com justificativa baseada em evidências

### etapa 8: tomada de decisão arquitetural
**Objetivo**: Formalizar a escolha arquitetural e comunicar decisões
**Atividades**:
- Documentar decisão arquitetural usando formato ADR (Architecture Decision Record)
- Justificar decisão com referência aos requisitos, restrições e trade-offs avaliados
- Documentar alternativas consideradas e motivo da rejeição
- Especificar consequências esperadas (positivas e negativas)
- Definir métricas para validar se decisão está produzindo resultados esperados
- Comunicar decisão para equipe de desenvolvimento, operações e stakeholders
**Resultado**: Decisão arquitetural documentada e comunicada

### etapa 9: validação com stakeholders
**Objetivo**: Garantir que a arquitetura escolhida atende às expectativas
**Atividades**:
- Revisar decisão arquitetural com patrocinadores de negócio
- Validar viabilidade técnica com equipe de desenvolvimento
- Confirmar que operações consegue suportar a arquitetura escolhida
- Verificar conformidade com requisitos de segurança e compliance
- Obter feedback de arquitetos pares e especialistas em domínio
- Ajustar arquitetura baseado em feedback válido
**Resultado**: Arquitetura validada e ajustada conforme necessário

### etapa 10: refinamento e detalhamento
**Objetivo**: Transformar decisão arquitetural em guia prático para implementação
**Atividades**:
- Criar detalhes de componentes, interfaces e fluxos de dados
- Definir padrões de codificação, convenções de nomes e práticas de desenvolvimento
- Especificar requisitos de infraestrutura (servidores, bancos de dados, redes)
- Definir estratégias de deploy, teste e monitoramento
- Criar guias de migração se estiver substituindo sistema existente
- Definir métricas de operação e painéis de observabilidade
- Planejar capacidade inicial e estratégias de escalabilidade futura
**Resultado**: Especificação arquitetural detalhada pronta para orientar implementação

### padrões deocumentação de decisões
Usar o formato ADR (Architecture Decision Record):

```text
# ADR-XXX — Título da Decisão

## Context
Descrição da situação que levou à necessidade desta decisão.
Inclui requisitos relevantes, restrições, suposições e fatores de negócio.

## Problem
Declaração clara do problema que precisa ser resolvido.
O que estamos tentando alcançar ou evitar com esta decisão?

## Decision
Descrição da escolha feita.
O que exatamente estamos decidindo fazer?

## Alternatives Considered
Lista de outras opções que foram avaliadas.
Para cada alternativa: prós, contras e motivo da não seleção.

## Trade-offs
Análise dos trade-offs envolvidos na decisão.
Quais beneficios estamos abrindo mão e o que estamos ganhando?

## Consequences
Resultado esperado da decisão.
Impactos positivos e negativos em diversos aspectos do sistema.

## Status
Estado atual da decisão (proposta, aprovada, substituída, etc.)
```

### dicas para entrevistas
> 🎯 **ENTREVISTA — ALTA FREQUÊNCIA**
> 
> Em entrevistas de System Design, entrevistadores frequentemente avaliam seu processo de pensamento mais do que a solução final. Mostrar que você segue um processo estruturado como este demonstra maturidade profissional.

#### o que fazer
- Começar sempre perguntando para esclarecer requisitos antes de projetar
- Mencionar explicitamente como você identificaria requisitos funcionais vs não-funcionais
- Discutir como você levaria em conta restrições de orçamento, prazo e equipe
- Explicar como você validaria suposições críticas antes de se comprometer
- Mostrar que você considera múltiplas alternações antes de decidir
- Falar sobre trade-offs de forma consciente e equilibrada
- Mentionar como você validaria decisões com stakeholders
- Discutir como você documentaria decisões arquiteturais para referência futura

#### armadilhas comuns a evitar
- Pular diretamente para solução técnica sem entender o problema
- Ignorar requisitos não-funcionais importantes (escalabilidade, segurança, etc.)
- Fazer suposições não documentadas e não validadas
- Apresentar apenas uma alternativa sem considerar outras
- Ignorar trade-offs ou apresentá-los de forma tendenciosa
- Não mencionar restrições reais (orçamento, prazo, legado)
- Focar apenas na solução ideal sem considerar viabilidade prática
- Esquecer do aspecto operacional (quem vai manter e suportar o sistema)

### exercício prático de entrevista
> 🎯 **ENTREVISTA — MODERADO**
> 
> "Você foi contratado para projetar um sistema de reservas de hotéis online. Descreva seu processo para entender os requisitos e transformá-los em decisões arquiteturais."
> 
> **Resposta esperada**: 
> 1. Começaria entendendo objetivos de negócio (aumentar ocupação, receita média, satisfação do cliente)
> 2. Elicitaria requisitos funcionais (busca por disponibilidade, reserva, pagamento, cancelamento, avaliação)
> 3. Identificaria requisitos não-funcionais (disponibilidade 99.9%, resposta <2s, segurança de dados de pagamento, suporte a 10k usuários simultâneos)
> 4. Documentaria restrições (orçamento limitado, prazo de 4 meses, equipe de 3 desenvolvedores, necessidade de integrar com sistema legados de POs)
> 5. Validaria suppositions (taxa de crescimento de 20% ao ano, hospedagem em AWS, usuários confortáveis com mobile-first)
> 6. Derivaria requisitos técnicos (banco de dados relacional para consistência de reservas, cache para busca de disponibilidade, fila para processamento de pagamento assíncrono)
> 7. Exploraria alternativas (monolito vs microsserviços, arquitetura em camadas vs hexagonal, sync vs async para pagamento)
> 8. Avaliaria trade-offs (complexidade operacional vs escalabilidade, consistência vs disponibilidade)
> 9. Tomaria decisão arquitetural (monolito modular com preparação para extração de serviços, PostgreSQL com read replicas, Redis para cache, RabbitMQ para fila de pagamento)
>10. Validaria decisão com stakeholders de negócio, desenvolvimento e operações
>11. Refineriaria arquitetura com detalhes de componentes, interfaces, padrões de código e estratégias de deploy

## Exercícios

### Exercício básico
Diferença entre requisitos funcionais e não-funcionais usando exemplos de um aplicativo de delivery de comida.

### Exercício intermediário
Dado um cenário de sistema bancário online, identifique:
- 3 objetivos de negócio
- 5 requisitos funcionais
- 4 requisitos não-funcionais
- 3 restrições
- 2 suposições
Mostre como cada tipo de requisito influencia o outro.

### Exercício avançado
Analise um sistema que você conhece ou trabalhou recentemente e:
1. Documente os requisitos de negócio, funcionais e não-funcionais
2. Identifique as principais restrições e suposições
3. Mostre como os requisitos técnicos foram derivados
4. Descreva o processo de tomada de decisão arquitetural usado (se houver)
5. Identifique oportunidades de melhoria no processo

### Exercício de entrevista
> 🎯 **ENTREVISTA — ALTA FREQUÊNCIA**
> 
> "Como você lidaria com requisitos conflitantes, onde atender a um requisito não-funcional iria diretamente contra outro?"
> 
> Forneça a resposta esperada e explique o que torna ela eficaz.

### Desafio
Crie um template de documento de requisitos que inclua seções para todos os tipos de requisitos discutidos nesta seção, com instruções claras para preenchimento e exemplos de bom e ruim preenchimento para cada seção.