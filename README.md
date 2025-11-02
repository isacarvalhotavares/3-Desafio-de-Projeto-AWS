# 📌 Desafio de Projeto 3 - AWS  

Este repositório faz parte do **3º Desafio de Projeto** do **Santander Code Girls 2025**, realizado em parceria com a **DIO**.  

👩‍💻 Meu nome é **Isadora Carvalho Tavares** e aqui compartilho meus estudos e práticas sobre **implementar sua primeira Stack com AWS CloudFormation**.  

---

## 🌟 Sobre o Projeto

Este projeto tem como objetivo **consolidar os conhecimentos em AWS CloudFormation**, permitindo criar, gerenciar e atualizar recursos da AWS de forma programática através de templates.
O repositório contém **exemplos práticos, casos de uso, insights e boas práticas**, servindo como referência para estudo e implementação.

---

## 🧩 O que é AWS CloudFormation?

O **AWS CloudFormation** é um serviço que permite criar e gerenciar **recursos da AWS por meio de código**, seguindo o conceito de *Infraestrutura como Código (IaC)*.
Em vez de criar manualmente buckets, funções Lambda, VPCs ou instâncias EC2 pelo console, você descreve tudo em um **template** — um arquivo JSON ou YAML que define **o estado desejado da sua infraestrutura**.

Quando o CloudFormation executa o template, ele compara o estado atual da conta AWS com o estado descrito no arquivo e realiza automaticamente as ações necessárias para alinhar ambos.
Isso permite **padronizar ambientes, evitar erros humanos e versionar toda a infraestrutura**, assim como se faz com código de software.

**Benefícios principais:**

* **Reprodutibilidade:** a mesma stack pode ser criada em diferentes ambientes.
* **Automação:** reduz erros manuais ao criar recursos.
* **Controle de versão:** todo o template é versionável via Git.
* **Padronização:** garante que todos os ambientes sigam o mesmo padrão e configuração.

---

# 🚀 Como Implementar Sua Primeira Stack com AWS CloudFormation

### 1️⃣ Planejamento da Stack

Antes de criar o template:

* Defina os **recursos necessários** (ex.: S3, Lambda, EC2, RDS).
* Identifique **parâmetros variáveis** (ex.: nomes de buckets, AMI, tipo de instância).
* Planeje **permissões mínimas (IAM)** necessárias para cada recurso.
* Determine **outputs úteis** que serão retornados após a criação da stack.

---

### 2️⃣ Validando o Template

```bash
aws cloudformation validate-template --template-body file://templates/meu-template.json
```

---

### 3️⃣ Criando a Stack

```bash
aws cloudformation create-stack \
  --stack-name minha-primeira-stack \
  --template-body file://templates/meu-template.json \
  --parameters ParameterKey=KeyName,ParameterValue=minha-chave-ssh \
  --capabilities CAPABILITY_NAMED_IAM
```

---

### 4️⃣ Acompanhando a Criação

* Pelo **Console**: CloudFormation → Stacks → clique na stack → verifique **Events**.
* Pelo **CLI**:

```bash
aws cloudformation describe-stacks --stack-name minha-primeira-stack
```

---

### 5️⃣ Atualizando ou Excluindo a Stack

* Atualizar:

```bash
aws cloudformation update-stack \
  --stack-name minha-primeira-stack \
  --template-body file://templates/meu-template.json \
  --parameters ParameterKey=KeyName,ParameterValue=minha-chave-ssh \
  --capabilities CAPABILITY_NAMED_IAM
```

* Excluir:

```bash
aws cloudformation delete-stack --stack-name minha-primeira-stack
```

> **Insight:** Sempre valide templates antes de atualizar para evitar recursos órfãos e inconsistência.

---

## ⚙️ Casos de Uso Práticos com AWS CloudFormation

### **Caso 1 — "PhotoSnap" : Plataforma de Processamento de Imagens**

**Empresa fictícia:** PhotoSnap, startup que oferece edição e armazenamento de fotos online.

**Cenário:** Automatizar o upload e processamento de fotos enviadas pelos usuários, enviando notificações quando o processamento é concluído.

**Recursos utilizados na stack:**

* **S3 Bucket:** Armazena fotos enviadas pelos usuários.
* **Lambda Function:** Processa fotos (ex.: redimensionamento, geração de miniaturas, análise de metadados).
* **SNS Topic:** Notifica o time ou outros serviços sobre a conclusão do processamento.

**Explicação:**
O CloudFormation cria o bucket S3, a função Lambda e o SNS Topic de forma automatizada, conectando-os com triggers e permissões corretas.
Isso permite replicar a infraestrutura em outros ambientes (teste, homologação) sem necessidade de configuração manual.

**Template completo em:** `templates/case1-photosnap.json`

---

### **Caso 2 — "ShopNow" : Plataforma de E-commerce com EC2 e Banco de Dados**

**Empresa fictícia:** ShopNow, e-commerce fictício que vende produtos online.

**Cenário:** Hospedar uma aplicação web simples com backend em EC2 e banco de dados RDS, garantindo infraestrutura segura e replicável.

**Recursos utilizados na stack:**

* **EC2 Instance:** Servidor web que hospeda a aplicação e gerencia pedidos.
* **RDS MySQL:** Banco de dados gerenciado para armazenar informações de produtos e clientes.
* **Security Group:** Regras de firewall para acesso seguro à aplicação e ao banco.

**Explicação:**
O CloudFormation cria automaticamente a instância EC2, o banco RDS e o Security Group, configurando corretamente conexões e permissões.
Outputs fornecem IP público da aplicação e endpoint do banco, permitindo replicação consistente em múltiplos ambientes.

**Template completo em:** `templates/case2-shopnow.json`

---

## 🛠 Boas práticas de implementação

1. Templates pequenos e modularizados.
2. Defina **Parameters** e **Outputs** para reutilização.
3. Use **roles IAM com privilégio mínimo**.
4. Ative **rollback automático**.
5. Documente recursos, parâmetros e outputs.

---

## 🧾 Checklist rápido

* [ ] Template validado
* [ ] IAM Role configurada corretamente
* [ ] Segurança e logs configurados
* [ ] Teste com entradas de sucesso, erro transitório e erro permanente