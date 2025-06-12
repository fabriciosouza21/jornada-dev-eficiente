# Qualidade

## Reúso de código

- A reutilização de código é uma das coisa mais difíceis de acerta
  - Compartilha muit pouco significa repetição e bugs
  - Compartilhar demais significa muito acoplamento
- Crie biblioteca/frameworks para resolver problemas comuns, eles são mais independentes, tratar somente como um modulo separado pode gerar muitos problemas com coesão e acoplamento.
- Quem é o dono.

Aumenta o quantidade de dependencias entre os módulos, no caso de um módulos entre aplicações.

Sam Newman - livro de microserviços

Não sai reutilizando código, sem planejamento

## Dívida técnica

- Todo mundo tem, você não é diferente
- Você não pode tomar as melhores decisões quando sabe muito pouco sobre o problema
- vai atrsá-lo no logo prazo
- Problemas comuns
  - Modularização ruim
  - Teste ruins
  - Sem documentação
  - sem proprieadade clara
  - Tecnologias antigas
  - Tecnologias antiga
  - Tecnologia interna
- Lute contra o TD(Technical Debt) como parte do trabalho
- Evite "refatoração big bang", mas tenha cuidado ao focar apenas nas coisa fáceis.

é simplemente natural que haja dívida técnica, o importante é como você lida com ela.

a dívida técnica te torna mais lento para produzir código

planos de teste e refatoração são essenciais para lidar com a dívida técnica

## Testando

**Teste rapidos** como prioridade.


- Esqueça os "Teste de unidade x integração"
- Dividir para conquista é o único camianho a seguir
- As suítes de teste devem fornece sinal e não ruidos (Flaky tests)
- Teste de contrato
