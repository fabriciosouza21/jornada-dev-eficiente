# Otimização de Persistência

## Faça Seu Dever de Casa

### Tipos de Workload
- **CPU Bound**
- **IO Bound** (a maioria das aplicações)

> A maioria dos problemas de performance está no código da aplicação.

## IO Bound - Persistência

**Objetivo:** Reduzir o Transaction Response Time (TRT) e aumentar o throughput.

Boa parte dos gargalos está na camada de Persistência.

### Fórmula do TRT
```
T = Tacq + Treq + Texec + Tresp + Tidle
```

> Quanto menor o transaction response time, maior o throughput

## Otimizações por Componente

### 1. Tacq (Tempo de Aquisição de Conexão)

**Problema:** Criar uma conexão é caro.
**Solução:** Pool de conexões gerencia as conexões (qtdInicial, qtdMáxima, qtdMínima).

#### Quantas Conexões Manter?
- O número de conexões não precisa ser alto
- **HikariCP:** Pool de conexões utilizado pelo Java
- Muitas conexões fazem o banco fazer contention (conexões esperando umas pelas outras)
- **Regra:** Manter a contention na própria aplicação, não no banco de dados
- A fila deve estar na aplicação, não no banco de dados

**Referência:** Vlad Mihalcea - https://vladmihalcea.com

### 2. Treq (Tempo de Requisição)

#### Habilitar Batch Size

**Problema:**
```java
List<Produto> produtos = new ArrayList<>();

for (produto: produtos){
    entityManager.persist(produto);
}
```

Isso gera um round trip para cada produto (100 produtos = 100 round trips).

**SQL gerado:**
```sql
INSERT INTO produto (id, nome) VALUES (1, 'produto1');
INSERT INTO produto (id, nome) VALUES (2, 'produto2');
INSERT INTO produto (id, nome) VALUES (3, 'produto3');
```

**Solução:**
```properties
hibernate.jdbc.batch_size=50
```

Funciona para DELETE e UPDATE também.

### 3. Tres (Tempo de Resposta)

#### Cuidado com Select N+1

**Soluções:**

1. **@Eager** (NÃO recomendado)
   - Traz dados relacionados em uma única consulta
   - Problema: Pode trazer dados desnecessários
   - Pergunta: Sempre que uma entidade é carregada, todos os dados relacionados são necessários?

2. **Consulta Planejada**
   - Consulta customizada para o caso de uso específico

3. **DTO Projections**
   - Criar DTO (projeção) para trazer apenas dados necessários
   - Evita carregamento de atributos desnecessários

4. **Paginação**
   - Evita carregar grandes volumes de dados de uma vez
   - Especialmente em consultas que retornam listas extensas

### 4. Texec (Tempo de Execução)

#### Otimização de Consultas SQL

> Utilizar o EXPLAIN

**Problema:**
```sql
SELECT u.*
FROM usuario u
WHERE lower(u.email) = 'email@example.com'
```

> Cuidado com funções como lower, upper, trim, etc. Elas não utilizam os índices por padrão.

**Resultado:** seq scan - varre toda a tabela (não eficiente para tabelas grandes)

**Solução:**
```sql
SELECT u.*
FROM usuario u
WHERE u.email = 'email@example.com'
```

**Resultado:** index scan - utiliza o índice (mais eficiente)

#### Estratégias Avançadas

1. **SQL Nativo**
   - Para consultas complexas
   - Funções de agregação
   - Window functions

2. **Stored Procedures**
   - Leva o processamento para perto dos dados
   - Não há overhead de transporte de dados
   - Otimização máxima
