**26-05-25**

# Resumo: MC/DC (Modified Condition/Decision Coverage)

## Conceito
MC/DC garante que **cada condição** afeta independentemente o **resultado final** da decisão. Para N condições, precisamos de **N+1 testes**.

## Exemplo: 3 Condições com AND (&&)

**Cenário:** `if (a && b && c)`

### Tabela de Verdade Completa
|Linha|a|b|c|Resultado|
|-----|---|---|---|---------|
|1|false|false|false|**false**|
|2|false|false|true|**false**|
|3|false|true|false|**false**|
|4|false|true|true|**false**|
|5|true|false|false|**false**|
|6|true|false|true|**false**|
|7|true|true|false|**false**|
|8|true|true|true|**true**|

### Derivação MC/DC

**Passo 1:** Comece pelo **caso de sucesso** (resultado = true)
- **Linha 8**: `a=true, b=true, c=true → true`

**Passo 2:** Para cada condição, encontre o par que **muda apenas essa condição**

**Para condição 'a':**
- Linha 8: `true, true, true → true`
- Linha 4: `false, true, true → false`
- **Par (8,4)** - apenas 'a' muda

**Para condição 'b':**
- Linha 8: `true, true, true → true`
- Linha 6: `true, false, true → false`
- **Par (8,6)** - apenas 'b' muda

**Para condição 'c':**
- Linha 8: `true, true, true → true`
- Linha 7: `true, true, false → false`
- **Par (8,7)** - apenas 'c' muda

### Conjunto Mínimo de Testes MC/DC
```
Teste 1: a=true,  b=true,  c=true  → true  (Linha 8)
Teste 2: a=false, b=true,  c=true  → false (Linha 4)
Teste 3: a=true,  b=false, c=true  → false (Linha 6)
Teste 4: a=true,  b=true,  c=false → false (Linha 7)
```

**Total: 4 testes** (3 condições + 1)

## Exemplo: 2 Condições com OR (||)

**Cenário:** `if (a || b)`

### Tabela de Verdade
|Linha|a|b|Resultado|
|-----|---|---|---------|
|1|false|false|**false**|
|2|false|true|**true**|
|3|true|false|**true**|
|4|true|true|**true**|

### Derivação MC/DC

**Caso de falha:** Linha 1 (`false, false → false`)

**Para condição 'a':**
- Linha 1: `false, false → false`
- Linha 3: `true, false → true`
- **Par (1,3)** - apenas 'a' muda

**Para condição 'b':**
- Linha 1: `false, false → false`
- Linha 2: `false, true → true`
- **Par (1,2)** - apenas 'b' muda

### Conjunto Mínimo de Testes MC/DC
```
Teste 1: a=false, b=false → false (Linha 1)
Teste 2: a=false, b=true  → true  (Linha 2)
Teste 3: a=true,  b=false → true  (Linha 3)
```

**Total: 3 testes** (2 condições + 1)

## Passos para Aplicar MC/DC

### 1. Monte a Tabela de Verdade
- Liste todas as combinações possíveis
- Calcule o resultado para cada linha

### 2. Identifique o Ponto de Partida
- **Para AND**: comece pelo caso de sucesso (todas true)
- **Para OR**: comece pelo caso de falha (todas false)

### 3. Encontre os Pares
Para cada condição, encontre duas linhas onde:
- **Apenas essa condição** muda de valor
- **Todas as outras** permanecem iguais
- O **resultado final** muda

### 4. Monte o Conjunto Mínimo
- Inclua a linha de referência
- Inclua uma linha de cada par identificado
- Total = N condições + 1 teste

## Documentação dos Testes

**Template:**
```
ID: MC001
Cenário: Validação de acesso (idade >= 18 && renda >= 1000 && aprovado == true)
Condições testadas: idade, renda, aprovado
Casos MC/DC:
- Teste 1: 25, 1500, true  → APROVADO
- Teste 2: 17, 1500, true  → NEGADO (idade)
- Teste 3: 25, 500,  true  → NEGADO (renda)
- Teste 4: 25, 1500, false → NEGADO (aprovado)
```

---

## Fórmula Rápida MC/DC

1. **Monte** tabela de verdade completa
2. **Escolha** ponto de partida (sucesso para AND, falha para OR)
3. **Para cada condição**, encontre par que muda apenas ela
4. **Colete** linha de referência + uma de cada par
5. **Valide** que tem N+1 testes no total

## Vantagens
- **Cobertura completa** com **mínimo de testes**
- **Detecta erros** em condições individuais
- **Padrão obrigatório** em sistemas críticos (aviação, medicina)




``` java

public boolean valorEntre(BigDecimal minimo, BigDecimal maximo) {
	boolean maiorQueMinimo = this.valor.compareTo(minimo) > 0;
	boolean menorQueMaximo = this.valor.compareTo(maximo) < 0;

/**Tabela verdade
 * A B
 * 0 0
 * 0 1
 * 1 0
 * 1 1
 */

	return maiorQueMinimo && menorQueMaximo;
}
```

