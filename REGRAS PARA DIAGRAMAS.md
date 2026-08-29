# REGRAS PARA DIAGRAMAS

Esta parte estabelece diretrizes para a criação de diagramas na documentação de arquitetura de software, garantindo que eles sejam claros, consistentes, úteis e mantíveis.

## Fundamentos

### Por que ter regras para diagramas?
- **Comunicação eficaz**: Diagramas bem projetados transmitem informações complexas de forma rápida e intuitiva.
- **Consistência**: Padronizar o estilo e a notação facilita a compreensão entre diferentes documentos e equipes.
- **Manutenção**: Diagramas seguindo regras claras são mais fáceis de atualizar e versionar.
- **Acessibilidade**: Diagramas claros são mais inclusivos para pessoas com diferentes níveis de experiência e até mesmo com deficiências visuais (quando seguem boas práticas de contraste e legenda).

### Princípios Gerais
1. **Claridade acima de tudo**: Um diagrama deve ser compreensível em poucos segundos por alguém familiarizado com o domínio.
2. **Propósito definido**: Cada diagrama deve ter um objetivo claro (ex: mostrar estrutura de componentes, fluxo de dados, dinamismo de tempo de execução).
3. **Notação apropriada**: Use uma notação padrão (como UML, C4, ArchiMate, SysML) ou estabeleça uma legenda clara se usar notação personalizada.
4. **Nível de detalhe adequado**: Ajuste o nível de abstração ao público-alvo e ao objetivo do diagrama.
5. **Evite o ruído**: Elimine elementos decorativos que não agregam informação (chartjunk).
6. **Legibilidade**: Use tamanhos de fonte, cores e contraste adequados.
7. **Atualizabilidade**: Estruture o diagrama de forma que mudanças sejam fáceis de fazer.

## Técnicas

### Escolha do Tipo de Diagrama
Selecione o tipo de diagrama com base no que você deseja comunicar:

- **Estrutura Estática**: Diagramas de componentes, pacotes, classes (UML), ou diagramas de containers e componentes (C4).
- **Comportamento/Dinâmica**: Diagramas de sequência, atividade, máquina de estados (UML), ou diagramas de fluxo de dados.
- **Infraestrutura e Implantação**: Diagramas de implantação (UML), diagramas de nuvem, ou diagramas de arquitetura de solução.
- **Domínio e Entidades**: Diagramas de entidades e relacionamentos (ER), ou diagramas de domínio (DDD).
- **Riscos e Visão Geral**: Diagramas de arquitetura empresarial, ou mapas de estratégia.

### Estrutura de um Diagrama na Documentação
Cada diagrama deve ser acompanhado por:

1. **Título descritivo**: Indique claramente o que o diagrama mostra e seu ponto de vista.
2. **Breve explicação**: Uma ou duas frases sobre o propósito e o contexto do diagrama.
3. **O diagrama**: Imagem ou markup (como Mermaid, PlantUML, etc.) com legenda clara.
4. **Legenda (se necessária)**: Explique símbolos, cores, linhas ou quaisquer elementos não padronizados.
5. **Considerações importantes**: Limitações, pressupostos ou pontos de atenção sobre o que o diagrama mostra ou não mostra.
6. **Onde encontrar mais**: Links para diagramas detalhados ou documentação relacionada (opcional).

### Diretrizes de Criação
- **Use ferramentas padronizadas**: Preferir ferramentas que suportem versionamento de texto (como PlantUML ou Mermaid) para facilitar diffs e revisões.
- **Mantenha simples**: Comece com o essencial e adicione detalhes apenas se necessário para o propósito.
- **Agrupe relacionados**: Use clusters, pacotes ou áreas para agrupar elementos relacionados.
- **Direção consistente**: Em diagramas de fluxo, mantenha uma direção geral (ex: esquerda para direita ou top-down) para facilitar a leitura.
- **Espaçamento adequado**: Deixe espaço suficiente entre elementos para evitar poluição visual.
- **Cores com propósito**: Use cores para categorizar ou destacar, não apenas para decorar. Certifique-se de que o diagrama seja legível em escala de cinza (para impressão).
- **Fontes legíveis**: Use fontes sans-serif e tamanho adequado (geralmente 10pt ou mais para diagramas impressos).
- **Evite setas cruzadas demais**: Reorganize o layout para minimizar cruzamentos de linhas, que podem causar confusão.
- **Versionamento**: Trate diagramas como código: versione-os, revise-os e mantenha-os atualizados.

### Notações e Linguagens Comuns
- **C4 Model**: Especialmente útil para arquitetura de software, com níveis de contexto, containers, componentes e código.
- **UML 2.x**: Amplamente conhecido, bom para detalhes de design orientado a objeto.
- **ArchiMate**: Focado em arquitetura de negócio, aplicação e tecnologia.
- **Mermaid**: Sintaxe baseada em texto que se integra bem com Markdown e ferramentas de documentação.
- **PlantUML**: Outra opção baseada em texto para UML e outros diagramas.
- **Diagramas de Infraestrutura**: Ferramentas como AWS Architecture Icons, Azure Icons, ou ícones genéricos de nuvem.

## Checklist para Diagramas

Antes de incluir um diagrama na documentação, verifique:

- [ ] O diagrama tem um título claro e descritivo?
- [ ] Há uma breve explicação do que o diagrama demonstra e seu contexto?
- [ ] A notação usada é apropriada para o propósito e está claramente indicada (ou legendada)?
- [ ] O nível de detalhe está adequado ao público-alvo e ao objetivo?
- [ ] Os elementos são legíveis (tamanho de fonte, contraste, espaçamento)?
- [ ] O uso de cor é intencional e acessível (verifique contraste e daltonismo)?
- [ ] O diagrama está livre de elementos decorativos não informativos (chartjunk)?
- [ ] A direção do fluxo (se aplicável) é clara e consistente?
- [ ] Agrupamentos relacionados são visualmente distintos?
- [ ] Legendas ou chaves estão presentes quando necessário para interpretar símbolos não padrão?
- [ ] O diagrama é versão-controlado (se possível) e há um processo para atualizá-lo?
- [ ] O diagrama é fiel ao estado atual ou planejado da arquitetura (conforme o contexto)?
- [ ] Há referências a diagramas mais detalhados ou à documentação relacionada (opcional)?
- [ ] O diagrama foi revisado por pelo menos outro arquiteto ou desenvolvedor para validar clareza e correção?

## Estudos de Caso

### Caso 1: Diagrama de Containers C4 para um Sistema de E-commerce
- **Contexto**: Mostrar a estrutura de alto nível de um sistema de e-commerce para stakeholders técnicos e de negócio.
- **Aplicação das regras**:
  - **Título**: "Diagrama de Containers - Sistema de E-commerce"
  - **Explicação**: Mostra os containers principais (aplicações web, mobile, APIs, bancos de dados) e suas interações.
  - **Notação**: C4 Model, nível de containers.
  - **Elementos**: 
    - Container: Aplicação Web ( tecnologia: JavaScript, React )
    - Container: Aplicação Mobile ( tecnologia: Kotlin/Android, Swift/iOS )
    - Container: API de Pedidos ( tecnologia: Java, Spring Boot )
    - Container: API de Catálogo ( tecnologia: Node.js, Express )
    - Container: Banco de Dados de Pedidos ( tecnologia: PostgreSQL )
    - Container: Banco de Dados de Catálogo ( tecnologia: MongoDB )
    - Container: Sistema de Pagamento ( tecnologia: Tercerizado, exposição via API )
    - Container: Sistema de Email ( tecnologia: Tercerizado, exposição via API )
  - **Relacionamentos**: Showam fluxos de dados e de comandos usando setas labeled.
  - **Legenda**: Usa os ícones oficiais do C4 (retângulos com ícones de tecnologia opcional no canto superior direito).
  - **Considerações importantes**: 
    - Este diagrama não mostra detalhes de componentes dentro de cada container (isso seria um diagrama de componentes).
    - Infraestrutura de rede (como firewalls, balanceadores de carga) é omitida para focar nos containers de aplicação.
- **Resultado**: O diagrama rapidamente comunica a estrutura de alto nível e as responsabilidades de cada parte do sistema.

### Caso 2: Diagrama de Sequência de Autenticação OAuth 2.0
- **Contexto**: Ilustrar o fluxo de concessão de código de autorização em um sistema de login social.
- **Aplicação das regras**:
  - **Título**: "Diagrama de Sequência - Fluxo de Código de Autorização OAuth 2.0"
  - **Explicação**: Mostra as interações entre o usuário, o navegador, o aplicativo cliente, o servidor de autorização e o servidor de recursos.
  - **Notação**: UML 2.x Diagrama de Sequência.
  - **Participantes**: Usuário, Navegador, Cliente Aplicativo, Servidor de Autorização, Servidor de Recursos.
  - **Mensagens**: 
    - Usuário -> Navegador: Inicia login
    - Navegador -> Cliente Aplicativo: Redireciona para login
    - Cliente Aplicativo -> Servidor de Autorização: Solicita autorização (URL com client_id, redirect_uri, scope, state)
    - Servidor de Autorizador -> Navegador: Página de login e consentimento
    - Navegador -> Usuário: Mostra página de login
    - Usuário -> Navegador: Insere credenciais e consente
    - Navegador -> Servidor de Autorização: Envia credenciais e consentimento
    - Servidor de Autorizador -> Navegador: Redireciona de volta ao cliente com código de autorização
    - Navegador -> Cliente Aplicativo: Recebe código de autorização
    - Cliente Aplicativo -> Servidor de Autorizador: Troca código por token de acesso
    - Servidor de Autorizador -> Cliente Aplicativo: Retorna token de acesso
    - Cliente Aplicativo -> Servidor de Recursos: Solicita recurso com token de acesso
    - Servidor de Recursos -> Cliente Aplicativo: Retorna recurso protegido
  - **Considerações importantes**: 
    - O diagrama simplifica ao não mostrar tratamento de erros (ex: códigos de autorização expirados).
    - URLs de redirecionamento e parâmetros de estado são omitidos para clareza, mas seriam importantes em uma implementação real.
- **Resultado**: O diagrama de sequência claramente mostra as trocas de mensagens no fluxo OAuth 2.0, facilitando o entendimento do protocolo.

## Tendências Futuras

### Diagramas como Código com Integração em CI/CD
- Tratar diagramas como artefatos de código que são construídos, testados (por exemplo, verificar sintaxe) e implantados junto com a documentação e o software.

### Validação Automática de Notação
- Usar ferramentas que validem a notação do diagrama (por exemplo, verificadores de sintaxe PlantUML ou regras do C4) como parte do processo de revisão de pull request.

### Diagramas Interativos e Exploráveis
- Diagramas que permitem ao usuário clicar em elementos para ver detalhes, navegar para níveis mais baixos ou acessar documentação associada.

### Integração com Ferramentas de Arquitetura em Tempo Real
- Ligar diagramas a fontes de dados em tempo real (como CMDBs ou ferramentas de descoberta de serviço) para mostrar o estado atual da arquitetura de forma automática.

### Uso de IA para Sugestão de Melhorias em Diagramas
- Modelos de linguagem grande sugerindo layout, agrupamentos ou até mesmo detectando inconsistências entre o diagrama e a descrição textual.

## Resumo

Diagramas são instrumentos poderosos na comunicação de arquitetura de software, mas sua eficácia depende de seguir princípios de clareza, propósito e notação apropriada. Ao estabelecer e aplicar regras consistentes para a criação de diagramas, arquitetos garantem que seus artefatos visuais sejam verdadeiramente úteis para entender, analisar e evoluir sistemas de software.

Lembre-se de que o melhor diagrama é aquele que comunica a mensagem desejada com o mínimo de esforço de interpretação por parte do público-alvo. Equilibre o nível de detalhe com a simplicidade, e sempre considere o contexto de uso e a audiência.

Ao aplicar consistentemente estas regras, você assegura que os diagramas na sua documentação sejam confiáveis, fáceis de entender e úteis para arquitetos, desenvolvedores e stakeholders em diferentes níveis de experiência.