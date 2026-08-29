---
trilha: "PARA ENTREVISTAS"
---
**Navegação:** [[MOC — TRILHA PARA ENTREVISTAS]]
← [[PARTE 79 — PROJETOS PRÁTICOS]] | #trilha/entrevistas | [[PARTE 81 — ARQUITETURA PARA SISTEMAS LEGADOS]] →

---
# PARTE 80 — ARQUITETURA PARA SISTEMAS LEGADOS

## Fundamentos

Sistemas legados são aqueles que foram desenvolvidos há muito tempo, muitas vezes com tecnologias ou práticas que hoje são consideradas obsoletas, mas que ainda são críticos para as operações do negócio. Eles podem ser difíceis de manter, escalar ou integrar com novos sistemas devido à falta de documentação, dependência de tecnologias antigas ou arquiteturas rígidas. Arquitetar para sistemas legados envolve entender suas limitações, planejar estratégias de evolução e garantir que eles possam continuar entregando valor enquanto se preparam para futuras modernizações.

### Características de Sistemas Legados

1. **Tecnologia Obsoleta**: uso de linguagens, frameworks ou plataformas que não são mais suportadas ou têm comunidade reduzida.
2. **Falta de Documentação**: documentação insuficiente, desatualizada ou inexistente, tornando difícil entender o funcionamento do sistema.
3. **Arquitetura Rígida**: baixa modularidade, alto acoplamento e dificuldade de fazer alterações sem afetar outras partes do sistema.
4. **Dívida Técnica Acumulada**: anos de atalhos, patches e soluções rápidas que tornaram o código complexo e frágil.
5. **Integração Limitada**: dificuldade de se comunicar com sistemas modernos devido à falta de APIs padronizadas ou uso de protocolos proprietários.
6. **Dependência de Pessoas-Chave**: conhecimento do sistema concentrado em poucos indivíduos, criando risco operacional.
7. **Criticalidade para o Negócio**: apesar dos problemas, o sistema é essencial para operações diárias, tornando riscos de mudança elevados.

### Objetivos da Arquitetura para Sistemas Legados

1. **Continuidade de Operações**: garantir que o sistema continue funcionando de forma confiável enquanto mudanças são planejadas.
2. **Redução de Riscos**: identificar e mitigar pontos de falha, gargalos de desempenho e vulnerabilidades de segurança.
3. **Facilitação da Manutenção**: tornar o sistema mais fácil de entender, depurar e modificar.
4. **Preparação para Evolução**: estabelecer fundamentos que permitam futuras modernizações, seja por replatformação, refatoração ou substituição.
5. **Integração com Novos Sistemas**: permitir que o legado se comunique com aplicações modernas através de APIs, serviços ou mecanismos de troca de dados.
6. **Observabilidade e Monitoramento**: instrumentar o sistema para entender seu comportamento, desempenho e uso.

### Abordagens Gerais

- **Encapsulamento**: isolar o legado atrás de uma camada de abstração (como uma API ou serviço) para protegê-lo de mudanças diretas e permitir interação controlada.
- **Expansão Gradual**: adicionar nova funcionalidade fora do legado, gradualmente reduzindo sua responsabilidade.
- **Refatoração Controlada**: melhorar o código do legado em pequenos incrementos, preservando seu comportamento externo.
- **Substituição Incremental (Strangler Fig)**: substituir partes do legado por novas implementações, redirecionando tráfego à medida que novos componentes ficam prontos.
- **Manutenção com Melhorias**: continuar mantendo o legado enquanto se introduzem melhorias em áreas específicas (ex: banco de dados, interface do usuário).
- **Desativação Planejada**: quando a substituição for viável, planejar a aposentadoria do legado com migração de dados e treinamento de usuários.

## Técnicas

### Técnica 1: Camada de Anticorrupção (Anti-Corruption Layer)

- **Descrição**: criar uma camada que traduz entre o modelo de dados e as interfaces do legado e o modelo desejado pelos novos sistemas, evitando que as limitações do legado "corrompam" o novo desenvolvimento.
- **Passos**:
  1. Definir o modelo de domínio ou contrato desejado para a integração.
  2. Implementar tradutores que convertem chamadas e dados do legado para o novo modelo (e vice-versa).
  3. Novos sistemas se comunicam apenas com a camada de anticorrupção, não diretamente com o legado.
  4. A camada pode incluir lógica de validação, enriquecimento ou adaptação de protocolos.
- **Benefícios":
  - Isola novos sistemas das particularidades e limitações do legado.
  - Permite evolução independente do modelo de domínio dos novos sistemas.
  - Facilita testes e substituição futura do legado.
- **Desafios":
  - Overhead de tradução pode afetar desempenho.
  - Requer manutenção contínua à medida que ambos os sistemas evoluem.
  - Pode se tornar um gargalo se não for bem projetada.

### Técnica 2: Fachada (Facade) ou API de Integração

- **Descrição**: expor uma interface simplificada e bem definida (geralmente uma API REST, gRPC ou mensageria) que encapsula a complexidade do legado, permitindo que outros sistemas o utilizem sem precisar conhecer seus detalhes internos.
- **Passos**:
  1. Identificar as funcionalidades do legado que precisam ser expostas.
  2. Definir contratos de API claros, versionados e documentados.
  3. Implementar um serviço ou módulo que recebe requisições da API, chama o legado apropriadamente e retorna resultados no formato esperado.
  4. Tratar erros, timeouts e conversões de dados na camada da fachada.
  5. Implementar segurança, rate limiting e monitoramento na API.
- **Beneficios":
  - Fornece uma interface estável mesmo que o interno do legado mude.
  - Permite uso de protocolos modernos (HTTP/JSON, gRPC) com sistemas que só entendem legados (ex: SOAP, corba, chamadas diretas ao banco).
  - Facilita governança, versionamento e controle de acesso.
- **Desafios":
  - Pode mascarar problemas de desempenho do legado se não houver otimização subjacente.
  - Requer desenvolvimento e manutenção da camada da fachada.
  - Pode introduzir latência adicional.

### Técnica 3: Espelhamento de Dados e Sincronização (Data Mirroring and Synchronization)

- **Descrição**: manter cópias de dados do legado em sistemas mais modernos (como um data warehouse, banco de dados NoSQL ou plataforma de eventos) para permitir consultas, análises e integração sem sobrecarregar o legado operativo.
- **Passos**:
  1. Identificar quais dados são necessários para integração ou análise.
  2. Estabelecer mecanismos de captura de alterações (change data capture - CDC) do legado (logs de transação, triggers, ou polling).
  3. Transportar alterações para um sistema de destino usando filas, streaming (Kafka, Kinesis) ou batch.
  4. Aplicar transformações, limpeza e modelagem dos dados no destino.
  5. Garantir consistência eventual ou exatamente-once conforme necessário.
  6. Fornecer acesso aos dados espelhados através de APIs, ferramentas de BI ou plataformas de análise.
- **Beneficios**:
  - Reduz carga de consulta no legado operativo.
  - Permite uso de tecnologias modernas para análise e relatórios.
  - Facilita integração com sistemas que necessitam de acesso rápido a dados.
  - Pode servir como base para migração futura.
- **Desafios**:
  - Complexidade de implementar CDC confiável, especialmente em legados sem suporte nativo.
  - Latência entre alteração no legado e disponibilidade no espelho.
  - Necessidade de gerenciamento de esquemas e evolução de dados.
  - Custo de armazenamento e processamento adicional.

### Técnica 4: Rehosting (Lift and Shift) com Otimização Seletiva

- **Descrição": migrar o legado para uma infraestrutura mais moderna (como máquinas virtuais na nuvem ou containers) sem alterar seu código, inicialmente focando em melhorar operabilidade, escalabilidade e resiliência, depois otimizando partes críticas.
- **Passos":
  1. Avaliar dependências de hardware, sistema operacional e middleware do legado.
  2. Escolher uma plataforma de destino compatível (ex: VMware, Hyper-V, AWS EC2, Azure VMs, ou containers se o legado puder ser empacotado).
  3. Migrar o ambiente inteiro (sistema operacional, aplicações, dependências) para a nova plataforma.
  4. Testar funcionalidade e desempenho após a migração.
  5. Introduzir melhorias operacionais: backup automático, monitoramento, patching de OS.
  6. Identificar gargalos de desempenho e otimizar seletivamente (ex: mover banco de dados para uma instância dedicada, adicionar cache).
  7. Planejar próximos passos: refatoração, replatformação ou substituição.
- **Beneficios":
  - Reduz riscos associados a hardware envelhecido ou data centers obsoletos.
  - Melhora disponibilidade através de recursos de nuvem (auto-scaling, múltiplas zonas de disponibilidade).
  - Facilita gerenciamento através de ferramentas modernas de provisionamento e orquestração.
  - Pode reduzir custos de infraestrutura física e manutenção de data center.
- **Desafios":
  - Não resolve problemas intrinsicos de arquitetura ou qualidade de código.
  - Licenciamento de software pode ser um obstáculo em ambientes de nuvem.
  - Pode haver necessidade de rearquitetamento para aproveitar totalmente benefícios da nuvem (ex: arquitetura nativa de nuvem).
  - Dependências específicas de hardware (dongles, placas especiais) podem impedir a migração.

### Técnica 5: Refatoração Estranguladora (Strangler Refactoring)

- **Descrição": aplicar o padrão Strangler Fig no nível de código dentro do mesmo sistema, gradualmente substituindo módulos ou componentes legados por novas implementações dentro dos mesmos limites de processo ou aplicação.
- **Passos":
  1. Identificar um componente bem delimitado do legado (ex: um módulo de cálculo de impostos, um serviço de geração de relatórios).
  2. Definir uma interface clara para esse componente (entrada, saída, comportamento assíncrono se aplicável).
  3. Implementar uma nova versão do componente seguindo práticas modernas (melhor estrutura de código, testes automatizados, etc.).
  4. Usar alternadores de funcionalidade ou roteamento para direcionar chamadas para a nova implementação quando disponível.
  5. Gradualmente aumentar o uso da nova implementação até que ela substitua completamente a antiga.
  6. Remover o código legado correspondente.
- **Beneficios":
  - Permite melhoria incremental sem exigir reescrita total.
  - Reduz risco ao permitir comparação direta entre antigo e novo.
  - Facilita aprendizado e adoção de novas práticas pela equipe.
  - Mantém o sistema em execução durante todo o processo.
- **Desafios":
  - Requer que o componente seja suficientemente desacoplado para ser substituído isoladamente.
  - Pode haver dependências ocultas que dificultam a isolamento.
  - A nova implementação deve ser meticulosamente testada para garantir compatibilidade comportamental.

### Técnica 6: Embrulho com Scripts ou Adaptadores (Wrapping with Scripts or Adapters)

- **Descrição": usar scripts, adaptadores ou ferramentas de automação para interagir com o legado de maneira mais confiável e padronizada, especialmente quando o legado só oferece interfaces manuais ou proprietárias.
- **Passos":
  1. Identificar tarefas repetitivas ou pontos de integração que sejam manuais ou frágeis (ex: entrada de dados via tela, geração de relatórios via job batch, extração de logs).
  2. Automatizar essas tarefas usando scripts (PowerShell, Bash, Python) ou ferramentas de automação de UI (Selenium, AutoIt) se necessário.
  3. Padronizar entradas e saídas (ex: aceitar arquivos CSV, JSON ou chamadas de API simples).
  4. Tratar erros, timeouts e condições de borda nos scripts.
  5. Expor a automatização através de uma interface simples (pasta monitorada, API REST leve, fila de mensagens).
  6. Integrar essa camada automatizada com novos sistemas ou processos.
- **Beneficios":
  - Torna possível integração com legados que não têm APIs ou pontos de conexão adequados.
  - Reduz erro humano e aumenta confiabilidade de operações manuais.
  - Fornece logs e rastreabilidade para tarefas que antes eram opacas.
  - Pode ser implementado rapidamente com baixo custo inicial.
- **Desafios":
  - Fragilidade se depender de automacação de UI (mudanças na interface quebram scripts).
  - Sobrecarga de manutenção de scripts à medida que o legado evolui.
  - Limitações de desempenho e escalabilidade em comparação com integrações nativas.
  - Necessidade de ambientes adequados para execução (estações de trabalho, sessões de terminal).

### Técnica 7: Estrangulamento por Banco de Dados (Database Strangling)

- **Descrição": ao invés de substituir todo o sistema legado, focar primeiro em modernizar a camada de dados, migrando para um banco de dados mais moderno enquanto mantém as aplicações existentes (ou gradualmente as atualizando).
- **Passos":
  1. Avaliar o uso do banco de dados legado (consultas, transações, volume de dados).
  2. Escolher um banco de dados destino moderno (ex: migrar de Oracle para PostgreSQL, de SQL Server para MySQL, de mainframe para um banco relacional distribuído).
  3. Implementar camada de tradução ou replicação que sincroniza dados entre o legado e o novo banco (pode ser bidirecional durante a transição).
  4. Migrar gradualmente leituras para o novo banco, monitorando desempenho e consistência.
  5. Após estabilizar leituras, migrar escritas para o novo banco (usando padrões como dual-write ou evento de atualização).
  6. Uma vez que todas as aplicações estejam usando o novo banco, desativar o legado.
  7. Opcionalmente, refatorar aplicações para aproveitar recursos do novo banco (melhores índices, particionamento, etc.).
- **Beneficios":
  - Melhora desempenho, escalabilidade e custos de licenciamento frequentemente associados a bancos de dados legados.
  - Permite uso de ferramentas modernas de administração, backup e análise.
  - Reduz risco ao migrar camada por camada em vez de tudo de uma vez.
  - Fornece base para futuras modernizações de aplicação.
- **Desafios":
  - Complexidade de garantir consistência durante a sincronização bidirecional.
  - Diferenças em recursos, SQL e comportamentos entre bancos de dados legados e modernos.
  - Necessidade de tratamento de tipos de dados especiais (LOBs, tipos proprietários).
  - Latência adicional durante períodos de duplicação de escrita.

### Técnica 8: Virtualização ou Emulação de Ambiente Legado

- **Descrição": quando o legado depende de hardware específico, sistema operacional antigo ou software que não roda em plataformas modernas, usar tecnologias de virtualização, emulação ou containers para preservar seu ambiente de execução enquanto melhora gerenciamento, backup e recuperação de desastre.
- **Passos":
  1. Documentar o ambiente de execução necessário (hardware, SO, dependências, drivers).
  2. Criar uma imagem virtual (VMware, VirtualBox, Hyper-V) ou container que reproduza esse ambiente.
  3. Migrar o legado para executar dentro dessa imagem virtual.
  4. Gerenciar a imagem virtual usando ferramentas modernas (backup automático, snapshots, monitoramento de recursos).
  5. Fornecer acesso através de redes virtuais, publicação de serviços ou APIs de gerenciamento.
  6. Planejar estratégias de longo prazo: atualização do legado dentro do ambiente virtual ou planejamento de substituição eventual.
- **Beneficios":
  - Preserva funcionalidade do legado mesmo quando o hardware original falha ou se torna indisponível.
  - Melhora recuperação de desastre através de facilidade de clonagem e distribuição de imagens.
  - Permite isolamento e testagem segura (ex: testar patches em uma cópia da VM).
  - Facilita auditoria e conformidade através de controle de ambiente.
- **Desafios":
  - Pode não resolver problemas de desempenho se o legado for ineficiente por natureza.
  - Licenciamento de software pode não permitir virtualização em certos casos.
  - Overhead de virtualização (geralmente pequeno, mas pode ser significativo em cargas de trabalho específicas).
  - Ainda depende da manutenção do legado dentro do ambiente virtualizado.

## Checklist

### Avaliação Inicial do Sistema Legado

- [ ] Documentar a finalidade do sistema e sua criticalidade para o negócio.
- [ ] Mapear tecnologias utilizadas (linguagens, frameworks, bancos de dados, middleware, sistemas operacionais).
- [ ] Identificar pontos de integração existentes (APIs, tabelas de banco de dados compartilhadas, arquivos trocados, protocolos de comunicação).
- [ ] Avaliar a qualidade e disponibilidade de documentação (código, arquitetura, operações).
- [ ] Identificar pessoas-chave com conhecimento do sistema e níveis de dependência.
- [ ] Avaliar desempenho atual (tempos de resposta, throughput, taxas de erro).
- [ ] Identificar problemas conhecidos (falhas frequentes, gargalos de desempenho, limitações de escalabilidade).
- [ ] Avaliar riscos de segurança (vulnerabilidades conhecidas, falta de patches, exposição indevida).
- [ ] Determinar requisitos de conformidade ou regulatórios que o sistema deve atender.
- [ ] Entender o cronograma de vida útil anunciado pelo fornecedor (se aplicável) ou data de fim de suporte.

### Planejamento da Estratégia de Arquitetura

- [ ] Definir objetivos claros (ex: manter operacional por X anos, reduzir custos de manutenção, preparar para substituição, melhorar integração).
- [ ] Escolher uma ou mais abordagens de arquitetura (encapsulamento, strangler fig, camada de anticorrupção, etc.).
- [ ] Estabelecer métricas de sucesso (KPIs) para medir o impacto das mudanças.
- [ ] Avaliar custos e benefícios de cada abordagem considerada.
- [ ] Identificar restrições (orçamento, prazo, disponibilidade de habilidades, limitações contratuais).
- [ ] Planejar fases de implementação com marcos claros e critérios de aceitação.
- [ ] Definir estratégias de mitigação de riscos (rollback, testes em ambiente de homologação, monitoramento intensivo).
- [ ] Estabelecer processos de governança e aprovação para mudanças arquiteturais.
- [ ] Planejar treinamento e transferência de conhecimento para a equipe.
- [ ] Definir critérios para desativação ou substituição final do legado.

### Implementação de Técnicas de Arquitetura

- [ ] Implementar camadas de abstração (fachadas, APIs, camadas de anticorrupção) conforme planejado.
- [ ] Configurar mecanismos de observabilidade (logs, métricas, tracing) se ainda não existirem.
- [ ] Estabelecer processos de backup e recuperação de desastre adequados à criticalidade do sistema.
- [ ] Implementar segurança adequada (autenticação, autorização, criptografia em trânsito e em repouso).
- [ ] Configurar gerenciamento de mudanças e versionamento para qualquer novo componente introduzido.
- [ ] Documentar interfaces, contratos e procedimentos de operação para novos elementos arquiteturais.
- [ ] Testar funcionalidade integrada em ambiente de homologação antes da produção.
- [ ] Implementar monitoramento de uso e desempenho para validar eficácia das mudanças.
- [ ] Planejar e executar atividades de transferência de conhecimento e treinamento.
- [ ] Revisar e atualizar documentação arquitetural conforme o sistema evolui.

### Monitoramento e Evolução Contínua

- [ ] Monitorar métricas-chave de desempenho, disponibilidade e uso continuamente.
- [ ] Coletar feedback de usuários, equipe de operações e partes interessadas de negócio.
- [ ] Revisar periodicamente a arquitetura e eficácia das técnicas aplicadas.
- [ ] Identificar novas oportunidades de melhoria ou modernização com base no aprendizado.
- [ ] Atualizar planos de longo prazo conforme mudanças no negócio, tecnologia ou ambiente regulatório.
- [ ] Gerenciar dívida técnica introduzida durante o processo de arquitetura (ex: código duplicado, adaptações temporárias).
- [ ] Planejar a próxima fase de evolução ou entrar em modo de manutenção com melhoria contínua.
- [ ] Documentar lições aprendidas para referência futura em outros sistemas legados.

## Estudos de Caso

### Caso 1: Modernização de um Sistema de Folha de Pagamento Legado na Indústria Manufatureira

- **Contexto": uma grande empresa manufatureira tinha um sistema de folha de pagamento desenvolvido em COBOL executando em um mainframe IBM que processava pagamentos para mais de 10.000 funcionários mensalmente. O sistema era crítico, mas enfrentava desafios de custos de licenciamento alto, dificuldade de encontrar programadores COBOL e integração limitada com novos sistemas de RH baseados em nuvem.
- **Objetivo da Arquitetura": reduzir custos operacionais, melhorar capacidade de integração com sistemas de RH modernos e mitigar risco de indisponibilidade devido à escassez de habilidades, mantendo a precisão e confiabilidade do processamento de folha de pagamento.
- **Abordagem Adotada":
  1. Implementou uma camada de anticorrupção usando middleware de integração (MuleSoft) que expõe uma API REST para operações de folha de pagamento (consulta de salários, lançamento de vencimentos, processamento de pagamentos).
  2. A API traduz entre o modelo de dados moderno usado pelos sistemas de RH e os formatos proprietários necessários para interagir com o sistema COBOL através de chamadas de tela automatizadas e acessos a arquivos de troca.
  3. Implementou espelhamento de dados em tempo real usando change data capture do banco de dados DB2 do mainframe para um data warehouse PostgreSQL, permitindo que análises e relatórios fossem executados sem sobrecarregar o mainframe.
  4. Virtualizou o ambiente do mainframe em uma plataforma LPAR moderna, melhorando recuperação de desastre e reduzindo dependência de hardware específico.
  5. Estabeleceu um processo de dupla escrita durante um período de transição onde novos lançamentos de vencimentos iam tanto para o sistema COBOL legado quanto para um novo módulo de cálculo de folha de pagamento em Java (como backup e para validação).
  6. Após seis meses de validação paralela, transacionou gradualmente o processamento de folha de pagamento para o novo módulo Java, mantendo o COBOL como fallback por mais três meses antes da desativação final.
- **Desafios Enfrentados":
  - Latência nas chamadas de tela automatizadas foi mitigada com otimização de scripts e uso de conexões persistentes.
  - Diferenças em regras de cálculo de benefícios entre o legado e a nova implementação exigiram reconciliação cuidadosa e ajuste de algoritmos.
  - Complexidade de gerenciar chaves de criptografia e segredos em ambientes distribuídos foi resolvida usando um cofre de senhas centralizado (HashiCorp Vault).
- **Resultados":
  - Redução de 40% nos custos anuais de licenciamento e manutenção do mainframe após a transição completa.
  - Tempo de entrega de relatórios gerenciais reduziu de dias para minutos graças ao acesso direto ao data warehouse espelhado.
  - Novos sistemas de RH puderam se integrar facilmente através da API REST bem documentada.
  - A empresa desenvolveu capacidade interna em Java e modernização, reduzindo dependência de consultores especializados em legado.
- **Lições Aprendidas":
  - Uma camada de anticorrupção bem projetada pode permitir integração sem exigir mudanças imediatas no legado.
  - O espelhamento de dados fornece valor imediato mesmo antes de concluir a modernização completa da aplicação.
  - Períodos de validação paralela são essenciais para sistemas de alta criticalidade como folha de pagamento.
  - Investir em plataforma de integração e ferramentas de gerenciamento paga dividendos em flexibilidade e redução de risco a longo prazo.

### Caso 2: Arquitetura para um Sistema de Gerenciamento de Estoque Legado no Varejo

- **Contexto": uma rede varejista regional tinha um sistema de gerenciamento de estoque desenvolvido em Visual Basic 6.0 com banco de dados Microsoft Access que rastreava níveis de estoque em 50 lojas e dois centros de distribuição. O sistema era crítico para operações diárias, mas enfrentava limitações graves de escalabilidade (travava com mais de 10 usuários simultâneos), falta de suporte do fornecedor (VB6 não é mais suportado) e impossibilidade de integrar com plataformas de e-commerce modernos.
- **Objetivo da Arquitetura": permitir que o sistema continuasse operando enquanto se estabelecia a base para substituição por uma solução moderna de gerenciamento de estoque baseada em nuvem, melhorando imediatamente a confiabilidade e preparando o terreno para transição futura.
- **Abordagem Adotada":
  1. Implementou uma fachada de API usando ASP.NET Core que expõe operações de estoque (consulta de níveis, lançamento de entradas/saídas, transferências entre locais).
  2. A fachada comunica-se com o legado VB6/Access através de chamadas COM automatizadas e acesso direto ao arquivo .mdb quando necessário, tratando bloqueios e timeouts adequadamente.
  3. Introduziu um banco de dados SQL Server como camada de dados espelhada, usando um serviço de sincronização que lê alterações do Access via triggers e polling frequente, mantendo o SQL Server como fonte primária para novas operações sempre que possível.
  4. Virtualizou as estações de trabalho executando o legado VB6 em ambientes Windows 7 dentro de máquinas virtuais Hyper-V, melhorando backup, recuperação de desastre e permitindo que múltiplas instâncias fossem executadas simultaneamente para testes.
  5. Implementou um sistema de filas (RabbitMQ) para desacoplar operações de alta volume (como processamento de recebimentos de carga) do legado, permitindo que picos de trabalho fossem processados assincronamente.
  6. À medida que a confiança no novo sistema crescia, migrou gradualmente funcionalidades da camada de API para implementações diretas em SQL Server/ASP.NET Core, reduzindo a dependência do legado.
  7. Após 18 meses de operação paralela, desativou o legado VB6/Access completamente, mantendo o acesso apenas para consulta de dados históricos através do espelhamento em SQL Server.
- **Desafios Enfrentados":
  - Bloqueios de banco de dados no Access foram mitigados implementando um sistema de fila de operações com retry e backoff exponencial.
  - Diferenças de comportamento entre Access e SQL Server (tratamento de nulos, tipos de dados) exigiram mapeamento cuidadoso e validação de dados durante a sincronização.
  - Performance inicial da fachada foi melhorada com caching agressivo de dados de referência relativamente estáticos (informações de lojas, produtos).
- **Resultados":
  - O sistema passou de suportar no máximo 10 usuários simultâneos para lidar com conforto com mais de 100 usuários através da nova arquitetura.
  - Tempo de resposta médio para consultas de estoque reduziu de segundos para dezenas de milissegundos.
  - Integração com plataformas de e-commerce tornou-se possível através da API REST bem definida.
  - Custos de licenciamento foram eliminados ao migrar para tecnologias livres ou já licenciadas pela empresa (SQL Server, ASP.NET Core).
  - A equipe de TI desenvolveu habilidades em tecnologias modernas enquanto mantinha o sistema legado operacional.
- **Lições Aprendidas":
  - Espelhamento de dados pode servir como ponte eficaz para transição, permitindo que novas aplicações sejam desenvolvidas e testadas enquanto o legado ainda está operacional.
  - Virtualização de estações de trabalho legadas melhora significativamente gerenciamento e disponibilidade.
  - Desacoplar operações de alta volume através de filas protege o legado de sobrecarga e melhora resposta do sistema como um todo.
  - Uma abordagem faseada, onde novas funcionalidades vão diretamente para a arquitetura moderna enquanto o legado é mantido para compatibilidade, pode ser mais eficaz do que tentativa de reescrita total imediata.

### Caso 3: Estrangulamento de um Sistema de Troca de Mensagens Legado na Indústria Financeira

- **Contexto": uma corretora de valores tinha um sistema legado de troca de mensagens financeiras (basedo em protocolos proprietários e arquivos trocados via FTP) que processava negociações, confirmações e liquidações para suas operações de mesa de câmbio. O sistema era crítico para operações em tempo real, mas tinha arquitetura monolítica difícil de escalar, falta de visibilidade em tempo real e integração limitada com novos sistemas de gestão de risco e compliance baseados em eventos.
- **Objetivo da Arquitetura": melhorar desempenho, visibilidade em tempo real e capacidade de integração com sistemas modernos de risco e compliance, enquanto mantinha a confiabilidade e precisão do processamento de mensagens financeiras críticas.
- **Abordagem Adotada":
  1. Identificou os diferentes tipos de mensagens (MT103 para transferências, MT202 para liquidações bancárias, etc.) e seus fluxos de processamento.
  2. Implementou um sistema de ingestão de mensagens usando Apache Kafka como log distribuído de eventos, com adaptadores que convertem os formatos legados (arquivos FIX, arquivos posicionalmente definidos) para eventos padronizados em tópicos Kafka específicos por tipo de mensagem.
  3. Criou microservices de processamento especializados que consomem os tópicos Kafka relevantes, executam validações, enriquecimento e roteamento apropriado, publicando resultados em outros tópicos para downstream.
  4. Construiu uma camada de compatibilidade que mantém a interface legado para contrapartes que ainda não puderam migrar imediatamente, traduzindo entre eventos Kafka e os formatos de mensagem esperados pelos sistemas externoslegados.
  5. Implementou um sistema de dashboard em tempo real usando Kafka Streams e uma aplicação React que consome os tópicos de processamento para mostrar status, métricas de throughput e alertas de anomalia.
  6. Gradualmente moveu contrapartes de trading para usar a nova interface baseada em eventos, mantendo o legado como fallback para aquelas que não puderam migrar imediatamente.
  7. Após validar que o novo sistema processava mensagens com latência menor e maior confiabilidade, desativou o legado ponto a ponto, mantendo apenas o adaptador de ingestão para fontes que ainda não puderam ser atualizadas.
- **Desafios Enfrentados":
  - Garantia de ordem e exatamente-once no processamento de mensagens financeiras exigiu uso de chaves de particionamento adequadas no Kafka e estratégias de idempotência nos microservices.
  - Latência inicial na ingestão de arquivos legados foi melhorada com monitoramento de diretórios em tempo real e processamento em paralelo.
  - Complexidade de gerenciar esquemas de eventos em evolução foi abordada usando Schema Registry e versionamento cuidadoso.
- **Resultados":
  - Latência de processamento de mensagens críticas reduziu de segundos para dezenas de milissegundos.
  - Visibilidade em tempo real permitió detecção precoce de problemas operacionais e melhoria na tomada de decisão de trading.
  - Novos sistemas de risco e compliance puderam se integrar facilmente consumindo diretamente os fluxos de eventos relevantes.
  - A arquitetura de eventos permitiu fácil adição de novos tipos de mensagem e rotas de processamento sem modificar o núcleo do sistema.
  - Capacidade de processamento escalou horizontalmente simplesmente adicionando mais instâncias dos microservices de processamento.
- **Lições Aprendidas":
  - Uma plataforma de eventos (como Kafka) pode servir como espinha dorsal eficaz para modernizar sistemas de troca de mensagens legados.
  - Separar preocupações de ingestão, processamento e roteamento permite evolução independente de cada camada.
  - Investir em observabilidade em tempo real desde o início fornece valor imediato e guiia decisões de arquitetura subsequentes.
  - Manter compatibilidade com contrapartes externas durante a transição é essencial para sistemas que operam em ecossistemas maiores.

## Tendências Futuras

### 1. Arquiteturas de Modernização Guiada por IA

- **Descrição": uso de modelos de aprendizado de máquina para analisar código legado, identificar pontos de alta acoplamento, sugerir limites de contexto para microsserviços e priorizar áreas de refatoração com base em impacto de negócio e complexidade técnica.
- **Impacto": arquitetos passarão a usar copilotos para entender rapidamente grandes bases de código legado, gerar diagramas arquiteturais preliminares e receber sugestões de padrões de estrangulamento ou camadas de anticorrupção.
- **Habilidades Relevantes": avaliação crítica de sugestões de IA, compreensão de limites de análise estática em contextos legados, integração de recomendações em processos de revisão de arquitetura.

### 2. Plataformas de Modernização Legado como Serviço (LMaaS - Legacy Modernization as a Service)

- **Descrição": plataformas integradas que combinam ferramentas de análise de código legado, transformação automática ou semi-automática, geração de APIs, teste de compatibilidade e gerenciamento de dados em uma oferta coesa para acelerar projetos de modernização.
- **Impacto": equipes podem focar na definição de estratégias de negócio e validação de resultados em vez de construir do zero a infraestrutura técnica de modernização.
- **Habilidades Relevants": compreensão de plataformas de transformação de código (como aquelas baseadas em grammars e reescrita), gerenciamento de dados em migrações heterogêneas, orquestração de workflows complexos de modernização.

### 3. Estrangulamento Seletivo com Funções Serverless

- **Descrição": ao invés de substituir todo o legado por microsserviços tradicionais, usar funções serverless (AWS Lambda, Azure Functions) para implementar novos pontos de integração ou substituir funcionalidades específicas do legado, beneficiando-se do escalonamento automático e menor sobrecarga operacional.
- **Impacto": arquitetos precisarão projetar fronteiras claras entre legado e funções serverless, gerenciar estado e conexões de forma apropriada e considerar padrões de invocação e custos em modelos de mistura.
- **Habilidades Relevants": conhecimento de plataformas de funções serverless, padrões de integração (event-driven, síncrono via API gateway), gerenciamento de estado em funções stateless e otimização de custos baseada em padrões de uso.

### 4. Arquiteturas de Integração Híbrida para Períodos de Transição Prolongados

- **Descrição": reconhecendo que algumas modernizações podem levar anos devido à complexidade, criticalidade ou restrições de negócio, projetar arquiteturas de integração que são projetadas para durar, com versãoamento explícito, contratos de longo prazo e mecanismos de evolução controlada embutidos.
- **Impacto": a tarefa do arquiteto muda de projetar uma ponte temporária para projetar uma via de longo prazo que suporte coexistência segura e evolucionária entre legado e moderno.
- **Habilidades Relevants": modelagem de contratos de integração de longo prazo, projeto de mecanismos de versão e depreciação, compreensão de padrões de governança para arquiteturas de transição estendida.

### 5. Modernização Legado com Foco em Sustentabilidade e Eficiência Energética

- **Descrição": considerar o consumo de energia e pegada de carbono como critérios na escolha de estratégias de modernização legada, favorecendo abordagens que reduzam recursos computacionais, otimizem uso de infraestrutura ou migrem para plataformas mais eficientes energeticamente.
- **Impacto": arquitetos precisarão incluir métricas de eficiência energética (como PUE ajustado para carga de trabalho) em avaliações de trade-offs junto com custo, desempenho e segurança ao escolher entre rehosting, refatoração ou substituição.
- **Habilidades Relevants": conhecimento de métricas de pegada de carbono de software, ferramentas de medição de eficiência energética em data centers e nuvem, compreensão de como diferentes arquiteturas afetam consumo de recursos.

## Resumo

Arquitetar para sistemas legados é uma disciplina crítica que equilibra a necessidade de preservar o valor de investimentos existentes com a pressão para adotar novas tecnologias, melhorar qualidades não-funcionais e reduzir riscos associados a tecnologias obsoletas. Ao contrário da arquitetura de sistemas verdes, onde se começa do zero, arquitetar para legados exige compreensão profunda do existente, empatia com as limitações operacionais e uma mentalidade de evolução incremental que minimize riscos enquanto entrega valor continuamente.

As técnicas descritas — desde camadas de anticorrupção e fachadas até espelhamento de dados, rehosting e strangler fig aplicado em diferentes níveis — fornecem um arsenal de abordagens que podem ser combinadas e adaptadas conforme o contexto específico do legado, os objetivos de negócio e as restrições técnicas. O checklist fornecido guia arquitetos e equipes desde a avaliação inicial até o monitoramento contínuo, garantindo que aspectos críticos como compreensão do existente, planejamento de riscos, implementação de técnicas adequadas e evolução baseada em feedback não sejam negligenciados.

Os estudos de caso ilustraram como essas técnicas se aplicam em cenários reais de diferentes indústrias e tecnologias legadas, mostrando tanto os benefícios imediatos (melhoria de desempenho, redução de custos, capacidade de integração) quanto os aprendizados sobre o que funciona na prática. Eles destacaram a importância de validação paralela, investimento em plataforma de integração e observabilidade, e a necessidade de abordagens faseadas que respeitem a criticalidade do sistema.

Finalmente, ao observar tendências futuras como modernização guiada por IA, plataformas de legado como serviço, funções serverless para estrangulamento seletivo, arquiteturas de integração híbrida de longo prazo e considerações de sustentabilidade, o profissional de arquitetura se prepara não apenas para lidar com os sistemas legados de hoje, mas também para moldar as práticas que definirão como organizações abordam a modernização de seus ativos tecnológicos críticos nos próximos anos.

Lembre-se: sistemas legados não são apenas problemas a serem resolvidos, mas muitas vezes representam valor de negócio acumulado que merece respeito e cuidadosa evolução. O verdadeiro mérito em arquitetar para legados está em estabelecer um caminho que honre esse valor enquanto o prepara para o futuro — seja através de modernização completa, integração de longo prazo ou substituição bem planejada. Cada decisão arquitetural nesse contexto deve ser feita com consciência do impacto no negócio, respeito pelas limitações existentes e visão clara dos possíveis caminhos à frente.
