---
trilha: "PARA ENTREVISTAS"
---
**Navegação:** [[MOC — TRILHA PARA ENTREVISTAS]]
← [[PARTE 81 — ARQUITETURA PARA SISTEMAS LEGADOS]] | #trilha/entrevistas | [[PARTE 83 — TESTES E ARQUITETURA]] →

---
# PARTE 82 — MIGRAÇÃO DE ARQUITETURA

## Fundamentos

Migração de arquitetura refere-se ao processo de transformar a estrutura fundamental de um sistema de software, alterando seus estilos arquiteturais, padrões de comunicação, tecnologias subjacentes ou organização de componentes, mantendo ou aprimorando sua funcionalidade e qualidades não-funcionais. Diferente de manutenção ou evolução incremental, a migração de arquitetura frequentemente envolve mudanças mais radicais e estruturais, embora possa ser realizada de forma incremental para reduzir riscos.

### Tipos de Migração de Arquitetura

Com base no modelo de modernização de aplicações (como o 5R's da Gartner ou as 6R's da AWS), as migrações de arquitetura podem ser categorizadas da seguinte forma:

1. **Rehosting (Lift and Shift)**: mover uma aplicação para uma nova infraestrutura (como nuvem) sem alterar sua arquitetura ou código. Foca em melhorar operabilidade, escalabilidade e custo de infraestrutura.
2. **Replatforming (Lift, Tinker and Shift)**: fazer otimizações seletivas na plataforma durante a migração (ex: mudar de um banco de dados legado para um gerenciado na nuvem) sem alterar a arquitetura núcleo da aplicação.
3. **Refactoring (Reestruturar)**: reestruturar e otimizar o código existente usando tecnologias nativas da nova plataforma (ex: decompor um monolítico em microservices) sem alterar o comportamento externo fundamental.
4. **Rearchitecting (Reformular)**: mudar significativamente o estilo arquitetural (ex: de monolítico para event-driven, ou de camadas para hexagonal) para melhorar qualidades não-funcionais como escalabilidade, resiliência ou agilidade.
5. **Rebuilding (Reconstruir)**: reconstruir a aplicação do zero preservando apenas o escopo e requisitos de negócio, geralmente usando arquiteturas e tecnologias modernas.
6. **Replacing (Substituir)**: abandonar a aplicação existente e adotar uma solução pronta (Saas, COTS) que atenda aos mesmos requisitos de negócio.

### Motivações para Migração de Arquitetura

1. **Melhoria de Escalabilidade**: arquiteturas monolíticas ou com gargalos de arquitetura podem limitar a capacidade de escalar horizontalmente ou lidar com picos de carga.
2. **Aumento da Agilidade**: arquiteturas rígidas tornam lento e arriscado fazer mudanças, impactando o time-to-market.
3. **Redução de Custos**: tecnologias legadas podem ter altos custos de licenciamento, manutenção ou operação; migração para plataformas modernas pode reduzir esses custos.
4. **Melhoria de Resiliência e Disponibilidade**: arquiteturas antigas podem não ter mecanismos adequados de tolerância a falhas, levando a indisponibilidades frequentes.
5. **Integração com Novas Tecnologias**: legados podem ser difíceis de integrar com novos sistemas de IA, IoT, analytics avançado ou plataformas de terceiros.
6. **Atração e Retenção de Talentos**: equipes podem preferir trabalhar com tecnologias e práticas modernas.
7. **Conformidade e Segurança**: arquiteturas antigas podem não atender a padrões modernos de segurança ou regulatórios.
8. **Eliminação de Dívida Técnica**: anos de atalhos e patches podem tornaram o sistema difícil de manter e evoluir.

### Desafios Comuns na Migração de Arquitetura

1. **Preservação de Funcionalidade**: garantir que o comportamento externo do sistema permaneça inalterado (ou melhorado de forma controlada) durante e após a migração.
2. **Migração de Dados**: mover grandes volumes de dados, muitas vezes com esquemas complexos, minimizando downtime e garantindo consistência.
3. **Dependências Ocultas**: descobrir dependências não documentadas durante o processo de migração.
4. **Gap de Habilidades**: a equipe pode falta experiência com as novas tecnologias ou padrões arquiteturais.
5. **Complexidade Operacional**: novas arquiteturas podem introduzir complexidade em áreas como deploy, monitoramento e solução de problemas.
6. **Custo e Prazo**: migrações de arquitetura podem ser subestimadas em termos de esforço, tempo e recursos necessários.
7. **Resistência à Mudança**: stakeholders podem temer riscos associados a mudanças significativas em sistemas críticos.
8. **Performance Inesperada**: otimizações teóricas podem não se traduzir em melhorias de performance reais devido a gargalos não anticipados.

### Princípios para Migração de Arquitetura Bem-sucedida

1. **Valor Incremental**: entregar valor em cada incremento da migração, evitando "big bang" migrations sempre que possível.
2. **Feedback Contínuo**: validar hipóteses de desempenho, custo e usabilidade após cada incremento.
3. **Compatibilidade e Coexistência**: projetar para que antigo e novo possam coexistir durante a transição.
4. **Observabilidade**: instrumentar ambos os sistemas para entender comportamento, desempenho e uso.
5. **Automatização**: usar pipelines de CI/CD, infraestrutura como código e testes automatizados para reduzir riscos e aumentar frequência de feedback.
6. **Mitigação de Riscos**: ter estratégias de rollback, testes de fallback e monitoramento intensivo.
7. **Alinhamento de Negócio**: garantir que a migração esteja alinhada com objetivos de negócio e métricas de sucesso claras.

## Técnicas

### Técnica 1: Padrão Strangler Fig Aplicado à Migração de Arquitetura

- **Descrição": adaptar o padrão Strangler Fig (figueira estranguladora) para migração de arquitetura, onde uma nova arquitetura é construída aos poucos ao redor da existente, redirecionando gradualmente funcionalidades para a nova implementação.
- **Passos**:
  1. Identificar um domínio de negócio bem delimitado ou um conjunto de funcionalidades de baixo acoplamento.
  2. Implementar essa funcionalidade usando a nova arquitetura-alvo (ex: microservices, event-driven, serverless).
  3. Usar um mecanismo de roteamento (API gateway, proxy, feature toggle) para direcionar requisições para a nova implementação quando disponível.
  4. Validar a nova implementação em termos de funcionalidade, desempenho e qualidade.
  5. Gradualmente aumentar o percentual de tráfego roteado para a nova arquitetura.
  6. Repetir para outros domínios até que a nova arquitetura substitua completamente a antiga.
  7. Desativar componentes legados correspondentes.
- **Beneficios":
  - Reduz risco ao permitir rollback fácil e validação incremental.
  - Fornece valor continuamente à medida que cada domínio é migrado.
  - Permite aprendizado e adaptação da equipe durante o processo.
- **Desafios":
  - Necessidade de roteamento inteligente e gerenciamento de estado entre arquiteturas.
  - Potencial duplicação de lógica durante a transição.
  - Complexidade aumentada em gerenciar duas arquiteturas simultaneamente.

### Técnica 2: Migração por Camada (Layer-by-Layer Migration)

- **Descrição": migrar a arquitetura camada por camada, começando pelas camadas de menor risco ou maior valor, preservando contratos entre camadas durante a transição.
- **Passos":
  1. Identificar as camadas da arquitetura atual (ex: presentation, application, business logic, data access).
  2. Para cada camada, definir um contrato claro com as camadas adjacentes.
  3. Migrar uma camada de cada vez para a nova tecnologia ou padrão, mantendo o contrato com as outras camadas.
  4. Após migrar uma camada, validar que o sistema como um todo ainda funciona corretamente.
  5. Prossiga para a próxima camada, repetindo o processo.
  6. Após todas as camadas serem migradas, otimizar as interfaces entre elas se necessário.
- **Beneficios":
  - Permite foco em uma preocupação de de cada vez.
  - Reduz risco ao isolar mudanças em uma camada específica.
  - Facilita testes de contrato entre camadas.
- **Desafios":
  - Camadas podem ter dependências circulares ou acoplamento oculto que dificultam o isolamento.
  - Contratos entre camadas podem precisar de ajustes durante a migração.
  - Pode não ser adequado para mudanças arquiteturais radicais que afetam múltiplas camadas simultaneamente.

### Técnica 3: Padrão de Camada Anti-Corrupção para Migração

- **Descrição": usar uma camada de anticorrupção para isolar a nova arquitetura das limitações e particularidades da arquitetura legada durante a migração.
- **Passos":
  1. Definir o modelo de domínio ou contrato desejado para a nova arquitetura.
  2. Implementar uma camada de serviço que traduz entre as interfaces do legado e o modelo da nova arquitetura.
  3. A nova arquitetura se comunica exclusivamente com a camada de anticorrupção.
  4. À medida que mais funcionalidades são migradas, a camada de anticorrupção lida com menos tráfego do legado.
  5. Após a migração completa, a camada de anticorrupção pode ser simplificada ou removida se não for mais necessária.
- **Beneficios":
  - Isola a nova arquitetura das restrições e peculiaridades do legado.
  - Permite evolução independente da nova arquitetura.
  - Facilita testes e validação da nova arquitetura em isolamento.
- **Desafios":
  - Overhead de tradução pode afetar desempenho.
  - Requer manutenção da camada de tradução durante o período de transição.
  - Pode se tornar um gargalo se não for dimensionada adequadamente.

### Técnica 4: Migração de Banco de Dados com Estrangulamento

- **Descrição": focar primeiro na migração da camada de dados, movendo para um banco de dados mais moderno enquanto mantém as aplicações existentes ou as atualiza gradualmente.
- **Passos":
  1. Avaliar o uso do banco de dados atual (consultas, transações, volume, padrões de acesso).
  2. Escolher um banco de dados destino moderno (ex: migrar de Oracle para PostgreSQL, de SQL Server para Amazon Aurora).
  3. Implementar mecanismos de sincronização (change data capture, replication, dual-write) entre o banco legado e o novo.
  4. Migrar gradualmente leituras para o novo banco, validando desempenho e consistência.
  5. Após estabilizar leituras, migrar escritas usando padrões como evento de atualização ou transação distribuída.
  6. Atualizar aplicações para usar o novo banco de dados diretamente.
  7. Desativar o banco de dados legado após confirmar que todas aplicações estão usando o novo.
- **Beneficios":
  - Melhora desempenho, escalabilidade e custos frequentemente associados a bancos de dados legados.
  - Permite uso de ferramentas modernas de administração, backup e análise.
  - Fornece base para futuras modernizações de aplicação.
- **Desafios":
  - Complexidade de garantir consistência durante a sincronização bidirecional.
  - Diferenças em recursos, SQL e comportamentos entre bancos de dados.
  - Necessidade de tratamento de tipos de dados especiais e migração de esquemas complexos.
  - Latência adicional durante períodos de duplicação.

### Técnica 5: Replatforming com Containers e Orquestração

- **Descrição": empacotar a aplicação existente em containers e usar uma plataforma de orquestração (como Kubernetes) para melhorar deploy, escalabilidade e gerenciamento, antes de realizar mudanças arquiteturais mais profundas.
- **Passos":
  1. Containerizar a aplicação (ou componentes dela) usando Docker ou similar.
  2. Definir manifestos de orquestração (Deployments, Services, etc.) para Kubernetes ou outra plataforma.
  3. Migrar para executar na plataforma de orquestração, inicialmente com configuração mínima (réplica única, recursos básicos).
  4. Melhorar gradualmente a configuração: auto-scaling, health checks, estratégias de deploy (blue/green, canary).
  5. Introduzir recursos nativos da plataforma: service mesh, políticas de segurança, observabilidade integrada.
  6. Após estabilizar na plataforma, considerar refatoração arquitetural (ex: decompor para microservices) usando a mesma infraestrutura.
- **Beneficios":
  - Melhora portabilidade e consistência entre ambientes.
  - Fornece base para escalabilidade e resiliência aprimoradas.
  - Facilita gerenciamento através de declarações e APIs padrão.
  - Permite experimentação com arquiteturas nativas de nuvem sem mudar imediatamente o código.
- **Desafios":
  - Overhead de containerização pode afetar desempenho em alguns casos.
  - Curva de aprendizado para equipes não familiarizadas com containers e orquestração.
  - Necessidade de ajustar aplicações para serem "cloud-native" (stateless, externação de estado, etc.).
  - Gerenciamento de estado persistente em ambientes de orquestração pode ser complexo.

### Técnica 6: Migração Orientada por API (API-First Migration)

- **Descrição": definir ou refinar APIs como contrato estável entre sistemas, permitindo que a implementação por trás da API mude independente dos consumidores.
- **Passos":
  1. Identificar pontos de integração críticos (APIs existentes, eventos, troca de arquivos).
  2. Definir ou refinar esses contratos usando padrões abertos (OpenAPI/Swagger, AsyncAPI, Protobuf).
  3. Versionar os contratos explicitamente para permitir evolução.
  4. Implementar a nova arquitetura por trás da mesma interface de API.
  5. Usar mecanismos de roteamento ou feature flags para direcionar tráfego entre implementações antigas e novas.
  6. Validar com testes de contrato para garantir que tanto a implementação antiga quanto a nova atendem ao mesmo contrato.
  7. Gradualmente migrar consumidores para a nova implementação ou simplesmente deixar o roteador fazer o trabalho.
- **Beneficios":
  - Desacopla evolução da implementação da experiência do consumidor.
  - Fornece interface estável para equipes externas e parceiros de integração.
  - Facilita testes e validação independente da implementação.
- **Desafios":
  - Requer investimento inicial em definição e versionamento de contratos.
  - Pode mascarar problemas de desempenho se a camada de API não for otimizada.
  - Gerenciamento de múltiplas versões de API pode aumentar complexidade.

### Técnica 7: Padrão de Expansão e Contratura para Interfaces

- **Descrição": aplicar o padrão de expansão e contratura (expand and contract) em nível de arquitetura, onde novas interfaces são adicionadas junto às antigas, e depois as antigas são removidas após confirmação de que não são mais usadas.
- **Passos":
  1. **Expandir**: adicionar novos endpoints, eventos ou interfaces que suportam a nova arquitetura, mantendo as existentes.
  2. **Migrar**: atualizar sistemas consumidores (interno ou externos) para usar as novas interfaces.
  3. **Contratar**: após confirmar que nenhum consumidor depende mais das interfaces antigas, removê-las ou desativá-las.
  4. Usar monitoramento de uso, logs e análise para decidir quando é seguro contratar.
- **Beneficios":
  - Permite evolução sem quebrar consumidores existentes.
  - Fornece métricas claras de adoção para decidir quando remover o antigo.
  - Reduz risco ao permitir validação incremental.
- **Desafios":
  - Sobrecarga de manutenção de múltiplas interfaces simultaneamente.
  - Necessidade de sistemas de monitoramento e alerta para rastrear uso de interfaces antigas.
  - Pode haver consumidores "silenciosos" que são difíceis de rastrear.

### Técnica 8: Migração com Espelhamento e Sincronização de Dados

- **Descrição": manter cópias sincronizadas de dados em ambos os sistemas (legado e novo) durante a transição, permitindo leitura em qualquer um e validação paralela.
- **Passos":
  1. Estabelecer mecanismos de captura de alterações (CDC) do sistema de origem (legado ou novo, dependendo da direção da migração).
  2. Transportar alterações para o sistema de destino usando filas, streaming (Kafka, Kinesis) ou batch.
  3. Aplicar transformações necessárias para adaptar esquemas de dados entre sistemas.
  4. Permitir que ambos os sistemas sirvam leituras durante o período de transição.
  5. Usar técnicas de leitura de quórum ou validação paralela para garantir consistência.
  6. Após validar que o novo sistema está correto, redirecionar todas as leituras e escritas para ele.
  7. Desativar o sistema de origem após confirmação de estabilidade.
- **Beneficios":
  - Permite validação paralela e comparação direta entre sistemas.
  - Fornece mecanismo de rollback instantâneo (basta ler do legado).
  - Reduz risco de perda ou corrupção de dados durante a migração.
- **Desafios":
  - Complexidade de implementar CDC confiável em ambos os sistemas.
  - Latência entre alteração em um sistema e disponibilidade no outro.
  - Necessidade de tratamento de conflitos se ambos os sistemas puderem escrever simultaneamente.
  - Custo de armazenamento e processamento duplicado durante a transição.

## Checklist

### Avaliação e Planejamento Pré-Migração

- [ ] Definir claramente os objetivos da migração (ex: melhorar escalabilidade, reduzir custo, atender novo requisito regulatório).
- [ ] Mapear a arquitetura atual (estilos, padrões, tecnologias, dependências, pontos de integração).
- [ ] Identificar métricas de sucesso (KPIs) que indicarão se a migração atingiu seus objetivos (ex: tempo de resposta, custo por transação, frequência de deploy).
- [ ] Avaliar riscos técnicos, operacionais e de negócio associados à migração.
- [ ] Estimar esforço, cronograma e recursos necessários (pessoas, infraestrutura, licenças).
- [ ] Escolher uma estratégia de migração (rehosting, replatforming, refactoring, rearchitecting, rebuilding, replacing).
- [ ] Planejar a abordagem de migração (big bang, phased, strangler fig, camada por camada, etc.).
- [ ] Definir critérios de aceitação para cada fase da migração.
- [ ] Estabelecer estratégias de mitigação de riscos (rollback, testes em ambiente de homologação, monitoramento intensivo).
- [ ] Planejar treinamento e capacitação da equipe para novas tecnologias e padrões.
- [ ] Definir processos de governança e aprovação para mudanças arquiteturais durante a migração.
- [ ] Identificar requisitos de conformidade ou regulatórios que devem ser atendidos durante e após a migração.
- [ ] Planejar comunicação com stakeholders (equipes de desenvolvimento, operações, suporte, negócio, clientes externos).

### Preparação de Ambiente e Infraestrutura

- [ ] Preparar ambiente de destino (nuvem, data center, plataforma de orquestração) com capacidade adequada.
- [ ] Configurar rede, segurança e governança no ambiente de destino.
- [ ] Implementar mecanismos de observabilidade (logs, métricas, tracing) no ambiente de destino.
- [ ] Configurar backup e recuperação de desastre para o ambiente de destino.
- [ ] Estabelecer pipelines de CI/CD para a nova arquitetura.
- [ ] Preparar ambientes de desenvolvimento, teste e homologação que espelhem o destino.
- [ ] Configurar gerenciamento de configuração e secrets (ex: Vault, AWS Secrets Manager).
- [ ] Implementar mecanismos de controle de acesso e autenticação adequados.
- [ ] Testar conectividade e desempenho entre ambientes se houver integração durante a transição.

### Execução da Migração

- [ ] Implementar mecanismos de roteamento ou feature flags para controlar o fluxo de tráfego entre arquiteturas.
- [ ] Migrar dados em lotes ou usando sincronização contínua, validando integridade após cada lote.
- [ ] Executar testes de funcionalidade, desempenho e segurança em ambiente de homologação antes da produção.
- [ ] Iniciar migração com um segmento de baixa risco ou alto valor para validar a abordagem.
- [ ] Coletar e analisar métricas antes, durante e após cada incremento de migração.
- [ ] Validar com testes de contrato para garantir compatibilidade com consumidores existentes.
- [ ] Monitorar de perto indicadores de saúde (error rates, latência, throughput, recursos).
- [ ] Implementar estratégias de rollback rápido caso sejam detectados problemas críticos.
- [ ] Atualizar documentação arquitetural, diagramas e runbooks conforme a migração avança.
- [ ] Comunicar claramente marcos, status e impactos a todas as partes interessadas.

### Validação e Otimização Pós-Migração

- [ ] Executar testes abrangentes de regressão funcional no novo sistema.
- [ ] Validar que métricas de desempenho atendem ou superam os objetivos estabelecidos.
- [ ] Revisar logs e traces para identificar padrões de erro ou gargalos de desempenho.
- [ ] Otimizar configuração com base em observação real (tuning de bancos de dados, ajustes de recursos, etc.).
- [ ] Realizar testes de carga e stress para validar escalabilidade e resiliência.
- [ ] Revisar custos operacionais e comparar com estimativas pré-migração.
- [ ] Coletar feedback de usuários, equipe de operações e stakeholders de negócio.
- [ ] Documentar lições aprendidas, incluindo o que funcionou bem e o que poderia ser melhorado.
- [ ] Planejar a próxima fase de evolução ou entrar em modo de manutenção com melhoria contínua.
- [ ] Arquivar ou desativar componentes legados após confirmação de que não são mais necessários.
- [ ] Realizar uma revisão de arquitetura final para garantir conformidade com padrões e princípios estabelecidos.

## Estudos de Caso

### Caso 1: Migração de um Sistema de Pedidos Monolítico para Microservices com Strangler Fig e API Gateway

- **Contexto": uma plataforma de e-commerce de médio porte tinha um monolítico Ruby on Rails que processava pedidos, gestão de catálogo e contas de usuários. O sistema enfrentava longos tempos de deploy (45+ minutos), dificuldade de escalar durante promoções e baixa frequência de releases devido ao acoplamento alto entre funcionalidades.
- **Objetivo da Migração": aumentar a frequência de deploy de semanal para diário, permitir escalonamento independente de componentes de alto volume (checkout, busca, recomendações) e reduzir o impacto de falhas isoladas.
- **Abordagem Adotada":
  1. Começou com o módulo de recomendações de produtos, que tinha baixo acoplamento com o restante do sistema e alto valor de negócio.
  2. Implementou um novo serviço de recomendações em Python usando FastAPI, implantado em containers Docker no Amazon ECS.
  3. Configurou um API gateway (AWS API Gateway) para rotear requisições para `/recommendations/*` para o novo serviço quando disponível, com fallback para o monolítico.
  4. Implementou eventos de atualização sempre que o catálogo de produtos mudava, usando Amazon SNS para notificar o novo serviço de recomendações.
  5. Após validação de funcionalidade e desempenho, aumentou gradualmente o percentual de tráfego de recomendações roteado para o novo serviço de 0% para 100% ao longo de duas semanas.
  6. Repetiu o processo para o módulo de checkout, implementando um novo serviço em Node.js separado para processamento de pagamentos e geração de pedidos.
  7. Para o checkout, usou um padrão de saga com orquestração (AWS Step Functions) para gerenciar transações distribuídas entre o novo serviço de checkout e o monolítico legado para funcionalidades como estoque e notificação.
  8. Migrou o módulo de busca para um serviço baseado em Elasticsearch, mantendo sincronização com o catálogo de produtos legado através de change data capture do banco de dados PostgreSQL.
  9. Após migrar mais de 70% da funcionalidade para novos serviços, iniciou o processo de refatoração do monolítico restante, extraindo funcionalidades de baixa utilização primeiro.
  10. Após oito meses, desativou o monolítico completamente, mantendo-o apenas como referência por mais um mês antes da exclusão final.
- **Desafios Enfrentados":
  - Latência aumentada inicialmente devido à chamada de serviço entre microservices foi mitigada com caching agressivo em camadas de decisão.
  - Complexidade de gerenciamento de estado de saga exigiu investimento em ferramentas de monitoramento e visualização de workflows.
  - Diferenças em modelos de dados entre o legado e os novos serviços exigiram camadas de tradução cuidadosas durante o período de sobreposição.
- **Resultados":
  - Tempo de deploy de funcionalidades isoladas reduziu de 45 minutos para menos de 5 minutos.
  - Frequência de release aumentou de semanal para múltiplas vezes por dia.
  - Escalonamento horizontal do serviço de checkout permitiu lidar com 3x o tráfego de pico sem degradação de desempenho.
  - Isolamento de falhas melhorou significativamente: problemas no serviço de recomendações não afetavam mais o processo de checkout.
  - Equipe de desenvolvimento relatou aumento de satisfação e produtividade devido à menor complexidade cognitiva por serviço.
- **Lições Aprendidas":
  - Começar com um domínio de baixo risco e alto valor constrói confiança e fornece aprendizado valioso para fases subsequentes.
  - Investir em plataforma de integração (API gateway, service mesh, orquestração) é crucial para gerenciar complexidade durante a migração.
  - Períodos de sobreposição com mecanismos de fallback são essenciais para sistemas de alta criticalidade.
  - Métricas de negócio e de sistema devem ser monitoradas continuamente para validar cada incremento da migração.

### Caso 2: Replatforming de um Sistema Bancário Legado para Arquitetura Nativa de Nuvem com Containers e Service Mesh

- **Contexto": um banco regional tinha um núcleo de sistema desenvolvido em Java EE executando em servidores físicos de um data center próprio, com banco de dados Oracle licenciado. O sistema era crítico para operações de contas corrente, poupança e empréstimos, mas enfrentava custos altos de infraestrutura, longos ciclos de provisionamento de ambiente e dificuldade de integrar com novos fintechs e plataformas de pagamento modernos.
- **Objetivo da Migração": reduzir custos de infraestrutura em 40%, melhorar time-to-market de novas funcionalidades de meses para semanas e estabelecer base para inovação em open banking e integração com terceiros.
- **Abordagem Adotada":
  1. Começou com a containerização de aplicações menos críticas (portais de consulta, ferramentas de relatório interno) usando Docker e implantação em um cluster Kubernetes de prova de conceito no AWS EKS.
  2. Implementou um service mesh (Istio) para gerenciar tráfego, segurança e observabilidade entre os serviços containerizados.
  3. Migrou o banco de dados Oracle para Amazon Aurora PostgreSQL usando o serviço de migração de banco de dados da AWS (DMS) com replicação contínua para minimizar downtime.
  4. Para o núcleo de transações críticas, inicialmente fez rehosting lift-and-shift: moveu as aplicações Java EE para máquinas virtuais no AWS EC2 mantendo a mesma arquitetura.
  5. Após estabilizar na plataforma de nuvem, começou a refatorar o núcleo para microservices usando o padrão strangler fig, começando pelo módulo de consulta de saldo e extrato.
  6. Implementou um padrão de camada anti-corrupção entre o núcleo Java EE legado e os novos microservices, traduzindo entre o modelo de dados legado e os contratos REST/JSON dos novos serviços.
  7. Usou o service mesh para aplicar políticas de segurança (mTLS, autorização) e observabilidade (traces, métricas) entre legado e novos serviços.
  8. Gradualmente moveu mais funcionalidades do núcleo para microservices (transferências, pagamento de boletos, abertura de contas).
  9. Após validar que os novos microservices atendiam aos requisitos de desempenho e confiabilidade, desativou gradualmente as instâncias EC2 do legado.
  10. Após 18 meses, concluiu a migração, com aproximadamente 60% do núcleo refatorado para microservices e 40% ainda em arquitetura legado Java EE rodando em EC2 como plano de transição para serem migrados nos próximos 12 meses.
- **Desafios Enfrentados":
  - Licenciamento do Oracle foi um obstáculo inicial para o rehosting direto; a migração para Aurora exigiu testes extensivos de compatibilidade de SQL e procedimentos armazenados.
  - Complexidade de gerenciamento de Java EE em containers exigiu ajustes de inicialização, memória e configuração de thread pool.
  - Latência de chamada entre serviços no service mesh foi otimizada com ajustes de configuração do Istio e uso de locality load balancing.
  - Curva de aprendizado da equipe para Kubernetes, Istio e práticas de microservices exigiu investimento em treinamento e pair programming.
- **Resultados":
  - Custo de infraestrutura reduziu 35% no primeiro ano após a migração completa para EKS e Aurora.
  - Tempo de provisionamento de ambiente de desenvolvimento reduziu de dias para minutos através de namespaces Kubernetes e GitOps.
  - Novas funcionalidades de integração com open banking puderam ser desenvolvidas e lançadas em semanas devido à arquitetura de microservices e API bem definida.
  - Observabilidade em tempo real permitiu detecção precoce de problemas de desempenho e gargalos de recursos.
  - A equipe desenvolveu experiência em tecnologias nativas de nuvem, reduzindo dependência de consultores externos.
- **Lições Aprendidas":
  - Uma abordagem faseada, onde se melhora a infraestrutura primeiro antes de mudar a arquitetura, pode reduzir riscos e fornecer valor imediato.
  - Investir em plataforma (containers, service mesh, GitOps) paga dividendos em agilidade e operacionalidade a longo prazo.
  - Migração de banco de dados requer validação extensiva de compatibilidade, especialmente para procedimentos armazenados e recursos proprietários.
  - Períodos de transição prolongados são aceitáveis e muitas vezes necessários para sistemas críticos, desde que haja governança e métricas claras.

### Caso 3: Rebuilding de um Sistema de Gestão de Conteúdo Legado para Arquitetura Serverless com API Gateway e Funções

- **Contexto": uma organização de mídia tinha um CMS legados desenvolvido em PHP com banco de dados MySQL que gerenciava a publicação de artigos, vídeos e podcasts para múltiplos canais web e mobile. O sistema enfrentava problemas de escalabilidade durante eventos ao vivo, dificuldade de integrar com novas plataformas de distribuição (como apps de smart TV) e altos custos de manutenção devido à base de código antiga e falta de documentação.
- **Objetivo da Migração": melhorar escalabilidade para lidar com picos de tráfego de 10x o normal durante eventos ao vivo, reduzir custos operacionais pagando apenas pelo uso e estabelecer base para integração fácil com novos canais de distribuição.
- **Abordagem Adotada":
  1. Decidiu-se por um rebuilding completo (substituição do zero) devido à idade do código, falta de documentação e desejo de adotar arquitetura moderna desde o início.
  2. Definiu o escopo funcional do CMS através de workshops com stakeholders de conteúdo, engenharia e produto.
  3. Escolheu uma arquitetura serverless usando AWS Lambda para funções de processamento, Amazon API Gateway para exposição de APIs e Amazon DynamoDB para armazenamento flexível de conteúdo.
  4. Implementou funções Lambda para operações críticas: criação/atualização de conteúdo, publicação, busca, gerenciamento de usuários e webhook para notificação de plataformas externas.
  5. Usou o DynamoDB com modelo de dados denormalizado e índices globais para suportar padrões de acesso variados (busca por tag, data, autor, tipo de conteúdo).
  6. Implementou um pipeline de ingestão de conteúdo usando AWS Step Functions para orquestrar validação, enriquecimento e armazenamento de conteúdo enviado por equipes externas.
  7. Configurou o API Gateway com throttling, cache de respostas e autenticação Cognito para controlar acesso e proteger contra abusos.
  8. Construiu um painel de administração em React consumindo as mesmas APIs públicas, fornecendo interface para equipes de conteúdo e edição.
  9. Migrou conteúdo existente do MySQL legado para o DynamoDB usando um job de migração em lote com validação pós-migração.
  10. Após validar funcionalidade e desempenho com um grupo piloto de usuários, fez o cutover direto para o novo sistema, mantendo o legado apenas como fallback de leitura por mais duas semanas antes da desativação final.
- **Desafios Enfrentados":
  - Modelagem de dados no DynamoDB exigiu múltiplas iterações para atender aos diversos padrões de acesso sem scans inefficientes.
  - Limites de concorrência iniciais do Lambda foram aumentados após monitoramento mostrar throttling durante testes de carga.
  - Custos inesperados de leitura no DynamoDB foram mitigados com uso agressivo do API Gateway cache e otimização de padrões de acesso.
  - Integração com sistemas legados de pagamento e analytics exigiu adapters Lambda específicos para traduzir entre formatos.
- **Resultados":
  - O sistema passou de travar com mais de 500 usuários simultâneos para lidar com conforto com picos de 5000+ usuários durante eventos ao vivo.
  - Custo operacional reduziu 60% devido ao modelo pay-per-use do serverless e eliminação de custos de licença e manutenção do legado.
  - Tempo de lançamento de novas funcionalidades reduziu de meses para semanas devido à arquitetura desacoplada e deploy independente de funções.
  - Integração com novos canais de distribuição (smart TV, apps mobile) tornou-se simples através da API pública bem documentada.
  - A equipe de conteúdo relatou maior satisfação com a interface de administração moderna e responsiva.
- **Lições Aprendidas":
  - Um rebuilding completo pode ser viável e vantajoso quando o legado tem alta dívida técnica e baixa documentação, desde que o escopo seja bem definido.
  - Arquiteturas serverless fornecem escalabilidade automática que é ideal para cargas de trabalho imprevisíveis como eventos ao vivo e publicação de conteúdo.
  - Investir em modelo de dados bem pensado para o banco de dados destino é crucial para desempenho e custo em arquiteturas serverless.
  - Painéis de administração consumindo as mesmas APIs públicas garantem consistência e reduzem esforço de desenvolvimento.

## Tendências Futuras

### 1. Migração Guiada por Observabilidade e Análise de Tráfego Real

- **Descrição": uso de ferramentas de observabilidade em tempo real para analisar padrões de tráfego, dependências e desempenho do sistema legado, informando decisões sobre o que migrar primeiro, como particionar funcionalidades e onde focar esforços de otimização.
- **Impacto": arquitetos passarão a basear estratégias de migração em dados reais de uso em vez de apenas suposições ou documentação muitas vezes desatualizada.
- **Habilidades Relevantes": interpretação de dashboards de observabilidade, análise de dependências de serviço, definição de SLIs/SLOs para guiar decisões de migração.

### 2. Plataformas de Migração Automatizada e Orquestrada

- **Descrição": plataformas integradas que combinam análise de código legado, geração de plantas de migração, teste de compatibilidade, orquestração de workflows de migração e validação automática em ambientes de homologação e produção.
- **Impacto": equipes podem focar na validação de resultados de negócio em vez de construir do zero a infraestrutura técnica complexa de migração.
- **Habilidades Relevants": compreensão de plataformas de transformação de código (baseadas em grammars e reescrita), gerenciamento de workflows complexos de migração, orquestração de testes em múltiplos ambientes.

### 3. Migração em Direção a Arquiteturas de Intents e Orquestração Avançada

- **Descrição": à medida que plataformas de orquestração avançada (workflow engines, service mesh com políticas dinâmicas, plataformas de low-code/no-code) amadurecem, a migração pode focar menos em reconstruir componentes e mais em expor intenções de negócio que a plataforma realiza, migrando gradualmente a lógica de implementação para essas plataformas.
- **Impacto": a tarefa do arquiteto muda de projetar conexões e APIs para modelar resultados de negócio e definir como eles serão realizados por plataformas de orquestração e automação avançada.
- **Habilidades Relevants": modelagem de resultados de negócio, engenharia de políticas (OPA, Rego), compreensão de plataformas de orquestração e automação avançada (como Temporal, Camunda, Zapier para empresas).

### 4. Padrões de Migração para Arquiteturas Nativas de IA

- **Descrição": sistemas que incorporam modelos de ML como componentes de primeira classe exigem padrões específicos para migrar tanto o software tradicional quanto os modelos de ML, seus dados, pipelines de treinamento/serving e infraestrutura de MLOps.
- **Impacto": arquitetos precisarão lidar com versionamento de modelos, drift de dados, re-treinamento contínuo, incerteza de inferência e integração de componentes de IA em padrões de migração estabelecidos.
- **Habilidades Relevants": conhecimento de MLOps, versionamento de modelos (MLflow, DVC), testes de modelos, monitoramento de drift e performance de inferência, compreensão de como arquiteturas afetam a vida útil de modelos de ML.

### 5. Migração com Foco em Sustentabilidade e Pegada de Carbono

- **Descrição": considerar o consumo de energia e pegada de carbono como critérios na escolha de estratégias de migração, favorecendo abordagens que reduzam recursos computacionais, otimizem uso de infraestrutura ou migrem para plataformas mais eficientes energeticamente.
- **Impacto": arquitetos precisarão incluir métricas de eficiência energética (como PUE ajustado para carga de trabalho) em avaliações de trade-offs junto com custo, desempenho e segurança ao escolher entre rehosting, refatoração ou replatforming.
- **Habilidades Relevants": conhecimento de métricas de pegada de carbono de software, ferramentas de medição de eficiência energética em data centers e nuvem, compreensão de como diferentes arquiteturas afetam consumo de recursos (ex: arquiteturas event-driven vs polling, eficiência de funções serverless vs containers sempre ligados).

## Resumo

A migração de arquitetura é uma disciplina crítica para organizações que buscam modernizar seus sistemas de software, reduzir dívida técnica, melhorar qualidades não-funcionais e manter competitividade em um ambiente tecnológico em constante evolução. Ao contrário do desenvolvimento de sistemas verdes, onde se começa do zero, a migração de arquitetura exige compreensão profunda do existente, respeito pelas limitações operacionais e uma mentalidade de transformação cuidadosa que minimize riscos enquanto entrega valor continuamente.

As técnicas descritas — desde o padrão Strangler Fig aplicado à arquitetura, migração por camada, camadas de anticorrupção, replatforming com containers, até abordagens orientadas por API e rebuilding completo — fornecem um arsenal de abordagens que podem ser combinadas e adaptadas conforme o contexto específico do sistema legado, os objetivos de negócio e as restrições técnicas. O checklist fornecido guia arquitetos e equipes desde a avaliação inicial até a validação pós-migração, garantindo que aspectos críticos como definição de objetivos, mapeamento do existente, análise de riscos, escolha de estratégia adequada, execução cuidadosa e evolução baseada em feedback não sejam negligenciados.

Os estudos de caso ilustraram como essas técnicas se aplicam em cenários reais de diferentes indústrias e tipos de sistemas legados, mostrando tanto os benefícios imediatos (melhoria de desempenho, redução de custos, aumento da agilidade) quanto os aprendizados sobre o que funciona na prática. Eles destacaram a importância de começar com incrementos de baixo risco e alto valor, investir em plataforma de integração e observabilidade, e a necessidade de períodos de transição com mecanismos de fallback e validação paralela para sistemas de alta criticalidade.

Finalmente, ao observar tendências futuras como migração guiada por observabilidade, plataformas de migração automatizada, arquiteturas baseadas em intents, padrões para sistemas nativos de IA e considerações de sustentabilidade, o profissional de arquitetura se prepara não apenas para lidar com os desafios de migração de hoje, mas também para moldar as práticas que definirão como organizações abordam a modernização de seus ativos tecnológicos críticos nos próximos anos.

Lembre-se: a migração de arquitetura não é um projeto com início, meio e fim definidos, mas uma capacidade organizacional que deve ser cultivada continuamente. Cada migração bem-sucedida constrói confiança, gera aprendizado e estabelece a base para o próximo passo de evolução. O verdadeiro mérito está em estabelecer um ritmo sustentável de melhoria que alinhe a arquitetura às necessidades do negócio sem comprometer a estabilidade ou a velocidade de inovação. Seja através de modernização incremental, replatforming cuidadoso ou rebuilding bem planejado, cada decisão arquitetural nesse contexto deve ser feita com consciência do impacto no negócio, respeito pelas limitações existentes e visão clara dos possíveis caminhos à frente.
