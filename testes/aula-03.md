**27-05-25**

# Teste: design para testabilidade

1. Utilizar o construtor para injeção de dependências facilita demais a testabilidade do código. É possível até mesmo criar objetos fake para testes.

2. Priorizar classes que sejam mais fáceis de testar. Por exemplo, podemos usar o EntityManager ou um repositório - utilizar um repositório é muito mais fácil.

3. Sempre utilize, se existir, as classes de mock que o framework oferece. No caso do Spring, existem muitas classes que podemos instanciar como um novo objeto simplificado para ser utilizado em testes, como o MockMvc, que permite simular requisições HTTP e testar controladores de forma isolada.

# Sistema de testes

Testes que vão ser recomendados durante os projetos da jornada.

É preciso ter um sistema definido de testes. Pensando em organização e manutenção, é ideal que todos criem testes similares.

- Boundary test
- MC/DC
- Property based testing
- Self testing

## Prioridades

Vai ser focado em testes de unidade os mais integrados possíveis. Mockar realmente os objetos que podem impactar o tempo de execução - os exemplos mais comuns são banco de dados, serviços externos, etc.

## Testes

### Se escreveu uma condição, o código deve ser testado

Utilizar o MC/DC, utilizar uma variação onde somente vamos testar os métodos que foram escritos. Métodos como `any.stream().allMatch()` não precisam ser testados, pois são métodos da biblioteca padrão do Java.

Quando o controlador/código não tiver uma branch ou condicional, vamos usar o property based testing, que é um teste que verifica se o código funciona para uma variedade de entradas, sem precisar especificar cada caso de teste individualmente.

No caso abaixo, vamos utilizar o property based testing para gerar os valores aleatórios e verificar se o código funciona para esses valores. Para exercitar esse código, se houvesse if/else, loop, seria necessário utilizar o MC/DC para testar as branches e, nesse caso, não usaríamos o property based testing.

```java
@RestController
public class MeuControlador {

    @Autowired
    private MeuServico meuServico;

    @PostMapping("/exemplo")
    public ResponseEntity<String> exemplo() {
        String resultado = meuServico.processar();
        return ResponseEntity.ok(resultado);
    }
}
```
