# Praticas de Design no Código

02/05/2025

design de código focando em linguagem de programação orientadas a objetos.



## Direcionamento

### Aula 02 - qualidade não é negociado

Qualidade não é negociável. O Código deve ser feito com o design proprcional a complexidade considerando os conhecimentos que temos no momento da produção.

### Aula 03 - você vai tomar decisões ruins

Vocẽ vai deixa desões ruins pelo caminho, não importa seu nível.

### Aula 04 - fazer o que foi combinado

A prioridade máxima é funcionar de acordo com o caso de uso.

## Praticas

### Aula 05 - implemente fora para dentro

Execute o seu código o mais rápido possível. Priorize implementar de fora para dentro, dessa forma você visualiza o que realmente precisa e usa uma  abordagem mais incremental. O "Fora" aqui pode ser o endpoint que vai receber uma chamada, pode ser seu teste automatizado.

### Aula 06 - implementar fora para dentro

quanto mais detelhada e sua imaginação maior a chance de você conseguir implementar

### Aula 13 - exemplo da implementação

``` java

/**
 * Criar um novo endpoint que vai receber um post
 * Nest endpoint eu preciso receber a pessoa logada que gerar o convite e também o  projeto
 *  	- A pessoa logada vai vir via header
 * 		- O projeto pode via parametro comibnado na  prória url (path variable)
 * - Também preciso receber os dados do convite. Email e dias de expiração
 * - Carregar a pessoa logadaa e verifica se ele existe mesmo
 * - Carregar o projeto e verificar se ele existe mesmo
 * - A pessoa logada precisar ser o dono do projeto
 * - eu crio o nvo convite para aquel email com aquela data de experação
 * - Salvo este convite
 * - precioso manda um email para a pessoa que vai receber o convite
 *  */
```

### Aula 14 - maximizar a coesão

deixar logica mais proxima possivel do estado. as classes podem manibular o estado dela, vai tornar a classe mais coesa.

A propria classe pode manipular o estado dela.

estou fazendo uma logica sobre o estado, se sim, adicionar a logica na classe.

você ganhar reuso

A classe de request pode ter um metodo que vai retorna uma classe de modelo. toModel

## aula 15

Protegemos as borda do sitemas como se não houvese amanhã

devemos adicinar as validações nas bordas, Objetos de Request, Controllers


Adicionar as validações no DTO, use e abuse do Validator do jakata.

realizer sempre a validação antes de fazer a chamada realmente fazer a execução do serviço.

sempre adicionar validações e retorna o status.


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

## Aula 20 - utilizer o que ta pronto

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
##
