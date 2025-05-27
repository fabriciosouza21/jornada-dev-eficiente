**27-05-25**

# Cobertura de Código

A cobertura de código é uma ferramenta que, assim como os testes de fronteira e os specification tests, fornece uma métrica para identificar quais partes do código estão mais ou menos cobertas por testes. É um bom indicador para determinar se você deve ou não escrever mais testes, gerando uma criticidade em relação aos pontos não cobertos.

## Artigos

**"Experiments on the effectiveness of dataflow- and controlflow-based test adequacy criteria"** - Demonstrou que 90% de cobertura de testes resultou em uma detecção eficaz de falhas.

**"The influence of size and coverage on Test suite effectiveness"** - Mostra uma relação direta entre a cobertura e a eficácia dos testes.

Testes que cobrem funcionalidades novas têm mais chances de encontrar novas falhas.

## Estratégia Recomendada

1. Primeiro, desenvolva testes baseados em especificações
2. Depois, foque na cobertura de código para identificar lacunas

## Considerações Importantes

- **Alta cobertura** pode não significar muito por si só
- **Baixa cobertura** definitivamente significa que há riscos significativos no código
