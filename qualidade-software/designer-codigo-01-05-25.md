# Designer de código

## Aula 11

a diferença entre arquitetura e design de código e que as decisões arquiteturais são mais estaveis, respeitando os atributos de qualidade.

também vão haver decisões de arquitetura de código, mas que também vão ser mais estaveis.

as decisões de design vão ser mais aplicadas no dia a dia, decisões que vão ser tomadas na hora de implementação.

as decisões de design devem que pode melhorar a experiencia de desenvolvimento.


## Aula 12

### CDD (Cognitive-Driven Development)

expirado na teoria da carga cognitiva, ele utilizar o conceito que temos uma memoria de trabalho limitada,

A ideia do Cdd é que pessoas da mesma equipe, consigam mensurar a carga cognitiva.


medir algo é objetivo,

avaliar algo é subjetivo, pois depende do contexto.

quando definimos um limite alto, vão haver varios dos elementos de ICP.

Com forma de mensurar você pode criar um processo para refatorar o código. A ideia é criar um indicador de qualidade e dinâmico que se adapte ao projeto.

### Exemplo

- vamos supor que `if/else/operador ternario,switch/loops` cada condiconal aumenta + 1 (intrinsic complex point)

- acoplamento com classe especificas do projeto - 1 ICP

- blocos de código extra (try, catch, funções como argumento) - 1 ICP

limite de 40 ICP


## Aula 13 - open/closed

Mais importante do que entende metodos é importante entender que estamos buscando mecanismo para melhorar a qualidade do código e evolução do código.

principio do aberto e fechado, encontre meus de escreve o código, que não precise modificar o código existente.

em liguagem de programção como java, utilizamos interfaces, as classes usuario a classe abstrata e um serviço seria responsavel por injectar a implementação correta.

partes do seu software ja precisar saber se vai ser extendidos via extensão ou via modificação.

Nem todas as features precisam ser extensíveis, generalizar tornar o código mais complexo, e mais difícil de entender.



## Aula 14 - me acoclobo a frameworks ou não

frameworks

rail e django, foram criados para ter muito beneficio, no acoplamento, tendo um beneficio na construção do software.

alguns frameworks com struct do java, o acoplamento é muito alto, e não gerava o beneficio esperado.

No caso do spring, para controller triviais, para trocar do spring para micronaut, seria baixa, pois o spring usar metadados(anotações),

Os frameworks mais modernos, como o spring, eles são poucos intrusivos.

Precisamos pensar no trade-off entre o acoplamento e o beneficio que o framework vai trazer.

frameworks modernos, como o spring, não tem nem dependencias com o http, se precisamos trocar de metodo com o GRPC, precisamos somente fazer o ajuste, mas o serviço continua o mesmo.


## Aula 15 - dtos com metodos

quero ter uma um arquivo aonde quero adicionar variaveis, aonde essa variaveis eram acessadas por metodos.


um padrão para instanciar um objeto especifico, toModel, toEntity, toDto.


## Aula 16 - preciso mesmo de um monte de camadas

É muito dificil isolar o nucleo/core, no caso do JPA um acoplamento muito alto com as entidades de dominio, se fossemos seguir a riscar o DDD, teriamos que criar uma classe separada para entidade de dominio, e uma classe separada para o JPA.

ficou nitido que você receber dados da requisição que não são dados do dominio, por isso para controladores é interessante ter um DTO, e não usar o dominio diretamente.

verificar se é realmente necessário implementar essas camadas.

Aumenta a quantidade de indireções.

## Aula 17 - Acoplamento Mental

quando você criar metodos que são utilizados, mas que possuir um acoplamento mental, por causa de uma implentação de uma biblioteca.

no caso abaixo, dadosBusca, ja considerar que a informação é uma string separada por espaço

podemos ajustar o metodo para ter um demilitador e ficar mais claro o que estamos fazendo.

O acomplamento mental é mais sutil, ele pode ser mais complicado de identificar, pois não algo que está claro no código, mas sim algo que está na cabeça do desenvolvedor.



``` java

class DadosBusca {

	public Optinal<String> getMetaInformacoes() {
		if(this.metaInformacoes.size() > 0){
			return Optional.of(String.join(",", this.metaInformacoes));
		}
		return Optional.empty();
	}
}

class Busca {

	public SearchPage<Documento> buscar(DadosBusca dadosBusca) {
		dadosBusca.getMetaInformacoes().ifPresent(meta -> {
			qb.must(QueryBuilders.matchQuery("metaInformacoes", meta));
		});
	}
}
```

## Aula 18 - abrace o legado


Legado de conhecimento, no lagado vão haver decisões que foram tomadas, e que não são mais válidas, mas que foram tomadas por um motivo.

aprender a tratar código lagado, pode fornerce a habilidade de entender o código, e melhorar a qualidade do código.

aprendar a lidar com o legado.




