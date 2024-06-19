#Devops-Ninja

Configuração do Ambiente com EC2 t3.large e Instalação do Docker
Este guia descreve os passos necessários para configurar um ambiente com quatro instâncias EC2 t3.large e instalar o Docker em cada uma delas.

Pré-requisitos
Uma conta AWS.
Acesso ao AWS Management Console.
Chave SSH para acesso às instâncias.
Passo 1: Lançar Instâncias EC2
Acesse o AWS Management Console e navegue para o serviço EC2.
Clique em "Launch Instance".
Selecione a Amazon Machine Image (AMI):
Escolha Amazon Linux 2 AMI (HVM), SSD Volume Type.
Escolha o tipo de instância:
Selecione t3.large.
Configure as instâncias:
Lançar quatro instâncias.
Adicione storage (opcional).
Configure o grupo de segurança:
Permitir tráfego SSH (porta 22) do seu IP.
Permitir tráfego HTTP (porta 80) e HTTPS (porta 443) se necessário.
Revisar e lançar as instâncias:
Selecionar ou criar um par de chaves para acesso SSH.
Lançar as instâncias.
Passo 2: Conectar às Instâncias via SSH
Use o comando SSH para conectar-se a cada instância. Substitua <your-key.pem> pelo caminho para sua chave PEM e <instance-ip> pelo endereço IP da instância.

sh
Copiar código
ssh -i <your-key.pem> ec2-user@<instance-ip>
Passo 3: Instalar Docker
Execute os seguintes comandos em cada instância para instalar o Docker.

Atualize os pacotes do sistema:
sh
Copiar código
sudo yum update -y
Instale o Docker:
sh
Copiar código
sudo amazon-linux-extras install docker -y
Inicie o serviço Docker:
sh
Copiar código
sudo service docker start
Adicione o usuário ec2-user ao grupo docker para executar comandos Docker sem sudo:
sh
Copiar código
sudo usermod -aG docker ec2-user
Verifique a instalação do Docker:
sh
Copiar código
docker --version
Passo 4: Configurar o Docker para Iniciar ao Boot
Para garantir que o Docker inicie automaticamente após reinicializações, habilite o serviço Docker:

sh
Copiar código
sudo systemctl enable docker
Passo 5: Repetir para Todas as Instâncias
Repita os passos 2 a 4 para cada uma das quatro instâncias EC2.

Arquivo README.md para o GitHub
Aqui está um exemplo de um arquivo README.md que você pode usar para descrever o processo no GitHub:

markdown
Copiar código
# Configuração de Ambiente com EC2 t3.large e Docker

Este repositório descreve os passos necessários para configurar um ambiente com quatro instâncias EC2 t3.large e instalar o Docker em cada uma delas.

## Pré-requisitos

1. Conta AWS.
2. Acesso ao AWS Management Console.
3. Chave SSH para acesso às instâncias.

## Passos

### 1. Lançar Instâncias EC2

1. Acesse o AWS Management Console e navegue para o serviço EC2.
2. Clique em "Launch Instance".
3. Selecione a Amazon Machine Image (AMI):
    - Escolha `Linux Ubuntu, SSD Volume Type`.
4. Escolha o tipo de instância:
    - Selecione `t3.large`.
5. Configure as instâncias:
    - Lançar quatro instâncias.
6. Adicione storage (opcional).
7. Configure o grupo de segurança:
    - Permitir tráfego SSH (porta 22) do seu IP.
    - Permitir tráfego HTTP (porta 80) e HTTPS (porta 443) se necessário.
8. Revisar e lançar as instâncias:
    - Selecionar ou criar um par de chaves para acesso SSH.
9. Lançar as instâncias.

### 2. Conectar às Instâncias via SSH

Use o comando SSH para conectar-se a cada instância. Substitua `<your-key.pem>` pelo caminho para sua chave PEM e `<instance-ip>` pelo endereço IP da instância.

```sh
ssh -i <your-key.pem> ec2-user@<instance-ip>
3. Instalar Docker
Execute os seguintes comandos em cada instância para instalar o Docker.

Atualize os pacotes do sistema:
sh
Copiar código
sudo yum update -y
Instale o Docker:
sh
Copiar código
sudo amazon-linux-extras install docker -y
Inicie o serviço Docker:
sh
Copiar código
sudo service docker start
Adicione o usuário ec2-user ao grupo docker para executar comandos Docker sem sudo:
sh
Copiar código
sudo usermod -aG docker ec2-user
Verifique a instalação do Docker:
sh
Copiar código
docker --version
4. Configurar o Docker para Iniciar ao Boot
Para garantir que o Docker inicie automaticamente após reinicializações, habilite o serviço Docker:

sh
Copiar código
sudo systemctl enable docker
5. Repetir para Todas as Instâncias
Repita os passos 2 a 4 para cada uma das quatro instâncias EC2.

