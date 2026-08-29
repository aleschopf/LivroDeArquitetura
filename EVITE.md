# EVITE

Esta parte lista práticas, padrões ou ideias que arquitetos de software devem geralmente evitar, pois tendem a levar a problemas de qualidade, manutenibilidade ou sucesso do projeto.

## Fundamentos

### Por que listar o que evitar?
- **Aprender com os erros**: Conhecer armadilhas comuns ajuda a evitá-las em projetos futuros.
- **Decisões mais informadas**: Saber quais abordagens têm histórico de falhas permite escolher alternativas melhores.
- **Cultura de melhoria**: Reconhecer o que não funciona é um passo importante para evolucionar práticas de arquitetura.
- **Comunicação de riscos**: Facilita discutir com a equipe por que certa direção é perigosa.

### Princípios Gerais
1. **Contexto importa**: Algumas práticas listadas podem ser úteis em situações muito específicas, mas são geralmente arriscadas.
2. **Não é uma proibição absoluta**: Em vez de "nunca faça isso", pense em "pense muito bem antes de fazer isso e tenha fortes justificativas".
3. **Entenda o porquê**: Para cada item, compreenda as razões pelas quais ele é problemático para poder avaliar se seu caso é uma exceção.
4. **Foque no resultado**: O objetivo não é evitar o item em si, mas evitar os resultados negativos que ele costuma produzir.
5. **Use como checklist de risco**: Ao propor uma arquitetura, revise esta lista para ver se algum item se aplica.

## Técnicas

### Como Usar Esta Lista
- **Durante o projeto**: Quando estiver prestes a tomar uma decisão arquitetural, consulte a lista para ver se algum item se aplica.
- **Em revisões de arquitetura**: Use como um dos pontos de verificação para identificar potenciais problemas.
- **Para orientar equipes**: Compartilhe com desenvolvedores e outros arquitetos para construir um entendimento comum do que é considerado de alto risco.
- **Como ponto de partida para discussão**: Se alguém propor algo da lista, use-a para iniciar uma conversa sobre os trade-offs e riscos envolvidos.

### Checklist para Avaliar Riscos
Antes de adotar uma prática que aparece nesta lista, pergunte-se:

- [ ] Entendo completamente por que esta prática é considerada arriscada?
- [ ] Tenho dados ou evidências sólidas de que, neste contexto específico, os riscos são mitigados?
- [ ] Há alternativas menos arriscadas que eu deveria considerar primeiro?
- [ ] Se eu proceeding, quais medidas de mitigação estou colocando em prática?
- [ ] Estou preparado para lidar com as consequências negativas se elas ocorrerem apesar das mitigações?
- [ ] Esta decisão está alinhada com a tolerância a risco da minha organização e dos stakeholders?
- [ ] Documentei a decisão, os riscos considerados e as mitigações aplicadas (ideal como um ADR)?

## Lista de O Que Evitar

### 1. **Escolher tecnologia apenas porque é nova ou estilosa (Resumo-Driven Development)**
- **Por que evitar**: Leva a curvas de aprendizado inesperadas, falta de suporte mature, bugs não documentados e dificuldade em contratação.
- **Quando pode ser aceitável**: Quando há uma vantagem técnica clara e mensurável que justifica o risco, e a equipe tem capacidade de absorver o novo.

### 2. **Abstração prematura ou excessiva**
- **Por que evitar**: Adiciona complexidade sem benefício imediato, torna o código mais difícil de entender e pode não atender às necessidades reais quando elas surgirem.
- **Quando pode ser aceitável**: Quando há padrões claros e repetitivos que são compreendidos bem e a abstração simplifica significativamente o uso.

### 3. **Especulação excessiva sobre necessidades futuras (Over-engineering for hypothetical futures)**
- **Por que evitar**: Gera código que nunca será usado, aumenta o tempo de desenvolvimento inicial e pode tornar o sistema mais rígido para mudanças reais.
- **Quando pode ser aceitável**: Quando há um roteiro público e comprometido com alta certeza de que o recurso será necessário em um prazo curto.

### 4. **Ignorar dívida técnica intencionalmente**
- **Por que evitar**: Acumula problemas que tornam o sistema cada vez mais difícil e caro de mudar, levando à lentidão e à fragilidade.
- **Quando pode ser aceitável**: Só se houver um plano claro e acordado para pagar a dívida em um futuro próximo, e o atraso for estritamente necessário para sobreviver a uma pressão imediata de negócio.

### 5. **Falta de observabilidade em sistemas distribuídos**
- **Por que evitar**: Torna extremamente difícil diagnosticar problemas de desempenho, falhas ou comportamento inesperado em produção.
- **Quando pode ser aceitável**: Nunca em sistemas distribuídos de qualquer relevância; até mesmo um monolítico se beneficia de logs e métricas básicas.

### 6. **Dependência explícita de estado em componentes que deveriam ser stateless (especialmente em microsserviços)**
- **Por que evitar**: Complica escalabilidade, tolerância a falhas e atualizações, pois o estado precisa ser gerenciado e replicado.
- **Quando pode ser aceitável**: Quando o estado é realmente intrínseco à responsabilidade do componente e há uma estratégia clara para gerenciá-lo (ex: usando um banco de dados dedicado ou um cache distribuído com políticas de consistência claras).

### 7. **Escolher consistência forte em todos os lugares sem considerar o custo**
- **Por que evitar**: Pode introduzir latência desnecessária, reduzir disponibilidade e aumentar complexidade operacional (ex: requerendo protocolos de consenso pesados).
- **Quando pode ser aceitável**: Quando a corretude absoluta dos dados é um requisito rígido de negócio ou regulatório (ex: transferências financeiras).

### 8. **Monolíticos gigantescos sem limites claros (desde que não sejam intencionalmente modulares)**
- **Por que evitar**: Torna o código difícil de entender, testar e mudar; aumenta o acoplamento e o risco de efeitos colaterais.
- **Quando pode ser aceitável**: Quando o sistema é realmente pequeno, simples e estável, e a equipe é pequena o suficiente para gerenciá-lo eficazmente (mas mesmo assim, limites internos são bons).

### 9. **Falta de versionamento de API ou contrato de serviço**
- **Por que evitar**: Leva a quebrar clientes inesperadamente quando mudanças são feitas, causando incidentes e perda de confiança.
- **Quando pode ser aceitável**: Só se o serviço for puramente interno e houver um mecanismo garantido de atualizar todos os consumidores simultaneamente (o que é raro).

### 10. **Ignorar falhas parciais em sistemas distribuídos**
- **Por que evitar**: Assume que ou tudo funciona ou tudo falha, levando a tratamento inadequado quando apenas parte do sistema está com problemas.
- **Quando pode ser aceitável**: Nunca em sistemas distribuídos; sempre projetar para degradação graceful e tratamento de falhas parciais.

### 11. **Usar threads diretamente para concorrência em aplicações de alto nível quando alternativas melhores existem**
- **Por que evitar**: Introduz complexidade de sincronização, risco de condições de corrida e deadlocks, e é difícil de escalar corretamente.
- **Quando pode ser aceitável**: Quando se está construindo um framework de concorrência ou quando há necessidade de controle muito fino que não pode ser alcançado com abstrações de nível superior (como pools de workers ou modelos de ator).

### 12. **Falta de limites claros (bounded contexts) em domínios complexos**
- **Por que evitar**: Leva a modelos de dados confusos, termos sobrecarregados com múltiplos significados e dificuldade em evoluir partes do sistema independente.
- **Quando pode ser aceitável**: Só em domínios muito simples e estáveis onde a ambiguidade não causa problemas.

### 13. **Depender de sorte ou configuração manual em ambientes de produção**
- **Por que evitar**: Leva a inconsistências entre ambientes, dificuldade em reproduzir problemas e falhas em escala.
- **Quando pode ser aceitável**: Nunca; sempre use infraestrutura como código e processos automatizados para provisionamento e configuração.

### 14. **Optimização prematura baseada em intuição**
- **Por que evitar**: Gera código mais complexo que pode não abordar o verdadeiro gargalo, desviando esforço de onde realmente importa.
- **Quando pode ser aceitável**: Só após medição mostrar claramente onde o gargalo está e que a otimização traz benefício significativo.

### 15. **Não considerar a evolutibilidade ao escolher arquitetura ou tecnologia**
- **Por que evitar**: Pode ficar preso em uma escolha que se torna um obstáculo significativo quando o negócio precisa mudar de direção.
- **Quando pode ser aceitável**: Só se houver alta certeza de que o escopo e os requisitos do projeto são fixos e não mudarão por um período muito longo (o que é raro em software).

## Estudos de Caso

### Caso 1: Adoção de uma Nova Tecnologia de Banco de Dados apenas por ser a "moda do momento"
- **Contexto**: Uma startup decidiu usar um banco de dados NoSQL recente e pouco estabelecido para seu produto principal, atraída pelas promessas de escalabilidade e flexibilidade.
- **O que aconteceu**: 
  - A equipe gastou meses lidando com bugs não documentados e falta de ferramentas de administração maduras.
  - Contratar novos desenvolvedores com experiência naquele banco específico provou ser difícil e caro.
  - Quando precisaram de recursos transacionais fortes, descobriram que o banco não os suportava bem, exigindo trabalho extra na camada de aplicação.
- **Resultado**: O projeto atrasou significativamente, e eventualmente migraram para um banco de dados mais estabelecido, incurindo em custos de migração e perda de confiança.
- **Lição**: Mesmo que uma nova tecnologia pareça emocionante, avalie-a com os mesmos critérios de maturidade, suporte e adequação ao problema que faria com qualquer outra escolha.

### Caso 2: Abstração Prematura de um Framework de Log Interno
- **Contexto**: Uma equipe decidiu criar seu próprio framework de logging interno para padronizar o formato em todos os serviços, apesar de existir bibliotecas maduras disponíveis.
- **O que aconteceu**:
  - O framework interno faltava recursos que as bibliotecas padrão tinham (como rotação de arquivos assíncrona, níveis configuráveis por ambiente, integração com sistemas de alerta).
  - Toda vez que precisavam de um novo recurso, a equipe tinha que implementá-lo e mantê-lo.
  - Quando uma biblioteca padrão lançou uma atualização de segurança crítica, a equipe teve que esperar até que o framework interno fosse atualizado, deixando-os vulneráveis por um período.
- **Resultado**: Depois de um ano, a equipe abandonou o framework interno e adotou uma biblioteca padrão, economizando tempo de desenvolvimento e melhorando a segurança.
- **Lição**: Antes de criar uma abstração interna, pergunte-se se ela realmente oferece algo que as soluções existentes não oferecem, e se o custo de manutenção será justificado pelo uso real.

### Caso 3: Especulação sobre Necessidades Futuras de Integração
- **Contexto**: Ao projetar um módulo de pagamento, uma equipe decidiu adicionar suporte para dez métodos de pagamento diferentes, incluindo alguns que eram apenas rumores de que a empresa poderia apoiar no futuro.
- **O que aconteceu**:
  - O código inicial ficou muito mais complexo devido à necessidade de abstrair sobre as diferentes APIs de pagamento.
  - Nenhum dos métodos de pagamento especulativos foi realmente necessário nos primeiros dois anos do produto.
  - Quando chegou a hora de adicionar um novo método de pagamento real, a abstração existente não se encaixou bem e teve que ser significativamente modificada.
- **Resultado**: A equipe gastou tempo significativo construindo e mantendo código que nunca foi usado, e quando chegou a hora de usar, a abstração não ajudou tanto quanto esperado.
- **Lição**: Construa para as necessidades conhecidas e claras; quando o futuro chegar, avalie então a melhor maneira de integrá-lo.

## Tendências Futuras

### Detecção Automática de Antipadrões em Código e Arquitetura
- Ferramentas que analisam repositórios de código ou diagramas de arquitetura para sinalizar práticas que correspondem a itens desta lista (ex: detecção de serviços com estado em arquiteturas que deveriam ser stateless).

### IA para Sugestão de Alternativas Menos Arriscadas
- Modelos de linguagem grande que, dado um problema ou uma proposta de arquitetura, sugerem abordagens que evitam os antipadrões conhecidos.

### Integração com Revisões de Pull Request e Diagramas como Código
- Verificações automáticas que impedem a mesclagem de código que introduz claramente um antipadrão desta lista, ou que pelo menos exigem uma revisão adicional.

### Gamificação da Evitação de Armadilhas
- Sistemas que recompensam equipes por identificar e evitar riscos arquiteturais, promovendo uma cultura de pensamento crítico e aprendizado com os erros dos outros.

## Resumo

Saber o que evitar é tão importante quanto saber o que fazer na arquitetura de software. Esta lista de práticas para evitar serve como um guia de boas práticas negativo, ajudando a arquitetos a reconhecer sinais de alerta e a tomar decisões mais sólidas.

Lembre-se de que o contexto é rei: nenhuma proibição é absoluta. O objetivo não é seguir esta lista como um dogma, mas usá-la como ponto de partida para pensamento crítico. Quando você se deparar com uma sugestão que aparece aqui, não a rejeite imediatamente, mas sim use-a como gatilho para perguntar: "Por que isso é arriscado em geral, e o que faria nosso caso ser uma exceção?"

Ao aplicar consistentemente esta mentalidade de questionamento e aprendizado com os erros alheios, você assegura que seus projetos de arquitetura de software se beneficiem da sabedoria acumulada da comunidade, evitando armadilhas comuns e focando em soluções que têm maior probabilidade de sucesso.