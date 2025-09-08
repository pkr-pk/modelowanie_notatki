# Rozdział 4: Uogólnione modele liniowe (GLMs)

## 4.5 Dewiancja

### 4.5.3 Dewiancja

#### 4.5.3.1 Statystyka ilorazu wiarygodności

Model statystyczny opisuje, jak aktuariusz dzieli zmienność obecną w danych na strukturę systematyczną (odzwierciedloną w punktacji GLM) i losowe odchylenia od wartości oczekiwanych (zmienność wprowadzona przez założony rozkład ED). Model zerowy reprezentuje jeden skrajny przypadek, w którym dane są czysto losowe, podczas gdy model pełny lub nasycony przedstawia dane jako całkowicie systematyczne. Model pełny dostarcza miary tego, jak dobrze jakikolwiek model oparty na rozważanym rozkładzie ED może dopasować się do danych. Ponieważ model pełny daje najwyższą osiągalną log-wiarygodność przy danym rozkładzie ED, różnica między log-wiarygodnością $L_{full}$ modelu pełnego a log-wiarygodnością $L(\hat{\beta})$ badanego modelu GLM mierzy dobroć dopasowania uzyskaną w tym GLM. Prowadzi to do statystyki ilorazu wiarygodności, odpowiadającej dwukrotności tej różnicy. W rodzinie ED ta statystyka ilorazu wiarygodności jest dana przez:

$$LR = 2(L_{full} - L(\hat{\beta})) = \frac{2}{\phi} \sum_{i=1}^{n} \nu_i [y_i(\tilde{\theta}_i - \hat{\theta}_i) - a(\tilde{\theta}_i) + a(\hat{\theta}_i)]$$

gdzie $\tilde{\theta}_i$ to estymaty w modelu pełnym, a $\hat{\theta}_i$ to estymaty w badanym modelu. Zbyt duża wartość LR wskazuje, że rozważany model nie dopasowuje się zadowalająco do rzeczywistych danych, podczas gdy zbyt mała wartość LR może być sygnałem, że model ma niską moc wyjaśniającą. Tutaj "zbyt mała" odnosi się do kwantyla rozkładu chi-kwadrat, który wydaje się rozsądnym przybliżeniem rozkładu LR przy łagodnych warunkach regularności.

#### 4.5.3.2 Dewiancja

Podczas pracy z GLM przydatne jest posiadanie wielkości, którą można interpretować jako sumę kwadratów błędów (lub reszt) w modelu regresji liniowej normalnej. Ta wielkość nazywana jest dewiancją lub dewiancją resztową modelu i jest zdefiniowana jako:

$$\begin{align*}
D(y, \hat{\mu}) 
&= \phi LR \\
&= 2\phi(L_{full} - L(\hat{\beta})) \\
&= 2 \sum_{i=1}^{n} \nu_i [y_i(\tilde{\theta}_i - \hat{\theta}_i) - a(\tilde{\theta}_i) + a(\hat{\theta}_i)]
\end{align*}$$

Jest to miara odległości między konkretnym modelem a obserwowanymi danymi, zdefiniowana za pomocą modelu nasyconego. Podobnie jak suma kwadratów błędów, kwantyfikuje ona zmienność w danych, która nie jest wyjaśniona przez rozważany model.

**Tabela 4.7:** Dewiancja związana z GLM dla wybranych rozkładów z rodziny ED.
| Rozkład | Dewiancja |
| --- | --- |
| **Dwumianowy** | $2 \sum_{i=1}^{n} \left( y_i \ln \frac{y_i}{\hat{\mu}_i} + (n_i - y_i) \ln \frac{n_i - y_i}{n_i - \hat{\mu}_i} \right)$ gdzie $\hat{\mu}_i = n_i \hat{q}_i$ |
| **Poissona** | $2 \sum_{i=1}^{n} \left( y_i \ln \frac{y_i}{\hat{\mu}_i} - (y_i - \hat{\mu}_i) \right)$ gdzie $y \ln y = 0$ jeśli $y = 0$ |
| **Normalny** | $\sum_{i=1}^{n} (y_i - \hat{\mu}_i)^2$ |
| **Gamma** | $2 \sum_{i=1}^{n} \left( -\ln \frac{y_i}{\hat{\mu}_i} + \frac{y_i - \hat{\mu}_i}{\hat{\mu}_i} \right)$ |
| **Odwrotny Gaussowski** | $\sum_{i=1}^{n} \frac{(y_i - \hat{\mu}_i)^2}{\hat{\mu}_i^2 y_i}$ |

Ponieważ model nasycony musi pasować do danych co najmniej tak dobrze jak każdy inny model, dewiancja resztowa nigdy nie jest ujemne. Im większa dewiancja, tym większe różnice między rzeczywistymi danymi a dopasowanymi wartościami. Dewiancja modelu zerowego nazywana jest dewiancją zerową.

Tabela 4.7 przedstawia dewiancje związane z modelami GLM opartymi na niektórych członkach rodziny rozkładów ED. Drugi człon $\sum_{i=1}^{n} (y_i - \hat{\mu}_i)$ w dewiancji Poissona jest zwykle równy 0, gdy w punktacji uwzględniony jest wyraz wolny, więc dewiancja Poissona redukuje się do $2 \sum_{i=1}^{n} y_i \ln(y_i / \hat{\mu}_i)$. W przypadku Poissona dewiancja jest również nazywana **statystyką G**. Należy zauważyć, że w tej tabeli $y \ln y$ jest przyjmowane jako 0, gdy $y = 0$ (jego granica, gdy $y \to 0$).

#### 4.5.3.3 Skalowana dewiancja

W rodzinach o znanym parametrze dyspersji $\phi$, takich jak rozkład dwumianowy, dla którego $\phi=1$, dewiancja stanowi podstawę do testowania braku dopasowania modelu lub do porównywania modeli zagnieżdżonych. Jeśli $\phi$ musi być estymowane na podstawie danych, zamiast tego należy użyć skalowanej dewiancji. Dokładniej, skalowana dewiancja jest dana wzorem:

$$
\tilde{D}(y, \hat{\mu}) = \frac{1}{\phi}D(y, \hat{\mu})
$$

Zauważmy, że skalowana dewiancja zależy od parametru dyspersji $\phi$. Gdy $\phi=1$ (podobnie jak w przypadkach rozkładu dwumianowego i Poissona), dewiancja i skalowana dewiancja są takie same. Powszechną praktyką jest po prostu podstawienie estymatora $\hat{\phi}$ w celu obliczenia $\tilde{D}$.

#### 4.5.3.4 Rozkład z próby skalowanej dewiancji

Pojęcie dewiancji rozszerza na wszystkich członków rodziny wykładniczej (ED family) znaną sumę kwadratów reszt stosowaną w normalnej regresji liniowej. Liczba stopni swobody związanej ze skalowaną dewiancją jest równa liczbie obserwacji $n$ pomniejszonej o liczbę $p + 1$ parametrów regresji $\beta_0, \beta_1, ..., \beta_p$, które mają być estymowane. W dużych próbach skalowana dewiancja ma w przybliżeniu rozkład Chi-kwadrat z $n - (p + 1)$ stopniami swobody, tj.

$$
\tilde{D} \approx \chi_{n-p-1}^{2} \quad \text{pod warunkiem, że } n \text{ jest wystarczająco duże.} \quad (4.16)
$$

Duża dewiancja wskazuje na źle dopasowany model. Dokładniej, jeśli uogólniony model liniowy (GLM) jest rozsądnie dopasowany do danych, to skalowana dewiancja $\tilde{D}$ powinna być bliska liczbie resztowych stopni swobody dla modelu, czyli $n - p - 1$.

Należy zauważyć, że do aproksymacji rozkładem Chi-kwadrat w (4.16) należy podchodzić z ostrożnością. Istnieją sytuacje, w których nie jest ona w ogóle prawdziwa, na przykład w przypadku odpowiedzi binarnych (jak pokazano w następnej sekcji).

## 4.9 Surowe, standaryzowane i "studendyzowane" reszty

### 4.9.2 Reszty w GLM

W nienormalnych GLM (Uogólnionych Modelach Liniowych) można zdefiniować kilka typów reszt w analogii do normalnego modelu regresji liniowej. Często odnoszą się one do normalnego modelu regresji liniowej używanego w ostatnim kroku algorytmu IRLS (Iteratively Re-weighted Least Squares). Reszta surowa $r_i$ to po prostu różnica $y_i - \hat{\mu}_i$ między obserwowaną odpowiedzią a oszacowaną średnią $\hat{\mu}_i = g^{-1}(x_i^T \hat{\beta})$. Są one nazywane **resztami odpowiedzi** dla GLM.

Aktuariusze często dopuszczają większe odchylenia między obserwacjami $y_i$ a wartościami dopasowanymi $\hat{\mu}_i$ wraz ze wzrostem średniej, ponieważ wariancja jest zazwyczaj rosnącą funkcją średniej. To sprawia, że wykres reszt surowych jest mniej informatywny. Aby obejść ten problem, reszty surowe są normalizowane przez pierwiastek kwadratowy z wariancji odpowiedzi. Prowadzi to do **reszt Pearsona**, zdefiniowanych jako:

$$r_{i}^P = \frac{y_i - \hat{\mu}_i}{\sqrt{V(\hat{\mu}_i)/\nu_i}}$$

Reprezentują one pierwiastek kwadratowy z wkładu każdej obserwacji do statystyki $X^2$ Pearsona, jako że 

$$\sum_{i=1}^{n} (r_{i}^P)^2 = X^2.$$

To wyjaśnia, dlaczego te reszty nazywane są resztami Pearsona.

Reszty Pearsona pochodzą z modeli liniowych i nie uwzględniają kształtu rozkładu. Ponieważ obliczanie dewiancji koryguje skośność odpowiedzi, zaproponowano inną resztę do stosowania w GLM. **Reszty dewiancyjne** $r_{i}^D$ są definiowane jako pierwiastek kwadratowy ze znakiem z wkładu i-tej obserwacji do dewiancji. Pomaga to wykryć obserwacje, które w dużym stopniu zwiększają dewiancję, a tym samym unieważniają model. W szczególności, rozłóżmy dewiancję na:

$$D(\mathbf{y}, \hat{\boldsymbol{\mu}}) = 2 \sum_{i=1}^{n} v_i \left(y_i(\tilde{\theta}_i - \hat{\theta}_i) - a(\tilde{\theta}_i) + a(\hat{\theta}_i)\right) = \sum_{i=1}^{n} d_i$$

gdzie $d_i = d(y_i, \hat{\theta}_i)$ zostało wprowadzone w równaniu (4.19). Reszty dewiancyjne są wtedy zdefiniowane jako:

$$r_{i}^D = \text{sign}(y_i - \hat{\mu}_i)\sqrt{d_i}$$

Tutaj notacja „sign” oznacza znak wyrażenia w nawiasach:

$$\text{sign}(y_i - \hat{\mu}_i) = \begin{cases} 1 & \text{jeżeli } y_i > \hat{\mu}_i \\ -1 & \text{w p.p.} \end{cases}$$

Oczywiście, suma kwadratów reszt dewiancyjnych daje dewiancję, to znaczy 

$$\sum_{i=1}^{n} (r_{i}^D)^2 = D.$$

Reszty dewiancyjne są bardziej odpowiednie w kontekście GLM i często są preferowaną formą reszt. W przypadku normalnym reszty Pearsona i reszty dewiancyjne są identyczne. Należy jednak zauważyć, że analityk nie szuka normalności w resztach dewiancyjnych, ponieważ ich rozkład z próby pozostaje nieznany. Chodzi o brak dopasowania i szukanie wzorców widocznych w resztach: jeśli aktuariusz wykryje jakiś wzorzec w resztach wykreślonych względem:

* wartości dopasowanych,
* cech zawartych w scoringu lub
* cech nieujętych w modelu

to coś jest nie tak i należy podjąć działania. Na przykład, jeśli aktuariusz wykryje jakąś strukturę w resztach wykreślonych względem pominiętej cechy, cecha ta powinna zostać włączona do scoringu. Jeśli na wykresie pokazującym reszty względem każdej cechy zawartej w scoringu widoczne są wzorce, oznacza to, że odpowiednia cecha nie jest właściwie zintegrowana ze scoringiem (powinna zostać przekształcona lub aktuariusz powinien zrezygnować z GLM na rzecz modelu addytywnego). Czasami interesujące jest również wykreślenie reszt względem czasu lub współrzędnych przestrzennych. Wzorce w rozproszeniu (wykryte przez wykreślenie reszt względem wartości dopasowanych) mogą wskazywać na naddyspersję lub, bardziej ogólnie, na użycie niewłaściwej relacji średnia-wariancja.

Reszty standaryzowane uwzględniają różne dźwignie (leverages). Rozważmy macierz kapeluszową (hat matrix) (4.12) pochodzącą z ostatniej iteracji algorytmu IRLS. Wartości kapeluszowe $h_{ii}$ mierzą dźwignię, czyli potencjał obserwacji do wpływania na wartości dopasowane. Duża wartość dźwigni $h_{ii}$ wskazuje, że dopasowanie jest wrażliwe na odpowiedź $y_i$. Duże dźwignie zazwyczaj oznaczają, że odpowiadające im cechy są w jakiś sposób nietypowe. Reszty standaryzowane, nazywane również resztami skalowanymi, są następnie podawane przez:

$$t_i^P = \frac{r_i^P}{\sqrt{\hat{\phi}(1-h_{ii})}} \quad oraz \quad t_i^D = \frac{r_i^D}{\sqrt{\hat{\phi}(1-h_{ii})}}.$$

Reszty studentyzowane (nazywane również resztami jackknife lub resztami z usunięciem) odpowiadają resztom standaryzowanym wynikającym z pomijania każdej obserwacji po kolei. Ponieważ w przypadku GLM zazwyczaj nie istnieją proste zależności między $\hat{\beta}$ a $\hat{\beta}_{(-i)}$, dokładne ich obliczenie wymagałoby ponownego dopasowania modelu n razy, co jest niewykonalne dla dużych rozmiarów prób. Przybliżenie podają reszty likelihoodowe, zdefiniowane jako ważona średnia reszt standaryzowanych dewiancyjnych i Pearsona, więc przyjmujemy:

$$t_i^* = \text{sign}(y_i - \hat{\mu}_i)\sqrt{(1-h_{ii})(t_i^D)^2 + h_{ii}(t_i^P)^2}.$$

W przeciwieństwie do normalnego modelu regresji liniowej, gdzie standaryzowane reszty mają znany rozkład, rozkład reszt w GLM jest zazwyczaj nieznany. Dlatego zakres reszt jest trudny do oszacowania, ale można je badać pod kątem struktury i dyspersji, wykreślając je względem wartości dopasowanych lub niektórych cech. Dźwignia (Leverage) mierzy jedynie potencjał wpływu na dopasowanie, podczas gdy miary wpływu bardziej bezpośrednio oceniają wpływ każdej obserwacji na dopasowanie. Zazwyczaj odbywa się to poprzez badanie zmiany w dopasowaniu po pominięciu tej obserwacji. W GLM można to zrobić, patrząc na zmiany we współczynnikach regresji. Statystyka Cooka mierzy odległość między $\hat{\beta}$ a $\hat{\beta}_{(-i)}$ uzyskaną przez pominięcie i-tego punktu danych $(y_i, x_i)$ ze zbioru uczącego.