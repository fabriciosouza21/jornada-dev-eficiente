
# Técnicas

## Melhore seu serviço principal

Você consegue melhorar seu serviço principal, talvez alterando para um plano mais robusto?

## Load Balancing

- **Round Robin**: é um algoritmo de balanceamento de carga que distribui requisições sequencialmente entre servidores disponíveis, como uma roda que gira: servidor 1, servidor 2, servidor 3, volta para o servidor 1, e assim por diante.
- **API Gateway**: é um ponto único de entrada que fica na frente de todos os seus microserviços, funcionando como um “porteiro inteligente” que recebe todas as requisições externas, aplica regras (autenticação, rate limiting, roteamento) e encaminha para o serviço correto.

## Chave de Idempotência

É um atributo enviado pelo cliente que garante que a operação será executada apenas uma vez, pois só deve haver um “hash” para a operação. Isso é útil para tornar operações como POST e PATCH idempotentes, evitando duplicações acidentais.

## Fluxos Mais Complexos de Idempotência

Em um motor de workflow, idempotência é a propriedade que garante que a mesma etapa executada várias vezes — algo comum quando há retries automáticos, replays de eventos ou mensagens duplicadas — produza exatamente um único efeito observável. Para obtê-la, cada tarefa de serviço deve aceitar um identificador de correlação (request ID/business key), verificar se já processou aquela solicitação e, em caso afirmativo, retornar o mesmo resultado sem repetir efeitos colaterais; na prática, isso envolve operações upsert/PUT, checagem de estado antes de avançar ou modelagem de passos de compensação. Dessa forma, o workflow engine pode reexecutar ações com segurança, mantendo consistência e evitando problemas como faturamento ou notificações duplicadas em sistemas distribuídos.

## Princípio Constant Work Pattern

O cache é uma técnica que aumenta a complexidade do fluxo. O banco de dados funciona como um fallback que quebra o princípio de *constant work pattern*.

Utilizar cache somente se for um impacto absurdo: no exemplo dado pelo Alberto, se a requisição sem cache demora 500 ms e com cache vai para 250 ms, talvez não valha a pena. O ideal é melhorar a consulta SQL, criar índices, etc.

Criar *cache-first* deve ser uma decisão bem pensada, refletir se realmente precisa de cache. Adicionar cache adiciona um fluxo a mais, precisa ser pensado em criação, deleção, atualização.

## Retry

Use Resilient4J para retentivas.

## Exponential Backoff (não faça sofrer quem já está sofrendo)

Backoff exponencial é uma técnica de controle de tráfego que aumenta gradualmente o tempo entre tentativas de conexão ou requisições, começando com um intervalo curto e dobrando esse tempo a cada falha, até atingir um limite máximo.

## Jitter (não faça sofrer quem já está sofrendo)

Jitter é uma técnica que adiciona uma variação aleatória ao tempo de espera entre tentativas de conexão ou requisições, evitando que múltiplos clientes façam novas tentativas exatamente no mesmo instante, o que poderia causar picos de carga e sobrecarga no sistema.

## Retry Throttle (AWS)

Uma espécie de "circuit breaker" que limita o número de tentativas de conexão a um serviço, evitando sobrecarga e permitindo que o sistema se recupere de falhas temporárias.

## Sistema Menor Não Pode Ser Sufocado Pelo Sistema Maior (não faça sofrer quem já está sofrendo)

O sistema menor pode enviar um header informando que a informação pode ser cacheada. Assim o cliente pode cachear a informação e não fazer requisições constantes para o sistema menor. O sistema menor pode utilizar um sistema mediador que escala mais facilmente, como uma fila SQL que foi feita para escalar.

## Princípio Let It Crash — Self Testing

Os métodos devem validar as entradas; um bom exemplo é executar o `Assert.state` do Spring para validar o valor de um enum:

```java
public void finalizarPedido(@NotNull Pedido pedido) {
    Assert.state(
        pedido.getStatus() == StatusPedido.AGUARDANDO_PAGAMENTO,
        "Pedido não está aguardando pagamento"
    );
    // lógica para finalizar o pedido
}
```

### Let It Crash — Self Testing #2

Utilizar o `@Validated` do Spring para validar os parâmetros de entrada.

## Transforme Fallback em Fluxos Padrão (Constant Work Pattern)

Evite utilizar fallback; transforme o fallback em um fluxo padrão. Caminhos alternativos executados poucas vezes, que somente ocorrem quando o caminho principal falha.

**Importante:** estude chamadas assíncronas, como o `CompletableFuture` do Java, que permite criar fluxos alternativos sem bloquear a execução principal.

## Observabilidade

Tenha no guideline os padrões para logs, criar uma abstração padrão para todos os logs e definir em quais situações deve ser utilizado cada nível de log.

## Classificação de Necessidade de Resiliência

Veja referência:
https://www.researchgate.net/publication/224436791_Towards_a_Conceptual_Framework_for_Resilience_Engineering
