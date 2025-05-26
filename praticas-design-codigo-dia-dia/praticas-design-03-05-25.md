# Práticas de design no código

**03/05/2025**

## Aula 16 - Não retorne null

Utilizar Optional, a partir do fluxo para evitar a validação de nulidade em vários pontos do código, precisamos ter confiança que o objeto não é nulo.

Se for retornar a referência, retorne ou retorne uma abstração que faça sentido. No caso do Java, temos o Optional.

## Aula 17 - Não ligamos parâmetros com entidades

Separamos as bordas externas do sistema do seu núcleo. Não ligamos parâmetros de requisição externa com objetos de domínio diretamente, assim como não serializamos objetos de domínio para resposta de API.

É importante manter separados os atributos de requisição das classes de domínio.

No geral, a classe de domínio não precisa saber do DTO, pois caso houvesse vários DTOs que utilizam a mesma classe, a classe de domínio ficaria muito inchada com métodos muito específicos de cada borda.

A forma mais comum de fazer isso é criar um construtor no DTO que recebe a classe de domínio.


## Aula 18 - Prática: informação obrigatória no construtor


**Usamos o construtor para criar o objeto no estado válido**

O código deve ser o mais restrito possível. Se um parâmetro é obrigatório, o código deve induzir o desenvolvedor a passar o parâmetro obrigatório.

Se você tiver muitos construtores, você pode usar uma fábrica para criar objetos de uma forma mais semântica.


```java
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

## Aula 19 - Deixe pistas quando compilação não resolver

**Deixamos pistas que facilitem o uso do código onde não conseguimos resolver com compilação.**

Podemos adicionar anotações no construtor para indicar que o parâmetro é obrigatório, ou podemos usar o @NotNull do Jakarta. Por mais que não haja uma validação no construtor, o compilador não vai conseguir validar isso, mas o desenvolvedor vai ter uma pista de que aquele parâmetro é obrigatório. No caso do construtor, em alguns casos onde somos obrigados a criar um construtor vazio, podemos anotar o construtor vazio com o @Deprecated, assim o desenvolvedor vai saber que aquele construtor não deve ser usado.


Comentários são uma forma de deixar pistas e de cuidado com as próximas manutenções.

David Parnas fala da importância da documentação.

```java
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

## Aula 21 - Utilize o que está pronto

**Usamos tudo que conhecemos que está pronto. Só fazemos código do zero se for estritamente necessário.**

No Spring temos várias classes utilitárias como Asserts.

```java
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
## Aula 22 - CDD

**Utilizamos o CDD para facilitar o processo de medição e avaliação de legibilidade.**

O CDD é uma régua que mensura a complexidade do código, olhando o peso cognitivo.

O objetivo é tornar o processo mais automatizado.

É importante estabelecer um limite.

### Definição de peso cognitivo

1. Acoplamento com classes específicas de projeto - 1 ICP
2. Condicionais - 1 ICP
3. Blocos extras de código - 1 ICP


## Aula 23 - Práticas CDD

**Só alteramos estado de referências que criamos. Não mexemos nos objetos alheios. A não ser que esse objeto seja criado para isso.**


Controle muito fino para alteração do estado do sistema.

É importante sempre manter as validações, evite fazer acoplamento mental. Então, sempre faça validações. Nunca mantenha um objeto com o estado inválido.

No método adicionar pessoa, receber o convite ao invés da pessoa, pois somente adicionamos pessoas que estão no convite. Assim, obrigamos a passar o convite.

Usar o this duas vezes na mesma linha é um sinal de um smell que você está alterando o estado de um objeto que não é seu.

No caso abaixo, o convite altera o estado de conta.

Para resolver isso, no ponto em que esse método é chamado, substituir para que a própria conta adicione a pessoa. Ou seja, a própria conta vai adicionar/alterar o estado dela.



```java
// Entidade Convite
public void transferePraConta(){
	this.convite.getConta().adicionaPessoa(this);
}
```

```java
// Entidade Conta

public void adicionaPessoa(Convite conviteAceito) {
	PessoaUsuaria novaPessoa = conviteAceito.getPessoa();

	Assert.isTrue(!this.pessoas.contains(novaPessoa), "pessoa já existe");

	this.pessoas.add(novaPessoa);
}
```

## Aula 24 - Práticas CDD

**Testes automatizados devem ser derivados de maneira pragmática através das técnicas já conhecidas. Só depois de derivar casos padrões, usamos nossa criatividade para buscar extrapolar.**

Criar os casos base padrões.

No caso abaixo, seria alterando cada if. Seguir os fluxos de execução.

```java
public class DiscountCalculator {
	public double calculateDiscount(double salary, int yearsWorked, boolean hasMetTarget){
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
