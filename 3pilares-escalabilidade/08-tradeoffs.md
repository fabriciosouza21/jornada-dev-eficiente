# Trade-offs dos Pilares de Escalabilidade

## Caching

### Performance vs Consistência

Quanto mais distante da fonte da verdade, maior a performance, porém menor a consistência. A invalidação de cache pode ser um problema significativo. Quando utilizamos cache em nível de apresentação (Spring Cache ou EhCache), temos um cache mais fácil de manipular por estar próximo da fonte da verdade (Source of Truth).

### Cache em Memória vs Cache Distribuído

Migrar de cache em memória para cache distribuído (Redis) melhora o throughput mas aumenta a latência. O cache em memória oferece maior velocidade, porém não é escalável horizontalmente.

## Trade-offs de Processamento Assíncrono

### Mudança no Workflow do Cliente

O cliente precisa se adaptar ao novo fluxo de trabalho, aumentando a complexidade da solução. A aplicação cliente necessitará implementar polling ou WebSockets para receber o resultado das tarefas assíncronas.

### Fire and Forget Strategy

Para tarefas não críticas, podemos utilizar o TaskExecutor do Spring Boot sem confirmação de entrega, dispensando brokers como RabbitMQ. O trade-off é a perda de garantia de execução - tarefas podem ser perdidas em caso de falhas no envio ou indisponibilidade do broker. Esta estratégia é aceitável apenas para operações não críticas.

## Trade-offs de Load Balancer

### Latência vs Consistência

Reduzir latência frequentemente significa abrir mão de consistência forte. Estratégias de réplica de banco de dados, embora evitem sobrecarga no banco principal, podem gerar inconsistência eventual entre réplicas. Este comportamento é aceitável em muitos cenários, mas deve ser cuidadosamente avaliado.

### Coordenação e Race Conditions

Race conditions em load balancing ocorrem quando múltiplas requisições simultâneas competem por recursos compartilhados. Por exemplo, dois servidores backend atualizando um contador de conexões ativas simultaneamente podem ler o mesmo valor inicial e incrementar, resultando em contagem incorreta que compromete as decisões de roteamento.
