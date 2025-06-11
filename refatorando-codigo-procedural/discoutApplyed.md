
# Refatorando código procedural

[https://github.com/mauricioaniche/refactoring-workshop/tree/main/solutions/src/main/java/discountapplier](https://github.com/mauricioaniche/refactoring-workshop/tree/main/solutions/src/main/java/discountapplier)

## discountApplier

1. A classe tem muitas funcionalidades.
2. O método de desconto por produto é extensível por modificações.
3. O método de desconto por valor tem o mesmo problema.
4. O método `apply` não torna claro que pode ser aplicado somente um desconto.
5. O método `subtract` muda o estado do atributo `valor` diretamente; `subtract` deveria receber qualquer valor.
6. O tipo do produto está hardcoded.

```java
package discountApplier;

import common.Basket;

public class DiscountApplier {

    public void apply(Basket basket) {
        // O método apply não torna claro que pode ser aplicado somente um desconto
        boolean ok = discountPerProduct(basket);
        if(!ok) discountPerAmount(basket);
    }

    private boolean discountPerProduct(Basket basket) {
        if(basket.has("MACBOOK") && basket.has("IPHONE")) {
            basket.subtract(basket.getAmount() * 0.15);
            return true;
        }

        if(basket.has("NOTEBOOK") && basket.has("WINDOWS PHONE")) {
            basket.subtract(basket.getAmount() * 0.12);
            return true;
        }

        if(basket.has("XBOX")) {
            basket.subtract(basket.getAmount() * 0.7);
            return true;
        }

        // ... and many more ...

        return false;
    }

    private void discountPerAmount(Basket basket) {

        if(basket.getAmount() <= 1000 && basket.qtyOfItems() <= 2) {
            basket.subtract(basket.getAmount() * 0.02);
            return;
        } else if(basket.getAmount() > 3000 && basket.qtyOfItems() < 5 && basket.qtyOfItems() > 2) {
            basket.subtract(basket.getAmount() * 0.05);
            return;
        } else if(basket.getAmount() > 3000 && basket.qtyOfItems() >= 5) {
            basket.subtract(basket.getAmount() * 0.06);
            return;
        }
    }
}
```

## Quebrando classes

1. Quebra os dois métodos privados em classes separadas (`DiscountPerProduct` e `DiscountPerAmount`).
2. Cria uma interface de desconto entre as duas classes (`DiscountStrategy`), com o método `apply`.
3. Faz a injeção de dependência no construtor dos descontos, e usa um `for` iterando com `break` caso o desconto seja aplicado (Obs: não garante a ordem; tem precedência?).
4. Cria uma classe factory para criar os descontos e injetar no construtor do `DiscountApplier`. Com a ordem correta, resolve parcialmente o problema; usa uma injeção simples instanciando os tipos dos descontos.

> Implementação inicial

`DiscountStrategy`

```java
package discountapplier;

import common.Basket;

public interface DiscountStrategy {
    void apply(Basket basket);
}
```

`DiscountApplier`

```java
package discountapplier;

import common.Basket;
import java.util.List;

public class DiscountApplier {

    private final List<DiscountStrategy> discounts;

    public DiscountApplier(List<DiscountStrategy> discounts) {
        this.discounts = discounts;
    }

    public void apply(Basket basket) {
        for (DiscountStrategy discount : discounts) {
            boolean isApply = discount.apply(basket);
            if (isApply) {
                break; // Se um desconto for aplicado, sai do loop
            }
        }
    }
}
```

## Abstraindo regras

**Resolvendo o problema dos `ifs`**: o problema dos `ifs` é resolvido usando uma lista que é injetada no construtor. Ela é iterada e, assim que encontra o primeiro desconto aplicável, é usado um `break` para sair do loop. A factory é responsável por criar os descontos e injetá-los no construtor do `DiscountApplier`.

A principal sacada é ter cuidado com o uso da interface. A interface `DiscountStrategy` é utilizada de forma muito adequada.

A factory tem uma função muito bem definida, que é instanciar corretamente os descontos:

```java
package discountapplier;

import java.util.Arrays;
import java.util.List;

public class DiscountApplierFactory {

    public DiscountApplier build() {
        List<DiscountStrategy> discounts = Arrays.asList(
            new DiscountPerProduct(Arrays.asList("MACBOOK", "IPHONE"), 0.15),
            new DiscountPerProduct(Arrays.asList("NOTEBOOK", "WINDOWS PHONE"), 0.12),
            new DiscountPerAmount(1000, 1500, 6, 7, 0.02)
        );

        return new DiscountApplier(discounts);
    }
}
```

## Command ou Query

O método `apply` pode não aplicar o desconto, o que gera um comportamento que não é muito claro. Para resolver, podemos separar a lógica da condicional: na interface `DiscountStrategy`, podemos fazer o `apply` retornar `void` e criar um método `shouldBeApplied`, que faz a verificação se o desconto deve ou não ser aplicado. Após essa mudança, podemos ajustar a classe `DiscountApplier` para chamar o método `shouldBeApplied` antes de aplicar o desconto, deixando claro que o desconto pode ou não ser aplicado.

```java
package discountapplier;

import common.Basket;
import java.util.List;

public class DiscountApplier {

    private final List<DiscountStrategy> discounts;

    public DiscountApplier(List<DiscountStrategy> discounts) {
        this.discounts = discounts;
    }

    public void apply(Basket basket) {
        for (DiscountStrategy discount : discounts) {
            if(discount.shouldBeApplied(basket)) {
                discount.apply(basket);
                break;
            }
        }
    }
}
```

## Melhorando `Basket`

O primeiro passo é renomear o método `subtract` para `applyDiscountByPercentage`, tornando mais claro o que o método faz. O método `subtract` fazia a lógica de alteração do estado do desconto diretamente. O novo método recebe o valor do desconto como parâmetro e aplica sobre o valor do `Basket`, deixando a intenção explícita.

Além disso, agora podemos adicionar validações — por exemplo, o desconto deve ser maior que zero e menor ou igual a 1. O texto aborda duas estratégias para lidar com o erro:

* A primeira, que é mais recomendada, lança uma exceção (`IllegalArgumentException`), pois é claro que o valor não é permitido.
* A segunda apenas ignora o desconto, mas isso gera ambiguidade, já que o valor não é alterado.

A decisão final deve ser pensada com cuidado, pois impacta diretamente quem chama o método `applyDiscountByPercentage`.

```java
package common;

public class Basket {

    public void applyDiscountByPercentage(double discount) {
        if (discount <= 0 || discount > 1) {
            throw new IllegalArgumentException("Discount must be greater than 0 and less than or equal to 1");
        }
        this.amount -= this.amount * discount;
    }
}
```

## Dados e comportamento

**Resolve o problema da factory com valores hardcoded**

A classe factory fica poluída com valores vindos do banco de dados ou da fonte onde os descontos estão definidos.

No exemplo, ele cria as classes de domínio `DiscountProduct` e `DiscountAmount` e também um repositório para simular essa busca. A factory fica responsável por criar as estratégias corretas baseadas no retorno do "banco", removendo o código hardcoded. No exemplo abaixo, somente a factory tem a dependência do repositório.

Tenta-se separar ao máximo a fonte dos dados ("banco de dados") da regra de cálculo do desconto.

Às vezes, desacoplar a lógica de negócio do banco de dados não é uma tarefa simples — e nem sempre é viável.

Não conseguir desacoplar é mais exceção do que regra.

```java
public class DiscountApplierFactory {

    private DiscountRepository repository;

    public DiscountApplierFactory(DiscountRepository repository) {
        this.repository = repository;
    }

    public DiscountApplier build() {
        List<DiscountStrategy> discounts = new ArrayList<>();

        List<DiscountProduct> allDiscountsPerProduct = repository.getAllDiscountsPerProduct();
        for (DiscountProduct discountProduct : allDiscountsPerProduct) {
            discounts.add(new DiscountPerProduct(
                discountProduct.getProducts(),
                discountProduct.getDiscount()
            ));
        }

        List<DiscountAmount> allDiscountsPerAmount = repository.getAllDiscountsPerAmount();
        for (DiscountAmount discountAmount : allDiscountsPerAmount) {
            discounts.add(new DiscountPerAmount(
                discountAmount.getMinAmount(),
                discountAmount.getMaxAmount(),
                discountAmount.getMinItems(),
                discountAmount.getMaxItems(),
                discountAmount.getDiscount()
            ));
        }

        return new DiscountApplier(discounts);
    }
}
```
