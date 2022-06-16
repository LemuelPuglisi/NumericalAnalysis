# 5. Minimi quadrati

[TOC]

## Definizione del problema

Vogliamo creare un polinomio interpolante che passi il più vicino possibile ai campioni indicati, pur mantenendo minimo il grado del polinomio stesso (condizione di interpolazione viene rilassata). 



### Problema di ottimizzazione dei minimi quadrati

Si definisce **polinomio approssimante ottimo** per un certo insieme di campioni $f(x_i)$ un polinomio $p$ tale che: 
$$
\min\sum_{_i=0}^n \left( f(x_i) - p(x_i) \right)^2
$$


### Problema di ottimizzazione min-max

Si definisce **polinomio approssimante ottimo** per un certo insieme di campioni $f(x_i)$ un polinomio $p$ tale che: 
$$
\min \max|f(x_i) - p(x_i)|
$$


### Legame tra norme e problemi di ottimizzazione

La norma è anche utilizzata per definire la distanza tra due punti nello spazio, quindi data una tolleranza $\epsilon \in \R$, possiamo dire che $p$ approssima $f$ se 
$$
\|f - p\| \le \epsilon
$$


## Norma di funzioni

Si può [provare](https://www.youmath.it/forum/algebra-lineare/14867-spazio-vettoriale-delle-funzioni-continue.html) che lo spazio delle funzioni continue sull'intervallo $[a,b]$ è uno spazio vettoriale.  Lo spazio delle funzioni continue è uno spazio **infinito-dimensionale**.  Uno spazio in cui è definita una [funzione norma](https://it.wikipedia.org/wiki/Norma_(matematica)), è detto **spazio normato**. 

Sia definita una funzione $w: [a,b] \to \R^+$ continua ed integrabile in $[a,b]$, detta **funzione peso**. Definiamo le tre norme precedenti nel caso continuo come segue: 
$$
\begin{split}
\text{Norma 1} \hspace{.5cm} & \|f\|_1 = \int_a^b |f(x)|w(x)dx \\
\text{Norma 2} \hspace{.5cm} & \|f\|_2 = \sqrt{\int_a^b f^2(x)w(x)dx} \\
\text{Norma infinito} \hspace{.5cm} & \|f\|_\infty = \max_{x \in [a,b]} |f(x)|
\end{split}
$$


### Non equivalenza delle norme in spazi infinito-dimensionali

Abbiamo visto precedentemente l'equivalenza delle norme in spazi vettoriali di dimensione finita. Per spazi infinito-dimensionali questo non vale. Possiamo osservarlo attraverso la seguente classe di funzioni:
$$
f_k(x) = \begin{cases}
k(k^2x-1)  &  \frac{1}{k^2} \le x \le \frac{2}{k^2} \\
-k(k^2x-3) &  \frac{2}{k^2} \le x \le \frac{3}{k^2} \\
0 & \text{altrimenti}
\end{cases}
$$
Sia $f(x)=0$ la funzione nulla, abbiamo che: 
$$
\|f - f_k\| = \frac1k \hspace{1cm}
\|f - f_k\|_2 = \frac{\sqrt2}{\sqrt3} \hspace{1cm}
\|f - f_k\|_\infty = k
$$
Per $k\to \infty$ si ottiene: 
$$
\lim_{k\to\infty}\|f - f_k\| = 0^+ \hspace{1cm}
\lim_{k\to\infty}\|f - f_k\|_2 = \frac{\sqrt2}{\sqrt3} \hspace{1cm}
\lim_{k\to\infty}\|f - f_k\|_\infty = +\infty
$$
Le tre norme sono asintoticamente non equivalenti al variare del parametro $k$, il quale identifica la signola funzione all'interno della classe di funzioni. 



> D'ora in poi faremo implicitamente riferimento alla norma 2 come norma di funzioni. 



## Miglior polinomio approssimante

Sia $V$ uno spazio infinito-dimensionale normato (es. spazio delle funzioni continue in un certo intervallo) e sia $W \subset V$ uno spazio finito-dimensionale con [dimensione](https://www.youmath.it/domande-a-risposte/view/6294-dimensione-spazio-vettoriale.html) $\dim(W)=n$ (es. polinomi di grado $n$).  Diremo che $w^* \in W$ è la migliore approssimazione di $f \in V$ se: 
$$
\| f- w^* \| \le \| f- w \| \hspace{.8cm} \forall w \in W
$$

### Problemi sottoposti

1. (analitico) Esistenza di $w^*$
   - Esiste se $V$ è uno spazio normato.
2. (analitico) Unicità di $w^*$
   - Esiste unico in uno spazio $V$ in cui è definito il prodotto scalare.
3. (numerico) Costruzione $w^*$
   - Nel caso della norma due, attraverso i minimi quadrati.
   - Nel caso della norma del massimo, attraverso il metodo min-max.



 ### Teorema dell'approssimazione di Weierstrass

Se $f \in C[a,b]$ allora per ogni $\epsilon >0$ esiste un polinomio $p \in P_n$ ($n$ non fissato) tale che
$$
\|f-p\|< \epsilon
$$

> Questo implica che per qualunque tolleranza $\epsilon$ scelta esiste un polinomio di un certo grado che riesce ad approssimare la funzione nei termini di tolleranza. 



## Problema discreto ai minimi quadrati

Supponiamo di avere un insieme discreto di punti $\{(x_0, y_0), \dots, (x_m, y_m) \}$ e di voler costruire il miglior polinomio approssimante $p^* \in P_n$. In tal caso: 
$$
p^* = arg\min_{p \in P_n} \sum_{i=0}^m w_i \left[p^*(x_i) - y_i\right]^2
$$
Dove il generico $w_i$ pesa l'$i$-esimo errore di misura. Nel caso generale porremo $w_i=1$, quindi si avrà il seguente problema di ottimizzazione: 
$$
p^* = arg\min_{p \in P_n} \sum_{i=0}^m \left[p^*(x_i) - y_i\right]^2
$$

### Costruzione del miglior polinomio approssimante

Solitamente il problema di ottimizzazione proposto si traduce in un sistema lineare sovradimensionato, quindi con $m$ equazioni ed $n$ incognite e dove $m > n$. Un sistema del genere ha soluzione solo se $b$ si trova nell'immagine di $Ax$. Spesso però questo tipo di sistema non ha una soluzione, ma è comunque  possibile calcolare un punto $\bar x$ tale che: 
$$
\|A\bar{x}-b\| \le \|Ax - b\| \hspace{1cm} \forall x\in \R^n
$$
In pratica, $\bar x$ deve essere minimo rispetto ad ogni altra soluzione. Il seguente teorema ci fornisce un metodo di costruzione di tale punto, ovvero il **metodo dei minimi quadrati discreto**.



### Teorema (Sistema classico corrispondente)

Sia $Ax=b$ un sistema lineare sovradimensionato, allora $\bar x$ tale che 
$$
\|A\bar{x}-b\| \le \|Ax - b\| \hspace{1cm} \forall x\in \R^n
$$
Sarà soluzione del sistema quadrato
$$
A^TA x = A^T b
$$

### Svantaggi del metodo dei M.Q. discreto

La matrice $A^TA$ diventa malcondizionata per il problema dei sistemi lineari al crescere della dimensione di $A$. 



## Problema continuo ai minimi quadrati

Sia $f \in C[a,b]$ la funzione da approssimare e sia $w \in C^+[a,b]$ la funzione peso, si deve trovare il polinomio $p^* \in P_n$ tale che: 
$$
p^* = arg\min_{p \in P_n}\int_a^b w(x) \left[p^*(x) - f(x)\right]^2dx
$$


### Conoscenze preliminari

Richiamiamo dei concetti necessari alla comprensione dell'argomento. 



#### Prodotto scalare

Sia $V$ uno spazio vettoriale, il prodotto scalare $\langle \cdot, \cdot \rangle$ è definito da una qualunque funzione che  rispetti le seguenti proprietà $\forall v, v_1, v_2 \in V$ e per ogni $\alpha, \beta \in \R$: 

* Linearità rispetto alla prima componente

$$
\langle \alpha v_1 + \beta v_2, v\rangle =
\alpha\langle v_1, v \rangle + \beta\langle v_2, v \rangle
$$

* Simmetria 

$$
\langle v_1, v_2 \rangle = \langle v_2, v_1 \rangle 
$$

* Linearità rispetto alla seconda componente

$$
\langle v, \alpha v_1 + \beta v_2\rangle =
\alpha\langle v, v_1 \rangle + \beta\langle v, v_2 \rangle
$$

* Se almeno una delle due componenti è il vettore nullo $0_V$ allora il risultato è 0

$$
\langle v, 0_V\rangle = \langle 0_V, v \rangle = 0
$$



#### Ortogonalità rispetto al prodotto scalare

Sia $V$ uno spazio vettoriale e siano $f,g \in V$, sia definito un prodotto scalare $\langle \cdot, \cdot \rangle$ in $V$ e si verifichi che $\langle f, g\rangle =0$, allora $f,g$ sono ortogonali rispetto al prodotto scalare definito. 



#### Norma indotta dal prodotto scalare

