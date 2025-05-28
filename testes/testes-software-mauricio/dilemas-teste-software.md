**28-05-25**

# Como evitar Flaky Tests

**Flaky tests** são testes intermitentes: às vezes passam, às vezes falham.

"An Empirical Analysis of Flaky Tests" tenta entender as principais causas de flaky tests:

- **Concorrência**: nesse cenário é mais complicado de controlar. É um tipo de teste flaky que você pode aceitar dependendo do contexto.
- **Async/await**: é um teste que depende de uma resposta assíncrona e pode falhar se a resposta não chegar no tempo esperado. Testes end-to-end tendem a ser mais flaky, pois dependem de várias partes do sistema funcionando corretamente.
- **Dependência entre testes**: testes que não seguem a coesão, independência e isolamento têm mais chances de serem flaky, pois dependem de outros testes para funcionar corretamente.

Uma ótima maneira de evitar flaky tests é "limpar a bagunça que você fez" – uma forma eficaz de evitar o problema. Restaure o estado após a execução do teste, garantindo que o próximo teste comece com um estado limpo.

Muitas equipes separam a suíte de testes flaky da suíte de testes não flaky, para que os testes flaky não atrapalhem o desenvolvimento. Muitas equipes também usam a estratégia de "cemitério de testes".

Escrever código simples e direto é uma forma de evitar flaky tests. Tente sempre escrever testes simples: "dividir para conquistar".

# Devs e QAs

O QA pode ter um papel importante no sentido de validação. O QA pode ter uma visão mais ampla, focando nas regras de negócio e ajudando a criar cenários de testes que são mais relevantes para o negócio.

# Testando microserviços

De modo geral, os microserviços devem ser testados de forma isolada. Os testes com mocks conseguem escalar melhor. Para testar sem mocks é necessário ter uma infraestrutura de testes que suporte a execução de vários serviços ao mesmo tempo, o que pode ser mais complexo e custoso, além de garantir que os testes não estão influenciando uns aos outros.

É preciso ter testes de integração em menor quantidade quando comparado com testes unitários, mas eles são importantes para garantir que os serviços estão se comunicando corretamente e que a lógica de negócio está funcionando como esperado.

O principal problema ao trabalhar com mocks são os contratos da API. Muitos sistemas começam como monólitos e depois são quebrados em microserviços. Nesse caso, é importante mudar a mentalidade de testes para que os testes se adaptem à nova arquitetura.

# Testes em sistemas legados

Um sistema legado é um sistema sem testes, sem "design de testes".

No caso de sistemas legados, crie ferramentas para testar o legado. Crie bibliotecas que vão ajudar a fazer o setup ou mesmo criar cenários de testes.

Um bom caminho pode ser avaliar esses casos:

1. Como colocar o sistema no estado que você precisa
2. Como executar o caso de uso de maneira fácil
3. Como observar o comportamento do sistema

# "Não tenho tempo de testar"

Hoje fazer testes é mais fácil. É um investimento que se paga muito rapidamente. Detectar erros o mais cedo possível faz você economizar tempo e dinheiro. Testes automatizados são facilmente replicáveis.
