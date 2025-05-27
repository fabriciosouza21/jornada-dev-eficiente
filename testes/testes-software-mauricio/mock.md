**27-05-25**

# Por que Mocks?

Mocks funcionam como facilitadores na criação de testes. Eles são importantes quando temos algum serviço externo que não podemos controlar, como uma API de terceiros. Permitem simular o comportamento desse serviço, evitando dependências externas e tornando os testes mais rápidos e confiáveis.

# O que Mockar e o que Não Mockar

## Quando mockar:
1. **Componentes externos** com pouco controle (APIs de terceiros, serviços externos)
2. **Banco de dados** quando a funcionalidade testada não depende diretamente dele

## Quando não mockar:
1. **Banco de dados** quando a funcionalidade depende diretamente dele - você precisa garantir que a lógica de persistência está funcionando corretamente ou que a query está retornando os dados esperados
2. **Entidades, classes de domínio, DTOs e classes utilitárias** - estes fazem parte da lógica central

**Regra geral:** Mockamos aquilo que dá trabalho. Esta é a máxima que devemos seguir.

# Internals vs Peers

**Peers** são objetos que são colegas - dependências externas. Geralmente queremos mockar peers, pois eles são dependências que não queremos testar diretamente.

**Internals** são dependências que fazem parte do mesmo contexto. Geralmente não mockamos internals, pois queremos garantir que a interação entre eles está funcionando corretamente, já que integram como partes do mesmo sistema.

# Fakes, Stubs e Mocks

**Stubs** retornam valores fixos. Não têm implementação, apenas retornam valores pré-definidos. São úteis para simular comportamentos simples.

**Fakes** são versões mais simples, como por exemplo um banco de dados em memória. Eles têm uma implementação real, mas são simplificados para facilitar os testes.

**Mocks** permitem fazer asserções sobre as interações que ocorreram naquele objeto. São stubs com capacidade de verificação. Eles permitem verificar se métodos foram chamados, quantas vezes foram chamados, etc. São úteis para verificar interações entre objetos.

# Problemas com Mocks

## Acoplamento com Código de Produção
O código de teste tende a saber muito sobre o código de produção. Ao fazer um mock de repository, você acaba tendo que conhecer o retorno de métodos específicos, criando uma dependência forte entre teste e implementação.

## Desconexão do Componente Real
Os mocks se desconectam do componente real. Se você alterar o comportamento de um método, pode ser que seu teste não quebre, porque você está mockando o comportamento.

Isso fica mais evidente em APIs externas: você pode mockar o retorno de uma chamada, mas se a API mudar, seu teste não vai quebrar, mesmo que o comportamento esperado tenha mudado. Isso cria uma falsa sensação de segurança.
