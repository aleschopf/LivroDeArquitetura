---
trilha: "AVANÇADA"
---
**Navegação:** [[MOC — TRILHA AVANÇADA]]
← [[PARTE 43 — CONTAINERS]] | #trilha/avancada | [[PARTE 45 — SERVERLESS]] →

---
# PARTE 43 — KUBERNETES

> 🧠 **ESSENCIAL**
> Kubernetes é um sistema de orquestração de containers de código aberto que automatiza o deployment, escalonamento e gerenciamento de aplicações containerizadas, fornecendo uma plataforma para executar sistemas distribuídos de forma resiliente e escalável.

> 🎯 **ENTREVISTA — ALTA FREQUÊNCIA**
> Perguntas sobre arquitetura do Kubernetes (control plane, worker nodes), objetos principais (Pods, Deployments, Services), estratégias de deploy (rolling update, blue/green), gerenciamento de estado com StatefulSets, e padrões de serviço (Service Mesh, Ingress) são extremamente comuns em entrevistas de arquitetura de software.

## Arquitetura do Kubernetes

### Componentes do Master Node (Control Plane)
O control plane gerencia o estado do cluster e toma decisões globais (agendamento, escalonamento, atualizações).

1. **API Server (kube-apiserver)**:
   - Interface frontal para o control plane
   - Expõe a API Kubernetes via HTTP/HTTPS
   - Processa e valida todas as operações (RESTful)
   - Armazena estado no etcd
   - Único componente que se comunica diretamente com o etcd

2. **etcd**:
   - Armazenamento chave-valor distribuído e consistente
   - Guarda todo o estado do cluster (configuração, estado desejado, estado atual)
   - Usa protocolo Raft para consenso
   - Crítico para recuperação de desastre (precisa de backup regular)

3. **Scheduler (kube-scheduler)**:
   - Responsável por asignar Pods aos Nodes
   - Considera requisitos de recursos, affinidades/anti-afinidades, taints/tolerations
   - Algoritmo de agendamento em duas fases: filtragem e pontuação
   - Pode ser estendido com schedulers personalizados

4. **Controller Manager (kube-controller-manager)**:
   - Processo que executa controladores reguladores
   - Cada controlador gere um aspecto específico do estado do cluster
   - Exemplos: Node Controller, Replication Controller, Endpoints Controller
   - Trabalha em loop: observa estado atual, compara com estado desejado, toma ações corretivas

5. **Cloud Controller Manager (kube-cloud-controller-manager)**:
   - Integra o Kubernetes com provedores de nuvem externos
   - Separa lógica que interage com a nuvem dos componentes core do Kubernetes
   - Controladores específicos de nuvem: Node Controller, Route Controller, Service Controller

### Componentes do Worker Node
Worker Nodes são máquinas onde as cargas de trabalho onde os Pods (containers) são executados.

1. **Kubelet**:
   - Agente que roda em cada node
   - Garante que containers estejam rodando em Pods
   - Comunica-se com o API Server para receber instruções
   - Gerencia ciclo de vida de containers (via container runtime)
   - Reporta status do node e dos Pods de volta ao master

2. **Kube-proxy**:
   - Proxy de rede que roda em cada node
   - Implementa parte do conceito de Service do Kubernetes
   - Mantém regras de rede para encaminhamento de conexões
   - Pode operar em modos: userspace, iptables, IPVS
   - Responsável pelo load balancing de serviços

3. **Container Runtime**:
   - Software responsável por executar containers
   - Exemplos: Docker, containerd, CRI-O, rkt
   - Interface com o Kubelet através do CRI (Container Runtime Interface)
   - Gerencia download de imagens, execução e monitoramento de containers

## Objetos Kubernetes Essenciais

### Pod: Unidade Básica de Implantação
- Menor objeto que pode ser criado e gerenciado no Kubernetes
- Encapsula um ou mais containers tightly coupled
- Compartilha namespace de rede, IPC e volumes de storage
- Tem um único endereço IP por pod
- Containers dentro de um pod podem comunicar via localhost
- Modelo de ciclo de vida: Pending → Running → Succeeded/Failed/Unknown

### Nome e Namespaces
- Pods são criados dentro de namespaces (padrão: "default")
- Namespaces proporcionam escopo para nomes e divisão de recursos
- Namespaces comuns: default, kube-system, kube-public, kube-node-lease
- Nomes devem ser únicos dentro de um namespace

### Labels e Seletores
- Labels: pares chave-valor anexados a objetos para identificação
- Seletores: filtram objetos baseado em labels (ex: app=nginx, env in (prod,staging))
- Base para agrupamento e seleção de objetos em controladores e serviços

### Annotations
- Metadados não-identificadores anexados a objetos
- Usado por ferramentas e bibliotecas para armazenar informação adicional
- Não usado por seletores (diferente de labels)

## Controladores e Cargas de Trabalho

### ReplicaSet
- Garante que um número específico de réplicas de um Pod esteja sempre ativo
- Substitui o antigo Replication Controller
- Usado raramente diretamente (geralmente através de Deployments)
- Seleciona pods baseado em seu template de label
- Executa operações de criação/exclusão para manter a contagem desejada

### Deployment
- Objeto de alto nível para gerenciamento de aplicações stateless
- Gerencia ReplicaSets e fornece atualizações declarativas
- Recursos: atualizações rolling, rollback, pausar/continuar estratégias
- Estratégias de atualização:
  - RollingUpdate (padrão): substitui pods gradualmente
  - ReCreate: mata todos os pods antigos antes de criar novos
- Permite definir limites de surge e indisponibilidade durante atualizações

### StatefulSet
- Gerencia aplicações stateful com identidade estável e armazenamento persistente
- Garante ordem de deploy e scaling (ordinal-based)
- Cada pod tem identidade de rede estável (hostname: nome-statefulset-índice)
- Associado a volumes persistentes específicos por réplica
- Atualizações ordenadas de 0 a N-1 (para delete) e de N-1 a 0 (para create)
- Usado para bancos de dados, sistemas distribuídos que requerem identidade estável

### DaemonSet
- Garante que uma cópia de um Pod rode em todos (ou alguns) Nodes
- Usado para agentes de sistema: logs, monitoramento, coletadores de métricas
- Pods são criados automaticamente quando novos nodes são adicionados ao cluster
- Pode ser limitado a nodes específicos usando seletores de node
- Atualizações seguem padrão similar ao Deployment (rolling update por padrão)

### Job e CronJob
- **Job**: executa uma tarefa até conclusão (executa com sucesso um número especificado de vezes)
  - Paralelismo controlado por paralellism e completions
  - Pode ter política de backoff para falhas
  - Pods não são reinicializados após conclusão bem-sucedida
- **CronJob**: Jobs que rodam em schedule baseado em cron
  - Útil para tarefas periódicas: backups, relatórios, manutenção
  - Pode ter política de concorrência (allow, forbid, replace)
  - Histórico de execuções pode ser limitado para limpeza

## Serviços e Networking

### Service: Abstração para Exposição de Aplicações
Define um conjunto lógico de Pods e uma política para acessá-los.
- Disocia consumidores de implementações específicas de pods
- Fornece IP estável e nome DNS dentro do cluster
- Tipos de Service:
  - **ClusterIP** (padrão): acessível apenas dentro do cluster
  - **NodePort**: expõe porta em cada Node (IP:NodePort)
  - **LoadBalancer**: provê balanceador de carga externo (em nuvens suportadas)
  - **ExternalName**: mapeia serviço para nome DNS externo (não proxy)

### Descoberta de Serviço
- Variáveis de ambiente: Docker-style links (legado, não recomendado)
- DNS: Kubernetes oferece serviço DNS interno (kube-dns ou CoreDNS)
  - Formato: `<service-name>.<namespace>.svc.cluster.local`
  - Pods podem resolver serviços por nome automaticamente
  - Headless services (ClusterIP=None) retornam múltiplos IPs (um por pod)

### Ingress: Gerenciamento de Acesso Externo HTTP/HTTPS
- API object que gerencia acesso externo aos serviços em um cluster
- Fornece balanceamento de carga, terminação SSL, hospedagem baseada em nome
- Requer um Ingress Controller para funcionar (ex: NGINX, Traefik, AWS ALB)
- Regras definem como rotear tráfego baseado em host e caminho
- Annotations permitem configuração específica do controlador

### Network Policies
- Especifica como grupos de pods podem comunicar entre si e com outros endpoints de rede
- Funciona como firewall em nível de aplicação para pods
- Baseado em rótulos (labels) para selecionar pods
- Pode controlar tráfego de entrada (ingress) e saída (egress)
- Requer um plugin de rede que suporte network policies (ex: Calico, Cilium, Weave Net)

## Armazenamento e Estado

### Volumes
- Diretório acessível aos containers em um pod
- Vida útil vinculada ao pod (não ao container)
- Tipos de volumes:
  - **emptyDir**: diretório vazio inicialmente, dados perdidos quando pod é removido
  - **hostPath**: monta arquivo ou diretório do node host (perigoso, não portável)
  - **network storage**: NFS, iSCSI, discos de cloud provider (AWS EBS, Azure Disk)
  - **cloud provider volumes**: específicos da plataforma (ex: AWS EBS, GCP Persistent Disk)
  - **persistentVolumeClaim (PVC)**: solicitação de armazenamento por um usuário

### Persistent Volume (PV) e Persistent Volume Claim (PVC)
- **PV**: peça de armazenamento provisionada no cluster (manual ou dinamicamente)
  - Recurso de cluster assim como um node
  - Tem capacidade e modos de acesso (ReadWriteOnce, ReadOnlyMany, ReadWriteMany)
  - Pode ser estático (pré-provisionado) ou dinâmico (via StorageClass)
- **PVC**: solicitação de armazenamento por um usuário
  - Consumo de PV (bind entre PVC e PV)
  - Especifica tamanho, modos de acesso e storage class
  - Pods usam PVCs como volumes

### StorageClass
- Descreve "classes" de armazenamento (ex: ouro, prata, bronze ou desempenho)
- Permite provisionamento dinâmico de volumes
- Contém provisionador (ex: kubernetes.io/aws-ebs) e parâmetros específicos
- Pode definir políticas de retenção (Retain, Delete, Recycle)
- Classe padrão pode ser definida para PVCs sem storageClassName explícito

### StatefulSets com Armazenamento Persistente
- Cada réplica em um StatefulSet tem seu próprio PVC
- VolumeClaimTemplates no StatefulSet especificam o PVC a ser criado
- PVCs são vinculados a PVs com mesmo nome padrão: `<statefulset-name>-<pvc-name>-<ordinal>`
- Quando um pod é reagendado, ele se reconecta ao mesmo PVC (mesmo nome ordinal)
- Garantia de estado estável mesmo após reagendamento de pods

## Configuração e Secrets

### ConfigMaps
- Armazena dados de configuração não-sensíveis em pares chave-valor
- Pode ser usado como:
  - Variáveis de ambiente em containers
  - Argumentos de linha de comando
  - Arquivos em volume montado
- Pode conter dados simples ou arquivos completos (como arquivos de configuração)
- Atualizações podem ser propagadas para pods dependendo do tipo de uso

### Secrets
- Armazena dados sensíveis (senhas, tokens, chaves) em formato base64
- Tipos de secret:
  - **Opaque**: dados arbitrários (padrão)
  - **kubernetes.io/service-account-token**: token de serviço automático
  - **kubernetes.io/dockercred Credenciais de registro Docker
  - **kubernetes.io/tls**: certificados TLS
  - **kubernetes.io/basic-auth**: credenciais de autenticação básica
- Pode ser usado como variáveis de ambiente ou arquivos em volume montado
- Dados são armazenados como base64 no etcd (deveria ser criptografado em repouso)
- Pode ser criado a partir de literais, arquivos ou manifests

### Service Accounts
- Fornece identidade para processos rodando em pods
- Automáticamente associado a pods quando nenhum service account é especificado
- Tokens são montados em `/var/run/secrets/kubernetes.io/serviceaccount/`
- Pode ser associado a roles e clusterroles via RoleBindings para autorização
- Gerenciado automaticamente pelo controlador de service account

## Estratégias de Deploy e Atualização

### Atualizações Rolling (Padrão em Deployments)
- Substitui pods antigos por novos gradualmente
- Controlado por parâmetros:
  - `maxSurge`: número máximo de pods que podem ser criados além do desejado
  - `maxUnavailable`: número máximo de pods que podem estar indisponíveis durante atualização
- Exemplos de configuração:
  - `maxSurge: 25%, maxUnavailable: 25%` (padrão)
  - `maxSurge: 1, maxUnavailable: 0` (garante disponibilidade total)
  - `maxSurge: 0, maxUnavailable: 50%` (acelera atualização permitindo metade indisponível)

### Estratégia ReCreate
- Mata todos os pods antigos antes de criar novos
- Causa downtime durante a transição
- Útil quando aplicações não podem coexistir com versões diferentes
- Configurado especificando `type: ReCreate` na estratégia de deployment

### Deployments Blue/Green
- Mantém duas ambientes idênticos (blue e verde)
- Tráfego direcionado para um ambiente enquanto o outro é atualizado
- Implementado usando dois deployments separados e serviço com seletores
- Troca de tráfego feita atualizando seletor do serviço
- Permite teste rápido e rollback imediato

### Deployments Canary
- Libera nova versão para um pequeno subset de usuários inicialmente
- Gradualmente aumenta percentual de tráfego para nova versão
- Implementado usando múltiplos deployments com diferentes seletores
- Pode ser feito com service mesh (Istio, Linkerd) para controle granular de tráfego
- Permite teste em produção com risco limitado

### Estratégias de Rollback
- Deployments mantêm histórico de réplicasets para permitir rollback
- `kubectl rollout undo deployment/<name>` retorna à revisão anterior
- Pode especificar revisão específica: `kubectl rollout undo deployment/<name> --to-revision=3`
- Histórico de revisões pode ser limitado (revisionHistoryLimit)
- Rollback também possível manipulando diretamente o manifesto e aplicando novamente

## Escalonamento

### Escalonamento Horizontal de Pods (HPA)
- Ajusta automaticamente número de réplicas em um deployment, replica set ou stateful set
- Baseado em métricas observadas (CPU, memoria, métricas customizadas)
- Controla o especificador de réplicas do objeto alvo
- Requer metrics server instalado no cluster
- Configura alvo de utilização (ex: 50% CPU) e limites mínimos/máximos de réplicas

### Escalonamento Vertical de Pods (VPA)
- Ajusta automaticamente requests e limites de recursos de containers
- Pode atualizar pods em tempo real (requere reinicialização) ou recomendar apenas
- Três modos: Off, Initial (apenas no deploy), Auto (atualiza em tempo real)
- Útil para ajustar provisionamento baseado em uso observado
- Pode conflitar com HPA se ambos ajustarem o mesmo recurso (geralmente configurado para métricas diferentes)

### Escalonamento de Cluster (Cluster Autoscaler)
- Ajusta automaticamente o tamanho do nódo cluster (adiciona/remove nodes)
- Baseado na incapacidade de agendar pods e subutilização de nodes
- Integra-se com provedores de nuvem (AWS Auto Scaling Groups, GCP Managed Instance Groups)
- Opera em nós groups com templates de lançamento similares
- Balanceia entre necessidade de agendar pods e custo de nós ociosos

## Segurança no Kubernetes

### RBAC (Controle de Acesso Baseado em Funções)
- Autoriza usuários e processos a realizar ações no cluster
- Recursos principais:
  - **Role**: conjunto de permissões dentro de um namespace
  - **ClusterRole**: conjunto de permissões em todo o cluster
  - **RoleBinding**: concede permissões de um Role a usuários/grupos/service accounts em um namespace
  - **ClusterRoleBinding**: concede permissões de um ClusterRole em todo o cluster ou nomespace específico
- Permissões são definidas por verbs (get, list, watch, create, update, patch, delete) e recursos (pods, services, deployments, etc.)
- Pode restringir acesso a nomespecificos recursos ou nomes de recursos

### Segurança de Pods (Pod Security Standards)
- Conjunto de políticas para isolar pods e limitar privilégios
- Três níveis pré-definidos:
  - **Privileged**: sem restrições (não recomendado para cargas de trabalho)
  - **Baseline**: minimiza privilégios conhecida para permitir cargas de trabalho comuns
  - **Restricted**: fortemente restrito, segue melhores práticas atuais de segurança
- Implementado através de Pod Security Admission (PSA) ou PodSecurityPolicy (depreciado)
- Política pode ser aplicada em nível de namespace ou cluster

### Segurança de Runtime
- **Seccomp**: restringe chamadas de sistema que containers podem fazer
- **AppArmor/SELinux**: restringe capacidades dos processos através de perfis de segurança
- **Capabilities Linux**: retira todos os privilégios e adiciona apenas os necessários
- **RunAsUser**: executa containers como usuário não-root
- **ReadOnlyRootFilesystem**: monta sistema de arquivos raiz como somente leitura
- **AllowPrivilegeEscalation**: impede que processos ganhem mais privilégios que seus pais

### Segurança de Rede
- **Network Policies**: controla tráfego de entrada e saída para pods
- **Service Mesh**: fornece mTLS, autorização e políticas de tráfego entre serviços (Istio, Linkerd)
- **API Server**: protege acesso à API com autenticação, autorização e criptografia
- **etcd**: deve ser protegido (acesso restrito, criptografia em repouso, backups seguros)
- **Kubelet**: protege acesso ao kubelet (autenticação, autorização, TLS)

### Gerenciamento de Imagens Seguras
- **Image Policy Webhook**: bloqueia imagens baseadas em políticas (assinatura, origem, vulnerabilidades)
- **ImagePullSecrets**: credenciais para puxar imagens de registries privados
- **Trivy, Clair, Anchore**: ferramentas de scanning de vulnerabilidades em imagens
- **Cosign, Sigstore**: assinatura e verificação de provenance de imagens
- **SBOM (Software Bill of Materials)**: rastreamento de componentes em imagens

## Monitoramento, Logging e Tracing

### Métricas e Monitoramento
- **Metrics Server**: coleta métricas de recursos (CPU, memória) de nodes e pods
- **Prometheus**: sistema de monitoramento e alerta amplamente adotado
  - Descobre alvos automaticamente através de service discovery do Kubernetes
  - Métricas padronizadas: kube_state_metrics, container_* metrics, aplicação metrics
  - Alertas baseados em regras (PromQL)
- **Grafana**: plataforma de visualização que trabalha com Prometheus e outras fontes
- **Dashboard Kubernetes**: interface web básica para visualização de recursos do cluster
- **cAdvisor**: coletor de métricas de containers integrado ao Kubelet

### Logging
- Containers devem escrever logs para stdout/stderr (padrão de container)
- **Fluentd/Fluent Bit**: agentes de coleta de logs que enviam para backend
- **Elasticsearch, Logstash, Kibana (ELK)**: stack popular para armazenamento e visualização de logs
- **Loki**: sistema de agregação de logs levemente acoplado ao Prometheus
- **EFK (Elasticsearch, Fluentd, Kibana)**: variação popular do ELK
- Logs podem ser enriquecidos com metadata do Kubernetes (namespace, pod name, labels)

### Tracing Distribuído
- Propagação de contexto de trace entre serviços (W3C Trace Context)
- **Jaeger**: sistema de tracing distribuído open source da Uber (agora CNCF)
- **Zipkin**: sistema de tracing distribuído inspirado no Dapper do Google
- **AWS X-Ray**: serviço de tracing gerenciado da AWS
- Integração com frameworks de aplicação (Spring Cloud, OpenTelemetry, etc.)
- Correlação de traces com logs e métricas para observabilidade completa (três pilares)

## Service Mesh

### Problemas que o Service Mesh Resolve
À medida que o número de microserviços cresce, gerenciar comunicação, segurança e observabilidade se torna complexo:
- Observabilidade: dificuldade em obter métricas, logs e traces distribuídos
- Segurança: necessidade de mTLS, autorização e políticas de acesso entre serviços
- Confiabilidade: necessidade de timeouts, retries, circuit breaking, detecção de falhas
- Tráfego: dificuldade em controlar roteamento, divisão de tráfego (canary, blue/green)

### Componentes de um Service Mesh
- **Data Plane**: proxies sidecar (geralmente Envoy) que interceptam todo o tráfego de entrada/saída dos pods
- **Control Plane**: gerencia configuração e políticas para os proxies (ex: Istiod, Linkerd control plane)

### Service Meshes Populares
- **Istio**: funcionalidade rica, maior curva de aprendizado
  - Componentes: Pilot (control plane), Envoy (data plane), Galley (configuração), Citadel (segurança)
  - Funcionalidades: tráfego avançado, segurança mTLS, políticas, telemetria
- **Linkerd**: focado em simplicidade e performance
  - Arquitetura mais simples, proxy baseado em Rust
  - Recursos essenciais: mTLS, telemetria, perfis de serviço
- **Consul Connect**: integrado ao ecossistema Consul para service discovery
- **AWS App Mesh**: serviço gerenciado da AWS para service mesh
- **Kuma**: focado em multi-cloud e multi-cluster

### Funcionalidades do Service Mesh
- **Gerenciamento de Tráfego**:
  - Roteamento baseado em regras (path, header, weight)
  - Divisão de tráfego (canary testing, blue/green deployments)
  - Timeouts, retries, circuit breaking
  - Espelhamento de tráfego para debug
- **Segurança**:
  - mTLS automático entre serviços
  - Autorização baseada em identidade (RBAC para serviços)
  - Gerenciamento de credenciais e rotação automática
- **Observabilidade**:
  - Métricas automáticas (taxa de requisições, latência, taxas de erro)
  - Geração automática de traces distribuídos
  - Agregação de logs com contexto de trace
- **Resiliência**:
  - Detecção de falhas e saída rápida do circuito
  - Balanceamento de carga com descoberta de serviço
  - Saída de conexão e pooling de conexões

## Helm: Gerenciador de Pacotes para Kubernetes

### O que é o Helm?
Gerenciador de pacotes que facilita instalação, configuração e gerenciamento de aplicações Kubernetes
- Charts: pacotes de recursos Kubernetes pré-configurados
- Repositórios: coleções de charts (ex: helm stable, bitnami, repositórios privados)
- Releases: instâncias de charts executando em um cluster

### Estrutura de um Chart
```
mychart/
  Chart.yaml          # metadados do chart (nome, versão, descrição)
  values.yaml         # valores padrão configuráveis
  charts/             # charts dependentes (opcional)
  templates/          # templates Kubernetes gerados a partir de valores
    deployment.yaml
    service.yaml
    ingress.yaml
    _helpers.tpl      # templates reutilizáveis e funções
  README.md           # documentação do chart
  LICENSE             # licença do chart
  requirements.yaml   # dependências (depreciado em favor de charts/)
```

### Fluxo de Trabalho com Helm
1. `helm create mychart`: cria estrutura básica de um chart
2. Editar Chart.yaml e values.yaml com configurações desejadas
3. `helm install myrelease ./mychart`: instala o chart como uma release
4. `helm upgrade myrelease ./mychart`: atualiza a release com novas configurações
5. `helm uninstall myrelease`: remove a release do cluster
6. `helm repo add bitnami https://charts.bitnami.com/bitnami`: adiciona repositório
7. `helm search repo bitnami`: procura charts no repositório
8. `helm install myrelease bitnami/mysql`: instala chart de repositório

### Benefícios do Helm
- Padroniza instalação de aplicações complexas
- Permite versionamento e rollback de releases
- Facilita compartilhamento e reutilização de configurações
- Suporte a templates poderosos (sprig library) para configuração condicional
- Gerencia dependências entre charts
- Integração com CI/CD para deploy automatizado

## Boas Práticas e Padrões

### Organização de Recursos
- Use namespaces para separar ambientes (dev, staging, prod) ou equipes
- Aplique labels consistentes para identificação (app, versão, ambiente, equipe)
- Organize relacionados em mesmos namespaces quando fazem sentido lógico
- Evite nomes de recursos muito genéricos que causem colisões

### Gestão de Configuração
- Separe configuração de imagem (Dockerfile) de configuração de runtime (ConfigMaps/Secrets)
- Use ConfigMaps para dados não-sensíveis que mudam frequentemente
- Use Secrets para dados sensíveis, mesmo que pareçam não críticos inicialmente
- Externalize configurável para facilitar diferentes ambientes
- Documente variáveis de ambiente esperadas pelos containers

### Estratégias de Implantação
- Planeje estratégias de atualização baseado na natureza da aplicação
- Use readiness e liveness probes adequadamente para saúde do pod
- Defina requests e limites de recursos para melhor agendamento e QoS
- Considere orçamento de disrupção (PDB) para cargas de trabalho críticas
- Use annotations para informações de deploy (versão, timestamp, usuário)

### Monitoramento e Alertas
- Instrumented aplicações com métricas (Prometheus client libraries)
- Defina SLOs e SLIs para métricas de negócio e infraestrutura
- Configure alertas para sintomas, não apenas causas (evite alert fatigue)
- Use dashboards operacionais para visibilidade em tempo real
- Implemente retenção e arquivamento de logs baseado em requisitos de compliance

### Segurança
- Execute containers como usuário não-root sempre que possível
- Aplique princípio do menor privilégio em todos os níveis (RBAC, PSP, runtime)
- Scan de imagens para vulnerabilidades como parte do CI/CD
- Use image pull secrets para registros privados
- Limite acesso ao API server e etcd apenas para componentes necessários
- Mantenha componentes do Kubernetes atualizados com patches de segurança

### Resiliência e Escalabilidade
- Designe aplicações para serem stateless quando possível
- Use padrões de circuito aberto e retry com exponential backoff
- Implemente health checks profundos que verifiquem dependências críticas
- Planeje para falha de zona ou região (multi-zone, multi-region quando aplicável)
- Teste comportamento sob carga e condições de falha regularmente

## Tendências e Futuro do Kubernetes

### 1. Kubernetes Simplificado
- Distribuições focadas em desenvolvedor (Kind, K3s, Minikube) para aprendizado e teste
- Serviços gerenciados de nuvem (EKS, AKS, GKE) reduzindo complexidade operacional
- Distribuições leves para edge computing (K3s, KubeEdge)
- Abstrações como Knative para serverless sobre Kubernetes

### 2. Segurança Aprimorada
- Pod Security Standards substituindo PodSecurityPolicies depreciados
- Sigstore e cosign para providência e assinatura de imagens
- OPA/Gatekeeper e Kyverno para policy as code avançado
- Confidential Computing para proteção de dados em uso (SGX, SEV)

### 3. Service Mesh Evoluído
- Integração nativa de service mesh com arquitetura de lado do servidor
- Melhoria de performance através de eBPF e aceleradores de hardware
- Expansão além do tráfego L7 para L4 e capacidades de gateway unificado
- Integração com identidade.zero trust e políticas de segurança avançadas

### 4. Computação de Borda e Híbrida
- KubeEdge, K3s para cenários de edge computing com conectividade intermitente
- Cluster API para provisão gerenciada de clusters Kubernetes em qualquer infraestrutura
- Federated Kubernetes para gerenciamento de múltiplos clusters como um único entidade
- Arquiteturas híbridas que estendem clusters on-premises para nuvem pública

### 5. Inteligência Artificial e Machine Learning
- Kubeflow para orquestração de cargas de trabalho de ML em Kubernetes
- Operadores especializados para frameworks de ML (TensorFlow, PyTorch, XGBoost)
- Inferência otimizada com GPUs e hardware especializado (Triton Inference Server)
- MLOps integrado para ciclo de vida completo de modelos de ML

### 6. Sustentabilidade e Eficiência
- Escalonamento baseado em métricas de carbono e energia
- Otimização de agendamento para reduzir pegada ambiental
- Ferramentas para medição e reporte de impacto ambiental
- Arquiteturas que aproveitam energia renovável disponível geograficamente

## Checklist de Implementação

- [ ] Definir estratégia de provisionamento do cluster (kops, kubeadm, serviços gerenciados)
- [ ] Configurar alta disponibilidade para componentes do control plane (etcd cluster, múltiplos masters)
- [ ] Planejar arquitetura de rede (CNI plugin, serviços de load balancing, network policies)
- [ ] Implementar estratégias de armazenamento persistente (CSI drivers, classes de armazenamento)
- [ ] Configurar monitoramento e logging (Prometheus, Grafana, ELK/EFK, Loki)
- [ ] Estabelecer políticas de segurança (RBAC, Pod Security Standards, imagem scanning)
- [ ] Definir estratégias de CI/CD para build, teste e deploy de aplicações Kubernetes
- [ ] Planejar escalonamento automático (HPA, VPA, Cluster Autoscaler)
- [ ] Implementar service mesh quando necessário para observabilidade, segurança e gerenciamento de tráfego
- [ ] Definir estratégias de backup e disaster recovery (etcd backups, volume snapshots)
- [ ] Configurar políticas de gerenciamento de recursos (limits, requests, limit ranges)
- [ ] Planejar atualizações e upgrades do cluster com estratégias de versionamento
- [ ] Documentar procedimentos de operação, troubleshooting e runbooks para equipe

## Resumo

Kubernetes emergiu como a plataforma padrão para orquestração de containers, fornecendo um ecossistema robusto para deploy, escalonamento e gerenciamento de aplicações containerizadas em escala. Sua arquitetura modular separa o control plane (responsável pelo estado global e decisões de orquestração) dos worker nodes (onde as cargas de trabalho são executadas). Os objetos Kubernetes essenciais - Pods, Deployments, Services, ConfigMaps e Secrets - fornecem os blocos de construção para aplicações modernas. Estratégias de deploy como rolling updates, blue/green e canary permitem lançamentos seguros e controlados. Mecanismos de escalonamento (HPA, VPA, Cluster Autoscaler) ajustam recursos dinamicamente baseado em demanda. Segurança é abordada em múltiplas camadas: RBAC para autorização, políticas de pod para restrição de privilégios, segurança de rede e gestão segura de imagens. Observabilidade é alcançada através de métricas (Prometheus), logging (ELK/EFK/Loki) e tracing distribuído (Jaeger/Zipkin). Service meshes (Istio, Linkerd) avançam estas capacidades fornecendo gerenciamento de tráfego, segurança mTLS e telemetria automática. Helm simplifica o gerenciamento de aplicações complexas através de charts e repositórios. As tendências apontam para Kubernetes mais simples, segurança aprimorada, service mesh evoluído, computação de borda, integração com ML e foco em sustentabilidade. Um checklist estruturado ajuda a garantir que todos os aspectos críticos sejam abordados na implementação de soluções Kubernetes, desde desenvolvimento até produção em ambientes empresariais.

