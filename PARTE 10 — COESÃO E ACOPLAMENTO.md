---
trilha: "INICIANTE"
---
**Navegação:** [[MOC — TRILHA INICIANTE]]
← [[PARTE 9 — SOLID]] | #trilha/iniciante | [[PARTE 11 — DESIGN PATTERNS]] →

---
# PARTE 10 — COESÃO E ACOPLAMENTO

> 🧠 **ESSENCIAL**
> 
> Coesão e acoplamento são conceitos fundamentais na engenharia de software que medem, respectivamente, o quão bem os elementos dentro de um módulo pertencem juntos e o quão dependentemente os módulos estão uns dos outros. Alta coesão e baixo acoplamento são objetivos-chave para criar sistemas de software mantíveis, compreensíveis e flexíveis.

## O que é Coesão e Acoplamento?
Coesão refere-se ao grau em que as responsabilidades ou funcionalidades dentro de um único módulo (classe, função, componente) são relacionadas e focadas em um único propósito. Acoplamento refere-se ao grau de dependência direta entre dois módulos - quão muito um módulo sabe ou depende do outro.

### Por que existe?
Como resposta à necessidade de medir qualitativamente a qualidade do design de software além de apenas funcionalidade correta. Sistemas com baixa coesão e alto acoplamento são difíceis de entender, modificar, testar e manter.

### Qual problema resolve?
- Dificuldade em entender o propósito de um módulo devido a responsabilidades não relacionadas
- Medo de fazer alterações devido a efeitos colaterais em outros módulos
- Dificuldade em reutilizar módulos devido a dependências desnecessárias
- Código que se torna frágil e propenso a bugs quando modificado
- Dificuldade em testar unidades isoladamente devido a dependências externas
- Aumento do custo de mudança (cost of change) ao longo do tempo

### Como funciona internamente?
- **Coesão:** Medida de quão fortemente relacionada e focada é a responsabilidade dentro de um módulo. Variará de coidental (pior) até funcional (melhor).
- **Acoplamento:** Medida de quão direta e forte é a dependência entre módulos. Variará de conteúdo (pior) até nenhum/indireto (melhor).

### Como implementar?
1. **Aumentar coesão:** Garantir que cada módulo tenha uma única, bem definida responsabilidade
2. **Reduzir acoplamento:** Minimizar dependências diretas entre módulos usando abstrações, interfaces e injeção de dependência
3. **Usar princípios de design:** Aplicar SOLID, GRASP e outros princípios que naturalmente promovem alta coesão e baixo acoplamento
4. **Refatorar continuamente:** Melhorar coesão e reduzir acoplamento como parte do processo de melhoria do código
5. **Dividir responsabilidades:** Separar funcionalidades não relacionadas em módulos diferentes
6. **Ocultar implementação:** Usar encapsulamento para impedir que módulos dependam de detalhes internos de outros
7. **Usar padrões de projeto:** Muitos padrões (como Mediator, Observer, etc.) ajudam a reduzir acoplamento

### Quais são as alternativas?
- Módulos com múltiplas responsabilidades não relacionadas (baixa coesão)
- Dependências diretas e explícitas entre módulos (alto acoplamento)
- Código que viola encapsulamento expoe detalhes internos desnecessariamente
- Uso excessivo de variáveis globais ou singletons
- Herança inadequada que cria hierarquias rígidas e acopladas

### Quais são os trade-offs?
**Vantagens de alta coesão e baixo acoplamento:**
- Código mais fácil de entender porque cada módulo tem um propósito claro
- Menor risco de introdzir bugs ao fazer alterações devido a menos efeitos colaterais
- Facilidade de testar unidades de código isoladamente
- Maior reutilização pois módulos focados são mais prováveis de serem úteis em outros contextos
- Melhor manutenibilidade pois mudanças são mais previsíveis e localizadas
- Facilidade de modificação quando requisitos relacionados à responsabilidade mudam
- Código que se auto-documenta através de sua estrutura clara

**Desvantagens/custos:**
- Pode levar a overengineering se aplicado rigidamente a problemas simples
- Pode aumentar inicialmente o número de módulos devido à divisão de responsabilidades
- Pode parecer fragmentado para desenvolvedores iniciantes
- Pode haver inicialmente mais código de "cola" para coordenar entre módulos
- Requer disciplina para manter conforme o código evolui
- Pode haver desempenho ligeiramente menor devido a indireções (geralmente insignificativamente insignificante em comparação com benefícios)

### Quando usar?
- Sempre que se estiver projetando ou refatorando módulos de software
- Quando se quer construir sistemas que sejam fáceis de manter e estender
- Quando múltiplos desenvolvedores vão trabalhar no mesmo código ao longo do tempo
- Quando se quer reduzir o risco associado a mudanças no código
- Quando se está construindo bibliotecas ou frameworks que serão usados por outros
- Quando a qualidade e manutenibilidade a longo prazo são importantes

### Quando não usar?
- Quando se está prototipando e velocidade é a única prioridade (mas mesmo então, princípios básicos ajudam)
- Quando se está escrevendo código descartável que será usado uma vez e jogado fora
- Quando o overhead de modularização não traz benefício proporcional ao problema sendo resolvido
- Quando se está em um ambiente altamente restrito onde cada módulo conta (sistemas embarcados ultra-restritos)
- Quando se está aprendendo programação e ainda está dominando conceitos básicos

### Quais são os erros mais comuns?
- Confundir coesão apenas com tamanho (achar que módulos pequenos automaticamente têm alta coesão)
- Focar apenas em reduzir acoplamento sem aumentar coesão (pode levar a módulos sem propósito claro)
- Achar que baixo acoplamento significa nenhuma dependência entre módulos (impossível em sistemas não triviais)
- Modificar coesão e acoplamento apenas em algumas partes do código deixando outras partes violando os princípios
- Esquecer que coesão e acoplamento são características relativas, não absolutas
- Aplicar métricas de coesão e acoplamento de forma mecânica sem considerar contexto de negócio

### Como isso afeta:
- *performance:* Impacto mínimo devido a indireções (geralmente indireções (geralmente insignificante em comparação com benefícios)
- *escalabilidade:* Similar; alta coesão e baixo acoplamento não impõem limitações de escalabilidade
- *disponibilidade:* Nenhum impacto direto
- *consistência:* Melhora devido a menor acoplamento e maior previsibilidade do comportamento
- *segurança:* Similar; coesão e acoplamento não afetam diretamente preocupações de segurança
- *custo:* Custo inicial pode ser maior devido ao overhead de design, mas custo de manutenção a longo prazo tende a ser menor
- *observabilidade:* Melhora pois código menos acoplado é mais fácil de instrumentar e monitorar
- *complexidade operacional:* Pode reduzir devido a menos bugs e maior facilidade de fazer mudanças

### Exemplos reais de aplicação
- Bibliotecas padrão de linguagens (como java.util, std::cpp, Python stdlib) onde cada pacote tem alta coesão
- Frameworks como Spring onde módulos como spring-context, spring-tx, spring-web têm responsabilidades claras
- Arquiteturas de microserviços onde cada serviço tem alta coesão em torno de uma capacidade de negócio
- Sistemas de plugin onde novas funcionalidades podem ser adicionadas sem modificar o núcleo (baixo acoplamento)
- Arquiteturas hexagonal e limpa que dependem fortemente de interfaces para reduzir acoplamento entre camadas

### Exemplo simplificado
Baixa coesão e alto acoplamento (errado):
```java
// ❌ ERRADO: Classe com baixa coesão (múltiplas responsabilidades não relacionadas)
// e alto acoplamento (dependência direta em implementações específicas)
public class LojaVirtual {
    // Responsabilidade 1: Gerenciamento de produtos
    private Map<String, Produto> catalogo;
    
    // Responsabilidade 2: Processamento de pagamentos
    private GatewayPagamentoStripe gatewayStripe;
    private GatewayPagamentoPayPal gatewayPayPal;
    
    // Responsabilidade 3: Envio de emails
    private ServicoEmailSMTP servicoEmail;
    
    // Responsabilidade 4: Geração de relatórios
    private GeradorDeRelatorioPDF geradorRelatorio;
    
    // Responsabilidade 5: Log de auditoria
    private Logger arquivoLog;
    
    // Métodos com alta coesão interna baixa (misturam responsabilidades)
    public void processarPedido(String produtoId, String clienteId, String tipoPagamento) {
        // Mistura de responsabilidades: busca produto, processa pagamento, envia email, gera relatório, faz log
        Produto produto = catalogo.get(produtoId);
        
        // Alto acoplamento: depende diretamente de implementações específicas de pagamento
        BigDecimal valor = produto.getPreco();
        if ("stripe".equals(tipoPagamento)) {
            gatewayStripe.processarPagamento(valor, clienteId);
        } else if ("paypal".equals(tipoPagamento)) {
            gatewayPayPal.processarPagamento(valor, clienteId);
        }
        
        // Alto acoplamento: depende diretamente de implementação específica de email
        servicoEmail.enviarEmail(clienteId, "Confirmação", "Seu pedido foi processado");
        
        // Mistura de responsabilidades: gera relatório e faz log no mesmo método
        geradorRelatorio.gerarRelatorioDiario();
        arquivoLog.log("Pedido processado: " + produtoId);
    }
    
    // Mais métodos misturando responsabilidades...
}
```

Alta coesão e baixo acoplamento (correto):
```java
// ✅ CORRETO: Cada classe tem alta coesão (única responsabilidade)
// e baixo acoplamento (depende de abstrações, não de implementações)

// Classe com alta coesão: apenas gerenciamento de catalogo de produtos
public class CatalogoProdutos {
    private Map<String, Produto> produtos;
    
    public Produto buscarPorId(String id) {
        return produtos.get(id);
    }
    
    public void adicionarProduto(Produto produto) {
        produtos.put(produto.getId(), produto);
    }
    
    // Outros métodos relacionados apenas ao catalogo...
}

// Classe com alta coesão: apenas processamento de pagamentos
public interface ProcessadorDePagamento {  // Abstração para baixo acoplamento
    void processarPagamento(BigDecimal valor, String clienteId);
}

public class ProcessadorPagamentoStripe implements ProcessadorDePagamento {
    public void processarPagamento(BigDecimal valor, String clienteId) {
        // Implementação específica do Stripe
    }
}

public class ProcessadorPagamentoPayPal implements ProcessadorDePagamento {
    public void processarPagamento(BigDecimal valor, String clienteId) {
        // Implementação específica do PayPal
    }
}

// Classe com alta coesão: apenas envio de notificações
public interface ServicoNotificacao {  // Abstração para baixo acoplamento
    void enviar(String destinatario, String assunto, String corpo);
}

public class ServicoNotificacaoEmail implements ServicoNotificacao {
    public void enviar(String destinatario, String assunto, String corpo) {
        // Implementação específica de email
    }
}

// Classe com alta coesão: apenas geração de relatórios
public class GeradorDeRelatorios {
    public void gerarRelatorioDiario() {
        // Lógica apenas para geração de relatórios
    }
}

// Classe com alta coesão: apenas log de auditoria
public class LoggerAuditoria {
    public void log(String mensagem) {
        // Lógica apenas para logging
    }
}

// Orquestrador com baixa coesão intencional (apenas coordena, não implementa)
public class ProcessadorDePedidos {
    private final CatalogoProdutos catalogo;
    private final ProcessadorDePagamento processadorPagamento;
    private final ServicoNotificacao servicoNotificacao;
    private final GeradorDeRelatorios geradorRelatorios;
    private final LoggerAuditoria logger;
    
    // Baixo acoplamento: depende de abstrações, não de implementações concretas
    public ProcessadorDePedidos(
            CatalogoProdutos catalogo,
            ProcessadorDePagamento processadorPagamento,
            ServicoNotificacao servicoNotificacao,
            GeradorDeRelatorios geradorRelatorios,
            LoggerAuditoria logger) {
        this.catalogo = catalogo;
        this.processadorPagamento = processadorPagamento;
        this.servicoNotificacao = servicoNotificacao;
        this.geradorRelatorios = geradorRelatorios;
        this.logger = logger;
    }
    
    public void processarPedido(String produtoId, String clienteId, String tipoPagamento) {
        // Alta coesão: apenas orquestra o processo de pedido
        Produto produto = catalogo.buscarPorId(produtoId);
        if (produto == null) {
            throw new IllegalArgumentException("Produto não encontrado");
        }
        
        // Baixo acoplamento: delega para abstrações
        BigDecimal valor = produto.getPreco();
        processadorPagamento.processarPagamento(valor, clienteId);
        
        servicoNotificacao.enviar(
                clienteId, 
                "Confirmação do Pedido", 
                "Seu pedido foi recebido e está sendo processado."
        );
        
        // Essas responsabilidades podem ser delegadas a outros serviços especializados
        geradorRelatorios.gerarRelatorioDiario();
        logger.log("Pedido processado: " + produtoId);
    }
}

// Uso com injeção de dependência (mantém baixo acoplamento):
// ProcessadorDePedidos processador = new ProcessadorDePedidos(
//         new CatalogoProdutos(),
//         new ProcessadorPagamentoStripe(),  // Pode trocar para PayPal facilmente
//         new ServicoNotificacaoEmail(),     // Pode trocar para SMS facilmente
//         new GeradorDeRelatorios(),
//         new LoggerAuditoria()
// );
//
// Para trocar o processador de pagamento:
// ProcessadorDePedidos processador = new ProcessadorDePedidos(
//         catalogo,
//         new ProcessadorPagamentoPayPal(),  // Apenas esta linha mudou
//         servicoNotificacao,
//         geradorRelatorios,
//         logger
// );
```

### Exemplo de sistema de produção
Sistema de gestão de biblioteca digital:
- **Alta coesão:** Cada módulo tem uma única responsabilidade bem definida
  - `CatalogService`: apenas gerenciamento do catálogo de livros (busca, adição, remoção)
  - `LoanService`: apenas gerenciamento de empréstimos (criação, renovação, devolução)
  - `UserService`: apenas gerenciamento de usuários (cadastro, autenticação, perfis)
  - `NotificationService`: apenas envio de notificações (lembretes, confirmações, alertas)
  - `ReportService`: apenas geração de relatórios (estatísticas, histórico, desempenho)
  - `AuthService`: apenas autenticação e autorização (login, permissões, sessões)
  - `StorageService`: apenas gerenciamento de armazenamento de arquivos (upload, download, exclusão)
- **Baixo acoplamento:** Módulos interagem através de interfaces bem definidas
  - `LoanService` depende de `UserService` e `CatalogService` através de interfaces
  - `NotificationService` é usado por múltiplos serviços mas não conhece suas implementações
  - `ReportService` consome dados de múltiplos serviços através de APIs ou eventos
  - Nenhum módulo conhece detalhes de implementação de outros (ex: `LoanService` não sabe se `UserService` usa banco de dados ou LDAP)
- **Benefícios:** 
  - É possível trocar o mecanismo de autenticação (de LDAP para OAuth) sem afetar outros serviços
  - É possível adicionar novos tipos de notificação (push, SMS) sem modificar serviços existentes
  - Cada serviço pode ser desenvolvido, testado e implantado independentemente
  - Bugs em um serviço são menos propensos a afetar outros serviços

### Como esse assunto pode aparecer em uma entrevista.
> 🎯 **ENTREVISTA — ALTA FREQUÊNCIA**
> 
> "Explique como você melhoraria a coesão e reduziria o acoplamento em uma classe que atualmente lida com validação de entrada, persistência no banco de dados, envio de notificações e geração de relatórios."
> 
> **Armadilha:** Sugerir apenas separar métodos em classes diferentes sem explicar o critério para decidir o que constitui uma responsabilidade coesa ou como manter dependências mínimas entre as novas classes.
> 
> **Como raciocinar:** Identificar que a classe tem quatro responsabilidades distintas: validação de entrada (verificar se dados são válidos), persistência (salvar/buscar dados), notificação (enviar alertas/mensagens) e geração de relatórios (produzir documentos resumo). Explicar que cada uma deve ser separada em sua própria classe porque muda por razões diferentes e tem pouca relação funcional entre si. Mostrar como isso resulta em classes com alta coesão (cada uma focada em uma área específica) e como reduzir acoplamento usando abstrações e injeção de dependência para que as classes colaborem sem conhecer detalhes de implementação umas das outras.

## Tipos de Coesão

> 🎯 **ENTREVISTA — MÉDIA FREQUÊNCIA**
> 
> Entender os diferentes tipos de coesão ajuda a avaliar e melhorar o design; entrevistadores querem ver se você conhece o espectro desde o pior até o melhor tipo.

### definição
Coesão mede o grau em que as responsabilidades ou funcionalidades dentro de um único módulo são relacionadas e focadas. Existem vários tipos de coesão, classificados do pior (acidental) ao melhor (funcional), baseado em quão fortemente os elementos dentro do módulo pertencem juntos.

### Por que existe?
Para fornecer uma linguagem comum e um framework para discutir a qualidade do design interno de módulos, ajudando desenvolvedores a identificar quando um módulo está fazendo muitas coisas não relacionadas e precisa ser refatorado.

### Como funciona internamente?
Os tipos de coesão são tipicamente classificados da seguinte forma (do pior para o melhor):
1. **Coincidental:** Elementos dentro do módulo não têm relação nenhuma (pior tipo)
2. **Logical:** Elementos estão agrupados por serem logicamente relacionados (ex: todos os métodos de tratamento de erros)
3. **Temporal:** Elementos estão agrupados por ocorrerem no mesmo tempo (ex: métodos de inicialização ou limpeza)
4. **Procedural:** Elementos estão agrupados por seguirem uma certa sequência de execução
5. **Comunicational:** Elementos operam nos mesmos dados de entrada ou produzem os mesmos dados de saída
6. **Sequencial:** Saída de um elemento é entrada para o próximo em uma sequência definida
7. **Funcional:** Todos os elementos contribuem para uma única, bem definida tarefa (melhor tipo)

### Como implementar?
1. **Identificar o tipo atual** de coesão de um módulo analisando suas responsabilidades
2. **Buscar mover-se para cima** na escala (de coincidental para funcional) através de refatoramento
3. **Separar responsabilidades não relacionadas** em módulos diferentes
4. **Garantir que elementos dentro de um módulo** trabalhem juntos para um único propósito claro
5. **Usar coesão funcional como ideal** quando possível, aceitando outros tipos quando funcional não for viável
6. **Refatorar continuamente** à medida que se descobre novas responsabilidades ou muda o entendimento do domínio
7. **Considerar o contexto** - às vezes coesão sequencial ou comunicacional é aceitável e até preferível

### Quais são as alternativas?
- Ignorar coesão e focar apenas na funcionalidade correta
- Tentar tornar todos os módulos functionais (pode levar a módulos excessivamente pequenos)
- Aplicar métricas de coesão de forma mecânica sem entender o significado
- Focar apenas em reduzir métodos por classe sem considerar relações entre eles
- Deixar a coesão como algo subjetivo sem tentar melhorá-la ativamente

### Quais são os trade-offs?
**Vantagens de buscar maior coesão:**
- Código mais fácil de entender porque cada módulo tem um propósito claro
- Facilidade de manutenção pois mudanças são mais previsíveis e localizadas
- Maior reutilização pois módulos focados são mais prováveis de serem úteis em outros contextos
- Facilidade de teste pois é possível isolar unidades de código com propósito único
- Redução de complexidade cognitiva ao ler e entender o código
- Código que se auto-documenta através de sua estrutura e nomes claros

**Desvantagens/custos:**
- Pode levar a overengineering se aplicado rigidamente a problemas simples
- Pode aumentar inicialmente o número de módulos devido à divisão de responsabilidades
- Pode parecer fragmentado para desenvolvedores iniciantes
- Pode haver inicialmente mais código de "cola" para coordenar entre módulos
- Requer disciplina para manter conforme o código evolui
- Pode haver casos onde coesão menos que funcional é mais apropriado (ex: utilitários relacionados)

### Quando usar?
- Sempre que se estiver projetando ou refatorando módulos de software
- Quando se percebe que um módulo está fazendo muitas coisas não relacionadas
- Quando se quer melhorar a legibilidade e compreensibilidade do código
- Quando múltiplos desenvolvedores vão trabalhar no mesmo módulo
- Quando se quer facilitar teste e manutenção
- Quando se está construindo sistemas que devem ter longa vida e múltiplas iterações

### Quando não usar?
- Quando se está prototipando e velocidade é a única prioridade
- Quando o módulo é tão simples que dividir ele seria overengineering
- Quando se está em um ambiente altamente restrito onde cada módulo conta
- Quando se sabe que todas as responsabilidades no módulo mudam juntas por razões de negócio
- Quando a sobrecarga de separação de responsabilidades não traz benefício proporcional

### Quais são os erros mais comuns?
- Achar que coesão incidental é aceitável porque "pelo menos o código funciona"
- Confundir coesão com acoplamento (eles são conceitos diferentes, embora relacionados)
- Esquecer que coesão se aplica a todos os níveis: métodos dentro de classes, classes dentro de pacotes, pacotes dentro de módulos
- Aplicar coesão funcional tão fortemente que se perdem oportunidades de reutilização legítima
- Não considerar que alguns tipos de coesão (como sequencial) podem ser legítimos e eficientes
- Modificar coesão apenas em algumas partes do código deixando outras partes com baixa coesão
- Achar que qualquer separação de responsabilidades automaticamente melhora a coesão

### Como isso afeta:
- *performance:* Nenhum impacto direto
- *escalabilidade:* Nenhum impacto direto
- *disponibilidade:* Nenhum impacto direto
- *consistência:* Melhora pois módulos com alta coesão tendem a ter comportamento mais previsível
- *segurança:* Nenhum impacto direto
- *custo:* Similar; foco em onde a responsabilidade reside ao invés de adicionar ou remover código
- *observabilidade:* Melhora pois módulos com propósito claro são mais fáceis de instrumentar
- *complexidade operacional:* Melhora devido a menos código confuso e mais fácil de modificar

### Exemplos reais de aplicação
- Classe `ValidadorDeEntrada` com alta coesão funcional: apenas métodos para validar diferentes tipos de entrada (email, CPF, telefone, etc.)
- Classe `GerenciadorDeConexaoBanco` com alta coesão funcional: apenas métodos para abrir, fechar, reutilizar e monitorar conexões de banco de dados
- Pacote `java.util.concurrent` com alta coesão funcional: apenas classes relacionadas à concorrência e multithreading
- Módulo de autenticação em um sistema web com alta coesão funcional: apenas lidar com login, logout, sessões, recuperação de senha
- Classe `CalculadoraDeImposto` com alta coesão funcional: apenas calcular diferentes tipos de impostos baseado em regras fiscais

### Exemplo simplificado
Baixa coesão (coincidental - pior tipo):
```java
// ❌ ERRADO: Métodos não relacionados agrupados juntos por acaso
public class Utilitario {
    // Métodos de validação de entrada
    public boolean ehEmailValido(String email) { /* ... */ }
    public boolean ehCPFValido(String cpf) { /* ... */ }
    
    // Métodos de manipulação de data (não relacionados à validação)
    public LocalDate adicionarDias(LocalDate data, int dias) { /* ... */ }
    public LocalDate primeiroDiaDoMes(LocalDate data) { /* ... */ }
    
    // Métodos de formatação de string (não relacionados aos acima)
    public String removerEspacos(String texto) { /* ... */ }
    public String formatarMoeda(BigDecimal valor) { /* ... */ }
    
    // Métodos de acesso a banco de dados (completamente não relacionados)
    public List<Usuario> buscarUsuariosAtivos() { /* ... */ }
    public void salvarLog(String mensagem) { /* ... */ }
    
    // Mais métodos aleatórios...
}
```

Média coesão (lógica - melhor que coincidental):
```java
// ✅ MELHOR: Métodos relacionados por serem logicamente similares (validação)
public class ValidadorDeEntrada {
    public boolean ehEmailValido(String email) { /* ... */ }
    public boolean ehCPFValido(String cpf) { /* ... */ }
    public boolean ehTelefoneValido(String telefone) { /* ... */ }
    public boolean ehCNPJValido(String cnpj) { /* ... */ }
    public boolean ehCEPValido(String cep) { /* ... */ }
    
    // Ainda poderia ser melhor se focássemos apenas em validação de documentos fiscais, por exemplo
}
```

Alta coesão (funcional - melhor tipo):
```java
// ✅ CORRETO: Todos os métodos contribuem para uma única tarefa bem definida
public class ValidadorDeDocumentosFiscais {
    // Apenas validação de documentos fiscais brasileiros
    public boolean ehCPFValido(String cpf) { /* ... */ }
    public boolean ehCNPJValido(String cnpj) { /* ... */ }
    public boolean ehIEValido(String ie, String estado) { /* ... */ }
    public boolean ehIMValido(String im, String municipio) { /* ... */ }
    
    // Métodos auxiliares específicos para validação de documentos fiscais
    private boolean validarDigitosCPF(String cpf) { /* ... */ }
    private boolean validarDigitosCNPJ(String cnpj) { /* ... */ }
    private int calcularModulo11(String numero, int[] pesos) { /* ... */ }
}
```

### Exemplo de sistema de produção
Sistema de processamento de imagens:
- **Baixa coesão (ruim):** Classe `ProcessadorDeImagem` que faz redimensionamento, conversão de formato, aplicação de filtros, extração de metadados e salvamento em banco de dados
- **Média coesão (aceitável):** Pacote `processamento.imagem` onde:
  - `Redimensionador` faz apenas redimensionamento
  - `ConversorDeFormato` faz apenas conversão entre formatos (JPEG, PNG, GIF)
  - `AplicadorDeFiltro` faz apenas aplicação de filtros (desfoque, nitidez, etc.)
  - `ExtratorDeMetadados` faz apenas extração de dados como resolução, data de criação, etc.
  - `SalvadorEmBanco` faz apenas salvamento e recuperação de imagens do banco de dados
- **Alta coesão (ideal):** Cada classe acima tem um propósito único e bem definido, tornando-as fáceis de entender, testar e reutilizar

### Como esse assunto pode aparecer em uma entrevista.
> 🎯 **ENTREVISTA — MÉDIA FREQUÊNCIA**
> 
> "Descreva um cenário onde aumentar a coesão de um módulo poderia realmente piorar o design, e como você identificaria essa situação."
> 
> **Armadilha:** Sugerir que aumentar a coesão sempre melhora o design sem reconhecer que pode levar a fragmentos demais ou perda de coesão de nível superior.
> 
> **Como raciocinar:** Explicar que aumentar a coesão além do ponto funcional pode levar a módulos que fazem praticamente nada (como uma classe que apenas tem um método `calcularImposto()` quando o imposto sempre é 0%). Dar exemplos como separar getters e setters em classes diferentes, ou criar uma classe apenas para cada operação matemática básica. Mostrar que o equilíbrio está em identificar o nível certo de granularidade onde módulos têm propósito claro mas não são excessivamente fragmentados, e que às vezes coesão sequencial ou comunicacional em um nível superior é mais benéfico que coesão funcional em muitos módulos pequenos.

## Tipos de Acoplamento

> 🎯 **ENTREVISTA — MÉDIA FREQUÊNCIA**
> 
> Entender os diferentes tipos de acoplamento ajuda a avaliar e melhorar dependências entre módulos; entrevistadores querem ver se você conhece o espectro desde o pior até o melhor tipo.

### definição
Acoplamento mede o grau de dependência direta entre dois módulos - quão muito um módulo sabe ou depende do outro. Existem vários tipos de acoplamento, classificados do pior (conteúdo) ao melhor (nenhum/indireto), baseado em quão direta e forte é a dependência entre módulos.

### Por que existe?
Para fornecer uma linguagem comum e um framework para discutir a qualidade das dependências entre módulos, ajudando desenvolvedores a identificar quando módulos estão muito interdependentes e precisam ser desacoplados.

### Como funciona internamente?
Os tipos de acoplamento são tipicamente classificados da seguinte forma (do pior para o melhor):
1. **Conteúdo:** Um módulo modifica ou depende diretamente do funcionamento interno de outro módulo (pior tipo)
2. **Comum:** Módulos compartilham dados globais (variáveis globais, sistemas de arquivos compartilhados)
3. **Externo:** Módulos dependem de um ambiente externo ou protocolo de comunicação (formato de mensagem, tipo de mensagem)
4. **De controle:** Um módulo controla o fluxo de execução de outro (passando flags que alteram comportamento)
5. **De stamp:** Módulos compartilham uma estrutura de dados composta (registro, objeto) mas usam apenas partes diferentes dela
6. **De dados:** Módulos compartilham apenas dados primitivos ou estruturas simples através de parâmetros
7. **De mensagem:** Módulos se comunicam através de parâmetros oficiais sem compartilhar estruturas de dados (ideal, mas raro na prática)
8. **Nenhum:** Módulos são completamente independentes (melhor tipo teórico, raramente alcançável em sistemas não triviais)

### Como implementar?
1. **Identificar o tipo atual** de acoplamento entre módulos analisando suas dependências
2. **Buscar mover-se para baixo** na escala (de conteúdo para nenhum) através de refatoramento e uso de boas práticas
3. **Eliminar conteúdo e comum** acoplamento sempre que possível (sempre ruins)
4. **Minimizar externo, de controle e de stamp** acoplamento através de abstrações e encapsulamento
5. **Buscar acoplamento de dados** como aceitável quando necessário para comunicação
6. **Considerar padrões de projeto** como Mediator, Observer, etc. para reduzir acoplamento
7. **Usar injeção de dependência e interfaces** para converter acoplamento direto em dependência de abstrações
8. **Refatorar continuamente** à medida que se descobre novas dependências ou muda o entendimento do domínio
9. **Considerar o contexto** - às vezes algum nível de acoplamento é necessário e aceitável

### Quais são as alternativas?
- Ignorar acoplamento e focar apenas na funcionalidade correta
- Tentar eliminar todo acoplamento (pode levar a sobreengineering e perda de coesão)
- Aplicar métricas de acoplamento de forma mecânica sem entender o significado
- Focar apenas em reduzir o número de dependências sem considerar sua natureza
- Deixar o acoplamento como algo subjetivo sem tentar reduzi-lo ativamente

### Quais são os trade-offs?
**Vantagens de buscar menor acoplamento:**
- Código mais fácil de entender porque dependências entre módulos são explícitas e mínimas
- Facilidade de manutenção pois mudanças em um módulo são menos propensas a afetar outros
- Maior reutilização pois módulos com poucas dependências são mais prováveis de serem úteis em outros contextos
- Facilidade de teste pois é possível isolar unidades de código usando mocks ou stubs
- Redução de risco de regressão pois menos coisas podem quebrar quando se faz uma mudança
- Flexibilidade para trocar implementações sem afetar outros módulos
- Código que se adapta mais facilmente a mudanças de requisitos

**Desvantagens/custos:**
- Pode levar a overengineering se aplicado rigidamente a dependências simples e estáveis
- Pode aumentar inicialmente a complexidade devido a abstrações, interfaces e indireções adicionais
- Pode parecer indireto para desenvolvedores acostumados com dependências diretas e explícitas
- Pode haver inicialmente mais código de "cola" para coordenar entre módulos desacoplados
- Requer disciplina para manter conforme o código evolui
- Pode haver desempenho ligeiramente menor devido a indireções (geralmente insignificante)

### Quando usar?
- Sempre que se estiver projetando ou refatorando módulos de software
- Quando se percebe que módulos estão altamente interdependentes
- Quando se quer melhorar a legibilidade e compreensibilidade do código
- Quando múltiplos desenvolvedores vão trabalhar nos mesmos módulos
- Quando se quer facilitar teste, manutenção e reutilização
- Quando se está construindo sistemas que devem ter longa vida e múltiplas iterações

### Quando não usar?
- Quando se está prototipando e velocidade é a única prioridade
- Quando o acoplamento é tão simples e estável que reduzi-lo seria overengineering
- Quando se está em um ambiente altamente restrito onde cada módulo conta
- Quando se sabe com certeza que nenhuma mudança na dependência será necessária
- Quando a sobrecarga de redução de acoplamento não traz benefício proporcional

### Quais são os erros mais comuns?
- Achar que algum acoplamento é sempre ruim e tentando eliminá-lo completamente
- Confundir acoplamento com coesão (eles são conceitos diferentes, embora relacionados)
- Esquecer que acoplamento se aplica a todos os níveis: módulos dependendo de outros módulos, pacotes dependendo de outros pacotes
- Focar apenas em eliminar dependências diretas sem considerar que indireções também têm custo
- Aplicar redução de acoplamento apenas em algumas partes do código deixando outras partes com alto acoplamento
- Achar que qualquer introdução de abstração automaticamente reduz acoplamento (pode aumentar se feita incorretamente)
- Não considerar que alguns tipos de acoplamento (como de dados) podem ser legítimos e eficientes

### Como isso afeta:
- *performance:* Impacto depende do tipo de acoplamento (conteúdo e comum podem ser rápidos; indireções adicionam overhead mínimo)
- *escalabilidade:* Similar; baixo acoplamento geralmente melhora escalabilidade pois módulos podem ser escalados independentemente
- *disponibilidade:* Melhora pois falhas em um módulo são menos propensas a se propagar para outros
- *consistência:* Melhora pois comportamento de módulos é mais previsível quando menos dependente de outros
- *segurança:* Similar; acoplamento não afeta diretamente preocupações de segurança
- *custo:* Similar; foco em onde a dependência reside ao invés de adicionar ou remover código
- *observabilidade:* Melhora pois dependências explícitas são mais fáceis de instrumentar e monitorar
- *complexidade operacional:* Melhora devido a menos efeitos colaterais inesperados e mais facilidade de fazer mudanças

### Exemplos reais de aplicação
- Arquitetura hexagonal/limpa onde camadas de alto nível dependem apenas de interfaces (portas) de baixo nível, não de implementações específicas
- Sistemas de evento onde módulos se comunicam através de eventos publicados, não chamadas diretas
- Frameworks de injeção de dependência (Spring, Guice, Dagger) onde dependências são resolvidas em tempo de execução através de abstrações
- Arquiteturas de microserviços onde serviços se comunicam através de APIs bem definidas (HTTP/REST, gRPC), não compartilhamento de memória ou banco de dados direto
- Bibliotecas de logging onde lógica de aplicação depende de interface `Logger`, não de implementação específica como Log4j ou SLF4J

### Exemplo simplificado
Alto acoplamento (conteúdo - pior tipo):
```java
// ❌ ERRADO: Classe A modifica diretamente o funcionamento interno da classe B
public class ClasseA {
    public void modificarEstadoInternoDeB(ClasseB b) {
        // Acesso direto aos campos privados de B (quebrando encapsulamento)
        b.campoPrivado = novoValor;  // Alto acoplamento de conteúdo
        b.metodoPrivado();           // Chamada direta a método privado
    }
}

public class ClasseB {
    private String campoPrivado;  // Deveria ser privado
    
    private void metodoPrivado() {  // Deveria ser privado
        // Implementação interna
    }
    
    // Getters e setters públicos...
}
```

Médio acoplamento (de dados - aceitável):
```java
// ✅ ACEITÁVEL: Classe A depende apenas de dados simples passados como parâmetros
public class ClasseA {
    public void processarDados(String identificador, BigDecimal valor) {
        // Apenas usa os dados recebidos, não conhece internos de B
        ClasseB b = new ClasseB();
        b.processar(identificador, valor);  // Depende apenas da interface pública de B
    }
}

public class ClasseB {
    public void processar(String identificador, BigDecimal valor) {
        // Processa apenas os dados recebidos
        // Não expõe internos desnecessariamente
    }
}
```

Baixo acoplamento (de mensagem - melhor na prática):
```java
// ✅ MELHOR NA PRÁTICA: Comunicação através de mensagens/eventos bem definidos
public class ClasseA {
    private final PublicadorDeEventos publicador;
    
    public ClasseA(PublicadorDeEventos publicador) {
        this.publicador = publicador;
    }
    
    public void fazerAlgo() {
        // Publica evento sem saber quem vai receber ou como vai processar
        publicador.publicar(new EventoAlgoFeito(identificador, valor));
        // Nenhum conhecimento de quem processa o evento ou como
    }
}

public class ClasseB {
    private final RecebedorDeEventos recebedor;
    
    public ClasseB(RecebedorDeEventos recebedor) {
        this.recebedor = recebedor;
        // Se inscreve para receber eventos do tipo EventoAlgoFeito
        recebedor.inscrever(EventoAlgoFeito.class, this::processarEvento);
    }
    
    private void processarEvento(EventoAlgoFeito evento) {
        // Processa o evento sem saber quem o publicou
        // Apenas reage à mensagem recebida
    }
}

// Benefício: Classe A e Classe B são fracamente acopladas
// - A não sabe que B existe ou como processa eventos
// - B não sabe que A existe ou quem publica eventos
// - Ambos dependem apenas da abstração do sistema de eventos
```

### Exemplo de sistema de produção
Sistema de negociação de ações:
- **Alto acoplamento (ruim):** Classe `OrdensService` que diretamente instancia e usa classes como `ConexaoBolsaValores`, `ConversorDeMoeda`, `ValidadorDeRegulamento`, gerando dependências difíceis de trocar ou testar
- **Médio acoplamento (aceitável):** `OrdensService` depende de interfaces como `IBolsaValores`, `IMoedaConversor`, `IValidadorRegulamento`, permitindo trocar implementações
- **Baixo acoplamento (ideal):** 
  - `OrdensService` publica eventos como `OrdemCriada`, `OrdemExecutada` em um barramento de eventos
  - `BolsaValoresService` se inscreve para receber `OrdemCriada` e processa a execução
  - `NotificacaoService` se inscreve para receber `OrdemExecutada` e envia alertas aos traders
  - `AuditoriaService` se inscreve para receber todos os eventos de ordem para fins de conformidade
  - Nenhum módulo conhece detalhes de implementação ou existência direta de outros módulos além do contrato de eventos
  - É possível trocar o mecanismo de comunicação com a bolsa (de API direta para FIX protocol) sem afetar outros serviços
  - É possível adicionar novos tipos de processamento (como detecção de fraude) simplesmente se inscrevendo nos eventos relevantes

### Como esse assunto pode aparecer em uma entrevista.
> 🎯 **ENTREVISTA — MÉDIA FREQUÊNCIA**
> 
> "Explique como você reduziria o acoplamento entre dois serviços que atualmente têm dependência direta onde o Serviço A instancia e chama métodos específicos do Serviço B."
> 
> **Armadilha:** Sugerir apenas criar uma interface para o Serviço B sem considerar se a dependência deveria existir em primeiro lugar ou se deveria ser invertida (Serviço B dependendo de Serviço A) ou se deveria ser feita através de eventos.
> 
> **Como raciocinar:** Analisar se a dependência é realmente necessária ou se pode ser eliminada através de reorganização de responsabilidades. Se necessária, determinar o tipo de acoplamento atual e buscar reduzi-lo: converter dependência direta em dependência de abstração (interface), considerar injeção de dependência para tornar a dependência configurável, avaliar se comunicação assíncrona através de eventos ou mensagens seria mais apropriada, verificar se o Serviço B deveria realmente depender do Serviço A em vez do contrário, e sempre perguntar se existe uma maneira de reduzir ainda mais o acoplamento sem sacrificar funcionalidade ou clareza.

## Estratégias para Melhorar Coesão e Reduzir Acoplamento

> 💡 **DICA DE ENTREVISTA**
> 
> Sempre relacione a escolha ao contexto específico - não trate como regra universal.

### Melhorando a Coesão
1. **Extrair Classe:** Quando uma classe está fazendo muitas coisas não relacionadas, separar responsabilidades em classes distintas
2. **Extrair Método:** Quando um método está fazendo muitas coisas, separar em métodos menores e mais focados
3. **Extrair Subclasse:** Quando parte da funcionalidade varia independente do resto, usar herança para separar a parte variável
4. **Extrair Interface:** Quando várias classes compartilham um conjunto comum de métodos, criar uma interface para esse conjunto
5. **Formular Modelo de Objeto:** Quando estruturas de dados estão espalhadas, criar objetos que representam conceitos de domínio
6. **Extrair Pacote:** Quando um pacote está fazendo muitas coisas não relacionadas, separar em pacotes distintos
7. **Extrair Módulo:** Quando um componente está fazendo muitas coisas não relacionadas, separar em módulos ou serviços distintos
8. **Substituir Algoritmo:** Quando um algoritmo está confuso ou mal estruturado, substituir por uma versão mais clara e focada
9. **Consolidar Condicional Expressão:** Quando várias condições verificam a mesma coisa, consolidar em uma expressão única
10. **Consolidar Função Duplicada:** Quando a mesma lógica aparece em múltiplos lugares, extrair para um único local

### Reduzindo o Acoplamento
1. **Injeção de Dependência:** Depender de abstrações (interfaces) em vez de implementações concretas
2. **Interface Extrair:** Quando uma classe depende de muitos métodos de outra, extrair uma interface com apenas os métodos necessários
3. **Não Falar com Estrangeiros:** Um módulo não deve falar com múltiplas classes de outro módulo - usar um intermediário ou facade
4. **Mensageiro:** Usar mensagens ou eventos para comunicação em vez de chamadas diretas
5. **Observador:** Dependência unilateral onde um módulo se registra para receber notificações de outro
6. **Facade:** Fornecer uma interface simplificada para um subsistema complexo
7. **Mediator:** Usar um objeto centralizador para gerenciar comunicações entre múltiplos colegas
8. **Adapter:** Converter a interface de uma classe em outra interface esperada pelos clientes
9. **Proxy:** Fornecer um representante ou substituto para controlar acesso a outro objeto
10. **Liskov Substitution Principle:** Garantir que subclasses possam ser substituídas por suas classes base sem alterar a corretidão
11. **Dependency Inversion Principle:** Depender de abstrações, não de implementações
12. **Law of Demeter:** Um módulo deveria conhecer apenas seus amigos próximos (não falar com estranhos)
13. **Encapsular Campo:** Tornar campos privados e fornecer acesso através de métodos
14. **Encapsular Coleção:** Tornar coleções privadas e fornecer acesso somente-leitura ou métodos controlados
15. **Substituir Tipo por Classe:** Quando se usa tipos primitivos ou strings para representar conceitos de domínio, criar classes apropriadas
16. **Substituir Tipo por Subclasse:** Quando se usa códigos ou condições para determinar comportamento, usar subclasses
17. **Extrair Classe:** Quando uma classe está fazendo muitas coisas, separar responsabilidades
18. **Inline Classe:** Quando uma classe não está fazendo muito, mover suas responsabilidades para outra classe
19. **Hide Delegate:** Quando um cliente está chamando métodos em um delegate através de um wrapper, expor os métodos do delegate diretamente
20. **Remove Middle Man:** Quando um intermediário não está fazendo muito, fazer o cliente chamar o alvo diretamente
21. **Introduzir Foreign Method:** Quando se precisa de um método adicional em uma classe que não se pode modificar, criar o método no cliente
22. **Introduzir Local Extensão:** Quando se precisa de métodos adicionais em uma classe que não se pode modificar, criar uma classe wrapper

### Quando aplicar cada estratégia
- **Extrair Classe/Extrair Subclasse:** Quando coesão está baixa devido a múltiplas responsabilidades não relacionadas
- **Extrair Método/Extrair Interface:** Quando coesão está baixa dentro de um método ou classe devido a tarefas não relacionadas
- **Injeção de Dependência:** Quando acoplamento está alto devido a dependência direta em implementações concretas
- **Mensageiro/Observador:** Quando acoplamento está alto devido a chamadas diretas síncronas que criam dependências temporais
- **Facade/Mediator:** Quando acoplamento está alto devido a um módulo precisar conhecer muitos detalhes de outro subsistema
- **Law of Demeter:** Quando acoplamento está alto devido a cadeias longas de chamadas (obj.getA().getB().getC())
- **Observer:** Quando se quer reduzir acoplamento mantendo possibilidade de reação a mudanças
- **Mediator:** Quando múltiplos módulos precisam se comunicar e o acoplamento direto entre eles seria complexo

### Exemplo de aplicação de estratégias
Sistema original com baixa coesão e alto acoplamento:
```java
// ❌ ERRADO: Baixa coesão (múltiplas responsabilidades) e alto acoplamento (dependências diretas)
public class LojaVirtual {
    // Múltiplas responsabilidades: catalogo, pagamento, estoque, notificacao, relatorio
    
    // Alta coesão interna baixa: métodos misturam diferentes responsabilidades
    public void processarPedido(String produtoId, int quantidade, String tipoPagamento) {
        // 1. Validação de entrada (responsabilidade de validação)
        if (produtoId == null || quantidade <= 0) {
            throw new IllegalArgumentException("Dados inválidos");
        }
        
        // 2. Busca no catalogo (responsabilidade de catalogo)
        Produto produto = catalogoRepositorio.buscarPorId(produtoId);
        if (produto == null) {
            throw new IllegalArgumentException("Produto não encontrado");
        }
        
        // 3. Verificação de estoque (responsabilidade de estoque)
        if (!estoqueServico.temEstoqueSuficiente(produtoId, quantidade)) {
            throw new IllegalStateException("Estoque insuficiente");
        }
        
        // 4. Processamento de pagamento (responsabilidade de pagamento)
        Pagamento pagamento = null;
        if ("cartao".equals(tipoPagamento)) {
            pagamento = gatewayCartao.processar(produto.getPreco().multiply(new BigDecimal(quantidade)));
        } else if ("boleto".equals(tipoPagamento)) {
            pagamento = gatewayBoleto.gerarBoleto(produto.getPreco().multiply(new BigDecimal(quantidade)));
        }
        
        // 5. Baixa estoque (responsabilidade de estoque)
        estoqueServico.baixarEstoque(produtoId, quantidade);
        
        // 6. Criação de pedido (responsabilidade de dominio)
        Pedido pedido = new Pedido(produto, quantidade, pagamento);
        pedidoRepositorio.salvar(pedido);
        
        // 7. Envio de notificação (responsabilidade de notificacao)
        notificacaoServico.enviarConfirmacao(pedido);
        
        // 8. Geração de relatório (responsabilidade de relatorio)
        relatorioServico.registrarPedido(pedido);
    }
}
```

Após aplicar estratégias de melhoria:
```java
// ✅ CORRETO: Alta coesão e baixo acoplamento após refatoramento

// Classe com alta coesão: apenas validação de entrada para pedidos
public class ValidadorDePedido {
    public void validar(String produtoId, int quantidade) {
        if (produtoId == null || quantidade <= 0) {
            throw new IllegalArgumentException("Dados inválidos: produtoId não pode ser nulo e quantidade deve ser positiva");
        }
    }
}

// Classe com alta coesão: apenas gerenciamento de catalogo de produtos
public class CatalogoProdutos {
    private final ProdutoRepositorio produtoRepositorio;
    
    public CatalogoProdutos(ProdutoRepositorio produtoRepositorio) {
        this.produtoRepositorio = produtoRepositorio;
    }
    
    public Produto buscarPorId(String id) {
        Produto produto = produtoRepositorio.buscarPorId(id);
        if (produto == null) {
            throw new IllegalArgumentException("Produto não encontrado: " + id);
        }
        return produto;
    }
}

// Classe com alta coesão: apenas verificação e gerenciamento de estoque
public class ServicoDeEstoque {
    private final EstoqueRepositorio estoqueRepositorio;
    
    public ServicoDeEstoque(EstoqueRepositorio estoqueRepositorio) {
        this.estoqueRepositorio = estoqueRepositorio;
    }
    
    public boolean temEstoqueSuficiente(String produtoId, int quantidade) {
        return estoqueRepositorio.obterQuantidadeDisponivel(produtoId) >= quantidade;
    }
    
    public void baixarEstoque(String produtoId, int quantidade) {
        estoqueRepositorio.baixarQuantidade(produtoId, quantidade);
    }
}

// Classe com alta coesão: apenas processamento de pagamentos
public interface ProcessadorDePagamento {  // Abstração para baixo acoplamento
    Pagamento processar(BigDecimal valor);
}

public class ProcessadorPagamentoCartao implements ProcessadorDePagamento {
    public Pagamento processar(BigDecimal valor) {
        // Implementação específica para cartão de crédito
        return gatewayCartao.processar(valor);
    }
}

public class ProcessadorPagamentoBoleto implements ProcessadorDePagamento {
    public Pagamento processar(BigDecimal valor) {
        // Implementação específica para boleto
        return gatewayBoleto.gerarBoleto(valor);
    }
}

// Classe com alta coesão: apenas gerenciamento de pedidos de domínio
public class GerenciadorDePedidos {
    private final PedidoRepositorio pedidoRepositorio;
    
    public GerenciadorDePedidos(PedidoRepositorio pedidoRepositorio) {
        this.pedidoRepositorio = pedidoRepositorio;
    }
    
    public Pedido criarPedido(Produto produto, int quantidade, Pagamento pagamento) {
        Pedido pedido = new Pedido(produto, quantidade, pagamento);
        return pedidoRepositorio.salvar(pedido);
    }
}

// Classe com alta coesão: apenas envio de notificações
public interface ServicoDeNotificacao {  // Abstração para baixo acoplamento
    void enviarConfirmacao(Pedido pedido);
}

public class ServicoNotificacaoEmail implements ServicoDeNotificacao {
    public void enviarConfirmacao(Pedido pedido) {
        // Implementação específica de email
        emailService.enviar(
                pedido.getCliente().getEmail(),
                "Confirmação do Pedido #" + pedido.getId(),
                "Seu pedido foi recebido e está sendo processado."
        );
    }
}

// Classe com alta coesão: apenas geração de relatórios
public class ServicoDeRelatorios {
    private final RelatorioRepositorio relatorioRepositorio;
    
    public ServicoDeRelatorios(RelatorioRepositorio relatorioRepositorio) {
        this.relatorioRepositorio = relatorioRepositorio;
    }
    
    public void registrarPedido(Pedido pedido) {
        relatorioRepositorio.registrarPedido(pedido);
    }
}

// Orquestrador com baixa coesão intencional (apenas coordena fluxo de negócio)
public class ProcessadorDePedidos {
    private final ValidadorDePedido validador;
    private final CatalogoProdutos catalogo;
    private final ServicoDeEstoque estoque;
    private final ProcessadorDePagamento processadorPagamento;
    private final GerenciadorDePedidos gerenciadorPedidos;
    private final ServicoDeNotificacao notificacao;
    private final ServicoDeRelatorios relatorios;
    
    // Construtor com injeção de dependência (baixo acoplamento)
    public ProcessadorDePedidos(
            ValidadorDePedido validador,
            CatalogoProdutos catalogo,
            ServicoDeEstoque estoque,
            ProcessadorDePagamento processadorPagamento,
            GerenciadorDePedidos gerenciadorPedidos,
            ServicoDeNotificacao notificacao,
            ServicoDeRelatorios relatorios) {
        this.validador = validador;
        this.catalogo = catalogo;
        this.estoque = estoque;
        this.processadorPagamento = processadorPagamento;
        this.gerenciadorPedidos = gerenciadorPedidos;
        this.notificacao = notificacao;
        this.relatorios = relatorios;
    }
    
    public void processarPedido(String produtoId, int quantidade, String tipoPagamento) {
        // Alta coesão: apenas orquestra o processo de pedido de acordo com regras de negócio
        
        // 1. Validação de entrada
        validador.validar(produtoId, quantidade);
        
        // 2. Busca no catalogo
        Produto produto = catalogo.buscarPorId(produtoId);
        
        // 3. Verificação de estoque
        if (!estoque.temEstoqueSuficiente(produtoId, quantidade)) {
            throw new IllegalStateException("Estoque insuficiente para o produto: " + produtoId);
        }
        
        // 4. Processamento de pagamento (baixo acoplamento: depende de abstraçāo)
        ProcessadorDePagamento processador;
        if ("cartao".equals(tipoPagamento)) {
            processador = new ProcessadorPagamentoCartao();
        } else if ("boleto".equals(tipoPagamento)) {
            processador = new ProcessadorPagamentoBoleto();
        } else {
            throw new IllegalArgumentException("Tipo de pagamento não suportado: " + tipoPagamento);
        }
        Pagamento pagamento = processador.processar(produto.getPreco().multiply(new BigDecimal(quantidade)));
        
        // 5. Baixa estoque
        estoque.baixarEstoque(produtoId, quantidade);
        
        // 6. Criação de pedido
        Pedido pedido = gerenciadorPedidos.criarPedido(produto, quantidade, pagamento);
        
        // 7. Notificação (baixo acoplamento: depende de abstraçāo)
        notificacao.enviarConfirmacao(pedido);
        
        // 8. Relatório (baixo acoplamento: depende de abstraçāo)
        relatorios.registrarPedido(pedido);
    }
}
```

Benefícios alcançados:
- **Alta coesão:** Cada classe tem uma única, bem definida responsabilidade
- **Baixo acoplamento:** Módulos dependem de abstrações, não de implementações concretas
- **Facilidade de teste:** É possível mockar cada dependência isoladamente
- **Facilidade de mudança:** É possível trocar gateway de pagamento, serviço de email ou mecanismo de estoque sem modificar lógica de negócio
- **Clareza:** O fluxo de negócio é fácil de seguir no método `processarPedido`
- **Reutilização:** Classes como `ValidadorDePedido`, `CatalogoProdutos` e `ServicoDeEstoque` podem ser usadas em outros contextos

## Mensagem para Levar

> 💡 **DICA DE ENTREVISTA**
> 
> Sempre relacione a escolha ao contexto específico - não trate como regra universal.

Coesão e acoplamento são dois lados da mesma moeda: ambos medem aspectos da qualidade do design de software, mas focam em direções diferentes. Enquanto a coesão olha para dentro de um módulo (quão bem suas partes pertencem juntas), o acoplamento olha para fora (quão dependente um módulo está de outros).

### Princípios-Chave
1. **Busque alta coesão:** Cada módulo deve ter uma única, bem definida responsabilidade
2. **Busque baixo acoplamento:** Módulos devem depender o mínimo possível uns dos outros, preferindo abstrações sobre concretas
3. **Equilibre os dois:** Nem sempre é possível ou desejável ter coesão funcional perfeita ou acoplamento zero - o objetivo é encontrar o melhor equilíbrio para o contexto
4. **Aplique continuamente:** Melhorar coesão e reduzir acoplamento não é uma atividade única, mas um processo contínuo de refatoramento
5. **Considere o contexto:** O que constitui boa coesão e baixo acoplamento pode variar dependendo do domínio, tamanho da equipe e restrições do projeto
6. **Use métricas como guia, não como regra:** Métricas podem ajudar a identificar problemas, mas julgamento profissional é sempre necessário
7. **Lembre-se dos trade-offs:** Toda decisão de design envolve trade-offs - entenda-os e faça escolhas informadas
8. **Prefira simplicidade:** Quando em dúvida, opte pela solução mais simples que atenda aos requisitos
9. **Teste seu design:** Código com alta coesão e baixo acoplamento é mais fácil de testar unitariamente
10. **Evolua com o sistema:** À medida que o sistema cresce e muda, revise continuamente coesão e acoplamento para garantir que ainda sejam apropriados

### Perguntas-Chave para Autoavaliação
- **Sobre coesão:** Este módulo tem apenas uma razão para mudar? Se eu precisar alterar algo aqui, estarei afetando apenas uma área de responsabilidade?
- **Sobre acoplamento:** Se eu mudar este módulo, quantos outros módulos provavelmente serão afetados? Posso reduzir esse número dependendo de abstrações em vez de concretas?
- **Sobre ambos:** Este módulo está fazendo mais do que deveria ou dependendo de mais do que deveria?

### Aplicação Prática
Ao projetar ou refatorar código, pergunte-se constantemente:
1. Qual é a única responsabilidade deste módulo?
2. O que este módulo realmente precisa saber sobre outros módulos para cumprir sua responsabilidade?
3. Como posso minimizar o que este módulo precisa saber sobre outros módulos?
4. Como posso tornar as dependências deste módulo explícitas e configuráveis?
5. Este módulo é fácil de entender, testar e modificar?
6. Se eu precisar reutilizar este módulo em outro contexto, quanto trabalho seria necessário?

### Checklist Final
- [ ] Analisei a coesão de cada módulo para garantir que tenha uma única responsabilidade bem definida?
- [ ] Separei responsabilidades não relacionadas em módulos distintos?
- [ ] Verifiquei se métodos dentro de cada módulo trabalham juntos para um propósito claro?
- [ ] Analisei o acoplamento entre módulos para minimizar dependências diretas?
- [ ] Substituí dependências em implementações concretas por dependências em abstrações?
- [ ] Usei injeção de dependência para tornar dependências configuráveis?
- [ ] Apliquei o Law of Demeter para reduzir cadeias longas de chamadas?
- [ ] Considerei usar padrões como Observer ou Mediator para reduzir acoplamento quando apropriado?
- [ ] Garanti que módulos não dependam de detalhes internos de outros módulos (encapsulamento)?
- [ ] Verifiquei se meu código é mais fácil de entender, modificar e testar após melhorar coesão e reduzir acoplamento?
- [ ] Meus testes de unidade são isolados graças ao baixo acoplamento e uso de abstrações?
- [ ] Documentei exemplos reais de aplicação, exemplos simplificados e exemplos de sistemas de produção?
- [ ] Expliquei como esse assunto pode aparecer em uma entrevista e forneci respostas esperadas?
- [ ] Incluí exercícios de diferentes níveis para fixar o aprendizado?