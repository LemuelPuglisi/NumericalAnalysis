# 7. Autovalori

[TOC]



## Definizione del problema

Sia $A \in R^{n \times n}$, si definisce $\lambda \in \C$ un autovalore di $A$ se $\exist v \in \R^n$ autovettore non nullo tale che
$$
Av = \lambda v
$$
Data una matrice $A$ il problema può porsi diversi obiettivi: 

* Trovare tutte le autocoppie
* Trovare solo gli autovalori
* Trovare l'autovalore più grande / più piccolo



## Richiami alle caratteristiche

Richiamiamo alcune caratteristiche degli autovalori, già esposte nel capitolo sulle matrici:

- Un autovettore è sempre diverso dal vettore nullo
- Un autovalore può essere nullo
- Se $\lambda_1, \lambda_2 \in \sigma(A)$ allora essi possono essere:
  - Reali e distinti
  - Reali e coincidenti
  - Complessi coniugati
- Se è presente un autovalore complesso, sarà presente anche il coniugato
- Gli autovalori di una matrice $n\times n$ sono al più $n$ 
- Gli autovettori sono invece infiniti: $Av = \lambda v \to A\alpha v = \lambda \alpha v, \forall \alpha \in \R$ 
- Gli autovalori linearmente indipendenti sono al più $n$ e formano un autospazio



## Strategia numerica per il calcolo degli autovalori

La strategia numerica per il calcolo degli autovalori è differente rispetto a quella analitica, che consiste nel risolvere l'equazione caratteristica della matrice. 

La strategia numerica consiste nel calcolare una matrice triangolare $C$ che sia simile ad $A$. Questo poiché:

1. Vale il teorema di Shur ([vedasi capitolo 2 - matrici](./Ch_2_richiami_sulle_matrici.md))
2. Matrici simili hanno gli stessi autovalori
3. Gli autovalori di una matrice triangolare risiedono sulla diagonale principale

Spesso è complesso trovare $C$ triangolare, per cui un trade-off tra complessità dell'algoritmo e risoluzione del problema è quello di trovare $C$ come **matrice di Hessemberg**. La soluzione in tali matrici non è immediata, ma è più semplice rispetto al problema principale. 



## Condizionamento nel problema degli autovalori

Il condizionamento nel problema degli autovalori è differente rispetto a quello nei sistemi lineari. La matrice di Hilbert risulta mal condizionata per sistemi lineari, ben condizionata per gli autovalori. Si supponga che $A$ sia diagonalizzabile e sia $D$ la matrice diagonale ad essa simile: 
$$
D = P^{-1}AP
$$
Sia la norma matriciale di $D$ pari all'autovalore principale di $A$: 
$$
\|D\| = \max |d_i| = \max |\lambda_i|
$$
Vale il seguente teorema: 



### Teorema di Bauer-Fike

Sotto le precedenti condizioni, sia $E$ una matrice di perturbazione per $A$ e sia $(A+E)$ la matrice perturbata, sia inoltre $\lambda \in \sigma(A+E)$ e siano $\lambda_i \in \sigma(A)$ per $i=1,\dots,n$. Allora:
$$
\min_{\lambda_i \in \sigma(A)} |\lambda - \lambda_i| \le 
\|P\| \cdot \|P^{-1}\| \cdot \|E\|
$$

> La distanza più piccola credo serva ad identificare l'autovalore $\lambda_i$ in $A$ corrispondente a $\lambda$ in $(A+E)$. Il teorema ci dice che l'errore è più piccolo dello scalare a destra. 

Se $A$ è una matrice normale, $AA^T = A^T A$, allora la matrice $P$ può essere scelta unitaria, e in tale caso si dimostra analiticamente che
$$
\|P\| = \|P^{-1}\|= 1
$$
E quindi la tesi del teorema sarà:
$$
\min_{\lambda_i \in \sigma(A)} |\lambda - \lambda_i| \le \|E\|
$$

### Numero di condizionamento

Sia $P$ la matrice di trasformazione $A$ tale che $D=P^{-1}AP$, allora il numero di condizionamento $k(A)$ di $A$ sarà: 
$$
k(A) = \|P\| \cdot \|P^{-1}\|
$$
Il miglior condizionamento nel problema degli autovalori si ha quando la matrice di trasformazione $P$ è unitaria / ortogonale: 
$$
k(A) = \|P\| \cdot \|P^{-1}\| = 1
$$


## Metodo delle potenze

Il metodo delle potenze è un metodo iterativo che calcola l'autovalore principale $\lambda_1$. Si supponga che $A$ sia diagonalizzabile, un teorema enuncia che $A$ è diagonalizzabile se e solo se possiede $n$ autovettori linearmente indipendenti. Siano $u_1, \dots, u_n$ tali autovettori. 

Il metodo parte da $x_0 \in \R$ casuale. Consideriamo $x_0$ come combinazione lineare degli autovettori: 
$$
x_0 = \alpha_1 u_1 + \dots + \alpha_n u_n
$$
Adesso osserviamo che: 
$$
A u_i = \lambda_i u_i \\
AAu_i = \lambda_i A u_i \\
A^2u_i = \lambda_i A u_i \\
A^2u_i = \lambda_i \lambda_i u_i \\
A^2u_i = \lambda_i^2 u_i \\
$$
Generalizzando si ha $A^{k}u_i = \lambda^k u_i$.
