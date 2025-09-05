# Projeto 2: Aplicação Web com Docker e AWS ECS

Este projeto representa a evolução da hospedagem tradicional em servidores virtuais para uma arquitetura moderna baseada em contêineres, implantada na nuvem da AWS de forma "serverless".

**Você já pode acessar o resultado [aqui](http://15.229.254.255:8080/).

---
## 🎯 Objetivo

O desafio era pegar uma aplicação web simples desenvolvida em Python (Flask), empacotar em um Docker e publicar na AWS. O objetivo final era ter uma aplicação acessível globalmente, rodando de forma escalável e resiliente com o mínimo de sobrecarga operacional.
---

## 🛠️ Tecnologias Utilizadas

### Linguagem e Framework
* **Python**
* **Flask**

### Containerização
* **Docker:**

### Serviços AWS
* **Amazon ECR (Elastic Container Registry):** 
* **Amazon ECS (Elastic Container Service) com AWS Fargate:** 

---

## 📜 Resumo do Projeto

O fluxo foi dividido em duas fases:

1.  **Ambiente Local:**
    * Foi desenvolvida uma aplicação "Olá, Mundo" simples em Python/Flask.
    * Foi escrito um **`Dockerfile`**, para construir a imagem da aplicação, incluindo o sistema operacional base, a instalação de dependências e a cópia do código-fonte.
    * A imagem foi construída e testada localmente, validando que o contêiner funcionava perfeitamente.

2.  **Implantação na Nuvem (AWS):**
    * Foi criado um repositório privado no **Amazon ECR**.
    * A imagem Docker local foi "etiquetada" (tag) e enviada ("push") para o ECR.
    * Foi criado um **Cluster** no ECS para ser o ambiente lógico da nossa aplicação.
    * Foi criada uma **Definição de Tarefa (Task Definition)**, que aponta para imagem no ECR e define os recursos de CPU e memória.
    * Finalmente, foi criado um **Serviço** no ECS, que utilizou a Definição de Tarefa para a aplicação no **Fargate**, garantindo que ela estivesse sempre em execução e atribuindo a ela um IP público para acesso via web.

---

## ✅ Resultado Final

Uma aplicação web totalmente conteinerizada, rodando em uma arquitetura serverless na AWS. O projeto demonstra o fluxo completo de trabalho: desde a criação de uma imagem Docker local até a sua orquestração na nuvem, eliminando a necessidade de gerenciamento manual de servidores e adotando as melhores práticas de "Cloud Native".
