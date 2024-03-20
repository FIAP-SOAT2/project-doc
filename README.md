# 📚 Documentação Fase 5 - Tech-Challenge

## 🔗 Microsserviços

#### Cada um dos microsserviços possuem readme individual com orientação para execução:

[**Order**](https://github.com/FIAP-SOAT2/ms-order) : Criação e gerenciamento do pedido

[**Payment**](https://github.com/FIAP-SOAT2/ms-payment): Processamento de pagamento e integração com api externa Mercado Pago.

[**Product**](https://github.com/FIAP-SOAT2/ms-product): Cadastro de produto e gerenciamento do cardápio.

[**User**](https://link-da-documentação): Cadastro de usuário e gerenciamento de dados LGPD.


## SAGA PATTERN

Ao longo da construção da nossa arquitetura, estabelecemos como objetivo principalmente buscar a promoção da autonomia dos serviços, reduzir o acoplamento e por fim garantir a resiliência e disponibilidade durante a interações. Entendemos que o melhor caminho seria a utilização do padrão coreografado no contexto saga pois permitiria uma abordagem eficaz, especialmente quando diferente do padrão orquestrador, onde um serviço centralizado controla o fluxo de interações, no padrão coreografado, cada serviço conhece seu papel dentro de uma transação e comunica seu estado para os outros serviços por meio de eventos. Isso permitiu a construção de uma arquitetura mais descentralizada e resiliente.

Dentre os principais pontos que consideramos como referência esses foram os quais nortearam a construção do nosso projeto:

**Desacoplamento e autonomia**: Cada serviço, como product, user, e payment e order opera de forma autônoma, publicando e assinando eventos sem a necessidade de um coordenador central. Isso reduz as dependências diretas entre os serviços, facilitando a manutenção, atualizações, e a escalabilidade de cada um deles de maneira independente.

**Flexibilidade e adaptabilidade**: Mudanças em um serviço ou a introdução de novos serviços no ecossistema podem ser mais facilmente acomodadas, uma vez que a comunicação baseia-se em eventos. Novos microsserviços podem assinar eventos existentes sem afetar a lógica dos serviços produtores desses eventos.

**Resiliência e tolerância a falhas**: Em caso de falhas temporárias, os serviços podem continuar a operar de maneira isolada, processando eventos assim que possível.

**Eficiência na comunicação**: Utilizando serviços de mensageria como SQS e SNS da AWS para a comunicação baseada em eventos entre os serviços, o sistema pode se beneficiar da entrega garantida de mensagens e da capacidade de processamento assíncrono, melhorando o desempenho geral do sistema.

## Arquitetura 

![tech-challenge-oficial](https://github.com/FIAP-SOAT2/project-doc/assets/42720116/4503c2f9-97cb-42dd-a895-1931ec0e577d)

Para nossa arquitetura optamos por utilizarmos o Api gateway para o gerenciamento dos três microsserviços principais (user, product e order). Dessa forma, o api gateway é responsável por encaminhar a request para o microserviço correspondente, dependendo da sua do path da requisição.
No nosso contexto temos quatro Microsserviços sendo eles o microsserviço de (produto, user, order e o payment). Uma observação é que a comunicação com o microsserviços payment é feito utilizando o serviços de mensagerias (SQS e SNS) levando em consideração o padrão saga.

### Microsserviços User
#### Database
-   Banco postgres na instancia o RDS
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

### Network Load Balancer
- nome: ecs-fargate-order-poc-nlb
- target group: ecs-fargate-nlb-group

### VPC LINKS
- nome: ecs-fargarte-vpc-link

### Api Gateway 
- nome: ecs-fargate-api
