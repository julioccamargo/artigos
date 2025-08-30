## Parte 1: Hospedagem de Site Estático com Amazon S3

Objetivo: Publicar um site HTML simples sem precisar de servidor.
Serviço Principal: Amazon S3 (Simple Storage Service)

1. Criação do Conteúdo → Escrever um arquivo index.html básico localmente.
2. Criação do Bucket → Criar um bucket no S3 com nome globalmente único.
3. Liberação de Acesso → Desabilitar a opção "Bloquear todo o acesso público".
4. Upload do Arquivo → Enviar o index.html para o bucket.
5. Habilitação do Site → Ativar Hospedagem de site estático, usando index.html como página inicial.
6. Política de Acesso → Configurar uma Bucket Policy (JSON) permitindo leitura pública (s3:GetObject). **Peça ajuda à uma IA**

✅ Resultado: O site ficou disponível em uma URL pública do S3.

### Parte 2: Servidor Web com Amazon EC2

Objetivo: Criar um servidor virtual, instalar software web e publicar uma página.
Serviço Principal: Amazon EC2 (Elastic Compute Cloud)

1. Lançamento da Instância → Criar uma instância EC2. **AMI: Amazon Linux 2**
2. Tipo: t3.micro (Free Tier). **Conferir antes de escolher, pode mudar**
3. Chave de Acesso → Gerenciar e baixar o par de chaves .pem para acesso via SSH.
4. Configuração de Rede (Security Group):
  Porta 22 (SSH): acesso remoto.
  Porta 80 (HTTP): acesso público ao site.
5. Conexão Remota → Acessar via terminal com SSH usando o .pem + IP público.
6. Instalação do Servidor Web (Apache):
	sudo yum install httpd -y
	sudo systemctl start httpd
7. Publicação do Conteúdo → Editei o arquivo /var/www/html/index.html no servidor.

✅ Resultado: O site ficou acessível pelo IPv4 público da instância EC2.

### Parte 3: Personalizando o site - DNS

Objetivo: Substituir o acesso via endereço IP por um nome de domínio amigável (ex: www.meusite.com.br).
Serviço Principal: Amazon Route 53

Pré-requisito → Ter um nome de domínio já registrado (ou utilizar um grátis, dê um google aí meu velho!)

1. Criação da Zona Hospedada (Hosted Zone) → Inserir o nome do domínio no Route 53 para que a AWS possa gerenciá-lo.
2. Delegação de DNS → Copiar os 4 Name Servers (registros do tipo NS) fornecidos pela AWS na Zona Hospedada.
3. Atualização no Registrador de Domínio → No painel da empresa onde o domínio, substituir os Name Servers antigos pelos 4 da AWS.
4. Criação do Registro 'A' → Dentro da Zona Hospedada, criar um registro do tipo 'A' para apontar o nome do domínio para o endereço IPv4 público da instância EC2.
5. Propagação de DNS → Aguardar a atualização dos Name Servers se espalhar pela internet (pode levar de minutos a horas - 48h).

✅ Resultado: O site, antes acessível apenas pelo IP, passou a responder pelo nome de domínio configurado.

## Parte 4: Dando Vida ao Site com WordPress e um Banco de Dados
O objetivo: Transformar nosso servidor simples em uma plataforma dinâmica, capaz de rodar um site completo como o WordPress, que precisa de um banco de dados para funcionar.
A Estratégia: Usamos uma arquitetura profissional, separando o "cérebro" (banco de dados) do "rosto" (o site).
Serviços Usados: Amazon EC2 (nosso servidor) e Amazon RDS (nosso banco de dados gerenciado).

1. Criamos o Banco de Dados (RDS): Lançamos um banco de dados MySQL usando o serviço RDS da AWS. O mais importante foi mantê-lo privado e seguro, sem acesso público, e usar a conexão automática que a AWS oferece para criar a "ponte" de comunicação com nosso servidor EC2.
2. Preparamos o Servidor (EC2): Com o banco de dados pronto, voltamos ao nosso servidor e fizemos três coisas:
	2.1 Instalamos o PHP, a linguagem que o WordPress usa.
	2.2 Baixamos os arquivos do WordPress e colocamos no diretório do site.
	2.3 Editamos o arquivo de configuração (wp-config.php), informando ao WordPress o "endereço" e as "credenciais" do nosso novo banco de dados.
3. Finalizamos a Instalação: Com tudo conectado, o último passo foi acessar nosso site no navegador. Lá, encontramos a famosa tela de instalação de 5 minutos do WordPress, onde criamos nosso usuário administrador e demos um título ao novo site.

✅ Resultado Final: Um site WordPress rodando na nuvem da AWS, com uma arquitetura segura onde o site (EC2) e o banco de dados (RDS) trabalham juntos, mas de forma separada e protegida.

## Parte 4: Aplicação Dinâmica com Banco de Dados (DB) --- O MEU DEU ERRADO SE QUISER VER O QUE HOUVE
Objetivo: Criar um banco de dados gerenciado e conectar a uma aplicação web (WordPress) para permitir conteúdo dinâmico.
Serviços Principais: Amazon RDS (Relational Database Service), Amazon EC2.

1. Criação do Banco de Dados → Lançar uma instância de banco de dados MySQL usando o serviço Amazon RDS, utilizando o template de Nível Gratuito.
2. Configuração das Credenciais → Definir um usuário (padrão: admin), uma senha forte e um "Nome do banco de dados inicial" (usei wordpress_db). **Atenção: Anotar a senha pra não vacilar.**
3. Configuração de Rede e Segurança → Criar um Security Group para o RDS, garantindo que a opção "Acesso Público" estivesse marcada como **Não**.
4. Criação da Regra de Conexão → Utilizada a opção de "Configuração automática de conexão com EC2" fornecida pela AWS durante a criação do RDS. Isso automaticamente criou a regra de entrada no Security Group do RDS (tipo MYSQL/Aurora, porta 3306) permitindo acesso a partir do Security Group da instância EC2.
5. Preparação do Servidor (Instalação de PHP e Cliente MySQL)
5.1. Instalar o PHP: Conectar no EC2 via SSH e instalar o PHP e os módulos necessários para o WordPress.
	sudo yum install -y php php-mysqlnd php-gd
5.2. Instalar o Cliente MySQL (Etapa de Debugging): Foi necessário um processo de investigação para instalar a ferramenta de linha de comando mysql.
5.3  Identificação do S.O.: O comando cat /etc/os-release revelou que o sistema era o Amazon Linux 2023.
5.4 Busca do Pacote: Comandos para versões antigas (yum, amazon-linux-extras) falharam. A solução foi pesquisar o pacote correto com dnf search mariadb.
5.5 Instalação Correta: O pacote foi identificado como mariadb1011-client-utils e instalado com o comando:
	sudo dnf install mariadb1011-client-utils -y
6. Download e Preparação do WordPress → Baixar, descompactar e mover os arquivos do WordPress para o diretório raiz do servidor web.
	cd /tmp
	wget https://wordpress.org/latest.tar.gz
	tar -xzf latest.tar.gz
	sudo cp -r /tmp/wordpress/* /var/www/html/
7. Configuração do wp-config.php → Criar o arquivo de configuração a partir do exemplo e editá-lo com as informações do RDS.
	cd /var/www/html/
	sudo cp wp-config-sample.php wp-config.php
	sudo nano wp-config.php 
	As 4 linhas críticas foram preenchidas: DB_NAME, DB_USER, DB_PASSWORD e DB_HOST (com o endpoint do RDS).
8. Ajuste de Permissões de Arquivos → Definir o usuário apache como dono dos arquivos para permitir que o WordPress gerencie temas e plugins.
	sudo chown -R apache:apache /var/www/html/
	sudo chmod -R 775 /var/www/html/
9. Diagnóstico e Correção Final (Etapa de Debugging) → Após a configuração, o erro "Error establishing a database connection" apareceu.
10. Teste de Conexão: A conexão direta com o banco via cliente MySQL (mysql -h [endpoint] -u admin -p) funcionou, provando que a Rede e as Credenciais estavam corretas.
**Causa**: O comando SHOW DATABASES; dentro do banco revelou que o banco wordpress_db não havia sido criado na inicialização do RDS.
*Solução**: O banco de dados foi criado manualmente com o comando SQL:
	SQL
	CREATE DATABASE wordpress_db;
11. Finalização da Instalação → Com o banco de dados criado, o acesso ao IP/domínio do site no navegador finalmente exibiu a tela de instalação de 5 minutos do WordPress, que foi preenchida para concluir o processo.

✅ Resultado: Uma aplicação WordPress totalmente funcional, rodando em uma arquitetura de nuvem segura e desacoplada (servidor web + banco de dados gerenciado), com todo o processo de diagnóstico e correção documentado.

### Parte 5 (opcional): Automatizar com Terraform - IaC
O objetivo: Sair do mundo dos cliques manuais e aprender a construir a infraestrutura na AWS de forma automatizada, usando Infraestrutura como Código (IaC).
Serviços Principais: Terraform e bloco de notas
1. Preparação do ambiente:
	1.1 Instalado Terraform CLI vua terminal (uso linux ZorinOS)
	1.2 Instalado AWS CLI
	1.3 Criado usuario IAM separado do root e as credenciais salvas
2. Codificando:
	2.1 Criar **main.itf**
	2.2 Registrar os comandos de instância EC2, Security Group e Par de chaves SSH
3. Executando: Use os comandos **init, plan (para revisar) e apply**
4. Verificar acesso: Acesse o servidor via SSH com a chave criada
5. Delete tudo para não gastar recurso: **terraform destroy**

✅ Resultado Final: Uma base de infraestrutura (servidor + firewall + chaves de acesso) criada e gerenciada de forma 100% automatizada
