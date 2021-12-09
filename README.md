# Github-AWS-CodeDeploy-CodePipeline-CI-CD 
Realizando o deploy de um site html dentro de uma instância EC2 na AWS. 

### Etapa 1: Criar um repositório no Github 
O conteúdo do repositório e estrutura deve ser da forma de como esta monstrando abaixo; Com os "arquivo appspec.yml" para ler os "scripts shell" e instalar as dependencias dentro do caminho criado /var/www/html/ "Nome da pasta do repositório contendo os arquivos do site" dentro da instância EC2 (index.html "site" e as dependencias dos arquivos do site na raiz License.txt)
       
       
       
       
       │-- appspec.yml
       │-- index.html
       │-- LICENSE.txt
       └-- scripts
           │-- install_dependencies
           │-- start_server
           └-- stop_server
           

![image](https://user-images.githubusercontent.com/80183407/145441444-649cdb4a-4566-416d-b0fe-d9b622d1962a.png)
    

### Etapa 2: Criação de uma função de instância no IAM Roles (Funções) "Amazonec2RoleforAWScodeDeploy" e uma Roles (Funções) aplicativo CodeDeploy. AWS service (Serviço da AWS) "CodeDeploy"

### Etapa 3: Criação da instância EC2 Amazon Linux 2 CentOS e anexar a função criada anteriormente e adicinoar o script shell no "user data" para instalar o agente CodeDeploy na inicialização da instância EC2 de forma automatizada.

#!/bin/bash
yum -y update
yum install -y ruby
yum install -y aws-cli
cd /home/ec2-user
wget https://aws-codedeploy-us-east-2.s3.us-east-2.amazonaws.com/latest/install
chmod +x ./install
./install auto

### Etapa 4: Criar um aplicativo no CodeDeploy e grupo de implantação no CodeDeploy

Selecione Criar aplicativo.

DentroApplication name, insira nome do aplicativo.

Em Compute platform (Plataforma de computação), selecione EC2/On-Premises (EC2/no local).

Selecione Criar aplicativo.

### criar um grupo de implantação no CodeDeploy

Na página que mostra o aplicativo, selecione Create deployment group (Criar grupo de implantação).

Em Deployment group name (Nome do grupo de implantação), insira nome do Group.

Em Service Role (Função de serviço), escolha a função de serviço criada anteriormente (por exemplo, CodeDeployRole).

Em Deployment type (Tipo de implantação), selecione In-place (No local).

Em Environment configuration (Configuração do ambiente), selecione Amazon EC2 Instances (Instâncias do Amazon EC2). No Key (Chave), insira Name. No Valor Insira o nome usado para marcar a instância (por exemplo nome da tag da instância EC2).

Em Deployment configuration (Configuração de implantação), selecione CodeDeployDefault.OneAtaTime.

Selecione Create deployment group (Criar grupo de implantação).

### Etapa 5: Criar o Pipeline no CodePipeline

Selecione Create pipeline (Criar pipeline).

DentroEtapa 1: Escolha as configurações do pipeline, em Pipeline name (Nome do pipeline).

Dentro Função de serviço, escolha Nova função de serviço Para permitir que o CodePipeline crie uma função de serviço no IAM.

Deixe as configurações em Advanced settings (Configurações avançadas) como padrão e escolha Next (Próximo).

DentroEtapa 2: Adicionar estágio de origem, em Provedor de origem, escolha "Github version 2". Dentro Nome do repositório Escolha o nome do repositório do github criado em Etapa 1: Em Branch name (Nome da ramificação), escolha main e, depois, selecione Next step (Próxima etapa).

![image](https://user-images.githubusercontent.com/80183407/145439144-c4f6cb49-fca0-48db-9130-ea11442d2aaf.png)

### CodePipeline

![image](https://user-images.githubusercontent.com/80183407/145436534-54093172-2e0a-441d-ae6e-f6ca13108fef.png)

### Pipeline foi executado com êxito

![image](https://user-images.githubusercontent.com/80183407/145439718-d284a336-90f7-4e30-b0dc-297dfe00cd10.png)

Fonte de referencia: https://docs.aws.amazon.com/pt_br/codepipeline/latest/userguide/tutorials-simple-codecommit.html

