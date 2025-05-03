# Práticas de Design no Código

*Última atualização: 03/05/2025*

Este repositório contém princípios e práticas de design de código focados em linguagens de programação orientadas a objetos, com exemplos em Java.

## Princípios Fundamentais

### Qualidade não é negociável
O código deve ser feito com design proporcional à complexidade, considerando os conhecimentos que temos no momento da produção.

### Você vai tomar decisões ruins
Todos deixam decisões ruins pelo caminho, independente do nível. O importante é aprender com elas.

### Fazer o que foi combinado
A prioridade máxima é que o código funcione de acordo com o caso de uso definido.

## Práticas Recomendadas

### Implementar de fora para dentro
Execute seu código o mais rápido possível. Priorize implementar de fora para dentro, dessa forma você visualiza o que realmente precisa e usa uma abordagem mais incremental. O "fora" pode ser o endpoint que vai receber uma chamada ou seu teste automatizado.

Quanto mais detalhada for sua imaginação, maior a chance de você conseguir implementar corretamente.

#### Exemplo de implementação

```java
/**
 * Criar um novo endpoint que vai receber um post
 * Nest endpoint eu preciso receber a pessoa logada que gerar o convite e também o projeto
 *      - A pessoa logada vai vir via header
 *      - O projeto pode via parametro comibnado na própria url (path variable)
 * - Também preciso receber os dados do convite. Email e dias de expiração
 * - Carregar a pessoa logada e verificar se ele existe mesmo
 * - Carregar o projeto e verificar se ele existe mesmo
 * - A pessoa logada precisar ser o dono do projeto
 * - eu crio o novo convite para aquele email com aquela data de expiração
 * - Salvo este convite
 * - preciso mandar um email para a pessoa que vai receber o convite
 */
```

### Maximizar a coesão
Deixe a lógica o mais próxima possível do estado. As classes podem manipular seu próprio estado, o que torna a classe mais coesa.

Se você está fazendo uma lógica sobre o estado, adicione essa lógica na própria classe. Isso gera reuso. Por exemplo, a classe de request pode ter um método `toModel()` que retorna uma classe de modelo.

### Proteger as bordas do sistema
Devemos adicionar validações nas bordas, como Objetos de Request e Controllers, como se não houvesse amanhã.

Adicione validações no DTO, use e abuse do Validator do Jakarta. Realize sempre a validação antes de fazer a chamada que realmente executa o serviço.

### Não retorne null
Utilize `Optional` para evitar a validação de nulidade em vários pontos do código. Precisamos ter confiança que o objeto não é nulo.

Se for retornar uma referência, retorne uma abstração que faça sentido. No caso do Java, temos o `Optional`.

### Não ligue parâmetros com entidades
Separe as bordas externas do sistema do seu núcleo. Não ligue parâmetros de requisição externa com objetos de domínio diretamente, assim como não serialize objetos de domínio para resposta de API.

É importante manter separados os atributos de requisição das classes de domínio. No geral, a classe de domínio não precisa saber do DTO, pois caso houvesse vários DTOs que utilizam a mesma classe, ela ficaria muito inchada com métodos específicos de cada borda.

A forma mais comum de fazer isso é criar um construtor no DTO que recebe a classe de domínio.

### Use o construtor para criar objetos em estado válido
O código deve ser o mais restrito possível. Se um parâmetro é obrigatório, o código deve induzir o desenvolvedor a passá-lo.

Se você tiver muitos construtores, pode usar uma fábrica para criar objetos de forma mais semântica.

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

### Deixe pistas quando a compilação não resolver
Deixe pistas que facilitem o uso do código onde não conseguimos resolver com compilação.

Podemos adicionar anotações no construtor para indicar que o parâmetro é obrigatório, usando o `@NotNull` do Jakarta. Por mais que não haja uma validação no construtor, o desenvolvedor terá uma pista de que aquele parâmetro é obrigatório.

Em casos onde somos obrigados a criar um construtor vazio, podemos anotar o construtor com `@Deprecated`, assim o desenvolvedor saberá que aquele construtor não deve ser usado.

Comentários são uma forma de deixar pistas e demonstrar cuidado com as próximas manutenções. David Parnas fala da importância da documentação.

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

### Utilize o que já está pronto
Use tudo que conhecemos que está pronto. Só faça código do zero se for estritamente necessário.

No Spring temos várias classes utilitárias, como os `Assert`.

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

### Use CDD (Cognitive-Driven Development)
Utilize o CDD para facilitar o processo de medição e avaliação de legibilidade. O CDD é uma régua que mensura a complexidade do código, olhando o peso cognitivo.

Torna o processo mais automatizado e é importante estabelecer um limite.

#### Definição de peso cognitivo:
1. Acoplamento com classes específicas de projeto - 1 ICP
2. Condicionais - 1 ICP
3. Blocos extras de código - 1 ICP

### Só altere estado de referências que você cria
Não mexemos nos objetos alheios, a não ser que esse objeto seja criado para isso.

Mantenha um controle muito fino para alteração do estado do sistema. É importante sempre manter as validações, evitando acoplamento mental. Nunca mantenha um objeto com estado inválido.

Usar o `this` duas vezes na mesma linha é um sinal de um "smell" que você está alterando o estado de um objeto que não é seu.

#### Exemplo: Problema
```java
// Entidade Convite
public void transferePraConta(){
    this.convite.getConta().adicionaPessoa(this);
}
```

Para resolver isso, no ponto em que esse método é chamado, substitua para que a própria conta adicione a pessoa. Ou seja, a própria conta vai adicionar/alterar o estado dela.

#### Exemplo: Solução
```java
// Entidade Conta
public void adicionaPessoa(Convite conviteAceito) {
    PessoaUsuaria novaPessoa = conviteAceito.getPessoa();

    Assert.isTrue(!this.pessoas.contains(novaPessoa), "pessoa já existe");

    this.pessoas.add(novaPessoa);
}
```

### Testes automatizados pragmáticos
Testes automatizados devem ser derivados de maneira pragmática através das técnicas já conhecidas. Só depois de derivar casos padrões, usamos nossa criatividade para buscar extrapolar.

Crie os testes base padrões, seguindo os fluxos de execução. Por exemplo, testando cada ramo condicional de um método:

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

---

Este README contém as principais práticas abordadas nas aulas sobre design de código orientado a objetos. Aplicar esses princípios ajudará a desenvolver código mais limpo, manutenível e de alta qualidade.
