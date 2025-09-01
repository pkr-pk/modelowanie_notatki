# Rozdział 15: Selekcja modelu

## 15.3 Graficzne porównanie funkcji gęstości i dystrybuanty

Najbardziej bezpośrednim sposobem na sprawdzenie, jak dobrze model pasuje do danych, jest wykreślenie odpowiednich funkcji gęstości i dystrybuanty.

***
**Przykład 15.1**

Rozważ Zbiory Danych B i C podane w Tabelach 15.1 i 15.2. W tym przykładzie i we wszystkich następnych, w Zbiorze Danych B zastąp wartość 15 743 wartością 3 476 (aby wykresy wygodnie mieściły się na stronie). Przytnij Zbiór Danych B w punkcie 50, a Zbiór Danych C w punkcie 7 500. Oszacuj parametr modelu wykładniczego dla każdego zbioru danych. Wykreśl odpowiednie funkcje i skomentuj jakość dopasowania modelu. Powtórz to dla Zbioru Danych B ocenzurowanego w punkcie 1000 (bez żadnego przycinania).

**Tabela 15.1** Zbiór Danych B ze zmienioną najwyższą wartością.
| | | | | | | | | | |
|---|---|---|---|---|---|---|---|---|---|
| 27 | 82 | 115 | 126 | 155 | 161 | 243 | 294 | 340 | 384 |
| 457 | 680 | 855 | 877 | 974 | 1 193 | 1 340 | 1 884 | 2 558 | 3 476 |

**Tabela 15.2** Zbiór Danych C.
| **Zakres płatności** | **Liczba płatności** |
|---|---|
| 0–7 500 | 99 |
| 7 500–17 500 | 42 |
| 17 500–32 500 | 29 |
| 32 500–67 500 | 28 |
| 67 500–125 000 | 17 |
| 125 000–300 000 | 9 |
| Powyżej 300 000 | 3 |

W przypadku Zbioru Danych B mamy 19 obserwacji (pierwsza obserwacja jest usunięta z powodu przycięcia). Typowy wkład do funkcji wiarygodności to $f(82)/[1 − F(50)]$. Estymata największej wiarygodności parametru wykładniczego wynosi $\hat{\theta} = 802.32$. Empiryczna dystrybuanta zaczyna się w punkcie 50 i skacze o 1/19 przy każdej obserwacji. Dystrybuanta, przy użyciu punktu przycięcia 50, wynosi:

$$F^*(x) = \frac{1 - e^{-x/802.32} - (1 - e^{-50/802.32})}{(1 - (1 - e^{-50/802.32}))} = 1 - e^{-(x-50)/802.32}.$$

Rysunek 15.1 przedstawia wykres tych dwóch funkcji.

Dopasowanie nie jest tak dobre, jak byśmy sobie tego życzyli, ponieważ model zaniża wartości dystrybuanty dla mniejszych wartości x i zawyża je dla większych wartości x. Ten wynik nie jest dobry, ponieważ oznacza to, że prawdopodobieństwa w ogonie rozkładu są zaniżone.

W przypadku Zbioru Danych C, funkcja wiarygodności wykorzystuje wartości przycięte. Na przykład, wkład do funkcji wiarygodności dla pierwszego przedziału wynosi:

$$\left[\frac{F(17 500) - F(7 500)}{(1 - F(7 500))}\right]^{42}.$$

Estymata największej wiarygodności wynosi $\hat{\theta} = 44 253$. Wysokość pierwszego słupka histogramu to:
$42 / (128 * (17 500 - 7 500)) = 0.0000328$,
a ostatni słupek dotyczy przedziału od 125 000 do 300 000 (nie można skonstruować słupka dla przedziału od 300 000 do nieskończoności). Funkcja gęstości musi być przycięta w punkcie 7 500 i staje się:

$$\begin{align*}
f^*(x) &= \frac{f(x)}{(1 - F(7 500))} = \frac{(1/44 253 * e^{-x/44 253})}{(1 - (1 - e^{-7 500/44 253}))} \\
&= \frac{e^{-(x-7 500)/44 253}}{44 253}, \quad x > 7500.
\end{align*}$$

Wykres funkcji gęstości w porównaniu z histogramem przedstawiono na Rysunku 15.2.
Model wykładniczy zaniża wczesne prawdopodobieństwa. Trudno jest na podstawie wykresu ocenić, jak się one mają powyżej 125 000.

Dla Zbioru Danych B zmodyfikowanego o limit, estymata największej wiarygodności wynosi $\hat{\theta} = 718.00$. Przy konstrukcji wykresu, empiryczna dystrybuanta musi kończyć się w punkcie 1 000. Wykres przedstawiono na Rysunku 15.3.

Po raz kolejny, model wykładniczy nie jest dobrze dopasowany.
***

Gdy dystrybuanta modelu jest zbliżona do empirycznej dystrybuanty, trudno jest dostrzec drobne różnice. Spośród wielu sposobów na uwypuklenie tych różnic, przedstawiono tutaj dwa. Pierwszym jest po prostu wykreślenie różnicy między tymi dwiema funkcjami. To znaczy, jeśli $F_n(x)$ jest empiryczną dystrybuantą, a $F^*(x)$ jest dystrybuantą modelu, wykreślamy $D(x) = F_n(x) − F^*(x)$. Nie ma odpowiadającego wykresu dla danych pogrupowanych.

***
**Przykład 15.2**

Wykreśl $D(x)$ dla Zbioru Danych B z poprzedniego przykładu.

Dla Zbioru Danych B przyciętego w punkcie 50, wykres przedstawiono na Rysunku 15.4. Brak dopasowania tego modelu jest na tym wykresie powiększony.

Dla Zbioru Danych B ocenzurowanego w punkcie 1000, wykres musi ponownie kończyć się na tej wartości. Przedstawiono go na Rysunku 15.5. Brak dopasowania jest wciąż widoczny.
***

Innym sposobem na podkreślenie różnic jest wykres p-p, nazywany również wykresem prawdopodobieństwa. Wykres tworzy się poprzez uporządkowanie obserwacji jako $x_1 ≤ ⋯ ≤ x_n$. Następnie dla każdej wartości wykreślany jest punkt. Współrzędne do wykreślenia to $(F_n(x_j), F^*(x_j))$. Jeśli model jest dobrze dopasowany, wykreślone punkty będą znajdować się blisko linii 45° biegnącej od (0, 0) do (1, 1). Aby jednak tak było, potrzebna jest inna definicja empirycznej dystrybuanty. Można wykazać, że wartość oczekiwana $F(X_j)$ wynosi $j/(n+1)$ i dlatego empiryczna dystrybuanta powinna przyjmować tę wartość, a nie zwykłe $j/n$. Jeśli dwie obserwacje mają tę samą wartość, można albo wykreślić oba punkty (miałyby tę samą wartość „y”, ale różne wartości „x”), albo wykreślić pojedynczą wartość, uśredniając dwie wartości „x”.

***
**Przykład 15.3**

Utwórz wykres p-p dla kontynuowanego przykładu.

Dla Zbioru Danych B przyciętego w punkcie 50, $n = 19$, a jedna z obserwowanych wartości to $x = 82$. Wartość empiryczna to $F_{19}(82) = 1/20 = 0.05$. Druga współrzędna to

$$F^*(82) = 1 - e^{-(82-50)/802.32} = 0.0391.$$

Jeden z wykreślonych punktów będzie miał współrzędne (0.05, 0.0391). Kompletny wykres przedstawiono na Rysunku 15.6.

Z lewej dolnej części wykresu jasno wynika, że model wykładniczy przypisuje mniejsze prawdopodobieństwo małym wartościom, niż wynika to z danych. Podobny wykres można skonstruować dla Zbioru Danych B ocenzurowanego w punkcie 1 000 i przedstawiono go na Rysunku 15.7.

Wykres ten kończy się w okolicach 0.75, ponieważ jest to najwyższe prawdopodobieństwo zaobserwowane przed punktem cenzurującym w 1 000. Nie ma wartości empirycznych przy wyższych prawdopodobieństwach. Ponownie, model wykładniczy ma tendencję do zaniżania wartości empirycznych.
***

## 15.4 Testy Hipotez

Obraz może być wart tysiąca słów, ale czasami najlepiej jest zastąpić wrażenia wywołane obrazami dowodami matematycznymi. Jednym z takich dowodów jest test następujących hipotez:

* $H_0$: Dane pochodzą z populacji o zadanym modelu.

* $H_1$: Dane nie pochodzą z takiej populacji.

Statystyka testowa jest zazwyczaj miarą tego, jak blisko dystrybuanta modelu jest do empirycznej dystrybuanty. Gdy hipoteza zerowa w pełni określa model (np. rozkład wykładniczy ze średnią 100), wartości krytyczne są dobrze znane. Jednakże częściej zdarza się, że hipoteza zerowa podaje nazwę modelu, ale nie jego parametry. Gdy parametry są estymowane na podstawie danych, statystyka testowa ma tendencję do bycia mniejszą, niż byłaby, gdyby wartości parametrów były z góry określone. Ta zależność występuje, ponieważ sama metoda estymacji stara się dobrać parametry, które tworzą rozkład zbliżony do danych. Gdy parametry są estymowane na podstawie danych, testy stają się przybliżone. Ponieważ odrzucenie hipotezy zerowej następuje dla dużych wartości statystyki testowej, przybliżenie to ma tendencję do zwiększania prawdopodobieństwa błędu II rodzaju (uznania modelu za akceptowalny, gdy taki nie jest), jednocześnie obniżając prawdopodobieństwo błędu I rodzaju (odrzucenia akceptowalnego modelu). W modelowaniu aktuarialnym tendencja ta jest prawdopodobnie akceptowalnym kompromisem.

Jedną z metod uniknięcia tego przybliżenia jest losowy podział próby na dwa zbiory. Jeden nazywany jest **zbiorem uczącym**. Ten zbiór służy do estymacji parametrów. Drugi zbiór nazywany jest **zbiorem testowym** lub **walidacyjnym**. Ten zbiór służy do oceny jakości dopasowania modelu. Jest to bardziej realistyczne, ponieważ model jest walidowany na nowych danych. To podejście jest łatwiejsze do zastosowania, gdy jest dużo danych, dzięki czemu oba zbiory są wystarczająco duże, aby dać użyteczne wyniki.

### 15.4.1 Test Kołmogorowa-Smirnowa

Niech $t$ będzie punktem lewostronnego obcięcia ($t=0$, jeśli nie ma obcięcia), a $u$ punktem prawostronnego cenzurowania ($u = \infty$, jeśli nie ma cenzurowania). Wtedy statystyka testowa to:

$$D = \max_{t \le x \le u} |F_n(x) - F^*(x)|.$$

Test w przedstawionej tu formie powinien być stosowany tylko dla danych indywidualnych, aby zapewnić, że funkcja schodkowa $F_n(x)$ jest dobrze zdefiniowana. Ponadto zakłada się, że dystrybuanta modelu $F^*(x)$ jest ciągła w odpowiednim zakresie.

***
**Przykład 15.4**

Oblicz $D$ dla Przykładu 15.1.

Tabela 15.3 zawiera potrzebne wartości. Ponieważ empiryczna dystrybuanta skacze w każdym punkcie danych, dystrybuantę modelu należy porównać zarówno przed, jak i po skoku. Wartości tuż przed skokiem oznaczono w tabeli jako $F_n(x-)$. Maksimum wynosi $D = 0.1340$.

Dla Zbioru Danych B cenzurowanego w punkcie 1000, 15 z 20 obserwacji jest nieocenzurowanych. Tabela 15.4 ilustruje potrzebne obliczenia. Maksimum wynosi $D = 0.0991$.

**Tabela 15.3** Obliczenie D dla Przykładu 15.4.
| x | $F^*(x)$ | $F_n(x-)$ | $F_n(x)$ | Maksymalna różnica |
|---|---|---|---|---|
| 82 | 0.0391 | 0.0000 | 0.0526 | 0.0391 |
| 115 | 0.0778 | 0.0526 | 0.1053 | 0.0275 |
| 126 | 0.0904 | 0.1053 | 0.1579 | 0.0675 |
| 155 | 0.1227 | 0.1579 | 0.2105 | 0.0878 |
| 161 | 0.1292 | 0.2105 | 0.2632 | 0.1340 |
| 243 | 0.2138 | 0.2632 | 0.3158 | 0.1020 |
| 294 | 0.2622 | 0.3158 | 0.3684 | 0.1062 |
| 340 | 0.3033 | 0.3684 | 0.4211 | 0.1178 |
| 384 | 0.3405 | 0.4211 | 0.4737 | 0.1332 |
| 457 | 0.3979 | 0.4737 | 0.5263 | 0.1284 |
| 680 | 0.5440 | 0.5263 | 0.5789 | 0.0349 |
| 855 | 0.6333 | 0.5789 | 0.6316 | 0.0544 |
| 877 | 0.6433 | 0.6316 | 0.6842 | 0.0409 |
| 974 | 0.6839 | 0.6842 | 0.7368 | 0.0529 |
| 1 193 | 0.7594 | 0.7368 | 0.7895 | 0.0301 |
| 1 340 | 0.7997 | 0.7895 | 0.8421 | 0.0424 |
| 1 884 | 0.8983 | 0.8421 | 0.8947 | 0.0562 |
| 2 558 | 0.9561 | 0.8947 | 0.9474 | 0.0614 |
| 3 476 | 0.9860 | 0.9474 | 1.0000 | 0.0386 |

**Tabela 15.4** Obliczenie D z cenzurowaniem dla Przykładu 15.4.
| x | $F^*(x)$ | $F_n(x-)$ | $F_n(x)$ | Maksymalna różnica |
|---|---|---|---|---|
| 27 | 0.0369 | 0.00 | 0.05 | 0.0369 |
| 82 | 0.1079 | 0.05 | 0.10 | 0.0579 |
| 115 | 0.1480 | 0.10 | 0.15 | 0.0480 |
| 126 | 0.1610 | 0.15 | 0.20 | 0.0390 |
| 155 | 0.1942 | 0.20 | 0.25 | 0.0558 |
| 161 | 0.2009 | 0.25 | 0.30 | 0.0991 |
| 243 | 0.2871 | 0.30 | 0.35 | 0.0629 |
| 294 | 0.3360 | 0.35 | 0.40 | 0.0640 |
| 340 | 0.3772 | 0.40 | 0.45 | 0.0728 |
| 384 | 0.4142 | 0.45 | 0.50 | 0.0858 |
| 457 | 0.4709 | 0.50 | 0.55 | 0.0791 |
| 680 | 0.6121 | 0.55 | 0.60 | 0.0621 |
| 855 | 0.6960 | 0.60 | 0.65 | 0.0960 |
| 877 | 0.7052 | 0.65 | 0.70 | 0.0552 |
| 974 | 0.7425 | 0.70 | 0.75 | 0.0425 |
| 1 000 | 0.7516 | 0.75 | 0.75 | 0.0016 |
***

Pozostaje tylko określić wartość krytyczną. Powszechnie stosowane wartości krytyczne dla tego testu to $1.22/\sqrt{n}$ dla $\alpha = 0.10$, $1.36/\sqrt{n}$ dla $\alpha = 0.05$ oraz $1.63/\sqrt{n}$ dla $\alpha = 0.01$. Gdy $u < \infty$, wartość krytyczna powinna być mniejsza, ponieważ jest mniej możliwości, aby różnica stała się duża.

***
**PRZYKŁAD 15.5**

Dokończ test Kołmogorowa-Smirnowa dla poprzedniego przykładu.

Dla Zbioru Danych B obciętego w punkcie 50, wielkość próby wynosi 19. Wartość krytyczna na poziomie istotności 5% wynosi $1.36/\sqrt{19} = 0.3120$. Ponieważ $0.1340 < 0.3120$, hipoteza zerowa nie jest odrzucana, a rozkład wykładniczy jest wiarygodnym modelem. Chociaż jest mało prawdopodobne, aby model wykładniczy był odpowiedni dla tej populacji, wielkość próby jest zbyt mała, aby dojść do takiego wniosku. Dla Zbioru Danych B cenzurowanego w punkcie 1 000, wielkość próby wynosi 20, więc wartość krytyczna to $1.36/\sqrt{20} = 0.3041$, a model wykładniczy jest ponownie postrzegany jako wiarygodny.
***

Zarówno dla tego testu, jak i dla następującego po nim testu Andersona-Darlinga, wartości krytyczne są poprawne tylko wtedy, gdy hipoteza zerowa w pełni określa model. Gdy zbiór danych jest używany do estymacji parametrów dla rozkładu z hipotezy zerowej (jak w przykładzie), poprawna wartość krytyczna jest mniejsza. Dla obu testów zmiana zależy od konkretnego rozkładu, który jest hipotezą, a może nawet od konkretnych prawdziwych wartości parametrów.

### 15.4.4 Test ilorazu wiarygodności

Alternatywnym pytaniem do „Czy populacja mogła pochodzić z rozkładu A?” jest „Czy rozkład B jest bardziej odpowiednim przedstawieniem populacji niż rozkład A?”. Bardziej formalnie:

$H_0$: Dane pochodzą z populacji o rozkładzie A.

$H_1$: Dane pochodzą z populacji o rozkładzie B.

Aby przeprowadzić formalny test hipotez, rozkład A musi być szczególnym przypadkiem rozkładu B, na przykład wykładniczy w porównaniu z gamma. Prosty sposób na ukończenie tego testu jest następujący.

**Definicja 15.1** **Test ilorazu wiarygodności** przeprowadza się w następujący sposób. Po pierwsze, niech funkcja wiarygodności będzie zapisana jako $L(\theta)$. Niech $\theta_0$ będzie wartością parametrów, która maksymalizuje funkcję wiarygodności. Jednakże, można rozważać tylko te wartości parametrów, które mieszczą się w hipotezie zerowej. Niech $L_0 = L(\theta_0)$. Niech $\theta_1$ będzie estymatorem największej wiarygodności, gdzie parametry mogą przyjmować wszystkie możliwe wartości z hipotezy alternatywnej, a następnie niech $L_1 = L(\theta_1)$. Statystyką testową jest $T = 2 \ln(L_1/L_0) = 2(\ln L_1 - \ln L_0)$. Hipoteza zerowa jest odrzucana, jeśli $T > c$, gdzie $c$ jest obliczane z $\alpha = \text{Pr}(T > c)$, gdzie $T$ ma rozkład chi-kwadrat z liczbą stopni swobody równą liczbie wolnych parametrów w modelu z hipotezy alternatywnej minus liczba wolnych parametrów w modelu z hipotezy zerowej.

Ten test ma pewien sens. Gdy prawdziwa jest hipoteza alternatywna, wymuszenie wyboru parametru z hipotezy zerowej powinno dać znacznie niższą wartość wiarygodności.

**Przykład 15.9**

Chcesz przetestować hipotezę, że populacja, z której pochodzi Zbiór Danych B (używając oryginalnej największej obserwacji), ma średnią inną niż 1.200. Załóż, że populacja ma rozkład gamma i przeprowadź test ilorazu wiarygodności na poziomie istotności 5%. Określ również wartość p.

Hipotezy są następujące:

$H_0$: rozkład gamma ze średnią $\mu = 1.200$,

$H_1$: rozkład gamma ze średnią $\mu \neq 1.200$.

Z wcześniejszej pracy wiemy, że estymatory największej wiarygodności to $\hat{\alpha} = 0.55616$ i $\hat{\theta} = 2.561.1$. Logarytm wiarygodności w maksimum wynosi $\ln L_1 = -162.293$. Następnie należy zmaksymalizować wiarygodność, ale tylko dla tych wartości $\alpha$ i $\theta$, dla których $\alpha\theta = 1.200$. To ograniczenie oznacza, że $\alpha$ może przyjmować dowolne wartości dodatnie, ale $\theta = 1.200/\alpha$. Zatem, przy hipotezie zerowej, jest tylko jeden wolny parametr. Funkcja wiarygodności jest zmaksymalizowana przy $\hat{\alpha} = 0.54955$ i $\hat{\theta} = 2.183.6$. Logarytm wiarygodności w tym maksimum wynosi $\ln L_0 = -162.466$. Statystyką testową jest $T = 2(-162.293 + 162.466) = 0.346$. Dla rozkładu chi-kwadrat z jednym stopniem swobody, wartość krytyczna wynosi 3.8415. Ponieważ $0.346 < 3.8415$, hipoteza zerowa nie jest odrzucana. Prawdopodobieństwo, że zmienna losowa chi-kwadrat z jednym stopniem swobody przekroczy 0.346 wynosi 0.556, co jest wartością p wskazującą na niewielkie poparcie dla hipotezy alternatywnej.

**Tabela 15.11** Sześć użytecznych modeli dla Przykładu 15.10.
| Model | Liczba parametrów | Ujemny logarytm wiarygodności | $\chi^2$ | $p$-value |
|---|---|---|---|---|
| Ujemny dwumianowy | 2 | 5348.04 | 8.77 | 0.0125 |
| ZM logarytmiczny | 2 | 5343.79 | 4.92 | 0.1779 |
| Poissona-odwrotny Gaussa | 2 | 5343.51 | 4.54 | 0.2091 |
| ZM ujemny dwumianowy | 3 | 5343.62 | 4.65 | 0.0979 |
| Geometryczno-ujemny dwumianowy | 3 | 5342.70 | 1.96 | 0.3754 |
| Poisson-ETNB | 3 | 5342.51 | 2.75 | 0.2525 |

**Przykład 15.10**

(Ciąg dalszy Przykładu 6.3) Członkowie klasy (a, b, 0) nie byli wystarczający do opisania tych danych. Określ odpowiedni model.

Do danych dopasowano trzynaście różnych rozkładów. Wyniki tego procesu ujawniły sześć modeli z wartościami p powyżej 0.01 dla testu zgodności chi-kwadrat. Informacje o tych modelach podano w Tabeli 15.11. Test ilorazu wiarygodności wskazuje, że trójparametrowy model o najmniejszym ujemnym logarytmem wiarygodności (Poisson-ETNB) nie jest znacząco lepszy od dwuparametrowego modelu Poissona-odwrotnego Gaussa. Ten ostatni wydaje się być doskonałym wyborem.

Kuszące jest użycie tego testu, gdy rozkład alternatywny ma po prostu więcej parametrów niż rozkład zerowy. W takich przypadkach test może nie być odpowiedni. Na przykład, jest możliwe, że dwuparametrowy model lognormalny będzie miał wyższą wartość logarytmu wiarygodności niż trójparametrowy model Burra, co skutkuje ujemną statystyką testową, wskazując, że rozkład chi-kwadrat nie jest odpowiedni. Gdy rozkład zerowy jest granicznym (a nie szczególnym) przypadkiem rozkładu alternatywnego, test nadal może być używany, ale rozkład statystyki testowej jest teraz mieszaniną rozkładów chi-kwadrat. Niezależnie od tego, nadal jest rozsądne używanie „testu” do podejmowania decyzji w tych przypadkach, pod warunkiem, że jest jasne, iż formalny test hipotez nie został przeprowadzony.