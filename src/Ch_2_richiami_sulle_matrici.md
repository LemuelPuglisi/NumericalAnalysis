# 2. Richiami sulle matrici

> Inserirò solo teoremi e definizioni non di base. 

## Tipi di matrici

Esploriamo alcune definizioni particolari. 



### Matrice trasposta coniugata

La matrice $A \in \C^{m\times n}$ con elementi complessi ha una trasposta coniugata $C = A^*$, ovvero una semplice trasposta dove ogni elemento è il coniugato dell'elemento corrispondente in $A$: $c_{ij} = \bar{a}_{ij}$. 



### Matrice di Hessemberg

Una matrice di Hessemberg è una matrice triangolare superiore in cui è aggiunta una subdiagonale inferiore. La regola è che l'elemento $a_{ij}=0$ se $i > j + 1$ oppure $j > i+1$. 
$$
\begin{bmatrix}
1 & 4 & 2 & 3 \\
2 & 2 & 8 & 9 \\
0 & 7 & 2 & 4 \\
0 & 0 & 7 & 2 \\
\end{bmatrix}
$$

### Matrice definita positiva e negativa

Una matrice simmetrica si dice definita positiva se $\forall x \in \R^n$ e $x \ne 0$ si ha $x^T A x > 0$. Si dice definita negativa il viceversa con ($<$). Se si usa il minore-maggiore uguale, allora anziché definita la matrice è semidefinita. 



### Matrice diagonalmente dominante

Una matrice è strettamente diagonalmente dominante se per ogni riga, la somma dei valori assoluti degli elementi non in diagonale è minore stretta del valore assoluto dell'elemento in diagonale. Se anziché essere minore stretta è minore o uguale, allora la matrice si dice debolmente diagonalmente dominante. 



### Matrici unitarie, ortogonali e normali

* Matrice unitaria: $U^* U = UU^* = I$ da cui $U^{-1} = U^*$
* Matrice ortogonale: $U^TU = UU^T = I$ da cui $U^{-1} = U^T$
* Matrice normale: $U^T U = U U^T$



## Teoremi sul determinante



### Regola di Laplace per il determinante

Sia $A_{ij}$ la matrice privata dell'$i$-esima riga e della $j$-esima colonna. Si definisce complemento algebrico dell'elemento $a_{ij}$ il numero definito come segue: 
$$
c_{ij} = (-1)^{i+j} \cdot \det(A_{ji})
$$
Si definisce il determinante di $A$ sviluppato rispetto all'$i$-esima riga come segue: 
$$
\det(A) = \sum_{j=1}^n a_{ij} \cdot c_{ij}
$$


### Teorema di Binet

Date due matrici moltiplicabili $A,B$ si ha che $\det(AB)=\det(A)\cdot \det(B)$.



### Teorema di Sylvester

Indichiamo con $A_k$ la matrice $A$ formata dalle prime $k$ righe e $k$ colonne. Una matrice $A$ simmetrica si dice definita positiva se e solo se $\det(A_k) > 0$ per $k=1, \dots,n$. 



### Corollario del teorema di Sylvester

Sia $A$ una matrice simmetrica, diagonalmente dominante a diagonale positiva, allora la matrice è definita positiva. 



### Matrice non degenere

Una matrice non degenere è una matrice con determinante non nullo. 



## Teoremi sulla matrice inversa



### Costruzione della matrice inversa

Sia $\hat{A}$ la matrice trasposta dei complementi algebrici, tale matrice gode della seguente proprietà: 
$$
A\hat{A} = \hat{A}A = \det(A)\cdot I_n
$$
Questo implica che per ottenere l'inversa $A^{-1}$ di $A$ basta calcolare
$$
\frac{\hat{A}}{\det(A)} = A^{-1}
$$
Per calcolare direttamente ogni elemento dell'inversa $b_{ij}$ basterà calcolare come segue: 
$$
b_{ij} = (-1)^{i+j} \frac{\det(A_{ji})}{\det(A)}
$$


### Unicità dell'inversa

La matrice inversa quando esiste è unica. 

**Dimostrazione (per assurdo).** Supponiamo che esista una matrice $B$ diversa da $A^{-1}$ e tale che $BA=I$. Allora si ha $BAA^{-1} = IA^{-1} \Longrightarrow B =A^{-1}$, assurdo $\bot$. 



### Inversa del prodotto tra matrici

Siano $A,B$ non degeneri e sia $C=AB$, che per il teorema di Binet è non degenere. Allora si ha che $C^{-1} = B^{-1} A^{-1}$ (si noti l'inversione). 



## Norme vettoriali e matriciali



### Norma p o norma Holderiana

Sia $1 \le p \le \infty$, la norma $p$ è definita come segue: 
$$
\|x\|_p = \root{p}\of{\sum_{i} | x_i |^p}
$$
Definiamo le norme più comuni come segue: 
$$
\begin{split}

& \|x\|_1  = & \sum_i |x_i| \hspace{1cm} & \text{norma 1} \\
& \|x\|_2  = & \sqrt{\sum_i x_i^2} \hspace{1cm} & \text{norma 2} \\
& \|x\|_\infty = & \max_{i} |x_i|  \hspace{1cm} & \text{norma }\infty \\

\end{split}
$$


### Norme equivalenti in spazi discreti

In $\R^n$ le norme $1,2,\infty$ sono equivalenti, ovvero esistono $\alpha,\beta \in \R$ con $0 < \alpha < \beta$ tali che: 
$$
\alpha \|x\|_a  \le \|x\|_b \le \beta \|x\|_a 
$$
Dove $a$ e $b$ possono essere due norme distinte tra le sopracitate. 



### Norme matriciali indotte

> Credits: [Advanced LAFF, Robert van de Geijn](https://www.youtube.com/watch?v=2rIOWQY9rp4)

Supponiamo di avere una trasformazione lineare $L: \C^n \to \C^m$ e supponiamo di voler misurare quanto il vettore $x \in \C^n$ venga magnificato dalla trasformazione. Per avere una sorta di magnitudo del vettore utilizziamo la norma vettoriale $||x||$. Per capire il grado di magnificazione dato dalla trasformazione $L$ potremmo considerare il rapporto tra la norma del vettore di output $y = L(x)$ e quella del vettore di input, come segue: 
$$
\frac{||L(x)||}{||x||}
$$
Supponiamo ancora che per descrivere $L$ vogliamo trovare $x$ che ci dia il massimo rapporto: 
$$
\sup_{x \ne 0} \frac{||L(x)||}{||x||}
$$
Abbiamo quindi una definizione della magnificazione data da $L$, quindi potremmo costruire la norma di $L$ basandoci su questo concetto: 
$$
||L|| = \sup_{x \ne 0} \frac{||L(x)||}{||x||}
$$
Se abbiamo una matrice $A$ che definisce la trasformazione lineare $L$, ovvero $L(x)=Ax$, allora possiamo definire la norma matriciale di $A$ come: 
$$
||{A}|| = \sup_{x \ne 0} \frac{||{Ax}||}{||{x}||}
$$
La norma matriciale di $A$ si dice **indotta dalla norma vettoriale** utilizzata. Ritornando alla trasformazione, se effettuiamo prima lo scaling e dopodiché trasformiamo, il risultato non cambia. Questo vuol dire che le due seguenti espressioni sono equivalenti: 
$$
||{L}|| = \sup_{x \ne 0} \frac{||{L(x)}||}{||{x}||} = 
\sup_{x \ne 0} \frac{||{L(\frac{x}{||{x}||})}||}{||{\frac{x}{||{x}||}||}} = 
\sup_{x \ne 0} \frac{||{L(\frac{x}{||{x}||})}||}
{ \frac{||{x}|| }{||{x}||}}   
= 
\sup_{x \ne 0} ||{L(\frac{x}{||{x}||})}||
$$
Ma $\frac{x}{||{x}||}$ è un vettore unitario, quindi possiamo scrivere: 
$$
||{L}|| = \sup_{||{x}||=1}||norm{L(x)}||
$$
L'insieme $\{x \mid ||{x}|| = 1\}$ ha il massimo ed il minimo inclusi (compact set), quindi l'estremo superiore combacia con il massimo. 
$$
||{L}|| = \max_{||{x}|| =1} ||{L(x)}||
$$
Possiamo effettuare esattamente gli stessi passaggi con la norma matriciale indotta, quindi: 
$$
||{A}|| = \max_{||{x}|| = 1} ||{Ax}||
$$
Dalle norme vettoriali $1,2,\infty$ vengono indotte le omonime norme matriciali: 
$$
\begin{split}

||{x}||_1 = \sum_i \abs{x_i} & \to & ||{A}||_1 = \max_{1 \le j \le n} \sum_{i=1}^n |a_{ij}|\\
||{x}||_2 = \sqrt{\sum_i x_i^2} & \to & ||{A}||_2 = \sqrt{\rho(A^* A)} \\
||{x}||_{\infty} = \max_{i} \abs{x_i} & \to & ||{A}||_\infty =\max_{1 \le i \le n} \sum_{j=1}^n |a_{ij}|\\

\end{split}
$$

> Nota: nella norma 1 si prende la somma in valore maggiore assoluto della colonna, mentre nella norma infinito si prende la stessa somma, ma della riga. 

 

## Autovalori e autovettori



### Definizione classica

Sia $A \in \R^{n\times n}$ diciamo che $\lambda$ è autovalore di $A$ se $\exist \bar x \in \C^n$ che chiameremo autovettore, $\bar{x} \ne 0$, tale che $(A-\lambda I)\bar{x} = 0$ o analogamente $A\bar x = \lambda \bar x$.  Definiamo $\sigma(A)$ l'insieme degli autovalori di $A$. 



### Autovalore nullo

Una matrice contiene un autovalore nullo se e solo se è singolare ($\det(A)=0$). 



### Raggio spettrale

Il raggio spettrale è l'autovalore più grande in valore assoluto: $\rho = \max_{\lambda \in \sigma(A)} |\lambda|$. 



### Trasformata per contragradienza (simili)

Siano $A,B \in \R^{n \times n}$ ed esista $B^{-1}$ e sia $C=B^{-1}AB$, allora $C$ si dice trasformata per contragradienza di $A$ mediante $B$. Due matrici trasformate per contragradienza l'una dall'altra si dicono **simili**. 



### Proprietà matrici simili

Se $A$ e $C$ sono simili allora:

* $A^s$ e $C^s$ per $s \in \N$. 
* $A^{-1}$ e $C^{-1}$ sono simili.
* se $\det(A) \ne 0$ allora anche $\det(C) \ne 0$



### Matrice diagonalizzabile

$A \in \R^{n \times n}$  si dice diagonalizzabile se è simile ad una matrice diagonale. 



### 6 rapide proprietà sugli autovalori

1. La matrice e la sua trasposta hanno gli stessi autovalori.
2. Due matrici simili hanno gli stessi autovalori.
3. Una matrice degenere ha almeno un autovalore nullo.
4. Se $\lambda$ è autovalore di $A$ ed esiste l'inversa, allora $\lambda^{-1}$ è autovalore di $A^{-1}$.
5. Se $\lambda$ è autovalore di $A$ con autovettore $x$, $\lambda^s$ è autovalore di $A^s$ con lo stesso autovettore ($s \in \N$)
6. Se $\lambda$ è autovalore di $A$, allora $\lambda^s$ è autovalore di $A^s$ con $s \in \Z$. 



### Molteplicità degli autovalori

Definiamo:

* **Molteplicità algebrica**: molteplicità come radice del polinomio caratteristico.
* **Molteplicità geometrica**: numero di vettori linearmente indipendenti associati ad esso.



### Teorema dei cerchi di Gershgorin

> Credits: [Mu Prime Math - Learning Linear Algebra](https://www.youtube.com/watch?v=tCvRJA7T7qg)

![inclusion exclusion - Example of a matrix with a gersgorin disk that does  not contain any eigenvalue - Mathematics Stack Exchange](Ch_2_richiami_sulle_matrici.assets/EFmbG.png)



Sia $A$ una matrice quadrata $n \times n$. Il teorema di Gerschgorin ci fornisce esattamente $n$ dischi nel piano complesso (dato che gli autovalori possono anche essere complessi) all'interno dei quali risiede **un** (vedasi note) autovalore. Viene creato un disco per ogni riga $i$ della matrice, centrato nell'elemento $a_{ii}$ e di raggio $\rho_i$ calcolato come segue: 
$$
\rho_i = \sum_{j=1, j \ne i}^{n} \abs{a_{ij}}
\hspace{1cm} i=1, \dots, n
$$
Possiamo definire ognuno dei dischi matematicamente come segue: 
$$
\gamma_i = \big\{ z \in \C : \abs{z - {a_{ii}}} \le \rho_i \big\}
\hspace{1cm} i=1, \dots, n
$$

> **Note**
> In realtà il teorema non ci dimostra che in un cerchio risieda un solo autovalore, o che ce ne sia almeno uno. Una estensione del teorema dimostra che se il disco non interseca con altri dischi, allora contiene uno ed un solo autovalore. 

Possiamo concludere che, definendo l'insieme dei dischi $\gamma = \bigcup_{i=1}^n \gamma_i$ allora: 
$$
\lambda \in \sigma(A) \Longrightarrow \lambda \in \gamma
$$
Dove $\sigma(A)$ è lo spettro di $A$, ovvero l'insieme dei suoi autovalori. La **dimostrazione** è contenuta nel documento della professoressa ed è molto simile a quella indicata nel video. 



### Conseguenze del teorema

Una matrice strettamente diagonalmente dominante è non degenere. 

**Dimostrazione (per assurdo)**. Supponiamo che $A$, strettamente diagonalmente dominante, sia degenere, quindi $\det(A)=0$. Questo implica che $0 \in \sigma(A)$ per i teoremi precedenti, e quindi per il teorema di Gershgorin $0$ è all'interno di uno dei dischi! Ovvero esiste il disco i-esimo tale che:
$$
|0 - a_{ii}| \le \rho_i \Longrightarrow |a_{ii}| \le \sum_{j=1, j \ne i}^{n} \abs{a_{ij}}
$$
Il che è assurdo per ipotesi di matrice strettamente diagonalmente dominante. 



### Teorema di Hermite

Se la matrice $A$ è hermitiana, ovvero $A = A^*$ (uguale alla sua trasposta coniugata) allora gli autovalori sono tutti reali. Il teorema vale anche per matrici simmetriche, quindi se $A=A^T$, a patto che sia anche definita positiva. 



### Teorema sulle matrici diagonalizzabili

Una matrice $A$ è diagonalizzabile se e solo se ha $n$ autovettori linearmente indipendenti.



### Teorema di Shur

Sia $A \in \C^{n\times n}$ allora esiste una matrice unitaria $U$ tale che $T = U^* AU$ dove $T$ è triangolare superiore. Se $A$ è reale, allora $U$ è ortogonale. 



### Matrice convergente

Una matrice $A$ si dice convergente se $\lim_{n\to \infty} A^s = 0$ (matrice zero).



### Altri teoremi

* Una matrice hermitiana è diagonalizzabile
* Una matrice è convergente se il raggio spettrale è inferiore a 1
* Il raggio spettrale è sempre minore o uguale alla norma della matrice
* Una matrice è convergente se la norma dell'esponenziazione della matrice tende a 0

