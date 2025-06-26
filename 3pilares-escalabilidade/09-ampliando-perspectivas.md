# Ampliando perspectivas

## Cache

### In-memory cache

Utilizar um cache em memória usando um Map com @Component do Spring garante que o cache seja singleton, ou seja, que exista apenas uma instância do cache em toda a aplicação. Funciona bem para aplicações pequenas, porém é um cache simples sem validações de tamanho ou expiração.

Usar o Google Guava Cache ou Caffeine é muito melhor, pois já possui validações de tamanho e expiração. Tem toda a parte de concorrência, é thread-safe, e é muito mais fácil de usar. Oferece muito mais opções para tornar o cache mais resiliente: políticas de invalidação, expiração e controle de tamanho.

Um cache hit de 80% é considerado bom, ou seja, 80% das vezes que você acessa o cache, consegue pegar o dado do cache e não precisa ir ao banco de dados.

### Cache distribuído

#### Materialized view

Cria uma tabela virtual que "encapsula queries". A view é computada e armazenada em disco, podendo ser atualizada periodicamente ou sob demanda.

Para atualizar a view:

```sql
REFRESH MATERIALIZED VIEW nome_da_view;
```

ou

```sql
REFRESH MATERIALIZED VIEW CONCURRENTLY nome_da_view;
```

### CDN

Serve como uma camada de cache mais próxima do usuário, reduzindo a latência e melhorando a performance. É especialmente útil para conteúdo estático, como imagens, vídeos e arquivos JavaScript/CSS.

## Processamento Assíncrono

### Scheduling

O Spring Boot oferece suporte a agendamento de tarefas assíncronas através da anotação `@Scheduled`. Isso permite que você execute métodos em intervalos regulares, sem bloquear o thread principal.

`TaskSchedule` é uma abstração que permite agendar tarefas de forma programática, oferecendo flexibilidade para definir quando e como as tarefas devem ser executadas.

### Distributed scheduling

<https://www.jobrunr.io/en/>

JobRunr open source é uma biblioteca que permite agendar e executar tarefas assíncronas em aplicações Java. Ele oferece uma interface simples para definir tarefas que podem ser executadas em segundo plano, sem bloquear o thread principal da aplicação.

### Kubernetes CronJob Implementation

Kubernetes oferece suporte nativo para agendamento de tarefas através de CronJobs. Um CronJob permite que você execute tarefas em intervalos regulares, semelhante ao cron do Unix.

### Serve Push (SSE)

**polling** é uma técnica onde o cliente faz requisições periódicas ao servidor para verificar se há novos dados. Isso pode ser ineficiente, pois gera tráfego desnecessário e aumenta a latência.

**websockets** permitem uma comunicação bidirecional entre cliente e servidor, onde o servidor pode enviar dados ao cliente assim que estiverem disponíveis, sem a necessidade de requisições periódicas.

**server-sent events (SSE)** é uma tecnologia que permite que o servidor envie atualizações em tempo real para o cliente através de uma conexão HTTP persistente. É mais leve que WebSockets e ideal para cenários onde o servidor precisa enviar atualizações frequentes, como notificações ou feeds de dados. Utilizar programação reativa.

## Balanceamento de Carga (Workload Distribution)

### Processamento paralelo

Distribuindo a carga de um processamento em múltiplas threads.

#### Por exemplo um CSV com muitas linhas

##### Processamento paralelo de um CSV com threads

1. Ponto a se pensar é em um pool de threads, ele resolve com o número de threads avaliado na máquina.
2. Particiona o CSV em 1000 páginas.
3. Criar um array de Future.
4. Ler as linhas do CSV adicionando ao array de Future, com o pool de threads criado no primeiro passo.
5. Espera as threads terminarem.

### Sharded Counters

**Lock contention** é um problema comum em sistemas distribuídos, onde múltiplas threads ou processos competem por um recurso compartilhado, como um contador.

Na prática, para resolver o problema de lock contention em contadores distribuídos, podemos utilizar a técnica de **sharded counters**. De modo prático, podemos fazer um sharded counter que é basicamente criar um identificador para cada counter, depois fazemos uma função para contar. Se quisermos realmente evitar lock contention, podemos criar um identificador único para cada vez que o contador for incrementado, assim cada thread vai incrementar o contador de forma independente.

``` sql
--Criar a tabela com sharded counters
CREATE TABLE sharded_like_post (
 id SERIAL PRIMARY KEY,
 post_id INT NOT NULL,
 shard_id INT NOT NULL,
 count BIGINT DEFAULT 0,
 UNIQUE (shard_id)
);

--Criar um index entre o shard_id e o post_id para evitar um tempo muito longo de lock contention
CREATE UNIQUE INDEX idx_shard_id ON sharded_like_post(shard_id, post_id);
```

O trade-off dessa solução é que precisamos utilizar uma função de agregação para somar os valores:

``` sql
SELECT SUM(count) AS total_likes
FROM sharded_like_post
WHERE post_id = 12345;
```

### Competing Consumers

Distribuindo a carga de uma Queue entre consumidores.

Utilizar um broker como RabbitMQ ou Kafka para distribuir mensagens entre múltiplos consumidores. Cada consumidor processa mensagens de forma independente, permitindo que a carga seja distribuída eficientemente. Para melhorar o processamento basta adicionar mais consumidores.

Para escalar o Kafka, aumentamos o número de partições, além de ser o mecanismo para evitar erros de concorrência.

---

Postgres tem uma opção chamada `FOR UPDATE` que permite criar um lock, mas isso causa lock contention. Podemos utilizar uma estratégia similar ao sharded counters, mas utilizando um identificador da máquina. `SKIP LOCKED` faz o banco de dados ignorar as linhas que estão bloqueadas por outras transações, permitindo que outras transações acessem essas linhas sem esperar.

``` sql
SELECT t.*
FROM task t
WHERE t.status = 'PENDING'
 AND mod(t.id, 10) = :machine_id
ORDER BY t.created_at ASC LIMIT 1
FOR UPDATE;
```

Uma solução mais simples é utilizar o `SKIP LOCKED`:

``` sql
SELECT t.*
FROM task t
WHERE t.status = 'PENDING'
ORDER BY t.created_at ASC LIMIT 1
FOR UPDATE SKIP LOCKED;
```
