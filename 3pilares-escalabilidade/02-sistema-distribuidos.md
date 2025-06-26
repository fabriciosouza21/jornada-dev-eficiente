# Sistemas Distribuídos

## Referência Principal
**Livro:** Designing Data-Intensive Applications - Martin Kleppmann

## 8 Falácias de Sistemas Distribuídos

1. A rede é confiável
2. Latência é zero
3. A largura de banda é infinita
4. A rede é segura
5. A topologia da rede é estável
6. Existe somente um administrador
7. O custo de transporte de dados é zero
8. A rede é homogênea

## Local Calls vs Remote Calls

### Local Calls
- **Spring Boot**: Utiliza o conceito através do Tomcat embedded
- Servidor web roda dentro da mesma JVM da aplicação
- Elimina necessidade de comunicação via rede entre componentes

### Remote Calls
- Comunicação entre componentes em processos ou máquinas diferentes
- Necessitam protocolos de rede para troca de dados
- **Envolvem:**
  - Serialização de dados
  - Transmissão via rede (HTTP/REST, gRPC, messaging queues)
  - Deserialização no destino
  - Tratamento de falhas de rede
- **Introduzem:**
  - Latência
  - Possibilidade de timeouts
  - Necessidade de estratégias como circuit breakers e retry mechanisms

## Microserviços

> Um bom arquiteto escolhe tecnologias por suas desvantagens.

**Livro:** Solving Imaginary Scaling Issues

### Conceitos Importantes

> Se você não tem requisitos de desempenho ou throughput então você não tem problema de escalabilidade.

> Microserviços não existem para escalar aplicações, mas sim para escalar equipes.

> Eu não escrevi um livro sobre microservices, mas sim sobre como você arquiteta para entregas contínuas.

**Referência:** Building Scalable Web Sites
