# Fundamentos da Escalabilidade

**Data: 24-06-25**

## Contexto da Arquitetura

> Arquitetura é sobre contextos, requisitos e restrições.

### Tipos de Contextos

- **Enterprise** (estabilidade, segurança)
- **Startup** (inovação, aceleração)
- **Big Tech** (escalabilidade)

## Problemas de Escalabilidade

### 1. Latência
O tempo de espera para uma resposta do sistema.

### 2. Throughput
A quantidade de requisições que o sistema consegue processar em um determinado período de tempo.

### 3. Coordination
A necessidade de coordenar diferentes partes do sistema para garantir que elas funcionem juntas corretamente.

## Os 3 Pilares da Escalabilidade

### 1. Cache
- Reduzir a latência e aumentar o throughput
- Armazenar dados frequentemente acessados em memória
- Cache distribuído utilizando Redis

### 2. Processamento Assíncrono
- Utilizar sistemas de mensageria
- Tecnologias: RabbitMQ, Kafka

### 3. Balanceamento de Carga
- Dividir o tráfego entre vários servidores
- Tecnologias: Load Balancers

## Considerações Importantes

Os "hypers" usam dicotomias e geralmente utilizam um vilão:
- SQL vs NoSQL
- Monolito vs Microserviços
- REST vs gRPC

A realidade é mais nuançada - cada tecnologia tem seu contexto adequado.
