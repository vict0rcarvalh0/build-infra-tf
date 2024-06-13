# Utilizando o Terraform para fazer o deploy de uma EC2
**Pré-requisitos**
- <a href="https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html">AWS CLI</a>: É a Command Line Interface da AWS, que permite o uso das credenciais de uma conta para conectar-se ao provedor da AWS e subir a infraestrutura desejada, no caso a EC2.
- <a href="https://developer.hashicorp.com/terraform/tutorials/aws-get-started/install-cli">Terraform</a> CLI: É a Command Line Interface do Terraform que permite o uso dos comandos para o deploy da infraestrutura.
- Conta AWS criada

## Configuração do arquivo de IaC
Inicialmente, foi criado um arquivo `main.tf` por meio do comando touch e o seguinte código foi inserido nele:
```
terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 4.16"
    }
  }

  required_version = ">= 1.2.0"
}

provider "aws" {
  region = "us-east-1"
}

resource "aws_instance" "app_server" {
  ami           = "ami-0c02fb55956c7d316"
  instance_type = "t2.micro"

  tags = {
    Name = "ExampleAppServerInstance"
  }
}
```

Esse código configura o provedor de nuvem da AWS e declara a criação de uma instância EC2, passando sua AMI, seu tipo e seu nome.
<p align="center">
<img src="/assets/1.png" width="80%">
</p>

## Atualização das credenciais AWS
Em seguida, foi necessário pegar as credenciais da AWS clicando no botão `AWS Details` e exibindo as credenciais, que foram devidamente atualizadas no arquivo `credentials` da pasta `.aws` utilizando nano.
<p align="center">
<img src="/assets/2.png" width="80%">
</p>

## Inicialização do diretório
Aqui o comando `terraform init` foi executado, que é responsável por:
1. Baixar Plugins: O Terraform baixa os plugins do provedor AWS que são necessários para interagir com a API da AWS.
2. Configurar Backend: Se configurado, o Terraform inicializa o backend, onde o estado da infraestrutura será armazenado.
3. Verificar Configurações: O Terraform verifica as versões dos provedores e do próprio Terraform para garantir compatibilidade.

<p align="center">
<img src="/assets/3.png" width="80%">
</p>

## Formatação e validação da configuração
Aqui os comandos `terraform fmt` e `terraform validate` foram executados. O primeiro formata o código Terraform de acordo com o estilo padrão do Terraform, melhorando a legibilidade e garantindo consistência no código. O segundo verifica a sintaxe e a semântica dos arquivos de configuração do Terraform sem mudar a infraestrutura.

<p align="center">
<img src="/assets/4.png" width="60%">
</p>

## Criação da infraestrutura
Aqui o comando `terraform apply` foi executado, que consiste em três etapas:
1. Plano de Execução: O Terraform cria um plano de execução, detalhando as ações que serão tomadas para alcançar o estado desejado, o plano é exibido na tela, mostrando os recursos que serão criados, modificados ou destruídos.
<p align="center">
<img src="/assets/5.png" width="80%">
</p>

2. Confirmação: O usuário é solicitado a confirmar a execução do plano digitando yes. Isso é uma medida de segurança para evitar mudanças não intencionais.

<p align="center">
<img src="/assets/6.png" width="60%">
</p>

3. Aplicação: Após a confirmação, o Terraform executa o plano, interagindo com a API da AWS para criar os recursos especificados.

<p align="center">
<img src="/assets/7.png" width="60%">
</p>

## Inspeção do estado
Por fim, o comando `terraform show` foi executado, com a finalidade de exibir o estado atual dos recursos gerenciados pelo Terraform, mostrando detalhes como IDs de recursos, atributos e outras informações importantes. Isso é útil para verificar se a infraestrutura foi criada conforme o esperado.

<p align="center">
<img src="/assets/8.png" width="60%">
</p>

## Resultados
No console da AWS, foi possível observar a criação bem sucedida da instância!

<p align="center">
<img src="/assets/9.png" width="100%">
</p>