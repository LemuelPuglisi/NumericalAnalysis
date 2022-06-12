# 1. Teoria degli errori

Si vuole stimare l'influenza degli errori sulla soluzione, ovvero di quanto la soluzione calcolata $\hat y$ si discosti da quella reale $y$. 



## Errore assoluto

Calcola la bontà dell'approssimazione in termini di cifre decimali.
$$
E_a = |y - \hat y|
$$


## Errore relativo

Calcola la bontà dell'approssimazione in termini di cifre significative. È spesso rappresentato in percentuale ed utilizzato per avere una rapida intuizione della grandezza dell'errore. 
$$
E_r = \bigg| \frac{y - \hat y}{y} \bigg|
$$

> L'errore relativo è una migliore stima della bontà dell'approssimazione, tranne quando $y$ è prossimo a zero.  Solitamente è più importante, poiché tiene in considerazione la grandezza dei numeri rispetto all'errore. 



## Approssimazioni corrette

Mentre con le cifre decimali ci riferiamo a tutte le cifre dopo la virgola, con **cifre significative** indichiamo le cifre necessarie ad esprimere il risultato di una misura con la precisione con cui è stata fatta. 

* Diciamo che $\hat{y}$ è una corretta approssimazione di $y$ a $p$ cifre decimali se $E_a < 10^{-p}$
  * $x=624.428731, y=624.420711, E_a=0.8\times10^{-2} < 10^{-2} \Rightarrow$ prime 2 cifre decimali uguali  

* Diciamo che $\hat{y}$ è una corretta approssimazione di $y$ a $p$ cifre significative se $E_r < 10^{-p+1}$
  * $x=624.428731, y=624.420711, E_r=1.3\times10^{-5} < 10^{-4} \Rightarrow$ prime 5 cifre significative uguali 



## Stime errore relativo

Il numero di cifre significative dà una stima dell'errore relativo. Consideriamo due numeri `990` e `110` con 2 cifre significative. L'errore sarà di $\pm5$, quindi avremo che $E_r(110) \approx 5\%$ $E_r(990) \approx 0.5\%$, quindi il range in cui varia l'errore relativo è $.5 \div 5 \%$. Si può stilare una tabella con la corrispondenza cifre significative - errore relativo. 



## Metodi iterativi

Lavoreremo spesso con metodi iterativi, che permettono di determinare una successione di soluzioni approssimate $\{x_n\}_n$ che converge alla soluzione esatta del problema $x^*$. Per ciascuna iterazione possiamo definire un errore come segue: 
$$
e_n = |x_n - x^*|
$$
Il metodo si dice convergente se si ha che: 
$$
\lim_{n\to \infty} |e_n| = 0 
\hspace{.6cm} \text{oppure} \hspace{.6cm}
\lim_{n\to \infty} |x_n| = x^*
$$


### Ordine di convergenza

Dopo aver definito la nozione di convergenza di un metodo iterativo (def. dipendente dalla norma $|\cdot|$ utilizzata), si passa a definire l'ordine di convergenza. Diciamo che il metodo ha ordine di convergenza $p$ se: 
$$
\lim_{n \to \infty} \frac{|e_{n+1}|}{|e_{n}|^p} = \text{costante}
\hspace{.6cm} \text{oppure} \hspace{.6cm}
|e_{n+1}| \le c|e_n|^p
$$
La seconda vale definitivamente a partire da un $n$ sufficientemente grande e per qualche costante $c$. L'ordine di convergenza esprime il numero di cifre decimali che il metodo guadagna, ad ogni iterazione, rispetto alla soluzione esatta. 



### Altro sui metodi iterativi

Il metodo potrebbe iterare all'infinito, per cui bisogna stabilire un **criterio di arresto**. Per ogni tipo di problema è possibile progettare diversi algoritmi, che verranno valutati in base a: 

* Stabilità
* Efficienza - Complessità computazionale
* Occupazione dello spazio



## Sistemi di numerazione

I sistemi di rappresentazione numerica sono posizionali, ovvero ogni cifra occupa una posizione corrispondente ad una potenza della base del sistema adottato. Una fonte di errore è data dal passaggio da un sistema di numerazione all'altro. Scelta una base $b$, ogni reale $a \in \R$ può essere scritto come 
$$
a = \pm (a_m b^m + \dots + a_0 b^0 + a_{-1} b^{-1} + \dots )
$$
Dove $0 \le a_i \le b-1$, $a_i \in \N$. La rappresentazione è univoca a meno che il numero non necessiti di infinite cifre consecutive $a_{-k} = b-1$ (in decimale ad esempio: 0.1299999...), allora una rappresentazione equivalente consiste nel sopprimere la successione aggiungendo un'unità all'ultima cifra rimasta (dall'esempio precedente: 0.13). 



### Teorema

Sia $b \in \N$ e $b \ge 2$ la base, sia $x \in \R \setminus \{0\}$ un numero reale, allora **esiste unico** $e \in \Z$ ed una successione $\{a_i\}$ di numeri naturali $a_i \in \N$ tale che
$$
x = \pm \bigg( \sum_{i=1}^{\infty} a_i b^{-i} \bigg) \cdot b^e
$$
Dove 

* $0 \le a_i \le b-1$
* $a_1 \ne 0$
* $a_i \ne b-1$ definitivamente

> Piccolo hint: sembra somigliare alla notazione scientifica. 



## Rappresentazione numerica in un calcolatore

In un calcolatore lo spazio di memoria è finito, e la sommatoria precedente si può estendere fino ad un $t$ numero finito. 



### Rappresentazione in virgola fissa

In sintesi $N_1$ cifre prima della virgola, $N_2$ cifre decimali, $N=N_1 + N_2$ cifre totali. Un bit $s$ è solitamente conservato per il segno.



### Rappresentazione in virgola mobile

La posizione della virgola non è fissa, ma è data dall'esponente. Un numero di cifre $t$ è riservato alla mantissa $m$, un numero $N_e$ è riservato all'esponente $e$. Un bit $s$ è solitamente conservato per il segno. I bit della mantissa $m$ determinano la precisione con cui viene rappresentato un numero, mentre i bit dell'esponente $e$ determinano il massimo ed il minimo numero rappresentabili. 



#### Insieme dei numeri macchina

L'insieme dei numeri macchina $F$ è l'insieme dei numeri reali che sono rappresentabili attraverso una mantissa da $t$ cifre e il cui esponente sta tra due interi $L$ (lower) ed $U$ (upper) definiti nel calcolatore. 
$$
F(t, b, L,U) = \{0\} \cup \bigg\{x \in \R : x = \pm b^e\sum_{i=1}^{t} a_i b^{-i} \bigg\}
$$
Lo 0 è rappresentato a parte poiché ha una rappresentazione particolare. L'insieme $F$ è finito e numerabile ed ha la seguente cardinalità: 
$$
|F| = 1 + 2 (b-1) b^{t-1}(U-L+1)
$$
La rappresentazione non è unica, ma si dice normalizzata se $a_1 \ne 0$ e la mantissa $m \ge b^{t-1}$ (in pratica le regole utilizzate nella notazione scientifica). 



#### Memorizzazione esponente

Tenendo conto che il lower bound $L$ dell'esponente è un numero negativo (minimo numero rappresentabile) allora per conservare un esponente $e$ si memorizza $e^* = e - L$, e dato che $e \ge L$ allora $e^* \ge 0$. 



### Conversione decimale-binario

La **parte intera** si divide per due: se c'è il resto si mette 1, altrimenti si mette 0 e si assegnano potenze di due crescenti.

La **parte decimale** si moltiplica per 2 (divide per 1/2), se il prodotto è minore di 1, allora si ha 0, altrimenti si ha 1. Quando il numero diventa maggiore di 1 si sottrae 1 procedendo come prima. 

```
0.2 | 2 => 0 x 2^-1
0.4 | 2 => 0 x 2^-2 
0.8 | 2 => 1 x 2^-3
1.6 | 2 => 1 x 2^-4  
... (periodico)
0.2 = .0011
```

In questo caso abbiamo una rappresentazione finita in decimale ed una rappresentazione approssimata in binario, e ciò da luogo ad un errore di arrotondamento (round-off). 



### Conversione decimale a base qualunque

Sia $n$ un numero in base 10 che vogliamo convertire in base $b \in \N$. Sia $n=(a_ja_{j-1}\ldots a_1a_0)_b$ la conversione attesa. Dividendo $n$ per $b$ sia ha: 
$$
\frac nb = a_j \cdot b^{j-1} + \ldots + a_1\cdot b^0 + \frac{a_0}b
\Longrightarrow n = bn_0 + a_0
$$
Dove $n_0 < n$. Quindi l'ultima cifra della rappresentazione $a_0$ non è che il resto intero di $n/b$. Per ottenere la penultima cifra si divide $n_0/b$ ed il procedimento si arresta ad $a_j$ tale che $n_j=0$.



### Scegliere la rappresentazione in floating point

Dato $x \in \R$ come scelgo $fl(x) \in F$? Si hanno i seguenti casi: 

* **Underflow** (esponente troppo piccolo) $e < L$ 
  * Si emette un warning e si pone $fl(x)=0$
* **Overflow** (esponente troppo grande) $e > U$
  * Segnale di errore e arresto del programma
* Se $L \le x \le U$, allora si procede:
  * Se $a_k=0$ per $k \ge t+1$ allora il numero $x$ è in $F$ e $x = fl(x)$
  * Altrimenti si ha:
    * **(Chopping)** Si esclude la parte destra della $t$-esima cifra
    * **(Rounding)** Si arrotonda in base alla $t$-esima cifra



### Epsilon macchina

Tutti i floating point che hanno lo stesso esponente $e$ hanno una spaziatura $b^{e- t + 1)}$, questo implica che due numeri con lo stesso esponente e consecutivi $p_1$ e $p_2$ sono legati dalla relazione $p_2 = p_1 + b^{e-t+1}$. La spaziatura cambia prima e dopo le potenze esatte della base. L'**epsilon macchina** è un upper bound dell'errore relativo che si ha arrotondando o troncando la rappresentazione reale per ottenere la rappresentazione in floating point. Dato che parliamo di errore relativo, possiamo studiare l'epsilon macchina limitandoci ad $e=0$ e ai numeri positivi. 

Utilizzando il rounding, l'errore assoluto più grande ottenibile è $b^{e-t+1}/2$, basta porre un numero reale al centro dello spacing. Il denominatore nell'errore relativo è il numero che stiamo approssimando. Per trovare un upper bound dell'errore relativo, questo numero deve essere il più piccolo possibile. I peggiori errori relativi capitano arrotondando numeri del tipo $1+a$ dove $a \in [0, b^{-t+1}/2]$, nello specifico il peggior caso è $a=b^{-t+1}/2$. Calcolando l'errore relativo, il denominatore è $1 + a \approx 1$, quindi abbiamo un errore relativo pari a $b^{-t+1}/2$. Questo upper bound è proprio l'epsilon macchina. Utilizzando il chopping, il ragionamento si ripete ma considerando $a \in [0, b^{-t+1}]$, quindi l'epsilon macchina è proprio pari a $b^{-t+1}$. 



### Aritmetica Standard IEEE

> PAG. 15

