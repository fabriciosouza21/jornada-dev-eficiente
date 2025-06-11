# NF

## Utilizar

Utilizar o padrão Observer dos design patterns. Basicamente é criar uma interface comum com método `process` que recebe um objeto `Invoice` e depois criar classes que implementam essa interface. Essas classes são as ações que serão executadas quando a fatura for gerada.

``` java

package invoicegenerator;

import java.util.List;

public class InvoiceGenerator {

    private List<InvoiceGeneratedAction> actions;

    public InvoiceGenerator(List<InvoiceGeneratedAction> actions) {
        this.actions = actions;
    }

    public Invoice generate(ProvidedService providedService) {

        double amount = providedService.getMonthlyAmount();

        Invoice invoice = new Invoice(amount, simpleTax(amount));

        for (InvoiceGeneratedAction action : actions) {
            action.process(invoice);
        }

        return invoice;
    }

    private double simpleTax(double value) {
        return value * 0.06;
    }
}
```

## Event Based Architectures

Arquitetura baseada em eventos, utilizar ferramentas como Kafka.
