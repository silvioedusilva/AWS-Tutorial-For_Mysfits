# Objetivo
Criar um primeiro projeto com as práticas modernas em linguagem Python integrado
com a AWS utilizando 
`Fargate`, `Lambda`, `DynamoDB`, baseado no `Build a Modern Web Application` 
dispobilizado pela _AWS_

# Dicas iniciais




## 1 - Criar um Website estático

### 1.1. IDE Cloud9

**Nome:** AWSTutorialIDEforMysfits

**Descrição:** IDE para criação do projeto https://aws.amazon.com/pt/getting-
started/hands-on/build-modern-app-fargate-lambda-dynamodb-python/

_**Anotando para facilitar futuras atualizações**_ 

``` json
{
  "Replace": [
    {
      "Tag": "REPLACE_ME_YOUR_REGION",
      "Value": "us-east-1"
    }
  ]
}
```



### 1.2. Clonando git do projeto usando CLI

_**Utilizando o `CLI`**_
``` cli
ec2-user:~/environment $ git clone -b python https://github.com/aws-samples/aws-modern-application-workshop.git
Cloning into 'aws-modern-application-workshop'...
remote: Enumerating objects: 56, done.
remote: Counting objects: 100% (56/56), done.
remote: Compressing objects: 100% (53/53), done.
remote: Total 5885 (delta 13), reused 14 (delta 3), pack-reused 5829
Receiving objects: 100% (5885/5885), 18.33 MiB | 28.31 MiB/s, done.
Resolving deltas: 100% (2501/2501), done.
```



### 1.3. Criando o bucket S3

_**Criando o bucket name e anotando para futuras substituições**_

``` json
{
  "Replace": [
    {
      "Tag": "REPLACE_ME_YOUR_REGION",
      "Value": "us-east-1"
    },
    {
      "Tag": "REPLACE_ME_BUCKET_NAME",
      "Value": "silvio-aws-tutorial-bucket"
    }
  ]
}
```
*** Obs.: Sugiiro que continue alimentando este JSON por todo o projeto

_**Mudando de diretório**_
``` cli
ec2-user:~/environment $ cd aws-modern-application-workshop
ec2-user:~/environment/aws-modern-application-workshop (python) $ aws s3 mb 
s3://silvio-aws-tutorial-bucket
```

_**Criando o bucket**_
``` cli
ec2-user:~/environment/aws-modern-application-workshop (python) $ aws s3 mb s3://silvio-aws-tutorial-bucket
make_bucket: silvio-aws-tutorial-bucket
```

_**Visualizando o bucket via Console**_
<h1 align="center">
  <img src="https://ik.imagekit.io/rzv50a740s/Capture_S3_show_bucket_h-O_GWuDg.PNG">
</h1>

_**Configurando o bucket**_
``` cli
ec2-user:~/environment/aws-modern-application-workshop (python) $ aws s3 website s3://silvio-aws-tutorial-bucket --index-document index.html
ec2-user:~/environment/aws-modern-application-workshop (python) $ 
``` 



## 1.4. Alterando a Default S3 Bucket Policy

#### 1.4.1. Alterando o JSON com as configurações
**Caminho:** /aws-modern-application-workshop/module-1/aws-cli/website-bucket-policy.json

_**Antes:**_
``` json
{
  "Id": "MyPolicy",
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "PublicReadForGetBucketObjects",
      "Effect": "Allow",
      "Principal": "*",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::REPLACE_ME_BUCKET_NAME/*"
    }
  ]
}
```

_**Depois:**_
``` json
{
  "Id": "MyPolicy",
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "PublicReadForGetBucketObjects",
      "Effect": "Allow",
      "Principal": "*",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::silvio-aws-tutorial-bucket/*"
    }
  ]
}
```

#### 1.4.2. Preparando o comando para atualizar

_**Antes:**_
``` cli
aws s3api put-bucket-policy --bucket REPLACE_ME_BUCKET_NAME --policy file://~/environment/aws-modern-application-workshop/module-1/aws-cli/website-bucket-policy.json
``` 
_**Depois:**_
``` cli
aws s3api put-bucket-policy --bucket silvio-aws-tutorial-bucket --policy file://~/environment/aws-modern-application-workshop/module-1/aws-cli/website-bucket-policy.json
``` 
#### 1.4.3. Alterando a S3 Bucket Policy

``` console
ec2-user:~/environment/aws-modern-application-workshop (python) $ aws s3api put-bucket-policy --bucket silvio-aws-tutorial-bucket --policy file://~/environment/aws-modern-application-workshop/module-1/aws-cli/website-bucket-policy.json
ec2-user:~/environment/aws-modern-application-workshop (python) $ 
```

_**Visualizando o a Policy via Console**_
<h1 align="center">
    <img src="https://ik.imagekit.io/rzv50a740s/Capture_bucket_policy_NMoO076k2.PNG">
</h1>


### 1.5. Publicando o Website no S3

#### 1.5.1. Preparando o comando para copiar o index.html da IDE para o novo bucket do S3

_**Antes:**_
``` cli
aws s3 cp ~/environment/aws-modern-application-workshop/module-1/web/index.html s3://REPLACE_ME_BUCKET_NAME/index.html 
``` 
_**Depois:**_
``` cli
aws s3 cp ~/environment/aws-modern-application-workshop/module-1/web/index.html s3://silvio-aws-tutorial-bucket/index.html 
``` 
#### 1.5.2. Executando comando de cópia para o bucket

``` cli
ec2-user:~/environment/aws-modern-application-workshop (python) $ aws s3 cp ~/environment/aws-modern-application-workshop/module-1/web/index.html s3://silvio-aws-tutorial-bucket/index.html 
upload: module-1/web/index.html to s3://silvio-aws-tutorial-bucket/index.html
```

_**Visualizando o arquivo no S3 Bucket**_
<h1 align="center">
    <img src="https://ik.imagekit.io/rzv50a740s/Capture_bucket_with_index_88QMQlxrN.PNG">
</h1>



#### 1.6. Testando o website

#### 1.6.1. Preparando a URL para testar

_**Antes:**_
``` cli
http://REPLACE_ME_BUCKET_NAME.s3-website-REPLACE_ME_YOUR_REGION.amazonaws.com
``` 
_**Depois:**_
``` cli
http://silvio-aws-tutorial-bucket.s3-website-us-east-1.amazonaws.com
``` 
_**Visualizando o site**_
<h1 align="center">
    <img src="https://ik.imagekit.io/rzv50a740s/Capture_static_website_k_A6NHhNS.PNG">
</h1>

#### 1.6. Conclusão

<p>Primeira parte concluída, e nada melhor do que comemorar.</p>

<h1 align="center">
    <img src="https://media.giphy.com/media/9fum7ZNMeZIaI/giphy.gif">
</h1>
<p align="center">*Mano Elcio, essa é pra vc.</p>


## 2 - Hospedar a aplicação em um servidor web

### 2.1. Configurando a Infraestrutura

_**Comando solicitar configuração:**_
``` cli
ec2-user:~/environment $ aws cloudformation create-stack --stack-name MythicalMysfitsCoreStack --capabilities CAPABILITY_NAMED_IAM --template-body file://~/environment/aws-modern-application-workshop/module-2/cfn/core.yml
```

_**Verificando o status após alguns minutos:**_

``` cli
ec2-user:~/environment $ aws cloudformation describe-stacks --stack-name MythicalMysfitsCoreStack
```

_**Configurado:**_

``` json
{
    "Stacks": [
        {
            "StackId": "arn:aws:cloudformation:us-east-1:242461291558:stack/MythicalMysfitsCoreStack/faf5f4e0-8365-11ea-b413-12dc156c2e93",
            "StackName": "MythicalMysfitsCoreStack",
            "Description": "This stack deploys the core network infrastructure and IAM resources to be used for a service hosted in Amazon ECS using AWS Fargate.",
            "CreationTime": "2020-04-21T00:21:01.650Z",
            "RollbackConfiguration": {},
            "StackStatus": "CREATE_COMPLETE",
            "DisableRollback": false,
            "NotificationARNs": [],
            "Capabilities": [
                "CAPABILITY_NAMED_IAM"
            ],
            "Outputs": [
                {
                    "OutputKey": "CurrentAccount",
                    "OutputValue": "242461291558",
                    "Description": "REPLACE_ME_ACCOUNT_ID",
                    "ExportName": "MythicalMysfitsCoreStack:CurrentAccount"
                },
                {
                    "OutputKey": "FargateContainerSecurityGroup",
                    "OutputValue": "sg-03d2c6d595824a58c",
                    "Description": "REPLACE_ME_SECURITY_GROUP_ID",
                    "ExportName": "MythicalMysfitsCoreStack:FargateContainerSecurityGroup"
                },
                {
                    "OutputKey": "PublicSubnetOne",
                    "OutputValue": "subnet-0d12b32f93781321e",
                    "Description": "REPLACE_ME_PUBLIC_SUBNET_ONE",
                    "ExportName": "MythicalMysfitsCoreStack:PublicSubnetOne"
                },
                {
                    "OutputKey": "ECSTaskRole",
                    "OutputValue": "arn:aws:iam::242461291558:role/MythicalMysfitsCoreStack-ECSTaskRole-1937HR4HIUY16",
                    "Description": "REPLACE_ME_ECS_TASK_ROLE_ARN",
                    "ExportName": "MythicalMysfitsCoreStack:ECSTaskRole"
                },
                {
                    "OutputKey": "PrivateSubnetTwo",
                    "OutputValue": "subnet-0baa00a4b73b94e27",
                    "Description": "REPLACE_ME_PRIVATE_SUBNET_TWO",
                    "ExportName": "MythicalMysfitsCoreStack:PrivateSubnetTwo"
                },
                {
                    "OutputKey": "CurrentRegion",
                    "OutputValue": "us-east-1",
                    "Description": "REPLACE_ME_REGION",
                    "ExportName": "MythicalMysfitsCoreStack:CurrentRegion"
                },
                {
                    "OutputKey": "VPCId",
                    "OutputValue": "vpc-03f39ec79a54df156",
                    "Description": "REPLACE_ME_VPC_ID",
                    "ExportName": "MythicalMysfitsCoreStack:VPCId"
                },
                {
                    "OutputKey": "PublicSubnetTwo",
                    "OutputValue": "subnet-00a8a66daa2615691",
                    "Description": "REPLACE_ME_PUBLIC_SUBNET_TWO",
                    "ExportName": "MythicalMysfitsCoreStack:PublicSubnetTwo"
                },
                {
                    "OutputKey": "CodeBuildRole",
                    "OutputValue": "arn:aws:iam::242461291558:role/MythicalMysfitsServiceCodeBuildServiceRole",
                    "Description": "REPLACE_ME_CODEBUILD_ROLE_ARN",
                    "ExportName": "MythicalMysfitsCoreStack:MythicalMysfitsServiceCodeBuildServiceRole"
                },
                {
                    "OutputKey": "CodePipelineRole",
                    "OutputValue": "arn:aws:iam::242461291558:role/MythicalMysfitsServiceCodePipelineServiceRole",
                    "Description": "REPLACE_ME_CODEPIPELINE_ROLE_ARN",
                    "ExportName": "MythicalMysfitsCoreStack:MythicalMysfitsServiceCodePipelineServiceRole"
                },
                {
                    "OutputKey": "EcsServiceRole",
                    "OutputValue": "arn:aws:iam::242461291558:role/MythicalMysfitsCoreStack-EcsServiceRole-1OJGEP50OAXDF",
                    "Description": "REPLACE_ME_ECS_SERVICE_ROLE_ARN",
                    "ExportName": "MythicalMysfitsCoreStack:EcsServiceRole"
                },
                {
                    "OutputKey": "PrivateSubnetOne",
                    "OutputValue": "subnet-0855610207aac10a0",
                    "Description": "REPLACE_ME_PRIVATE_SUBNET_ONE",
                    "ExportName": "MythicalMysfitsCoreStack:PrivateSubnetOne"
                }
            ],
            "Tags": [],
            "EnableTerminationProtection": false,
            "DriftInformation": {
                "StackDriftStatus": "NOT_CHECKED"
            }
        }
    ]
}
```
Arquivo salvo em `/aws-modern-application-workshop/module-2/cfn/cloudformation-core-output.json` para 
futuras consultas.

### 2.2. Implantar um serviço com o Fargate

#### 2.2.1. Encontrando sua AWS Account ID

No menu disposto na superior do Cloud9, clique em 'AWS Cloud9' -> 'Go To Your Dashboard'.

No menu localizado do lado direito superior, ao lado do nome da sua conta expanda o menu -> 'Minha conta' 

No agrupamento 'Configurações da Conta', é o número ao lado do 'ID da Conta'

#### 2.2.1. Criando uma imagem do Docker localmente


docker build . -t REPLACE_ME_AWS_ACCOUNT_ID.dkr.ecr.REPLACE_ME_REGION.amazonaws.com/mythicalmysfits/service:latest
cd ~/environment/aws-modern-application-workshop/module-2/app

docker build . -t ************.dkr.ecr.us-east-1.amazonaws.com/mythicalmysfits/service:latest



<h2>Dicas de links</h2>

<h3>Documentações AWS em português</h3>
<ul>
  <li><a href="https://aws.amazon.com/pt/fargate/" rel="nofollow">Fargate</a></li>
  <li><a href="https://aws.amazon.com/pt/lambda/" rel="nofollow">Lambda</a></li>
  <li><a href="https://aws.amazon.com/pt/dynamodb/" rel="nofollow">DynamoDB</a></li>
  <li><a href="https://docs.aws.amazon.com/console/cloud9/" rel="nofollow">Cloud9</a></li>
  <li><a href="https://aws.amazon.com/pt/s3" rel="nofollow">S3</a></li>
  <li><a href="https://aws.amazon.com/pt/cli/" rel="nofollow">CLI</a></li>
</ul>

<h3>Links úteis</h3> 
<ul>
  <li><a href="https://www.docker.com/" rel="nofollow">Docker</a></li>
  <li><a href="https://imagekit.io/dashboard#media-library" rel="nofollow">Site para realizar upload de imagens</a></li>
  <li><a href="https://giphy.com" rel="nofollow">Site para buscar gifs</a></li>
  <li><a href="https://jsonformatter-online.com/" rel="nofollow">Validador JSON</a></li>
</ul>

<h3>Referências</h3>
<ul>
  <li><a href="https://github.com/marciafc/tutorial-markdown.git" rel="nofollow">Tutorial de Markdown - Marcia Castagna</a></li>
  <li><a href="https://aws.amazon.com/pt/getting-started/hands-on/build-modern-app-fargate-lambda-dynamodb-python/" rel="nofollow">Build a Modern Web Application - AWS</a></li>
</ul>

