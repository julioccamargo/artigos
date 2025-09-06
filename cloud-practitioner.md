# Anotações para estudos de certificação

## Modulo 1: Amazon EC2

### TIPOS
* Instância de uso geral: Equilibra recursos de computação, memória e rede, as vezes do mesmo nível sempre. Ex: Servidores de aplicação, jogos,back-end empresari, banco de dados pequenos e médios
* Instâncias otimizadas para computação: São parecidas com as de uso geral, mas com mais poder e **cargas de trabalho em lote**, com muitas transações em um grupo;
* Instâncias otimizadas para memória: Tem rápido processamento para cargas de trabalho que processam dados na memória. É uma instância com pré-carregamento grande, antes de executar a aplicação, como um loading, coisas que exigem tempo real e dados não estruturados prontos, banco de dados de alto desempenho;
* Instâncias de computação acelerada: Usam aceleradoras de hardware, coprocessadores para executar algumas funções mais eficiente. Cálculos com vírgula flutuante, processamento de gráficos ou correspondência de dados, jogos, streaming;
* Instâncias otimizadas para armazenamento: Alto poder de acesso sequencial, leitura e gravação de big data no armazenamento local. Aplicativos distribuídos, warehouse, sistema de transação on-line de alta frequência (pix) são os OLTP. A métrica de desempenho é IOPS, de entrada e saída de dados, quanto mais alto mais rápido.

### COBRANÇA

* Sob demanda: Não devem ser interrompidas e é cobrado até que seja;
* Instâncias reservadas 1 ou 3 anos: Standard Reserved Instances: Quando você sabe o tamanho e qual região usar, OS, tenacy padrão ou dedicado. Instâncias reservadas conversíveis: Pode usar em diferentes zonas de disponibilidades, diferentes OS, mas com prazo determinado de uso, após isso entra no sob demanda;
* Saving Plans: Assumir o compromisso de uso por 1 ou 3 anos, 72% de economia. Diferença das instâncias resevadas é que não precisa especificar os tipos ou quantidades de instâncias que você quer.
* Spot: Ideais para cargas reduzidas e que toleram interrupções.
* Hosts dedicados: Servidores físicos da AWS EC2 separados e dedicados para seu uso.

### DIMENSIONAMENTO

* Amazon EC2 Auto Scaling: Pmite adicionar e remover instâncias automaticamente de acordo com a necessidade atual. Dinâmico: Responde as alterações ou Preditivo: Programa o aumento de acodrdo com uma previsão de necessidade.
* Tem quantidade mínima, desejada e extras para usar quando necessário, é possível cpnfigurar máxima para não perder controle dos custos.

### TRÁFEGO

* Elastic Load Balancing: Distribui automaticamente o tráfego

### ENFILEIRAMENTO

* Monolítico e enfileiramento: Você põe uma fila de comandos, ela é alimentada mesmo que o próximo serviço esteja indisponível.
* Amazon SNS: Usados para mensagens por assinatura. Um tópico, igual para todos ou Vários tópicos que é uma escolha do que vai receber.
* Amazon SQS: É o serviço de enfileiramento de mensagens.

### COMPUTAÇÃO SEM SERVIDOR

* AWS Lambda: Sem servidor, você paga pelo tempo que o código fica sendo executado. Scaling é feito pela AWS.
 * Contêiners: Amazon ECS é o sistema de gerenciamento que permite executar e dimencionar aplicaçẽso, é compatível com Docker.
 * Amazon EKS: Kubernetes permite implantar e gerenciar aplicações em grande escala.
 * AWS Fargate: É um genciamento de contâiners e kubernetes (ECS e EKS) autônomo

## Módulo 2: Confiabilidade e estrutura global

### Região

* Conformidade com governança e leis locais
* Proximidade com clientes para diminuir latência
* Serviços disponíveis podem não existir em determinados locais
* Preço, leve em consideração os impostos que irão diferenciar os custos de execução para a AWS

### Zonas de disponibilidade

* Uma Região consiste em 3 ou mais zonas de disponibilidade
* São os datacenters de uma região geográfica segura o suficiente para ter alta disponibilidade
* Locais de borda: Amazon CloudFront usa para armazenar coisas em cache para usuários distantes da hospedagem original, diminuindo a latência
* AWS Outposts: Estende a infraestrutura da AWS para diferentes locais, inclusive para data center on-premises

### Comoprovisionar recursos

* Console da AWS: Interface web intuitiva e completa, AWS Console Mobile Application é bom para monitoramento, alarmes e cobrança
* AWS Command Line Interface (AWS CLI): Permite controlar via linha de comando, automatizar ações, iniciar serviços, destuir, conectar, etc
* SDKs: São kits de desenvolvimento, facilitam o uso por API, C++, Java e .Net sãos as principais
* AWS Elastic Beanstalk: ferramenta que implanta os recursos para executar tarefas como ajuste de capacidade, balancear carga, auto scaling e monitorar o health da aplicação
* AWS CloudFormation: IaC, é a infraestutura como código da própria AWS (tipo o Terraform)

## Módulo 3: Redes

* Amazon VPC: É uma rede virtual privada para os seus recursos, podendo ter sub-redes
* Gateway de internet: É anexada a uma VPC para garantir tráfgo público
* Gateway privado virtual: É a VPN cm sua de privada ou seu data center
* AWS Direct Connect: É uma cpnexão dedicada de um data center e VPC, reduz custos e melhora a banda de tráfego

### Sub-redes
* Funcionamento parecido com VLAN
* Públicas: Contém recursos públicos, como um site de loja
* Privadas: Recursos que devem ser acessados por pessoas específicas, DB, arquivos
* ACL é um firewall virtual para controle dos pacotes das VPC, pode criar ACL personalizada para cada VPC. Como padrão é liberada entrada e bloqueado a saída
* Stateless: Filtragem de pacotes, é somente um verificador e não um registro, quem verifica permissões do pacote é o grupo de segurança. Como padrão é tudo liberado
* Grupo de segurança: É um firewall virtual que controla o tráfego de entrada e saída de uma insância do EC2, por padrão ele nega todo o tráfego de entrada e libera saída. Precisa adicionar no mesmo grupo tudo que você precisa que se comunique (tipo VLAN mesmo)
* Stateful: Grupo de segurança usa esse filtro de pacotes, eles lembram dos padrões que negam ou liberam

### Redes globais

* DNS: É uma lista de tudo, ela converte o IP por um nome de site por exemplo
* Amazon Route 53: É um serviço WEB de DNS, roteamento de usuários para aplicativos hospedados na AWS. Conecta o cliente aos balanceadores de carga e instâncias EC2. Ele trabalha com o CloudFront ára entregar conteúdo

## Módulo 4: Armazenamento

**Amazon Elastic Block Store (Amazon EBS)**: Fornece volumes de armazenamento como bloco que você pode usar com EC2, se ncerrar uma instância todos os volumes permanecerão disponíveis
Use **snapshots** para fazer backup, o primeiro dia é total e depois só o que firam alterados, incremental

**Amazon simple storage service (Amazon S3)**: Oferece armazeamento à nível de objeto. Ele é separado por frequência de uso dos dados:
* S3 Standart: Alta disponibilidade. Acessados com frequência, no mínimo de 3 Zonas de Disponibilidade
* S3 Standart - Infrequent Acess (IA): Alta disponibilidade, mas com baixa frequência de acesso
* S3 One Zone-Infrequent Acess: PAra economizar custos ele usa uma única zona de disponibilidade, mas você pode reproduzia em caso de falha
* S3 Intelligent-Tiering: Dados sem padrão de acesso, ele move a classe de acordo com esses padrões
* S3 Glacier Instant Retieval: Alta velocidade para recuperar itens arquivados *instant = instantâneo)
* S3 Glacier Flexible Retrieval: Itens arquivados que podem ser recuperados em minutos ou horas
* S3 Glacuer Deep Archive: O mais barato, demora 12 horas para recuperar
* S3 Outpost: Para arquivar coisas no on-premises, é uma redundância

**Amazon Elastic File System (Amazon EFS)**: É um serviço regional, várias zonas de disponibilidade podem acessar

**Amazon Relational Database Service (Amazon RDS)**: Banco de dados relacional, usa consulta estruturada (SQL), são separados em 6 serviços:
* Amazon Aurora - Alta disponibilidade
* PostreSQL
* MySQL
* MariaDB
* Oracle Database
* Microsoft SQL Server

**Amazon DynamoDB**: É um DB **NÃO RELACIONAL = NoSQL** e suas principais características são:
* Sem servidor
* Auto scaling

**Amazon Redshift**: É serviço de data warehouse, usado principalmente para Big Data

**AWS Database Migration Service (AWS DMS)**: Serve para migrar bancos de dados para Amazon e durante a migração o DB continua disponível

**Serviços adicionais**:
* DucumentDB - Compatível com MongoDB
* Neptune - Banco de dados grafo
* Quantun LEdger Database (QLDB) - Banco de dados ledger, com registro do histórico de alterações
* Managed Blockchain - Usado para gerenciar redes blockchain com frameworks de código aberto
* ElastiCache - Adiciona caadas cache sobre o DB, ajuda a melhorar o tempo de leitura
* DynamoDB Accelerator (DAX) - É um cache em memóra do DynamoDB

## Módulo 5: Segurança

**AWS Artifact**: É um serviço de auditoria
**AWS Shield**: Proteção contra DDoS, serviços Standart é grátise tem o Advanced para relatorios detalhados, detecção e mitigação de ataques
**AWS Key Management Service (AWS KMS)**: Permite usar chaves de criptografia
**AWS WAF**: É um firewall web para monitorar solicitações, trabalha junto com CloudFront e Application Load Balance
**Amazon Inspector**: Faz avaliações de segurança automatizadas, depois ele apresenta uma lista de descobertas por ível de gravidade
**Amazon GuardDuty**: Realiza detecção inteligtente de ameaças para sua infraestreutura e recursos AWS
