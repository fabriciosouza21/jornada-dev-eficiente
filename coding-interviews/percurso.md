## Percursos em Árvores Binárias

## Resumo

Percursos em árvores binárias são métodos para visitar todos os nós de uma árvore em uma ordem específica. Os três principais tipos são:

- **Pré-ordem (pre-order):** Visita o nó atual antes dos filhos. Útil para copiar ou serializar a árvore.
- **Pós-ordem (post-order):** Visita o nó atual após os filhos. Usado para excluir a árvore, liberando memória dos filhos antes do pai.
- **Em ordem (in-order):** Visita o nó esquerdo, depois o atual e, por fim, o direito. Em árvores binárias de busca, resulta em uma lista ordenada dos valores.

Cada percurso tem aplicações práticas diferentes e facilita operações como listagem, cópia ou remoção de árvores.

---

### Pseudocódigo para percurso em pré-ordem (pre-order)

Função preOrder(nó):
    se nó for nulo:
        retorne
    visite/processa o nó
    preOrder(nó.esquerda)
    preOrder(nó.direita)

### Pseudocódigo para percurso em pós-ordem (post-order)

Função postOrder(nó):
    se nó for nulo:
        retorne
    postOrder(nó.esquerda)
    postOrder(nó.direita)
    visite/processa o nó

### Pseudocódigo para percurso em ordem (in-order)

Função inOrder(nó):
    se nó for nulo:
        retorne
    inOrder(nó.esquerda)
    visite/processa o nó
    inOrder(nó.direita)

---

### Exemplos de execução

Considere a seguinte árvore binária balanceada, onde os valores menores estão à esquerda e os maiores à direita:

```
      4
     / \
    2   5
   / \
  1   3
```

---

## Exemplo de execução do percurso em pré-ordem

Execução de `preOrder(4)`:

1. Visita 4
2. Vai para a esquerda: preOrder(2)
    - Visita 2
    - Vai para a esquerda: preOrder(1)
        - Visita 1
        - Esquerda nula → retorna
        - Direita nula → retorna
    - Vai para a direita: preOrder(3)
        - Visita 3
        - Esquerda nula → retorna
        - Direita nula → retorna
3. Vai para a direita: preOrder(5)
    - Visita 5
    - Esquerda nula → retorna
    - Direita nula → retorna

**Ordem de visita:**
4, 2, 1, 3, 5

**Caso de uso:**
Copiar uma árvore ou serializar sua estrutura.

A ordem de visita permite reconstruir a árvore a partir dessa sequência.

---

## Exemplo de execução do percurso em pós-ordem

Execução de `postOrder(4)`:

1. Vai para a esquerda: postOrder(2)
    - Vai para a esquerda: postOrder(1)
        - Esquerda nula → retorna
        - Direita nula → retorna
        - Visita 1
    - Vai para a direita: postOrder(3)
        - Esquerda nula → retorna
        - Direita nula → retorna
        - Visita 3
    - Visita 2
2. Vai para a direita: postOrder(5)
    - Esquerda nula → retorna
    - Direita nula → retorna
    - Visita 5
3. Visita 4

**Ordem de visita:**
1, 3, 2, 5, 4

**Caso de uso:**
Excluir uma árvore, liberando memória dos filhos antes do pai.

A ordem garante que todos os filhos sejam processados antes do nó pai.


---

## Exemplo de execução do percurso em ordem (in-order)

Execução de `inOrder(4)`:

1. Vai para a esquerda: inOrder(2)
    - Vai para a esquerda: inOrder(1)
        - Esquerda nula → retorna
        - Visita 1
        - Direita nula → retorna
    - Visita 2
    - Vai para a direita: inOrder(3)
        - Esquerda nula → retorna
        - Visita 3
        - Direita nula → retorna
2. Visita 4
3. Vai para a direita: inOrder(5)
    - Esquerda nula → retorna
    - Visita 5
    - Direita nula → retorna

**Ordem de visita:**
1, 2, 3, 4, 5

**Caso de uso:**
Listar os valores em ordem crescente em uma árvore binária de busca (BST).

A ordem de visita resulta em uma lista ordenada dos valores da árvore.

