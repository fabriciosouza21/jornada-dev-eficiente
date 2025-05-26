
# Design by Contract - Self-test

## Conceito
**Design by Contract** define um "contrato" entre o método e quem o chama:
- **Pré-condições**: o que deve ser verdade ANTES da execução
- **Pós-condições**: o que deve ser verdade APÓS a execução
- **Invariantes**: o que deve ser sempre verdade para o objeto

## Estrutura

### Pré-condições (Preconditions)
- Validam **parâmetros de entrada** e **estado do objeto**
- **Responsabilidade do cliente** garantir

### Pós-condições (Postconditions)
- Validam **valor de retorno** e **estado final**
- **Responsabilidade do método** garantir

### Invariantes
- Propriedades **sempre verdadeiras** para o objeto
- Verificadas **antes e depois** de cada método público

## Exemplo Java

```java
public class ContaBancaria {
    private double saldo;
    private String titular;
    private boolean ativa;

    /**
     * Pré-condições: valor > 0, valor <= saldo, conta ativa
     * Pós-condições: saldo reduzido, retorna true
     */
    public boolean sacar(double valor) {
        // INVARIANTE - antes
        assert invariante() : "Invariante violada";

        // PRÉ-CONDIÇÕES
        assert valor > 0 : "Valor deve ser positivo";
        assert valor <= saldo : "Saldo insuficiente";
        assert ativa : "Conta deve estar ativa";

        // Guarda estado anterior
        double saldoAnterior = this.saldo;

        // EXECUÇÃO
        this.saldo -= valor;

        // PÓS-CONDIÇÕES
        assert this.saldo == (saldoAnterior - valor) : "Saldo incorreto";
        assert this.saldo >= 0 : "Saldo não pode ser negativo";

        // INVARIANTE - depois
        assert invariante() : "Invariante violada";

        return true;
    }

    /**
     * Pré-condições: valor > 0, conta ativa
     * Pós-condições: saldo aumentado, retorna novo saldo
     */
    public double depositar(double valor) {
        // INVARIANTE - antes
        assert invariante() : "Invariante violada";

        // PRÉ-CONDIÇÕES
        assert valor > 0 : "Valor deve ser positivo";
        assert ativa : "Conta deve estar ativa";

        double saldoAnterior = this.saldo;

        // EXECUÇÃO
        this.saldo += valor;

        // PÓS-CONDIÇÕES
        assert this.saldo == (saldoAnterior + valor) : "Saldo incorreto";
        assert this.saldo > saldoAnterior : "Saldo deve aumentar";

        // INVARIANTE - depois
        assert invariante() : "Invariante violada";

        return this.saldo;
    }

    // INVARIANTE DA CLASSE
    private boolean invariante() {
        return titular != null &&
               !titular.trim().isEmpty() &&
               saldo >= 0;
    }
}
```

## Uso em Java

### Habilitar Assertions
```bash
java -ea MinhaClasse  # Habilita assertions
java -da MinhaClasse  # Desabilita assertions
```

### Alternativa com Guava
```java
import static com.google.common.base.Preconditions.*;

public boolean sacar(double valor) {
    checkArgument(valor > 0, "Valor deve ser positivo");
    checkState(ativa, "Conta deve estar ativa");
    // ... resto do método
}
```

## Vantagens para Self-test
- **Documentação executável** - contratos são verificações automáticas
- **Detecção precoce** - falhas são capturadas imediatamente
- **Responsabilidades claras** - quem garante cada condição
- **Testes implícitos** - cada `assert` é uma verificação

---

## Fórmula Rápida

1. **Pré-condições**: valide entradas com `assert` no início
2. **Guarde estado**: valores anteriores para comparação
3. **Execute**: lógica principal do método
4. **Pós-condições**: valide resultados com `assert` no final
5. **Invariantes**: verifique antes e depois de métodos públicos
