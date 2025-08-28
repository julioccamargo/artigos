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

### Parte 3: Personalizando seu site - DNS

Objetivo: Substituir o acesso via endereço IP por um nome de domínio amigável (ex: www.meusite.com.br).
Serviço Principal: Amazon Route 53

Pré-requisito → Ter um nome de domínio já registrado (ou utilizar um grátis, dê um google aí meu velho!)

1. Criação da Zona Hospedada (Hosted Zone) → Inserir o nome do domínio no Route 53 para que a AWS possa gerenciá-lo.
2. Delegação de DNS → Copiar os 4 Name Servers (registros do tipo NS) fornecidos pela AWS na Zona Hospedada.
3. Atualização no Registrador de Domínio → No painel da empresa onde o domínio, substituir os Name Servers antigos pelos 4 da AWS.
4. Criação do Registro 'A' → Dentro da Zona Hospedada, criar um registro do tipo 'A' para apontar o nome do domínio para o endereço IPv4 público da instância EC2.
5 Propagação de DNS → Aguardar a atualização dos Name Servers se espalhar pela internet (pode levar de minutos a horas - 48h).

✅ Resultado: O site, antes acessível apenas pelo IP, passou a responder pelo nome de domínio configurado.

### Parte 4: Criar uma aplicação dinâmica - DB

### Parte 5 (opcional): Automatizar com Terraform - IaC
