# Rozdział 3: Estymacja metodą największej wiarygodności

## 3.7 Bootstrap

W niektórych zastosowaniach, analityczne wyniki są trudne do uzyskania, a metody numeryczne muszą być zastosowane. W tym względzie szczególnie użyteczne wydaje się przedstawione w tej sekcji podejście bootstrap.

### 3.7.1 Dokładność estymacji

Załóżmy, że mamy niezależne zmienne losowe $Y_1, Y_2, \dots, Y_n$ o dystrybuancie $F$ i jesteśmy zainteresowani ich wykorzystaniem do oszacowania pewnej wielkości $\theta(F)$ związanej z $F$. Estymator

$$
\hat{\theta} = g(Y_1, Y_2, \dots, Y_n)
$$

jest dostępny dla $\theta(F)$. Jego dokładność można zmierzyć błędem średniokwadratowym

$$
MSE(F) = E_F \left[ (g(Y_1, Y_2, \dots, Y_n) - \theta(F))^2 \right]
$$

gdzie indeks dolny "F" w $E_F$ jawnie wskazuje, że matematyczna wartość oczekiwana jest obliczana względem F.

**Przykład 3.7.1** Rozważmy przypadek $\theta(F) = E_F[Y_1]$. Ponieważ $F$ jest (przynajmniej częściowo) nieznane, niemożliwe jest obliczenie $MSE(F)$. W tym konkretnym przypadku, używając estymatora

$$
\widehat{E_F[Y_1]} = \bar{Y}_n = \frac{1}{n} \sum_{i=1}^{n} Y_i
$$

mamy

$$
MSE(F) = E_F \left[ (\bar{Y}_n - E_F[Y_1])^2 \right] = \text{Var}[\bar{Y}_n] = \frac{\text{Var}[Y_1]}{n}.
$$

Stąd istnieje naturalny estymator MSE

$$
\widehat{MSE}(F) = \frac{S_n^2}{n} \text{ gdzie } S_n^2 = \frac{1}{n-1} \sum_{i=1}^{n} (Y_i - \bar{Y}_n)^2
$$

jest nieobciążonym estymatorem wspólnej wariancji $Y_1, \dots, Y_n$.

W przypadku bardziej skomplikowanych statystyk, zwykle nie jest dostępne żadne rozwiązanie analityczne, a bootstrap może stanowić realne rozwiązanie, jak wyjaśniono dalej.

### 3.7.2 Zasada wtyczki (Plug-In Principle)

Zdefiniujmy empiryczny odpowiednik $F$ jako

$$
\hat{F}_n(x) = \frac{\#\{Y_i \text{ takie, że } Y_i \leq x\}}{n} = \frac{1}{n} \sum_{i=1}^{n} I[Y_i \leq x].
$$

Zatem, empiryczna funkcja dystrybucji $\hat{F}_n$ przypisuje równe prawdopodobieństwo $\frac{1}{n}$ każdemu z obserwowanych punktów danych $Y_1, \dots, Y_n$. Bootstrapowym oszacowaniem $MSE(F)$ jest

$$
MSE(\hat{F}_n) = E_{\hat{F}_n} \left[ (g(Y_1^*, Y_2^*, \dots, Y_n^*) - \theta(\hat{F}_n))^2 \right]
$$

gdzie zmienne losowe $Y_1^*, Y_2^*, \dots, Y_n^*$ są niezależne i mają wspólną dystrybuantę $\hat{F}_n$.

W niektórych przypadkach możliwe jest analityczne obliczenie oszacowań bootstrapowych, ale ogólnie konieczne jest ponowne próbkowanie (resampling).

**Przykład 3.7.2** Rozważmy ponownie przypadek $\theta(F) = E_F[Y_1]$. Oczywiście,

$$
\theta(\hat{F}_n) = \bar{y}_n = \frac{1}{n} \sum_{i=1}^{n} y_i
$$

odpowiada klasycznemu oszacowaniu parametru średniej. Bootstrapowe oszacowanie $MSE(F)$ jest wtedy równe

$$
MSE(\hat{F}_n) = E_{\hat{F}_n} \left[ \left( \frac{1}{n} \sum_{i=1}^{n} Y_i^* - \bar{y}_n \right)^2 \right]
$$

gdzie zmienne losowe $Y_i^*$ są niezależne i podlegają dystrybuancie $\hat{F}_n$. Mamy

$$
E_{\hat{F}_n} \left[ \frac{1}{n} \sum_{i=1}^{n} Y_i^* \right] = E_{\hat{F}_n}[Y_1^*] = \bar{y}_n.
$$

Stąd,

$$
MSE(\hat{F}_n) = \text{Var}_{\hat{F}_n} \left[ \frac{1}{n} \sum_{i=1}^{n} Y_i^* \right] = \frac{\text{Var}_{\hat{F}_n}[Y_1^*]}{n}.
$$

Ponieważ

$$
\text{Var}_{\hat{F}_n}[Y_1^*] = E_{\hat{F}_n}[(Y_1^* - \bar{y}_n)^2] = \frac{1}{n} \sum_{i=1}^{n} (y_i - \bar{y}_n)^2 = \hat{\sigma}_n^2
$$

$$
\Rightarrow MSE(\hat{F}_n) = \frac{\hat{\sigma}_n^2}{n} \approx \frac{S_n^2}{n} \text{ w dużych próbach.}
$$

Tak więc zasadniczo dochodzimy do tego samego wniosku co poprzednio.

### 3.7.3 Estymata Bootstrapowa

Ogólnie,

$$
MSE(\hat{F}_n) = \frac{1}{n^n} \sum_{i_1=1}^{n}\sum_{i_2=1}^{n} \dots \sum_{i_n=1}^{n} (g(y_{i_1}, y_{i_2}, \dots, y_{i_n}) - \theta(\hat{F}_n))^2.
$$

Obliczenie $MSE(\hat{F}_n)$ wymaga zatem sumowania $n^n$ składników, co staje się czasochłonne, a nawet niemożliwe do osiągnięcia, gdy $n$ rośnie. Dlatego $MSE(\hat{F}_n)$ jest aproksymowane za pomocą symulacji i uśredniania dużej liczby składników.

Idea nieparametrycznego bootstrapu polega na symulowaniu zbiorów niezależnych zmiennych losowych

$$
Y_1^{(b)}, Y_2^{(b)}, \dots, Y_n^{(b)}
$$

podlegających dystrybuancie $\hat{F}_n$, $b=1,2,\dots,B$. Można to zrobić, symulując $U_i \sim \mathcal{Uni}(0,1)$ i ustawiając

$$
Y_i^{(b)} = y_I \text{ gdzie } I = [nU_i] + 1.
$$

Następnie,

$$
MSE(\hat{F}_n) \approx \frac{1}{B} \sum_{b=1}^{B} (g(Y_1^{(b)}, Y_2^{(b)}, \dots, Y_n^{(b)}) - \theta(\hat{F}_n))^2
$$

dla pewnego wystarczająco dużego $B$.

### 3.7.4 Bootstrap Nieparametryczny a Parametryczny

W ujęciu nieparametrycznym, ponowne próbkowanie jest wykonywane z $\hat{F}_n$, czyli próbkowanie z obserwacji $y_1, \dots, y_n$ z powtórzeniami. Prowadzi to do rozkładu interesującej nas statystyki, z której można oszacować takie wielkości jak błędy standardowe. W bootstrapie parametrycznym, podstawowy rozkład jest szacowany na podstawie danych w modelu parametrycznym, to znaczy,

$$
F = F_{\theta}.
$$

Próbki bootstrapowe są następnie generowane przez próbkowanie z $F_{\hat{\theta}}$, a nie z $\hat{F}_n$.

### 3.7.5 Zastosowania

Użyjmy podejścia bootstrapowego, aby uzyskać 95% przedział ufności dla prawdopodobieństwa zgonu w ciągu jednego roku $q$. Przypomnijmy, że obserwacje składają się z 30 zgonów i 70 ocalałych, zebranych w wektorze wskaźników zgonów $Y_i = I[T_i \leq 1]$ (z 1 pojawiającą się 30 razy i 0 pojawiającą się 70 razy). Bez utraty ogólności, możemy uporządkować $Y_1, \dots, Y_{100}$ tak, aby wartości 1 pojawiły się najpierw w wektorze.

Następnie próbujemy 10 000 razy z powtórzeniami z tego wektora za pomocą funkcji `sample` dostępnej w oprogramowaniu statystycznym R. Dla każdego ponownego próbkowania danych, obliczamy prawdopodobieństwo sukcesu, uśredniając wartości ponownie próbkowanego wektora i przechowujemy wyniki. Przedział zaczynający się od 2.5% percentyla do 97.5% percentyla jest [0.21, 0.39]. Otrzymane wyniki okazały się być bardzo zbliżone do tych opartych na aproksymacji rozkładem normalnym dla dużych prób i dokładnym dwumianowym przedziale ufności, o których mowa wcześniej. Zauważmy, że zamiast ponownego próbkowania ze 100 obserwowanych wartości, inną możliwością jest generowanie liczby zgonów z rozkładu dwumianowego z parametrami 100 i $\hat{q} = 0.3$ za pomocą funkcji R `rbinom` i dzielenie wynikowych 10 000 realizacji przez 100. Rysunek 3.3 (górny panel) przedstawia histogram 10 000 wartości bootstrapowych dla rocznego prawdopodobieństwa zgonu, który wydaje się być zgodny z aproksymacją rozkładem normalnym dla dużych prób.

Funkcja `boot` z biblioteki R `boot` pozwala nam uzyskać różne wersje podejścia bootstrapowego. Średnia z dostępnej próby wynosi $\hat{q} = 0.3$. Różnica między $\hat{q}$ a średnią $\hat{q}^b$ z 10 000 próbek bootstrapowych wynosi

$$
\hat{q} - \frac{1}{10000} \sum_{b=1}^{10000} \hat{q}^b = -0.000279
$$

podczas gdy odpowiadający błąd standardowy wynosi 0.04546756. Histogram 10 000 wartości jest przedstawiony na dolnym panelu Rys. 3.3. Nawet jeśli funkcja `boot` nie zapewnia różnych wyników w porównaniu z bezpośrednim wykorzystaniem funkcji `sample`, oferuje zaawansowane możliwości bootstrapowe, takie jak przedział ufności oparty na aproksymacji rozkładem normalnym [0.2112, 0.3894], przedział ufności uzyskany z kwantyli 2.5 i 97.5% z 10 000 wartości bootstrapowych [0.21, 0.39] oraz skorygowany o obciążenie i przyspieszony (bias-corrected accelerated) percentylowy przedział ufności [0.21, 0.38], uważany za najdokładniejszy (nawet w tym przykładzie, wszystkie przedziały są bardzo zgodne).