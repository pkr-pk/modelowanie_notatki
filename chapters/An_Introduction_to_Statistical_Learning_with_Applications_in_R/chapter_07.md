# Rozdział 7: Wychodząc poza liniowość

## 7.4 Splajny regresyjne

### 7.4.3 Podstawowa reprezentacja splajnów

Splajny regresyjne, które właśnie widzieliśmy w poprzedniej sekcji, mogły wydawać się nieco skomplikowane: jak dopasować wielomian odcinkowy stopnia $d$ przy ograniczeniu, że musi on (i ewentualnie jego pierwsze $d − 1$ pochodne) być ciągły? Okazuje się, że możemy użyć modelu bazowego (7.7) do reprezentacji splajnu regresyjnego. Splajn sześcienny z $K$ węzłami można zamodelować jako

$$y_i = \beta_0 + \beta_1 b_1(x_i) + \beta_2 b_2(x_i) + \dots + \beta_{K+3} b_{K+3}(x_i) + \epsilon_i, \quad (7.9)$$

dla odpowiedniego wyboru funkcji bazowych $b_1, b_2,...,b_{K+3}$. Model (7.9) można następnie dopasować metodą najmniejszych kwadratów.

Podobnie jak istniało kilka sposobów reprezentacji wielomianów, istnieje również wiele równoważnych sposobów reprezentacji splajnów sześciennych przy użyciu różnych wyborów funkcji bazowych w (7.9). Najbardziej bezpośrednim sposobem reprezentacji splajnu sześciennego przy użyciu (7.9) jest rozpoczęcie od bazy dla wielomianu sześciennego – mianowicie, $x$, $x^2$, i $x^3$ – a następnie dodanie jednej **obciętej potęgowej funkcji bazowej** na każdy węzeł. Obcięta potęgowa funkcja bazowa jest zdefiniowana jako

$$h(x, \xi) = (x - \xi)^3_+ = \begin{cases} (x - \xi)^3 & \text{jeśli } x > \xi \\ 0 & \text{w przeciwnym razie}, \end{cases} \quad (7.10)$$

gdzie $\xi$ jest węzłem. Można wykazać, że dodanie składnika postaci $\beta_4h(x, \xi)$ do modelu (7.8) dla wielomianu sześciennego doprowadzi do nieciągłości tylko w trzeciej pochodnej w punkcie $\xi$; funkcja pozostanie ciągła, z ciągłymi pierwszą i drugą pochodną, w każdym z węzłów.

Innymi słowy, aby dopasować splajn sześcienny do zbioru danych z $K$ węzłami, wykonujemy regresję metodą najmniejszych kwadratów z wyrazem wolnym i $3 + K$ predyktorami postaci $X, X^2, X^3, h(X, \xi_1), h(X, \xi_2),...,h(X, \xi_K)$, gdzie $\xi_1,..., \xi_K$ to węzły. Sprowadza się to do estymacji łącznie $K + 4$ współczynników regresji; z tego powodu dopasowanie splajnu sześciennego z $K$ węzłami wykorzystuje $K+ 4$ stopnie swobody.

Niestety, splajny mogą mieć dużą wariancję na krańcach zakresu predyktorów — to znaczy, gdy X przyjmuje bardzo małą lub bardzo dużą wartość. Rysunek 7.4 pokazuje dopasowanie do danych `Wage` z trzema węzłami. Widzimy, że przedziały ufności w obszarze brzegowym wydają się dość niestabilne. **Splajn naturalny** to splajn regresyjny z dodatkowymi ograniczeniami brzegowymi: funkcja musi być liniowa na krańcach (w obszarze, gdzie X jest mniejsze od najmniejszego węzła lub większe od największego węzła). To dodatkowe ograniczenie oznacza, że splajny naturalne generalnie dają bardziej stabilne estymacje na krańcach. Na Rysunku 7.4, naturalny splajn sześcienny jest również pokazany jako czerwona linia. Zauważ, że odpowiadające mu przedziały ufności są węższe.

## 7.7 Uogólnione Modele Addytywne

W rozdziałach 7.1–7.6 przedstawiliśmy szereg metod elastycznego przewidywania odpowiedzi $Y$ na podstawie pojedynczego predyktora $X$. Metody te można postrzegać jako rozszerzenia prostej regresji liniowej. W tym miejscu zbadamy problem elastycznego przewidywania $Y$ na podstawie kilku predyktorów, $X_1,...,X_p$. Stanowi to rozszerzenie wielokrotnej regresji liniowej.

**Uogólnione modele addytywne (GAM)** zapewniają ogólne ramy do rozszerzania standardowego modelu liniowego, pozwalając na nieliniowe funkcje każdej ze zmiennych, przy jednoczesnym zachowaniu **addytywności**. Podobnie jak modele liniowe, GAM można stosować zarówno w przypadku odpowiedzi ilościowych, jak i jakościowych. Najpierw zbadamy GAM dla odpowiedzi ilościowej w podrozdziale 7.7.1, a następnie dla odpowiedzi jakościowej w podrozdziale 7.7.2.

### 7.7.1 GAM dla problemów regresji

Naturalnym sposobem rozszerzenia modelu regresji wielokrotnej

$$y_i = \beta_0 + \beta_1 x_{i1} + \beta_2 x_{i2} + \dots + \beta_p x_{ip} + \epsilon_i$$

w celu uwzględnienia nieliniowych zależności między każdą cechą a odpowiedzią jest zastąpienie każdego składnika liniowego $\beta_j x_{ij}$ (gładką) nieliniową funkcją $f_j(x_{ij})$. Model ten zapisalibyśmy wówczas jako

$$\begin{align*}
y_i &= \beta_0 + \sum_{j=1}^{p} f_j(x_{ij}) + \epsilon_i \\
&= \beta_0 + f_1(x_{i1}) + f_2(x_{i2}) + \dots + f_p(x_{ip}) + \epsilon_i. \quad (7.15)
\end{align*}$$

Jest to przykład modelu GAM. Nazywa się go **modelem addytywnym**, ponieważ obliczamy oddzielną funkcję $f_j$ dla każdej zmiennej $X_j$, a następnie sumujemy ich wkłady.

W rozdziałach 7.1–7.6 omówiliśmy wiele metod dopasowywania funkcji do pojedynczej zmiennej. Piękno modeli GAM polega na tym, że możemy wykorzystać te metody jako elementy składowe do dopasowania modelu addytywnego. W rzeczywistości, dla większości metod, które widzieliśmy do tej pory w tym rozdziale, można to zrobić w dość prosty sposób. Weźmy na przykład splajny naturalne i rozważmy zadanie dopasowania modelu

$$\text{wage} = \beta_0 + f_1(\text{year}) + f_2(\text{age}) + f_3(\text{education}) + \epsilon \quad (7.16)$$

na danych `Wage`. Tutaj `year` i `age` są zmiennymi ilościowymi, podczas gdy zmienna `education` jest jakościowa i ma pięć poziomów: `< HS`, `HS`, `< Coll`, `Coll`, `> Coll`, odnoszących się do poziomu ukończonej szkoły średniej lub studiów wyższych. Dopasowujemy pierwsze dwie funkcje za pomocą splajnów naturalnych. Trzecią funkcję dopasowujemy, używając osobnej stałej dla każdego poziomu, za pomocą zwykłego podejścia ze zmiennymi zero-jedynkowymi z podrozdziału 3.3.1.

Rysunek 7.11 pokazuje wyniki dopasowania modelu (7.16) za pomocą metody najmniejszych kwadratów. Jest to łatwe do zrobienia, ponieważ, jak omówiono w podrozdziale 7.4, splajny naturalne można skonstruować przy użyciu odpowiednio dobranego zestawu funkcji bazowych. Dlatego cały model to po prostu duża regresja na zmiennych bazowych splajnów i zmiennych zero-jedynkowych, wszystko spakowane w jedną dużą macierz regresji.

Rysunek 7.11 można łatwo zinterpretować. Panel lewostronny wskazuje, że przy stałym `age` i `education`, `wage` ma tendencję do nieznacznego wzrostu wraz z `year`; może to być spowodowane inflacją. Panel środkowy wskazuje, że przy stałym `education` i `year`, `wage` ma tendencję do bycia najwyższą dla średnich wartości `age`, a najniższą dla bardzo młodych i bardzo starych. Panel prawostronny wskazuje, że przy stałym `year` i `age`, `wage` ma tendencję do wzrostu wraz z `education`: im osoba jest lepiej wykształcona, tym wyższa jest jej średnia pensja. Wszystkie te wnioski są intuicyjne.

Rysunek 7.12 pokazuje podobny zestaw trzech wykresów, ale tym razem $f_1$ i $f_2$ to splajny wygładzające odpowiednio z czterema i pięcioma stopniami swobody. Dopasowanie modelu GAM ze splajnem wygładzającym nie jest tak proste, jak dopasowanie GAM ze splajnem naturalnym, ponieważ w przypadku splajnów wygładzających nie można użyć metody najmniejszych kwadratów. Jednakże, standardowe oprogramowanie, takie jak funkcja `gam()` w R, może być użyte do dopasowania modeli GAM za pomocą splajnów wygładzających, poprzez podejście znane jako **backfitting**. Metoda ta dopasowuje model obejmujący wiele predyktorów poprzez wielokrotne, naprzemienne aktualizowanie dopasowania dla każdego predyktora, utrzymując pozostałe na stałym poziomie. Piękno tego podejścia polega na tym, że za każdym razem, gdy aktualizujemy funkcję, po prostu stosujemy metodę dopasowania dla tej zmiennej do reszty cząstkowej. 

Dopasowane funkcje na Rysunkach 7.11 i 7.12 wyglądają dość podobnie. W większości sytuacji różnice w modelach GAM uzyskanych przy użyciu splajnów wygładzających w porównaniu ze splajnami naturalnymi są niewielkie.

Nie musimy używać splajnów jako elementów składowych modeli GAM: równie dobrze możemy użyć regresji lokalnej, regresji wielomianowej lub dowolnej kombinacji podejść przedstawionych wcześniej w tym rozdziale do stworzenia modelu GAM. Modele GAM są dokładniej badane w laboratorium na końcu tego rozdziału.

**Zalety i wady modeli GAM**

Zanim przejdziemy dalej, podsumujmy zalety i ograniczenia modelu GAM.

▲ Modele GAM pozwalają nam dopasować nieliniową funkcję $f_j$ do każdej zmiennej $X_j$, dzięki czemu możemy automatycznie modelować nieliniowe zależności, które standardowa regresja liniowa pominie. Oznacza to, że nie musimy ręcznie próbować wielu różnych transformacji na każdej zmiennej z osobna.

▲ Nieliniowe dopasowania mogą potencjalnie prowadzić do dokładniejszych przewidywań dla odpowiedzi Y.

▲ Ponieważ model jest addytywny, możemy badać wpływ każdej zmiennej $X_j$ na $Y$ indywidualnie, utrzymując wszystkie pozostałe zmienne na stałym poziomie.

▲ Gładkość funkcji $f_j$ dla zmiennej $X_j$ można podsumować za pomocą stopni swobody.

▼ Głównym ograniczeniem modeli GAM jest to, że model jest ograniczony do bycia addytywnym. Przy wielu zmiennych można pominąć ważne interakcje. Jednak, podobnie jak w przypadku regresji liniowej, możemy ręcznie dodać składniki interakcji do modelu GAM, włączając dodatkowe predyktory w postaci $X_j \times X_k$. Dodatkowo możemy dodać do modelu niskowymiarowe funkcje interakcji w postaci $f_{jk}(X_j, X_k)$; takie składniki można dopasować za pomocą dwuwymiarowych funkcji wygładzających, takich jak regresja lokalna lub dwuwymiarowe splajny (nie omówione tutaj).

### 7.7.2 GAM dla problemów klasyfikacji

Modele GAM można również stosować w sytuacjach, gdy $Y$ jest zmienną jakościową. Dla uproszczenia, zakładamy tutaj, że $Y$ przyjmuje wartości 0 lub 1, i niech $p(X) = Pr(Y = 1|X)$ będzie warunkowym prawdopodobieństwem (biorąc pod uwagę predyktory), że odpowiedź jest równa jeden. Przypomnijmy sobie model regresji logistycznej (4.6):

$$\log \left( \frac{p(X)}{1 - p(X)} \right) = \beta_0 + \beta_1 X_1 + \beta_2 X_2 + \dots + \beta_p X_p. \quad (7.17)$$

Lewa strona to logarytm szans $P(Y = 1|X)$ względem $P(Y = 0|X)$, który (7.17) przedstawia jako liniową funkcję predyktorów. Naturalnym sposobem rozszerzenia (7.17) w celu uwzględnienia nieliniowych zależności jest użycie modelu

$$\log \left( \frac{p(X)}{1 - p(X)} \right) = \beta_0 + f_1(X_1) + f_2(X_2) + \dots + f_p(X_p). \quad (7.18)$$

Równanie 7.18 to **GAM regresji logistycznej**. Ma ono te same zalety i wady, co omówiono w poprzednim podrozdziale dla odpowiedzi ilościowych.

Dopasowaliśmy model GAM do danych `Wage` w celu przewidzenia prawdopodobieństwa, że dochód danej osoby przekracza 250 000 dolarów rocznie. Dopasowany przez nas model GAM ma postać

$$\log \left( \frac{p(X)}{1 - p(X)} \right) = \beta_0 + \beta_1 \times \text{year} + f_2(\text{age}) + f_3(\text{education}), \quad (7.19)$$

gdzie
$$p(X) = Pr(\text{wage} > 250 | \text{year}, \text{age}, \text{education}).$$

Ponownie, $f_2$ jest dopasowywane za pomocą splajnu wygładzającego z pięcioma stopniami swobody, a $f_3$ jest dopasowywane jako funkcja schodkowa, tworząc zmienne zero-jedynkowe dla każdego z poziomów wykształcenia. Wynikowe dopasowanie pokazano na Rysunku 7.13. Ostatni panel wygląda podejrzanie, z bardzo szerokimi przedziałami ufności dla poziomu `< HS`. W rzeczywistości, dla tej kategorii żadne wartości odpowiedzi nie są równe jeden: żadna osoba z wykształceniem niższym niż średnie nie zarabia więcej niż 250 000 dolarów rocznie. Dlatego ponownie dopasowujemy model GAM, wykluczając osoby z wykształceniem niższym niż średnie. Wynikowy model pokazano na Rysunku 7.14. Podobnie jak na Rysunkach 7.11 i 7.12, wszystkie trzy panele mają podobne skale pionowe. Pozwala nam to wizualnie ocenić względny wkład każdej ze zmiennych. Obserwujemy, że `age` i `education` mają znacznie większy wpływ niż `year` na prawdopodobieństwo bycia osobą o wysokich zarobkach.
