# Tax Calculate

``` java

package taxcalculator;

import static taxcalculator.Role.*;

public class TaxCalculator {

    public double calculateTax(Employee employee) {
        if(DEVELOPER.equals(employee.getRole())) {
            return tenOrTwentyPercent(employee);
        }

        if(DBA.equals(employee.getRole()) || TESTER.equals(employee.getRole())) {
            return fifteenOrTwentyFivePercent(employee);
        }

        // ... and many more ...

        throw new RuntimeException("invalid employee");
    }

    private double tenOrTwentyPercent(Employee employee) {
        if(employee.getBaseSalary() > 3000.0) {
            return employee.getBaseSalary() * 0.8;
        }
        else {
            return employee.getBaseSalary() * 0.9;
        }
    }

    private double fifteenOrTwentyFivePercent(Employee employee) {
        if(employee.getBaseSalary() > 2000.0) {
            return employee.getBaseSalary() * 0.75;
        }
        else {
            return employee.getBaseSalary() * 0.85;
        }
    }

}

```

1. Extrair as interfaces comuns criando estratégias

No exemplo ele mostra duas classes que têm a mesma interface, as duas classes têm como parâmetro employee e utilizam os mesmos atributos de employee. Uma estratégia simples é separar esses dois métodos em classes separadas.

``` java

interface TaxStrategy {
 double calculateTax(Employee employee);
}

```

``` java

class TenOrTwentyPercent implements TaxStrategy {

 @Override
 public double calculateTax(Employee employee) {
  if(employee.getBaseSalary() > 3000.0) {
   return employee.getBaseSalary() * 0.8;
  } else {
   return employee.getBaseSalary() * 0.9;
  }
 }
}

```

``` java

class FifteenOrTwentyFivePercent implements TaxStrategy {

 @Override
 public double calculateTax(Employee employee) {
  if(employee.getBaseSalary() > 2000.0) {
   return employee.getBaseSalary() * 0.75;
  } else {
   return employee.getBaseSalary() * 0.85;
  }
 }
}

```

# Evitando repetição

A única diferença entre as duas classes são os valores usados. Podemos mapear os valores comuns e criar uma classe que implementa a interface `TaxStrategy` e recebe os valores como parâmetro no construtor. No exemplo ele cria uma classe chamada `ThresholdBasedTaxCalculator` que tem um threshold, aboveThresholdTax e belowThresholdTax, e implementa o método `calculateTax` da interface `TaxStrategy`.

Existe uma estratégia, também, que podemos criar uma classe que estende a classe ThresholdBasedTaxCalculator, e ela instancia a classe pai. Esse caso só vale a pena se for importante para a regra de negócio, pois tende a gerar uma quantidade grande de classes.

``` java
class ThresholdBasedTaxCalculator implements TaxStrategy {

 private final double threshold;
 private final double aboveThresholdTax;
 private final double belowThresholdTax;

 public ThresholdBasedTaxCalculator(double threshold, double aboveThresholdTax, double belowThresholdTax) {
  this.threshold = threshold;
  this.aboveThresholdTax = aboveThresholdTax;
  this.belowThresholdTax = belowThresholdTax;
 }

 @Override
 public double calculateTax(Employee employee) {
  if(employee.getBaseSalary() > threshold) {
   return employee.getBaseSalary() * aboveThresholdTax;
  } else {
   return employee.getBaseSalary() * belowThresholdTax;
  }
 }
}
```

Na classe cliente fica dessa forma:

``` java

public double calculateTax(Employee employee) {

 TaxStrategy fifteenOrTwentyFivePercent = new ThresholdBasedTaxCalculator(2000.0, 0.25, 0.15);

 TaxStrategy tenOrTwentyPercent = new ThresholdBasedTaxCalculator(3000.0, 0.20, 0.10);

 if(DEVELOPER.equals(employee.getRole())) {
  return tenOrTwentyPercent.calculateTax(employee);
 }

 if(DBA.equals(employee.getRole()) || TESTER.equals(employee.getRole())) {
  return fifteenOrTwentyFivePercent.calculateTax(employee);
 }

 // ... and many more ...

 throw new RuntimeException("invalid employee");
}
```

## Usando Enum

O enum instancia o tipo da estratégia, em casos onde temos tipos muito limitados é o caso ideal. Se as informações são criadas em tempo de compilação, o enum é maravilhoso.

``` java

public enum Role {
DEVELOPER(new ThresholdBasedTaxCalculator(3000.0, 0.20, 0.10)),
DBA(new ThresholdBasedTaxCalculator(2000.0, 0.25, 0.15)),
TESTER(new ThresholdBasedTaxCalculator(2000.0, 0.25, 0.15));


 private final TaxStrategy taxStrategy;

 Role(TaxStrategy taxStrategy) {
  this.taxStrategy = taxStrategy;
 }

 public TaxStrategy getTaxStrategy() {
  return taxStrategy;
 }

}
```

Porém isso não acontece em todos os casos. Em muitos casos teremos que acessar o banco de dados. Para implementações dinâmicas os enums não são a melhor opção.

## Informação do banco

Na aula ele cria um resolver para lidar com caráter dinâmico, o resolver `TaxCalculationResolver` recebe como parâmetro no construtor um `TaxStrategyRepository` e tem um método chamado `forRole` que retorna um `TaxStrategy`

``` java
public class TaxCalculationResolver {

 private final TaxStrategyRepository taxStrategyRepository;

 public TaxCalculationResolver(TaxStrategyRepository taxStrategyRepository) {
  this.taxStrategyRepository = taxStrategyRepository;
 }

 public TaxStrategy forRole(Role role) {

  TaxValues taxValues = taxStrategyRepository.findByRole(role);

  return new ThresholdBasedTaxCalculator(
   taxValues.getThreshold(),
   taxValues.getAboveThresholdTax(),
   taxValues.getBelowThresholdTax()
  );
 }
}
```
