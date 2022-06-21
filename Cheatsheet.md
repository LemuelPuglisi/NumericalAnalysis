# Analisi Numerica - Cheatsheet

[TOC]

## Teoria degli errori

**Ordine di convergenza di un metodo iterativo**
L'ordine di convergenza $p$ esprime il numero di cifre decimali che il metodo guadagna, ad ogni iterazione, rispetto alla soluzione esatta. 

**Insieme dei numeri macchina** 
$$
F(t, b, L,U) = \{0\} \cup \bigg\{x \in \R : x = \pm b^e\sum_{i=1}^{t} a_i b^{-i} \bigg\}
$$




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

**Norme vettoriali**
$$
\begin{split}

& \|x\|_1  = & \sum_i |x_i| \hspace{1cm} & \text{norma 1} \\
& \|x\|_2  = & \sqrt{\sum_i x_i^2} \hspace{1cm} & \text{norma 2} \\
& \|x\|_\infty = & \max_{i} |x_i|  \hspace{1cm} & \text{norma }\infty \\

\end{split}
$$
**Norme matriciali indotte**
$$
\begin{split}

\norm{A}_1 &= \max_{1 \le j \le n} \sum_{i=1}^n |a_{ij}|\\
\norm{A}_2 &= \sqrt{\rho(A^* A)} \\
\norm{A}_\infty &=\max_{1 \le i \le n} \sum_{j=1}^n |a_{ij}|\\

\end{split}
$$


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

|    Regola     |                          Formula                           |                            Errore                            |
| :-----------: | :--------------------------------------------------------: | :----------------------------------------------------------: |
| Trapezi $Q_1$ |                $\frac{f(a) + f(b)}{2}(b-a)$                |           $E_T(f) = -\frac{f''(\eta)}{12}(b-a)^3$            |
| Simpson $Q_2$ | $\frac {b-a}6 \big( f(a) + 4f(\frac{b-a}{2}) + f(b) \big)$ | $E_S (f) = - \frac{f^{(IV)}(\eta)}{2880}\left(b-a \right)^5$ |

Per le formule di Newton-Cotes vale in generale:

- Se $n$ è il grado del polinomio allora
  - Se $n$ è pari allora $m=n+1$
  - Se $n$ è dispari allora $m=n$
- L'errore $E_n$ di $Q_n$ è proporzionale a $E_n \propto h^{2n+1}$ dove $h$ è la spaziatura tra nodi



**Formula del Trapezio composto**
$$
T_N(f) = \frac h2 \bigg[ f(x_0) + f(x_N) \bigg] 
+ h \sum_{i=1}^{N-1} f(x_i) 
- f''(\eta) \frac{(b-a)^3}{12N^2}
$$
Numero $N$ di nodi necessari affinché l'errore scenda sotto $\epsilon$ nel metodo del Trapezio composto: 
$$
N \ge \sqrt{\frac{(b-a)^3 \|f''\|_{\infty}}{12 \epsilon}}
$$
Numero $N$ di nodi necessari affinché l'errore scenda sotto $\epsilon$ nel metodo di Simpson composto: 
$$
N \ge \root{4}\of{\frac{(b-a)^5 \|f^{(IV)}\|_{\infty} }{2880 \epsilon }}
$$


**Formule Gaussiane**

|        Regola        |               Formula               |           Errore           |
| :------------------: | :---------------------------------: | :------------------------: |
| Mid-point rule $Q_0$ | $(b-a) f\left(\frac{b-a}{2}\right)$ | $\frac{b-a}{2h} f''(\eta)$ |

Formula $Q_1$ con grado di precisione 3: 
$$
Q_1(f) = \frac{b-a}2 \left[
f\left( \frac{a+b}{2} -\frac{b-a}{2\sqrt{3}} \right) + 
f\left( \frac{a+b}{2} +\frac{b-a}{2\sqrt{3}} \right)
\right]
$$
Scorciatoia: i nodi sono incognite nelle formule gaussiane, ma si ottiene comunque un grado di precisione pari a $2n+1$ se come nodi si prendono gli zeri di un polinomio ortogonale (es. Chebichev). 



## Autovalori

Perché vogliamo ottenere una matrice triangolare $T$ simile alla matrice di partenza $A$: 

1. Vale il teorema di Shur ([vedasi capitolo 2 - matrici](./Ch_2_richiami_sulle_matrici.md))
2. Matrici simili hanno gli stessi autovalori
3. Gli autovalori di una matrice triangolare risiedono sulla diagonale principale



**Numero di condizionamento**

* Ripassare teorema di Bauer-Fike

Sia $P$ la matrice di trasformazione $A$ tale che $D=P^{-1}AP$, allora il numero di condizionamento $k(A)$ di $A$ sarà: 
$$
k(A) = \|P\| \cdot \|P^{-1}\|
$$
**Metodo delle potenze**

🚨 Prerequisiti: $A$ diagonalizzabile, autovalore principale non complesso

* Si parte da $x_0$ random
* Durante l'iterazione $i$-esima: 
  * Si calcola $x_{i+1} = A x_{i}$
  * Si calcola il coefficiente di Rayleigh $\beta_{i}$.
  * Se è soddisfatto un criterio di terminazione, si ferma l'algoritmo

Il coefficiente $\beta_i$ si calcola come segue
$$
\beta_k = \frac{\bar x_k^T \cdot \bar x_{k+1}}{\| \bar x_k \|_2^2}
$$

| Metodo                | Descrizione                                                  | Output                |
| --------------------- | ------------------------------------------------------------ | --------------------- |
| Fattorizzazione QR    | Si sfrutta una fattorizzazione per calcolare una matrice simile ad $A$ che tende a diventare triangolare | Matrice triangolare   |
| Metodo di Householder | Se la matrice è $n\times n$, allora in $n$ meno 1 passi la trasforma in una matrice di Hessemberg simile, operando sulle colonne. Buono per matrici piene. | Matrice di Hessemberg |
| Metodo di Givens      | Come Householder, ma anziché lavorare sull'intera colonna elimina i singoli elementi. Buono per matrici sparse. | Matrice di Hessemberg |

*****

Costruzione della **matrice di Householder** $\tilde P$ 
💫 La sua grandezza varia in base alla colonna che si sta esaminando
💫 La matrice prodotta è ortogonale e simmetrica 
$$
\tilde P = I - 2 \frac{v^Tv}{v^T v} 
\hspace{1cm} \text{dove} \hspace{1cm}
v = x + \|x\|_2 \cdot e_1
$$
Ed $x$ è il vettore estratto dalla colonna, con elementi dalla subdiagonale in giù. Per ottenere il vettore con gli elementi eliminati, basta eseguire $\bar x = \tilde Px$.

*****

Costruzione della **matrice di Givens** $J$ 
💫 Si vuole annullare l'elemento $x_k$ attraverso l'elemento $x_i$
💫 La matrice prodotta è ortogonale (ma non simmetrica)
$$
[J]_{rs} = \begin{cases}
c &  \text{ se } (r,s)=(k,k) \text{ oppure } (r,s)=(i,i) \\
1 &  \text{ se } r=s \text{ ma } r,s \ne k,i \\
s &  \text{ se } (r,s) = (i,k) \\
-s & \text{ se } (r,s) = (k, i) \\
0 &  \text{ altrimenti }
\end{cases}
$$
Dove 
$$
c = \frac{x_i}{\sqrt{x_i^2 + x_k^2}} \hspace{2cm}
s = \frac{x_k}{\sqrt{x_i^2 + x_k^2}}
$$
Per ottenere il vettore con l'elemento eliminato, basta eseguire $\bar x = Jx$. 



## Zeri di equazioni non lineari



**Metodo di bisezione**
💫 Metodo chiuso nell'intervallo $[a,b]$
💫 Richiede che $f(a)$ ed $f(b)$ abbiano segno opposto (teo. di esistenza degli zeri) 
💫 Si sceglie come punto iniziale il punto di mezzo
💫 In base al segno del punto, si aggiorna l'intervallo cambiando l'estremo dello stesso segno
💫 Si ripete fino a soddisfare un criterio di terminazione
😨 Se iteriamo $N$ volte il metodo, l'errore commesso è al più $2^{-n}(b-a)$.
🚂 La convergenza è **lineare**

*****

**Metodo della falsa posizione (regula falsi)**

💫 Metodo chiuso nell'intervallo $[a,b]$
💫 Anziché prendere sempre il punto di mezzo, si calcola la retta passante per gli estremi. 
💫 L'intersezione tra la retta e l'asse $x$ dà il nuovo punto (ponendo $y=0$)
💫 Si procede come nel metodo di bisezione per aggiornare l'intervallo
🚂 La convergenza è **lineare**
🎯 Formula diretta per il calcolo della successione di soluzioni:
$$
x_{n+1} = \frac{x_{n-1} \cdot f(x_n) - x_n \cdot f(x_{n-1})}{f(x_{n}) - f(x_{n-1})}
$$

*****

**Metodo delle corde**
💫 Metodo chiuso nell'intervallo $[a,b]$
💫 Sia $r$ la retta con pendenza $q$ che unisce i punti $(a, f(a))$ e $(b, f(b))$
💫 Il punto iniziale $x_0$ si ottiene tramite l'intersezione di $r$ con l'asse $x$ ($y=0$)
💫 Si aggiorna l'intervallo come nella bisezione
💫 Dalla 2° iterazione si utilizza la retta passante per $x_i$ e con pendenza $q$ (iniziale)
💫 Si determina il successivo intersecandola con l'asse $x$
🚂 La convergenza è **lineare**
$$
q = \frac{f(b) - f(a)}{b-a}
$$

*****

**Metodo del punto fisso**
💫 In un certo senso è un metodo chiuso dato che $g([a,b])\subseteq [a,b]$
💫 Si ottiene da $f$ una funzione ausiliaria $g$ t.c. gli zeri di $f$ siano punti fissi di $g$
💫 La successione $x_{n+1}=g(x_n)$ converge ad un punto fisso
😨 La successione converge solo se la funzione $g$ è crescente e concava o decrescente e convessa
🚂 Più derivate di $g$ si annullano nel punto fisso, più è rapida la convergenza  
🚂 Se nessuna derivata si annulla, il metodo è **lineare**, se si annulla la prima è **quadratico** (etc.)

*****

**Metodo di Newton o delle tangenti**
💫 Metodo aperto
💫 Si parte da un punto casuale $x_0$
💫 Si considera la retta tangente ad $f$ nel punto $x_i$ (quindi con pendenza data dalla derivata prima)
💫 Il punto successivo è dato dall'intersezione di tale tangente con l'asse $x$
🚄 La convergenza è **quadratica** se lo zero cercato è radice semplice (altrimenti **lineare**)
🎯 Formula diretta per il calcolo della successione di soluzioni:
$$
x_{n+1} = x_n - \frac{f({x_n})}{f'(x_n)}
$$

*****

**Metodo delle secanti**
💫 Metodo aperto
💫 Si parte da un punto casuale $x_0$
💫 Anziché calcolare la pendenza con la derivata, si calcola con il rapporto incrementale.
🚅 La convergenza è **superlineare** ($1<p<2$)
🎯 Formula diretta per il calcolo della successione di soluzioni:
$$
x_{n+1} =  \frac{f(x_n)x_{n-1} - f(x_{n-1})x_n}{f(x_n) - f(x_{n-1})}
$$
