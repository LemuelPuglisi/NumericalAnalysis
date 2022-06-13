# 3. Risoluzione di Sistemi Lineari

[TOC]

## Definizione problema

Sia $A \in \R^{m \times n}$ e $b \in \R^m$ vogliamo trovare un vettore $x\in\R^n$ che soddisfi l'equazione $Ax=b$. 

Tratteremo solo sistemi quadrati, in cui esiste un'unica soluzione $x$ se e solo se:

*  esiste l'inversa $A^{-1}$ della matrice $A$
* oppure il rango della matrice $A$ è $n$
* oppure $Ax=0$ e in tal caso $x=0$



## Teorema di Cramer

Se il determinante della matrice è non nullo, allora esiste unica una soluzione del sistema, ed è data da: 
$$
x_i = \frac{\det(\Delta_i)}{\det(A)}
$$
Dove $\Delta_i$ è $A$ a cui sostituiamo l'$i$-esima riga con il vettore dei termini noti. 



## Numero di condizionamento nei sistemi lineari

Sia $A$ una matrice quadrata, il numero di condizionamento $k(A)$ della matrice è un valore che misura la sensibilità della soluzione di $Ax=b$ alle perturbazioni nei dati. Idealmente vorremmo lavorare con matrici con numero di condizionamento basso. Supponendo l'esistenza dell'inversa, $k(A)$ si calcola come segue: 
$$
k(A) = \|A\| \cdot \|A^{-1}\|
$$

> La matrice identità ha il più basso numero di condizionamento (1) ed è l'esempio perfetto di matrice ben condizionata nel caso della risoluzione di un sistema lineare associato. 

In generale, in numero di condizionamento è correlato all'errore sui dati come segue: 
$$
k(A) \ge \frac{\text{errore sul risultato}}{\text{errore sui dati}}
$$


### Problemi di calcolo del numero di condizionamento

La complessità del calcolo della matrice inversa cresce insieme alla crescita del numero di condizionamento. Ma per calcolare il numero di condizionamento è necessaria la matrice inversa! Affermiamo che: 

1. Se $k(A)$ è vicino ad 1 allora $k(A)$ è facilmente calcolabile
2. Se $k(A)$ è grande, allora è difficilmente ricavabile



### Correlazione tra numero di condizionamento e raggio spettrale

Supponendo l'esistenza dell'inversa, è presente la seguente relazione tra i due valori: 
$$
k(A) \ge \rho(A) \rho(A^{-1})
$$
Questo implica che:
$$
k(A) \ge \frac{\max \lambda}{\min \lambda}
$$
Banalmente al numeratore è presente il raggio spettrale di $A$. Sappiamo che se esiste $\lambda$ autovalore di $A$, allora $\lambda^{-1}$ è autovalore di $A^{-1}$. Dato che invertiamo tutti gli autovalori, l'autovalore più piccolo di $A$, una volta invertito, diventa l'autovalore più grande di $A^{-1}$: 
$$
\rho(A) \rho(A^{-1}) = \max \lambda \cdot \frac{1}{\min\lambda} = \frac{\max \lambda}{\min \lambda}
$$


### Matrice di Hilbert 

La matrice di Hilbert è un esempio di matrice mal condizionata per i sistemi lineari (ma è ben condizionata per altri problemi). In generale, l'elemento $h_{ij} = (i+j-1)^{-1}$, [approfondire qui](https://it.wikipedia.org/wiki/Matrice_di_Hilbert). 



## Metodi diretti

I metodi diretti trovano la soluzione del sistema lineare in un numero finito di passi. Sono adatti a sistemi con matrici piene, essendo che tendono a rimpire gli zeri della matrice. 



### Metodo delle sostituzioni in avanti

Supponiamo che la matrice $A$ del sistema sia triangolare inferiore. Il metodo delle sostituzioni in avanti risolve il sistema una soluzione per volta. Partendo da $x_1$ come segue: 
$$
x_1 = \frac {b_1} {l_{11}}
$$
Si calcola il generico $x_i$ 
$$
x_i = \frac{b_i - \sum_{j=1}^{i-1} l_{ij} x_j}{l_{ii}}
$$
Il codice python è il seguente.

```python
def _forward_substitution(S: LinearSystem):
    assert S.is_squared(), "Needs a squared linear system."
    x = np.zeros(S.variables)
    for i in range(S.equations):
        tmp = S.b[i]
        # Given the index i=3, the range returns the
        # previous elements (j=0, j=1, j=2). When i=0,
        # we should set range(0) that is empty.
        for j in range( i if i - 1 >= 0 else 0 ): tmp -= S.A[i, j] * x[j]
        x[i] = tmp / S.A[i, i]
    return x
```



### Metodo delle sostituzioni indietro

Supponiamo che la matrice $A$ del sistema sia triangolare superiore. Il metodo delle sostituzioni indietro risolve il sistema una soluzione per volta, partendo dal basso. Partendo da $x_n$ come segue: 
$$
x_n = \frac{b_n}{u_{nn}}
$$
Si calcola il generico $x_i$ 
$$
x_i = \frac{b_i - \sum_{j=i+1}^{n} u_{ij} x_j}{u_{ii}}
$$
Il codice python è il seguente: 

```python
def _backward_substitution(S: LinearSystem):
    assert S.is_squared(), "Needs a squared linear system."
    n = S.equations
    x = np.zeros(S.variables)
    for i in reversed(range(n)):
        tmp = S.b[i]
        # The row iterations are reversed, so we start from
        # the bottom of the matrix. We should also iterate
        # the columns in a reversed manner (recall we need
        # to compute the solution components in a give order).
        for j in range(n-1, i, -1): tmp -= S.A[i, j] * x[j]
        x[i] = tmp / S.A[i, i]
    return x
```



### Complessità metodi di sostituzione e considerazioni

I metodi di sostituzione presentati eseguono circa $\frac{n(n+1)}{2}$ divisioni e moltiplicazioni e $\frac{n(n-1)}{2}$ somme algebriche, per un costo computazionale $\Theta(n^2)$. Per derivare la complessità basta osservare che nella prima iterazione facciamo 1 div/mul, nella seconda 2, nella terza 3 e così via. Avendo $n$ righe totali basta calcolare la somma dei primi $n$ numeri naturali, che corrisponde proprio alla prima formula mostrata.

Questi metodi sono molto leggeri, ma necessitano di matrici triangolari. Gli altri metodi presentati cercheranno di triangolarizzare la matrice, per poi utilizzare i metodi di sostituzione per risolvere il sistema lineare. Se dell'istanza del problema cambia solo il vettore dei coefficienti, è possibile precalcolarsi alcuni dei parametri per rendere il metodo più rapido. 



### Metodo di eliminazione naive di Gauss (MEG)

Supponendo che la matrice $A$ sia non degenere, e sia $a_{11} \ne 0$ (altrimenti scambia righe), allora è possibile applicare il metodo di Gauss (a meno di un'altra condizione che enunceremo dopo). Alla prima iterazione si calcolano i moltiplicatori: 
$$
m_{i1}^{(1)} = - \frac{a_{i1}}{a_{11}} \hspace{1cm} i=2, \dots, n
$$
Aggiungiamo alla $i$-esima equazione la prima equazione moltiplicata per $m_{i1}$. Così facendo andremo ad annullare tutti gli elementi della prima colonna meno che il primo. In generale, durante la $i$-esima iterazione lo scopo è a nnullare tutti gli elementi della $i$-esima colonna al di sotto dell'$i$-esimo, quindi si calcolano i moltiplicatori tramite l'elemento $a_{ii}$, che prende il nome di pivot: 
$$
m_{ji}^{(i)} = - \frac{a_{ji}}{a_{ii}} \hspace{1cm} j = i+1, \dots, n
$$
E si aggiunge l'$i$-esima equazione alle successive moltiplicata per il rispettivo moltiplicatore. Al passo $n-1$ si ottiene un sistema triangolare superiore che può essere risolto con la sostituzione all'indietro. Il costo computazionale del metodo è circa $\frac 4 3n^3$. 

Affinché il metodo funzioni è necessario che gli elementi della diagonale $a_{ii}$ siano non nulli ad ogni iterazione. Questo è garantito se tutti i minori principali di $A$ sono non nulli. Di fatti, è possibile effettuare un check tramite codice come segue:   

```python
def can_apply_gem(A: np.ndarray):
    """ If all the principal minors of the matrix
        have a non-zero determinant, all the diagonals
        elements will be non-zero during the Gaussian
        elimination method.
    """
    pms = mx.get_matrix_principal_minors(A)
    for pm in pms:
        if mx.determinant(pm) == 0:
            return False
    return True
```



### MEG con Pivot Parziale

La tecnica del pivot parziale evita le divisioni per zero o per numeri prossimi allo zero nel calcolo dei moltiplicatori. Alla $i$-esima iterazione si cerca la riga $i \le k \le n$ con l'elemento maggiore nella colonna $i$.
$$
a_{ki} = \max_{i \le s \le n} |a_{si}|
$$
Si sostituisce la riga $k$-esima con la riga $i$-esima. Il beneficio sta nel fatto che viene minimizzata la grandezza del moltiplicatore, minimizzando anche l'errore amplificato durante le moltiplicazioni. La complessità totale del metodo è $O(n^2)$. 



### MEG con Pivot Totale

Il pivot totale ha gli stessi benefici del pivot parziale, ma amplificati: il metodo può scegliere tra tutti gli elementi della matrice (incompleta) e non solo quelli della colonna analizzata. Il drawback sta nella complessità implementativa e nel cambio di variabile necessario. 

Supponiamo che il pivot corrente sia $a_{ii}$ e che l'elemento più grande della matrice sia $a_{rs}$. Allora prima si effettua una permutazione delle colonne $i \leftrightarrow s$ e si memorizza il cambio di variabile, e dopodiché si scambiano le righe $i \leftrightarrow r$. Quando si ottiene la soluzione, per ottenere la soluzione rispetto al problema originale bisogna applicare i cambi di variabile a ritroso alla soluzione. 



### Fattorizzazione LU

Sia $Ax=b$ il sistema lineare da risolvere, un metodo di fattorizzazione matriciale consiste nei seguenti passi:

1. Si trova una matrice $S$ non singolare tale che $SAx = Sb$ e $SA=U$ triangolare superiore.
2. Se $S$ è triangolare inferiore, lo sarà anche $S^{-1}$ e poniamo $L=S^{-1}$
3. Osserviamo che $A = LU = S^{-1} S A = A$ quindi $LU$ è una fattorizzazione di $A$

La fattorizzazione non dipende dai termini noti, quindi se nel sistema lineare in analisi variano solo i termini noti, precalcolando la fattorizzazione si ha un risparmio in efficienza. Attraverso la fattorizzazione LU riformuliamo il metodo di eliminazione di Gauss. Siano: 
$$
A = \begin{bmatrix}
a_{11} & a_{12} & \cdots & a_{1n} \\
a_{21} & a_{22} & \cdots & a_{2n} \\
\vdots & & \ddots \\
a_{n1} & a_{n2} & \cdots & a_{nn} \\
\end{bmatrix}

\hspace{1cm}

L_1 = \begin{bmatrix}
1      & 0 & \cdots & 0 \\
m_{21} & 1 & \cdots & 0 \\
\vdots &   & \ddots &   \\
m_{n1} & 0 & \cdots & 1
\end{bmatrix}
$$
dove $m_{i1}$ per $i=2, \dots, n$ sono i moltiplicatori mostrati in precedenza.  Il prodotto $L_1 A$ equivale al primo passo di Gauss. In generale, la matrice $L_i$ è definita come segue: 
$$
L_i = \begin{bmatrix}

1      & \dots &    0    		& \dots & 0 \\
\vdots &       & \vdots  		&       & \vdots  \\
0      & \dots &    1    		& \dots & 0 \\
0      & \dots &    m_{i+1,i} 	& \dots & 0 \\
\vdots &       & \vdots  		&       & \vdots  \\
0      & \dots &    m_{n,i} 	& \dots & 1 \\
\end{bmatrix}
$$
Alla fine si ha: 
$$
U = L_{n-1} L_{n-2}\dots L_2 L_1 A
$$
Poniamo $\tilde{L} = L_{n-1} L_{n-2}\dots L_2 L_1$, quindi $U = \tilde{L}A$. Poniamo $L= \tilde{L}^-1$ e osserviamo che: 
$$
LU = \tilde{L}^{-1} U = \tilde{L}^{-1} \tilde{L}A = A
$$
Adesso, la soluzione del sistema lineare $Ax=b$ si risolve in due passaggi: 

1. $Ly = b$ e si risolve per $y$
2. $Ux = y$ e si risolve per $x$

Questo poiché con il passo (1) si trova $y=L^{-1}b$, mentre con il passo 2 si trova $x=U^{-1}L^{-1}b$, che è sicuramente soluzione del sistema $Ax=LUx=b$, ed entrambi possono essere risolti con sostituzioni in avanti ed indietro, essendo matrici triangolari inferiori (L) e superiori (U). 

Non sempre esiste una fattorizzazione LU della matrice. Condizione necessaria di esistenza è che la matrice sia non degenere, quindi che abbia il determinante non nullo. Se vale tale condizione, allora esiste sicuramente una matrice di permutazione $P$ tale che $PA=LU$.  L'esistenza è garantita per le matrici **diagonalmente dominanti** e **simmetriche definite positive**, senza dover permutare la matrice. 

La fattorizzazione è implementata nel seguente codice: 

```python
@is_matrix
def LU(A: np.ndarray):
    """ This function calculate the LU factorization of
        the matrix A, moreover: A = L x U. A third value 
        P is returned along with L and U: that's the permutation
        matrix in order to reproduce the partial pivot operations. 
        (You may want to apply those steps to a coefficient vector b
        while solving a linear system, see function gem @ linearsystem.py)
    """
    assert determinant(A) != 0
    n, _ = A.shape
    Lt = P = generate_identity_matrix(n)    
    U = A.copy()
    for i in range(n-1):
        Li = generate_identity_matrix(n)
        for j in range(i+1, n): 
            Li[j, i]  = - U[j, i] / U[i, i]              
        U = np.matmul(Li, U)    
        Lt = np.matmul(Li, Lt)
    L = inverse(Lt)
    # P is just an identity matrix (for now)
    return L, U, P
```

Mentre la risoluzione con il metodo di Gauss è implementata come segue: 

```python
def gem_solve(S: LinearSystem):
    """ Perform the Gaussian elimination method.
    """
    assert S.is_squared(), "Needs a squared linear system."
    assert mx.determinant(S.A) != 0
    # assert can_apply_gem(A)
    L, U, P = mx.LU(S.A)
    # b = np.matmul(P, b)
    y =  _forward_substitution(LinearSystem(L, S.b))
    x = _backward_substitution(LinearSystem(U, y))
    return x
```



#### Matrici di permutazione

Le tecniche del pivot parziale e totale possono essere implementate nella forma matriciale del metodo di eliminazione di Gauss attraverso delle matrici di permutazione. Una matrice di permutazione è una matrice ottenuta scambiando righe o colonne della matrice identità. In particolare, scambiando la riga $i$ con la riga $j$ di $I$ e 

* premoltiplicandola per $A$ si scambiano le righe
* postmoltiplicandola per $A$ si scambiano le colonne

Esempio: scambiamo la riga 1 e la riga 3 della matrice identità e vediamo cosa succede se la premoltiplichiamo ad una matrice qualunque: 
$$
\begin{bmatrix}
0 & 0 & 1 \\
0 & 1 & 0 \\
1 & 0 & 0
\end{bmatrix}
\cdot
\begin{bmatrix}
4 & 2 & 7 \\
6 & 2 & 9 \\
7 & 3 & 5
\end{bmatrix}
= 
\begin{bmatrix}
7 & 3 & 5 \\
6 & 2 & 9 \\
4 & 2 & 7
\end{bmatrix}
$$
L'effetto è stato uno scambio di righe. Se invece postmoltiplichiamo la matrice di permutazione:
$$
\begin{bmatrix}
4 & 2 & 7 \\
6 & 2 & 9 \\
7 & 3 & 5
\end{bmatrix}
\cdot
\begin{bmatrix}
0 & 0 & 1 \\
0 & 1 & 0 \\
1 & 0 & 0
\end{bmatrix}
= 
\begin{bmatrix}
7 & 2 & 4 \\
9 & 2 & 6 \\
5 & 3 & 7
\end{bmatrix}
$$
L'effetto è stato uno scambio di colonne. 



#### Metodo di Doolittle e di Croud

Se esplicitassimo il sistema lineare $LU=A$ avremmo $n^2$ equazioni (una per ogni elemento di $A$) ed $n^2+n$ incognite (elementi di $L$ ed $U$ supponendo che siano rispettivamente triangolari inferiore e superiore), questo implica che abbiamo $n$ gradi di libertà, ovvero la fattorizzazione $LU$ non è unica. 

I seguenti metodi eliminano i gradi di liberta imponendo dei vincoli:

* **Doolittle**: si fissa $l_{ii} = 1$ in $L$ (equivalente a eliminazione gaussiana senza pivot) 
* **Crout**: si fissa $u_{ii} = 1$ in $U$  



#### Effetto Fill-in

I metodi di fattorizzazione modificano la matrice iniziale causando un effetto fill-in, ovvero gli zeri diventano elementi non nulli. Se la matrice è inizialmente sparsa, conviene optare per metodi iterativi. 





