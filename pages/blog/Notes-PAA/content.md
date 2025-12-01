# Métodos para Resolver Recorrências

## Teorema Mestre

---


### $$T(n) = aT(n/b) + f(n)$$

$$
\begin{cases}
T(n)\in\Theta\bigl(n^{\log_b a}\bigr) & \mid n^{\log_b a} > f(n),\\[4pt]
T(n)\in\Theta\bigl(n^{\log_b a}\log n\bigr) & \mid n^{\log_b a} = f(n),\\[4pt]
T(n)\in\Theta\bigl(f(n)\bigr) & \mid n^{\log_b a} < f(n).
\end{cases}
$$

### $$T(n) = aT(n/b) + f(n)$$

$$
\begin{cases}
T(n)\in\Theta\bigl(n^{\log_b a}\bigr) & \mid f(n) = O(n^{\log_b a - \epsilon}), \text{ para } \epsilon > 0,\\[6pt]
T(n)\in\Theta\bigl(n^{\log_b a}\log^{k+1} n\bigr) & \mid f(n) = \Theta(n^{\log_b a}\log^k n), \text{ para } k \geq 0,\\[6pt]
T(n)\in\Theta\bigl(f(n)\bigr) & \mid f(n) = \Omega(n^{\log_b a + \epsilon}), \text{ para } \epsilon > 0 \text{ e } af(n/b) \leq cf(n).
\end{cases}
$$


### $$T(n) = aT(n/b) + n^k$$

$$
\begin{cases}
T(n)\in\Theta\bigl(n^{\log_b a}\bigr) & \mid a > b^k,\\[4pt]
T(n)\in\Theta\bigl(n^{\log_b a}\log n\bigr) & \mid a = b^k,\\[4pt]
T(n)\in\Theta\bigl(f(n)\bigr) & \mid a < b^k.
\end{cases}
$$

## Árvores de recursão

---

### $$T(n) = 2T(n/2) + n$$

#### tabela inferida pela análise da árvore

| nível      | tamanho      | num. de nós      | tempo por nó |
|------------|--------------|------------------|------------------|
| 0          | $$n$$        | $$1$$            | $$n$$            |
| 1          | $$n/2$$      | $$2$$            | $$n/2$$          |
| 2          | $$n/4$$      | $$4$$            | $$n/4$$          |
| i          | $$n/2^i$$    | $$2^i$$          | $$n/2^i$$        |
| ?          | $$1$$        | $$2^?$$          | $$1$$            |

---

| nível      | tamanho      | num. de nós       | tempo por nó |
|------------|--------------|-------------------|------------------|
| 0          | $$n$$        | $$1$$             | $$n$$            |
| 1          | $$n/2$$      | $$2$$             | $$n/2$$          |
| 2          | $$n/4$$      | $$4$$             | $$n/4$$          |
| i          | $$n/2^i$$    | $$2^i$$           | $$n/2^i$$        |
| $$log_2n$$ | $$1$$        | $$2^{log_2n} = n$$| $$1$$            |

#### tempo total da árvore

$$ T(n) = \sum_{i=0}^{\text{número de níveis}} (\text{Tempo do Nível } i) * \text{número de nós do nível } i$$

$$ T(n) = \sum_{i=0}^{\log_2 n} \frac{n}{2^i} * 2^i = n(log_2n + 1) = nlog_2n + n = \Theta(nlog_2n)$$

---

## Substituição

### **Exemplo com Mergesort:** $$T(n) = 2T(n/2) + n$$

- **Passo 1: Chute**
  * $$O(n \lg n)$$.
  * *Hipótese Indutiva:* Assumimos que $$T(k) \le c \cdot k \lg k$$ para todo $$k < n$$.

- **Passo 2: Substituição**
  * Substitua $$T(n/2)$$ pela sua hipótese.
  * $$T(n) = 2 \cdot [\text{HIPÓTESE AQUI}] + n$$
  * $$T(n) = 2 \cdot [c (n/2) \lg(n/2)] + n$$

- **Passo 3: A Álgebra (Onde a mágica acontece)**
  * Expanda a matemática. Lembre-se que $$\lg(a/b) = \lg a - \lg b$$.
  * $$T(n) = c \cdot n (\lg n - \lg 2) + n$$
  * $$T(n) = c \cdot n \lg n - c \cdot n \cdot 1 + n$$  *(Note que $$\lg 2 = 1$$)*
  * $$T(n) = c \cdot n \lg n - cn + n$$
  * $$T(n) = c \cdot n \lg n - n(c - 1)$$

- **Passo 4: Conclusão**
  * Queremos provar que $$T(n) \le c \cdot n \lg n$$.
  * Olhe para a última linha: $$T(n) = \underbrace{c \cdot n \lg n}_{\text{O que queremos}} - \underbrace{n(c - 1)}_{\text{Resto}}$$.
  * Para que isso seja $$\le c \cdot n \lg n$$, o "resto" precisa ser subtraído (ou seja, positivo).
  * $$n(c - 1) \ge 0 \implies c \ge 1$$

**Fim da prova.**

# Divisão e Conquista - Algoritmos

O paradigma segue três passos:
1.  **Divisão:** Particionar o problema em subproblemas menores.
2.  **Conquista:** Resolver os subproblemas recursivamente.
3.  **Combinação:** Juntar as soluções dos subproblemas na solução original.

## 1. Mergesort (Clássico / 2-vias)

### Recorrência
$$T(n) = 2T(n/2) + n$$

### Explicação
O algoritmo divide o vetor na metade, ordena recursivamente cada metade e depois intercala (**merge**) as duas metades ordenadas.

*   **Divisão:** $$n \to n/2 \mid b=2$$.
*   **Ramificação:** 2 chamadas recursivas ($$a=2$$).
*   **Combinação:** Para intercalar 2 listas, fazemos **1 comparação** para decidir o menor elemento. Custo: $$1n$$.

### Complexidade (Teorema Mestre)
*   Comparamos $$n^{\log_2 2} = n^1$$ com $$f(n) = n$$.
*   **Empate (Caso 2):** $$\Theta(n \log n)$$.

---

## 2. Mergesort (Variação / 3-vias)

### Recorrência
$$T(n) = 3T(n/3) + n$$

O algoritmo divide o vetor em três partes, ordena cada uma e intercala as três.

*   **Divisão:** $$n \to n/3 \mid b=3$$.
*   **Ramificação:** 3 chamadas recursivas ($$a=3$$).
*   **Combinação:** Para intercalar 3 listas, precisamos descobrir o mínimo entre 3 elementos ($$x, y, z$$). Isso exige **2 comparações**. Custo: $$2n$$ e $$\Theta(2n) = \Theta(n)$$ por isso na recorrência não é mostrado esse tradeoff.

### Complexidade (Teorema Mestre)
*   Comparamos $$n^{\log_3 3} = n^1$$ com $$f(n) = n$$.
*   **Empate (Caso 2):** $$\Theta(n \log n)$$.

> **Nota:** Para o Teorema Mestre, $$n$$ e $$2n$$ pertencem à mesma classe de complexidade (Linear), por isso o resultado assintótico é o mesmo.

---

## 3. Trade-off: Por que o 3-vias é "pior"?

Apesar de ambos serem $$\Theta(n \log n)$$, o Mergesort de 3 partes é geralmente mais lento na prática. Isso acontece devido às constantes ocultas na notação assintótica.

### Análise das Recorrências "Honestas" (Contando comparações)

| Algoritmo----- | Recorrência Exata----------- | Altura da Árvore----- | Custo por Nível |
| :--- | :--- | :--- | :--- |
| **2-vias** | $$T(n) = 2T(n/2) + \mathbf{1}n$$ | $$\log_2 n$$ | $$1n$$ |
| **3-vias** | $$T(n) = 3T(n/3) + \mathbf{2}n$$ | $$\log_3 n$$ | $$2n$$ |

### A Batalha Matemática
1.  **Vantagem do 3-vias:** A árvore fica mais baixa.
    *   $$\log_3 n \approx 0.63 \cdot \log_2 n$$ (Redução de ~37% na altura).
2.  **Desvantagem do 3-vias:** O custo de combinação dobra.
    *   O trabalho por nível passa de $$1n$$ para $$2n$$ (Aumento de 100% no custo).

**Conclusão:** O esforço extra na combinação (dobro de comparações) supera o ganho obtido pela redução da altura da árvore.
*   **Assintoticamente:** Iguais ($$\Theta$$).
*   **Desempenho Real:** O clássico (2-vias) é mais eficiente.

# Ordenação Baseada em Estruturas de Dados

## Heapsort
[...]

# notas

## O Teorema Mestre é um atalho para a **Árvore de Recursão**

O Teorema Mestre é um atalho para a **Árvore de Recursão** resumindo o problema a uma batalha de pesos entre o **Custo da Raiz** ($$f(n)$$) e o **Custo das Folhas** ($$n^{\log_b a}$$).

### Custo das Folhas = $$n^{\log_b a}$$?
Sim, que é o número de folhas e cada folha ter custo 1 por definição. Número de folhas no nível ($$i$$) é ($$a^i$$).

No caso, $$T(n) = 4T(n/3) + nlogn$$:

$$
i = log_3n \text{ ,pois}\\
1 = n/3^i, \text{i = ?}\\
3^i = n\\
log3^i = log n\\
i = \frac{logn}{log3} = log_3n
$$

Com isso achamos o valor de $$?$$ que no exemplo da tabela era $$log_2n$$.

Então o número de nós no nível folha em ($$T(n) = 4T(n/3) + nlogn$$) é igual a $$4^{log_3n}$$, e usando a propriedade do log, trocamos $$4$$ e $$n$$, ficando $$n^{log_34}$$ que é $$n^{log_ba}$$

### Insights dessa Análise (A Geometria do Custo)

#### 1. A Regra do $$a$$ vs $$b$$ (Quem domina a soma?)
A relação entre a taxa de ramificação ($$a$$) e a taxa de divisão ($$b$$) define o formato do custo:

1.  **Topo Pesado / Decaimento ($$a < b^k$$)**
    *   O custo **diminui** descendo a árvore. A Raiz domina.
    *   Resposta: $$\Theta(f(n))$$.
2.  **Retângulo / Equilíbrio ($$a = b^k$$)**
    *   O custo é **constante** em todos os níveis. Empate.
    *   Resposta: $$\Theta(f(n) \cdot \log n)$$.
3.  **Fundo Pesado / Explosão ($$a > b^k$$)**
    *   O custo **aumenta** descendo a árvore. As Folhas dominam.
    *   Resposta: $$\Theta(\text{Folhas}) = \Theta(n^{\log_b a})$$.