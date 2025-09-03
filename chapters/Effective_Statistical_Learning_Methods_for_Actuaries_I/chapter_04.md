# Rozdział 4: Uogólnione modele liniowe (GLMs)

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