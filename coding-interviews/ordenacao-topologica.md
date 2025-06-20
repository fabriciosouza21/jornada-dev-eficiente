# Ordenação topológica

quando existe uma dependência entre os nós.

1. buscar as tarefas que não tem dependência.
2. remover as tarefas que não tem dependência.
3. repetir até que não existam mais tarefas.

- coloca nos sem dependência em uma fila

é importante sempre conversar peguntas como, "existe uma ordem entre os nós? ou podemos fazer em qualquer ordem?"

pode não ter solução, não tem solução quando o grafo tem um ciclo, ou seja, quando existe uma dependência circular entre os nós.

no algoritmos podemos pensar que quando não temos mais nós sem dependência, mas ainda existe nós no grafo.

da uma olhada nesse caso, não consiguir entende muito bem o problema que o mauricio resolveu.


## Find All possible Recipes from Given supplies (2115)

ordem de problemas

1. ordenar as receitas via ordem topológica.
2. para cada receita em ordem
