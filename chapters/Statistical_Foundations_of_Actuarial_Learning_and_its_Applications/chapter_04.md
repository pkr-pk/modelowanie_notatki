# Rozdział 4: Modelowanie predykcyjne oraz ewaluacja prognoz

## 4.3 Bootstrap

Metoda bootstrap została wynaleziona przez Efrona i Tibshiraniega. Bootstrap jest używany do symulacji nowych danych albo z dystrybuanty empirycznej $\hat{F}_n$, albo z estymowanego modelu $F(·; \hat{\theta})$. Pozwala to na przykład na ocenę zewnętrznej wartości oczekiwanej w ogólnej stracie dewiacyjnej (GL) (4.32), co wymaga modelu danych dla $Y_n.

***
### 4.3.1 Nieparametryczna symulacja bootstrap

Załóżmy, że mamy obserwacje i.i.d. $Y_1,...,Y_n$ z nieznanej dystrybuanty $F(·; \theta)$. Na podstawie tych obserwacji $Y=(Y_1,...,Y_n)^{\top}$ wybieramy regułę decyzyjną $A : \mathbb{Y} \rightarrow \mathbb{A} = \Theta \subseteq \mathbb{R}$, która dostarcza nam estymatora dla $\theta.

$$Y \mapsto \hat{\theta} = A(Y). \quad (4.39)$$

Zazwyczaj reguła decyzyjna A(·) jest znaną funkcją i chcielibyśmy określić właściwości dystrybucyjne estymatora parametru (4.39) jako funkcję (losowych) obserwacji $Y$. Na przykład dla dowolnego zbioru mierzalnego $C$, możemy chcieć obliczyć

$$P_{\theta} [\hat{\theta} \in C] = P_{\theta} [A(Y) \in C] = \int_{\mathbb{Y}} 1_{\{A(y) \in C\}} dP(y; \theta ). \quad (4.40)$$

Ponieważ zazwyczaj prawdziwa dystrybuanta generująca dane $Y_i \sim F(·; \theta)$ nie jest znana, właściwości dystrybucyjne $\hat{\theta}$ nie mogą być określone, nawet za pomocą symulacji Monte Carlo. Ideą bootstrapu jest aproksymacja $F(·; \theta)$. Jako aproksymację $F(·; \theta)$ wybieramy dystrybuantę empiryczną obserwacji i.i.d. Y, daną przez:

$$\hat{F}_n(y) = \frac{1}{n} \sum_{i=1}^{n} 1_{\{Y_i \le y\}} \quad \text{dla} \quad y \in \mathbb{R}.$$

Twierdzenie Gliwenki-Cantellego [64, 159] mówi nam, że dystrybuanta empiryczna $\hat{F}_n$ zbiega jednostajnie do $F (·; \theta )$, p.n., gdy $n \rightarrow \infty$, więc powinna być dobrą aproksymacją $F (·; \theta )$ dla dużego n. Ideą jest teraz symulowanie z dystrybuanty empirycznej $\hat{F}_n$.

**Algorytm (nieparametrycznego) bootstrapu**

(1) Powtarzaj dla $m = 1,...,M$:
(a) symuluj obserwacje i.i.d. $Y_1^*,...,Y_n^*$ z $\hat{F}_n$ (są one uzyskiwane przez losowe losowanie ze zwracaniem z obserwacji $Y_1,...,Y_n$; oznaczamy ten rozkład ponownego próbkowania $Y^* = (Y_1^*,...,Y_n^*)^{\top}$ przez $P^* = P_Y^* $);
(b) oblicz estymator $\hat{\theta}_{(m)}^* = A(Y^*)$.
(2) Zwróć $\hat{\theta}_{(1)}^*,...,\hat{\theta}_{(M)}^*$ i wynikającą z tego empiryczną dystrybuantę bootstrapową $\hat{F}_{M}^*(\vartheta) = \frac{1}{M} \sum_{m=1}^{M} 1_{\{\hat{\theta}_{(m)}^* \le \vartheta\}}$, dla estymowanego rozkładu $\hat{\theta}$.

Możemy użyć empirycznej dystrybuanty bootstrapowej $\hat{F}_{M}^*$ jako estymaty prawdziwej dystrybuanty $\hat{\theta}$, to znaczy estymujemy i aproksymujemy 
$P_{\theta} [\hat{\theta} \in C] \approx \hat{P}_{\theta} [\hat{\theta} \in C] \overset{\text{def.}}{=} P_Y^* [\hat{\theta}^* \in C] \approx \frac{1}{M} \sum_{m=1}^{M} 1_{\{\hat{\theta}_{(m)}^* \in C\}}$, (4.41)
gdzie $P_Y^*$ odpowiada rozkładowi bootstrapowemu z kroku (1a) powyższego algorytmu, i gdzie ustawiamy $\hat{\theta}^* = A(Y^*)$. Ten rozkład bootstrapowy $P_Y^*$ jest aproksymowany empirycznie przez empiryczną dystrybuantę bootstrapową $\hat{F}_{M}^*$ do badania $\hat{\theta}^*$.

**Uwagi 4.29**

* Jakość aproksymacji w (4.41) zależy od bogactwa obserwacji $Y = (Y_1,...,Y_n)^{\top}$, ponieważ rozkład bootstrapowy $P_Y^* [\hat{\theta}^* \in C] = P_{Y=y}^* [\hat{\theta}^* \in C]$, zależy od realizacji y danych Y, z których generujemy próbę bootstrapową $Y^*$. Zależy ona również od M i konkretnych losowań $Y_i^*$ dostarczających empiryczną dystrybuantę bootstrapową $\hat{F}_{M}^*$. Ta druga niepewność może być kontrolowana, ponieważ rozkład bootstrapowy $P_Y^*$ odpowiada rozkładowi wielomianowemu, a twierdzenie Gliwenki-Cantellego [64, 159] ma zastosowanie do $\hat{F}_{M}^*$ i $P_Y^*$ dla $M \rightarrow \infty$. Pierwsza niepewność, wynikająca z realizacji $Y = y$, nie może być zmniejszona, ponieważ nie możemy wzbogacić obserwacji Y.

* Empiryczna dystrybuanta bootstrapowa $\hat{F}_{M}^*$ może być użyta do **estymacji średniej estymatora** $\hat{\theta}$ podanego w (4.39) $\hat{E}_{\theta} [\hat{\theta}] = E_Y^* [\hat{\theta}^*] \approx \frac{1}{M} \sum_{m=1}^{M} \hat{\theta}_{(m)}^*$, i jego **wariancji** $\text{Var}_{\theta} (\hat{\theta}) = \text{Var}_{P_Y^*} (\hat{\theta}^*) \approx \frac{1}{M-1} \sum_{m=1}^{M} \left( \hat{\theta}_{(m)}^* - \frac{1}{M} \sum_{k=1}^{M} \hat{\theta}_{(k)}^* \right)^2$.

* Poprzedni punkt omawia aproksymację średniej i wariancji bootstrapowej. **Przedziały bootstrapowe** dla współczynników pokrycia wymagają pewnej ostrożności i istnieją różne wersje. Naiwny sposób polegający na obliczaniu kwantyli z $\hat{F}_{M}^*$ często nie działa dobrze, i mogą być potrzebne metody takie jak podwójny bootstrap.

* W (4.39) założyliśmy, że interesującą nas wielkością jest parametr $\theta$, ale podobne rozważania dotyczą również ogólnych reguł decyzyjnych estymujących $\gamma (\theta )$.

* Bootstrap zdefiniowany powyżej działa bezpośrednio na obserwacjach $Y_1,...,Y_n$, a podstawowym założeniem jest, że te obserwacje są i.i.d. Jeśli tak nie jest, może być konieczne najpierw przekształcenie obserwacji, na przykład można obliczyć reszty i założyć, że te reszty są i.i.d. W bardziej skomplikowanych przypadkach rezygnuje się nawet z założenia i.i.d. i zastępuje je założeniem o identycznej średniej i wariancji, tzn. zakłada się, że wszystkie reszty są niezależne, wycentrowane i mają jednostkową wariancję. Nazywa się to czasem **residual bootstrap** i może być odpowiednie w modelach regresji, które zostaną wprowadzone poniżej. Zatem, w tym drugim przypadku estymujemy dla każdej obserwacji $Y_i$ jej średnią $\hat{\mu}_i$ i odchylenie standardowe $\hat{\sigma}_i$, na przykład używając funkcji wariancji wybranego EDF. To pozwala na obliczenie reszt $\hat{\varepsilon}_i = (Y_i - \hat{\mu}_i)/\hat{\sigma}_i$. Dla *residual bootstrap* ponownie losujemy reszty $\hat{\varepsilon}_i^*$ z $\hat{\varepsilon}_1,...,\hat{\varepsilon}_n$. To dostarcza obserwacji bootstrapowych $Y_i^* = \hat{\mu}_i + \hat{\sigma}_i \hat{\varepsilon}_i^*$. **Wild bootstrap** zaproponowany przez Wu [386] dodatkowo używa wycentrowanej i znormalizowanej zmiennej losowej i.i.d. $V_i$ (również niezależnej od $\hat{\varepsilon}_i^*$), aby zmodyfikować obserwacje *residual bootstrap* do $Y_i^* = \hat{\mu}_i + \hat{\sigma}_i V_i \hat{\varepsilon}_i^*$.

Bootstrap nazywa się **zgodnym** dla $\hat{\theta}$, jeśli dla wszystkich $z \in \mathbb{R}$ mamy następującą zbieżność według prawdopodobieństwa, gdy $n \rightarrow \infty$ 
$P_{\theta} \left[ \sqrt{n} (\hat{\theta} - \theta) \le z \right] - P_Y^* \left[ \sqrt{n} (\hat{\theta}^* - \hat{\theta}) \le z \right] \xrightarrow{\text{wg. p.}} 0$,
wielkości $\hat{\theta} = \hat{\theta}_n$ i $\hat{\theta}^* = \hat{\theta}_n^*$ zależą od (rozmiaru n) obserwacji $Y = Y_n$; zbieżność według prawdopodobieństwa jest potrzebna, ponieważ $Y = Y_n$ są wektorami losowymi. Załóżmy, że $\hat{\theta}^{\text{MLE}} = \hat{\theta}$ jest estymatorem największej wiarygodności (MLE) dla $\theta$ spełniającym założenia Twierdzenia 3.28. Wtedy mamy asymptotyczną normalność, zobacz (3.30),
$\sqrt{n} (\hat{\theta} - \theta) \Rightarrow \mathcal{N}(0, I_1(\theta)^{-1})$ gdy $n \rightarrow \infty$,
z informacją Fishera $I_1(\theta)$. Zgodność bootstrapowa wymaga wtedy 
$\sqrt{n} (\hat{\theta}^* - \hat{\theta}) \xrightarrow[P_Y^*]{} \mathcal{N}(0, I_1(\theta)^{-1})$ według prawdopodobieństwa, gdy $n \rightarrow \infty$.
Zgodność bootstrapowa zazwyczaj zachodzi, jeśli $\hat{\theta}$ jest asymptotycznie normalny (gdy $n \rightarrow \infty$) i jeśli dane bazowe $Y_i$ są i.i.d. Co więcej, zgodność bootstrapowa zazwyczaj implikuje zgodną estymację wariancji i obciążenia 
$\text{Var}_{P_Y^*} (\hat{\theta}^*) / \text{Var}_{\theta} (\hat{\theta}) \xrightarrow{\text{wg. p.}} 1$ oraz $E_Y^* [\hat{\theta}^* - \hat{\theta}] / E_{\theta} [\hat{\theta} - \theta] \xrightarrow{\text{wg. p.}} 1$ gdy $n \rightarrow \infty$.
Po więcej informacji i przedziały ufności bootstrapowe odsyłamy do Rozdziału 5 w notatkach z wykładów Bühlmanna i Mächlera [59].

***
### 4.3.2 Parametryczna symulacja bootstrap

Dla bootstrapu parametrycznego zakładamy, że znamy rodzinę parametryczną $\mathcal{F} = \{F(·; \theta); \theta \in \Theta\}$, z której wygenerowano obserwacje i.i.d. $Y_1,...,Y_n \sim F(·; \theta)$, a jedynie konkretny wybór parametru $\theta \in \Theta$ nie jest znany. Na podstawie tych obserwacji konstruujemy estymator $\hat{\theta} = A(Y)$ dla nieznanego parametru $\theta \in \Theta$.

**Algorytm (parametrycznego) bootstrapu** 

(1) Powtarzaj dla $m = 1,...,M$:
(a) symuluj obserwacje i.i.d. $Y_1^*,...,Y_n^*$ z $F(·; \hat{\theta})$ (oznaczamy rozkład ponownego próbkowania $Y^* = (Y_1^*,...,Y_n^*)^{\top}$ przez $P^* = P_Y^*$);
(b) oblicz estymator $\hat{\theta}_{(m)}^* = A(Y^*)$.
(2) Zwróć $\hat{\theta}_{(1)}^*,...,\hat{\theta}_{(M)}^*$ i wynikającą z tego empiryczną dystrybuantę bootstrapową $\hat{F}_{M}^*(\vartheta) = \frac{1}{M} \sum_{m=1}^{M} 1_{\{\hat{\theta}_{(m)}^* \le \vartheta\}}$.

Następnie estymujemy i aproksymujemy rozkład $\hat{\theta}$ analogicznie do (4.41), i te same uwagi mają zastosowanie co w przypadku bootstrapu nieparametrycznego. Bootstrap parametryczny ma tę zaletę, że może wzbogacić dane poprzez próbkowanie nowych obserwacji z rozkładu $F(·; \hat{\theta})$. Wada bootstrapu parametrycznego pojawi się, jeśli rodzina $\mathcal{F}$ jest błędnie wyspecyfikowana; wtedy próba bootstrapowa $Y^*$ będzie tylko słabo opisywać prawdziwe dane Y, np. jeśli dane wykazują naddyspersję, ale wybrana rodzina $\mathcal{F}$ nie pozwala na modelowanie takiej naddyspersji.