# Rozdział 4: Uogólnione modele liniowe (GLMs)

## 4.1 Wprowadzenie

GLM jest definiowany przez określenie dwóch komponentów:
- odpowiedź musi podlegać rozkładowi ED przedstawionemu w Rozdz. 2, oraz
- funkcja łącząca opisuje, jak średnia odpowiedź jest powiązana z liniową kombinacją dostępnych cech (ogólnie nazywaną oceną (score) w zastosowaniach ubezpieczeniowych, nie mylić z oceną Fishera w statystyce przywołaną w Rozdz. 3).

GLM-y zostały pierwotnie wprowadzone w praktyce aktuarialnej jako metoda poprawy dokładności wyceny ubezpieczeń komunikacyjnych. Ich zastosowanie zostało następnie gwałtownie rozszerzone na większość linii biznesowych. Dziś są rutynowo stosowane w underwritingu, wycenie lub zarządzaniu szkodami. Oto kilka typowych zastosowań GLM-ów w ubezpieczeniach:

- **Modelowanie liczby szkód.** Doświadczenie związane z konkretną umową ubezpieczeniową jest generalnie rozkładane na liczbę szkód wraz z odpowiednimi kosztami tych szkód. Rozkład Poissona jest naturalnym kandydatem do modelowania liczby szkód zgłaszanych przez ubezpieczających. Typowe założenie w tych okolicznościach jest takie, że warunkowa średnia częstotliwość szkód może być zapisana jako funkcja wykładnicza liniowej oceny estymowanej na podstawie danych.
- **Modelowanie wysokości szkód.** Ponieważ koszty szkód są nieujemne i dodatnio skośne, rozkłady Gamma i Odwrotny Gaussowski są naturalnymi kandydatami do ich modelowania. Tutaj również aktuariusze generalnie zakładają, że warunkowa średnia wysokość szkody może być zapisana jako wykładnicza funkcja liniowej oceny.
- **Graduacja umieralności i chorobowości.** Aktuariusze są świadomi, że w przeżywalności ubezpieczających występuje duża heterogeniczność. Czynniki ryzyka wpływające na śmiertelność obejmują wiek, płeć, wykształcenie, dochód, zawód, stan cywilny i zachowania zdrowotne. Ogólnie rzecz biorąc, osoby o wyższym statusie społeczno-ekonomicznym mają tendencję do życia dłużej niż te z niższych grup społeczno-ekonomicznych. GLM-y dwumianowe i Poissona pozwalają aktuariuszowi na przeprowadzanie tego rodzaju klasyfikacji ryzyka w ubezpieczeniach na życie i zdrowotnych. Podejście zależy od rodzaju dostępnych danych: grupa otwarta lub zamknięta, dane indywidualne lub dane pogrupowane w przedziały według niektórych cech (takich jak np. suma ubezpieczenia).
- **Modelowanie elastyczności.** Modele elastyczności dla nowych i wznowionych transakcji pomagają firmom ubezpieczeniowym przewidywać wpływ różnych działań (i podwyżek składek) na udział w rynku. Rentowność i modele elastyczności można następnie połączyć w celu podjęcia optymalnych decyzji cenowych.
- **Rezerwacja strat.** Oprócz nadmiernie dyspersyjnego GLM Poissona odpowiadającego zagregowanej metodzie rezerwacji Chain-Ladder, GLM-y mogą być również stosowane w indywidualnej rezerwacji szkód w celu przewidywania czasu do rozliczenia w zależności od niektórych cech szkody, a także ostatecznej kwoty szkody. Pozwala to likwidatorom szkód na tworzenie bardziej odpowiednich rezerw i wczesną identyfikację roszczeń, które mogą być oszukańcze lub najprawdopodobniej zakończą się procesem sądowym.
- **Analiza konkurencji.** GLM-y mogą być wykorzystane do odtworzenia komercyjnych cenników stosowanych przez konkurentów. Odbywa się to poprzez inżynierię wsteczną ich cennika. Pozwala to aktuariuszom na ekstrapolację składek premium na inne profile ryzyka poza te, dla których dostępne są oferty.

Ten rozdział ma na celu staranne przejrzenie, krok po kroku, maszynerii GLM. Bardziej zaawansowane metody przedstawione w kolejnych rozdziałach można postrzegać jako rozszerzenia podstawowego podejścia GLM zaproponowanego w celu zaradzenia niektórym jego wadom. Dlatego głębokie zrozumienie mechanizmu GLM jest potrzebne i uzasadnia szczegółowe omówienie oferowane w tym rozdziale.

Jak wspomniano powyżej, GLM-y opierają się na wyborze rozkładu (wyborze członka rodziny ED do modelowania losowych odchyleń od średniej), liniowej ocenie obejmującej dostępne cechy oraz założonej funkcji łączącej oczekiwaną odpowiedź i ocenę. Gdy aktuariusz wybierze te składniki zgodnie z rozważanymi danymi, wnioskowanie statystyczne jest przeprowadzane za pomocą przeglądanej w Rozdz. 3 zasady największej wiarygodności. Miary diagnostyczne pozwalają następnie aktuariuszowi zbadać względne zalety rozważanych modeli w celu wybrania optymalnego dla rozważanych danych ubezpieczeniowych. Całą analizę można przeprowadzić przy użyciu istniejącego oprogramowania, w tym swobodnie dostępnego, otwartego oprogramowania R używanego w tej książce.

## 4.2 Struktura GLM-ów

Podejście GLM jest przykładem modelowania regresyjnego lub nadzorowanego uczenia się. Model regresyjny ma na celu wyjaśnienie niektórych cech odpowiedzi, z pomocą cech działających jako zmienne objaśniające. W zastosowaniach ubezpieczeniowych aktuariusze zazwyczaj chcą wyjaśnić średnią wartość odpowiedzi wchodzącą w skład czystych składek: na przykład oczekiwaną liczbę szkód i odpowiadającą jej oczekiwaną składkę za szkodę.

GLM-y oddzielają systematyczne cechy danych od losowych wariacji, które muszą być kompensowane przez ubezpieczenie. Cechy systematyczne są reprezentowane przez funkcję regresji, podczas gdy losowe wariacje są reprezentowane przez rozkład prawdopodobieństwa w rodzinie ED. Dokładniej, GLM składa się z trzech komponentów:
- rozkładu odpowiedzi (losowy komponent) wybranego z rodziny ED,
- liniowej oceny (komponent systematyczny), oraz
- funkcji łączącej, która wiąże średnią odpowiedź z oceną obejmującą dostępne cechy.

Komponent systematyczny podaje czystą składkę odpowiadającą obserwowalnym profilom ubezpieczających, podczas gdy komponent losowy reprezentuje zmienność, która ma być skompensowana przez ubezpieczyciela na mocy wzajemności lub solidarności losowej. Używając GLM, aktuariusz domyślnie zakłada, że wszystkie niezbędne informacje są dostępne (to znaczy, że wektory losowe $\boldsymbol{X}$ i $\boldsymbol{X}^{+}$ pokrywają się, zobacz notację w Sekcji 1.3.4). Jeśli tak nie jest, należy zamiast tego użyć modeli mieszanych (takich jak te badane w Rozdz. 5).

### 4.2.1 Rozkład Odpowiedzi

Rozważmy odpowiedzi $Y_1, Y_2, \dots, Y_n$ mierzone na $n$ osobach. Dla każdej odpowiedzi, aktuariusz ma informacje podsumowane w wektorze $\boldsymbol{x}_i = (1, x_{i1}, \dots, x_{ip})^\top$ wymiaru $p+1$ zawierającym odpowiadające mu cechy (uzupełnione o 1 z przodu, gdy w ocenie uwzględniony jest wyraz wolny, jak w większości zastosowań ubezpieczeniowych). Cechy zazwyczaj składają się z terminów polisy lub cech ubezpieczającego. Pomyśl o rodzaju typu pojazdu, wieku lub zakresie ubezpieczenia w ubezpieczeniach komunikacyjnych, lub rodzaju konstrukcji, dacie budowy lub kwocie ubezpieczenia w ubezpieczeniach domów. Ubezpieczyciele mają teraz dostęp do wielu zmiennych klasyfikacyjnych, które są oparte na warunkach polisy lub informacjach dostarczonych przez ubezpieczających, lub które zawarte są w zewnętrznych bazach danych. Czasami aktuariusz musi uzupełnić ubezpieczenie o dane bankowe lub telematyczne, znacznie rozszerzając dostępne informacje. Odtąd zakładamy, że cechy $x_{ij}$ zostały zarejestrowane bez błędów i brakujących wartości.

Oprócz odpowiedzi, mamy cel, tj. interesującą nas wielkość. W taryfikacji celem jest czysta składka

$$
\mu(\boldsymbol{x}) = \mathrm{E}[Y | \boldsymbol{X} = \boldsymbol{x}],
$$

zwana także funkcją regresji. Zauważ, że celem nie jest odpowiedź $Y$ sama w sobie (jak byłoby w przypadku predykcji). Dzieje się tak, ponieważ aktuariusz chce określić czyste składki, czyli oczekiwane straty. Do celów taryfikacji aktuariusz nie jest zainteresowany przewidywaniem rzeczywistych strat jakiegokolwiek ubezpieczającego, ale raczej średnimi stratami dla jednorodnej grupy ubezpieczonych osób.

Aktuariusze generalnie mają do czynienia z danymi obserwacyjnymi, tak że same cechy są zmiennymi losowymi. Analiza jest następnie przeprowadzana na próbie $(y_i, \boldsymbol{x}_i)$ obserwacji, które można rozumieć jako realizacje wektora losowego łączącego odpowiedź z wektorem cech. Jednak nie próbujemy modelować łącznego rozkładu tego wektora losowego. Zamiast tego, biorąc pod uwagę informacje zawarte w $\boldsymbol{x}_i$, odpowiedzi $Y_i$ zakłada się, że są niezależne z rozkładem warunkowym w rodzinie ED gruntownie zbadanym w Rozdz. 2. Warto zauważyć, że odpowiedzi nie muszą mieć identycznych rozkładów. W szczególności, $Y_i$ podlega rozkładowi (2.3) z określonym parametrem kanonicznym

$$
\theta_i = \theta(\boldsymbol{x}_i)
$$

wyrażonym w funkcji dostępnych cech $\boldsymbol{x}_i$, a ten sam parametr dyspersji $\phi$. Sposób, w jaki $\theta_i$ jest powiązany z informacjami zawartymi w $\boldsymbol{x}_i$, jest wyjaśniony dalej. Interesującą nas wielkością jest średnia odpowiedź specyficzna dla ubezpieczającego $\mu_i$ dana przez

$$
\mu_i = \mu(\boldsymbol{x}_i) = a'(\theta_i) = a'(\theta(\boldsymbol{x}_i)).
$$

Można uwzględnić określone wagi $\nu_i$ w celu uwzględnienia różnych ilości informacji, np. gdy $Y_i$ reprezentuje średnią wyników dla grupy jednorodnych ryzyk, a nie wynik pojedynczych ryzyk. Na przykład, odpowiedź może być średnią kwotą straty dla kilku roszczeń, wszystkie z tymi samymi cechami. Waga $\nu_i$ jest wtedy przyjmowana jako liczba strat wchodzących w skład średniej, w zastosowaniu Właściwości 2.4.2. Pomimo tej niezwykłej właściwości GLM-ów, pozwalającej aktuariuszowi na przeprowadzanie analiz na danych pogrupowanych, ważne jest, aby pamiętać, że zawsze jest preferowane nie agregowanie danych i zachowywanie jak największej liczby indywidualnych rekordów.

W praktyce aktuarialnej wagi czasami odzwierciedlają również wiarygodność przypisaną danej obserwacji. Wagi pozwalają aktuariuszowi zidentyfikować odpowiedzi niosące więcej informacji. Ponieważ obserwacje o wyższych wagach mają mniejszą wariancję, dopasowanie modelu będzie bardziej pod wpływem tych punktów danych. Wrócimy do tego zagadnienia w kilku przypadkach w tym rozdziale podczas omawiania estymacji.

### 4.2.2 Ocena Liniowa

Ocena GLM dla odpowiedzi $Y_i$ jest zdefiniowana jako

$$
\text{score}_i = \boldsymbol{x}_i^\top \boldsymbol{\beta} = \beta_0 + \sum_{j=1}^p \beta_j x_{ij}, \quad i=1, 2, \dots, n,
$$

gdzie $\boldsymbol{\beta} = (\beta_0, \beta_1, \dots, \beta_p)^\top$ jest wektorem wymiaru $p+1$ zawierającym nieznane współczynniki regresji. Zatem $\beta_j$ są parametrami do estymacji na podstawie danych. Jak wskazano w Rozdz. 3, ocena odnosi się do liniowej kombinacji $\boldsymbol{x}_i^\top \boldsymbol{\beta}$ cech, a nie do oceny Fishera.

Ilość $\beta_0$ odpowiada wszystkim wynikom. Nazywa się to wyrazem wolnym, ponieważ odpowiada ocenie dla osoby, dla której $x_{ij} = 0$ dla wszystkich $j=1, \dots, p$. Następnie, każda wielkość $\beta_j$ kwantyfikuje wpływ na ocenę wzrostu o jedną jednostkę w odpowiadającej jej cesze $x_{ij}$. Obecność wyrazu wolnego $\beta_0$ wyjaśnia, dlaczego pierwszy wpis wektora $\boldsymbol{x}_i$ to 1.

Wyjaśnijmy znaczenie liniowości oceny. Słowo „liniowy” w GLM oznacza, że zmienne objaśniające są łączone liniowo, aby przewidzieć (funkcję) średniej. Liniowość w GLM-ach odnosi się do liniowości w współczynnikach $\beta_j$, a nie w cechach. Na przykład, oceny

$$
\text{score}_i = \beta_0 + \beta_1 x_{i1} + \beta_2 x_{i1}^2 \quad \text{i} \quad \text{score}_i = \beta_0 + \beta_1 x_{i1} + \beta_2 x_{i2} + \beta_3 x_{i1}x_{i3}
$$

są obie „liniowe” w sensie GLM, pomimo kwadratu $x_{i1}^2$ i iloczynu $x_{i1}x_{i3}$. W rzeczywistości w GLM zawsze dozwolone jest przekształcanie cech $x_{ij}$, tworząc nowe, przekształcone cechy, takie jak $x_{ij}^2$ lub $x_{ij}x_{ik}$ (chociaż GAM-y oferują lepsze podejście do radzenia sobie z nieliniowymi efektami w skali oceny, jak zobaczymy w Rozdz. 6). Jednak ocena

$$
\text{score}_i = \beta_0 + \beta_1 x_{i1} + \exp(\beta_2 x_{i2})
$$

nie jest liniowa (i nie może być brana pod uwagę w ustawieniu GLM), więc takie nieliniowe oceny, które nie kwalifikują się do analizy GLM, będą rozważane w Rozdz. 8.

### 4.2.3 Macierz Projektowa

Wprowadźmy macierz $\boldsymbol{X}$, często nazywaną macierzą projektową w statystyce, otrzymaną przez powiązanie $\boldsymbol{x}_i$, wiersz po wierszu. Dokładniej, $i$-ty wiersz $\boldsymbol{X}$ jest $\boldsymbol{x}_i^\top$, tak że

$$
\boldsymbol{X} = \begin{pmatrix} \boldsymbol{x}_1^\top \\ \boldsymbol{x}_2^\top \\ \vdots \\ \boldsymbol{x}_n^\top \end{pmatrix} = \begin{pmatrix} 1 & x_{11} & \dots & x_{1p} \\ 1 & x_{21} & \dots & x_{2p} \\ \vdots & \vdots & \ddots & \vdots \\ 1 & x_{n1} & \dots & x_{np} \end{pmatrix}.
$$

Gdy model regresji zawiera wyraz wolny, macierz projektowa $\boldsymbol{X}$ zawiera kolumnę jedynek. Wektor $\boldsymbol{s}$ zbierający wyniki $s_1, \dots, s_n$ dla wszystkich $n$ odpowiedzi jest następnie otrzymywany z

$$
\boldsymbol{s} = (s_1, \dots, s_n)^\top = \boldsymbol{X}\boldsymbol{\beta}.
$$

W całym tym rozdziale zawsze zakładamy, że macierz projektowa $\boldsymbol{X}$ ma pełny rząd. To założenie jest naruszone, jeśli jedna z cech jest liniową kombinacją niektórych innych cech, co implikuje nadmiarowość informacji. Wrócimy do tego zagadnienia, omawiając współliniowość.

Jako przykład, rozważmy zbiór danych wyświetlony w Tabeli 4.1 sklasyfikowany krzyżowo według płci posiadaczy polis (z dwoma poziomami, mężczyzna i kobieta) oraz zakresu ubezpieczenia (z trzema poziomami, obowiązkowe ubezpieczenie od odpowiedzialności cywilnej (zwane dalej TPL), ograniczone szkody i kompleksowe). Gdy cechy $x_{ij}$ są numeryczne, całkowite lub ciągłe, mogą one bezpośrednio wejść do oceny: ich wkład jest wtedy otrzymywany przez pomnożenie $x_{ij}$ przez odpowiadający mu współczynnik $\beta_j$. Jednakże, jakościowe lub kategoryczne dane muszą być najpierw przetworzone przed wejściem do obliczenia oceny. GLM-y efektywnie uwzględniają efekty cech kategorycznych przez „dummyfikację” włączenia do oceny, jak wyjaśniono w Przykładzie 1.4.3.

Dwie cechy w Tabeli 4.1 są kategoryczne. Można je zakodować za pomocą cech binarnych. Dokładniej, każdego ubezpieczającego można reprezentować za pomocą wektora $\boldsymbol{x}_i = (1, x_{i1}, x_{i2}, x_{i3})^\top$ z trzema cechami binarnymi:

$$
\begin{aligned}
x_{i1} &= \begin{cases} 1 & \text{jeśli posiadacz polisy } i \text{ jest kobietą} \\ 0 & \text{w przeciwnym razie} \end{cases} \\
x_{i2} &= \begin{cases} 1 & \text{jeśli posiadacz polisy } i \text{ ma tylko TPL} \\ 0 & \text{w przeciwnym razie} \end{cases} \\
x_{i3} &= \begin{cases} 1 & \text{jeśli posiadacz polisy } i \text{ ma ubezpieczenie kompleksowe} \\ 0 & \text{w przeciwnym razie}. \end{cases}
\end{aligned}
$$

Tutaj wybraliśmy poziomy bazowe odpowiadające najbardziej zaludnionym kategoriom, a dokładniej kategorii o największej całkowitej ekspozycji: mężczyzna dla płci ($x_{i1} = 0$) i ograniczony zakres ubezpieczenia ($x_{i2} = x_{i3} = 0$). Powody tego wyboru staną się jaśniejsze później.

Dane można następnie wyświetlić w formacie używanym do analizy statystycznej, jak pokazano w Tabeli 4.2. Zobaczymy w Sekcji 4.3.5, dlaczego analiza GLM może być przeprowadzona na danych zagregowanych w formie tabelarycznej zamiast na indywidualnych rekordach, bez utraty informacji. Dlatego indeks $i$ może odnosić się do pojedynczej polisy lub całej klasy ryzyka (jak to ma miejsce tutaj).

Macierz projektowa dla danych wyświetlonych w Tabeli 4.1 to

$$
\boldsymbol{X} = \begin{pmatrix}
1 & 0 & 1 & 0 \\
1 & 0 & 0 & 0 \\
1 & 0 & 0 & 1 \\
1 & 1 & 1 & 0 \\
1 & 1 & 0 & 0 \\
1 & 1 & 0 & 1
\end{pmatrix}.
$$

---
**Tabela 4.1** Obserwowana liczba szkód wraz z odpowiadającymi jej ekspozycjami na ryzyko (w osobolatach) pojawiającymi się w nawiasach. Ubezpieczenia komunikacyjne, dane hipotetyczne.

| | Tylko TPL | Ograniczone szkody | Kompleksowe |
|---|---|---|---|
| **Mężczyźni** | 1,683 | 3,403 | 626 |
|               | (10,000) | (30,000) | (5,000) |
| **Kobiety**   | 873 | 2,423 | 766 |
|               | (6,000) | (24,000) | (7,000) |

---
**Tabela 4.2** Dane z Tabeli 4.1 ułożone w formacie wejściowym do analizy GLM za pomocą oprogramowania statystycznego, takiego jak R.

| $X_1$ | $X_2$ | $X_3$ | # szkód | Ekspozycja |
|-------|-------|-------|---------|------------|
| 0     | 1     | 0     | 1,683   | 10,000     |
| 0     | 0     | 0     | 3,403   | 30,000     |
| 0     | 0     | 1     | 626     | 5,000      |
| 1     | 1     | 0     | 873     | 6,000      |
| 1     | 0     | 0     | 2,423   | 24,000     |
| 1     | 0     | 1     | 766     | 7,000      |

Odpowiadający wektor obserwacji to

$$
\boldsymbol{Y} = \begin{pmatrix}
1,683 \\
3,403 \\
626 \\
873 \\
2,423 \\
766
\end{pmatrix}.
$$

### 4.2.4 Funkcja Łącząca

#### 4.2.4.1 Łączenie Średniej z Oceną Liniową

Zamiast utożsamiać średnią odpowiedź z oceną, jak w modelu regresji normalnej, istnieje jednoznaczne, różniczkowalne odwzorowanie mapujące średnią na ocenę liniową, zwane w żargonie GLM funkcją łączącą. W szczególności, średnia

$$
\mu_i = \mu(\boldsymbol{x}_i)
$$

odpowiedzi $Y_i, i = 1, 2, \dots, n$, jest powiązana z oceną obejmującą zmienne objaśniające za pomocą gładkiej i odwracalnej transformacji linearyzującej, to jest,

$$
g(\mu_i) = \text{score}_i.
$$

Funkcja $g$ jest monotoniczna, różniczkowalna i nazywana jest funkcją łączącą. Ponieważ funkcja łącząca $g$ jest jednoznaczna, możemy ją odwrócić, aby uzyskać

$$
g(\mu_i) = \text{score}_i \iff \mu_i = g^{-1}(\text{score}_i).
$$

Ważne jest, aby zdać sobie sprawę, że tutaj nie przekształcamy odpowiedzi $Y_i$, ale raczej jej oczekiwaną wartość $\mu_i$. Z punktu widzenia statystycznego, model, w którym $g(Y_i)$ jest liniowy w $\boldsymbol{x}_i$, nie jest taki sam jak GLM, w którym $g(\mu_i)$ jest liniowy w $\boldsymbol{x}_i$. Aby to zrozumieć, załóżmy, że $\ln Y_i \sim \mathcal{Nor}(\boldsymbol{x}_i^\top \boldsymbol{\beta}, \sigma^2)$. Chociaż model jest dopasowany w skali logarytmicznej, aktuariusz jest zainteresowany odpowiedziami w skali oryginalnej (w jednostkach monetarnych, dla kwot roszczeń). Jeśli estymowane wyniki $\boldsymbol{x}_i^\top \hat{\boldsymbol{\beta}}$ są potęgowane, aby wrócić do oryginalnej skali, $\exp(\boldsymbol{x}_i^\top \hat{\boldsymbol{\beta}})$ daje wartości dla mediany odpowiedzi, a nie dla średniej. Dla średniej, dopasowane wartości wynoszą

$$
\hat{\mu}_i = \exp\left(\boldsymbol{x}_i^\top \hat{\boldsymbol{\beta}} + \frac{\hat{\sigma}^2}{2}\right)
$$

zgodnie ze wzorem na matematyczną wartość oczekiwaną związaną z rozkładem LogNormalnym.

Zauważ, że GLM jest modelem warunkowym, który określa rozkład odpowiedzi $Y_i$ przy danym wektorze cech $\boldsymbol{X}_i = (X_{i1}, X_{i2}, \dots, X_{ip})$. W szczególności, $Y_i$ przy danym $\boldsymbol{X}_i = \boldsymbol{x}_i$ podlega rozkładowi z rodziny ED z

$$
\mu(\boldsymbol{x}_i) = g^{-1}\left(\beta_0 + \sum_{j=1}^p \beta_j x_{ij}\right).
$$

Niemniej jednak ważne jest, aby zdać sobie sprawę, że losowy wektor $\boldsymbol{X}_i^\top = (X_{i1}, X_{i2}, \dots, X_{ip})$ może mieć złożoną strukturę zależności, która wpływa na oszacowanie $\boldsymbol{\beta}$, jak zobaczymy później. Ważne jest również, aby pamiętać, że bezwarunkowo, $Y_i$ nie podlega temu samemu rozkładowi. Na przykład, jeśli $Y_i$ przy danym $\boldsymbol{X}_i = \boldsymbol{x}_i$ podlega $\mathcal{Poi}(\exp(\boldsymbol{\beta}^\top \boldsymbol{x}_i))$, to bezwarunkowo, $Y_i$ oznacza losową zmienną $\exp(\boldsymbol{\beta}^\top \boldsymbol{X}_i)$ i mamy do czynienia z mieszanym rozkładem Poissona (badanym w Rozdz. 5).

#### 4.2.4.2 Typowe Funkcje Łączące

W zasadzie, każda monotoniczna, ciągła i różniczkowalna funkcja $g$ może być funkcją łączącą. Ale w praktyce jest kilka wygodnych i powszechnych wyborów dla standardowych GLM-ów. W GLM funkcja łącząca jest określana przez aktuarza, podczas gdy w modelu pojedynczego indeksu (SIM) jest ona estymowana na podstawie danych. SIM-y są również oparte na rozkładzie ED dla odpowiedzi i liniowej ocenie, ale funkcja łącząca jest estymowana na podstawie danych.

Często średnia odpowiedź $\mu$ jest ograniczona (jest dodatnia lub zawarta w przedziale jednostkowym $[0, 1]$, na przykład). Ponieważ ocena liniowa nie jest ograniczona (może przyjmować dowolną wartość rzeczywistą, w zależności od znaku współczynników regresji $\beta_j$ i zakresu cech $x_{ij}$), funkcja łącząca może uwzględniać te warunki, tak że współczynniki regresji pozostają nieograniczone (co ułatwia maksymalizację wiarygodności w celu oszacowania $\boldsymbol{\beta}$). Zatem, jednym z celów funkcji łączącej jest odwzorowanie predyktora liniowego na zakres odpowiedzi.

Oczywiście, nie chodzi o to, aby wybór funkcji łączącej był całkowicie determinowany przez zakres zmiennej odpowiedzi. Wybór funkcji łączącej musi być również oparty na danych. Podejście SIM pozwala aktuariuszowi oszacować $g$ na podstawie danych, bez wcześniejszego określania go. Ta nieparametryczna estymacja może służyć jako punkt odniesienia do wyboru odpowiedniej formy funkcjonalnej. Alternatywnie, niektóre wykresy diagnostyczne mogą być użyte do sprawdzenia adekwatności wybranej funkcji łączącej po dopasowaniu modelu. Oznacza to, że wybór $g$ nie jest czysto arbitralny i że w tym zakresie mogą pomóc niektóre procedury oparte na danych.

Tabela 4.3 wymienia zwykłe funkcje łączące i ich odwrotności. Te funkcje łączące narzucają warunki na średnią $\mu_i$, a tym samym na zakres odpowiedzi $Y$. Na przykład, funkcje łączące kwadrat, pierwiastek kwadratowy i logarytm stosuje się tylko do wartości nieujemnych i dodatnich, odpowiednio. Ostatnie cztery funkcje łączące wymienione w Tabeli 4.3 są przeznaczone dla danych dwumianowych, gdzie $\mu_i$ reprezentuje prawdopodobieństwo sukcesu, a zatem należy do przedziału jednostkowego $[0, 1]$.

W następnej sekcji omówimy teraz niektóre funkcje łączące, które są szczególnie przydatne dla aktuariuszy.

---
**Tabela 4.3** Typowe funkcje łączące i ich odwrotności. Tutaj $\mu_i$ to oczekiwana wartość odpowiedzi, $s_i$ to ocena, a $\Phi(\cdot)$ to dystrybuanta standardowego rozkładu normalnego $\mathcal{Nor}(0, 1)$.

| Funkcja łącząca | $s_i = g(\mu_i)$ | $\mu_i = g^{-1}(s_i)$ |
|----|----|----|
| Tożsamościowa (Identity) | $\mu_i$ | $s_i$ |
| Logarytmiczna (Log) | $\ln \mu_i$ | $\exp(s_i)$ |
| Odwrotność (Inverse) | $\mu_i^{-1}$ | $s_i^{-1}$ |
| Odwrotność kwadratu (Inverse-square) | $\mu_i^{-2}$ | $s_i^{-1/2}$ |
| Pierwiastek kwadratowy (Square-root) | $\sqrt{\mu_i}$ | $s_i^2$ |
| Logit | $\ln \frac{\mu_i}{1-\mu_i}$ | $\frac{\exp(s_i)}{1+\exp(s_i)}$ |
| Probit | $\Phi^{-1}(\mu_i)$ | $\Phi(s_i)$ |
| Log-log | $-\ln(-\ln \mu_i)$ | $\exp(-\exp(-s_i))$ |
| Komplementarna log-log | $\ln(-\ln(1-\mu_i))$ | $1 - \exp(-\exp(s_i))$ |

#### 4.2.4.3 Łącze Logarytmiczne

Jak wyjaśniono wcześniej, obiecujący link usuwa ograniczenie na zakres oczekiwanej odpowiedzi. Na przykład, odpowiedź jest nieujemna w wielu zastosowaniach ubezpieczeniowych, gdzie reprezentuje częstotliwość szkód lub ciężkość szkody, tak że $\mu_i \ge 0$. Funkcja łącząca logarytmiczna usuwa wtedy to ograniczenie, ponieważ specyfikacja

$$
\ln \mu_i = \text{score}_i \iff \mu_i = \exp(\text{score}_i)
$$

zapewnia, że warunek ten jest zawsze spełniony. Funkcja łącząca logarytmiczna jest również często używana do budowy cenników, ponieważ zapewnia aktuariuszowi multiplikatywną strukturę taryfy:

$$
\exp(\text{score}_i) = \exp(\beta_0) \prod_{j=1}^p \exp(\beta_j x_{ij}).
$$

Wszystkie dopasowane wartości są wyrażone w odniesieniu do odpowiedzi referencyjnej ($\exp(\beta_0)$) odpowiadającej osobom z $x_{ij}=0$ dla wszystkich $j=1, \dots, p$. Czynniki $\exp(\beta_j x_{ij})$ wchodzące w skład iloczynu można następnie postrzegać jako korekty do $\exp(\beta_0)$, gdy $x_{ij} \ne 0$ dla niektórych $j.$

Gdy wszystkie zmienne objaśniające są kategoryczne, każdy posiadacz polisy jest reprezentowany przez wektor $\boldsymbol{x}_i$ ze składnikami równymi 0 lub 1 (patrz Przykład 1.4.3). Jeśli $x_{ij} \in \{0, 1\}$ dla wszystkich $j=1, \dots, p$, to oczekiwana odpowiedź jest równa

$$
\exp(\boldsymbol{\beta}^\top \boldsymbol{x}_i) = \exp(\beta_0) \prod_{j|x_{ij}=1} \exp(\beta_j)
$$

gdzie

$\exp(\beta_0)$ = oczekiwana odpowiedź odpowiadająca klasie referencyjnej, tj. $x_{ij} = 0$ dla wszystkich $j=1, 2, \dots, p$

$\exp(\beta_j)$ = względny efekt $j$-tej cechy.

Za każdym razem, gdy rozważany posiadacz polisy różni się od klasy referencyjnej, to jest, $x_{ij}=1$ dla niektórych $j$, $\exp(\beta_0)$ jest mnożone przez współczynnik korygujący postaci $\exp(\beta_j)$. Stąd znak $\beta_j$ ujawnia wpływ $j$-tej cechy na średnią odpowiedź: jeśli

$$
\beta_j > 0 \iff \exp(\beta_j) > 1
$$

wtedy $x_{ij}=1$ zwiększa oczekiwaną wartość w porównaniu do $x_{ij}=0$, podczas gdy jeśli

$$
\beta_j < 0 \iff \exp(\beta_j) < 1
$$

wtedy $x_{ij}=1$ zmniejsza oczekiwaną wartość w porównaniu do $x_{ij}=0$. Cechy ze związanym współczynnikiem $\beta_j>0$ wskazują zatem na profile wyższego ryzyka w porównaniu do tych referencyjnych, podczas gdy cechy z $\beta_j<0$ odpowiadają profilom niższego ryzyka.

Czynniki $\exp(\beta_j)$, $j=1, \dots, p$, nazywane są względnościami, ponieważ odpowiadają stosunkom oczekiwanych odpowiedzi dla dwóch posiadaczy polis różniących się tylko $j$-tą cechą, przy czym stosunek ten jest równy 1 dla pierwszego (pojawiającego się w mianowniku) i 0 dla drugiego (pojawiającego się w liczniku). Formalnie, biorąc pod uwagę dwóch posiadaczy polis $i_1$ i $i_2$ takich, że

$$
x_{i_1 j} = 1, \quad x_{i_2 j} = 0, \quad x_{i_1 k} = x_{i_2 k} \text{ dla wszystkich } k \ne j,
$$

widzimy, że stosunek średnich odpowiedzi dla posiadaczy polis $i_1$ i $i_2$ zapisuje się jako

$$
\frac{\exp(\boldsymbol{x}_{i_1}^\top \boldsymbol{\beta})}{\exp(\boldsymbol{x}_{i_2}^\top \boldsymbol{\beta})} = \exp(\beta_j).
$$

Typowy wynik GLM z funkcją łączącą logarytmiczną jest następujący. Składka bazowa $\exp(\beta_0) = 100$ jest określana dla profilu referencyjnego w portfelu: powiedzmy, kierowca w średnim wieku, z dużym doświadczeniem, mieszkający w dużym mieście. Następnie, składki dla innych profili są uzyskiwane z

$$\begin{align*}
\text{składka bazowa } 100 &\times 0.9 \text{ dla kobiety-kierowcy} \\
&\times 1.3 \text{ dla młodego, niedoświadczonego kierowcy} \\
&\times 0.95 \text{ dla starszego kierowcy} \\
&\times 0.9 \text{ dla kierowcy mieszkającego na przedmieściach} \\
&\times 0.85 \text{ dla kierowcy mieszkającego na wsi} \\
& \quad \text{ i tak dalej.}
\end{align*}$$

Czynniki 0.9, 1.3, 0.95, 0.9 i 0.85 wchodzące w skład ostatniego wzoru odpowiadają oszacowanym względnościom $\exp(\hat{\beta}_j)$. Wyrażony w ten sposób, wynik analizy GLM jest w pełni przejrzysty i łatwy do zakomunikowania wewnątrz firmy, pośrednikom, a także posiadaczom polis. Dlatego większość komercyjnych cenników ma strukturę multiplikatywną.

#### 4.2.4.4 Łącza Logit, Probit i Log-Log

Dla proporcji dwumianowych, średnia odpowiedź odpowiada prawdopodobieństwu sukcesu, tak że $\mu_i$ jest ograniczona do przedziału jednostkowego $[0, 1]$. Funkcje rozkładu są naturalnymi kandydatami do transformacji wyniku o wartościach rzeczywistych na średnią odpowiedź. Często stosowano rozkłady logistyczne i normalne, co dało początek tak zwanym funkcjom łączącym logit i probit. Komplementarna funkcja łącząca log-log jest dziedziczona z rozkładu Ekstremalnej Wartości.

Funkcje łączące logit, probit i komplementarna log-log odwzorowują przedział jednostkowy $[0, 1]$ na całą oś rzeczywistą, usuwając ograniczenie na średnią odpowiedź. Łącze logit

$$
\ln \frac{\mu_i}{1-\mu_i} = \text{score}_i \iff \mu_i = \frac{\exp(\text{score}_i)}{1+\exp(\text{score}_i)} = \frac{1}{1+\exp(-\text{score}_i)}
$$

jest często stosowane dla prawdopodobieństw $\mu_i$ w zastosowaniach aktuarialnych. Typowym GLM do modelowania retencji i konwersji nowego biznesu jest GLM dwumianowy z funkcją łączącą logit (znany również jako regresja logistyczna). Jeśli prawdopodobieństwo sukcesu jest bliskie 0 (tak, że odpowiedź jest generalnie 0), to możliwe jest również użycie multiplikatywnego GLM Poissona jako aproksymacji, biorąc pod uwagę, że wynik multiplikatywnego GLM jest łatwiejszy do zakomunikowania nietechnicznej publiczności. Dzieje się tak, ponieważ masa prawdopodobieństwa jest prawie całkowicie skoncentrowana na $\{0, 1\}$, gdy średnia Poissona jest mała.

Gdy $x_{ij} \in \{0, 1\}$ dla wszystkich $j=1, \dots, p$, widzimy, że

$$
\frac{\mu_i}{1-\mu_i} = \exp(\beta_0) \prod_{j|x_{ij}=1} \exp(\beta_j).
$$

Cechy $x_{ij}$ wchodzą zatem w tak zwany iloraz szans $\mu_i / (1-\mu_i)$ w sposób multiplikatywny. Jeśli $\mu_i$ jest małe, tak że $1-\mu_i \approx 1$, widzimy, że

$$
\mu_i \approx \exp(\beta_0) \prod_{j|x_{ij}=1} \exp(\beta_j)
$$

więc modele Poissona i regresji logistycznej dają podobne wyniki w tym przypadku. Model probit wykorzystuje dystrybuantę $\Phi$ rozkładu $\mathcal{Nor}(0, 1)$ do odwzorowania przedziału jednostkowego $[0, 1]$. Komplementarna funkcja łącząca log-log wykorzystuje rozkład Ekstremalnej (minimum) Wartości w tym celu. Ten link dokładnie łączy GLM-y Poissona i dwumianowe, jak pokazano dalej. Rozważmy $Y \sim \mathcal{Poi}(\mu)$ i zdefiniujmy obciętą odpowiedź $\tilde{Y} = \min\{1, Y\}$.

Jeśli dla obciętej odpowiedzi $Y$ zastosowano funkcję łączącą logarytmiczną, tj. $\ln \mu = \text{score}$, to

$$
\begin{aligned}
\mathrm{E}[\tilde{Y}] &= 1 - \mathrm{P}[\tilde{Y}=0] \\
&= 1 - \exp(-\mu) \\
&= 1 - \exp(-\exp(\text{score}))
\end{aligned}
$$

gdzie rozpoznajemy komplementarną funkcję łączącą log-log. Stąd, komplementarna funkcja łącząca log-log łączy GLM-y Poissona i Bernoulliego. Ta konstrukcja pozwala również aktuariuszowi uwzględnić różne ekspozycje na ryzyko w modelu regresyjnym dla cech binarnych, poprzez określenie

$$
\mathrm{E}[\tilde{Y}] = 1 - \exp(-\exp(\ln e + \text{score}))
$$

gdzie ekspozycja na ryzyko $e$ staje się widoczna. Dokładniej, logarytm ekspozycji jest dodawany do oceny jako przesunięcie. Ten koncept odgrywa kluczową rolę w wielu zastosowaniach ubezpieczeniowych. Jest badany szczegółowo w Sekcji 4.3.7.

#### 4.2.4.5 Łącza Kanoniczne

Gdy funkcja łącząca sprawia, że ocena jest taka sama jak parametr kanoniczny, nazywa się to łączem kanonicznym. Formalnie, kanoniczna funkcja łącząca jest taka, że tożsamość

$$
\theta_i = \text{score}_i
$$

jest prawdziwa. Ponieważ $g(\mu_i) = \text{score}_i$ i $\mu_i = a'(\theta_i)$, łącze kanoniczne jest zdefiniowane jako

$$
\theta_i = \text{score}_i \iff g(a'(\theta_i)) = \theta_i \implies g^{-1} = a'.
$$

Zatem, funkcja kumulacyjna $a(\cdot)$ określa kanoniczną funkcję łączącą, tak że każdy rozkład ED posiada własne specyficzne łącze kanoniczne. Na przykład, w przypadku Poissona wiemy, że $a(\theta) = \exp(\theta)$. Stąd kanoniczna funkcja łącząca jest $a'(\theta) = \exp(\theta)$, co daje funkcję łączącą logarytmiczną. Tabela 4.4 podsumowuje kanoniczne funkcje łączące dla głównych rozkładów ED.

Kanoniczny parametr znacznie upraszcza analizę opartą na GLM, ale inne funkcje łączące mogą być również używane. Jest to właśnie wielka siła podejścia GLM. Często w przeszłości aktuariusze przekształcali odpowiedź przed normalną analizą regresji liniowej (taką jak modelowanie logarytmiczno-normalne). Jednak ta sama transformacja musiała jednocześnie uczynić przekształconą odpowiedź w przybliżeniu normalnie rozłożoną i średnią przekształconej odpowiedzi liniową w zmiennych objaśniających, tj.

$$
\mathrm{E}[g(Y_i)] = \beta_0 + \sum_{j=1}^p \beta_j x_{ij}.
$$

W GLM modelowana jest przekształcona wartość oczekiwana odpowiedzi, tj.

$$
g(\mathrm{E}[Y_i]) = \beta_0 + \sum_{j=1}^p \beta_j x_{ij}
$$

tak że jedyną rolą $g$ jest uczynienie oczekiwanej odpowiedzi liniową w cechach $x_{ij}$ w tej konkretnej skali (zwanej skalą oceny), a nie renderowanie odpowiedzi $Y_i$ w przybliżeniu normalnie rozłożonej w zmodyfikowanej skali. W GLM rozkład odpowiedzi jest wybierany z rodziny ED zgodnie z charakterystyką obserwowanych danych. Nie ma a priori transformacji odpowiedzi, tak aby podlegała ona danemu rozkładowi. Gdy tylko funkcja łącząca $g$ nie jest liniowa, te dwa podejścia różnią się (choć są podobne w duchu).

---
**Tabela 4.4** Przykłady kanonicznych funkcji łączących. Zauważ, że kanoniczne łącze to $\mu^\xi$ dla rozkładów Gamma i Odwrotnego Gaussa, ale znak minus jest generalnie usuwany, a także dzielenie przez 2 w przypadku Odwrotnego Gaussa.

| Funkcja łącząca | $g(\mu)$ | Kanoniczne łącze dla |
|---|---|---|
| Tożsamościowa | $\mu$ | Normalny |
| Logarytmiczna | $\ln \mu$ | Poisson |
| Potęgowa | $\mu^\xi$ | Gamma ($\xi = -1$) |
| | | Odwrotny Gauss ($\xi = -2$) |
| Logit | $\ln \frac{\mu}{1-\mu}$ | Dwumianowy |

### 4.2.5 Interakcje

Interakcja powstaje, gdy wpływ określonego czynnika ryzyka zależy od wartości innego. Przykładem w ubezpieczeniach komunikacyjnych jest zależność wieku kierowcy od płci: często młode kobiety-kierowcy powodują mniej roszczeń w porównaniu z młodymi mężczyznami, podczas gdy ta różnica płci zanika (a czasem nawet odwraca się) w starszym wieku. Stąd wpływ wieku zależy od płci. W przeciwieństwie do technik regresji rozważanych w kolejnych tomach, takich jak metody oparte na drzewach, GLM-y nie uwzględniają automatycznie interakcji, a takie efekty muszą być ręcznie wprowadzane do oceny.

Aby właściwie zrozumieć interakcje, pomyśl o rozwinięciu Taylora średniej funkcji w skali oceny:

$$
g(\mathrm{E}[Y | \boldsymbol{X} = \boldsymbol{x}]) \approx \beta_0 + \sum_{j=1}^p \beta_j x_{ij}
$$

kiedy $\frac{\partial^2}{\partial x_j \partial x_k} g(\mathrm{E}[Y | \boldsymbol{X} = \boldsymbol{x}]) = 0$. W tym ustawieniu,

$$
\beta_0 = g(\mathrm{E}[Y | \boldsymbol{X} = \boldsymbol{0}])
$$

i

$$
\beta_j = \left. \frac{\partial}{\partial x_j} g(\mathrm{E}[Y | \boldsymbol{X} = \boldsymbol{x}]) \right|_{\boldsymbol{x}=\boldsymbol{0}}.
$$

Oznacza to, że ocena zawiera tylko główne efekty cech i jest zatem addytywna. Teraz, jeśli cechy oddziałują na siebie, drugie mieszane pochodne cząstkowe nie są już zerowe, a dokładniejsza aproksymacja prawdziwej funkcji regresji jest dana przez

$$
g(\mathrm{E}[Y | \boldsymbol{X} = \boldsymbol{x}]) \approx \beta_0 + \sum_{j=1}^p \beta_j x_{ij} + \sum_{j=1}^p \sum_{k \ge j+1} \gamma_{jk} x_{ij} x_{ik}
$$

gdzie

$$
\gamma_{jk} = \left. \frac{\partial^2}{\partial x_j \partial x_k} g(\mathrm{E}[Y | \boldsymbol{X} = \boldsymbol{x}]) \right|_{\boldsymbol{x}=\boldsymbol{0}}.
$$

Zauważ, że w tym podejściu, termin interakcji może istnieć tylko wtedy, gdy uwzględnione są również główne terminy. Dlatego generalnie zaleca się włączanie interakcji tylko wtedy, gdy w ocenie wchodzą również efekty główne.

Rozważmy model z dwiema ciągłymi cechami i oceną postaci

$$
\text{score}_i = \beta_0 + \beta_1 x_{i1} + \beta_2 x_{i2} + \beta_3 x_{i1} x_{i2}.
$$

Ocena liniowa obejmuje trzecią cechę $x_{i3} = x_{i1} x_{i2}$, zwaną terminem interakcji, zbudowaną z dwóch pierwszych. Włączenie iloczynu $x_{i1} x_{i2}$ do oceny indukuje interakcje, ponieważ wzrost o jednostkę $x_{i1}$ zwiększa ocenę liniową o $\beta_1 + \beta_3 x_{i2}$, a zatem wpływ pierwszej cechy zależy od drugiej. Podobnie, wpływ wzrostu o jednostkę $x_{i2}$ wynosi $\beta_2 + \beta_3 x_{i1}$, co zależy od $x_{i1}$. Te dwie cechy oddziałują na siebie. Zmienne $x_{i1}$ i $x_{i2}$ nazywane są efektami głównymi i odróżnia się je od terminu interakcji $x_{i3} = x_{i1} x_{i2}$.

Iloczyn $x_{i1} x_{i2}$ wchodzący do oceny pochodzi z ograniczonego rozwinięcia Taylora bardziej ogólnej funkcji kombinacji $h(x_{i1}, x_{i2})$ w skali oceny. W tym sensie, GAM-y wprowadzone w Rozdz. 6 pozwalają aktuariuszowi na włączenie bardziej elastycznych interakcji do modelu poprzez estymację gładkich funkcji $h(\cdot, \cdot)$. Interakcje są również skutecznie przechwytywane za pomocą metod opartych na drzewach (prezentowanych w Trufin et al. 2019) i sieci neuronowych (prezentowanych w Hainaut et al. 2019).

Interakcja zwykłej cechy ilościowej z cechą binarną jest również często przydatna w zastosowaniach ubezpieczeniowych. Załóżmy, że $x_{i1}$ jest ciągła, a $x_{i2}$ jest wskaźnikiem przyjmującym wartości 0 i 1. Wtedy ocena jest dana przez

$$
\begin{aligned}
\text{score}_i &= \beta_0 + \beta_1 x_{i1} + \beta_2 x_{i2} + \beta_3 x_{i1} x_{i2} \\
&= \begin{cases} \beta_0 + \beta_1 x_{i1} & \text{jeśli } x_{i2} = 0 \\ \beta_0 + \beta_2 + (\beta_1 + \beta_3) x_{i1} & \text{jeśli } x_{i2} = 1. \end{cases}
\end{aligned}
$$

Wpływ $x_{i1}$ (to jest, wyraz wolny i nachylenie) zależy od grupy wskazanej przez $x_{i2}$.

### 4.2.6 Podsumowanie Założeń dla Regresji z GLM-ami

Podsumowując, podejście GLM do modelowania danych ubezpieczeniowych wychodzi z następujących założeń:

- **Odpowiedź $Y_i$ podlegająca rozkładowi ED** z funkcją masy prawdopodobieństwa lub funkcją gęstości prawdopodobieństwa postaci
    $$
    f_{\theta_i}(y) = \exp\left(\frac{y\theta_i - a(\theta_i)}{\phi/v_i}\right) c(y, \phi/v_i)
    $$

    ze wspólnym parametrem dyspersji $\phi$ i określonymi wagami $v_i$.

- **Ocena liniowa**
    $$
    \text{score}_i = \text{score}(\boldsymbol{x}_i) = \boldsymbol{x}_i^\top \boldsymbol{\beta} = \beta_0 + \sum_{j=1}^p \beta_j x_{ij}.
    $$

- **Modelowanie średniej:** oczekiwana odpowiedź $\mu_i = \mu(\boldsymbol{x}_i)$ postaci
    $$
    \begin{aligned}
    a'(\theta_i) &= \mu_i \\
    g(\mu_i) &= \text{score}_i \iff \mu_i = g^{-1}(\text{score}_i) \\
    g &\in \{\text{ln, logit, \dots}\}.
    \end{aligned}
    $$

- **Monotoniczna funkcja łącząca $g$** jest określana przez aktuariusza, a nie estymowana na podstawie danych.

Zauważ, że wybór rozkładu ED sprowadza się do określenia funkcji kumulacyjnej $a(\cdot)$, więc jest to równoważne z wyborem funkcji wariancji $V(\cdot) = a''(\cdot)$ odzwierciedlającej zależność średnia-wariancja popartą rozważanymi danymi.

## 4.3 Równania Wiarygodności w GLM

### 4.3.1 Postać Ogólna

Gdy GLM zostanie określony pod względem funkcji łączącej i rozkładu ED, lub równoważnie, gdy funkcja wariancji zostanie wybrana przez aktuariusza, estymowane parametry są uzyskiwane metodą największej wiarygodności (przypomnianą w Rozdz. 3). Dostarcza to aktuariuszowi nie tylko estymat współczynników regresji $\beta_j$, ale także estymowanych błędów standardowych dla dużych prób. W istocie, to podejście dąży do estymacji GLM, który ma najwyższe prawdopodobieństwo wygenerowania rzeczywistych danych.

Niektóre z rozkładów ED, na których opierają się GLM, zawierają nieznany parametr dyspersji $\phi$. Chociaż ten parametr mógłby być również estymowany metodą największej wiarygodności, zamiast tego generalnie stosuje się estymator momentów, w drugim kroku, gdy $\boldsymbol{\beta}$ zostanie już oszacowane. W rzeczywistości okaże się, że znajomość $\phi$ nie jest potrzebna do oszacowania $\boldsymbol{\beta}$, więc te dwa zbiory parametrów można traktować oddzielnie. Dlatego tutaj postrzegamy funkcję wiarygodności jako funkcję tylko $\boldsymbol{\beta}$.

Funkcja wiarygodności jest iloczynem prawdopodobieństw zaobserwowania wartości każdej odpowiedzi, z funkcją gęstości prawdopodobieństwa używaną w miejsce funkcji masy prawdopodobieństwa dla odpowiedzi ciągłych. Jak wyjaśniono w Rozdz. 3, zwykle przełącza się na logarytm wiarygodności, ponieważ obejmuje on sumę, a nie iloczyn (a dowolna wartość parametru maksymalizująca logarytm wiarygodności maksymalizuje również samą wiarygodność). Zatem estymaty największej wiarygodności parametrów $\beta_0, \beta_1, \dots, \beta_p$ maksymalizują

$$
L(\boldsymbol{\beta}) = \sum_{i=1}^n \ln f_{\theta_i}(y_i) = \sum_{i=1}^n \frac{y_i\theta_i - a(\theta_i)}{\phi/v_i} + \text{stała względem } \boldsymbol{\beta}
$$

gdzie $\theta_i$ zależy od liniowej oceny $s_i = \boldsymbol{x}_i^\top \boldsymbol{\beta}$, ponieważ $\theta_i = g^{-1}(\boldsymbol{x}_i^\top \boldsymbol{\beta})$. Aby znaleźć estymatę największej wiarygodności dla $\beta$, $L$ jest różniczkowane względem $\beta_j$ i przyrównywane do 0. Używając reguły łańcuchowej, otrzymujemy

$$
\begin{aligned}
\frac{\partial}{\partial\beta_j}L(\boldsymbol{\beta}) &= \sum_{i=1}^n \frac{\partial}{\partial\beta_j}\left(\frac{y_i\theta_i - a(\theta_i)}{\phi/v_i}\right) \\
&= \sum_{i=1}^n \frac{\partial}{\partial\theta_i}\left(\frac{y_i\theta_i - a(\theta_i)}{\phi/v_i}\right) \frac{\partial\theta_i}{\partial\mu_i} \frac{\partial\mu_i}{\partial\beta_j} \\
&= \sum_{i=1}^n \left(\frac{y_i - \mu_i}{\phi/v_i}\right) \frac{\partial\theta_i}{\partial\mu_i} \frac{\partial\mu_i}{\partial\beta_j}.
\end{aligned}
$$

Teraz, jako że

$$
\frac{\partial\mu_i}{\partial\theta_i} = a''(\theta_i) = \frac{v_i \text{Var}[Y_i]}{\phi}
$$

$$
\frac{\partial\mu_i}{\partial\beta_j} = \frac{\partial\mu_i}{\partial s_i} \frac{\partial s_i}{\partial\beta_j} = \frac{\partial\mu_i}{\partial s_i} x_{ij}
$$

musimy rozwiązać

$$
\sum_{i=1}^n \frac{(y_i - \mu_i)x_{ij}}{\text{Var}[Y_i]} \frac{\partial\mu_i}{\partial s_i} = 0 \iff \sum_{i=1}^n \frac{(y_i - \mu_i)x_{ij}}{V(\mu_i)/v_i} \frac{\partial\mu_i}{\partial s_i} = 0.
\quad\quad\text{(4.1)}
$$

System (4.1) nie posiada jawnego rozwiązania i musi być rozwiązany numerycznie, przy użyciu algorytmu iteracyjnego. Zauważalnym wyjątkiem jest model regresji normalnej z funkcją łączącą tożsamościową, gdzie $\boldsymbol{\hat{\beta}}$ posiada analityczne wyrażenie. Rozwiązanie (4.1) zostanie wyjaśnione w Sekcji 4.4.

### 4.3.2 Informacja Fishera

Aby uzyskać informację Fishera, zapiszmy element $(j,k)$ macierzy informacji Fishera $I(\boldsymbol{\beta})$:

$$
\begin{aligned}
I_{jk}(\boldsymbol{\beta}) &= \mathrm{E}\left[\frac{\partial}{\partial\beta_j}L(\boldsymbol{\beta})\frac{\partial}{\partial\beta_k}L(\boldsymbol{\beta})\right] \\
&= \mathrm{E}\left[\sum_{i=1}^n \frac{(Y_i - \mu_i)x_{ij}}{\text{Var}[Y_i]} \frac{\partial\mu_i}{\partial s_i} \sum_{l=1}^n \frac{(Y_l - \mu_l)x_{lk}}{\text{Var}[Y_l]} \frac{\partial\mu_l}{\partial s_l}\right] \\
&= \sum_{i=1}^n \sum_{l=1}^n \frac{\text{Cov}[Y_i, Y_l] x_{ij} x_{lk}}{\text{Var}[Y_i]\text{Var}[Y_l]} \frac{\partial\mu_i}{\partial s_i} \frac{\partial\mu_l}{\partial s_l}.
\end{aligned}
$$

Ponieważ odpowiedzi są niezależne, składniki kowariancji znikają, to jest, $\text{Cov}[Y_i, Y_l] = 0$ dla $i \ne l$. Zatem pozostają tylko składniki odpowiadające $i=l$. Otrzymujemy

$$
I_{jk}(\boldsymbol{\beta}) = \sum_{i=1}^n \frac{x_{ij}x_{ik}}{\text{Var}[Y_i]} \left(\frac{\partial\mu_i}{\partial s_i}\right)^2.
$$

Zdefiniujmy wagę pomocniczą

$$
\tilde{v}_i = \frac{1}{\text{Var}[Y_i]} \left(\frac{\partial\mu_i}{\partial s_i}\right)^2 = \frac{v_i}{\phi V(\mu_i)} \left(\frac{\partial\mu_i}{\partial s_i}\right)^2
\quad\quad\text{(4.2)}
$$

oraz macierz diagonalną $n \times n$ $\tilde{\boldsymbol{W}}$ z elementami $\tilde{v}_1, \tilde{v}_2, \dots, \tilde{v}_n$. W postaci macierzowej mamy wtedy

$$
I(\boldsymbol{\beta}) = \boldsymbol{X}^\top \tilde{\boldsymbol{W}} \boldsymbol{X} = \sum_{i=1}^n \tilde{v}_i \boldsymbol{x}_i \boldsymbol{x}_i^\top.
\quad\quad\text{(4.3)}
$$

### 4.3.3 Wpływ Funkcji Wariancji

Rozważając równanie wiarygodności GLM (4.1), ważne jest, aby zdać sobie sprawę, jak wybór funkcji wariancji $V(\cdot)$ wpływa na dopasowanie modelu. Widzimy, że im większa wariancja $V(\mu_i)$, tym mniejszy wkład obserwacji $i$ do sumy pojawiającej się w równaniach wiarygodności. Innymi słowy, dopuszczalne są większe różnice $y_i - \mu_i$, gdy $\mu_i$ jest duże.

Z Rozdziału 2 wiemy, że typowe funkcje wariancji mają postać Tweediego $V(\mu) = \mu^\xi$. W rodzinie Tweediego:
- Dla $\xi=0$ (przypadek normalny), każda obserwacja ma taką samą ustaloną wariancję niezależnie od jej średniej, a dopasowane wartości są jednakowo przyciągane do obserwowanych punktów danych.
- Dla $\xi=1$ (przypadek Poissona), wariancja jest proporcjonalna do oczekiwanej wartości każdej obserwacji: obserwacje z mniejszą oczekiwaną wartością będą miały mniejszą założoną wariancję, co skutkuje większą wagą (lub wiarygodnością) przy estymacji parametrów. Dopasowanie modelu jest zatem bardziej pod wpływem obserwacji o mniejszej oczekiwanej wartości niż o większej oczekiwanej wartości (a więc większej założonej wariancji).
- Gdy $\xi=2$ (przypadek Gamma) i $\xi=3$ (przypadek odwrotny Gaussa), dopasowanie GLM jest jeszcze bardziej pod wpływem obserwacji o mniejszych oczekiwanych wartościach, tolerując większe błędy dla obserwacji o większych oczekiwanych wartościach.

Wagi a priori $v_i$ również działają na wariancję i skutkują tym, że punkty danych mają mniejszy wpływ na dopasowanie modelu. Rzeczywiście, zwiększenie wagi zmniejsza wariancję, a odpowiadająca jej obserwacja odgrywa ważniejszą rolę w dopasowaniu modelu.

Wybór funkcji wariancji jest zatem sprawą najwyższej wagi, aby uzyskać dokładne dopasowanie modelu. Jak zobaczymy, stosując techniki quasi-wiarygodności, techniki oparte na GLM wydają się być dość odporne na błędy w rozkładzie, o ile funkcja wariancji została poprawnie określona.

### 4.3.4 Równania Wiarygodności i Informacja Fishera z Łączem Kanonicznym

Z kanoniczną funkcją łączącą, parametr kanoniczny jest równy ocenie: $\theta_i = s_i$. W tym przypadku mamy

$$
\frac{\partial\mu_i}{\partial s_i} = \frac{\partial\mu_i}{\partial\theta_i} = \frac{\partial a'(\theta_i)}{\partial\theta_i} = a''(\theta_i).
$$

Stąd (4.1) dalej się upraszcza i musimy rozwiązać

$$
\frac{\partial}{\partial\beta_j}L(\boldsymbol{\beta}) = 0 \iff \sum_{i=1}^n \left(\frac{y_i - \mu_i}{\phi/v_i}\right) x_{ij} = 0.
$$

Jeśli $v_i=1$ dla wszystkich $i$, to równania wiarygodności zapewniają, że dopasowane wartości średnie

$$
\hat{\mu}_i = \mu_i(\hat{\boldsymbol{\beta}}) = g^{-1}(\boldsymbol{x}_i^\top \hat{\boldsymbol{\beta}})
$$

spełniają warunek

$$
\sum_{i=1}^n (y_i - \hat{\mu}_i) x_{ij} = 0 \iff \sum_{i=1}^n y_i x_{ij} = \sum_{i=1}^n \hat{\mu}_i x_{ij} \quad \text{dla } j=0, 1, \dots, p.
\quad\quad\text{(4.4)}
$$

Można to przepisać jako $\boldsymbol{X}^\top (\boldsymbol{y} - \hat{\boldsymbol{\mu}}) = \boldsymbol{0}$ w postaci macierzowej. Ogólnie, zdefiniujmy $\boldsymbol{W}$ jako macierz diagonalną z $i$-tym elementem $v_i$, to jest,

$$
\boldsymbol{W} = \begin{pmatrix}
v_1 & 0 & \dots & 0 \\
0 & v_2 & \dots & 0 \\
\vdots & \vdots & \ddots & \vdots \\
0 & 0 & \dots & v_n
\end{pmatrix}.
$$

Z kanoniczną funkcją łączącą, równania wiarygodności są

$$
\boldsymbol{X}^\top \boldsymbol{W} (\boldsymbol{y} - \boldsymbol{\mu}(\hat{\boldsymbol{\beta}})) = \boldsymbol{0}.
\quad\quad\text{(4.5)}
$$

Maksymalizacja logarytmu wiarygodności jest dobrze postawiona z kanonicznymi funkcjami łączącymi. Dzieje się tak, ponieważ z kanoniczną funkcją łączącą, logarytm wiarygodności jest dany przez

$$
L(\boldsymbol{\beta}) = \frac{1}{\phi} \sum_{i=1}^n v_i(y_i s_i - a(s_i)) + \text{stała względem } \boldsymbol{\beta}
$$

co jest wklęsłe w $\boldsymbol{\beta}$ (ponieważ $a$ jest wypukłe, a ocena $s_i$ jest liniowa w $\boldsymbol{\beta}$).

Inna interesująca właściwość kanonicznych funkcji łączących dotyczy macierzy informacji Fishera. Gdy wybrano kanoniczną funkcję łączącą, otrzymujemy

$$
\begin{aligned}
\frac{\partial^2}{\partial\beta_j \partial\beta_k}L(\boldsymbol{\beta}) &= -\sum_{i=1}^n \frac{v_i x_{ij}}{\phi} \frac{\partial\mu_i}{\partial\beta_k} \\
&= -\sum_{i=1}^n \frac{v_i x_{ij}}{\phi} \frac{\partial\mu_i}{\partial s_i} x_{ik} \\
&= -\sum_{i=1}^n \frac{v_i x_{ij} x_{ik}}{\phi} a''(\theta_i).
\end{aligned}
$$

Dlatego element $(j, k)$ obserwowanej macierzy informacyjnej jest dany przez

$$
H_{jk}(\boldsymbol{\beta}) = \sum_{i=1}^n \frac{x_{ij}x_{ik}}{\text{Var}[Y_i]} (a''(\theta_i))^2
$$

co nie zależy od odpowiedzi. Zakładając $\theta_i = s_i$, mamy zatem

$$
\frac{\partial^2}{\partial\beta_j \partial\beta_k}L(\boldsymbol{\beta}) = \mathrm{E}\left[\frac{\partial^2}{\partial\beta_j \partial\beta_k}L(\boldsymbol{\beta})\right]
$$

tak że oczekiwane i obserwowane macierze informacyjne pokrywają się.

### 4.3.5 Dane Indywidualne czy Grupowe

Rozważmy dane przedstawione w Tabeli 4.5. Interesujący nas portfel został pogrupowany w 4 klasy według dwóch cech binarnych kodujących informacje dostępne o każdym posiadaczu polisy (płeć i roczny dystans przejechany poniżej 20 000 km rocznie). Odpowiedź $Y_i$ jest liczbą szkód zgłoszonych przez posiadacza polisy $i$. Zakładamy, że $Y_1, \dots, Y_n$ są niezależne i mają rozkład Poissona z odpowiednimi średnimi $\mu_1, \dots, \mu_n$. Oczekiwana liczba szkód dla polisy $i$ w portfelu jest wyrażona jako

$$
\mu_i = e_i \exp(\beta_0 + \beta_1 x_{i1} + \beta_2 x_{i2})
$$

gdzie $e_i$ jest ekspozycją na ryzyko dla posiadacza polisy $i$, $x_{i1}$ rejestruje roczny przebieg, a $x_{i2}$ rejestruje płeć. Dokładniej,

$$
x_{i1} = \begin{cases} 1 & \text{jeśli posiadacz polisy } i \text{ przejeżdża mniej niż 20,000 km rocznie} \\ 0 & \text{w przeciwnym razie}, \end{cases}
$$

i

$$
x_{i2} = \begin{cases} 1 & \text{jeśli posiadacz polisy } i \text{ jest kobietą}, \\ 0 & \text{w przeciwnym razie}. \end{cases}
$$

Poziomy referencyjne odpowiadają najbardziej zaludnionym kategoriom w zbiorze danych: ponad 20 000 km dla dystansu ($x_{i1}=0$) i mężczyzna dla płci ($x_{i2}=0$).

Tabela 4.5 podsumowuje doświadczenie całego portfela w zaledwie czterech liczbach: po jednej dla każdej klasy ryzyka (to jest, agregując wszystkie polisy, które są identyczne pod względem dwóch cech $x_{i1}$ i $x_{i2}$). Pokażmy teraz, że takie grupowanie jest dozwolone podczas pracy z odpowiedziami o rozkładzie Poissona (i ogólniej, z dowolnymi odpowiedziami o rozkładzie ED). Można to łatwo zobaczyć w następujący sposób. Zacznijmy od indywidualnych rekordów $y_i$ dla posiadacza polisy $i$. Tutaj $y_i$ oznacza obserwowaną liczbę szkód dla posiadacza polisy $i$ dla ekspozycji $e_i$. Odpowiadająca funkcja wiarygodności to

---
**Tabela 4.5** Liczba szkód (ekspozycja na ryzyko) i odpowiadające im ekspozycje na ryzyko (w osobolatach, w nawiasach) dla hipotetycznego portfela ubezpieczeń komunikacyjnych obserwowanego w ciągu jednego roku kalendarzowego.

<table border="1" style="border-collapse: collapse; width: 100%;">
  <thead>
    <tr>
      <th rowspan="2" style="text-align: left; padding: 8px;">Płeć</th>
      <th colspan="2" style="text-align: center; padding: 8px;">Roczny przejechany dystans</th>
    </tr>
    <tr>
      <th style="text-align: center; padding: 8px;">&lt;20 000 km</th>
      <th style="text-align: center; padding: 8px;">&ge;20 000 km</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align: left; padding: 8px;">Mężczyźni</td>
      <td style="text-align: center; padding: 8px;">143<br>(2 000)</td>
      <td style="text-align: center; padding: 8px;">1 967<br>(18 000)</td>
    </tr>
    <tr>
      <td style="text-align: left; padding: 8px;">Kobiety</td>
      <td style="text-align: center; padding: 8px;">278<br>(6 000)</td>
      <td style="text-align: center; padding: 8px;">354<br>(4 000)</td>
    </tr>
  </tbody>
</table>

$$
\mathcal{L}_{ind}(\beta_0, \beta_1, \beta_2) = \prod_{i=1}^n \exp(-\mu_i) \frac{\mu_i^{y_i}}{y_i!}.
$$

Czynniki pojawiające się w iloczynie można uporządkować według 4 kategorii w Tabeli 4.5. Dokładniej, $\mathcal{L}_{ind}(\beta_0, \beta_1, \beta_2)$ można przepisać jako

$$
\begin{aligned}
\mathcal{L}_{ind} &= \prod_{i|x_{i1}=x_{i2}=0} \exp(-\mu_i) \frac{\mu_i^{y_i}}{y_i!} \prod_{i|x_{i1}=0, x_{i2}=1} \exp(-\mu_i) \frac{\mu_i^{y_i}}{y_i!} \\
&\quad \prod_{i|x_{i1}=1, x_{i2}=0} \exp(-\mu_i) \frac{\mu_i^{y_i}}{y_i!} \prod_{i|x_{i1}=1, x_{i2}=1} \exp(-\mu_i) \frac{\mu_i^{y_i}}{y_i!} \\
&\propto \exp\left(-\exp(\beta_0)\sum_{i|x_{i1}=x_{i2}=0} e_i\right) (\exp(\beta_0))^{\sum_{i|x_{i1}=x_{i2}=0} k_i} \\
&\quad \exp\left(-\exp(\beta_0+\beta_2)\sum_{i|x_{i1}=0, x_{i2}=1} e_i\right) (\exp(\beta_0+\beta_2))^{\sum_{i|x_{i1}=0, x_{i2}=1} k_i} \\
&\quad \exp\left(-\exp(\beta_0+\beta_1)\sum_{i|x_{i1}=1, x_{i2}=0} e_i\right) (\exp(\beta_0+\beta_1))^{\sum_{i|x_{i1}=1, x_{i2}=0} k_i} \\
&\quad \exp\left(-\exp(\beta_0+\beta_1+\beta_2)\sum_{i|x_{i1}=1, x_{i2}=1} e_i\right) (\exp(\beta_0+\beta_1+\beta_2))^{\sum_{i|x_{i1}=1, x_{i2}=1} k_i},
\end{aligned}
$$

gdzie „$\propto$” wskazuje, że dwa wyrażenia są proporcjonalne aż do stałych czynników (nie obejmujących żadnego współczynnika regresji $\beta_j$ do oszacowania). To pokazuje, że $\mathcal{L}_{ind}(\beta_0, \beta_1, \beta_2)$ jest proporcjonalna do wiarygodności opartej na danych zagregowanych dla 4 klas ryzyka, ponieważ angażuje tylko całkowitą ekspozycję i całkowitą liczbę szkód dla każdej klasy ryzyka (tj. dane przedstawione w Tabeli 4.5). Zatem, praca z indywidualnymi danymi $Y_1, \dots, Y_n$ lub z czterema zagregowanymi zliczeniami

$$
\begin{aligned}
Y_{00} &= \sum_{i|x_{i1}=x_{i2}=0} Y_i \\
Y_{01} &= \sum_{i|x_{i1}=0,x_{i2}=1} Y_i \\
Y_{10} &= \sum_{i|x_{i1}=1,x_{i2}=0} Y_i \\
Y_{11} &= \sum_{i|x_{i1}=1,x_{i2}=1} Y_i
\end{aligned}
$$

daje procedury wiarygodności, które są proporcjonalne, tak że oszacowania największej wiarygodności są dokładnie takie same. Z rozkładem Poissona wiemy, że cztery sumy $Y_{00}, Y_{01}, Y_{10}$ i $Y_{11}$ są nadal Poissona. Dokładniej,

$$
\begin{aligned}
Y_{00} &\sim \mathcal{Poi}(\lambda_{00}) \text{ z } \lambda_{00} = e_{00}\exp(\beta_0) \text{ gdzie } e_{00} = \sum_{i|x_{i1}=x_{i2}=0} e_i \\
Y_{01} &\sim \mathcal{Poi}(\lambda_{01}) \text{ z } \lambda_{01} = e_{01}\exp(\beta_0+\beta_2) \text{ gdzie } e_{01} = \sum_{i|x_{i1}=0,x_{i2}=1} e_i \\
Y_{10} &\sim \mathcal{Poi}(\lambda_{10}) \text{ z } \lambda_{10} = e_{10}\exp(\beta_0+\beta_1) \text{ gdzie } e_{10} = \sum_{i|x_{i1}=1,x_{i2}=0} e_i \\
Y_{11} &\sim \mathcal{Poi}(\lambda_{11}) \text{ z } \lambda_{11} = e_{11}\exp(\beta_0+\beta_1+\beta_2) \text{ gdzie } e_{11} = \sum_{i|x_{i1}=1,x_{i2}=1} e_i.
\end{aligned}
$$

Stąd funkcja wiarygodności $\mathcal{L}_{group}(\beta_0, \beta_1, \beta_2)$ związana z danymi zgrupowanymi jest iloczynem czterech prawdopodobieństw Poissona, to jest,

$$
\begin{aligned}
\mathcal{L}_{group} &= \exp(-\ e_{00}\exp(\beta_0)) \frac{(e_{00}\exp(\beta_0))^{y_{00}}}{y_{00}!} \\
&\quad \exp(-\ e_{01}\exp(\beta_0+\beta_2)) \frac{(e_{01}\exp(\beta_0+\beta_2))^{y_{01}}}{y_{01}!} \\
&\quad \exp(-\ e_{10}\exp(\beta_0+\beta_1)) \frac{(e_{10}\exp(\beta_0+\beta_1))^{y_{10}}}{y_{10}!} \\
&\quad \exp(-\ e_{11}\exp(\beta_0+\beta_1+\beta_2)) \frac{(e_{11}\exp(\beta_0+\beta_1+\beta_2))^{y_{11}}}{y_{11}!}.
\end{aligned}
$$

Oczywiście,

$$
\mathcal{L}_{ind}(\beta_0, \beta_1, \beta_2) \propto \mathcal{L}_{group}(\beta_0, \beta_1, \beta_2)
$$

więc możemy pracować z zagregowanymi danymi do celów estymacji, to jest, agregując doświadczenie całego portfela na poziomie 4 jednorodnych klas ryzyka, co nie skutkuje utratą informacji.

Należy jednak podkreślić, że ta właściwość jest ważna tylko pod założeniem ED dla odpowiedzi. Można to postrzegać jako zastosowanie Właściwości 2.4.6 zapewniającej, że ważone średnie obserwacji podlegających rozkładowi ED nadal podlegają temu samemu rozkładowi z zaktualizowanym parametrem dyspersji i zaktualizowaną wagą. Ogólnie, z rozkładami spoza rodziny ED, agregowanie danych może skutkować utratą informacji, dlatego aktuariusz powinien zawsze powstrzymywać się od postępowania w ten sposób, i przechowywać bazy danych rejestrujące indywidualne doświadczenia posiadaczy polis, gdy tylko jest to możliwe.

### 4.3.6 Sumy krańcowe pod łączem kanonicznym

Podajmy teraz intuicyjne wyjaśnienie dla równań wiarygodności z łączem kanonicznym (4.4). Pierwsze równanie wiarygodności (odpowiadające $j=0$) zapewnia, że globalna równowaga

$$
\underbrace{\sum_{i=1}^n \hat{\mu}_i}_{\text{dopasowana suma}} = \underbrace{\sum_{i=1}^n y_i}_{\text{obserwowana suma}}
\quad\quad\text{(4.6)}
$$

musi być zachowana. Dlatego, pod warunkiem, że w ocenach uwzględniony jest wyraz wolny $\beta_0$, suma dopasowanych wartości uzyskanych z modelu regresji jest równa jego obserwowanej sumie. Równość obowiązuje dla okresu obserwacji i niekoniecznie dla przyszłości, ale estymacje są wiarygodne tak długo, jak przyszłe doświadczenie jest podobne do przeszłego (założenie stacjonarności), ewentualnie z uwzględnieniem korekt na inflację lub inne przewidywalne efekty. Włączenie wyrazu wolnego $\beta_0$ jest zatem zasadą w zastosowaniach aktuarialnych, o ile globalna równowaga (4.6) ma sens.

Aby zrozumieć rzeczywiste znaczenie pozostałych równań wiarygodności GLM z łączem kanonicznym, rozważmy ponownie dane przedstawione w Tabeli 4.5, z $x_{i1}$ kodującym przejechany dystans (równe 1, jeśli mniej niż 20 000 km rocznie) i $x_{i2}$ kodującym płeć (równe 1, jeśli posiadaczem polisy jest kobieta). Wtedy (4.4) pokazuje, że $\hat{\boldsymbol{\beta}}$ jest takie, że

$$
\sum_{<20,000\text{ kms}} y_i = \sum_{<20,000\text{ kms}} \hat{\mu}_i
$$

i

$$
\sum_{\text{kobiety}} y_i = \sum_{\text{kobiety}} \hat{\mu}_i
$$

oba zachodzą. Oznacza to, że model pasuje dokładnie do całkowitych roszczeń zgłoszonych przez posiadaczy polis przejeżdżających mniej niż 20 000 km rocznie, z jednej strony, i przez kobiety posiadające polisy, z drugiej strony. Zatem, wraz z globalną równowagą (4.6) gwarantowaną przez włączenie wyrazu wolnego do oceny, równania wiarygodności zapewniają, że nie ma subsydiowania krzyżowego między posiadaczami polis przejeżdżającymi mniej niż 20 000 km rocznie a tymi przejeżdżającymi więcej, ani między mężczyznami a kobietami.

Jest to równoważne podejściu aktuarialnemu sum krańcowych (MMT). Idea polega na tym, że akceptowalny zestaw względności powinien odtwarzać doświadczenie dla każdego poziomu czynników ryzyka, a także ogólne doświadczenie, tj. być zrównoważonym na każdym poziomie i w całości. To podejście poprzedza GLM-y w praktyce aktuarialnej. Zostało zaproponowane w latach 60. przez północnoamerykańskich aktuariuszy.

**Uwaga 4.3.1** (Od technicznego do komercyjnego cennika). Równanie wiarygodności (4.4) narzuca naturalne ograniczenia od przełączania z technicznego na komercyjny cennik, unikając premiowych transferów między kategoriami w portfelu. Całe komercyjne pozycjonowanie może być narzucone przez odpowiednie ograniczenie niektórych współczynników regresji. Zmienną odpowiedzi jest techniczna składka $\mu(\boldsymbol{X})$, a zmiennymi objaśniającymi są tylko czynniki ratingowe. Ponieważ często pożądane są multiplikatywne składki premium, używana jest funkcja łącząca logarytmiczna, a odpowiadający rozkład odpowiedzi to Poisson. Wyjaśnia to, dlaczego założenie Poissona jest czasami przyjmowane dla odpowiedzi wykraczających poza zliczenia, nie dlatego, że założenie Poissona jest rozsądne, ale jako wygodny sposób implementacji ograniczeń MMT (4.4).

Dla każdego GLM z łączem kanonicznym istnieje dokładna równowaga między dopasowanymi i obserwowanymi zagregowanymi odpowiedziami w całym zbiorze danych, o ile ocena zawiera wyraz wolny. Jest to również prawdziwe dla każdego poziomu cech kategorycznych. Ta równowaga zachodzi dla regresji Poissona z łączem logarytmicznym lub dla regresji dwumianowej z łączem logitowym, nie wspominając o klasycznej regresji normalnej z łączem tożsamościowym. Zatem, po dopasowaniu jakiegokolwiek GLM, dopasowane wartości zawsze pasują do obserwacji w agregacie. Jednak te równowagi niekoniecznie są zachowane, gdy używane są łącza kanoniczne. Na przykład, ta równowaga nie jest zachowana w przypadku GLM Gamma z łączem logarytmicznym, co jest bardzo powszechnym modelem aktuarialnym. Powodem jest to, że łącze logarytmiczne nie jest kanoniczną funkcją łączącą dla rozkładu Gamma.

Ponieważ równania wiarygodności wyrażają globalne równowagi, sprawia to, że dopasowanie GLM jest stabilne w czasie z jednego zbioru danych do drugiego, o ile liczby roszczeń pozostają stabilne. Jest to atrakcyjna właściwość dla zastosowań ubezpieczeniowych.

### 4.3.7 Przesunięcia (Offsets)

#### 4.3.7.1 Definicja

Przesunięcia są używane w zdecydowanej większości badań aktuarialnych. Nie należy ich mylić z wagami, przesunięcia wchodzą do oceny addytywnie, bez mnożenia przez współczynnik regresji do oszacowania na podstawie danych. Przesunięcia reprezentują zatem znane wkłady i a priori informacje do oceny addytywnej. W przeciwieństwie do wyrazu wolnego $\beta_0$ wchodzącego do wszystkich ocen, przesunięcia zwykle różnią się w zależności od osoby.

W szczególności, przesunięcie jest wielkością dodawaną do oceny, bez mnożenia przez znany lub wstępnie określony współczynnik, który nie jest estymowany na podstawie danych. Zatem, przesunięcie można postrzegać jako dodatkową cechę, której współczynnik jest ograniczony, powiedzmy, do 1. Ocena staje się wtedy

$$
\text{score}_i = \text{offset}_i + \boldsymbol{x}_i^\top \boldsymbol{\beta}
$$

gdzie $\text{offset}_i$ oznacza przesunięcie specyficzne dla $i$-tej osoby.

W modelu regresji liniowej normalnej mamy

$$
Y_i = \text{offset}_i + \boldsymbol{x}_i^\top \boldsymbol{\beta} + Z_i \quad \text{gdzie } Z_i \sim \mathcal{Nor}(0, \sigma^2).
$$

Ponieważ przesunięcie jest znaną wielkością, jest to równoważne odjęciu tej wielkości od odpowiedzi $Y_i$ przed uruchomieniem regresji na $Y_i - \text{offset}_i$. Ta ostatnia wielkość może być traktowana jako reszta, do wyjaśnienia przez $\boldsymbol{x}_i$. To dlatego przesunięcia nie są omawiane w normalnej regresji liniowej z łączem tożsamościowym. Czasami przesunięcie odpowiada dopasowanym wartościom $\hat{\mu}_i$ dla średniej $Y_i$ uzyskanej z poprzedniego modelu regresji. Wtedy, przesunięcie jest równoważne użyciu reszt z wstępnej regresji odpowiedzi (która stanowi podstawę boosting, jak zobaczymy w Rozdz. 6).

#### 4.3.7.2 Zliczenia Poissona i Wskaźniki Poissona

Typowym zastosowaniem przesunięcia jest włączenie miary ekspozycji przy modelowaniu stawek. Na przykład, jeśli niektóre rekordy w ubezpieczeniach komunikacyjnych odpowiadają polisom obowiązującym przez 6 miesięcy, podczas gdy inne odpowiadają polisom obowiązującym przez cały rok, właściwe jest użycie z łączem logarytmicznym logarytmu okresu ubezpieczenia jako przesunięcia, jak wyjaśniono dalej. W modelu regresji Poissona, ekspozycja jest generalnie liczbą pojazdo-lat ubezpieczonych, a odpowiedzią jest liczba $Y_i$ zgłoszonych roszczeń. Biorąc pod uwagę proces Poissona z Rozdziału 2, wiemy, że ta ekspozycja mnoży roczną stopę szkód. Ponieważ ten przesunięcie musi być dodane w skali oceny, logarytm ekspozycji jest używany jako przesunięcie: oznaczając ekspozycję jako $e_i$,

$$
\ln \mu_i(\boldsymbol{x}_i) = \ln e_i + \boldsymbol{x}_i^\top \boldsymbol{\beta} \implies \text{offset}_i = \ln e_i.
\quad\quad\text{(4.7)}
$$

W przypadku Poissona, jest to równoważne zastąpieniu liczby roszczeń $Y_i$ obserwowaną stopą szkód $\tilde{Y}_i = \frac{Y_i}{e_i}$ przy użyciu ekspozycji $e_i$ jako wag $v_i$ i rezygnacji z przesunięcia, tak że

$$
\ln \tilde{\mu}_i(\boldsymbol{x}_i) = \boldsymbol{x}_i^\top \boldsymbol{\beta}
$$

co jest oczywiście równoważne (4.7). Jest to bezpośrednie zastosowanie wyników ustalonych w Przykładzie 2.5.2.

#### 4.3.7.3 Inne Zastosowania Przesunięć

Innym przydatnym zastosowaniem przesunięcia jest ograniczenie niektórych czynników ratingowych do wartości wstępnie określonych przy przechodzeniu z technicznego na komercyjny cennik. Takie ograniczenia są często motywowane konkurencją i względami marketingowymi, pozwalając na optymalne dostosowanie współczynników pozostałych czynników ratingowych do tych ograniczeń.

Przesunięcie może nawet pomóc w przejrzeniu lub udoskonaleniu istniejącego cennika. Ten ostatni jest wtedy umieszczany w przesunięciu, a oszacowane współczynniki regresji wskazują wtedy korekty potrzebne do poprawy obecnego cennika.

W ubezpieczeniach na życie, ekspozycje portfela są czasami ograniczone do rynku (lub innego odniesienia) tablicy życia. Sugeruje to pracę w kategoriach względnych, w odniesieniu do znaczącej referencyjnej tablicy życia. Odbywa się to poprzez wprowadzenie do wyniku regresji Poissona logarytmu oczekiwanej liczby zgonów zgodnie z tą tablicą życia, aby wyjaśnić specyficzną dla portfela siłę śmiertelności. To samo podejście dotyczy stawek chorobowości w ubezpieczeniach zdrowotnych.

Przesunięcia są również szczególnie przydatne, gdy ocena jest estymowana przy użyciu algorytmu backfittingu ogólnego dla Uogólnionych Modeli Addytywnych (lub GAMów, patrz Rozdz. 6) lub przy użyciu podejścia forward stagewise (lub boosting, patrz Sekcja 6.8). Poprzez forward stagewise budujemy ocenę, uwzględniając wariacje, które nie są jeszcze wyjaśnione przez obecny model. Obecny model służy zatem jako przesunięcie przy udoskonalaniu oceny.

#### 4.3.7.4 Przesunięcie w przypadku Tweediego z Łączem Logarytmicznym

Skomentujmy teraz subtelną różnicę między wagami a przesunięciami. Dopóki używane jest łącze logarytmiczne, to jest, średnia odpowiedź jest wykładniczą funkcją oceny, wagi i przesunięcia mogą być używane zamiennie w przypadku Tweediego, pod warunkiem, że odpowiedzi są odpowiednio zmodyfikowane. Jest to precyzyjnie pokazane dalej.

Rozważmy GLM Tweediego dla odpowiedzi $Y_i$, z funkcją łączącą logarytmiczną. W równaniu wiarygodności (4.1), mamy wtedy

$$
\frac{\partial\mu_i}{\partial s_i} = \mu_i \quad \text{ponieważ } \mu_i = \exp(s_i)
$$

i $V(\mu_i) = \phi \mu_i^\xi$ z $1 < \xi < 2$. Równania wiarygodności są zatem podane w tym przypadku przez

$$
\sum_{i=1}^n v_i \frac{y_i - \mu_i}{\mu_i^{\xi-1}} x_{ij} = 0, \quad j=0, 1, \dots, p.
\quad\quad\text{(4.8)}
$$

Załóżmy, że teraz wprowadzamy przesunięcie, tak że $\mu_i = z_i \tilde{\mu}_i$, przy czym przesunięcie to $\text{offset}_i = \ln z_i$. Równania wiarygodności (4.8) stają się

$$
\sum_{i=1}^n v_i \frac{y_i - z_i \tilde{\mu}_i}{(z_i \tilde{\mu}_i)^{\xi-1}} x_{ij} = 0, \quad j=0, 1, \dots, p,
\quad\quad\text{(4.9)}
$$

$$
\iff \sum_{i=1}^n v_i z_i^{2-\xi} \frac{y_i/z_i - \tilde{\mu}_i}{\tilde{\mu}_i^{\xi-1}} x_{ij} = 0, \quad j=0, 1, \dots, p.
\quad\quad\text{(4.10)}
$$

Z GLM Tweediego widzimy zatem, że włączenie przesunięcia $z_i$ jest równoważne użyciu GLM z przekształconymi odpowiedziami $y_i/z_i$ i wagami $v_i z_i^{2-\xi}$. Wyniki wyprowadzone tutaj za pomocą równania wiarygodności Tweediego (4.8) można postrzegać jako szczególne przypadki uzyskane z $\delta = \frac{\xi-2}{\xi-1}$ i łączem logarytmicznym (tak, że logarytm czynnika $z_i$ może wejść do oceny) przy użyciu Właściwości 2.5.1.

Rozważmy teraz dwa szczególne przypadki:
- **Przypadek Poissona ($\xi=1$)** Włączenie przesunięcia $\ln z_i$, tj. mnożenie oczekiwanej odpowiedzi przez $z_i$, jest równoważne uruchomieniu GLM z przekształconymi odpowiedziami $y_i/z_i$ i wagami $v_i z_i$.
W szczególnym przypadku GLM Poissona z kanonicznym łączem logarytmicznym jest to zatem równoważne modelowaniu liczby szkód z terminem przesunięcia równym logarytmowi ekspozycji (i wagami jednostkowymi) lub modelowaniu częstotliwości szkód bez terminu przesunięcia, ale z wagami równymi ekspozycji każdej obserwacji. Zgadza się to z ustaleniami z Sekcji 4.3.7.2.

- **Przypadek Gamma ($\xi=2$)** Włączenie przesunięcia $\ln z_i$, tj. mnożenie oczekiwanej odpowiedzi przez $z_i$, jest równoważne uruchomieniu GLM z przekształconymi odpowiedziami $y_i/z_i$ i wagami $v_i$. W przypadku Gamma z łączem logarytmicznym, wagi pozostają zatem nienaruszone przez tę transformację odpowiedzi.

To podejście może być również użyte do wykazania, że w niektórych bardzo szczególnych przypadkach estymacje uzyskane z różnych GLM-ów mogą w rzeczywistości się pokrywać. Na przykład, ze względu na ustawienie procesu Poissona, analiza danych zliczeniowych za pomocą regresji Poissona lub czasów między zdarzeniami za pomocą regresji Gamma jest równoważna. W ubezpieczeniach na życie, na przykład, oznacza to, że aktuariusz może dopasować model za pomocą regresji Poissona na liczbie zgonów lub za pomocą regresji Gamma na ekspozycjach na ryzyko, lub całkowitych czasach przeżycia.

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