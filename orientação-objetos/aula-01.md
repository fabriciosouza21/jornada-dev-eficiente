**28-05-25**

# neste modulo

sequência de passos que sempre chegam no mesmo resultado.

vamos apreder a criar euristicas que podem ou não nos ajudar a utilizar o a orientação a objetos de forma mais eficaz.

Um curso voltado para as observações do proprio Alberto.

# Heuristicas  bem padrões

## Salva no banco de dados

um padrão muito comum é criar a classe que vai salva no banco de dados.

## Controller

criar uma classe que vai receber as requisições e chamar os serviços.

## DTO

criar uma classe que vai receber os dados da requisição e enviar.

## Padrão arquitetura em camadas

casos de uso.

# Coesão basica

manter a coesão, ajudar a reaproveitamento de código, não é a intenção, mas se é necessário é mais fácil de fazer.

Uma boa euristica para melhorar a coesão é verificar se algum serviço utiliza um estado de uma outra classe.

no exemplo abaixo temos que o serviço utiliza o estado de uma classe, isso é um indicativo de que a classe pode ser melhorada, adicionando essa verificação no proprio objeto.

``` java

//Exemplo de classe com serviço

@Service

class ExemploServico {

 fazerAlgo(){
  Domain dominio = obtenerDominio();

     if(dominio.getAtributo() > 0){
   // Lógica quando o atributo é maior que zero
  }
 }
}

class Domain {
 atributo;
}

```

Ajustando o exemplo acima, podemos melhorar a coesão da classe `Domain`:

```java
class Domain {
 private int atributo;

 public boolean isAtributoMaiorQueZero() {
  return atributo > 0;
 }
}

@Service
class ExemploServico {

 fazerAlgo(){
  Domain dominio = obtenerDominio();

  if (dominio.isAtributoMaiorQueZero()) {
   // Lógica quando o atributo é maior que zero
  }
 }
}
```

# Oportunidade uso da coesão básica num vericação de nulidade

as classes devem manipular o seu próprio estado, utililizar o optinal o maximo possíve, ao invez de retorna null.

``` java
Optional.ofNullable(dominio).ifPresent(d -> {
 if (d.isAtributoMaiorQueZero()) {
  // Lógica quando o atributo é maior que zero
 }
});
```

# Explicação sobre criação de abstrações quando o tipo básico não é mais o suficiente

No exemplo do código ele utilizar uma string para representar a senha. o tipo senha tem caracteristicas que não são representadas por uma string, como a validação de tamanho, caracteres especiais, etc.

Nesse caso ele criou uma classe `Senha` que encapsula essas características e validações, tornando o código mais legível e seguro.

para identificar o caso de criar uma abstração, temos dois pontos que podemos obervar:

1. é possivel criar validações facilmente, por exemplo do e-mail, que ja existem validações prontas, como o tamanho, caracteres especiais, etc.
2. Caso não seja possível criar validações facilmente, mas o tipo básico não é suficiente para representar o conceito, como no caso da senha, que tem características específicas que não são representadas por uma string.

# Retorno básico não é mais suficiente

Em alguns casos o retorno básico não é mais suficiente, por exemplo o cpf, o cpf pode está formatado ou não. Nesse caso podemos criar uma classe `Cpf` que encapsula essas características e validações, tornando o código mais legível e seguro.

temos que ter cuidado para não gerar acoplamento mental, ou seja, no caso de retornar um bigdecimal, podemos ter o caso aonde retornamos somente o bigDecimal, quando podemos passar como parametro o tipo de arredondamento a precisão que queremos.

# uso de polimorfismo postegar decisões

processo para identificar para utilização de polimorfismo.

# Exemplo

sempre prestar atenção nos usos de atributos e de métodos que retornam valores, tenter usar na propria classe.

# temlate method utilizando funções

no exemplo ele explica no contexto de uma transação, aonde temos uma chamada asyncrona, no exemplo ele separa a classe que está marcada como trasacional, passando a função como argumento,E bem util no contexto de Template metodos, Outro caso de uso pode ser logs

```java
@Component
public class TransacaoService {
@Transactional
 public <T> T comRetorno(Supplier<T> supplier) {
    // Inicia a transação
    T resultado = supplier.get();
    // Commit da transação
    return resultado;
 }
}
```

# Utilizando Unums ricos

Enums ricos com metodos são possiveis no java.  O enum vai encapsular algumas regras internas, por exemplo injectar um objeto.

Sempre manter a coesão é regra.

``` java

public enum TipoTransacao {
 DEPOSITO {
  @Override
  public void processar(Conta conta, BigDecimal valor) {
   conta.depositar(valor);
  }
 },
 SAQUE {
  @Override
  public void processar(Conta conta, BigDecimal valor) {
   conta.sacar(valor);
  }
 };

 public abstract void processar(Conta conta, BigDecimal valor);
}
```

enums podem ter atributos que definem um comportamento como um booleano.

```java

public enum TrasacaoTipo {
 DEPOSITO(true),
 SAQUE(false);

 private final boolean permiteDeposito;

 TrasacaoTipo(boolean permiteDeposito) {
  this.permiteDeposito = permiteDeposito;
 }
 public boolean isPermiteDeposito() {
  return permiteDeposito;
 }
}
```

# Inverter dependências

A ideia é ter o conceito de port e adapters, ou seja, ter uma interface que define o contrato e uma implementação que define a lógica.

como ter dois serviços de email diferentes. no caso podemos até mesmo ter um mock para testes.

```java
public interface EmailService {
 void enviarEmail(String destinatario, String assunto, String mensagem);
}

public class EmailServiceImpl implements EmailService {
 @Override
 public void enviarEmail(String destinatario, String assunto, String mensagem) {
  // Lógica para enviar email
 }
}
public class EmailServiceMock implements EmailService {
 @Override
 public void enviarEmail(String destinatario, String assunto, String mensagem) {
  // Lógica mock para testes
 }
}
```

# Self testing

NO exemplo ele utilizar a logica do TreeSet que retorna o valor true, quando o valor é adicionado, e false quando o valor já existe. Se o valor já existe ele utilizar o Assert.isFalse do spring para validar.

Cuidado com mapeamento mentais, as prevalidações ajudam a evitar erros.

>O método add() de Set em Java é usado para adicionar um elemento específico a uma coleção de Sets. A função add() do conjunto adiciona o elemento somente se o elemento especificado ainda não estiver presente no conjunto, caso contrário, a função retorna False se o elemento já estiver presente no conjunto.

```java
public class ExemploSelfTesting {
 public void adicionarValor(Set<Integer> conjunto, Integer valor) {
  boolean adicionado = conjunto.add(valor);
  Assert.isFalse(adicionado, "O valor já existe no conjunto");
 }
}
```
