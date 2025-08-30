# Rozdział 5: Teoria wartości ekstremalnych

## 5.2 Przekraczanie progu

### 5.2.4 Metoda Hilla

Metoda GPD nie jest jedynym sposobem estymacji ogona rozkładu; jako alternatywę opiszemy w tym podrozdziale dobrze znaną metodę Hilla do modelowania ogonów rozkładów o grubych ogonach.

---
*Estymacja indeksu ogona.* W tej metodzie zakładamy, że bazowy rozkład strat znajduje się w maksymalnym obszarze przyciągania rozkładu Frécheta, tak że, zgodnie z Twierdzeniem 5.8, ma on ogon w postaci

$$\bar{F}(x) = x^{-\alpha}L(x) \quad (5.22)$$

dla wolno zmieniającej się funkcji L (patrz Definicja 5.7) i dodatniego parametru $\alpha$. Tradycyjnie w podejściu Hilla uwaga skupia się na indeksie ogona $\alpha$, a nie na jego odwrotności $\xi$, która pojawia się w (5.3). Celem jest znalezienie estymatora $\alpha$ na podstawie danych o jednakowym rozkładzie $X_1, ..., X_n$.

Estymator Hilla można wyprowadzić na różne sposoby (patrz EKM, s. 330–336). Być może najbardziej eleganckim jest rozważenie funkcji średniej nadwyżki dla logarytmicznej straty $\ln X$, gdzie $X$ jest zmienną losową o dystrybuancie (5.22). Oznaczając $e^*$ jako funkcję średniej nadwyżki dla $\ln X$ i stosując całkowanie przez części, otrzymujemy:

$$\begin{align*}
e^*(\ln u) 
&= E(\ln X - \ln u | \ln X > \ln u) \\ 
&= \frac{1}{\bar{F}(u)} \int_u^\infty (\ln x - \ln u) dF(x) \\ 
&= \frac{1}{\bar{F}(u)} \int_u^\infty \frac{\bar{F}(x)}{x} dx \\ 
&= \frac{1}{\bar{F}(u)} \int_u^\infty \frac{L(x)}{x^{\alpha+1}} dx.
\end{align*}$$

Dla dostatecznie dużego $u$, wolno zmieniającą się funkcję $L(x)$ dla $x \ge u$ można zasadniczo potraktować jako stałą i wynieść ją przed całkę. Bardziej formalnie, używając Twierdzenia Karamaty (patrz sekcja A.1.4), otrzymujemy, dla $u \to \infty$:

$$e^*(\ln u) \sim \frac{L(u) u^{-\alpha}}{\alpha \bar{F}(u)} = \alpha^{-1},$$

więc $\lim_{u\to\infty} \alpha e^*(\ln u) = 1$. Oczekujemy podobnego zachowania ogona w próbkowej funkcji średniej nadwyżki $e_n^*$ (patrz (5.16)) skonstruowanej na podstawie obserwacji logarytmicznych. Oznacza to, że oczekujemy, iż $e_n^*(\ln X_{k,n}) \approx \alpha^{-1}$ dla dużego $n$ i dostatecznie małego $k$, gdzie $X_{n,n} \le \dots \le X_{1,n}$ są statystykami porządkowymi. Obliczenie $e_n^*(\ln X_{k,n})$ daje nam estymator $\hat{\alpha}^{-1} = ((k-1)^{-1} \sum_{j=1}^{k-1} \ln X_{j,n} - \ln X_{k,n})$. Standardowa postać estymatora Hilla jest uzyskiwana przez niewielką modyfikację:

$$\hat{\alpha}^{(H)}_{k,n} = \left( \frac{1}{k} \sum_{j=1}^{k} \ln X_{j,n} - \ln X_{k,n} \right)^{-1}, \quad 2 \le k \le n. \quad (5.23)$$

Estymator Hilla jest jednym z najlepiej zbadanych estymatorów w literaturze EVT. Właściwości asymptotyczne (zgodność, asymptotyczna normalność) tego estymatora (gdy wielkość próby $n \to \infty$, liczba ekstremów $k \to \infty$ i tzw. frakcja ogona $k/n \to 0$) zostały szeroko zbadane w ramach różnych modeli danych, w tym ARCH i GARCH. Skupiamy się na praktycznym zastosowaniu estymatora, a w szczególności na jego wydajności w stosunku do podejścia opartego na estymacji GPD.

Gdy dane pochodzą z rozkładu z ogonem, który jest bliski doskonałej funkcji potęgowej, estymator Hilla jest często dobrym estymatorem $\alpha$ lub jej odwrotności $\xi$. W praktyce ogólna strategia polega na wykreśleniu estymacji Hilla dla różnych wartości *k*. Daje to wykres Hilla $\{(k, \hat{\alpha}_{k,n}^{(H)}): k = 2, ..., n\}$. Mamy nadzieję znaleźć na wykresie Hilla stabilny region, w którym estymacje skonstruowane na podstawie różnych liczb statystyk porządkowych są do siebie bardzo podobne.

**Przykład 5.27 (Wykresy Hilla).** Tworzymy wykresy Hilla dla danych dotyczących pożarów w Danii z Przykładu 5.23 oraz dla danych o tygodniowych stratach procentowych (tylko wartości dodatnie) z Przykładu 5.24 (pokazane na Rysunku 5.8).

Bardzo łatwo jest skonstruować wykres Hilla dla wszystkich możliwych wartości $k$, ale może to być mylące; doświadczenie praktyczne (zobacz Przykład 5.28) sugeruje, że najlepsze wybory $k$ są względnie małe: powiedzmy, 10–50 statystyk porządkowych w próbie o wielkości 1000. Z tego powodu powiększyliśmy fragmenty wykresów Hilla pokazujące estymacje uzyskane dla wartości *k* mniejszych niż 60.

Dla danych duńskich uzyskane estymacje $\alpha$ mieszczą się w przedziale od 1.5 do 2, co sugeruje estymacje $\xi$ w przedziale od 0.5 do 0.67, a wszystkie te wartości odpowiadają modelom o nieskończonej wariancji dla tych danych. Przypomnijmy, że estymacja uzyskana z naszego modelu GPD w Przykładzie 5.23 wynosiła $\hat{\xi} = 0.50$. Dla danych AT&T na wykresie nie ma szczególnie stabilnego regionu. Estymacje $\alpha$ oparte na $k = 2, ..., 60$ statystykach porządkowych przeważnie mieszczą się w zakresie od 2 do 4, co sugeruje wartość $\xi$ w przedziale 0.25–0.5, która jest większa niż wartości oszacowane w Przykładzie 5.26 przy użyciu modelu GPD.

Przykład 5.27 pokazuje, że interpretacja wykresów Hilla może być trudna. W praktyce mogą wystąpić różne odchylenia od sytuacji idealnej. Jeśli dane nie pochodzą z rozkładu o regularnie zmieniającym się ogonie, metoda Hilla jest naprawdę nieodpowiednia, a wykresy Hilla mogą być bardzo mylące. Zależność seryjna w danych również może pogorszyć działanie estymatora, chociaż dotyczy to także estymatora GPD. EKM zawiera szereg „horror plots” Hilla opartych na symulowanych danych, ilustrujących pojawiające się problemy.

*Estymaty ogona oparte na metodzie Hilla.* W zastosowaniach związanych z zarządzaniem ryzykiem, które omawiamy w tej książce, mniej interesuje nas estymacja indeksu ogona danych o grubych ogonach, a bardziej estymaty ogona i miar ryzyka. Przedstawiamy heurystyczne uzasadnienie dla standardowego estymatora ogona opartego na podejściu Hilla. Przyjmujemy model ogona w postaci $\bar{F}(x) = Cx^{-\alpha}, x \ge u > 0$, dla pewnego wysokiego progu $u$; innymi słowy, zastępujemy funkcję wolno zmieniającą się stałą dla dostatecznie dużych wartości $x$. Dla odpowiedniej wartości $k$ indeks ogona $\alpha$ jest estymowany przez $\hat{\alpha}^{(H)}_{k,n}$, a próg $u$ jest zastępowany przez $X_{k,n}$ (lub $X_{(k+1),n}$ w niektórych wersjach); pozostaje oszacować $C$. Ponieważ $C$ można zapisać jako $C = u^\alpha \bar{F}(u)$, jest to równoznaczne z estymacją $\bar{F}(u)$, a oczywistym estymatorem empirycznym jest $k/n$ (lub $(k-1)/n$ w niektórych wersjach). Zestawienie tych pomysłów daje estymator ogona Hilla w jego standardowej postaci:

$$\hat{\bar{F}}(x) = \frac{k}{n} \left( \frac{x}{X_{k,n}} \right)^{-\hat{\alpha}^{(H)}_{k,n}}, \quad x \ge X_{k,n}. \quad (5.24)$$

Zapisanie estymatora w ten sposób podkreśla sposób, w jaki jest on traktowany matematycznie. Dla każdej pary $k$ i $n$, zarówno estymator Hilla, jak i powiązany z nim estymator ogona, są traktowane jako funkcje $k$ górnych statystyk porządkowych z próby o wielkości $n$. Oczywiście możliwe jest odwrócenie tego estymatora w celu uzyskania estymatora kwantylowego, a także opracowanie estymatora wartości oczekiwanej straty przekraczającej VaR (expected shortfall) przy użyciu argumentów dotyczących ogonów regularnie zmieniających się.

Estymator ogona oparty na GPD (5.21) jest zazwyczaj traktowany jako funkcja losowej liczby $N_u$ górnych statystyk porządkowych dla stałego progu $u$. Różna prezentacja tych estymatorów w literaturze jest kwestią konwencji i możemy łatwo przekształcić oba estymatory do podobnej postaci. Załóżmy, że przepisujemy (5.24) w notacji (5.21), zastępując $1/\hat{\alpha}^{(H)}_{k,n}$, $X_{k,n}$ i $k$ odpowiednio przez $\hat{\xi}^{(H)}$, $u$ i $N_u$. Otrzymujemy

$$\hat{\bar{F}}(x) = \frac{N_u}{n} \left( 1 + \hat{\xi}^{(H)} \frac{x-u}{\hat{\xi}^{(H)}u} \right)^{-1/\hat{\xi}^{(H)}}, \quad x \ge u.$$

W tym estymatorze brakuje dodatkowego parametru skali $\beta$ z (5.21) i ma on tendencję do gorszego działania, co zostanie pokazane na symulowanych przykładach w następnej sekcji.