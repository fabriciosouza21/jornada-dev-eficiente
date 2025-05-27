**27-05-25**

# Teste de Unidade vs Integração

## Teste de Unidade

- Fácil de ser escrito, testado em nível de classe ou método
- Executam muito rapidamente
- Não são parecidos com o mundo real

## Teste de Integração
- Mais lento, testado em nível de sistema
- Mais difícil de ser escrito
- Mais parecido com o mundo real

## Feedback de Testes

Quando pensamos em testes, devemos considerar quais testes nos proporcionarão o feedback com maior qualidade.

## Escolha da Estratégia

**Para algoritmos complexos:** Pode valer a pena isolar a lógica e fazer testes unitários, pois permitem testar cenários específicos de forma isolada.

**Para backends convencionais (CRUDs):** Talvez obtenhamos um feedback melhor com testes de integração, pois os testes unitários refletirão apenas o fluxo individual, sem considerar as interações entre componentes que são essenciais neste contexto.

# Simplicidade e Rapidez

Testes simples proporcionam mais produtividade, pois quando você escreve um teste que gasta muita energia, acaba desanimando para escrever mais testes. Mas quando você escreve testes simples, você acaba escrevendo mais testes e obtendo mais feedback.

Os testes também devem ser rápidos, pois quanto mais demorados, menor a quantidade de vezes você irá executá-los, e menos feedback você terá.

# Mito: Bugs Somente com Testes de Integração

É uma falácia acreditar que somente testes de integração detectam bugs. Quem detecta o bug é o desenvolvedor. O nível de teste não é mágico - ele não acha bugs sozinho. O que ele faz é dar mais feedback e mais confiança de que o código está funcionando como deveria.

Por isso os testes unitários ganham mais valor: além de serem mais rápidos, eles tendem a ser mais simples. Você tem mais tempo para criar mais casos e, consequentemente, ter uma visão mais abrangente do que precisa ser feito.

# Dividir para Conquistar

Testar componentes de modo isolado, além de ser mais rápido, é mais simples. Utilize mocks para simular comportamentos e isolar as funcionalidades testadas.

# Infraestrutura de Testes

Às vezes é importante criar infraestrutura de testes, como por exemplo, criar um banco de dados em memória ou um servidor mockado para simular o comportamento de serviços externos. Isso permite que você teste a lógica do seu código sem depender de recursos externos, tornando os testes mais rápidos e confiáveis.

Além disso, permite o reaproveitamento de código entre diferentes testes.

A forma mais fácil de fazer a equipe construir testes é facilitando a sua construção através de uma boa infraestrutura.

# Valor em Todo Tipo de Teste

Todos os testes devem agregar valor. É importante avaliar se cada teste realmente contribui para a qualidade e confiabilidade do sistema, evitando testes desnecessários que apenas aumentam a complexidade sem benefícios tangíveis.
