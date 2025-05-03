# Cognitive Driven Development (CDD)

*Última atualização: 03/05/2025*

## Introdução

Cognitive Driven Development (CDD) é uma prática que promove o entendimento do código e o controle de sua complexidade cognitiva. Esta abordagem visa facilitar a manutenção e desenvolvimento de software através de métricas que medem o esforço mental necessário para compreender o código.

## Ferramentas e Recursos

### Complexity Tracker

Para demonstrar o aumento de complexidade no código, é possível utilizar a ferramenta [Complexity Tracker](https://github.com/asouza/complexity-tracker) desenvolvida por Alberto Souza.

Esta ferramenta monitora métricas importantes como:
- **LCOM** - Falta de coesão
- **WMC** - Complexidade ciclomática
- **CBO** - Acoplamento entre classes

É recomendado consultar estudos sobre métricas de complexidade de código para aprofundamento no tema.

## Experimentos e Evidências

### Caso Prático na Zup

Um experimento foi realizado na empresa Zup, onde:
- Metade da equipe refatorou código usando técnicas tradicionais
- Metade da equipe refatorou usando métricas derivadas do CDD

A classe de teste foi a `JournalEntryServiceImpl.java`

### Resultados Comparativos

#### Métricas Convencionais Analisadas
- CBO (Acoplamento entre Classes)
- WMC (Complexidade Ciclomática)
- RFC (Número de Métodos Públicos)
- LCOM (Falta de Coesão)
- LOC (Linhas de Código)

#### Resultados
- **CBO**: Resultados similares entre as abordagens
- **WMC**: CDD apresentou melhor resultado e menor variação
- **RFC**: CDD apresentou melhor resultado e menor variação
- **LCOM**: CDD apresentou melhor resultado e menor variação
- **LOC**: CDD mostrou resultados promissores

## Métricas do CDD

### Métrica Base (Alberto Souza)

O CDD atribui pontos de complexidade cognitiva:
- **1 ponto** para cada acoplamento com classe específica do projeto
- **1 ponto** para cada branch de código (if, else, ternário, loop, switch, case, try, catch)
- **1 ponto** para cada função passada como argumento (filter, map, reduce, Consumer)

### Critérios de Pontuação

- **Classes com atributos de dependência**: limite máximo de 7 pontos
- **Classes com atributos de estado**: limite máximo de 9 pontos

## Adaptação para Diferentes Cenários

### Métricas para Padrão de Indústria

Considerando dependências como pontos:
- **1 ponto** para qualquer acoplamento
- **1 ponto** para cada branch
- **1 ponto** para função como argumento

**Critério de pontuação**:
- Classes com atributos de dependência: limite máximo de 7 pontos
- Classes com atributos de estado: limite máximo de 9 pontos

### Métricas para Equipes Trabalhando em Frameworks ou Bibliotecas

Para equipes com maior senioridade, podemos dobrar os limites dos critérios de pontuação:

- **1 ponto** para cada acoplamento com classe específica do projeto
- **1 ponto** para cada branch de código
- **1 ponto** para função como argumento

**Critério de pontuação**:
- Classes com atributos de dependência: limite máximo de 14 pontos
- Classes com atributos de estado: limite máximo de 18 pontos

## Aplicação em Código Legado

Para código legado, Alberto Souza sugere a implementação de melhorias percentuais:
- Estabelecer metas de melhoria: 100%, 90%, 80%, 70%, 60%, 50%, 40%, 30%, 20%, 9%
- A cada modificação no código, aplicar a porcentagem de melhoria estabelecida

## Design Escalável e Sustentável

O CDD proporciona uma forma de controlar a complexidade do código no médio e longo prazo, promovendo um design mais escalável e sustentável.

## Design e Mudanças

Um princípio importante do CDD é que:

> O design do código deve acomodar mudanças de pessoas e escopo, e não mudanças de tecnologia.

A única exceção é quando a mudança de tecnologia representa uma mudança de escopo.

---

Este README fornece uma visão geral do Cognitive Driven Development (CDD) e como aplicá-lo para melhorar a qualidade do código. As métricas e critérios apresentados podem ser adaptados conforme as necessidades específicas de cada equipe e projeto.
