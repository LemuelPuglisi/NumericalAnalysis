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

Sia $V$ uno spazio vettoriale, il prodotto scalare $\langle \cdot, \cdot \rangle : V \times V \to \R$ è definito da una qualunque funzione che  rispetti le seguenti proprietà $\forall v, v_1, v_2 \in V$ e per ogni $\alpha, \beta \in \R$: 

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



#### informazioni importanti sul prodotto scalare

* Se $\langle v,v\rangle >0 $ per ogni $v \in V, v \ne 0$ allora il prodotto scalare è [definito positivo](https://www.youmath.it/lezioni/algebra-lineare/applicazioni-lineari/3971-studio-del-segno-di-un-prodotto-scalare.html)
* Se $\langle f, g \rangle = 0$ allora $f, g \in V$ sono ortogonali rispetto al prodotto scalare 



#### Norma indotta dal prodotto scalare

Sia $V$ uno spazio vettoriale in cui è definito un prodotto scalare [definito positivo](https://www.youmath.it/lezioni/algebra-lineare/applicazioni-lineari/3973-come-normalizzare-un-vettore.html), e sia $f \in V$. Si chiama norma indotta dal prodotto scalare la norma definita come segue: 
$$
\|f\| = \sqrt{\langle f,f \rangle}
$$


#### Disuguaglianza di Cauchy-Schwarz

Siano $f,g \in V$ due funzioni linearmente indipendenti tra loro, allora vale la disuguaglianza di Cauchy-Schwarz: 
$$
\left| \langle f, g\rangle  \right| \le \|f\| \cdot \|g\|
$$


#### Base ortogonale

Sia $v_1, \dots, v_n \in V$, dove è definito un prodotto scalare $\langle \cdot , \cdot \rangle$ e la dimensione $\dim(V) =n$, allora $\{v_1, \dots, v_n\}$ è una base ortogonale di $V$ se

* $\{v_1, \dots, v_n\}$ è una base di $V$
* $i, j = 1, \dots, n$ e $i \ne j$ si ha $\langle v_i, v_j \rangle = 0$



#### Vettori ortonormali

Siano $v_1, v_2 \in V$, dove è definito un prodotto scalare $\langle \cdot , \cdot \rangle$. Si definisce $v_1$ ortonormale a $v_2$ se: 

* $\langle v_1, v_2 \rangle = 0$
* $\|v_1\| = \|v_2\| = 1$



#### Base ortonormale

Sia $v_1, \dots, v_n \in V$, dove è definito un prodotto scalare $\langle \cdot , \cdot \rangle$ e la dimensione $\dim(V) =n$, allora $\{v_1, \dots, v_n\}$ è una base ortonormale di $V$ se

* $\{v_1, \dots, v_n\}$ è una base di $V$
* $i, j = 1, \dots, n$ e $i \ne j$ si ha $\langle v_i, v_j \rangle = 0$
* $\|v_i\|=1$ per $i=1, \dots, n$

> Oss. Una base ortonormale è anche una base ortogonale. 



#### Normalizzazione di un vettore

Sia $\{v_1, \dots, v_n\}$ una base ortogonale di uno spazio vettoriale $V$ in cui è definito un prodotto scalare $\langle \cdot , \cdot \rangle$, ed una norma $\|\cdot\|$ indotta dal prodotto scalare. Allora è possibile ricavare la rispettiva base ortonormale $\{w_1, \dots, w_n\}$ nel seguente modo:
$$
w_i = \frac{v_i}{\|v_i\|} \hspace{1cm} i=1, \dots, n
$$


#### Settings del problema

Nel problema che ispezioneremo assumeremo che lo spazio vettoriale sia $C[a,b]$ lo spazio delle funzioni continue definite in un intervallo generico $[a,b]$. Il prodotto scalare tra due funzioni continue $f,g \in C[a,b]$ sarà definito come segue: 
$$
\langle f,g \rangle = \int_{a}^b w(x)f(x)g(x) dx
$$
La norma scelta è la norma 2, definita come segue: 
$$
\|f\| = \sqrt{\langle f, f\rangle} = \sqrt{\int_{a}^b w(x)f^2(x) dx}
$$
Dopodiché è necessario porsi nel caso in cui sia la norma che il prodotto scalare siano quantità definite. Per fare ciò si definisce lo spazio $L^2_w(a,b)$ ovvero l'insieme delle funzioni a valori reali definite su $(a,b)$ tali che la quantità $w(x)|f(x)^2|$ sia integrabile. 

> Si può dimostrare che $C[a,b] \subset L^2_w(a,b)$

> **A CHE SERVE?** Semplicemente a garantire che l'integrale all'interno del prodotto scalare (e quindi anche nella norma) sia sempre definito, e mai infinito o indeterminabile.

Se poniamo la funzione peso $w(x)=1$ allora lo spazio si può esprimere come $L^2(a,b)$. 



### Ridefinizione problema continuo ai MQ 

Dopo le conoscenze preliminari ed il setting del problema, possiamo fornire una definizione più chiara. Sia $f \in L_w^2(a,b)$, lo scopo sarà quello di trovare il miglior polinomio approssimante $p^*$ di grado $n$, quindi tale che: 
$$
\|f-p^*\|_2 = \inf_{q \in P_n} \|f-q\|_2
$$

> Usiamo la norma per definire un concetto di errore di approssimazione. 

> Usiamo $\inf$ anziché $\min$ poiché $P_n$ è un insieme denso e non è detto che abbia il minimo. 



### Teorema di Fourier delle equazioni normali

Sia $V$ uno spazio vettoriale infinito dimensionale in cui è definito un prodotto scalare $\langle \cdot, \cdot \rangle$ e sia $W \subseteq V$ uno spazio finito dimensionale $\dim(W)=n$. Sia $\{\varphi_1, \dots, \varphi_n\}$ una base di $W$. Allora
$$
\forall f \in V, \exist_1 w^* \in W : \|f-w^*\| = \inf_{q \in W} \|f-q\|
$$
Ovvero $w^*$ è soluzione del problema continuo ai minimi quadrati. Dato che $w^* \in W$, allora può essere espresso come combinazione lineare della sua base, ovvero esistono $a_1, \dots, a_n$ tale che: 
$$
w^* = \sum_{i=1}^n a_i \varphi_i
$$
È possibile ottenere i coefficienti ponendoli incognite del seguente sistema quadrato, che prende il nome di **sistema delle equazioni normali**: 
$$
\begin{cases}
\sum_{i=0}^n a_i \langle \varphi_i, \varphi_1 \rangle =
\langle f, \varphi_1 \rangle \\
\vdots \\
\sum_{i=0}^n a_i \langle \varphi_i, \varphi_n \rangle =
\langle f, \varphi_n \rangle \\
\end{cases}
$$


#### Problemi correlati alla risoluzione

1. I prodotti scalari sono degli integrali
2. La matrice rimane malcondizionata (analogamente al caso discreto)



#### Problema del malcondizionamento

Si può dimostrare in particolari circostanze che la matrice ottenuta nel sistema lineare è proporzionale alla matrice di Hilbert, per cui è malcondizionata. La causa numerica del malcondizionamento è da attribuirsi alla base scelta per lo spazio $W$. Si dimostra che per risolvere tale problema è sufficiente utilizzare una base di $W$ ortogonale. Il metodo non dovrebbe essere però legato alla base, per cui sono stati costruiti metodi che prendono in input una base qualunque e la ortogonalizzano, o addirittura ortonormalizzano (prima ortogonalizzazione, poi normalizzazione). Si dimostra che così facendo, la matrice ottenuta nel sistema lineare sarà una matrice diagonale. Il metodo principale di ortogonalizzazione è quello di **Gram-schmidt**, di cui parleremo dopo. 



### Digressione sulla funzione peso

Variare la funzione peso $w(x)$ provoca un cambiamento nel prodotto scalare. Essa diventa quindi un selettore per ottenere diverse famiglie di polinomi relative a tale specifico prodotto scalare. Un esempio è proprio la famiglia dei polinomi di Chebichev, ottenuta tramite il prodotto scalare definito tramite la funzione peso 
$$
w(x) = \frac{1}{1-x^2}
$$


### Polinomi ortogonali

Data una funzione peso $w(x)$ su $(a,b)$, diremo che la successione di polinomi $\varphi_j$ per $j=0,1,\dots$ forma un sistema di polinomi ortogonali su $(a,b)$ rispetto a $w$, se ogni $\varphi_j$  di grado $j$ è tale che: 
$$
\langle \varphi_j, \varphi_k \rangle =
\int_a^b w(x)\varphi_j(x)\varphi_k(x) dx = 
\begin{cases}
0 & \forall k \ne j \\
\ne 0 & k=j
\end{cases}
$$
In particolare se $\langle \varphi_j, \varphi_j \rangle = 1$ allora i polinomi si dicono ortonormali. Indicheremo l'insieme dei polinomi ortogonali di grado $n$ con $\Pi_n$.  

**Osservazione.** Se i polinomi sono ortonormali, il sistema normale diventa: 
$$
\begin{cases}
\sum_{i=0}^n a_i \langle \varphi_i, \varphi_1 \rangle =
\langle f, \varphi_1 \rangle \\
\vdots \\
\sum_{i=0}^n a_i \langle \varphi_i, \varphi_n \rangle =
\langle f, \varphi_n \rangle \\
\end{cases} 

\hspace{1cm} \to \hspace{1cm}

\begin{cases}
a_1 = \langle f, \varphi_1 \rangle \\
\vdots \\
a_n = \langle f, \varphi_n \rangle \\
\end{cases}
$$
In pratica la matrice del sistema lineare diventa **diagonale**, se i polinomi sono ortogonali, o una matrice **identità** se i polinomi sono ortonormali. 



### Teorema (lineare indipendenza e ortogonalità)

Se $\varphi_0, \dots, \varphi_n$ sono ortogonali tra loro, allora sono linearmente indipendenti, ciò formano una base. Viceversa, dati $n$ polinomi linearmente indipendenti è possibile trovare una loro combinazione lineare per cui essi risultino ortogonali. 



### Teorema (zeri dei polinomi ortogonali)

Se $p \in \Pi_n[a,b]$ (polinomio ortogonale) allora esso ha $n$ zeri **reali**, **distinti** ed **interni** ad $[a,b]$.



### Ortogonalizzazione di Gram-Schmidt

Il metodo è generalizzabile per una qualsiasi base, ma lo utilizzermo per diagonalizzare la base canonica $S$ dello spazio dei polinomi $P_n$ 
$$
S = \{1, x, x^2, \dots, x^n\}
$$
Tale base non è ortogonale rispetto ad un qualsiasi prodotto interno. L'ortogonalizzazione procede calcolando per ogni polinomio $p_i(x) \in S$ della base canonica il rispettivo polinomio ortogonale $q_i(x)$. Nel caso della base canonica, $q_k(x)$ si ricava ricorsivamente dai precedenti:
$$
q_k(x) = x^k - \sum_{i=0}^{k-1} \langle x^k, p_i \rangle \cdot p_i(x)
$$
Per $p_i$ intendiamo il polinomio $q_i$ normalizzato (diviso per la sua norma). Bisogna necessariamente definire il caso base, che sarà: 
$$
q_0(x) = 1
$$

> Il metodo è molto oneroso anche per $n$ piccolo. È possibile però ricavarsi l'equazione di ricorrenza nel caso specifico, per velocizzare il tutto (come nei polinomi di Chebicev). 



### Famiglie di polinomi ortogonali standard

Modificando la funzione peso $w(x)$ cambiano le famiglie di polinomi che si ottengono dall'ortogonalizzazione:

* Per $w(x)=1$ si ottengono i **polinomi di Legendre**
* Per $w(x)= 1 / \sqrt{1-x^2}$ si ottengono i **polinomi di Chebichev**



### Errore di approssimazione ai minimi quadrati

Il risultato si basa sull'[identità di Parseval](https://en.wikipedia.org/wiki/Parseval%27s_identity), che segue da considerazioni quali il teorema dell'approssimazione di Weierstrass affrontato precedentemente, e la [disuguaglianza di Bessel](https://en.wikipedia.org/wiki/Bessel%27s_inequality). Tale identità è la seguente: 
$$
\|f - p_n^*\|_2^2 = \sum_{j=n+1}^{\infty} \langle f, p_j \rangle^2
$$
Tale errore dipende dai coefficienti di Fourier per $j=n+1, \dots$ Ma cosa sono i **coefficienti di Fourier**? Abbiamo visto che ortogonalizzando una base, il rispettivo sistema normale è calcolabile direttamente:
$$
\begin{cases}
a_1 = \langle f, \varphi_1 \rangle \\
\vdots \\
a_n = \langle f, \varphi_n \rangle \\
\end{cases}
$$
Ed $a_1, \dots, a_n$ sono proprio i coefficienti di Fourier. 

Per calcolare l'errore puntuale è possibile utilizzare il seguente teorema. 



#### Teorema del calcolo puntuale dell'errore di approssimazione

Sia $p_n^*(x)$ il polinomio ai minimi quadrati di $f \in C^2[-1, 1]$, allora 
$$
\exist \epsilon >0, \forall x \in [-1,1] \Longrightarrow 
\left| f(x) - p_n^*(x) \right| \le \frac{\epsilon}{\sqrt n}
$$


### 🔥 RIASSUNTAZZO: COME CALCOLO IL POLINOMIO APPROSSIMANTE?

1. Calcolo la base ortonormale attraverso la procedura di Gram-Schmidt
2. Calcolo i coefficienti del polinomio attraverso le formule dirette
3. Costruisco il polinomio come mostrato nel teorema di Fourier delle equazioni normali

