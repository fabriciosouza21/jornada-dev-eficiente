**26-05-25**

# Property-Based Testing

## Conceito
**Property-Based Testing** testa **propriedades** que devem ser sempre verdadeiras, gerando **dados aleatórios** automaticamente ao invés de usar casos específicos.

ao invez do @DisplayName utilizamos o @Label para nomear o teste, utilizar o @DisplayName, vai ocacionar um erro na execução.

## jqwik para Java + JUnit 5

### Dependência Maven
```xml
<dependency>
    <groupId>net.jqwik</groupId>
    <artifactId>jqwik</artifactId>
    <version>1.7.4</version>
    <scope>test</scope>
</dependency>
```

## Exemplo: Classe Validadora de Idade

### Classe a ser testada
```java
public class ValidadorIdade {
    private static final int IDADE_MINIMA = 0;
    private static final int IDADE_MAXIMA = 120;

    public boolean isIdadeValida(int idade) {
        return idade >= IDADE_MINIMA && idade <= IDADE_MAXIMA;
    }

    public String classificarIdade(int idade) {
        if (!isIdadeValida(idade)) {
            throw new IllegalArgumentException("Idade inválida: " + idade);
        }

        if (idade < 18) return "Menor";
        if (idade < 60) return "Adulto";
        return "Idoso";
    }
}
```

### Testes Property-Based

```java
import net.jqwik.api.*;

class ValidadorIdadePropertyTest {

    private ValidadorIdade validador = new ValidadorIdade();

    @Property
    void idadesValidasRetornamTrue(
        @ForAll @IntRange(min = 0, max = 120) int idade
    ) {
        // Propriedade: idades entre 0-120 são sempre válidas
        assertTrue(validador.isIdadeValida(idade));
    }

    @Property
    void idadesInvalidasRetornamFalse(
        @ForAll int idade
    ) {
        Assume.that(idade < 0 || idade > 120);

        // Propriedade: idades fora do range 0-120 são sempre inválidas
        assertFalse(validador.isIdadeValida(idade));
    }

    @Property
    void valoresLimitesComportamentoCorreto(
        @ForAll @IntRange(min = -10, max = 130) int idade
    ) {
        boolean resultado = validador.isIdadeValida(idade);

        if (idade >= 0 && idade <= 120) {
            // Dentro dos limites = válido
            assertTrue(resultado);
        } else {
            // Fora dos limites = inválido
            assertFalse(resultado);
        }
    }

    @Property
    void classificacaoNuncaFalhaParaIdadesValidas(
        @ForAll @IntRange(min = 0, max = 120) int idade
    ) {
        // Propriedade: idades válidas nunca lançam exceção
        assertDoesNotThrow(() -> {
            String classificacao = validador.classificarIdade(idade);
            assertNotNull(classificacao);
            assertTrue(classificacao.equals("Menor") ||
                      classificacao.equals("Adulto") ||
                      classificacao.equals("Idoso"));
        });
    }

    @Property
    void classificacaoSempreFalhaParaIdadesInvalidas(
        @ForAll int idade
    ) {
        Assume.that(idade < 0 || idade > 120);

        // Propriedade: idades inválidas sempre lançam exceção
        assertThrows(IllegalArgumentException.class, () -> {
            validador.classificarIdade(idade);
        });
    }
}
```

## Foco nos Valores Limite

```java
@Property
void testeLimitesEspecificos() {
    // Testa exatamente os valores críticos

    // Limites válidos
    assertTrue(validador.isIdadeValida(0));   // Mínimo
    assertTrue(validador.isIdadeValida(120)); // Máximo

    // Limites inválidos
    assertFalse(validador.isIdadeValida(-1));  // Abaixo do mínimo
    assertFalse(validador.isIdadeValida(121)); // Acima do máximo
}

@Property
void propriedadeLimitesMinMax(
    @ForAll @IntRange(min = Integer.MIN_VALUE, max = Integer.MAX_VALUE) int idade
) {
    boolean valida = validador.isIdadeValida(idade);

    // Propriedade dos limites
    if (idade == 0 || idade == 120) {
        assertTrue(valida); // Exatamente nos limites
    } else if (idade > 0 && idade < 120) {
        assertTrue(valida); // Dentro dos limites
    } else {
        assertFalse(valida); // Fora dos limites
    }
}
```

## Resultado

O jqwik executa **100 testes por padrão** com valores aleatórios:
- Gera idades entre -2.147.483.648 e 2.147.483.647
- Testa automaticamente valores extremos como 0, 120, -1, 121
- Encontra casos que quebram as propriedades
- Reduz automaticamente para o menor caso que falha

---

## Vantagem

Ao invés de testar apenas alguns casos específicos:
```java
@Test void testeTradicional() {
    assertTrue(validador.isIdadeValida(25));
    assertFalse(validador.isIdadeValida(-5));
}
```

O property-based testa **centenas de casos** automaticamente, incluindo todos os valores limite e extremos possíveis.




