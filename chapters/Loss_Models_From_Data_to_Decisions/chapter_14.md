# Rozdział 14: Konstrukcja modeli empirycznych

## 14.3 Estymacja empiryczna z danymi cenzurowanymi prawostronnie

W tej sekcji uogólniamy podejście empiryczne z poprzedniej sekcji na sytuacje, w których dane nie są kompletne. W szczególności zakładamy, że indywidualne obserwacje mogą być cenzurowane prawostronnie. Mamy następującą definicję.

**Definicja 14.7** Obserwacja jest cenzurowana od góry (nazywana również cenzurowaną prawostronnie) w punkcie $u$, jeśli gdy jest równa lub większa od $u$, jest zapisywana jako równa $u$, ale gdy jest poniżej $u$, jest zapisywana z jej obserwowaną wartością.

W danych dotyczących roszczeń ubezpieczeniowych, obecność limitu polisy może prowadzić do prawostronnie cenzurowanych obserwacji. Gdy kwota szkody jest równa lub przekracza limit $u$, świadczenia powyżej tej wartości nie są wypłacane, więc dokładna wartość zazwyczaj nie jest rejestrowana. Wiadomo jednak, że wystąpiła szkoda o wartości co najmniej $u$.

Podczas przeprowadzania badania śmiertelności ludzi, jeśli osoba żyje w momencie zakończenia badania, nastąpiło cenzurowanie prawostronne. Wiek osoby w chwili śmierci nie jest znany, ale wiadomo, że jest on co najmniej tak duży jak wiek w momencie zakończenia badania. Cenzurowanie prawostronne dotyczy również tych, którzy opuszczają badanie przed jego zakończeniem z powodu rezygnacji z polisy. Należy zauważyć, że ta dyskusja mogłaby dotyczyć również innych ubytków, takich jak niepełnosprawność, rezygnacja z polisy czy emerytura.

W tej sekcji i dwóch następnych zakładamy, że podstawowa zmienna losowa ma rozkład ciągły. Chociaż dane ze zmiennych losowych dyskretnych również mogą być cenzurowane prawostronnie (Zbiór Danych A jest przykładem), stosowanie estymatorów empirycznych jest rzadkie, a zatem opracowanie analogicznych wzorów prawdopodobnie nie jest warte wysiłku.

Teraz poczynimy konkretne założenia dotyczące sposobu zbierania i rejestrowania danych. Zakłada się, że mamy próbę losową, dla której niektóre (ale nie wszystkie) dane są cenzurowane prawostronnie. Dla niecenzurowanych (tj. w pełni znanych) obserwacji oznaczymy ich $k$ unikalnych wartości przez $y_1 < y_2 < \dots < y_k$. Niech $s_i$ oznacza liczbę wystąpień $y_i$ w próbie. Ustawiamy również $y_0$ jako minimalną możliwą wartość dla obserwacji i zakładamy, że $y_0 < y_1$. Często $y_0 = 0$. Podobnie, ustawiamy $y_{max}$ jako największą obserwację w danych, cenzurowaną lub niecenzurowaną. Stąd $y_{max} \ge y_k$. Naszym celem jest stworzenie empirycznego (zależnego od danych) rozkładu, który umieszcza prawdopodobieństwo na wartościach $y_1 < y_2 < \dots < y_k$.

Często posiadamy konkretną wartość, przy której obserwacja została ocenzurowana. Jednakże, zarówno dla wyprowadzenia estymatora, jak i jego implementacji, konieczne jest jedynie wiedzieć, między którymi wartościami $y$ to nastąpiło. Zatem jedynym potrzebnym wejściem jest $b_i$, liczba prawostronnie cenzurowanych obserwacji w przedziale $[y_i, y_{i+1})$ dla $i = 1, 2, \dots, k-1$. Przyjmujemy założenie, że jeśli obserwacja jest cenzurowana w $y_i$, to jest cenzurowana w $y_i + 0$ (tj. w sytuacji życiowej, natychmiast po śmierci). Możliwe jest posiadanie cenzurowanych obserwacji przy wartościach między $y_0$ a $y_1$. Jednakże, ponieważ umieszczamy prawdopodobieństwo tylko na niecenzurowanych wartościach, te obserwacje nie dostarczają informacji o tych prawdopodobieństwach i mogą być pominięte. Odnosząc się do wielkości próby, $n$ będzie oznaczać liczbę obserwacji po ich pominięciu. Obserwacje cenzurowane w $y_k$ lub powyżej nie mogą być zignorowane. Niech $b_k$ będzie liczbą obserwacji cenzurowanych prawostronnie w $y_k + 0$ lub później. Zauważmy, że jeśli $b_k = 0$, to $y_{max} = y_k$.

Ostatnią ważną wielkością jest $r_i$, określana jako liczba "narażonych na ryzyko" (at risk) w $y_i$. Myśląc w kategoriach badania śmiertelności, zbiór ryzyka (risk set) obejmuje osoby, które są pod obserwacją w tym wieku. Obejmuje on wszystkich, którzy umierają w tym wieku lub później, oraz wszystkich, którzy są cenzurowani w tym wieku lub później. Formalnie mamy następującą definicję.

**Definicja 14.8** Liczba narażonych na ryzyko, $r_i$ w obserwacji $y_i$ to
$$
r_i = \begin{cases} n, & i=1 \\ r_{i-1} - s_{i-1} - b_{i-1}, & i=2, 3, \dots, k+1. \end{cases}
$$
Ten wzór odzwierciedla fakt, że liczba narażonych na ryzyko w $y_i$ jest liczbą w $y_{i-1}$ pomniejszoną o $s_{i-1}$ dokładnych obserwacji w $y_{i-1}$ i $b_{i-1}$ cenzurowanych obserwacji w $[y_{i-1}, y_i)$. Zauważmy, że $b_k = r_k - s_k$, a zatem $r_{k+1} = 0$.

Poniższy przykład liczbowy ilustruje te idee.

---
**Przykład 14.5**

Określ liczby narażonych na ryzyko dla danych w Tabeli 14.8. Obserwacje oznaczone gwiazdką (*) zostały ocenzurowane przy tej wartości.

**Tabela 14.8** Surowe dane dla Przykładu 14.5.
1 2 3* 4 4 4* 4* 5 7* 8
8 8 9 9 9 9 10* 12 12 15*

Pierwszym krokiem jest podsumowanie danych przy użyciu wcześniej zdefiniowanej notacji. Podsumowane wartości i obliczenia zbioru ryzyka pojawiają się w Tabeli 14.9. Dodanie wartości w kolumnach $s$ i $b$ potwierdza, że jest $n=20$ obserwacji. Zatem, $r_1 = n = 20$. Gdyby dostępna była tylko ta tabela, fakt, że $b_7=1$ oznaczałby, że istnieje jedna ocenzurowana wartość większa niż 12, największa obserwowana wartość. Wartość $y_{max}=15$ wskazuje, że ta obserwacja została ocenzurowana w punkcie 15.

**Tabela 14.8** Surowe dane dla Przykładu 14.5.
|   |   |   |   |   |   |   |   |   |   |
|---|---|---|---|---|---|---|---|---|---|
| $1$ | $2$ | $3^*$ | $4$ | $4$ | $4^*$ | $4^*$ |$5$ | $7^*$ | $8$ |
| $8$ | $8$ | $9$ | $9$ | $9$ | $9$ | $10^*$ | $12$ | $12$ | $15^*$ |


**Tabela 14.9** Dane dla Przykładu 14.5.
| $i$ | $y_i$ | $s_i$ | $b_i$ | $r_i$ |
|---|---|---|---|---|
| $0$ | $0$ | — | — | — |
| $1$ | $1$ | $1$ | $0$ | $20$ |
| $2$ | $2$ | $1$ | $1$ | $20 - 1 - 0 = 19$ |
| $3$ | $4$ | $2$ | $2$ | $19 - 1 - 1 = 17$ |
| $4$ | $5$ | $1$ | $1$ | $17 - 2 - 2 = 13$ |
| $5$ | $8$ | $3$ | $0$ | $13 - 1 - 1 = 11$ |
| $6$ | $9$ | $4$ | $1$ | $11 - 3 - 0 = 8$ |
| $7$ | $12$ | $2$ | $1$ | $8 - 4 - 1 = 3$ |
| max | 15 | — | — | $3 - 2 - 1 = 0$ |

Jak zauważono wcześniej, obliczenie zbioru ryzyka nie wymaga dokładnych wartości cenzurowanych obserwacji, a jedynie przedziału, w którym się znajdują. Na przykład, gdyby ocenzurowana obserwacja 7 była przy wartości 6, wynikowe liczby narażonych na ryzyko nie zmieniłyby się (ponieważ $b_4$ nadal równałoby się jeden).

---
Należy zauważyć, że jeśli nie ma cenzurowania, tak że $b_i = 0$ dla wszystkich $i$, to dane są kompletne i można użyć technik z sekcji 14.1. Jako takie, podejście z tej sekcji można uznać za uogólnienie.

Teraz przedstawimy heurystyczne wyprowadzenie dobrze znanego uogólnienia empirycznej funkcji dystrybucji. Estymator ten jest nazywany **estymatorem Kaplana-Meiera** lub **estymatorem iloczynu granicznego**.

Aby kontynuować, najpierw przedstawimy kilka podstawowych faktów dotyczących rozkładu dyskretnej zmiennej losowej $Y$, powiedzmy, ze wsparciem na punktach $y_1 < y_2 < \dots < y_k$. Niech $p(y_j) = Pr(Y = y_j)$, a wtedy funkcja przeżycia jest (gdzie $i|y_i > y$ oznacza wzięcie sumy lub iloczynu po wszystkich wartościach $i$, dla których $y_i > y$)

$$S(y) = Pr(Y > y) = \sum_{i|y_i>y} p(y_i).$$

Ustawiając $y = y_j$ dla $j < k$, mamy

$$S(y_j) = \sum_{i=j+1}^k p(y_i),$$

a $S(y_k) = 0$. Mamy również $S(y_0) = 1$ z definicji $y_0$.

**Definicja 14.9** Dyskretna funkcja intensywności awarii (failure rate function) to

$$h(y_j) = Pr(Y = y_j|Y \ge y_j) = p(y_j)/S(y_{j-1}), \quad j = 1, 2, \dots, k.$$

Zatem,

$$h(y_j) = \frac{S(y_{j-1}) - S(y_j)}{S(y_{j-1})} = 1 - \frac{S(y_j)}{S(y_{j-1})},$$

co implikuje, że $S(y_j)/S(y_{j-1}) = 1 - h(y_j)$. Stąd,

$$S(y_j) = \frac{S(y_j)}{S(y_0)} = \prod_{i=1}^j \frac{S(y_i)}{S(y_{i-1})} = \prod_{i=1}^j [1 - h(y_i)].$$

Ponadto, $p(y_1) = h(y_1)$, a dla $j = 2, 3, \dots, k$,

$$p(y_j) = h(y_j)S(y_{j-1}) = h(y_j)\prod_{i=1}^{j-1} [1 - h(y_i)].$$

Heurystyczne wyprowadzenie polega na traktowaniu $\lambda_j = h(y_j)$ dla $j = 1, 2, \dots, k$ jako nieznanych parametrów i estymowaniu ich za pomocą nieparametrycznego argumentu opartego na "maksymalnej wiarygodności". W celu uzyskania bardziej szczegółowej dyskusji, patrz Lawless [77]. Dla obecnych danych, $s_j$ niecenzurowanych obserwacji w $y_j$ wnosi $p(y_j)$ do funkcji wiarygodności, gdzie $p(y_1) = \lambda_1$ i 

$$p(y_j) = \lambda_j \prod_{i=1}^{j-1}(1 - \lambda_i), \quad j = 2, 3, \dots, k.$$

Każda z $b_j$ cenzurowanych obserwacji wnosi

$$S(y_j) = \prod_{i=1}^j (1 - \lambda_i), \quad j = 1, 2, \dots, k-1,$$

do funkcji wiarygodności (przypomnijmy, że $S(y) = S(y_j)$ dla $y_j \le y < y_{j+1}$), a $b_k$ cenzurowanych obserwacji w $y_k$ lub powyżej wnosi $S(y_k) = \prod_{i=1}^k(1 - \lambda_i)$.

Funkcja wiarygodności jest tworzona przez branie iloczynów wszystkich wkładów (przy założeniu niezależności wszystkich punktów danych), a mianowicie

$$L(\lambda_1, \lambda_2, \dots, \lambda_k) = \prod_{j=1}^k \{[p(y_j)]^{s_j}[S(y_j)]^{b_j}\},$$

co, w terminach $\lambda_j$, staje się

$$\begin{align*}
L(\lambda_1, \lambda_2, \dots, \lambda_k) 
&= \lambda_1^{s_1} \left\{ \prod_{j=2}^k \left[ \lambda_j \prod_{i=1}^{j-1}(1 - \lambda_i) \right]^{s_j} \right\} \prod_{j=1}^k \left[ \prod_{i=1}^j(1 - \lambda_i) \right]^{b_j} \\
&= \left(\prod_{j=1}^k \lambda_j^{s_j}\right) \left[ \prod_{j=2}^k \prod_{i=1}^{j-1}(1 - \lambda_i)^{s_j} \right] \prod_{j=1}^k \prod_{i=1}^j (1 - \lambda_i)^{b_j} \\
&= \left(\prod_{j=1}^k \lambda_j^{s_j}\right) \left[ \prod_{i=1}^{k-1} \prod_{j=i+1}^k (1 - \lambda_i)^{s_j} \right] \prod_{i=1}^k \prod_{j=i}^k (1 - \lambda_i)^{b_j}, 
\end{align*}$$

gdzie ostatnia linia wynika z zamiany kolejności mnożenia w każdym z dwóch podwójnych iloczynów. Zatem,

$$\begin{align*}
L(\lambda_1, \lambda_2, \dots, \lambda_k) 
&= \left( \prod_{j=1}^k \lambda_j^{s_j} \right) (1-\lambda_k)^{b_k} \prod_{i=1}^{k-1} (1-\lambda_i)^{b_i + \sum_{m=i+1}^k (s_m+b_m)} \\
&= \left( \prod_{j=1}^k \lambda_j^{s_j} \right) (1-\lambda_k)^{b_k} \prod_{i=1}^{k-1} (1-\lambda_i)^{b_i + \sum_{m=i+1}^k (r_m-r_{m+1})} \\
&= \left( \prod_{j=1}^k \lambda_j^{s_j} \right) (1-\lambda_k)^{r_k-s_k} \prod_{i=1}^{k-1} (1-\lambda_i)^{b_i+r_{i+1}-r_{k+1}}.
\end{align*}$$

Zauważmy, że $r_{k+1} = 0$ i $b_i + r_{i+1} = r_i - s_i$. Stąd,

$$\begin{align*}
L(\lambda_1, \lambda_2, \dots, \lambda_k) 
&= \left( \prod_{j=1}^k \lambda_j^{s_j} \right) (1-\lambda_k)^{r_k-s_k} \prod_{i=1}^{k-1} (1-\lambda_i)^{r_i-s_i} \\
&= \prod_{j=1}^k \lambda_j^{s_j}(1 - \lambda_j)^{r_j-s_j}.
\end{align*}$$

Ta funkcja wiarygodności ma wygląd iloczynu funkcji wiarygodności z rozkładu dwumianowego. Oznacza to, że jest to ta sama funkcja wiarygodności, jak gdyby $s_1, s_2, \dots, s_k$ były realizacjami $k$ niezależnych obserwacji z rozkładu dwumianowego z parametrami $m=r_j$ i $q=\lambda_j$. "Estymator maksymalnej wiarygodności" $\hat{\lambda}_j$ dla $\lambda_j$ uzyskuje się, biorąc logarytmy, a mianowicie

$$ l(\lambda_1, \lambda_2, \dots, \lambda_k) = \ln L(\lambda_1, \lambda_2, \dots, \lambda_k) = \sum_{j=1}^k [s_j\ln\lambda_j + (r_j - s_j)\ln(1-\lambda_j)], $$

co implikuje, że

$$ \frac{\partial l}{\partial \lambda_j} = \frac{s_j}{\lambda_j} - \frac{r_j-s_j}{1-\lambda_j}, \quad j=1, 2, \dots, k. $$

Przyrównanie tego ostatniego wyrażenia do zera daje $\hat{\lambda}_j = s_j/r_j$.

Dla $y = y_k$, **estymator Kaplana-Meiera** $S_n(y)$ dla $S(y)$ uzyskuje się przez zastąpienie $\lambda_j$ przez $\hat{\lambda}_j = s_j/r_j$ wszędzie, gdzie się pojawia. Zauważając, że $S(y) = S(y_j)$ dla $y_j \le y < y_{j+1}$, wynika z tego, że

$$ S_n(y) = \begin{cases} 
1, & y < y_1, \\ 
\prod\limits_{i=1}^j (1-\hat{\lambda}_i) = \prod\limits_{i=1}^j (1 - \frac{s_i}{r_i}), & y_j \le y < y_{j+1}, \quad j=1, 2, \dots, k-1, \\
\prod\limits_{i=1}^k (1-\hat{\lambda}_i) = \prod\limits_{i=1}^k (1 - \frac{s_i}{r_i}), & y_k \le y < y_{max}. \end{cases} $$

Można to zapisać bardziej zwięźle jako $S_n(y) = \prod_{i|y_i \le y} (1 - \hat{\lambda}_i)$ dla $y < y_{max}$. Gdy $y_{max} = y_k$, należy interpretować $y_k \le y < y_{max}$ jako $y=y_k$.

**Tabela 14.10** Estymatory Kaplana-Meiera dla Przykładu 14.6.
| $i$ | $y_i$ | $s_i$ | $r_i$ | $ \hat{S}_n(y_i) $ |
|---|---|---|---|---|
| $1$ | $1$ | $1$ | $20$ | $1 - 1/20 = 0.950$ |
| $2$ | $2$ | $1$ | $19$ | $0.95(1 - 1/19) = 0.900$ |
| $3$ | $4$ | $2$ | $17$ | $0.9(1 - 2/17) = 0.794$ |
| $4$ | $5$ | $1$ | $13$ | $0.794(1 - 1/13) = 0.733$ |
| $5$ | $8$ | $3$ | $11$ | $0.733(1 - 3/11) = 0.533$ |
| $6$ | $9$ | $4$ | $8$ | $0.533(1 - 4/8) = 0.267$ |
| $7$ | $12$ | $2$ | $3$ | $0.267(1 - 2/3) = 0.089$ |

---
**Przykład 14.6**

Skonstruuj estymator Kaplana-Meiera dla $S(y)$, używając danych z Przykładu 14.5. Wskaż, jak zmieniłaby się odpowiedź, gdyby $s_7 = 3$ i $b_7 = 0$.

Obliczenia przedstawiono w Tabeli 14.10. Szacowana funkcja przeżycia to

$$ S_{20}(y) = \begin{cases} 1, & y < 1, \\ 0.950, & 1 \le y < 2, \\ 0.900, & 2 \le y < 4, \\ 0.794, & 4 \le y < 5, \\ 0.733, & 5 \le y < 8, \\ 0.533, & 8 \le y < 9, \\ 0.267, & 9 \le y < 12, \\ 0.089, & 12 \le y < 15. \end{cases} $$

Ze zmianą wartości mamy $y_{max} = y_7 = 12$ i $S_{20}(y) = 0.267(1-3/3) = 0$ dla $y=12$.

---
Teraz omawiamy estymację dla $y \ge y_{max}$. Po pierwsze, zauważmy, że jeśli $s_k = r_k$ (brak cenzurowanych obserwacji w $y_k$), to $S_n(y_k) = 0$ i $S_n(y) = 0$ dla $y \ge y_k$ jest oczywiście (jedynym) oczywistym wyborem. Jeśli jednak $S_n(y_k) > 0$, jak w poprzednim przykładzie, nie ma danych empirycznych do oszacowania $S(y)$ dla $y \ge y_{max}$, i potrzebne są estymacje ogona dla $y \ge y_{max}$ (często nazywane **korektami ogona**). Istnieją trzy popularne ekstrapolacje:
* **Korekta ogona Efrona** zakłada, że $S_n(y) = 0$ dla $y \ge y_{max}$.
* **Klein i Moeschberger** zakładają, że $S_n(y) = S_n(y_k)$ dla $y_k \le y < \gamma$ i $S_n(y) = 0$ dla $y \ge \gamma$, gdzie $\gamma > y_{max}$ jest prawdopodobnym górnym limitem dla podstawowej zmiennej losowej. Na przykład, w badaniu śmiertelności ludzkiej, limit ten może wynosić 120 lat.
* **Wykładnicza korekta ogona Browna, Hollandera i Korwara** zakłada, że $S_n(y_{max}) = S_n(y_k)$ oraz że $S_n(y) = e^{-\hat{\beta}y}$ dla $y \ge y_{max}$. Przy $y = y_{max}$, $\hat{\beta} = -\ln S_n(y_k)/y_{max}$, a zatem
$$ S_n(y) = e^{y[\ln S_n(y_k)]/y_{max}} = [S_n(y_k)]^{y/y_{max}}, \quad y \ge y_{max}. $$

---
**Przykład 14.7**

Zastosuj wszystkie trzy metody korekty ogona do danych użytych w Przykładzie 14.6. Załóż, że $\gamma = 22$.

Metoda Efrona daje $S_{20}(y) = 0, y \ge 15$. Z $\gamma = 22$, metoda Kleina i Moeschbergera daje $S_{20}(y) = 0.089, 15 \le y < 22$, i $S_{20}(y) = 0, y \ge 22$. Wykładnicza korekta ogona daje $S_{20}(y) = (0.089)^{y/15}, y \ge 15$.

---
Zauważmy, że jeśli nie ma cenzurowania ($b_i = 0$ dla wszystkich $i$), to $r_{i+1} = r_i - s_i$, a dla $y_j \le y < y_{j+1}$

$$ S_n(y) = \prod_{i=1}^j \frac{r_i - s_i}{r_i} = \prod_{i=1}^j \frac{r_{i+1}}{r_i} = \frac{r_{j+1}}{r_1}. $$

W tym przypadku $r_{j+1}$ jest liczbą obserwacji przekraczających $y$ i $r_1 = n$. Zatem, bez cenzurowania, estymator Kaplana-Meiera sprowadza się do estymatora empirycznego z poprzedniej sekcji.

Alternatywą dla estymatora Kaplana-Meiera jest czasami używany **estymator Nelsona-Åalena**. Aby go zmotywować, zauważmy, że jeśli $S(y)$ jest funkcją przeżycia rozkładu ciągłego z intensywnością awarii $h(y)$, to $-\ln S(y) = H(y) = \int_0^y h(t)dt$ nazywa się **skumulowaną funkcją intensywności awarii**. Dyskretny odpowiednik, w obecnym kontekście, jest dany przez $\sum\limits_{i|y_i \le y} \lambda_i$, co intuicyjnie można oszacować, zastępując $\lambda_i$ przez jej estymator $\hat{\lambda}_i = s_i/r_i$. Estymator Nelsona-Åalena dla $H(y)$ jest więc zdefiniowany dla $y < y_{max}$ jako

$$ \hat{H}(y) = \begin{cases} 
0, & y < y_1, \\ 
\sum\limits_{i=1}^j \hat{\lambda}_i = \sum\limits_{i=1}^j \frac{s_i}{r_i}, & y_j \le y < y_{j+1}, \quad j=1, 2, \dots, k-1, \\ 
\sum\limits_{i=1}^k \hat{\lambda}_i = \sum\limits_{i=1}^k \frac{s_i}{r_i}, & y_k \le y < y_{max}. \end{cases} $$

Oznacza to, że $\hat{H}(y) = \sum\limits_{i|y_i \le y} \hat{\lambda}_i$ dla $y < y_{max}$, a estymator Nelsona-Åalena funkcji przeżycia to $\hat{S}(y) = \exp(-\hat{H}(y))$. Notacja pod sumą wskazuje, że wartości $\hat{\lambda}_i$ powinny być uwzględniane tylko wtedy, gdy $y_i \le y$. Dla $y \ge y_{max}$ sytuacja jest podobna do tej z estymatorem Kaplana-Meiera, w tym sensie, że należy zastosować korektę ogona omówionego wcześniej typu. Zauważmy, że w przeciwieństwie do estymatora Kaplana-Meiera, $\hat{S}(y_k) > 0$, więc korekta ogona jest zawsze potrzebna.

**Tabela 14.11** Estymatory Nelsona-Åalena dla Przykładu 14.8.
| $i$ | $y_i$ | $s_i$ | $r_i$ | $ \hat{H}_n(y_i) $ |
|---|---|---|---|---|
| $1$ | $1$ | $1$ | $20$ | $1/20 = 0.050$ |
| $2$ | $2$ | $1$ | $19$ | $0.05 + 1/19 = 0.103$ |
| $3$ | $4$ | $2$ | $17$ | $0.103 + 2/17 = 0.220$ |
| $4$ | $5$ | $1$ | $13$ | $0.220 + 1/13 = 0.297$ |
| $5$ | $8$ | $3$ | $11$ | $0.297 + 3/11 = 0.570$ |
| $6$ | $9$ | $4$ | $8$ | $0.570 + 4/8 = 1.070$ |
| $7$ | $12$ | $2$ | $3$ | $1.070 + 2/3 = 1.737$ |

---
**Przykład 14.8**

Określ estymatory Nelsona-Åalena dla danych z Przykładu 14.6. Nadal zakładaj $\gamma = 22$.

Estymacje skumulowanej funkcji intensywności podano w Tabeli 14.11. Estymowana funkcja przeżycia to

$$ \hat{S}_{20}(y) = \begin{cases} 1, & y < 1, \\ e^{-0.050} = 0.951, & 1 \le y < 2, \\ e^{-0.103} = 0.902, & 2 \le y < 4, \\ e^{-0.220} = 0.803, & 4 \le y < 5, \\ e^{-0.297} = 0.743, & 5 \le y < 8, \\ e^{-0.570} = 0.566, & 8 \le y < 9, \\ e^{-1.070} = 0.343, & 9 \le y < 12, \\ e^{-1.737} = 0.176, & 12 \le y < 15. \end{cases} $$

Jeśli chodzi o korektę ogona, metoda Efrona daje $S_{20}(y) = 0, y \ge 15$. Metoda Kleina i Moeschbergera daje $S_{20}(y) = 0.176, 15 \le y < 22$, i $S_{20}(y) = 0, y \ge 22$. Wykładnicza korekta ogona daje $S_{20}(y) = (0.176)^{y/15}, y \ge 15$.

---
Aby ocenić jakość obu estymatorów, rozważymy teraz estymację wariancji. Przypomnijmy, że dla $y < y_{max}$, estymator Kaplana-Meiera można wyrazić jako

$$ S_n(y) = \prod_{i|y_i \le y} (1-\hat{\lambda}_i), $$

co jest funkcją $\hat{\lambda}_i$. Zatem, aby oszacować wariancję $S_n(y)$, najpierw potrzebujemy macierzy kowariancji $(\hat{\lambda}_1, \hat{\lambda}_2, \dots, \hat{\lambda}_k)$. Szacujemy ją z "funkcji wiarygodności", używając standardowych metod wiarygodności. Przypomnijmy, że

$$ L(\lambda_1, \lambda_2, \dots, \lambda_k) = \prod\limits_{j=1}^{k} \lambda_j^{s_j} (1 - \lambda_j)^{r_j - s_j}, $$

a zatem $l = \ln L$ spełnia

$$l(\lambda_1, \lambda_2, \dots, \lambda_k) = \sum_{j=1}^{k} [s_j \ln \lambda_j + (r_j - s_j) \ln(1 - \lambda_j)].$$

Zatem,

$$\frac{\partial l}{\partial \lambda_i} = \frac{s_i}{\lambda_i} - \frac{r_i - s_i}{1 - \lambda_i}, \quad i = 1, 2, \dots, k,$$

i

$$
\frac{\partial^2 l}{\partial \lambda_i^2} = -\frac{s_i}{\lambda_i^2} - \frac{r_i - s_i}{(1 - \lambda_i)^2},
$$

co, po zastąpieniu $ \lambda_i $ przez $ \hat{\lambda}_i = s_i/r_i $, staje się

$$
\left. \frac{\partial^2 l}{\partial \lambda_i^2} \right|_{\lambda_i = \hat{\lambda}_i} = -\frac{s_i}{\hat{\lambda}_i^2} - \frac{r_i - s_i}{(1 - \hat{\lambda}_i)^2} = -\frac{r_i^3}{s_i(r_i - s_i)}.
$$

Dla $ i \neq m $,

$$
\frac{\partial^2 l}{\partial \lambda_i \partial \lambda_m} = 0.
$$

Obserwowana informacja, obliczona dla estymatora największej wiarygodności, jest więc macierzą diagonalną, która po odwróceniu daje estymaty

$$
\text{Var}(\hat{\lambda}_i) \doteq \frac{s_i(r_i - s_i)}{r_i^3} = \frac{\hat{\lambda}_i(1 - \hat{\lambda}_i)}{r_i}, \quad i = 1, 2, \dots, k,
$$

i

$$
\text{Cov}(\hat{\lambda}_i, \hat{\lambda}_m) \doteq 0, \quad i \neq m.
$$

Wyniki te wynikają również bezpośrednio z dwumianowej postaci funkcji wiarygodności.
Wracając do rozważanego problemu, metoda delta daje przybliżoną wariancję
$ f(\hat{\theta}) $ jako $ \text{Var}[f(\hat{\theta})] \doteq [f'(\hat{\theta})]^2 \text{Var}(\hat{\theta}) $, dla estymatora $ \hat{\theta} $.
Aby kontynuować, zauważmy, że

$$
\ln S_n(y) = \sum_{i|y_i \le y} \ln(1 - \hat{\lambda}_i),
$$

a ponieważ zakłada się, że $ \hat{\lambda}_i $ są w przybliżeniu nieskorelowane,

$$
\text{Var}[\ln S_n(y)] \doteq \sum_{i|y_i \le y} \text{Var}[\ln(1 - \hat{\lambda}_i)].
$$

Wybór $f(x) = \ln(1-x)$ daje

$$\text{Var}[\ln(1 - \hat{\lambda}_i)] \doteq \frac{\text{Var}(\hat{\lambda}_i)}{(1 - \hat{\lambda}_i)^2} = \frac{\hat{\lambda}_i}{r_i(1 - \hat{\lambda}_i)} = \frac{s_i}{r_i(r_i - s_i)},$$

co implikuje, że

$$\text{Var}[\ln S_n(y)] \doteq \sum_{i|y_i \le y} \frac{s_i}{r_i(r_i - s_i)}.$$

Ponieważ $ S_n(y) = \exp[\ln S_n(y)] $, metoda delta z $f(x) = \exp(x)$ daje

$$\text{Var}[S_n(y)] \doteq \{\exp[\ln S_n(y)]\}^2 \text{Var}[\ln S_n(y)] = [S_n(y)]^2 \text{Var}[\ln S_n(y)],$$

Ostatecznie, estymata wariancji jest dana przez

$$ \widehat{\text{Var}}[S_n(y)] = [S_n(y)]^2 \sum_{i|y_i \le y} \frac{s_i}{r_i(r_i-s_i)}. \quad(14.2) $$

Równanie (14.2) jest znane jako **aproksymacja Greenwooda** wariancji $S_n(y)$ i wiadomo, że często zaniża prawdziwą wariancję.

Jeśli nie ma cenzurowania, i przyjmiemy $y_j \le y < y_{j+1}$, aproksymacja Greenwooda daje

$$\widehat{\text{Var}}[S_n(y)] = \widehat{\text{Var}}[S_n(y_j)] = [S_n(y_j)]^2 \sum_{i=1}^{j} \frac{s_i}{r_i(r_i - s_i)},$$

co można wyrazić (używając $r_{i+1} = r_i - s_i$ z powodu braku cenzurowania) jako

$$\begin{align*}
\widehat{\text{Var}}[S_n(y)] 
&= [S_n(y_j)]^2 \sum_{i=1}^{j} \left( \frac{1}{r_i - s_i} - \frac{1}{r_i} \right) \\
&= [S_n(y_j)]^2 \sum_{i=1}^{j} \left( \frac{1}{r_{i+1}} - \frac{1}{r_i} \right).
\end{align*}$$

Ponieważ $r_1 = n$, ta suma teleskopuje się, dając

$$\begin{align*}
\widehat{\text{Var}}[S_n(y)] &= [S_n(y_j)]^2 \left( \frac{1}{r_{j+1}} - \frac{1}{r_1} \right) \\
&= [S_n(y_j)]^2 \left[ \frac{1}{n S_n(y_j)} - \frac{1}{n} \right] \\
&= \frac{S_n(y_j)[1 - S_n(y_j)]}{n},
\end{align*}$$

co jest tą samą estymatą, jaką uzyskano w Sekcji 14.1, ale wyprowadzoną bez użycia metody delta.

Zauważamy, że w przypadku, gdy $r_k = s_k$ (tj. $S_n(y_k) = 0$), aproksymacja Greenwooda nie może być użyta do oszacowania wariancji $S_n(y_k)$. W tym przypadku, $r_k - s_k$ jest często zastępowane przez $r_k$ w mianowniku.

Przechodząc teraz do estymatora Nelsona-Åalena, zauważamy, że 

$$\hat{H}(y) = \sum_{i|y_i \le y} \hat{\lambda}_i,$$

i to samo rozumowanie co dla Kaplana-Meiera implikuje, że $\text{Var}[\hat{H}(y)] \doteq \sum_{i|y_i \le y} \text{Var}(\hat{\lambda}_i)$, dając estymatę

$$ \widehat{\text{Var}}[\hat{H}(y)] = \sum_{i|y_i \le y} \frac{s_i(r_i-s_i)}{r_i^3}, \quad(14.3) $$
nazywa się to **estymatą Kleina**. Powszechnie stosowana alternatywna estymata, przypisywana Åalenowi, jest uzyskiwana przez zastąpienie $r_i - s_i$ przez $r_i$ w liczniku.

Zazwyczaj bardziej interesuje nas $S(t)$ niż $H(t)$. Ponieważ $\hat{S}(y) = \exp(-\hat{H}(y))$, metoda delta z $f(x) = e^{-x}$ daje estymatę wariancji funkcji przeżycia Kleina

$$\text{Var}[\hat{S}(y)] \doteq [\exp(-\hat{H}(y))]^2 \widehat{\text{Var}}[\hat{H}(y)],$$

to znaczy, oszacowana wariancja wynosi

$$\widehat{\text{Var}}[\hat{S}(y)] = [\hat{S}(y)]^2 \sum_{i|y_i \le y} \frac{s_i(r_i - s_i)}{r_i^3}, \quad y < y_{\text{max}}.$$

---
**Przykład 14.9**

Dla danych z Przykładu 14.5, oszacuj wariancję estymatorów Kaplana-Meiera dla $S(2)$ i $S(9)$, oraz estymatora Nelsona-Åalena dla $S(2)$.

Dla estymatorów Kaplana-Meiera,

$$ \widehat{\text{Var}}[S_{20}(2)] = [S_{20}(2)]^2\left[\frac{1}{20(19)} + \frac{1}{19(18)}\right] = (0.90)^2(0.00556) = 0.0045, $$

$$ \widehat{\text{Var}}[S_{20}(9)] = [S_{20}(9)]^2\left[\frac{1}{20(19)} + \dots + \frac{4}{8(4)}\right] = (0.267)^2(0.17890) = 0.01271, $$

a dla estymatora Nelsona-Åalena,

$$ \widehat{\text{Var}}[\hat{S}(2)] = [\hat{S}(2)]^2\left[\frac{1(19)}{20^3} + \frac{1(18)}{19^3}\right] = (0.902)^2(0.00500) = 0.00407. $$

---
Estymacje wariancji dla $y \ge y_{\text{max}}$ zależą od zastosowanej korekty ogona. Metoda Efrona daje estymatę równą 0, co nie jest interesujące w obecnym kontekście. Dla wykładniczej korekty ogona w przypadku Kaplana-Meiera, mamy dla $y \ge y_{\text{max}}$, $S_n(y) = S_n(y_k)^{y/y_{\text{max}}}$, a metoda delta z $f(x) = x^{y/y_{\text{max}}}$ daje

$$\widehat{\text{Var}}[S_n(y)] = \left[ \frac{y}{y_{\text{max}}} S_n(y_k)^{\frac{y}{y_{\text{max}}}-1} \right]^2 \widehat{\text{Var}}[S_n(y_k)]$$

$$= \left( \frac{y}{y_{\text{max}}} \right)^2 [S_n(y)]^2 \sum_{i=1}^{k} \frac{s_i}{r_i(r_i - s_i)}$$

$$= \left( \frac{y}{y_{\text{max}}} \right)^2 \left[ \frac{S_n(y)}{S_n(y_k)} \right]^2 \widehat{\text{Var}}[S_n(y_k)].$$

Metody wiarygodności zazwyczaj prowadzą do przybliżonej asymptotycznej normalności estymatorów, co jest prawdą również dla estymatorów Kaplana-Meiera i Nelsona-Åalena. Używając wyników z Przykładu 14.9, przybliżony 95% przedział ufności dla $S(9)$ jest dany przez

$$S_{20}(9) \pm 1.96\sqrt{\{\text{Var}[S_{20}(9)]\}} = 0.267 \pm 1.96\sqrt{0.01271} = (0.046, 0.488).$$

Dla $S(2)$, estymator Nelsona-Åalena daje przedział ufności

$$0.90246 \pm 1.96\sqrt{0.00407} = (0.77740, 1.02753),$$

podczas gdy ten oparty na estymatorze Kaplana-Meiera wynosi

$$0.90 \pm 1.96\sqrt{0.0045} = (0.76852, 1.03148).$$

Oczywiście oba przedziały ufności dla $S(2)$ są niezadowalające, ponieważ oba zawierają wartości większe niż 1.

Alternatywne podejście można skonstruować w następujący sposób, używając jako przykładu estymatora Kaplana-Meiera.

Niech $Y = \ln[-\ln S_n(y)]$. Używając metody delta, wariancję $Y$ można przybliżyć w następujący sposób. Interesująca nas funkcja to $f(x) = \ln(-\ln x)$. Jej pochodna to

$$f'(x) = \frac{1}{-\ln x} \frac{-1}{x} = \frac{1}{x \ln x}.$$

Zgodnie z metodą delta,

$$\widehat{\text{Var}}(Y) = \{f'[S_n(y)]\}^2 \text{Var}[S_n(y)] = \frac{\text{Var}[S_n(y)]}{[S_n(y) \ln S_n(y)]^2}.$$

Wówczas, przybliżony 95% przedział ufności dla $\theta = \ln[-\ln S(y)]$ wynosi

$$\ln[-\ln S_n(y)] \pm 1.96 \frac{\sqrt{\widehat{\text{Var}}[S_n(y)]}}{S_n(y) \ln S_n(y)}.$$

Ponieważ $S(y) = \exp(-e^Y)$, obliczenie każdego z końców tego wzoru dostarcza przedziału ufności dla $S(y)$. Dla górnej granicy mamy (gdzie $\hat{v} = \widehat{\text{Var}}[S_n(y)]$)

$$\exp \left\{ -e^{\ln[-\ln S_n(y)] + 1.96\sqrt{\hat{v}}/[S_n(y)\ln S_n(y)]} \right\} = \exp \left\{ [\ln S_n(y)] e^{1.96\sqrt{\hat{v}}/[S_n(y)\ln S_n(y)]} \right\}$$

$$= S_n(y)^U, \quad U = \exp \left[ \frac{1.96\sqrt{\hat{v}}}{S_n(y) \ln S_n(y)} \right].$$

Podobnie, dolna granica to $S_n(y)^{1/U}$. Ten przedział zawsze będzie mieścił się w zakresie 0–1 i jest określany jako **przekształcony logarytmicznie przedział ufności**.

---
**Przykład 14.10**

Skonstruuj 95% przekształcony logarytmicznie przedział ufności dla $S(2)$ w Przykładzie 14.9 na podstawie estymatora Kaplana–Meiera.

W tym przypadku, $S_{20}(2) = 0.9$, a $U = \exp \left[ \frac{1.96(0.06708)}{0.90 \ln 0.90} \right] = 0.24994$. Dolna granica to $(0.90)^{1/U} = 0.65604$, a górna granica to $(0.90)^U = 0.97401$.

---
Dla estymatora Nelsona–Åalena, podobny przekształcony logarytmicznie przedział ufności dla $H(y)$ ma końce $\hat{H}(y)U$, gdzie $U = \exp[\pm 1.96 \sqrt{\widehat{\text{Var}}[\hat{H}(y)]} / \hat{H}(y)]$. Wykładniczenie ujemnych wartości tych końców daje odpowiadający przedział dla $S(y)$.

---
**Przykład 14.11**

Skonstruuj 95% liniowe i przekształcone logarytmicznie przedziały ufności dla $H(2)$ w Przykładzie 14.9 na podstawie estymatora Nelsona–Åalena. Następnie skonstruuj 95% przekształcony logarytmicznie przedział ufności dla $S(2)$.

Z (14.3), 95% liniowy przedział ufności dla $H(2)$ wynosi

$$0.10263 \pm 1.96 \sqrt{\frac{1(19)}{(20)^3} + \frac{1(18)}{(19)^3}} = 0.10263 \pm 1.96\sqrt{0.00500}$$

$$= (-0.03595, 0.24121)$$

co obejmuje wartości ujemne. Dla przekształconego logarytmicznie przedziału ufności mamy

$$U = \exp[\pm 1.96 \sqrt{0.00500}/0.10263]$$

$$= 0.25916 \text{ i } 3.8586.$$

Przedział rozciąga się od

$$(0.10263)(0.25916) = 0.02660 \quad \text{do} \quad (0.10263)(3.85865) = 0.39601.$$

Odpowiadający przedział dla $S(2)$ rozciąga się od

$$\exp(-0.39601) = 0.67300 \quad \text{do} \quad \exp(-0.02660) = 0.97375.$$

---