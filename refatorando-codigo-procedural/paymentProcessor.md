# PaymentProcessor

COESÃO É MUITO IMPORTANTE!

No getPayments, podemos fazer uma lista com `Collection.unmodifiableList` para evitar que o cliente modifique a lista de pagamentos diretamente. Isso é uma boa prática para manter a integridade da lista de pagamentos. Mas ainda é possível modificar os objetos, como modificar a informação de um pagamento específico.

Se realmente queremos garantir que não haja modificações nos objetos, podemos retornar uma lista com outro objeto que é uma projeção ou DTO. Assim não temos perigo de modificações indesejadas.

> Evite que o encapsulamento vaze.

## Errado

``` java

package paymentprocessor;

import java.util.List;

public class PaymentProcessor {

    public void process(List<Installment> installments, Billing billing) {
        for(Installment installment : installments) {
            Payment payment = new Payment(installment.getAmount());
            billing.getPayments().add(payment); //Não é bom
        }
    }

}


```

## Correto

``` java
package paymentprocessor;

import java.util.List;

public class PaymentProcessor {

    public void process(List<Installment> installments, Billing billing) {
        for(Installment installment : installments) {
            Payment payment = new Payment(installment.getAmount());

            billing.addPayment(payment);
        }
    }

}

```
