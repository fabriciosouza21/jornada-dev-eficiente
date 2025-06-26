# Resumo Completo da Entrevista - Sistema de Delivery

## **Problema Proposto**
Sistema de delivery conectando clientes, restaurantes e entregadores com navegação por cardápios, pedidos e rastreamento em tempo real.

## **Perguntas Feitas e Respostas**

### **Q1: Quantos restaurantes existem?**
**R:** 50.000 restaurantes cadastrados, 30.000 ativos simultaneamente (60%)

### **Q2: Quantidade de pedidos por dia?**
**R:** 2 milhões pedidos/dia, picos 11h-14h e 18h-22h (70% do volume), 80 req/s no pico

### **Q3: Existe freeze para atualização do cardápio?**
**R:** Sim, freeze durante picos. Apenas "indisponível" permitido. Alterações só fora do horário de pico.

### **Q4: Distribuição de pedidos por restaurante?**
**R:** Regra 80/20 - 80% dos pedidos em 20% dos restaurantes. Top 10% recebe 60% dos pedidos.

## **Decisões Arquiteturais**

### **Escalabilidade**
- **Load Balancer** para distribuir carga entre instâncias
- **Separação de serviços**: Cardápio e Pedidos independentes
- **Escalonamento horizontal** para momentos de pico

### **Cache Strategy**
- **Redis** cachando apenas cardápios dos restaurantes populares
- **Aproveitando regra 80/20** para otimizar cache hit
- **Cache hit** evita consultas ao banco durante picos

### **Bancos de Dados**
- **NoSQL** (MongoDB) para cardápios - fácil escalonamento horizontal
- **SQL** (PostgreSQL) para pedidos - consultas estruturadas e consistência

### **Filas e Processamento Assíncrono**
- **Fila "Processamento Pedido"** - desacoplamento do fluxo principal
- **Fila "Pedido Confirmado"** - atualização de status
- **Kafka** garantindo ordem por chave (order_id)
- **Duas filas com responsabilidades distintas**

### **Resiliência e Tratamento de Erros**
- **Idempotência**: Chaves únicas geradas no backend (segurança)
- **Status "processando"** com timeout e retry para pedidos órfãos
- **Backoff exponencial + jitter** para evitar thundering herd
- **Dead letter queue** em região separada para falhas de fila principal
- **Health checks** com polling para detecção de recuperação
- **BD NoSQL como buffer** quando fila principal falha

### **Políticas de Negócio**
- **Freeze de cardápio** nos horários de pico (11h-14h, 18h-22h)
- **Status do pedido**: confirmado → preparando → pronto → entregando → entregue
- **Ordem garantida** por pedido via particionamento Kafka

## **Próximos Passos Identificados**
- Notificação em tempo real para restaurantes
- Sistema de matching geográfico de entregadores
- Tratamento de recusas de entregadores

## **Pontos Técnicos Destacados**
- Uso consciente de padrões de resiliência
- Separação clara de responsabilidades
- Consideração de trade-offs (NoSQL vs SQL)
- Pensamento em falhas e recuperação automática

![alt text](<Sistema de Delivery de Comida (Tipo iFood_Uber Eats).jpg>)
