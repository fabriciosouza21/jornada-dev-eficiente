# praticas de design no código

**03/05/2025**

## Aula 16 não retorne null

Utilizar Optional, apartir do fluxo para evitar a validação de nulidade em varios pontos do código, precisamos ter confiaça que o objeto não é nulo.

se for retorna a referencia retorne ou retorne um abstração que faça sentido no caso do java temos o Optional.

## Aula 17 - não Ligamos parametros com entidades

Separamos as Bordas externas do sistema do seu núcleo. Não ligamos parâmetors de requisição externa com objetos de dominio diretamente assim como não srializamos objetos de domínio para eresposta de API.

È importante manter separado os atributos de requisição das classes de dominio.

No geral a classe de dominio não precisar saber do DTO, pois caso houve-se varios dto que utilizam a mesma classe, a classe de dominio ficaria muito inchada com metodos muitos especificos de cada borda.

A forma mais comum de fazer isso é fazer um construtor no dto que recebe a classe de dominio.


## Aula 18 - pratica informação obrigatoria construtor


**Usamos o contrutor para criar o objeto no estado Válido**

O código deve ser o mais restrito possivel, se um parametro é obrigatorio, o código deve induzir o desenvolvedor a passar o parametro obrigatorio.

se você estiver muito construtores, você pode usar uma fabrica para criar objetos de uma forma mais semântica.


``` java

public class User {
	private final String name;
	private final String email;
	private final String password;

	public User(String name, String email, String password) {
		this.name = name;
		this.email = email;
		this.password = password;
	}

	public String getName() {
		return name;
	}
	public String getEmail() {
		return email;
	}
	public String getPassword() {
		return password;
	}
}

```

## Aula 19 - deixe pistas quando compilação não resolver

**Deixamos pistas que facilitem o uso do código onde não conseguimos resolver com compilação.**

podemos adicionar anotações no construtor para indicar que o parametro é obrigatorio, ou podemos usar o @NotNull do jakarta. por mais não haja uma validação no construtor, o compilador não vai conseguir validar isso, mas o desenvolvedor vai ter uma pista de que aquele parametro é obrigatorio. no caso do construto em alguns caso aonde somos obrigados a criar um construtor vazio, podemos anotar o construtor vazio com o @Deprecated, assim o desenvolvedor vai saber que aquele construtor não deve ser usado.


comentarios são uma forma de deixar pistas, e de cuidado com as proximas manutenções.

David Parnas, fala da importancia da documentação.

``` java

public class User {
	private final String name;
	private final String email;
	private final String password;

	public User(@NotNull String name, @NotNull String email, @NotNull String password) {
		this.name = name;
		this.email = email;
		this.password = password;
	}

	@Deprecated
	public User() {
	}

	public String getName() {
		return name;
	}
	public String getEmail() {
		return email;
	}
	public String getPassword() {
		return password;
	}
}

```

## Aula 21 - utilizer o que ta pronto

**Usamos tudo que conhecemos que está pronto. Só fazemos código do zero se for estritametne necessário.**

No spring temos varias classe utilitarias com Asserts.

``` java

public class User {
	private final String name;
	private final String email;
	private final String password;

	public User(@NotNull String name, @NotNull String email, @NotNull String password, boolean ativo) {
		Assert.notNull(name, "name is required");
		Assert.hasText(email, "email is required");
		Assert.notNull(password, "password is required");
		Assert.isTrue(ativo, "ativo is required");
		this.name = name;
		this.email = email;
		this.password = password;
	}

	public String getName() {
		return name;
	}
	public String getEmail() {
		return email;
	}
	public String getPassword() {
		return password;
	}
}

```
## Aula 22 - cdd

**Utilizamos o CDD Para facilitar o processo de medição e valiação de legibilidade.**

o CDD é reguar que mesura a complexidade do código, olhando o pesso cognitivo.

O tornar o processo mais automatizado.

é importante estabelecer um limite.

### Definição de pesso cognitivo

1. acoplamento com classe especificas de projeto - 1 icp
2. conicionais - 1 icp
3. blocos extras de código - 1 icp


## aula 23 - praticas cdd

**Só alteramos estado de referências que criamos. Não mexemos nos objetos alheios. A não ser que esse objeto seja criado para isso.**


Controle muito fino para alteração do estado do sistema.

é importante sempre manter as validações, evite fazer acoplamento mental. então sempre faça validações. nunca mantenha um objeto com o estado inválido.

no metodo adicionar pessoa, receber o convite ao invez da pessoa, pois somente adicionar pessoas que estão no convite. asssim obrigamos a passar o convite.

usar o this duas 2vezes na mesma linha é um sinal de um smell que você ta alterando o estado de um objeto que não é seu.

no caso abaixo o convite alterar o estado de conta.

para resolver isso, no ponto em esse metodo é chamado substiuir para que a propria conta adicione a pessoa. Ou seja, o propria conta vai adicionar alterar o estado dela.



``` java
// Entidade Convite
public void transferePraConta(){
	this.convite.getConta().adicionaPessoa(this);
}
```

``` java

// Entitdade Conta

public void adicionaPessoa( Convite conviteAceito) {
	PessoaUsuaria novaPessoa = conviteAceito.getPessoa();

	Assert.isTrue(!this.pessoas.contains(novaPessoa), "pessoa já existe");

	this.pessoas.add(novaPessoa);
}
```

## Aula 24 - praticas cdd

**Teste automatizados devem ser derivados de maneira pragmática através das técnicas já conhecidas. Só depois de derivar casos padrões, usamos nossa criatividade para buscar extrapolar.**

criar o base padrões

no caso abaixo, seria um alterando cada if. seguir os fluxos de execução.

``` java

public class DiscountCalculator {
	public double calculateDiscount(double salary, intyearsWorked, boolean hasMetTarget){
		if(yearsWorked > 10 && hasMetTarget){
			return salary * 0.15; //15%
		}else if(yearsWorked > 5 && hasMetTarget){
			return salary * 0.10; //10%
		}else if(yearsWorked > 5 || hasMetTarget){
			return salary * 0.05; //5%
		}else {
			return 0;
		}
	}
}

```

