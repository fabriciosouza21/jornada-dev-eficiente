# Performance e Benchmark

## Aplicações Web - Métricas Principais

- **Performance** (latency)
- **Scale** (throughput)

## Benchmark - 3 Dimensões

### 1. Limites

#### Ponto de Saturação
- CPU geralmente é um gargalo comum
- Uso de CPU entre 60% e 80%

#### Otimização para Latência
- Baixa latência: uso de CPU deve estar abaixo de 50%
- Manter filas vazias (tempo de espera baixo)
- Bloquear ou descartar novas tarefas
- Limitar tamanho da fila para reduzir tempo de espera
- **Overprovisioning:** threads, processos, máquinas

#### Otimização para Throughput
- Utilizar o máximo possível de CPU (60% a 80%)
- Usar filas grandes para ter buffer de execução
- **Tradeoff:** quanto maior a fila, maior o tempo de espera
- Ter mais tarefas na fila do que capacidade de concorrência

### 2. Distribuição

Entender como funciona o workload da aplicação.

**Ter um milhão de requisições por dia não diz muito sobre a distribuição.**

#### Métricas de Distribuição:

-----read----|---write-------
----cpu----|---I/O-----------
----realtime----|---batch----
---------dia|--noite---------

**Discussão:** Distribuição por média não é uma boa métrica.
**Solução:** Usar histogramas para visualização.

### 3. Qualidade

#### Medição de Latência
- **Não usar:** média, mediana
- **Usar:** percentil (ex: 95% das requisições têm latência de 100ms)
- Métrica muito comum na AWS

#### Taxa de Erro
Monitorar e medir erros do sistema.

## Ferramentas para Encontrar Limites

- Apache JMeter
- Gatling
- K6

## Referência
**Livro:** Java Concurrency in Practice - Brian Goetz
