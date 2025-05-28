# O que é um bom código de teste

## Checklist de Boas Práticas para Testes

- [ ] **Coesão, independência e isolamento** - Os testes devem ser coesos, independentes e isolados
- [ ] **Propósito claro** - O teste deve ter uma razão para existir
- [ ] **Repetibilidade** - O teste deve ser repetitível e não pode ser flaky (flaky é quando o teste falha de forma intermitente, sem uma razão aparente). Os testes flaky fazem o desenvolvedor perder a confiança no teste
- [ ] **Sensibilidade a mudanças** - O teste deve quebrar se o sistema mudar. Quando a regra de negócio mudar, o teste deve quebrar
- [ ] **Falha clara** - Deve haver uma razão clara para a falha. O motivo da falha deve ser evidente
- [ ] **Legibilidade** - O teste deve ser fácil de ler e entender
- [ ] **Manutenibilidade** - O teste deve ser fácil de mudar e evoluir. O sistema evolui, e o teste deve evoluir junto

## Teste sem asserção

Testes sem asserção não testam nada. Se está difícil fazer uma asserção, você deve pensar nos conceitos de observabilidade e controlabilidade.

## Testes podem dificultar a refatoração

Observe o quão acoplados os testes estão com a implementação. Se você perceber que os testes estão sendo quebrados a cada mudança pequena, você pode diminuir a granularidade e fazer testes que testem uma funcionalidade ao invés de uma unidade específica. Assim, a única mudança será na interface que o teste consome, e não na implementação interna.

## Testes "estúpidos"

Os testes que parecem "estúpidos" podem ser bons, pois eles são simples e garantem a repetibilidade. É fácil replicar um "teste estúpido".

Um teste pode ser considerado ruim quando duplica o código de produção. Códigos que tentam simular o comportamento do código de produção, com constantes e valores fixos, são ruins de verdade. Ao alterar o código, você também altera o teste, e isso é um indicador negativo. Não no sentido de fazer um ajuste, mas sim de alterar o comportamento do teste.
