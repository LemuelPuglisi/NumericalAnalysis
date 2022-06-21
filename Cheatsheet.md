# Analisi Numerica - Cheatsheet

[TOC]

## Matrici

Concetti MUST: 

- Teorema di Sylvester
- norme vettoriali ed equivalenza
- norme matriciali indotte
- raggio spettrale
- trasformazione per contragradienza (matrici simili)
- matrice diagonalizzabile
- Teorema dei dischi di Gershgorin
- Teorema di Hermite
- Teorema di Shur





## Sistemi Lineari

Concetti MUST: 

- Quando esiste una soluzione?
- Teorema di Cramer
- Numero di condizionamento e relazione con raggio spettrale
- Descrizione generale di un algoritmo iterativo
- Decomposizione regolare
- Criteri d'arresto
- CNS per convergenza (norma matrice del metodo < 1)
- CNS per convergenza (raggio spettrale matrice del metodo < 1)
- CN per convergenza (determinante matrice del metodo  < 1)
- CN per convergenza (traccia matrice del metodo < n)
- Correlazione tra raggio spettrale e convergenza
- Calcolo del numero di iterazioni $K$ indicata la tolleranza

**Note sui metodi diretti**

| Metodo                          | Prerequisiti                                 | Note                                                         |
| ------------------------------- | -------------------------------------------- | ------------------------------------------------------------ |
| Metodo di eliminazione di Gauss | Minori principali con determinante non nullo | -                                                            |
| Fattorizzazione LU              | Matrice non degenere                         | Esiste sempre per matrici diagonalmente dominanti o simmetriche definite positive |
| Metodo di Cholesky              | Matrice simmetrica definita positiva         | Un'altra verifica, oltre a Sylvester, è matrice non degenere (quindi esiste LU) |
| Metodo di Thomas                | Matrice tridiagonale                         | -                                                            |

**Note sui metodi iterativi**

| Metodo                 | Prerequisiti         | particolare CS di convergenza                                |
| ---------------------- | -------------------- | ------------------------------------------------------------ |
| Metodo di Jacobi       | Matrice non degenere | Matrice strettamente diagonalmente dominante                 |
| Metodo di Gauss-Seidel | Matrice non degenere | Matrice strettamente diagonalmente dominante o simmetrica definita positiva |
| SOR                    | Matrice non degenere | Converge per $0<\omega<2$                                    |
| Discesa del gradiente  | Matrice non degenere | -                                                            |



## Interpolazione

- teorema di Vendermonde
- Metodo dei coefficienti indeterminati
- Metodo dei polinomi di Lagrange
- Metodo delle differenze divise
- Metodo delle differenze
- Teorema sull'errore Lagrangiano
- Interpolazione Hermitiana (caso particolare: osculatoria)
- Teorema sull'errore Hermitiano (estensione teo. precedente)



**Cheatsheet delle funzioni**

* [**Lagrange polynomials**] $L_i(x) = \prod_{j=0, j\ne i}^{n} \frac{x-x_j}{x_i -x_j}$
* [**Lagrange polynomials**] $p(x) = \sum_{i=0}^n y_i L_i(x)$
* [**Differenze divise**] $p(x) = b_0 + \sum_{i=1}^{n-1} b_i \prod_{j=0}^{i-1}(x-x_j)$
* [**Errore Lagrangiano**] $e(x) = \frac{f^{(n+1)}(\xi)}{(n+1)!}W(x)$
* [**Errore Lagrangiano**] $W(x) = \prod_{i=0}^n (x-x_i)$



**Disposizione dei nodi**

- teorema di Faber (non esiste triangolazione ottima generale)
- teorema di esistenza della triangolazione ottima (per specifica funzione)
- polinomi di Chebichev
- zeri dei polinomi di Chebichev
- Minimizzazione dell'errore con i polinomi di Chebichev



**Splines**

- Def. di polinomio a tratti
- Def di splines di grado k
- Splines di grado 1
- Splines di grado 3



## Minimi Quadrati

Data una funzione $f\in C[a,b]$ **problema discreto ai minimi quadrati** consiste nel trovare $p^* \in P_n$ 
$$
p^*(x) = \arg\min_{p \in P_n} \sum_{i=0}^m w_i [ p(x_i) - y_i ]^2
$$
Se i nodi e i valori sono $m > n+1$, allora si ha un sistema lineare sovradimensionato $Ax=b$. Un teorema dimostra che il polinomio $p^*$ si ottiene come soluzione al sistema $A^TAx=A^Tb$, che però soffre di malcondizionamento all'aumentare di $n$. 



Nel problema **continuo dei minimi quadrati** la $f$ è nota e si vuole costruire il miglior $p^*\in P_n$ (grado fissato ad $n$), ovvero:
$$
p^* = \arg\min_{p \in P_n}\int_a^b w(x) \left[p^*(x) - f(x)\right]^2dx
$$
Per trovare ciò si sfrutta il *teorema di Fourier delle equazioni normali*, che ci dice che il polinomio è rappresentabile come combinazione lineare delle componenti della base $\{\varphi_1, \dots, \varphi_n\}$, e che i coefficienti di tale combinazione lineare $a_1, \dots, a_n$ possono essere estratti da un sistema detto *sistema delle equazioni normali*: 
$$
\begin{cases}
\sum_{i=0}^n a_i \langle \varphi_i, \varphi_1 \rangle =
\langle f, \varphi_1 \rangle \\
\vdots \\
\sum_{i=0}^n a_i \langle \varphi_i, \varphi_n \rangle =
\langle f, \varphi_n \rangle \\
\end{cases}
$$
Tale sistema ha una matrice malcondizionata, e si dimostra che una matrice ortogonale / ortonormale risolva il problema. Si utilizza l'ortogonalizzazione di Gram-Schmidt per trasformare una base iniziale $p_i$ in una base ortonormale $q_i$, per $i=1, \dots, n$. Essa si costruisce ricorsivamente: 
$$
q_0(x)=1 \hspace{2cm}q_k(x) = p_k - \sum_{i=0}^{k-1} \langle x^k, p_i \rangle \cdot p_i(x)
$$
Se non si è vincolati ad una base, è possibile utilizzare i polinomi di Chebichev o di Legendre. Con una base ortonormale, il sistema delle equazioni normali si risolve banalmente: 

$$
\begin{cases}
a_1 = \langle f, \varphi_1 \rangle \\
\vdots \\
a_n = \langle f, \varphi_n \rangle \\
\end{cases}
$$


## Quadratura

