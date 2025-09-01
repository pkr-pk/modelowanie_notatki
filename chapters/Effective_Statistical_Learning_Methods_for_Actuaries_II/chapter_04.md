# Rozdział 4: Drzewa bagging i lasy losowe

## 4.6 Interpretowalność

### 4.6.2 Wykresy częściowej zależności

Wizualizacja wartości $\hat{\mu}(x)$ jako funkcji $x$ pozwala zrozumieć jej zależność od łącznych wartości cech. Taka wizualizacja jest jednak ograniczona do małych wartości $p$. Dla $p = 1$, $\hat{\mu}(x)$ można łatwo przedstawić na wykresie jako graf wartości $\hat{\mu}(x)$ względem każdej możliwej wartości $x$ dla pojedynczej zmiennej rzeczywistej $x$, a dla zmiennej kategorialnej $x$ jako wykres słupkowy, gdzie każdy słupek odpowiada jednej wartości zmiennej kategorialnej. Dla $p = 2$ graficzne przedstawienie $\hat{\mu}(x)$ jest wciąż możliwe. Funkcje dwóch zmiennych rzeczywistych można przedstawić za pomocą wykresów konturowych lub siatkowych wykresów perspektywicznych. Funkcje zmiennej kategorialnej i innej zmiennej, kategorialnej lub rzeczywistej, można przedstawić jako serię wykresów, z których każdy ukazuje zależność $\hat{\mu}(x)$ od drugiej zmiennej, warunkowo od wartości pierwszej zmiennej.

---

Dla $p > 2$ przedstawienie $\hat{\mu}(x)$ jako funkcji jej argumentów jest trudniejsze. Alternatywą jest wizualizacja zbioru wykresów, z których każdy pokazuje częściową zależność $\hat{\mu}(x)$ od wybranych małych podzbiorów cech.

Rozważmy podwektor $x_S$ składający się z $l < p$ cech z wektora $x = (x_1, x_2, ..., x_p)$, indeksowany przez $S \subset \{1, 2, ..., p\}$. Niech $x_{\bar{S}}$ będzie podwektorem uzupełniającym, takim że 

$$x_S \cup x_{\bar{S}} = x.$$

Zasadniczo $\hat{\mu}(x)$ zależy od cech w $x_S$ i $x_{\bar{S}}$, więc (w razie potrzeby po zmianie kolejności cech) możemy zapisać:

$$ \hat{\mu}(x) = \hat{\mu}(x_S, x_{\bar{S}}). $$

Zatem, jeśli uwarunkujemy na określone wartości cech w $x_{\bar{S}}$, to $\hat{\mu}(x)$ można postrzegać jako funkcję cech w $x_S$. Jednym ze sposobów zdefiniowania częściowej zależności $\hat{\mu}(x)$ od $x_S$ jest:

$$ \hat{\mu}_S(x_S) = E_{X_{\bar{S}}}[\hat{\mu}(x_S, X_{\bar{S}})]. \quad (4.6.6) $$

Ta uśredniona funkcja może być użyta jako opis wpływu wybranego podzbioru $x_S$ na $\hat{\mu}(x)$, gdy cechy w $x_S$ nie mają silnych interakcji z cechami w $x_{\bar{S}}$. Na przykład, w szczególnym przypadku, gdy zależność $\hat{\mu}(x)$ od $x_S$ jest addytywna:

$$ \hat{\mu}(x) = f_S(x_S) + f_{\bar{S}}(x_{\bar{S}}), \quad (4.6.7) $$

tak że nie ma interakcji między cechami w $x_S$ a $x_{\bar{S}}$, funkcja częściowej zależności (4.6.6) przyjmuje postać:

$$ \hat{\mu}_S(x_S) = f_S(x_S) + E_{X_{\bar{S}}}[f_{\bar{S}}(X_{\bar{S}})]. \quad (4.6.8) $$

Zatem (4.6.6) wyznacza $f_S(x_S)$ z dokładnością do stałej addytywnej. W tym przypadku widać, że (4.6.6) dostarcza pełnego opisu sposobu, w jaki $\hat{\mu}(x)$ zmienia się w podzbiorze $x_S$.

Funkcję częściowej zależności $\hat{\mu}_S(x_S)$ można estymować na podstawie zbioru uczącego jako:

$$ \frac{1}{|I|} \sum_{i \in I} \hat{\mu}(x_S, x_{i\bar{S}}), \quad (4.6.9) $$

gdzie $\{x_{i\bar{S}}, i \in I\}$ to wartości $X_{\bar{S}}$ w zbiorze uczącym. Należy zauważyć, że $\hat{\mu}_S(x_S)$ reprezentuje wpływ $x_S$ na $\hat{\mu}(x)$ po uwzględnieniu średnich efektów pozostałych cech $x_{\bar{S}}$ na $\hat{\mu}(x)$. Jest to zatem coś innego niż obliczenie wartości oczekiwanej warunkowej:

$$ \tilde{\mu}_S(x_S) = E_X[\hat{\mu}(X)|X_S = x_S]. \quad (4.6.10) $$

Istotnie, to drugie wyrażenie ujmuje wpływ $x_S$ na $\hat{\mu}(x)$, ignorując wpływ $x_{\bar{S}}$. Oba wyrażenia (4.6.6) i (4.6.10) są w rzeczywistości równoważne tylko wtedy, gdy $X_S$ i $X_{\bar{S}}$ są niezależne. Na przykład, w szczególnym przypadku (4.6.7), gdzie $\hat{\mu}(x)$ jest addytywne, $\tilde{\mu}_S(x_S)$ można zapisać jako:

$$ \tilde{\mu}_S(x_S) = f_S(x_S) + E_{X_{\bar{S}}}[f_{\bar{S}}(X_{\bar{S}})|X_S = x_S] \quad (4.6.11) $$

Widać, że $\tilde{\mu}_S(x_S)$ nie wyznacza $f_S(x_S)$ z dokładnością do stałej, jak to miało miejsce w przypadku $\hat{\mu}_S(x_S)$. Tym razem zachowanie $\tilde{\mu}_S(x_S)$ zależy od struktury zależności między $X_S$ a $X_{\bar{S}}$. W przypadku, gdy zależność $\hat{\mu}(x)$ od $x_S$ jest multiplikatywna, czyli:

$$ \hat{\mu}(x) = f_S(x_S) \cdot f_{\bar{S}}(x_{\bar{S}}), \quad (4.6.12) $$

tak że cechy w $x_S$ wchodzą w interakcję z cechami w $x_{\bar{S}}$, (4.6.6) przyjmuje postać:

$$ \hat{\mu}_S(x_S) = f_S(x_S) \cdot E_{X_{\bar{S}}}[f_{\bar{S}}(X_{\bar{S}})], \quad (4.6.13) $$

podczas gdy (4.6.10) jest dane przez:

$$ \tilde{\mu}_S(x_S) = f_S(x_S) \cdot E_{X_{\bar{S}}}[f_{\bar{S}}(X_{\bar{S}})|X_S = x_S]. \quad (4.6.14) $$

W tym drugim przykładzie widać, że $\hat{\mu}_S(x_S)$ wyznacza $f_S(x_S)$ z dokładnością do mnożnikowego czynnika stałego, więc jego postać nie zależy od struktury zależności między $X_S$ a $X_{\bar{S}}$, podczas gdy $\tilde{\mu}_S(x_S)$ zależy.