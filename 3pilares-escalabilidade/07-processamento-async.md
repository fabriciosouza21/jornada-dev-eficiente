# Máximizando o throughput da aplicação com processamento assíncrono

Não processe **hoje** o que você pode processar **amanhã**: postergando tarefas custosas e lentas com processamento assíncrono.

## ThreadPoolExecutor do Spring Boot

O **ThreadPoolExecutor** é uma abstração do Spring Boot que permite executar tarefas de forma assíncrona, utilizando uma task queue e thread pool.

### Limitações do ThreadPoolExecutor

O pool de threads não é uma solução ideal para picos de requisições em longos intervalos de tempo, pois:

- A fila de tarefas pode crescer muito, podendo causar **OutOfMemoryError**
- Quando o servidor precisa ser reiniciado, todas as tarefas pendentes são perdidas, pois estão armazenadas na memória
- Não oferece durabilidade das tarefas

## Message Brokers: Solução Durável

Para cenários que exigem maior robustez, precisamos de uma fila maior e **durável**. Os **message brokers** (filas de mensageria) oferecem essa solução:

- Mesmo que a aplicação falhe, as mensagens permanecem na fila
- As mensagens podem ser processadas após a recuperação do sistema
- Oferecem persistência e durabilidade

## Traffic Shaping: O Poder das Filas

As filas têm o poder de **normalizar o throughput** da aplicação. Analisando graficamente, fica claro que o consumo é feito de forma constante, evitando problemas que poderiam ocorrer com picos de requisições.

### Inversão do Fluxo de Controle

O verdadeiro poder das filas está na **inversão do fluxo de controle**, conhecido como **"Traffic Shaping"**:

- **Sem filas**: A aplicação processa requisições conforme chegam (picos podem sobrecarregar)
- **Com filas**: A aplicação controla o ritmo de processamento, mantendo performance estável

### Quando Usar Cada Abordagem

**ThreadPoolExecutor ideal para:**
- Tarefas rápidas e de baixo volume
- Processamento em tempo real não crítico
- Aplicações com padrão de uso previsível

**Message Brokers ideais para:**
- Tarefas críticas que não podem ser perdidas
- Alto volume de requisições com picos
- Sistemas distribuídos
- Necessidade de auditoria e reprocessamento
