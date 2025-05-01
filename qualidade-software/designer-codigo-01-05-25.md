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






