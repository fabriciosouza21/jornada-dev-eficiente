# Array

## towsum 1

1. opção é força bruta
2. utilizar um map para armazena valor como chave e o índice como valor

## trhee sum 15

### dois ponteiros

- Ordenar o array(O(n log n))
- Percorrer o array com dois ponteiros, um no início e outro no final
- fazemos a soma dos valores
- verificamos se o valor é igual a zero, se for, adicionamos os valores no resultado
- se não for igual a zero, verificamos se é maior ou menor que zero
- se for maior que zero, movemos o ponteiro do final para a esquerda
- se for menor que zero, movemos o ponteiro do início para a direita
- repetimos até que os ponteiros se cruzem
- fazemos isso para cada elemento do array, exceto o último, pois já foi verificado.


## prefix sum 724

O Prefix Sum é uma técnica de otimização que transforma cálculos repetitivos em consultas instantâneas através do pré-processamento de dados. A ideia central é criar um "histórico cumulativo" onde cada posição contém informações suficientes para reconstruir qualquer intervalo anterior de forma eficiente.

- criar dois arrays, direita e esquerda
- adicionar nos arrays as somas acumulativas


## Sliding Window – 03

### Intuição
A intuição do *sliding window* é que vamos manter uma janela de elementos até encontrar um elemento que não atende à condição.

### Exemplo
Para a string `abcabcbb`:

1. Dois ponteiros (`L` e `R`) iniciam no mesmo ponto.
2. Vamos **abrindo a janela** (movendo o ponteiro da direita `R`) até encontrar um elemento que não atende à condição.
3. Quando encontramos um elemento que viola a condição:
   - **Removemos** o elemento em `L` (movendo `L` para a direita).
   - **Adicionamos** o novo elemento em `R` somente depois que a condição for restaurada.
4. Repetimos o processo até que `R` chegue ao final do array/string.

> Temos que **pular** (avançar `L`) até que o elemento em `R` possa entrar sem violar a condição.

### Análise de Complexidade
- Cada elemento entra e sai da janela **no máximo uma vez** → **O(n)**.



