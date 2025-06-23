# Lista ligada

## Middle of the linked list

o modo mais simples é implementa um metodo que calcula o tamanho da lista ligada e uma função que dado o index retorna o no.


### Fast and Slow Pointers em Linked List

- **Inicialização**
  Ambos os ponteiros (`slow` e `fast`) começam referenciando o primeiro nó (`head`) da lista.

- **Velocidades diferentes**
  Em cada iteração:
  - `slow` avança **1 nó** (`slow = slow.next`).
  - `fast` avança **2 nós** (`fast = fast.next.next`).

- **Condição de término**
  O loop continua enquanto `fast != null` **e** `fast.next != null`.

- **Posição do ponteiro lento**
  - Para **lista de tamanho ímpar**, `slow` aponta exatamente para o nó do meio.
  - Para **lista de tamanho par**, `slow` aponta para o **segundo** dos dois nós centrais (por convenção).

- **Casos de uso comuns**
  - Encontrar o nó médio.
  - Detectar ciclos (quando `slow == fast`).
  - Dividir a lista em duas metades de tamanho quase igual.


## Reverse Linked List (LeetCode 206)

### 1. Solução “Intuitiva” com Array
- **Ideia:**
  1. Percorra a lista e armazene cada nó (ou seu valor) em um array.
  2. Percorra o array de trás para frente, criando uma nova lista ligada ou relinkando nós existentes.
- **Pontos de atenção:**
  - Usa O(n) de espaço extra.
  - Fácil de implementar, mas não é “in-place”.
  - Se recriar nós, cuidado com vazamento de memória ou referências antigas.

### 2. Solução “In-Place” com Dois Ponteiros
- **Ideia:** reverter as setas sem array extra, em O(1) de espaço adicional.
- **Passos:**
  1. Inicialize `prev = null` e `curr = head`.
  2. Enquanto `curr != null`:
     - Armazene `nextNode = curr.next`.
     - Faça `curr.next = prev` (inverte a seta).
     - Atualize `prev = curr`.
     - Atualize `curr = nextNode`.
  3. Ao final, `prev` é o novo head da lista invertida.
- **Complexidade:**
  - Tempo: O(n) (cada nó é visitado exatamente uma vez).
  - Espaço: O(1) (apenas variáveis auxiliares).

> **Dica:** inicializar `prev = null` garante que o antigo primeiro nó aponte para `null` e que, quando `curr` chegar a `null`, `prev` seja o head da lista invertida.


## Intersection of two linked list 160

### 1. Força Bruta
- **Descrição:** Para cada nó em **List A**, percorra **List B** inteiro e compare referências.
- **Complexidade:**
  - Tempo: O(m·n) (m = tamanho de A, n = tamanho de B)
  - Espaço: O(1)
- **Pontos de Atenção:**
  - Muito ineficiente para listas grandes.
  - Fácil de implementar, mas geralmente impraticável em entrevistas.

---

### 2. `HashSet`
- **Descrição:**
  1. Percorra **List A** e guarde cada nó (a referência, não o valor) em um `HashSet`.
  2. Percorra **List B** e, para cada nó, verifique se está no set.
- **Complexidade:**
  - Tempo: O(m + n)
  - Espaço: O(m)
- **Pontos de Atenção:**
  - Usa espaço extra proporcional ao tamanho da primeira lista.
  - Bom quando não há restrição de espaço, mas em entrevistas costuma esperar-se uma solução O(1) de espaço.

---

### 3. Dois Ponteiros com Alinhamento de Comprimento
- **Descrição:**
  1. Calcule `lenA` e `lenB`.
  2. Avance o ponteiro da lista maior em `|lenA − lenB|` nós, para igualar o “restante” de nós nas duas listas.
  3. Agora percorra **A** e **B** juntos, passo a passo, até encontrar a primeira referência igual (ou `null`).
- **Complexidade:**
  - Tempo: O(m + n)
  - Espaço: O(1)
- **Pontos de Atenção:**
  - É a **solução ideal**: linear e in-place.
  - Lembre-se de tratar corretamente o caso em que não há interseção (ambos chegarem a `null` sem coincidência).


## “Merge Two Sorted Lists” 21

### 1. Dois Ponteiros para Construir a Nova Lista
- ✔️ **Essência correta:** você mantém dois ponteiros (`p1` e `p2`) apontando para o início de cada lista e sempre compara `p1.val` vs `p2.val`.
- ⚠️ **Detalhe:** além de `p1` e `p2`, normalmente usamos um ponteiro auxiliar `tail` (inicializado no sentinela) para anexar o próximo nó ordenado.

### 2. Complexidade
- ⏱ **Tempo:** O(n + m), pois cada lista é percorrida no máximo uma vez.
- 📦 **Espaço:**
  - **Criando nova lista:** O(n + m) para novos nós.
  - **In-place (reaproveitando nós):** O(1), pois você só ajusta ponteiros.

### 3. Estratégia do Nó Sentinela
- ✔️ Um “dummy” (ou sentinela) simplifica o código porque você nunca precisa tratar o caso especial de “lista vazia” ao iniciar.
- 📌 **Padrão:**
  ```java
  ListNode dummy = new ListNode(0);
  ListNode tail  = dummy;
  while (p1 != null && p2 != null) {
      if (p1.val <= p2.val) {
          tail.next = p1;
          p1 = p1.next;
      } else {
          tail.next = p2;
          p2 = p2.next;
      }
      tail = tail.next;
  }
  // Anexa o restante
  tail.next = (p1 != null) ? p1 : p2;
  return dummy.next;

  ```


## remove nth node from end of list 19

## reorde listed list 143

