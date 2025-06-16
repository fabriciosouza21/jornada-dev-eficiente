
# Princípios de Resiliência

Resiliência é a capacidade de um sistema de software **continuar operando de maneira aceitável mesmo diante de falhas, degradações ou eventos inesperados**.

> “A capacidade de um sistema, antes, durante e depois de uma perturbação, de absorver o impacto, restaurar-se a um desempenho aceitável e sustentar esse nível por tempo adequado.”
> — [NIST SP 800-160 vol. 2 (draft)](https://csrc.nist.gov/CSRC/media/Publications/sp/800-160/vol-2/draft/documents/sp800-160-vol2-draft.pdf)

---

## Constant Work Pattern (CWP)

O padrão de **trabalho constante** defende que **todos os caminhos de execução sejam exercitados continuamente**.
Em vez de manter um serviço apenas como fallback, cada rota é usada simultaneamente (ou balanceada).

Exemplo — provedores de e-mail
Em vez de configurar um servidor “primário” e outro “de reserva”, opere com **dois servidores ativos** atrás de um balanceador de carga. Assim:

- nenhum fica subutilizado;
- ambos são testados o tempo todo;
- a carga torna-se previsível e a recuperação, automática.

> **Definição resumida:** o CWP propõe que cada componente execute sempre o mesmo volume de trabalho em intervalos regulares, geralmente enviando ou recalculando *snapshots* completos em vez de deltas, para reduzir variação de carga e facilitar a recuperação.

---

## Fallback × Failover

| Conceito   | Quando ativa | O que entrega | Relação com CWP |
|------------|--------------|---------------|-----------------|
| **Failover** | Falha do primário | **Serviço idêntico** em infraestrutura redundante | Coerente (ambos caminhos exercitados) |
| **Fallback** | Indisponibilidade do recurso principal | **Versão simplificada/degradada** do serviço | **Viola** o CWP (rota só usada na falha) |

---

## Idempotência

Idempotência é a propriedade de certas operações poderem ser **executadas várias vezes com o mesmo resultado** após a primeira aplicação.

| Verbo HTTP | Idempotente? |
|------------|--------------|
| **GET**    | ✔️ |
| **POST**   | ❌ |
| **PUT**    | ✔️ |
| **DELETE** | ✔️ |
| **PATCH**  | ❌ |
| **HEAD**   | ✔️ |
| **CONNECT**| ❌ |
| **OPTIONS**| ✔️ |

### Estratégias para POST/PATCH

- **Idempotency-Key**: o cliente envia um identificador único; o servidor garante que a operação será executada **apenas uma vez** para esse hash.

---

## “Não faça sofrer quem já está sofrendo”

### Políticas de Retry

- **Número máximo de tentativas** — evita loops infinitos.
- **Intervalo entre tentativas** — fixo, exponencial (*backoff*) ou com *jitter*.
- **Condições de retry** — nem todo erro merece retry (ex.: 404 não, timeout sim).
- **Timeout por tentativa** — cada tentativa tem limite próprio.

### Rate Limiting / Throttling

Controla quantas requisições podem ser processadas em um período. Ao exceder o limite, rejeita chamadas com resposta informativa.

### Circuit Breaker

Age como um disjuntor: **abre o circuito** após muitas falhas e impede novas chamadas até que o serviço se recupere.

---

## Fail Fast

Falhe **o mais rápido possível**: detecte problemas cedo, evite propagar estados inválidos e reduza custo de recuperação.

---

## Desacoplamento

Reduza o **acoplamento mental** — tudo que está materializado no código, mas não declarado publicamente. Componentes independentes sofrem menos impacto em cascata.

---

## Resumo de Princípios

1. **Constant Work Pattern**
2. **Idempotência**
3. **Não fazer sofrer quem já está sofrendo** (retry, rate-limit, circuit breaker)
4. **Fail Fast**
5. **Desacoplamento**
