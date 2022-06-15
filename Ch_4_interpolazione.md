# 4. Interpolazione

[TOC]

## Definizione del problema

È un problema di approssimazione di una funzione o di un insieme di dati con una funzione che sia più semplice e che abbia buone proprietà di regolarità. È utilizzata quando i dati sono forniti con precisione. La **condizione di interpolazione** è che la funzione interpolante e la funzione interpolata coincidano nei nodi che sono le ascisse dei dati.



## Interpolazione polinomiale

Nell'interpolazione polinomiale la funzione interpolata sarà un polinomio. Con $P_n$ indicheremo l'insieme dei polinomi di grado minore o uguale ad $n$. 



### Teorema di Vandermonde

Siano dato gli $n+1$ nodi $x_0, \dots, x_n$ distinti ed i dati in corrispondenza $y_0, \dots, y_n$, allora esiste sempre uno ed un solo polinomio interpolante di grado $n$ passante per tutti i punti $x_i$.
$$
\exist_1 p \in P_n : p(x_i) = y_i \hspace{1cm} i=0, 1, \dots, n
$$


### Metodo dei coefficienti indeterminati

Anche detto metodo di Vandermonde, consiste nel ricavare il polinomio risolvendo un sistema lineare. Un polinomio $p \in P_n$ ha $n+1$ coefficienti, che sarebbero le incognite. Se abbiamo $n+1$ nodi allora possiamo facilmente costruire un sistema lineare quadrato per ottenere i coefficienti del polinomioì, e quindi un polinomio che rispetti la condizione di interpolazione. 
$$
\begin{cases}
a_0 + a_1 x_0^{1} + \dots + a_n x_0^{n} = y_0 \\
a_0 + a_1 x_1^{1} + \dots + a_n x_1^{n} = y_1 \\
\vdots \\
a_0 + a_1 x_n^{1} + \dots + a_n x_n^{n} = y_n \\
\end{cases}
$$
Purtroppo il metodo genera una matrice mal condizionata per il problema dei sistemi lineari al crescere di $n$. Esistono metodi migliori per il problema dell'interpolazione. 



### Metodo dei polinomi di Lagrange

Si pone il polinomio $p \in P_n$ alla seguente espressione: 
$$
p(x) = \sum_{i=0}^{n} y_i L_i(x)
$$
Dove gli $L_i$ sono i polinomi di Lagrange e sono definibili come una base dello spazio dei polinomi $P_n$, mentre nei casi precedenti viene usata la base canonica ($1, x, x^2, \dots, x^n$). La costruzione avviene come segue: 
$$
L_i(x) = \prod_{j\ne i, j=0}^n \frac{x - x_j}{x_i - x_j}
$$
Il comportamento di $L_i$ con i nodi è quello di un interruttore: 
$$
Li(x_j) = \begin{cases}
0 & x \ne x_i\\
1 & x = x_i
\end{cases}
$$
Quindi è ovvio che il polinomio definito precedentemente interpola i nodi, essendo che quando è sottomesso a $x_i$ si attiva $L_i(x_i)=1$ e si annullano tutti gli altri, quindi $p(x_i) = y_i L_i(x_i) = y_i$.  



### Metodo delle differenze divise di Newton

Anche in questo metodo si utilizza una base differente, e si crea $p \in P_n$ come segue: 
$$
p(x) = b_0 + b_1(x - x_0) + b_2(x-x_0)(x-x_1) + \dots +
b_n\prod_{i=0}^{n-1}(x - x_i)
$$
Osserviamo che:

1. $p(x_0) = b_0$
2. $p(x_1) = b_0 + b_1(x-x_0)$
3. etc.

Le espressioni successive si annullano a causa di moltiplicazioni per $0$. Una volta osservato questo, bisogna calcolare i termini $b_i$ imponendo che sia soddisfatta la condizione di interpolazione. Per il primo nodo si ha: 
$$
p(x_0) = b_0 = y_0 \Longrightarrow b_0 = y_0
$$
Vediamo cosa succede per il secondo nodo: 
$$
p(x_1) = b_0 + b_1(x_1-x_0) = y_1
$$
Quindi: 
$$
b_1 = \frac{y_1 - b_0}{x_1 - x_0} = \frac{y_1 - y_0}{x_1 - x_0}
$$
Questo termine, che somiglia al rapporto incrementale, prende il nome di **prima differenza divisa**
$$
f[x_0, x_1] = \frac{y_1 - y_0}{x_1 - x_0}
$$
In generale, si ha che per $0 < i \le n$ il termine $b_i$ corrisponde alla differenza divisa: 
$$
b_i = f[x_0, x_1, \dots, x_i]
$$

#### Calcolo delle differenze divise

Un trucco per calcolare le differenze divise è quello di creare una tabella piramidale come segue: 

![image-20220615112218108](Ch_4_interpolazione.assets/image-20220615112218108.png)

Sia $P$ la tabella piramidale, con $P[i, j]$ ci riferiamo all'elemento $i$ della colonna $j$. Possiamo riempire la colonna calcolando gli elementi come segue (dalla 3° colonna in poi, dato che le prime due sono note): 
$$
P[i, j] = \frac{P[i-1, j] - P[i-1, j+1]}{P[0, j+i] - P[0, j]}
$$
Le differenze divise che ci interessano stanno nella prima riga della tabella, quindi $P[i, 0]$ per $i=1, \dots, n$, quindi in generale: 
$$
b_i = P[i, 0]
$$


### Metodo delle differenze

Deriva dal metodo delle differenze divise e si adotta solo nel caso in cui i nodi sono **ugualmente spaziati** tra loro, e quindi data una spaziatora $h$ si può definire una relazione: 
$$
x_i = x_0 + hi
$$
Se anziché considerare le differenze divise, considerassimo solo le differenze, allora potremmo definire ricorsivamente il seguente schema: 
$$
\begin{cases}
\Delta^0 f(x) &= f(x) \\
\Delta^1 f(x) &= \Delta(\Delta^0 f(x)) = f(x+h) - f(x) \\
\Delta^i f(x) &= \Delta(\Delta^{i-1}f(x))
\end{cases}
$$
Per capire meglio come funzionano, calcoliamo $\Delta^2 f(x)$: 
$$
\begin{split}
\Delta^2 f(x) &= \Delta(\Delta^1 f(x)) \\
&= \Delta(f(x+h) - f(x)) \\
&= \Delta f(x+h) - \Delta f(x) \\
&= f(x+2h) - f(x+h) - f(x+h) + f(x)) \\
&= f(x+2h) + 2 f(x+h) + f(x)
\end{split}
$$
Si può effettuare il calcolo delle differenze attraverso la tabella, ma senza dividere:
$$
P[i, j] = P[i-1, j] - P[i-1, j+1]
$$

 #### Teorema 

Se i nodi sono equispaziati, allora vale che: 
$$
f[x_i, \dots, x_{i+k}] = \frac{\Delta^k f(x)}{k! h^k}
$$
e vale anche la seguente formula per il calcolo del polinomio interpolante: 
$$
p(x) = p(x_0 + rh) = \sum_{j=0}^n \left[\Delta^j f(x_0) \binom{r}{j}\right]
$$
Dove $r$ è ottenuto come $r = (x - x_0) / h$. Tramite questa formula è possibile ignorare completamente il calcolo dei coefficienti $b_i$ e valutare il polinomio interpolante in un qualsiasi punto (previa costruzione della tabella che determina le differenze $\Delta^j f(x_0)$). 



### Problemi legati all'interpolazione polinomiale

Un polinomio di grado troppo alto continuerà ad interpolare i nodi, ma tra un punto ed il successivo oscillerà in maniera crescente al crescere di $n$. Vedremo come rimediare quando studieremo le splines. 



## Interpolazione Lagrangiana





 
