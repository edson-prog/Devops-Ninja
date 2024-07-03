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
Escolha Ubuntu (HVM), SSD Volume Type.
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
sudo usermod -aG docker $USER
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



# Aula 5 - Construindo sua aplicação

### Fazer build das imagens, rodar docker-compose.

Nesse exercício iremos construir as imagens dos containers que iremos usar, colocar elas para rodar em conjunto com o docker-compose.

Sempre que aparecer <dockerhub-user>, você precisa substituir pelo seu usuário no DockerHub.

Entrar no host A, e instalar os pacotes abaixo, que incluem Git, Python, Pip e o Docker-compose.
```sh

$ sudo su
$ apt-get install git -y
$ curl -L "https://github.com/docker/compose/releases/download/1.25.5/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
$ chmod +x /usr/local/bin/docker-compose
$ ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose
```
Com os pacotes instalados, agora iremos baixar o código fonte e começaremos a fazer os build's e rodar os containers.
```sh
$ cd /home/ubuntu
$ git clone https://github.com/jonathanbaraldi/devops
$ cd devops/exercicios/app
```


#### Container=REDIS
Iremos fazer o build da imagem do Redis para a nossa aplicação.
```sh
$ cd redis
$ docker build -t <dockerhub-user>/redis:devops .
 docker run -d --name redis -p 6379:6379 <dockerhub-user>/redis:devops
$ docker ps
$ docker logs redis
```
Com isso temos o container do Redis rodando na porta 6379.
#### Container=NODE
Iremos fazer o build do container do NodeJs, que contém a nossa aplicação.
```sh
$ cd ../node
$ docker build -t <dockerhub-user>/node:devops .
```
Agora iremos rodar a imagem do node, fazendo a ligação dela com o container do Redis.
```sh
$ docker run -d --name node -p 8080:8080 --link redis <dockerhub-user>/node:devops
$ docker ps
$ docker logs node
```
Com isso temos nossa aplicação rodando, e conectada no Redis. A api para verificação pode ser acessada em /redis.



#### Container=NGINX
Iremos fazer o build do container do nginx, que será nosso balanceador de carga.
```sh
 cd ../nginx
$ docker build -t <dockerhub-user>/nginx:devops .
```
Criando o container do nginx a partir da imagem e fazendo a ligação com o container do Node
```sh
$ docker run -d --name nginx -p 80:80 --link node <dockerhub-user>/nginx:devops
$ docker ps
```
Podemos acessar então nossa aplicação nas portas 80 e 8080 no ip da nossa instância.

Iremos acessar a api em /redis para nos certificar que está tudo ok, e depois iremos limpar todos os containers e volumes.
```sh
$ docker rm -f $(docker ps -a -q)
$ docker volume rm $(docker volume ls)
#### DOCKER-COMPOSE
Nesse exercício que fizemos agora, colocamos os containers para rodar, e interligando eles, foi possível observar  como funciona nossa aplicação que tem um contador de acessos.
Para rodar nosso docker-compose, precisamos remover todos os containers que estão rodando e ir na raiz do diretório para rodar.

É preciso editar o arquivo docker-compose.yml, onde estão os nomes das imagens e colocar o seu nome de usuário.

- Linha 8 = <dockerhub-user>/nginx:devops
- Linha 18 = image: <dockerhub-user>/redis:devops
- Linha 37 = image: <dockerhub-user>/node:devops

Após alterar e colocar o nome correto das imagens, rodar o comando de up -d para subir a stack toda.

```sh
$ cd ..
$ vi docker-compose.yml
$ docker-compose -f docker-compose.yml up -d
$ curl <ip>:80
      ----------------------------------
        This page has been viewed 29 times
        ----------------------------------
```
Se acessarmos o IP:80, iremos acessar a nossa aplicação. Olhar os logs pelo docker logs, e fazer o carregamento do banco em /load

Para terminar nossa aplicação temos que rodar o comando do docker-compose abaixo:
```sh
docker compose down (para derrubar os containeres com as aplicações)

 Aula 6 - Rancher - Single Node

### Instalar Rancher - Single Node

Nesse exercício iremos instalar o Rancher 2.2.5 versão single node. Isso significa que o Rancher e todos seus componentes estão em um container.

Entrar no host A, que será usado para hospedar o Rancher Server. Iremos verficar se não tem nenhum container rodando ou parado, e depois iremos instalar o Rancher.
```sh
$ docker ps -a
$ docker run -d --name rancher --restart=unless-stopped -v /opt/rancher:/var/lib/rancher  -p 80:80 -p 443:443 rancher/rancher:v2.4.3
```
Com o Rancher já rodando, irei adicionar a entrada de cada DNS para o IP de cada máquina.

```sh
$ rancher.<dominio> = IP do host A
```
