# Rozdział 6: Inne miary dopasowania modeli

## 6.3 Measuring Lift

### 6.3.7 Uporządkowana krzywa Lorenza

Niech $\hat{\mu}_1(X)$ i $\hat{\mu}_2(X)$ będą dwoma predyktorami dla zmiennej odpowiedzi Y. W ustalaniu stawek, te predyktory odnoszą się do prawdziwej składki technicznej $\mu(X)$. Możemy sobie wyobrazić, że $\hat{\mu}_1$ jest obecnym predyktorem i rozważamy jego zastąpienie przez $\hat{\mu}_2$, pod warunkiem, że wydajność tego drugiego jest lepsza. Aby porównać te dwa predyktory, zdefiniujmy **relację** jako stosunek nowego predyktora do starego, czyli:

$$
R = \frac{\hat{\mu}_2(X)}{\hat{\mu}_1(X)}
$$

Jeśli $R$ jest mniejsze niż 1, oznacza to, że profil ryzyka $X$ jest przeszacowany przy użyciu obecnego predyktora. Taki profil jest więc narażony na **selekcję negatywną**: konkurent używający predyktora $\hat{\mu}_2$ mógłby zaoferować lepszą stawkę takim ubezpieczającym, którzy mogliby wówczas opuścić portfel.

Tak jak poprzednio, zakłada się, że oba predyktory są zbilansowane, tj. 

$$E[\hat{\mu}_1(X)] = E[\hat{\mu}_2(X)] = E[Y],$$

a dla ułatwienia wyjaśnienia, zakładamy, że $\hat{\mu}_1(X)$, $\hat{\mu}_2(X)$ i $\mu(X)$ są wszystkie ciągłe.

Zgodnie z Frees i in. (2013), definiujemy **uporządkowaną krzywą Lorenza** jako zbiór punktów $(CC[\hat{\mu}_1(X), R(X); \alpha], CC[Y, R(X); \alpha])$

$$
= \left( \frac{E[\hat{\mu}_1(X)I[R(X) \le F^{-1}_R (\alpha)]]}{E[\hat{\mu}_1(X)]} , \frac{E[Y I[R(X) \le F^{-1}_R (\alpha)]]}{E[Y]} \right)
$$

gdzie $F_R$ oznacza dystrybuantę relacji R, a $F^{-1}_R$ powiązaną funkcję kwantylową.

Zauważmy, że:

$$\begin{align*}
CC[Y, R(X); \alpha] 
&= E[Y I[R(X) \le F^{-1}_R (\alpha)]] \\
&= E[E[Y I[R(X) \le F^{-1}_R (\alpha)]|X]] \\
&= E[μ(X)I[R(X) \le F^{-1}_R (\alpha)]] \\
&= CC[μ(X), R(X); \alpha]
\end{align*}$$

więc dozwolone jest zastąpienie prawdziwej składki $\mu(X)$ rzeczywistą stratą Y.

Obie funkcje

$$s \to \frac{E[\hat{\mu}_1(X)I[R(X) \le s]]}{E[\hat{\mu}_1(X)]}$$ 

oraz 

$$s \to \frac{E[Y I[R(X) \le s]]}{E[Y]}$$

można interpretować jako dystrybuanty. Podają one udział całkowitych bieżących składek $\hat{\mu}_1(X)$ oraz udział całkowitych strat Y (lub prawdziwych składek $\mu(X)$) w subportfelu określonym przez warunek $R(X) \le s$. Podejście to opiera się zatem na selekcji negatywnej (antysylekcji) przeciwko ubezpieczycielowi. Załóżmy, że konkurent przyciąga wszystkie profile X, które są przeszacowane w ramach obecnego cennika $\hat{\mu}_1$, tj. te, dla których $R(X) \le s$ dla pewnego dostatecznie małego s. Dokładniej, w subportfelu gromadzącym wszystkie profile ryzyka takie, że $R(X) \le F^{-1}_R (\alpha)$, tj. $100\alpha\%$ polis z niższymi relacjami, odnotowujemy udział 

$$t_1 = \frac{E[Y I[R(X) \le F^{-1}_R (\alpha)]]}{E[Y]}$$ 

całkowitych strat, dla udziału 

$$t_2 = \frac{E[\hat{\mu}_1(X)I[R(X) \le F^{-1}_R (\alpha)]]}{E[\hat{\mu}_1(X)]}$$

przychodu ze składek. Rozważając punkt $(t_2, t_1)$ uporządkowanej krzywej Lorenza, odpowiadający konkretnemu $\alpha$, jego znaczenie jest następujące. Tworząc portfel ze wszystkich ubezpieczających, których relacje $R(X)$ są mniejsze niż $F^{-1}_R (\alpha)$, tj. wszystkich polis, dla których nowa składka $\hat{\mu}_2(X)$ jest mniejsza niż $F^{-1}_R (\alpha)$ razy stara składka $\hat{\mu}_1(X)$, odpowiadający przychód ze składek wynosi $t_2$, a odpowiadające straty wynoszą $t_1$, średnio. Jeśli $t_1 < t_2$, to jest to portfel rentowny, który warto utrzymać.

Te procedury graficzne mogą być uzupełnione o pojedyncze liczby. W literaturze zaproponowano dwa współczynniki do pomiaru dobroci liftu: **Wartość Liftu** (Value-of-Lift) autorstwa Meyersa i Cummingsa (2009) oraz **indeks Giniego** promowany przez Freesa i in. (2011).