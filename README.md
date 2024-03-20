# Documentação Fase 5 - Tech-Challenge


## Arquitetura 

![tech-challenge-oficial](https://github.com/FIAP-SOAT2/project-doc/assets/42720116/4503c2f9-97cb-42dd-a895-1931ec0e577d)

Para nossa arquitetura optamos por utilizarmos o Api gateway para o gerenciamento dos três microsserviços principais (user, product e order). Dessa forma, o api gateway é responsável por encaminhar a request para o microserviço correspondente, dependendo da sua do path da requisição.
No nosso contexto temos quatro Microsserviços sendo eles o microsserviço de (produto, user, order e o payment). Uma observação é que a comunicação com o microsserviços payment é feito utilizando o serviços de mensagerias (SQS e SNS) levando em consideração o padrão saga.

### Microsserviços User
#### Database
-   Banco postgres na instancia o RDS:
-   Nome: rds-user
-   Security group: db_user_sg

#### Repositório ECR
- name: ms-user

#### ECS Fargate
- Task definition: ecs-fargate-user-td
- Container: ecs-fargate-user-container
- Service ECS: ecs-fargate-user-service
- Security group do service: ms-user-service-sg
- Alarme: auto_scale_user
- Target Group: ecs-fargate-user-tg 
- Port: 4001

### Microsserviços Product

#### Database
- Banco postgres na instancia RDS
-   Nome: rds-product
-   Security group: db_product_sg

#### Repositório ECR
- nome: ms-product

#### ECS Fargate
- Task definition: ecs-fargate-product-td
- Container: ecs-fargate-product-container
- Service ECS: ecs-fargate-product-service
- Security group do service: ms-product-service-sg
- Alarme: auto_scale_product
- Target Group: ecs-fargate-product-tg 
- Port: 4002

### Microsserviços Order

#### Database
- Banco Atlas Mongo

#### Repositório ECR
- nome: ms-order

#### ECS Fargate
- Task definition: ecs-fargate-order-td
- Container: ecs-fargate-order-container
- Service ECS: ecs-fargate-order-service
- Security group do service: ms-order-service-sg
- Alarme: auto_scale_order
- Target Group: ecs-fargate-order-tg 
- Port: 4003

### Microsserviços Payment

#### Repositório ECR
- nome: ms-payment

#### ECS Fargate
- Task definition: ecs-fargate-payment-td
- Container: ecs-fargate-payment-container
- Service ECS: ecs-fargate-payment-service
- Security group do service: ms-payment-service-sg
- Alarme: auto_scale_payment
- Target Group: ecs-fargate-payment-tg 
- Port: 4004
