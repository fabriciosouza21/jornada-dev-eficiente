# Praticas de Design no Código

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

