# Busca em largura e profundidade

> Tente usar a busca em largura geralmente quando você quer explorar todos os níveis antes de aprofundar.

## Busca em largura

Olhar nível a nível, primeiro os filhos, depois os netos, etc.

Podemos criar uma árvore como uma fila, onde o primeiro elemento é o nó raiz e os filhos são adicionados ao final da fila.

### Pseudocódigo para busca em largura (BFS)
```
1. Crie uma fila vazia
2. Adicione o nó raiz à fila
3. Enquanto a fila não estiver vazia:
    a. Remova o primeiro nó da fila (chame de nó atual)
    b. Visite/processa o nó atual
    c. Adicione todos os filhos do nó atual ao final da fila
```
## Busca em profundidade

Vai o mais fundo possível, depois volta e visita os irmãos.

### Pseudocódigo para busca em profundidade (DFS)

```
1. Crie uma pilha vazia
2. Adicione o nó raiz à pilha
3. Enquanto a pilha não estiver vazia:
    a. Remova o nó do topo da pilha (chame de nó atual)
    b. Visite/processa o nó atual
    c. Adicione todos os filhos do nó atual à pilha (pode adicionar da direita para a esquerda para visitar da esquerda para a direita)

``` 
