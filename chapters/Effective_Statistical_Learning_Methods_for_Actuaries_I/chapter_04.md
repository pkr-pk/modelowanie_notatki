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

Ważne jest, aby zdać sobie sprawę, że tutaj nie przekształcamy odpowiedzi $Y_i$, ale raczej jej oczekiwaną wartość $\mu_i$. Z punktu widzenia statystycznego, model, w którym $g(Y_i)$ jest liniowy w $\boldsymbol{x}_i$, nie jest taki sam jak GLM, w którym $g(\mu_i)$ jest liniowy w $\boldsymbol{x}_i$. Aby to zrozumieć, załóżmy, że $\ln Y_i \sim \mathcal{Norororor}(\boldsymbol{x}_i^\top \boldsymbol{\beta}, \boldsymbol{\sigma}^2)$. Chociaż model jest dopasowany w skali logarytmicznej, aktuariusz jest zainteresowany odpowiedziami w skali oryginalnej (w jednostkach monetarnych, dla kwot roszczeń). Jeśli estymowane wyniki $\boldsymbol{x}_i^\top \hat{\boldsymbol{\beta}}$ są potęgowane, aby wrócić do oryginalnej skali, $\exp(\boldsymbol{x}_i^\top \hat{\boldsymbol{\beta}})$ daje wartości dla mediany odpowiedzi, a nie dla średniej. Dla średniej, dopasowane wartości wynoszą

$$
\hat{\mu}_i = \exp\left(\boldsymbol{x}_i^\top \hat{\boldsymbol{\beta}} + \frac{\hat{\boldsymbol{\sigma}}^2}{2}\right)
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
**Tabela 4.3** Typowe funkcje łączące i ich odwrotności. Tutaj $\mu_i$ to oczekiwana wartość odpowiedzi, $s_i$ to ocena, a $\Phi(\cdot)$ to dystrybuanta standardowego rozkładu normalnego $\mathcal{Norororor}(0, 1)$.

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

więc modele Poissona i regresji logistycznej dają podobne wyniki w tym przypadku. Model probit wykorzystuje dystrybuantę $\Phi$ rozkładu $\mathcal{Norororor}(0, 1)$ do odwzorowania przedziału jednostkowego $[0, 1]$. Komplementarna funkcja łącząca log-log wykorzystuje rozkład Ekstremalnej (minimum) Wartości w tym celu. Ten link dokładnie łączy GLM-y Poissona i dwumianowe, jak pokazano dalej. Rozważmy $Y \sim \mathcal{Poi}(\mu)$ i zdefiniujmy obciętą odpowiedź $\tilde{Y} = \min\{1, Y\}$.

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
Y_i = \text{offset}_i + \boldsymbol{x}_i^\top \boldsymbol{\beta} + Z_i \quad \text{gdzie } Z_i \sim \mathcal{Norororor}(0, \boldsymbol{\sigma}^2).
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

## 4.4 Rozwiązywanie Równań Wiarygodności w GLM

### 4.4.1 Powrót do Modelu Liniowej Regresji Normalnej

Nawet jeśli symetryczna zmienna losowa o rozkładzie normalnym ze stałą wariancją nie opisuje adekwatnie ani liczby szkód, ani kwot szkód, klasyczny model liniowej regresji normalnej pozostaje mimo wszystko bardzo użyteczny do dopasowywania GLM. Algorytm iteracyjny używany do estymacji współczynników regresji w GLM w rzeczywistości wykorzystuje model liniowej regresji normalnej na każdym kroku, stosując go do odpowiedzi roboczych (working responses) wyprowadzonych z rzeczywistych, co zostanie pokazane w następnej sekcji. Dlatego zaczniemy od przypomnienia estymacji współczynników regresji w modelu wielorakiej regresji liniowej

$$
Y_i \sim \mathcal{Nororor} \left( \text{score}_i, \frac{\boldsymbol{\sigma}^2}{v_i} \right), \quad i=1, 2, \dots, n, \text{ gdzie } \text{score}_i = \boldsymbol{x}_i^\top \boldsymbol{\beta}.
$$

To jest normalny GLM z funkcją łączącą tożsamościową. Zatem pracujemy z łączem kanonicznym, a równania wiarygodności zapisują się jako

$$
\boldsymbol{X}^\top \boldsymbol{W} (\boldsymbol{y} - \boldsymbol{X}\hat{\boldsymbol{\beta}}) = \boldsymbol{0},
$$

gdzie $\boldsymbol{W}$ jest macierzą diagonalną z wagami $v_1, \dots, v_n$ pojawiającymi się wzdłuż przekątnej. Jest to szczególny przypadek (4.5). Pod warunkiem, że $\boldsymbol{X}^\top \boldsymbol{W} \boldsymbol{X}$ jest odwracalna, daje to

$$
\hat{\boldsymbol{\beta}} = (\boldsymbol{X}^\top \boldsymbol{W} \boldsymbol{X})^{-1} \boldsymbol{X}^\top \boldsymbol{W} \boldsymbol{y}.
\quad\quad\text{(4.11)}
$$

Mamy więc w tym przypadku jawne wyrażenie na $\boldsymbol{\beta}$. Definiując macierz kapeluszową (hat matrix) $\boldsymbol{H}$ jako

$$
\boldsymbol{H} = \boldsymbol{W}^{1/2} \boldsymbol{X} (\boldsymbol{X}^\top \boldsymbol{W} \boldsymbol{X})^{-1} \boldsymbol{X}^\top \boldsymbol{W}^{1/2}
\quad\quad\text{(4.12)}
$$

widzimy, że estymator $\hat{\boldsymbol{Y}} = \boldsymbol{X}\hat{\boldsymbol{\beta}}$ z $\mathrm{E}[\boldsymbol{Y}]$ jest $\hat{\boldsymbol{Y}} = \mathrm{E}[\hat{\boldsymbol{Y}}] = \boldsymbol{H}\boldsymbol{Y}$. Wartości dopasowane $\hat{\boldsymbol{Y}}$ są zatem otrzymywane przez pomnożenie obserwacji $\boldsymbol{Y}$ przez macierz kapeluszową $\boldsymbol{H}$. Ta ostatnia odwzorowuje, czyli rzutuje $\boldsymbol{Y}$ na $\hat{\boldsymbol{Y}}$.

### 4.4.2 Algorytm IRLS

Równanie wiarygodności (4.1) nie dopuszcza jawnych rozwiązań (z wyjątkiem przypadku normalnego, jak pokazano powyżej) i dlatego musi być rozwiązywane iteracyjnie. Jedną z przyczyn, które przyczyniły się do sukcesu GLM, jest możliwość użycia jednego algorytmu do rozwiązywania równania wiarygodności (4.1) we wszystkich przypadkach. Zaczynając od odpowiedniej wartości początkowej dla $\boldsymbol{\beta}$, do uzyskania rozwiązań można użyć algorytmu Newtona-Raphsona. Algorytm Newtona-Raphsona wykorzystuje macierz Hessego, czyli drugie pochodne funkcji celu. W odniesieniu do maksymalizacji funkcji log-wiarygodności, hesjan odpowiada obserwowanej informacji Fishera, ze zmianą znaku (jak przypomniano w Rozdz. 3). W porównaniu z algorytmem Newtona-Raphsona, algorytm scoringu Fishera zastępuje obserwowaną macierz informacyjną jej oczekiwanym odpowiednikiem, czyli informacją Fishera. Gdy obie macierze informacyjne pokrywają się, scoring Fishera odpowiada metodzie Newtona-Raphsona. Oczekiwane i obserwowane macierze informacyjne są równe, gdy używane są kanoniczne funkcje łączące (pamiętaj, że funkcja log-wiarygodności jest zawsze wklęsła w tym przypadku, więc estymator największej wiarygodności jest zawsze unikalny, jeśli istnieje). Zazwyczaj potrzeba tylko kilku kroków, aby uzyskać $\boldsymbol{\beta}$, gdy używane są łącza kanoniczne. Każdą iterację algorytmu Newtona-Raphsona można zinterpretować jako dopasowanie normalnego modelu regresji liniowej na odpowiedziach roboczych, co daje początek podejściu iteracyjnie ważonych najmniejszych kwadratów (IRLS) wyjaśnionemu dalej. IRLS działa zatem poprzez sekwencję problemów najmniejszych kwadratów, dla których dostępne są dobrze zbadane techniki numeryczne.

W GLM średnia odpowiedź jest powiązana z oceną liniową przez funkcję łączącą. Aby oszacować parametry, przełączamy się na alternatywne podejście polegające na transformacji odpowiedzi przez funkcję łączącą. Zamiast używać $g(y_i)$, definiujemy odpowiedzi robocze $z_i$ jako lokalną aproksymację liniową do $g(y_i)$. W szczególności, ograniczone rozwinięcie Taylora

$$
\begin{aligned}
g(y_i) &\approx g(\mu_i) + (y_i - \mu_i) g'(\mu_i) \\
&= s_i + (y_i - \mu_i) \frac{\partial s_i}{\partial \mu_i} \text{ ponieważ } g(\mu_i) = s_i \\
&= z_i.
\end{aligned}
$$

Rozwiązywanie równań wiarygodności z GLM jest zatem równoważne dopasowywaniu odpowiedzi roboczych $z_i$ metodą ważonych najmniejszych kwadratów w sposób iteracyjny. Jak w każdym dopasowaniu ważonych najmniejszych kwadratów, wagi do użycia są odwrotnie proporcjonalne do wariancji odpowiedzi roboczej, danej przez

$$
\text{Var}[z_i] = \text{Var}[Y_i] \left( \frac{\partial s_i}{\partial \mu_i} \right)^2 = \frac{\phi}{v_i} V(\mu_i) \left( \frac{\partial s_i}{\partial \mu_i} \right)^2.
\quad\quad\text{(4.13)}
$$

Odwrócenie (4.13) prowadzi do wag pomocniczych $\tilde{v}_i$ zdefiniowanych w (4.2). Ponieważ wagi $\tilde{v}_i$ zależą od $\boldsymbol{\beta}$, potrzebna jest procedura iteracyjna. Dokładniej, proces jest inicjowany z pewnymi odpowiednimi wartościami początkowymi, takimi jak

$$
\begin{cases}
\hat{\beta}_0^{(0)} = g^{-1}(\bar{Y}) \\
\hat{\beta}_j^{(0)} = 0 \text{ dla } j = 1, \dots, p.
\end{cases}
$$

Oznacza to, że proces dopasowywania rozpoczyna się od przypadku jednorodnego, w którym wszystkie wyniki są równe wyrazowi wolnemu $\beta_0$. Alternatywnie, $\boldsymbol{\beta}^{(0)}$ można przyjąć jako parametry regresji w modelu liniowym $(g(y_i), \boldsymbol{x}_i), i=1, \dots, n$, modyfikując definicję dla $g(y_i)$ w przypadku, gdy $g$ nie jest zdefiniowane (na przykład, $y_i=0$ w przypadku Poissona z łączem logarytmicznym).

Zdefiniuj odpowiedź roboczą w kroku $r$ jako

$$
z_i^{(r)} = \hat{s}_i^{(r)} + (y_i - \hat{\mu}_i^{(r)}) \left. \frac{\partial s_i}{\partial \mu_i} \right|_{\boldsymbol{\beta}=\hat{\boldsymbol{\beta}}^{(r)}}
$$

gdzie

$$
\hat{s}_i^{(r)} = \sum_{j=0}^p \hat{\beta}_j^{(r)} x_{ij} \text{ i } \hat{\mu}_i^{(r)} = g^{-1}(\hat{s}_i^{(r)}).
$$

Przypomnijmy, że

$$
\frac{\partial s_i}{\partial \mu_i} = g'(\mu_i).
$$

Odpowiadające wagi robocze wchodzące do algorytmu IRLS są dane przez

$$
\tilde{v}_i^{(r)} = \left. \frac{1}{V(\hat{\mu}_i^{(r)})} \left( \frac{\partial \mu_i}{\partial s_i} \right|_{\boldsymbol{\beta}=\hat{\boldsymbol{\beta}}^{(r)}} \right)^2 \text{ gdzie } \frac{\partial \mu_i}{\partial s_i} = \frac{1}{g'(\mu_i)}.
$$

Estymata największej wiarygodności $\hat{\boldsymbol{\beta}}$ dla GLM jest następnie otrzymywana za pomocą sekwencji dopasowań ważonych najmniejszych kwadratów odpowiedzi roboczej $z_i^{(r)}$ do cech $\boldsymbol{x}_i$, z wagami $\tilde{v}_i^{(r)}$, to jest,

$$
\hat{\boldsymbol{\beta}}^{(r+1)} = \left( \boldsymbol{X}^\top \tilde{\boldsymbol{W}}^{(r)} \boldsymbol{X} \right)^{-1} \boldsymbol{X}^\top \tilde{\boldsymbol{W}}^{(r)} \boldsymbol{z}^{(r)}
\quad\quad\text{(4.14)}
$$

co pokrywa się ze wzorem (4.11) dającym szacowane parametry regresji metodą ważonych najmniejszych kwadratów, z macierzą diagonalną $\tilde{\boldsymbol{W}}^{(r)}$ wypełnioną wagami $\tilde{v}_i^{(r)}$ i odpowiedziami roboczymi $z_i^{(r)}$ w iteracji $r+1$. Dopasowanie GLM jest zatem równoważne dopasowaniu kilku normalnych liniowych modeli regresji w rzędzie. IRLS implementuje tak zwaną metodę scoringu Fishera do dopasowywania GLM. Pomimo swojej pozornej prostoty, wzór (4.14) może czasami stać się niestabilny numerycznie, zwłaszcza w wysokich wymiarach, to jest, gdy liczba $p$ cech staje się duża. Jest to spowodowane odwracaniem macierzy. Metody spadku gradientu oferowane w Hainaut et al. (2019) oferują w tym przypadku solidną alternatywę.

Zauważ, że wagi robocze $\tilde{v}_i^{(r)}$ są odwrotnie proporcjonalne do wariancji odpowiedzi roboczej $z_i^{(r)}$ podanej w (4.13). Parametr dyspersji $\phi$ upraszcza się, tak że znika z iteracyjnego wzoru (4.14) na $\hat{\boldsymbol{\beta}}^{(r+1)}$. To wyjaśnia, dlaczego nie pojawia się on w definicji wag roboczych $\tilde{v}_i^{(r)}$ podanej powyżej, w porównaniu do (4.2).

Reguła zatrzymania dla algorytmu IRLS generalnie polega na utrzymywaniu bieżącej iteracji, gdy

$$
\|\hat{\boldsymbol{\beta}}^{(r)} - \hat{\boldsymbol{\beta}}^{(r+1)}\| \quad \text{lub} \quad \frac{L(\hat{\boldsymbol{\beta}}^{(r+1)}) - L(\hat{\boldsymbol{\beta}}^{(r)})}{L(\hat{\boldsymbol{\beta}}^{(r)})} \text{ staje się wystarczająco małe.}
$$

Niech $r_{\text{stop}}$ będzie liczbą kroków przed zatrzymaniem algorytmu IRLS. Ostateczna iteracja $\hat{\boldsymbol{\beta}}^{(r_{\text{stop}})}$ jest przyjmowana jako estymata największej wiarygodności $\hat{\boldsymbol{\beta}}$. Wagi robocze $\tilde{v}_i^{(r_{\text{stop}})}$ w ostatniej iteracji są przechowywane w macierzy diagonalnej

$$
\tilde{\boldsymbol{W}}^{(r_{\text{stop}})} = \begin{pmatrix}
\tilde{v}_1^{(r_{\text{stop}})} & 0 & \dots & 0 \\
0 & \tilde{v}_2^{(r_{\text{stop}})} & \dots & 0 \\
\vdots & \vdots & \ddots & \vdots \\
0 & 0 & \dots & \tilde{v}_n^{(r_{\text{stop}})}
\end{pmatrix}.
$$

Zauważ, że procedura dopasowywania dla GLM wykorzystuje tylko funkcję łączącą $s=g(\mu)$ i zależność średnia-wariancja $V(\cdot)$, ale nie wymaga dalszej wiedzy o rozkładzie odpowiedzi.

W większości przypadków zbieżność algorytmu IRLS jest bardzo szybka. Jednak czasami pojawiają się trudności. Gdy zbieżność nie występuje, może to być spowodowane problemem z rozważanymi danymi. Na przykład, gdy dwie grupy są liniowo rozdzielalne w przestrzeni cech z odpowiedziami binarnymi, dane mogą być dopasowane idealnie, a niestabilne estymaty parametrów z wysokimi błędami standardowymi są typowo uzyskiwane. Powoduje to problemy ze zbieżnością w algorytmie IRLS.

Jako przykład, rozważmy ponownie dane ubezpieczenia komunikacyjnego wyświetlone w Tabeli 4.1. Odpowiedzi $Y_1, \dots, Y_n$ reprezentują liczbę szkód zgłoszonych przez $n$ ubezpieczających, zakłada się, że są niezależne i podlegają rozkładowi Poissona. Istnieją dwie zmienne objaśniające:
- Płeć z dwoma poziomami, mężczyzna i kobieta, oraz
- Zakres ubezpieczenia z trzema poziomami, tylko OC, szkody ograniczone i kompleksowe.

Wiemy, że analiza GLM może być przeprowadzona na danych zagregowanych w formie tabelarycznej z indywidualnych rekordów, bez utraty informacji. Dlatego indeks $i$ odnosi się teraz do całej klasy ryzyka.

Klasa referencyjna dla portfela jest taka, że $x_{ij}=0$ dla wszystkich $j$. Tutaj odpowiada to ubezpieczającym posiadającym ubezpieczenie od szkód ograniczonych, oprócz obowiązkowego OC. Odpowiadająca roczna oczekiwana liczba szkód wynosi $\exp(\beta_0)$. Wszystkie wyniki są interpretowane w odniesieniu do klasy referencyjnej (dla której $x_{i1}=x_{i2}=x_{i3}=0$). Portfel jest podzielony na 6 klas ryzyka zgodnie z płcią i zakresem ubezpieczenia posiadaczy polis. Na przykład, sekwencja $(0, 1, 0)$ reprezentuje mężczyznę posiadającego polisę z OC.

Wyniki dla każdej komórki są podane przez kombinację liniową $\boldsymbol{\beta}^\top \boldsymbol{x}_i = \beta_0 + \beta_1 x_{i1} + \beta_2 x_{i2} + \beta_3 x_{i3}$. Roczna oczekiwana częstotliwość szkód dla każdej komórki jest dana przez

$$\begin{align*}
\exp(\boldsymbol{\beta}^\top \boldsymbol{x}_i) &= \beta_0 + \beta_1 x_{i1} + \beta_2 x_{i2} + \beta_3 x_{i3}\\
&=\begin{cases}
\exp(\beta_0) & \text{dla mężczyzny z ubezpieczeniem od szkód ograniczonych} \\
\exp(\beta_0 + \beta_2) & \text{dla mężczyzny z OC} \\
\exp(\beta_0 + \beta_3) & \text{dla mężczyzny z ubezpieczeniem kompleksowym} \\
\exp(\beta_0 + \beta_1) & \text{dla kobiety z ubezpieczeniem od szkód ograniczonych} \\
\exp(\beta_0 + \beta_1 + \beta_2) & \text{dla kobiety z OC} \\
\exp(\beta_0 + \beta_1 + \beta_3) & \text{dla kobiety z ubezpieczeniem kompleksowym}
\end{cases}
\end{align*}$$

Zastosujmy algorytm IRLS do oszacowania współczynników regresji $\beta_j$. Pracujemy ze wskaźnikiem szkód $y_i / e_i$ jako odpowiedzią, tak że używamy wag $v_i = e_i$ odpowiadających całkowitej ekspozycji na ryzyko dla każdej klasy ryzyka. W przypadku Poissona z łączem logarytmicznym, mamy

$$
s_i = \ln \mu_i \implies \frac{\partial s_i}{\partial \mu_i} = \frac{1}{\mu_i}
$$

więc odpowiedzi robocze są dane przez

$$
z_i = s_i + \frac{y_i - \mu_i}{\mu_i}.
$$

Wagi robocze są podane przez $\tilde{v}_i = v_i \mu_i$.

Zaczynamy algorytm IRLS od przypadku jednorodnego, dla którego wszystkie dopasowane wartości są równe $\exp(\beta_0)$. Tutaj $\beta_0$ powiela obserwowaną średnią częstotliwość szkód w portfelu, to jest,

$$
\exp(\hat{\beta}_0^{(0)}) = \frac{1,683 + 3,403 + 626 + 873 + 2,423 + 766}{10,000 + 30,000 + 5,000 + 6,000 + 24,000 + 7,000}
$$

co daje $\hat{\beta}_0^{(0)} = -2.126993$. Zaczynając od

$$
\hat{\beta}^{(0)} = \begin{pmatrix} -2.126993 \\ 0.000000 \\ 0.000000 \\ 0.000000 \end{pmatrix},
$$

po trzech krokach otrzymujemy

$$
\hat{\beta}^{(1)} = \begin{pmatrix} -2.16632152 \\ -0.12493520 \\ 0.42641819 \\ 0.08540113 \end{pmatrix}, \quad
\hat{\beta}^{(2)} = \begin{pmatrix} -2.17244198 \\ -0.12633430 \\ 0.38501342 \\ 0.09002269 \end{pmatrix}, \quad \text{i} \quad
\hat{\beta}^{(3)} = \begin{pmatrix} -2.17245491 \\ -0.12635812 \\ 0.38384364 \\ 0.09004595 \end{pmatrix}
$$

co jest dokładnie wynikiem funkcji `glm` z R implementującej algorytm IRLS.

### 4.4.3 Wybór Poziomu Bazowego dla Cech Kategorycznych

Oprogramowanie GLM automatycznie zastępuje każdą cechę kategoryczną zbiorem wskaźników, po jednym dla każdego poziomu innego niż poziom bazowy. Ten ostatni musi być wybrany z rozwagą, ponieważ cała analiza jest wykonywana w kategoriach względnych, w odniesieniu do wybranego poziomu bazowego.

Rozważmy na przykład Przykład 1.4.3, gdzie obszar zamieszkania posiadacza polisy jest uważany za potencjalną zmienną objaśniającą dla częstotliwości szkód $Y$. Istnieją trzy obszary, A, B i C, zakodowane za pomocą dwóch wskaźników (zmiennych zero-jedynkowych) $x_{i1}$ (równe 1, jeśli posiadacz polisy mieszka w obszarze A, a 0 w przeciwnym razie) i $x_{i2}$ (równe 1, jeśli posiadacz polisy mieszka w obszarze B, a 0 w przeciwnym razie). Nie jest potrzebny żaden wskaźnik dla poziomu referencyjnego C. Wtedy, wyraz wolny $\beta_0$ odpowiada za efekt poziomu referencyjnego, tutaj obszaru C. Następnie, $\beta_1$ kwantyfikuje różnicę między obszarami A i C, podczas gdy $\beta_2$ kwantyfikuje różnicę między obszarami B i C. Zauważ, że aktuariusz nie jest ograniczony do porównywania tylko z poziomem bazowym: różnice między obszarami A i B są kwantyfikowane przez $\beta_1 - \beta_2$.

Wybór poziomu bazowego należy do aktuariusza, ale nie powinien on być rzadki. Załóżmy na przykład, że nie ma posiadaczy polis w obszarze C. Wtedy dla każdego posiadacza polisy albo $x_{i1}$ lub $x_{i2}$ jest równe 1, a drugi jest równy 0. To z kolei oznacza, że $x_{i1} + x_{i2} = 1$ dla wszystkich $i$ i stąd macierz projektowa $\boldsymbol{X}$ jest osobliwa: suma dwóch ostatnich kolumn jest równa pierwszej (kolumna jedynek odpowiadająca wyrazowi wolnemu). To sprawia, że oszacowanie $\boldsymbol{\beta}$ jest niemożliwe, ponieważ algorytm IRLS musi odwrócić macierz $\boldsymbol{X}^\top \boldsymbol{X}$. Intuicyjnie, jedna z dwóch cech binarnych w takiej sytuacji jest redundantna, a model nie może rozwikłać ich odpowiednich współczynników regresji $\beta_1$ i $\beta_2$: każda modyfikacja w $\beta_1$ może być skompensowana przez odpowiednią zmianę w $\beta_2$ i odwrotnie.

Bardziej realistycznie, jeśli poziom bazowy ma bardzo mało przypadków, wtedy kolumny $\boldsymbol{X}$ są prawie zależne, co sprawia, że obliczenie $\boldsymbol{\beta}$ jest numerycznie niestabilne. Nie zawsze jest to brane pod uwagę w zautomatyzowanym kodowaniu implementowanym w oprogramowaniu, więc domyślny wybór musi być czasami modyfikowany przez aktuariusza.

Zauważ, że nie jest sensowne definiowanie cechy

$$
\text{area}_i = \begin{cases}
2 & \text{jeśli posiadacz polisy } i \text{ mieszka w obszarze A} \\
1 & \text{jeśli posiadacz polisy } i \text{ mieszka w obszarze B} \\
0 & \text{jeśli posiadacz polisy } i \text{ mieszka w obszarze C.}
\end{cases}
$$

Odpowiadający wynik GLM jest wtedy równy

$$
\beta_0 + \beta_{\text{area}}\text{area}_i = \begin{cases}
\beta_0 + 2\beta_{\text{area}} & \text{jeśli posiadacz polisy } i \text{ mieszka w obszarze A,} \\
\beta_0 + \beta_{\text{area}} & \text{jeśli posiadacz polisy } i \text{ mieszka w obszarze B,} \\
\beta_0 & \text{jeśli posiadacz polisy } i \text{ mieszka w obszarze C,}
\end{cases}
$$

więc ta specyfikacja implikuje równe „odstępy” między A, B i C: wynik $\beta_0 + \beta_{\text{area}}\text{area}_i$ daje różnicę $\beta_{\text{area}}$ między obszarami A i B, i taką samą różnicę $\beta_{\text{area}}$ między obszarami B i C. Niekoniecznie jest to zgodne z danymi i może obciążać analizę.

Nawet jeśli analiza GLM jest przeprowadzana warunkowo na wartościach $x_{ij}$ cech $X_{ij}$, struktura korelacji wektorów losowych $(X_{i1}, \dots, X_{ip})$ może wpłynąć na oszacowanie współczynników regresji. Korelacja między losowymi zmiennymi $X_{ij}$ oznacza, że niektóre cechy mogą służyć jako substytuty dla innych w obliczaniu wyniku.

### 4.4.4 Skorelowane Cechy, Współliniowość

Większość czasu, cechy $X_{ij}$ są skorelowane w badaniach obserwacyjnych. Gdy te korelacje są umiarkowane, GLM są w stanie wyizolować wpływ każdego czynnika ryzyka na średnią odpowiedź. Jest to jedna z wielkich zalet GLM w porównaniu z analizami jednowymiarowymi lub wielowymiarowymi, które są często zniekształcone, ponieważ ten sam efekt jest liczony wielokrotnie. Jednakże, gdy korelacja między niektórymi cechami staje się duża, GLM mogą napotkać problemy. Takie wysokie korelacje oznaczają, że ta sama informacja jest zakodowana w kilku cechach, co prowadzi do nadmiarowości informacji, jak wyjaśniono dalej.

Aliasing występuje, gdy istnieje liniowa zależność między cechami. Na przykład, jedna cecha może być identyczna z jakąś liniową kombinacją niektórych innych. Rozważmy następujące zmienne objaśniające:

$x_{i1}$ = wiek, w którym rozpoczęto prowadzenie pojazdu

$x_{i2}$ = staż prawa jazdy

$x_{i3}$ = obecny wiek posiadacza polisy $i$.

Oczywiście, tożsamość

$$
x_{i3} = x_{i1} + x_{i2}
$$

zachodzi dla wszystkich $i$, więc $x_{i3}$ jest redundantny w wyjaśnianiu odpowiedzi. Wynik można zapisać jako funkcję tylko dwóch cech. Na przykład, możemy napisać

$$
\beta_0 + \beta_1 x_{i1} + \beta_2 x_{i2} + \beta_3 x_{i3} = \beta_0 + (\beta_1 + \beta_3) x_{i1} + (\beta_2 + \beta_3) x_{i2}.
$$

Stąd, trzy zmienne $x_{i1}, x_{i2}$ i $x_{i3}$ wyjaśniają to samo co dowolne dwie z tych zmiennych. Zatem, indywidualne efekty każdej z trzech zmiennych nie mogą być ocenione.

Takie zmienne są doskonale współliniowe lub aliasowane. Aliasing odpowiada zatem liniowej zależności między kolumnami macierzy projektowej $\boldsymbol{X}$. Gdy dwie cechy są doskonale skorelowane, mówi się, że są aliasowane, a estymaty parametrów nie są unikalne. W tych przypadkach, macierz projektowa $\boldsymbol{X}$ ma dokładną zależność liniową między swoimi kolumnami i stąd $\boldsymbol{X}^\top \boldsymbol{X}$ jest osobliwa, co unieważnia podejście IRLS.

Oprócz dokładnej współliniowości zilustrowanej powyżej, częściej spotyka się sytuacje bliskiej współliniowości (lub prawie aliasowania). Na przykład, w zbiorze danych, w którym obszar zamieszkania, poziom dochodów i poziom wykształcenia posiadacza polisy są silnie skorelowane, włączenie trzech zmiennych objaśniających sprawia, że $\boldsymbol{X}^\top \boldsymbol{X}$ jest bliska osobliwości i powoduje trudności w obliczaniu odwrotności $(\boldsymbol{X}^\top \boldsymbol{X})^{-1}$. Regularyzacja Tichonowa i regresja grzbietowa (dokładnie omówione w Hainaut et al. 2019) mogą być w tym względzie pomocne.

Jeśli odwrotność jest obliczalna, zazwyczaj będzie miała duże wpisy diagonalne, co implikuje duże błędy standardowe dla jednego lub więcej współczynników regresji (z powodów wyjaśnionych w następnej sekcji). Współliniowość może również wprowadzać w błąd analityka w wyborze predykcyjnych zmiennych: jeśli 2 zmienne objaśniające są silnie skorelowane i obie są predykcyjne dla odpowiedzi, to każda z nich, przy obecności drugiej, nie wniesie wiele dodatkowych informacji do odpowiedzi. Testy hipotez dla każdej zmiennej, przy założeniu, że druga jest w modelu, pokażą brak istotności. Rozwiązaniem jest włączenie tylko jednej z tych dwóch zmiennych objaśniających do modelu. To wyjaśnia, dlaczego wstępne badanie struktury korelacji cech jest przydatne przed rozpoczęciem analizy GLM.

**Uwaga 4.4.1** (Współliniowość a Interakcja). Współliniowość pochodzi z korelacji między czynnikami ryzyka, z których kilka koduje te same informacje. Interakcja nie ma nic wspólnego ze współliniowością/korelacją. Na przykład, rozważmy portfel ubezpieczeń komunikacyjnych z taką samą strukturą wiekową dla mężczyzn i kobiet (tak, że czynniki ryzyka wiek i płeć są wzajemnie niezależne). Jeśli młodzi mężczyźni są bardziej niebezpieczni w porównaniu z młodymi kobietami, podczas gdy ta różnica zanika lub odwraca się w starszym wieku, to wiek i płeć oddziałują na siebie pomimo bycia niezależnymi.

### 4.4.5 V Craméra

Siłę zależności między dwiema zmiennymi ciągłymi można ocenić za pomocą współczynnika korelacji liniowej Pearsona (tj. kowariancji podzielonej przez iloczyn odchyleń standardowych) lub, preferencyjnie, za pomocą współczynników korelacji rang, takich jak rho Spearmana lub tau Kendalla. Jednak te klasyczne miary zależności nie są odpowiednie do oceny korelacji między cechami nieciągłymi. Biorąc pod uwagę różne formaty cech wchodzących do GLM (binarne, kategoryczne, dyskretne lub ciągłe), istnieje potrzeba miary korelacji mającej zastosowanie do wszystkich tych przypadków. Dlatego V Craméra jest często używane do mierzenia siły powiązania między cechami w badaniach ubezpieczeniowych. To podejście opiera się na danych wyświetlanych w formie tabelarycznej, tak że cechy dyskretne lub kategoryczne mogą być traktowane bezpośrednio. Cechy ciągłe muszą być skutecznie traktowane (lub grupowane) przez podział ich dziedziny na rozłączne przedziały i stąd traktowane jako dyskretne.

Rozważmy na przykład wiek i płeć posiadacza polisy. Wiek jest najpierw kategoryzowany w kilku klasach: na przykład, kategoria „młody kierowca” grupująca posiadaczy polis w wieku poniżej 25 lat, kategoria „w średnim wieku” grupująca posiadaczy polis w wieku od 26 do 50 lat i kategoria „senior” grupująca posiadaczy polis w wieku 51 lat i starszych. Portfel można następnie podzielić na 6 klas, krzyżując 3 wspomniane kategorie wiekowe z 2 płciami. W każdej komórce aktuariusz rejestruje obserwowane odpowiedzi i związaną z nimi ekspozycję. Jednak takie wstępne grupowanie zmiennej ciągłej (jak wiek w naszym przykładzie) jest subiektywne (wybór punktów odcięcia jest generalnie nieco arbitralny przez analityka) i może prowadzić do możliwej utraty informacji. Nie są dostępne ogólne optymalne punkty odcięcia.

V Craméra opiera się na tablicy kontyngencji klasyfikującej krzyżowo dane według pary cech w badaniu. Przypomnijmy, że statystyka Chi-Kwadrat dla testowania niezależności między dwiema cechami w tablicy kontyngencji, oznaczona jako $\chi^2$, jest otrzymywana przez zsumowanie kwadratów różnic między obserwowaną częstotliwością a oczekiwaną częstotliwością w każdej komórce tabeli, podzielonych przez oczekiwaną częstotliwość. Oczekiwana częstotliwość w każdej komórce odpowiada niezależności między dwiema cechami. Jest ona otrzymywana przez pomnożenie sumy wiersza przez sumę kolumny, podzieloną przez sumę całkowitą. Zauważ, że ekspozycje muszą być uwzględnione w obliczeniach.

Teraz, biorąc pod uwagę dwie cechy $X_{i1j_1}$ i $X_{i1j_2}$ przyjmujące odpowiednio $k_1$ i $k_2$ wartości dyskretnych, V Craméra jest zdefiniowane jako

$$
V = \sqrt{\frac{\chi^2/n}{\min\{k_1-1, k_2-1\}}} \in [0, 1]
$$

gdzie $\chi^2$ jest statystyką testu Chi-Kwadrat Pearsona dla niezależności cech $X_{i1j_1}$ i $X_{i1j_2}$. Dzielenie $\chi^2$ przez liczbę obserwacji sprawia, że statystyka jest niezależna od liczby obserwacji, tj. mnożenie każdej komórki tablicy kontyngencji przez dodatnią liczbę całkowitą nie zmienia wartości V Craméra. Co więcej, V Craméra przyjmuje wartości w przedziale jednostkowym [0, 1], przy czym 0 i 1 odpowiadają niezależności i doskonałej zależności. Będąc opartym na danych wyświetlanych w formie tabelarycznej, V Craméra jest szeroko stosowane.

Co więcej, maksimum $\chi^2/n$ jest osiągane, gdy istnieje całkowita zależność, tj. każdy wiersz (lub kolumna, w zależności od tego, na której stronie tabeli) wykazuje tylko jeden ściśle dodatni wiersz. Oznacza to, że wartość $X_{i1j_1}$ determinuje wartości $X_{i1j_2}$. Ta maksymalna wartość $\chi^2/n$ jest równa najmniejszemu wymiarowi minus 1. Zatem, dzielenie $\chi^2/n$ przez $\min\{k_1-1, k_2-1\}$ zapewnia, że V przyjmuje wartości w przedziale jednostkowym [0, 1]. Wyciągnięcie pierwiastka kwadratowego gwarantuje, że V Craméra i współczynnik korelacji liniowej Pearsona pokrywają się w tabelach 2x2 (tj. dla dwóch cech binarnych, z $k_1=k_2=2$).

**Przykład 4.4.2** Rozważmy ponownie dane wyświetlone w Tabeli 4.5. Ten hipotetyczny zbiór danych jest sklasyfikowany krzyżowo według dwóch zmiennych kategorycznych (z dwoma kategoriami zdefiniowanymi w odniesieniu do progu 20 000 km). V Craméra jest równe 0.533, co wskazuje na dość silną dodatnią korelację między płcią a przejechanym dystansem, mężczyźni posiadający polisy mają tendencję do przejeżdżania dłuższych dystansów. Dla danych w Tabeli 4.1, stwierdzamy, że V Craméra jest równe 0.123, więc powiązanie jest słabsze.

### 4.4.6 Błąd Pominiętej Zmiennej

Z Rozdziału 2 wiemy, że aktuariusz nie może pominąć ważnych informacji o ryzyku posiadaczy polis. Jeśli ukryte czynniki ryzyka są skorelowane zarówno z odpowiedzią, jak i z jedną lub więcej dostępnymi cechami, będą one obciążać estymatę odpowiedniego współczynnika regresji. Zjawisko to jest powszechnie znane jako błąd pominiętej zmiennej i można je wyjaśnić w następujący sposób.

Rozważmy liczbę roszczeń $Y_i$ złożonych przez posiadacza polisy $i$ z ekspozycją $e_i$ i cechami $\boldsymbol{x}_i$. Załóżmy, że

$$
\mathrm{E}[Y_i] = e_i \exp\left( \beta_0 + \sum_{j=1}^p \beta_j x_{ij} + \beta^+ \boldsymbol{x}_i^+ \right).
$$

Jeśli cecha $\boldsymbol{x}_i^+$ jest pominięta i $\boldsymbol{x}_i^+$ jest skorelowana z pozostałymi cechami $x_{i1}, \dots, x_{ip}$ w tym sensie, że przybliżenie

$$
\boldsymbol{x}_i^+ \approx \beta_0^+ + \sum_{j=1}^p \beta_j^+ x_{ij}
$$

jest stosunkowo dokładne, to

$$
\mathrm{E}[Y_i] \approx e_i \exp\left( \beta_0 + \beta^+\beta_0^+ + \sum_{j=1}^p (\beta_j + \beta^+\beta_j^+) x_{ij} \right).
$$

W takim przypadku, estymata największej wiarygodności $\tilde{\beta}_j$ wyprodukowana przez algorytm IRLS nie może być wyizolowana, tylko $\beta_j + \beta_j^+$, a nie $\beta_j$. Prawdziwy efekt $\beta_j$ z $x_{ij}$ na skali oceny nie może być oszacowany, tylko efekt obciążony

$$
\beta_j + \beta^+\beta_j^+
$$

może być oszacowany, mieszając

- prawdziwy efekt $\beta_j$ z $x_{ij}$
- z efektem $\beta^+\beta_j^+$ pominiętej cechy $\boldsymbol{x}_i^+$.

W takim przypadku, możemy mieć nawet $\tilde{\beta}_j < 0$, podczas gdy $\beta_j > 0$.
Zatem, każda pominięta cecha skorelowana z $x_{ij}$ obciąża oszacowanie $\beta_j$ uzyskane z algorytmu IRLS. Dlatego standardowe modele branżowe nie mogą być używane do ustanawiania związku przyczynowego, a jedynie do wykrywania korelacji.

Zilustrujmy teraz efekt pominiętej zmiennej na prostym przykładzie. Rozważmy ponownie zbiór danych wyświetlony w Tabeli 4.5 sklasyfikowany krzyżowo według płci i rocznego przebiegu (z punktem odcięcia 20 000 km). Ponieważ obie cechy są kategoryczne z dwoma poziomami, można je zakodować za pomocą dwóch cech binarnych. Dokładniej, $x_{i1}$ rejestruje roczny przebieg, z

$$
x_{i1} = \begin{cases}
0 & \text{jeśli posiadacz polisy } i \text{ przejeżdża więcej niż 20,000 km rocznie} \\
1 & \text{w przeciwnym razie}.
\end{cases}
$$

Podobnie, $x_{i2}$ rejestruje płeć, z

$$
x_{i2} = \begin{cases}
0 & \text{jeśli posiadacz polisy } i \text{ jest mężczyzną} \\
1 & \text{w przeciwnym razie}.
\end{cases}
$$

Tutaj wybraliśmy poziomy bazowe odpowiadające najbardziej zaludnionym kategoriom: mężczyzna dla płci i ponad 20 000 km rocznie dla przejechanego dystansu.

Włączając obie binarne cechy kodujące informacje w odniesieniu do tej polisy ($x_{i1}$ dla płci i $x_{i2}$ dla rocznego przebiegu), IRLS daje następujące wyniki po trzech iteracjach:

$$
\begin{aligned}
\hat{\beta}_0 &= -2.20597 \\
\hat{\beta}_1 &= -0.54753 \\
\hat{\beta}_2 &= -0.26383.
\end{aligned}
$$

Teraz, jeśli płeć zostanie usunięta z oceny, zachowując tylko przejechany dystans, otrzymujemy po trzech iteracjach:

$$
\begin{aligned}
\hat{\beta}_0 &= -2.24904 \\
\hat{\beta}_1 &= -0.69552.
\end{aligned}
$$

Zatem widzimy, że oszacowane współczynniki regresji są modyfikowane w porównaniu z oszacowaniami uzyskanymi z płcią w modelu. W szczególności, oszacowany współczynnik regresji $\hat{\beta}_1$ związany z przejechanym dystansem staje się bardziej ujemny, zmieniając się z -0.55 na -0.70. Można to wyjaśnić korelacją istniejącą między dwiema cechami. Wiemy, że korelacja jest dodatnia, na co wskazuje stosunkowo wysoka wartość V Craméra uzyskana w Przykładzie 4.4.2. Słownie, mężczyźni posiadający polisy mają tendencję do przejeżdżania więcej niż 20 000 km rocznie, podczas gdy kobiety posiadające polisy mają tendencję do przejeżdżania mniej. Stąd, jeśli wiemy, że posiadacz polisy przejeżdża mniej niż 20 000 km rocznie, jest prawdopodobne, że jest to kobieta. Formalnie,

$$
\text{P}[\text{kobieta} | < 20,000 \text{ km}] = \frac{6,000}{6,000+2,000} = \frac{3}{4}.
$$

Stąd, $\hat{\beta}_1$ staje się bardziej ujemny, gdy płeć jest pominięta w ocenie, ponieważ przejmuje negatywny wpływ bycia kobietą na ocenę.

Wstępne badanie powiązań istniejących między dostępnymi cechami wskazuje, że włączenie lub przesunięcie (offsetting) jednej z nich będzie miało największy wpływ na te cechy, które są silnie skorelowane z pominiętą.

### 4.4.7 Ilustracje Numeryczne

#### 4.4.7.1 Graduacja Rocznych Prawdopodobieństw Zgonu

Rozważmy zamkniętą grupę osób, obserwowaną przez jeden rok. W każdym wieku $x$ od urodzenia do wieku 100, odpowiedzią jest liczba zgonów $D_x$ odnotowana wśród $l_x$ osób w wieku $x$ 1 stycznia. Rysunek 4.1 przedstawia dostępne dane $D_x$ i $l_x$. Odpowiadające surowe roczne prawdopodobieństwa zgonu

$$
\hat{q}_x = \frac{D_x}{l_x}
$$

są estymatami największej wiarygodności parametru $q_x$ w modelu $D_x \sim \mathcal{B}in(l_x, q_x)$, jak pokazano w Sekcji 3.6.1. Są one wyświetlone na Rys. 4.2. Można je również uzyskać za pomocą dwumianowego GLM traktującego wiek $x$ jako cechę kategoryczną, to jest, używając cech binarnych

$$
x_{ij} = \begin{cases}
1 & \text{jeśli wiek} = j, \\
0 & \text{w przeciwnym razie},
\end{cases}
$$

dla $j=1, 2, \dots, 100$ (wybierając wiek 0 jako referencyjny). Mając do czynienia z odpowiedziami dwumianowymi, używamy parametru kanonicznego $\ln \frac{q_x}{1-q_x}$, to jest, logitu $q_x$. Odpowiadające estymaty największej wiarygodności są wtedy podane przez

$$
\hat{\beta}_j = \ln \frac{\hat{q}_j}{1-\hat{q}_j} - \ln \frac{\hat{q}_0}{1-\hat{q}_0}, \quad j=1, \dots, 100,
$$

i

$$
\hat{\beta}_0 = \ln \frac{\hat{q}_0}{1-\hat{q}_0}.
$$

Rysunek 4.2 pokazuje, że logit $\hat{q}_x$ wydaje się być rozsądnie liniowy w wieku $x$, z wyjątkiem młodego wieku (gdzie wysoka śmiertelność okołoporodowa, spłaszczenie około 30 roku życia i garb wypadkowy około 20 roku życia są bardziej skomplikowane do uchwycenia), z lekką krzywizną po wieku emerytalnym. Dało to początek tak zwanej (kwadratowej) formule Perksa, zgodnie z którą

$$
\ln \frac{q_x}{1-q_x} = \beta_0 + \beta_1 x + \beta_2 x^2 \iff q_x = \frac{\exp(\beta_0 + \beta_1 x + \beta_2 x^2)}{1 + \exp(\beta_0 + \beta_1 x + \beta_2 x^2)}.
\quad\quad\text{(4.15)}
$$

Wiek $x$ jest traktowany jako cecha ciągła i uzupełniony o swój kwadrat, używany jako dodatkowa cecha, a model jest ograniczony do wieku 65 lat i więcej.

Wzór (4.15) zapewnia oszczędną reprezentację jednorocznych prawdopodobieństw zgonu w starszym wieku. Może być dopasowany za pomocą dwumianowego GLM do danych ($D_x, l_x$) wyświetlonych na Rys. 4.1. Plik danych zawiera odpowiedź $D_x$ wraz z jedną cechą (Wiek) i rozmiarami dwumianowymi $l_x$. Używając funkcji R `glm`, otrzymujemy $\hat{\beta}_0 = -3.34$, $\hat{\beta}_1 = -0.1038$ i $\hat{\beta}_2 = 0.001384$. Algorytm IRLS zbiegł się w zaledwie trzech iteracjach. Wynikowe dopasowane wartości są wyświetlone na Rys. 4.3. Przedstawiamy tam również wynik dopasowania liniowego, które wyraźnie pomija krzywiznę obecną w danych.

W tym przykładzie, Rys. 4.3 wyraźnie pokazuje, że kwadratowa formuła Perksa przewyższa swój liniowy odpowiednik na rozważanym zbiorze danych. Należy jednak podkreślić, że ten wniosek jest wyciągany na podstawie inspekcji wizualnej, co może być niemożliwe w badaniach ubezpieczeniowych obejmujących wiele cech. Dlatego potrzebujemy również formalnych procedur testowych do porównywania dwóch specyfikacji modelu, takich jak te przedstawione w następnych sekcjach.

#### 4.4.7.2 Rezerwacja Strat

W ubezpieczeniach majątkowych i wypadkowych (P&C), roszczenia są czasami rozliczane przez kilka lat. Może to być spowodowane długimi procedurami prawnymi związanymi z roszczeniami z tytułu odpowiedzialności cywilnej (spory sądowe w celu ustalenia odpowiedzialności w spornych sprawach mogą być długim procesem) lub po prostu dlatego, że roszczenie może być zgłoszone dopiero później. W przypadku odpowiedzialności za produkt, na przykład, roszczenie może powstać na długo po odkryciu i zgłoszeniu szkody, a nawet dłużej przed oceną zakresu szkody.

Typowa historia roszczenia jest przedstawiona na Rys. 4.4. Rozważane roszczenie pochodzi ze zdarzenia (wypadku, na przykład), które miało miejsce w czasie $t_1$. Zostało zgłoszone ubezpieczycielowi w czasie $t_2$. Proces rozliczenia dla tego konkretnego roszczenia rozwijał się od czasu $t_2$ do czasu $t_6$, to jest, od zgłoszenia do ostatecznego rozliczenia, lub zamknięcia. Roszczenie jest otwarte od czasu $t_2$ do czasu $t_6$. Dwa poziomy widoczne na Rys. 4.4 reprezentują punkt widzenia posiadacza polisy (górna część), gdzie
- wystąpienie,
- zgłoszenie i
- płatności na rzecz ubezpieczonego lub osób trzecich)
i punkt widzenia ubezpieczyciela (dolna część), gdzie
- ubezpieczyciel nie jest świadomy roszczenia przed jego zgłoszeniem,
- tworzy rezerwę, zaczynając od początkowej oceny przypadku w czasie $t_2$,
- dostosowuje rezerwę w czasie, aby odzwierciedlić najlepszą ocenę ostatecznego kosztu roszczenia, w zależności od informacji otrzymanych od ubezpieczonego (ocena eksperta lub orzeczenie, na przykład).

Zaległe roszczenia odpowiadają wszystkim roszczeniom, które nie zostały jeszcze rozliczone. Są one klasyfikowane w różnych kategoriach:
- **IBNR** (Incurred But Not Reported): roszczenia, które wystąpiły, ale nie zostały zgłoszone (plik roszczenia jest klasyfikowany jako IBNR od czasu $t_1$ do czasu $t_2$ na Rys. 4.4).
- **otwarte lub RBNS** roszczenia (Reported But Not Settled): roszczenia, które zostały zgłoszone ubezpieczycielowi, ale nie (całkowicie) zapłacone (plik roszczenia jest klasyfikowany jako RBNS od czasu $t_2$ do czasu $t_6$ na Rys. 4.4).

Na dzień bilansowy aktuariusz ma zestaw otwartych roszczeń i oblicza odpowiadającą rezerwę RBNS, plus zestaw ukrytych roszczeń, dla których należy ustalić rezerwę IBNR. Całkowita rezerwa na zaległe roszczenia oznacza sumę rezerwy RBNS plus IBNR.

Oczywiście, wzorzec zgłaszania i rozliczania roszczeń różni się w zależności od linii biznesowej. Ogólnie rzecz biorąc, szkody majątkowe prowadzą do krótkiego ogona w spływie (co oznacza, że roszczenie jest szybko zgłaszane i rozliczane), a odpowiedzialność cywilna do średniego lub długiego ogona. Dopóki wszystkie roszczenia nie zostaną rozliczone, ubezpieczyciele muszą tworzyć rezerwy reprezentujące ich szacunkowe zobowiązania z tytułu roszczeń, które wystąpiły w dniu wyceny lub przed nim. Dla ogólnej firmy ubezpieczeniowej, największą pozycją bilansową jest często rezerwa na zaległe roszczenia. W praktyce, rezerwy te muszą być oceniane metodami aktuarialnymi
- aby uniknąć subiektywnych osądów i w celu raportowania do władz regulacyjnych.
- ponieważ organy podatkowe wymagają, aby kwoty rezerw były obliczane na podstawie dobrze ugruntowanych metod/ogólnie przyjętych zasad.
- aby uwzględnić ukryte, roszczenia IBNR.

Obliczanie rezerw tradycyjnie było przeprowadzane na podstawie zagregowanych danych podsumowanych w trójkątach spływu odpowiadających latom wypadku i kolumnom odpowiadającym latom rozwoju. Takie dane wykazują trzy wymiary: dla każdego wypadku (lub roku underwritingowego) AY i okresu rozwoju (uważanego za jeden rok, nawet jeśli może to być semestr lub kwartał) DY = 1, 2, ..., odczytujemy w komórce (AY, DY) wewnątrz trójkąta całkowitą kwotę zapłaconą przez ubezpieczyciela w roku kalendarzowym CY = AY + DY - 1 dla roszczeń pochodzących z roku AY, liczbę dokonanych płatności lub średnią kwotę na płatność. Na podstawie danych o roszczeniach w trójkącie spływu, aktuariusz chce dokonać prognoz dotyczących płatności, które mają być dokonane w przyszłych latach kalendarzowych. Celem aktuarialnych technik rezerwacji jest przewidzenie tych danych, tak aby uzupełnić trójkąt do prostokąta.

Często obliczenia rezerw są przeprowadzane na jednym trójkącie zbierającym roczne sumy płatności przez AY. To podejście miesza dynamikę częstotliwości i ciężkości, a to często zaciemnia analizę. Prowadzenie oddzielnych analiz dla częstotliwości i ciężkości pozwala aktuariuszowi wyizolować każdy efekt, co ułatwia interpretację modelu i prognozy.

Załóżmy na przykład, że tabele używane w sprawach sądowych do obliczania przyszłych strat w sprawach o obrażenia ciała i śmiertelne wypadki są korygowane w roku kalendarzowym $c^*$ (takie tabele są dostępne w większości krajów; nazywane są tabelami Ogdena w Wielkiej Brytanii). Może to być spowodowane na przykład zmianą stopy dyskontowej, w wyniku czego wszystkie kwoty płatne do rozliczenia zostaną zwiększone w znacznym stopniu. Biorąc pod uwagę trójkąt z (przyrostowymi) rocznymi płatnościami, oczekuje się, że spowoduje to wzrost wzdłuż diagonali $c^*$ i następnych przekątnych CY. Jednak efekt ten wpływa tylko na koszty i nie modyfikuje komponentu częstotliwości. Staje się to jasne, gdy aktuariusz pracuje z dwoma oddzielnymi trójkątami, to jest,

Trójkąt 1: liczba płatności przez AY i DY, oraz

Trójkąt 2: średnia kwota płatności przez AY i DY.

Zmiany w tabelach referencyjnych manifestują się w trójkącie 2, powodując wzrost na przekątnych $c^*$ i następnych. W modelach regresyjnych można wprowadzić dodatkowe cechy, takie jak $1[CY \ge c^*]$ w analizie trójkąta 2, aby uwzględnić ten efekt. Wiele problemów można uniknąć, jeśli analizy oddzielnych liczebności i ciężkości są analizowane.

Rozważmy dane roszczeń z portfela ubezpieczeń OC komunikacyjnych firmy ubezpieczeniowej działającej w Unii Europejskiej. Dostępne informacje obejmują 11 lat wypadków. Tabela 4.6 przedstawia statystyki opisowe dla płatności na rok wypadku i rok rozwoju. Widzimy tam liczbę płatności, średnie płatności na AY, a także odpowiadające odchylenie standardowe. Dane są wyświetlane na rok wypadku i rok rozwoju. Odpowiedzią rozważaną tutaj jest średnia wypłacona kwota. Tabela 4.6 pokazuje typowy wzrost średnich płatności wraz z rozwojem, wraz z odpowiadającym spadkiem liczby płatności.

Kwoty pojawiające się w Tabeli 4.6 są wyrażone w euro. Wypłaty ubezpieczeniowe są generalnie korygowane o inflację przed rozpoczęciem analizy GLM. Kwoty odszkodowań za obrażenia ciała są często korygowane za pomocą wskaźnika płac, podczas gdy wskaźnik cen konsumpcyjnych jest używany do szkód materialnych. Ale często pozostaje pewna inflacja, nawet po tej korekcie. Dzieje się tak, ponieważ inflacja ubezpieczeniowa podlega również superinflacji, to jest, odbiega od wskaźników makroekonomicznych (i generalnie rośnie w szybszym tempie). Modele regresyjne, takie jak GLM, mogą być używane do szacowania efektów inflacji. Odbywa się to poprzez włączenie odpowiednich cech CY do oceny. Szacowane efekty regresji odpowiadające CY ujawniają wtedy inflację mającą zastosowanie do rozważanego produktu ubezpieczeniowego. Jeśli kwoty inflacji wchodzą do analizy GLM, to te współczynniki regresji korygują inflację zawartą w danych o roszczeniach wstępnie przetworzonych.

Trzy efekty AY, DY i CY obecne w trójkątach spływu wychwytują różne aspekty dynamiki strat ubezpieczeniowych:

**AY** rok wypadku AY (efekt wiersza), zwany również rokiem wystąpienia lub rokiem pochodzenia, reprezentuje wariacje wielkości portfela.

**DY** rok rozwoju DY (efekt kolumny), zwany również opóźnieniem zgłoszenia lub opóźnieniem rozliczenia, reprezentuje opóźnienia w raportowaniu przez posiadaczy polis i procedurze obsługi roszczeń przez firmę.

**CY** rok kalendarzowy CY (efekt diagonalny), zwany również rokiem płatności, reprezentuje inflację i orzecznictwo.

Przejdźmy teraz od klasycznej reprezentacji trójkątnej do danych wyświetlanych dla analizy GLM. Mamy tutaj trzy cechy: AY, DY i CY, powiązane relacją CY = AY + DY - 1. W pierwszym kroku analizy cechy te są traktowane jako kategoryczne w celu maksymalizacji elastyczności. Oznacza to, że jeden parametr jest powiązany z każdym okresem w celu wyjaśnienia wypłaconej kwoty. Każda obserwacja $i$ odpowiada jednej komórce trójkąta spływu. Odtąd, oznaczamy przez AY(i), DY(i) i CY(i) odpowiedni rok wypadku, rok rozwoju i rok kalendarzowy dla obserwacji $i$.

Całkowita płatność $Y_i$ w roku kalendarzowym CY(i) pochodząca z roku wypadku AY(i) jest rozkładana na złożoną sumę

$$
Y_i = \sum_{k=1}^{N_i} P_{ik} = N_i \bar{P}_i,
$$

gdzie

$N_i$ = liczba płatności dokonanych w roku kalendarzowym CY(i) dla roszczeń pochodzących z roku wypadku AY(i) wciąż otwartych w roku rozwoju DY(i);

$P_{ik}$ = odpowiadające kwoty;

$\bar{P}_i = \frac{Y_i}{N_i}$ = średnia płatność dokonana w roku kalendarzowym CY(i) dla roszczeń pochodzących z roku wypadku AY(i) wciąż otwartych w roku rozwoju DY(i).

Zakłada się, że wszystkie te zmienne losowe są wzajemnie niezależne. Dla danego $i$, zakłada się, że zmienne losowe $P_{i1}, P_{i2}, \dots$ są identycznie rozłożone. Zauważ, że tutaj płatności związane z indywidualnymi polisami nie są śledzone, tylko płatności za zbiór są modelowane, a wszystkie płatności związane z tym samym roszczeniem są agregowane w jednej, rocznej kwocie.

Wszystkie zmienne losowe $P_i$ zakłada się, że są wzajemnie niezależne, biorąc pod uwagę efekty AY, DY i CY zawarte w ocenie. Zauważ, że znajomość średniej kwoty płatności jest wystarczająca do oszacowania parametrów w ustawieniu GLM (nie ma utraty informacji z powodu agregacji wszystkich pojedynczych płatności $P_{i1}, P_{i2}, \dots, P_{iN_i}$ w jedną wartość $\bar{P}_i$ w tym przypadku, jak wyjaśniono wcześniej).

Plik danych zawiera cechy AY i DY, wraz z liczbą płatności, średnią spłatą i odpowiadającym odchyleniem standardowym. Odtąd zakładamy, że baza danych obejmuje $m$ lat wypadków. Oznaczamy przez $\omega$ maksymalną liczbę lat potrzebnych do rozliczenia wszystkich roszczeń pochodzących z danego roku kalendarzowego. Oczywiście, $\omega \ge m$, ale tutaj zakładamy $\omega=m$ dla uproszczenia.

Biorąc pod uwagę liczbę płatności, pozwalamy sobie przyjąć podejście modelowania zgodne z klasycznym podejściem Chain-Ladder do rezerwowania. W tym celu zakładamy, że liczba płatności w komórce $i$ może być zapisana jako

$$
\mathrm{E}[N_i] = \alpha_{\text{AY}(i)} \delta_{\text{DY}(i)}.
$$

Czasami jako przesunięcie (takie jak całkowity dochód ze składek, na przykład) używana jest miara wolumenu. Aby uniknąć nadmiernej parametryzacji, aktuariusze generalnie nakładają tożsamość

$$
\sum_{j=1}^\omega \delta_j = 1
$$

zachodzi. Wtedy,

$$
\alpha_k = \alpha_k \sum_{j=1}^\omega \delta_j = \sum_{i|AY(i)=k} \mathrm{E}[N_i]
$$

więc $\alpha_k$ reprezentuje oczekiwaną liczbę płatności potrzebnych do rozliczenia wszystkich roszczeń pochodzących z roku wypadku $k$. Z tym ograniczeniem, $\delta_j$ jest proporcją każdej $\alpha_k$ odpowiadającą rozwojowi $j$. Intuicyjna idea stojąca za tym modelem polega na tym, że stabilny procent $\delta_j$ całkowitej liczby płatności potrzebnych do rozliczenia wszystkich roszczeń jest dokonywany w każdym roku wypadku, to jest, kolumny trójkąta spływu są z grubsza proporcjonalne (ponieważ oczekiwane wartości są zakładane jako proporcjonalne).
Parametry $\alpha_k$ i $\delta_j$ są szacowane metodą największej wiarygodności w modelu

$$
N_i \sim \mathcal{Poi}(\alpha_{\text{AY}(i)} \delta_{\text{DY}(i)}) \text{ niezależnie.}
$$

Oznacza to, że używamy GLM Poissona z oceną postaci

$$
\text{score}_i = \sum_{k=1}^\omega \beta_k I[\text{AY}(i)=k] + \sum_{l=1}^\omega \beta_{\omega+l} I[\text{DY}(i)=l]
$$

gdzie cechy binarne $I[AY(i)=k]$ i $I[DY(i)=l]$ kodują cechy kategoryczne AY i DY, a $\beta_k = \ln \alpha_k$, podczas gdy $\beta_{\omega+k} = \ln \delta_k$, $k=1, 2, \dots, \omega$.

Równania wiarygodności można łatwo rozwiązać bezpośrednio, bez użycia algorytmu IRLS. Idea (znana jako algorytm Verbeeka we wspólnocie aktuarialnej) polega na postępowaniu w następujący sposób. Ponieważ użyliśmy kanonicznej funkcji łączącej dla GLM Poissona, równania wiarygodności polegają na zrównaniu sumy wszystkich liczb płatności w każdym wierszu i w każdej kolumnie z ich odpowiadającą wartością oczekiwaną. Obliczając sumę wszystkich wartości wzdłuż pierwszego wiersza trójkąta spływu (odpowiadającego pierwszemu rokowi wypadku, założonemu jako w pełni rozliczony jako $m=\omega$) i przyrównując wynik do sumy wartości oczekiwanych, otrzymujemy

$$
\sum_{i|AY_i=1} N_i = \omega \sum_{j=1}^\omega \alpha_1 \beta_j \implies \hat{\alpha}_1 = \sum_{i|AY_i=1} N_i.
$$

Teraz, rozważmy ostatnią kolumnę trójkąta spływu, dla której jest jedna komórka z obserwowaną wartością. Równanie wiarygodności daje wtedy

$$
N_i|_{AY_i=1 \text{ i } DY_i=\omega} = \alpha_1 \beta_\omega \implies \hat{\beta}_\omega = \frac{N_i|_{AY_i=1 \text{ i } DY_i=\omega}}{\hat{\alpha}_1}.
$$

Przejdźmy teraz do drugiego wiersza trójkąta, rejestrując obserwacje dla drugiego roku wypadku. Wszystkie wartości pojawiające się w drugim wierszu zostały zaobserwowane, z wyjątkiem ostatniej komórki (w rozwoju $\omega$). Sumując wszystkie wartości pojawiające się w tym wierszu i przyrównując wynik do odpowiadających wartości oczekiwanych, otrzymujemy

$$
\sum_{i|AY_i=2} N_i = \sum_{j=1}^{\omega-1} \alpha_2 \beta_j = \alpha_2(1-\beta_\omega) \implies \hat{\alpha}_2 = \frac{\sum_{i|AY_i=2} N_i}{1 - \hat{\beta}_\omega}.
$$

Następnie rozważamy kolumnę $\omega-1$, wiersz 3, kolumnę $\omega-2$ i tak dalej, aż wszystkie parametry zostaną oszacowane. Oczywiście, połączenie linku z GLM uzupełnia klasyczny wynik Chain-Ladder o błędy standardowe i miary diagnostyczne, które wydają się być przydatne do oceny adekwatności tego podejścia na konkretnym trójkącie spływu.

Rozważmy liczbę płatności $N_i$, wyświetloną w Tabeli 4.6. Jeśli użyjemy specyfikacji multiplikatywnej odpowiadającej podejściu Chain-Ladder, faktoringu $E[N_i]$ w iloczyn efektu wiersza i kolumny, oznacza to, że ocena postaci

$$
\text{score}_i = \sum_{k=1}^{11} \beta_k I[\text{AY}(i)=k] + \sum_{j=1}^{11} \beta_{11+j} I[\text{DY}(i)=l]
$$

jest w GLM Poissona. Efekt kolumny podlega teraz ograniczeniu identyfikowalności

$$
\sum_{j=12}^{22} \exp(\beta_j) = 1.
$$

Stąd $\exp(\beta_j), j=11, \dots, 22$, można interpretować jako proporcję oczekiwanej całkowitej liczby płatności $\exp(\beta_1)$, potrzebnych do rozliczenia wszystkich roszczeń pochodzących z roku wypadku $l \in \{1, \dots, 11\}$ dokonanych w rozwoju $j=11$. Rysunek 4.5 przedstawia oszacowane $\alpha_j$ i $\delta_j$, które zostały uzyskane metodą największej wiarygodności Poissona.

Algorytm Chain-Ladder zastosowany do tych samych danych dałby te same dopasowane wartości (ale inne czynniki łączące, ponieważ Chain-Ladder pozwala aktuariuszowi przełączać się z jednego rozwoju na następny w trójkącie spływu). Widzimy, że oczekiwane liczby płatności, to jest, oszacowane parametry $\alpha_j$ dla $j=1, \dots, 11$, wyświetlone na Rys. 4.5, najpierw rosną, osiągając maksimum przed spadkiem. To odzwierciedla konkretną zmianę wolumenu dla rozważanego portfela. Intuicyjnie, oczekiwalibyśmy, że oszacowane $\alpha_j$ pozostaną z grubsza stabilne w czasie, co sugeruje, że wolumen biznesu pozostaje niezmieniony, lub umiarkowanie wzrasta, kompensowany przez postępującą redukcję roszczeń w ubezpieczeniach komunikacyjnych. W tym przypadku, kształt dzwonu może być wyjaśniony okolicznościami specyficznymi dla tej firmy ubezpieczeniowej, w tym postępującą integracją działalności sprzedawanej za pośrednictwem banku.

Kształt oszacowanych parametrów $\delta_j$ w odniesieniu do DY, $j=1, \dots, 11$, wyświetlony na Rys. 4.5, jest dość standardowy. Widzimy, że większość płatności jest dokonywana w roku wypadku (około 55%) i rok później (około 35%), późniejsze developments uwzględniają pozostałe roszczenia. W ubezpieczeniach OC komunikacyjnych, doświadczenie generalnie pokazuje, że zdecydowana większość roszczeń jest rozliczana w rozwoju 1-2.

Przejdźmy teraz do średnich płatności. Oczywiście, podejście Poissona leżące u podstawy metody Chain-Ladder nie jest tutaj bardzo atrakcyjne. Jak wyjaśniono w Sekcji 4.11, ważnym aspektem modelowania jest relacja średnia-wariancja. Założenie Poissona odpowiada przypadkowi, w którym średnia-wariancja jest równa. Założenie Poissona jest równoznaczne z oczekiwaną wartością wypłaconej kwoty. Jest to kluczowe założenie ukryte za podejściem Chain-Ladder: w przypadku, gdy średnia wypłacona kwota jest równa 1 milionowi, wariancja jest również równa tej kwocie, a odchylenie standardowe wynosi tysiąc. Biorąc pod uwagę, że rozkład Poissona z tak wysoką średnią jest bardzo zbliżony do rozkładu normalnego, oznacza to, że mamy bardzo dużą koncentrację wokół średniej, z dyspersją wynoszącą zaledwie kilka tysięcy. Tak mała dyspersja może być zbyt optymistyczna, a relacja średnia-wariancja musi być zakwestionowana i nie ufana ślepo, jak wyjaśniono dalej.

Biorąc pod uwagę średnie kwoty płatności $\bar{P}_i$, moglibyśmy użyć specyfikacji Gamma lub odwrotnej Gaussa. Wiemy, że te dwa modele różnią się funkcją wariancji: $V(\mu)=\mu^2$ dla dystrybucji Gamma, podczas gdy $V(\mu)=\mu^3$ dla dystrybucji odwrotnej Gaussa. Aby wybrać bardziej odpowiednią odpowiedź, dopasowujemy model regresji kubicznej i kwadratowej z odpowiedzią wariancji na średnią odpowiedzi (bez wyrazu wolnego). Skorygowany R-kwadrat wynosi 73.66% (100% reprezentuje idealne dopasowanie) dla funkcji łącza kubicznego, podczas gdy zmniejsza się do 52.53% dla funkcji łącza kwadratowego. Sugeruje to użycie dystrybucji odwrotnej Gaussa dla odpowiedzi. Zauważ, że funkcja wariancji $V(\mu)=\mu$ odpowiadająca modelowi Chain-Ladder nie jest dobrym kandydatem dla rozważanych danych, ponieważ skorygowany R-kwadrat wynosi tylko 31.41% w tym przypadku.

Najpierw porównujemy model dekomponujący oczekiwaną kwotę na płatność na iloczyn efektu AY i DY oraz efektu CY z modelem dekomponującym go na iloczyn efektu AY i DY. Dopasowanie uzyskane w tym drugim przypadku było znacznie lepsze, więc odtąd zakładamy, że

$$
\bar{P}_i \sim \mathcal{IGau}(\mu_i, \tau)
$$

ze średnią wypłaconą kwotą rozłożoną na iloczyn efektu AY i DY:

$$
\mu_i = \alpha_{\text{AY}(i)} \delta_{\text{DY}(i)}.
$$

Zauważ, że ta specyfikacja uwzględnia stałą stopę inflacji $i$, jako inflację

$$
(1+i)^{\text{AY}(i)+\text{DY}(i)-2}
$$

przyjmując pierwszy rok wypadku jako rok bazowy dla inflacji. To uwzględnia

$$
(1+i)^{\text{AY}(i)+\text{DY}(i)-2} = (1+i)^{\text{AY}(i)-1} (1+i)^{\text{DY}(i)-1}
$$

gdzie każdy $(1+i)^{\text{AY}(i)-1}$ i $(1+i)^{\text{DY}(i)-1}$ może być wchłonięty przez efekty AY $\alpha$ i efekty DY $\delta$. Stopa inflacji $i$ jest zatem implicite zawarta w oszacowanym wierszu i kolumnie. Odpowiadająca ocena to

$$
\text{score}_i = \sum_{k=1}^{11} \beta_k I[\text{AY}(i)=k] + \sum_{l=1}^{11} \beta_{11+l} I[\text{DY}(i)=l]
$$

gdzie cechy binarne $I[AY(i)=k]$ i $I[DY(i)=l]$ kodują cechy kategoryczne AY i DY. Rozważając wyniki analizy GLM, musimy pamiętać, że dopasowanie jest idealne w ostatniej komórce pierwszego wiersza (AY=1, DY=11). Dzieje się tak, ponieważ jest to jedyna obserwacja obejmująca współczynnik regresji $\beta_{22}$, który jest zatem określany tylko przez tę obserwację. Podobnie, dla ostatniej komórki w pierwszym kolumnie (AY=11, DY=1), która jest jedyną obejmującą $\beta_{11}$.

Model jest dopasowywany za pomocą funkcji R `glm`, z łączem logarytmicznym i liczbą płatności w każdej komórce jako wagą. Algorytm IRLS zbiegł się w 7 krokach. Wyniki są wyświetlone na Rys. 4.6. Widzimy, że oszacowane współczynniki regresji odpowiadające latom wypadków są najpierw stabilne w latach AY 1-6, a następnie rosną w ostatnich latach. Biorąc pod uwagę oszacowane współczynniki regresji związane z opóźnieniem rozwoju, widzimy, że najpierw rosną regularnie wraz z rozwojem, ale wykazują mniej stabilne zachowanie w ostatnich latach rozwoju.

## 4.5 Dewiancja

### 4.5.1 Model Zerowy

Model zerowy odpowiada przypadkowi, w którym cechy nie wnoszą żadnych informacji o odpowiedzi. Wobec braku związku między cechami a odpowiedzią, dopasowujemy wspólną średnią $\mu$ do wszystkich obserwacji. Jest to więc pojedynczy parametr $\beta_0$ dla wszystkich obserwacji, taki że

$$
a'(\theta_i) = \bar{y} \iff \hat{\mu}_i = \bar{y} \text{ dla } i=1, 2, \dots, n \iff \hat{\beta}_0 = g^{-1}(\bar{y}).
$$

Wynik ten bezpośrednio wynika z właściwości estymat największej wiarygodności dla rozkładów ED ustalonych w Rozdz. 3.

W modelu zerowym dane są reprezentowane w całości jako losowe wahania wokół wspólnej średniej $\mu$. Jeśli model zerowy ma dobre dopasowanie, dane są jednorodne i nie ma powodu, aby naliczać różne składki premium podgrupom ubezpieczonych.

### 4.5.2 Model Pełny, czyli Nasycony

Dla wybranego rozkładu ED, model o najlepszym możliwym dopasowaniu liczy tyle samo parametrów co obserwacji. Dopasowanie jest więc idealne, a model estymuje $\mu_i$ dla średniej odpowiedzi jako odpowiadającą jej obserwację $y_i$. Ten model nazywany jest modelem pełnym lub nasyconym. Jest on używany jako odniesienie do oceny dobroci dopasowania dowolnego GLM opartego na tym samym rozkładzie ED.

Model pełny zapewnia całkowitą elastyczność w dopasowaniu, tak że odpowiadająca estymata największej wiarygodności $\hat{\theta}_i$ rozwiązuje

$$
a'(\tilde{\theta}_i) = y_i \iff \tilde{\mu}_i = y_i \text{ dla } i=1, 2, \dots, n,
$$

jak ustalono w Rozdz. 2. Rozwiązanie jest oznaczone jako

$$
\tilde{\theta}_i = (a')^{-1}(y_i), \quad i=1, 2, \dots, n.
$$

Zatem każda dopasowana wartość jest równa obserwacji, a model pełny idealnie pasuje. Jednakże model ten nie wydobywa z danych żadnej struktury, a jedynie powtarza dostępne obserwacje, nie kondensując ich.

### 4.5.3 Dewiancja

#### 4.5.3.1 Statystyka Ilorazu Wiarygodności

Model statystyczny opisuje, jak aktuariusz dzieli zmienność obecną w danych na strukturę systematyczną (reprezentowaną w wyniku GLM) i losowe odchylenia od wartości oczekiwanych (wahania wywołane przez założony rozkład ED, lub relację średnia-wariancja). Model zerowy reprezentuje jedną skrajność, gdzie dane są czysto losowe, podczas gdy model nasycony lub pełny reprezentuje dane jako całkowicie systematyczne. Model pełny zapewnia aktuariuszowi miarę tego, jak dobrze jakikolwiek model oparty na rozważanym rozkładzie ED może dopasować dane. Ponieważ model pełny daje najwyższą osiągalną log-wiarygodność z rozkładem ED w ramach rozważań, różnica między log-wiarygodnością $L_{\text{full}}$ modelu pełnego a log-wiarygodnością $L(\hat{\boldsymbol{\beta}})$ uzyskaną z tego GLM mierzy dobroć dopasowania. Prowadzi to do statystyki ilorazu wiarygodności odpowiadającej dwukrotności tej różnicy. W rodzinie ED, ta statystyka ilorazu wiarygodności jest dana przez

$$
LR = 2(L_{\text{full}} - L(\hat{\boldsymbol{\beta}})) = \frac{2}{\phi} \sum_{i=1}^n v_i \left( y_i(\tilde{\theta}_i - \hat{\theta}_i) - a(\tilde{\theta}_i) + a(\hat{\theta}_i) \right)
$$

gdzie $\tilde{\theta}_i$ są estymatami w modelu pełnym, a $\hat{\theta}_i$ są estymatami w rozważanym modelu. Zbyt duża wartość LR wskazuje, że model nie pasuje zadowalająco do rzeczywistych danych, podczas gdy zbyt mała wartość LR może sygnalizować, że rozważany model ma niską moc wyjaśniającą. Tutaj „zbyt mała” odnosi się do kwantyla rozkładu Chi-kwadrat, który wydaje się zapewniać rozsądną aproksymację rozkładu LR przy łagodnych warunkach regularności.

#### 4.5.3.2 Dewiancja

Podczas pracy z GLM, przydatne jest posiadanie wielkości, którą można interpretować jako sumę kwadratów błędów (lub reszt) w modelu regresji normalnej. Ta wielkość nazywana jest dewiancją, czyli dewiancją resztową modelu i jest zdefiniowana jako

$$
\begin{aligned}
D(\boldsymbol{y}, \boldsymbol{\hat{\mu}}) &= \phi LR \\
&= 2\phi(L_{\text{full}} - L(\hat{\boldsymbol{\beta}})) \\
&= 2 \sum_{i=1}^n v_i \left( y_i(\tilde{\theta}_i - \hat{\theta}_i) - a(\tilde{\theta}_i) + a(\hat{\theta}_i) \right).
\end{aligned}
$$

Jest to miara odległości między konkretnym modelem a obserwowanymi danymi zdefiniowanymi za pomocą modelu nasyconego. Podobnie do sumy kwadratów błędów, kwantyfikuje ona wahania w danych, które nie są wyjaśnione przez rozważany model.

---
**Tabela 4.7** Dewiancja związana z GLM opartymi na niektórych członkach rodziny rozkładów ED.

| Rozkład | Dewiancja |
| :--- | :--- |
| Dwumianowy | $2 \sum_{i=1}^n (y_i \ln \frac{y_i}{\hat{\mu}_i} + (n_i - y_i) \ln \frac{n_i - y_i}{n_i - \hat{\mu}_i})$ gdzie $\hat{\mu}_i = n_i \hat{q}_i$ |
| Poissona | $2 \sum_{i=1}^n (y_i \ln \frac{y_i}{\hat{\mu}_i} - (y_i - \hat{\mu}_i))$ gdzie $y \ln y = 0$ jeśli $y=0$ |
| Normalny | $\sum_{i=1}^n (y_i - \hat{\mu}_i)^2$ |
| Gamma | $2 \sum_{i=1}^n (- \ln \frac{y_i}{\hat{\mu}_i} + \frac{y_i - \hat{\mu}_i}{\hat{\mu}_i})$ |
| Odwrotny Gaussowski | $\sum_{i=1}^n \frac{(y_i - \hat{\mu}_i)^2}{\hat{\mu}_i^2 y_i}$ |
---

Ponieważ model nasycony musi pasować do danych co najmniej tak samo dobrze jak każdy inny model, dewiancja resztowa nigdy nie jest ujemna. Im większa dewiancja, tym większe różnice między rzeczywistymi danymi a dopasowanymi wartościami. Dewiancja modelu zerowego nazywana jest dewiancją zerową.

Tabela 4.7 przedstawia dewiancję związaną z GLM opartymi na niektórych członkach rodziny rozkładów ED. Drugi składnik $\sum_{i=1}^n (y_i - \hat{\mu}_i)$ w dewiancji Poissona jest zwykle równy 0, gdy w ocenie uwzględniony jest wyraz wolny, więc dewiancja Poissona redukuje się do $2\sum_{i=1}^n y_i \ln \frac{y_i}{\hat{\mu}_i}$. W przypadku Poissona, dewiancja jest również nazywana statystyką G. Zauważ, że w tej tabeli, $y \ln y$ przyjmuje się jako 0, gdy $y=0$ (jego granica, gdy $y \to 0$).

#### 4.5.3.3 Dewiancja Skalowana

Dla rodzin z znanym parametrem dyspersji $\phi$, takich jak dwumianowy lub Poissona, dla których $\phi=1$, dewiancja stanowi podstawę do testowania braku dopasowania modelu lub do porównywania zagnieżdżonych modeli. Jeśli $\phi$ musi być oszacowane na podstawie danych, wtedy należy użyć dewiancji skalowanej. Dokładniej, dewiancja skalowana jest dana przez

$$
\tilde{D}(\boldsymbol{y}, \boldsymbol{\hat{\mu}}) = \frac{1}{\phi} D(\boldsymbol{y}, \boldsymbol{\hat{\mu}}).
$$

Zauważ, że dewiancja skalowana zależy od parametru dyspersji $\phi$. Gdy $\phi=1$ (podobnie jak w przypadkach dwumianowym i Poissona), dewiancja i dewiancja skalowana są takie same. Powszechną praktyką jest po prostu podłączenie oszacowania $\hat{\phi}$ w celu obliczenia $\tilde{D}$.

#### 4.5.3.4 Rozkład Próbkowy Dewiancji Skalowanej

Koncepcja dewiancji rozciąga się na wszystkich członków rodziny ED znaną sumę kwadratów reszt używaną w regresji liniowej normalnej. Liczba stopni swobody związana z dewiancją skalowaną jest równa liczbie $n$ obserwacji minus liczba $p+1$ parametrów regresji $\beta_0, \beta_1, \dots, \beta_p$ do oszacowania. W dużych próbach, dewiancja skalowana ma w przybliżeniu rozkład Chi-kwadrat z $n-(p+1)$ stopniami swobody, tj.

$$
\tilde{D} \approx \chi^2_{n-p-1} \text{ pod warunkiem, że } n \text{ jest wystarczająco duże.}
\quad\quad\text{(4.16)}
$$

Duża dewiancja wskazuje na źle dopasowany model. Dokładniej, jeśli GLM pasuje do danych rozsądnie dobrze, to skalowana dewiancja $\tilde{D}$ powinna być bliska liczbie resztowych stopni swobody, tj. $n-p-1$.

Zauważ, że do aproksymacji Chi-kwadrat w (4.16) należy podchodzić z pewną ostrożnością. Istnieją sytuacje, w których nie jest ona w ogóle zachowana, na przykład w przypadku odpowiedzi binarnych (jak pokazano w następnej sekcji).

### 4.5.4 Przypadek Bernoulliego

Maksymalizacja log-wiarygodności lub minimalizacja dewiancji jest równoważna z punktu widzenia numerycznego, z wyjątkiem niektórych szczególnych przypadków, jak pokazano dalej. Załóżmy, że $Y_i \sim \mathcal{Ber}(q_i)$. Wtedy log-wiarygodność zapisuje się jako

$$
L(\boldsymbol{\beta}) = \sum_{i=1}^n (y_i \ln q_i + (1 - y_i) \ln(1 - q_i)).
$$

W modelu nasyconym, $\hat{q}_i = y_i$, więc log-wiarygodność jest równa 0, ponieważ

$$
y_i \ln y_i = (1 - y_i) \ln(1 - y_i) = 0 \text{ gdy } y_i \in \{0, 1\}.
$$

Dewiancja jest wtedy równa minus dwukrotności log-wiarygodności rozważanego modelu, to jest,

$$
\begin{aligned}
D = -2L(\hat{\boldsymbol{\beta}}) &= -2 \sum_{i=1}^n (y_i \ln \hat{q}_i + (1 - y_i) \ln(1 - \hat{q}_i)) \\
&= -2 \sum_{i=1}^n \left( y_i \ln \frac{\hat{q}_i}{1-\hat{q}_i} + \ln(1 - \hat{q}_i) \right).
\end{aligned}
\quad\quad\text{(4.17)}
$$

Różniczkując log-wiarygodność Bernoulliego $L(\boldsymbol{\beta})$ względem $\beta_j$, otrzymujemy

$$
\frac{\partial}{\partial\beta_j}L(\boldsymbol{\beta}) = \sum_{i=1}^n \left( \frac{y_i}{q_i} - \frac{1-y_i}{1-q_i} \right) q_i(1-q_i)x_{ij} = \sum_{i=1}^n (y_i - q_i)x_{ij}
$$

gdzie rozpoznajemy szczególny przypadek (4.4) (ponieważ logit jest kanoniczną funkcją łączącą dla GLM Bernoulliego). Stąd,

$$
\sum_{j=1}^p \beta_j \frac{\partial}{\partial\beta_j}L(\boldsymbol{\beta}) = \sum_{i=1}^n (y_i - q_i) \sum_{j=1}^p \beta_j x_{ij} = \sum_{i=1}^n (y_i - q_i) \ln \frac{q_i}{1-q_i}.
$$

Lewa strona jest równa zero, gdy jest oceniana przy estymacie największej wiarygodności $\hat{\boldsymbol{\beta}}$, tak że

$$
\sum_{i=1}^n (y_i - \hat{q}_i) \text{logit}(\hat{q}_i) = 0 \iff \sum_{i=1}^n y_i \text{logit}(\hat{q}_i) = \sum_{i=1}^n \hat{q}_i \text{logit}(\hat{q}_i).
$$

Wstawiając ten wynik do (4.17), otrzymujemy

$$
D = -2 \sum_{i=1}^n (\hat{q}_i \text{logit}(\hat{q}_i) + \ln(1 - \hat{q}_i)).
$$

Widzimy, że dewiancja $D$ zależy tylko od dopasowanych wartości $\hat{q}_i$ z $q_i$, a nie od rzeczywistych obserwacji $y_i$. Dla dewiancji, aby zmierzyć dobroć dopasowania, musi ona porównywać dopasowane wartości $\hat{q}_i$ z danymi $y_i$, ale tutaj mamy tylko funkcję $\hat{q}_i$. To pokazuje, że dewiancja nie wnosi żadnych informacji o dobroci dopasowania, gdy $Y_i \sim \mathcal{Ber}(q_i)$ i używana jest funkcja łącząca logit. Stąd nie można jej użyć do pomiaru adekwatności modelu w takim przypadku.

### 4.5.5 Kary Kowariancyjne i AIC

#### 4.5.5.1 Pomiar Dobroci Dopasowania

Zagnieżdżone modele można porównywać za pomocą dewiancji. Jednakże, gdy modele nie są zagnieżdżone lub postulują różne rozkłady dla odpowiedzi, bezpośrednie porównanie staje się problematyczne. W takim przypadku, kryteria informacyjne pozwalają aktuariuszowi na porównywanie różnych modeli. Klasyczne kryteria obejmują Kryterium Informacyjne Akaikego

$$
AIC = -2L + 2\#\text{parametrów},
$$

i Bayesowskie Kryterium Informacyjne

$$
BIC = -2L + \ln(n)\#\text{parametrów}.
$$

Zauważ, że liczba parametrów uwzględnia wszystkie parametry zawarte w modelu (parametr dyspersji $\phi$ jest również liczony jako parametr w oparciu o log-wiarygodność $L$ i odpowiada za miarę złożoności modelu). Preferowane są mniejsze wartości AIC lub BIC.

Powszechną praktyką jest odrzucanie części wiarygodności, które nie są funkcjami parametrów. To jest wspólne do porównywania modeli zakładających ten sam rozkład dla odpowiedzi, ponieważ odrzucone części są równe w takim przypadku. Dla odpowiedzi o różnych rozkładach, istotne jest jednak, aby wszystkie części wiarygodności zostały zachowane.

Skorygowana wersja AIC (oznaczona jako AICC) została opracowana do użytku w małych próbach lub gdy liczba parametrów $p+1$ jest umiarkowaną do dużej częścią wielkości próby $n$, co jest typowe w przypadku rezerwowania strat w oparciu o dane zagregowane w trójkątach spływu. Jest formalnie zdefiniowana jako

$$
AICC = AIC + \frac{2(p+2)(p+3)}{n-p-1}.
$$

Z reguły, AICC powinno być używane zamiast AIC, chyba że

$$
n > 40 \times \#\text{parametrów}.
$$

#### 4.5.5.2 Powrót do Modelu Liniowej Regresji Normalnej: Kary Kowariancyjne

Zdolność predykcyjna lub wydajność generalizacji (terminologia ze społeczności uczenia maszynowego) modelu opisuje jego zdolność do predykcji na nowych danych testowych. Zdolność predykcyjna modelu może być zatem oceniana tylko na nowych, niewidzianych danych. Posiadanie modelu, który może dobrze przewidywać, jest bardzo ważne do porównywania różnych modeli lub do wyboru optymalnych wartości parametrów. Walidacja krzyżowa jest ogólną metodą estymacji takiej generalizacji lub wydajności poza próbą. Dla modeli ustrukturyzowanych, takich jak GLM, dostępne są jednak pewne wyniki analityczne, dzięki czemu intensywna obliczeniowo walidacja krzyżowa nie jest potrzebna. Błąd predykcji w modelu regresyjnym można ocenić za pomocą kar kowariancyjnych.

Rozważmy klasyczny model liniowej regresji normalnej dla odpowiedzi

$$
Y_i \sim \mathcal{Noror}(\mu_i, \boldsymbol{\sigma}^2) \text{ z } \mu_i = \boldsymbol{\beta}^\top \boldsymbol{x}_i.
$$

Parametr $\boldsymbol{\beta}$ jest szacowany na podstawie dostępnych danych $(y_i, \boldsymbol{x}_i), i=1, \dots, n$, zwanych również próbą treningową, ponieważ służy do trenowania estymatora. Wydajność modelu można następnie ocenić za pomocą pozornego błędu podanego przez średnią sumę kwadratów reszt

$$
\widehat{MSRR} = \frac{1}{n} \sum_{i=1}^n (y_i - \hat{\mu}_i)^2
$$

jest zbyt optymistyczny, ponieważ model został dopasowany, aby jak najlepiej przewidywać dane $y_i$. Jeśli użyjemy tej samej próby treningowej ponownie do pomiaru zdolności predykcyjnej modelu w trakcie rozważania, wynik będzie zbyt optymistyczny i zawsze będzie faworyzował bardziej złożone modele (tj. modele z większą liczbą stopni swobody), ponieważ MSRR może tylko maleć wraz ze wzrostem złożoności modelu. Dlatego kryteria informacyjne, takie jak AIC, są tak pomocne w porównywaniu wydajności modelu.

Alternatywnie, moglibyśmy ocenić wydajność modelu na nowych danych testowych. Pamiętaj, że wielkość $\widehat{MSRR}$ jest próbkowym analogiem

$$
MSRR = \frac{1}{n} \sum_{i=1}^n \mathrm{E}[(Y_i - \hat{\mu}_i)^2]
$$

w oparciu o próbę $(Y_i, \boldsymbol{x}_i), i=1, \dots, n$. Tutaj odpowiedzi $Y_i$ są tymi, które zostały użyte do wytrenowania modelu, to jest, do uzyskania $\hat{\mu}_i$. Aby usunąć to obciążenie, gdy oceniana jest wydajność modelu, przełączamy się na średni kwadratowy błąd predykcji zdefiniowany jako

$$
MSEP = \frac{1}{n} \sum_{i=1}^n \mathrm{E}[(Y_i^{\text{new}} - \hat{\mu}_i)^2]
$$

gdzie wartość oczekiwana jest obliczana w odniesieniu do nowej obserwacji $Y_i^{\text{new}}$ z cechą $\boldsymbol{x}_i$, rozłożoną jak $Y_i$, która nie została uwzględniona w obserwowanej próbie i jest niezależna od $Y_1, Y_2, \dots, Y_n$. Zatem $Y_i^{\text{new}}$ nie został użyty do budowy $\hat{\mu}_i$, co zależy od rzeczywistych obserwacji $Y_1, Y_2, \dots, Y_n$.

Powiążmy teraz MSEP z MSRR. W tym celu piszemy

$$
\mathrm{E}[(Y_i - \hat{\mu}_i)^2] = \mathrm{E}[(Y_i - \mu_i)^2] - 2\mathrm{E}[(Y_i - \mu_i)(\hat{\mu}_i - \mu_i)] + \mathrm{E}[(\hat{\mu}_i - \mu_i)^2].
$$

Podobnie,

$$
\begin{aligned}
\mathrm{E}[(Y_i^{\text{new}} - \hat{\mu}_i)^2] &= \mathrm{E}[(Y_i^{\text{new}} - \mu_i)^2] - 2\mathrm{E}[(Y_i^{\text{new}} - \mu_i)(\hat{\mu}_i - \mu_i)] + \mathrm{E}[(\hat{\mu}_i - \mu_i)^2] \\
&= \text{Var}[Y_i^{\text{new}}] - 2\text{Cov}[Y_i^{\text{new}}, \hat{\mu}_i] + \mathrm{E}[(\hat{\mu}_i - \mu_i)^2] \\
&= \text{Var}[Y_i] + \mathrm{E}[(\hat{\mu}_i - \mu_i)^2],
\end{aligned}
$$

gdzie ostatnia równość wynika z niezależności $Y_i^{\text{new}}$ i $\hat{\mu}_i$ oraz faktu, że $Y_i^{\text{new}}$ ma taki sam rozkład jak $Y_i$. Ostatecznie otrzymujemy

$$
\mathrm{E}[(Y_i^{\text{new}} - \hat{\mu}_i)^2] = \mathrm{E}[(Y_i - \mu_i)^2] + 2\text{Cov}[Y_i, \hat{\mu}_i].
$$

Średnio, pozorny błąd zaniża zatem prawdziwy błąd predykcji o karę kowariancyjną $2\text{Cov}[Y_i, \hat{\mu}_i]$. Intuicyjnie mówiąc, $\text{Cov}[Y_i, \hat{\mu}_i]$ mierzy, jak bardzo $Y_i$ wpływa na własną prognozę $\hat{\mu}_i$. Zapewnia to skuteczny sposób oceny MSEP na podstawie dostępnych danych (które zostały użyte do wytrenowania modelu), używając tożsamości

$$
\widehat{MSEP} = \widehat{MSRR} + \frac{2}{n} \sum_{i=1}^n \widehat{\text{Cov}}[Y_i, \hat{\mu}_i].
$$

Jeśli $Y_i \sim \mathcal{Noror}(\mu_i, \boldsymbol{\sigma}^2)$, a dopasowane wartości $\hat{\mu}_i$ są liniowe, wiemy, że

$$
\hat{\boldsymbol{\mu}} = \boldsymbol{H}\boldsymbol{Y}
$$

gdzie $\boldsymbol{H}$ jest znaną macierzą (4.12) odwzorowującą obserwacje na dopasowane wartości. Macierz kowariancji między $\hat{\boldsymbol{\mu}}$ a $\boldsymbol{Y}$ jest wtedy $\boldsymbol{\sigma}^2 \boldsymbol{H}$, dając

$$
\text{Cov}[Y_i, \hat{\mu}_i] = \boldsymbol{\sigma}^2 h_{ii}
$$

gdzie $h_{ii}$ jest $i$-tym diagonalnym elementem $\boldsymbol{H}$ mierzącym lewarowanie. W przypadku normalnym, kara kowariancyjna pokrywa się z estymatą $C_p$ Mallowa błędu predykcji:

$$
\widehat{MSEP} = \frac{1}{n} \sum_{i=1}^n (y_i - \hat{\mu}_i)^2 + \frac{2\boldsymbol{\sigma}^2}{n} \text{trace}(\boldsymbol{H}).
$$

**Uwaga 4.5.1** Dla modelu regresji liniowej, można wywnioskować z (4.12), że

$$
\text{trace}(\boldsymbol{H}) = \sum_{i=1}^n h_{ii} = \dim(\boldsymbol{\beta}) = p+1.
\quad\quad\text{(4.18)}
$$

To prowadzi również do ogólnej definicji liczby stopni swobody dla modelu regresji zbudowanego dla normalnych odpowiedzi $Y_i$. Zapewnia to aktuariuszowi odpowiednią miarę złożoności modelu. Jeśli równoważna liczba stopni swobody jest większa, model jest bardziej elastyczny i może stać się zbyt bliski rzeczywistym danym, co wymaga większych kar kowariancyjnych dla uczciwego porównania.

#### 4.5.5.3 Kary Kowariancyjne w GLM

Rozważmy teraz odpowiedzi $Y_i$ podlegające rozkładowi ED. Pozorny błąd jest mierzony przez dewiancję. Niech

$$
d(y, \hat{\theta}) = 2(y\hat{\theta}_y - a(\hat{\theta}_y)) - (y\hat{\theta} - a(\hat{\theta}))
\quad\quad\text{(4.19)}
$$

oznacza wkład odpowiedzi $y$ do dewiancji, gdzie $\theta_y$ jest wartością parametru kanonicznego, taka że tożsamość

$$
a'(\theta_y) = y
$$

zachodzi (tak jak musi być w modelu pełnym). Postępujmy teraz jak w przypadku normalnym. Biorąc pod uwagę nową obserwację $Y_{\text{new}}$ rozłożoną jak $Y_i$ i niezależną od obserwacji $Y_1, \dots, Y_n$ produkujących $\hat{\theta}_1, \dots, \hat{\theta}_n$, otrzymujemy

$$
\begin{aligned}
\mathrm{E}[d(Y_{\text{new}}, \hat{\theta}_i)] - \mathrm{E}[d(Y_i, \hat{\theta}_i)] &= 2(\mathrm{E}[Y_i^{\text{new}}\hat{\theta}_{Y_i^{\text{new}}} - a(\hat{\theta}_{Y_i^{\text{new}}})] - \mathrm{E}[Y_i^{\text{new}}\hat{\theta}_i - a(\hat{\theta}_i)] \\
& -\mathrm{E}[Y_i\theta_{Y_i} - a(\theta_{Y_i})] + \mathrm{E}[Y_i\hat{\theta}_i - a(\hat{\theta}_i)]) \\
&= 2(\mathrm{E}[Y_i\hat{\theta}_i] - \mathrm{E}[Y_i^{\text{new}}\hat{\theta}_i]) \\
&= 2(\mathrm{E}[Y_i\hat{\theta}_i] - \mathrm{E}[Y_i]\mathrm{E}[\hat{\theta}_i]) \\
&= 2\text{Cov}[Y_i, \hat{\theta}_i].
\end{aligned}
$$

W GLM, kara kowariancyjna wynosi $2\text{Cov}[Y_i, \hat{\theta}_i]$, gdzie $\hat{\theta}_i = \hat{\theta}(\boldsymbol{x}_i)$ jest oszacowanym parametrem kanonicznym dla obserwacji $i$. Średnia predykcyjna dewiancja, to jest dewiancja obliczona na nowych, poza próbą obserwacjach, może być zatem oszacowana na podstawie sumy

$$
\frac{1}{n} D(\boldsymbol{y}, \boldsymbol{\hat{\mu}}) + \frac{2}{n} \sum_{i=1}^n \text{Cov}[Y_i, \hat{\theta}_i].
$$

Załóżmy, że aktuariusz określa kanoniczną funkcję łączącą. W badaniach ubezpieczeniowych, oznacza to zasadniczo, że rozważane są rozkłady odpowiedzi Poissona lub dwumianowe (więc możemy bezpiecznie założyć $\phi=1$). Teraz, jeśli estymaty zostały uzyskane metodą największej wiarygodności i $\dim(\boldsymbol{\beta}) = p+1$, to następująca aproksymacja jest wystarczająco dokładna do celów praktycznych:

$$
\frac{2}{n} \sum_{i=1}^n \text{Cov}[Y_i, \hat{\theta}_i] = \frac{2}{n} \sum_{i=1}^n \mathrm{E}[(Y_i - \mu_i)(\boldsymbol{x}_i^\top \hat{\boldsymbol{\beta}} - \boldsymbol{x}_i^\top \boldsymbol{\beta})] \approx \frac{2(p+1)}{n}.
$$

Ta aproksymacja jest uzyskiwana z właściwości estymatora największej wiarygodności $\hat{\boldsymbol{\beta}}$ w rodzinie ED dla dużych prób. Po więcej szczegółów odsyłamy do Efrona (1986). Średnia predykcyjna dewiancja jest wtedy przybliżona przez

$$
\frac{1}{n} D(\boldsymbol{y}, \boldsymbol{\hat{\mu}}) + \frac{2(p+1)}{n},
$$

pod warunkiem, że $n$ jest wystarczająco duże.

#### 4.5.5.4 Kary Kowariancyjne i AIC

Rozważmy teraz odpowiedzi Poissona lub dwumianowe z łączem kanonicznym i oznaczmy przez $p_\theta(\cdot)$ odpowiadającą funkcję masy prawdopodobieństwa związaną z parametrem kanonicznym $\theta$. Błąd predykcji można wtedy obliczyć jako

$$
\begin{aligned}
\frac{1}{n}D(\boldsymbol{y}, \boldsymbol{\hat{\mu}}) + \frac{2}{n}\sum_{i=1}^n \text{Cov}[Y_i, \hat{\theta}_i] &\approx \frac{1}{n}D(\boldsymbol{y}, \boldsymbol{\hat{\mu}}) + \frac{2(p+1)}{n} \\
&= -\frac{2}{n}\left(\sum_{i=1}^n \ln p_{\hat{\theta}_i}(y_i) - (p+1)\right) + \text{stała} \\
&= \frac{\text{AIC}}{n} + \text{stała}
\end{aligned}
$$

obejmując kryterium informacyjne Akaikego AIC oparte na ukaranej największej wiarygodności (zauważ, że założyliśmy $\phi=1$, więc $p+1$ liczy wszystkie parametry zaangażowane w model regresji). Porównywanie rozważanych GLM na podstawie wartości AIC sprowadza się zatem do uwzględnienia optymizmu w dewiancji obliczonej na zbiorze treningowym.

## 4.6 Estymacja Parametru Dyspersji

Niektóre rozkłady z rodziny ED zawierają nieznany parametr dyspersji $\phi$. Dzieje się tak na przykład w przypadku rozkładów Gamma lub Odwrotnego Gaussa. Równania wiarygodności dla $\boldsymbol{\beta}$ nie zawierają parametru dyspersji $\phi$, więc nie jest konieczne szacowanie $\phi$, aby uzyskać $\boldsymbol{\hat{\beta}}$. Jednakże, oszacowany parametr dyspersji jest potrzebny do przeprowadzenia wnioskowania na temat współczynników regresji. Chociaż $\phi$ w zasadzie może być oszacowane metodą największej wiarygodności, częściej stosuje się alternatywny estymator wyprowadzony z metody momentów, który jest zdefiniowany w następujący sposób.

Gdy algorytm IRLS wygeneruje $\boldsymbol{\hat{\beta}}$, parametr dyspersji $\phi$ można oszacować, dzieląc statystykę dobroci dopasowania Pearsona

$$
X^2 = \sum_{i=1}^n \frac{(y_i - \hat{\mu}_i)^2}{V(\hat{\mu}_i)/v_i}
$$

przez resztowe stopnie swobody modelu, to jest,

$$
\hat{\phi} = \frac{1}{n-p-1} \sum_{i=1}^n \frac{v_i(y_i - \hat{\mu}_i)^2}{V(\hat{\mu}_i)} = \frac{X^2}{n-p-1}.
$$

Estymator ten wykorzystuje fakt, że $X^2$ Pearsona ma asymptotycznie rozkład Chi-kwadrat z $n-p-1$ stopniami swobody. Jest on znacznie prostszy do obliczenia i w niektórych przypadkach oferuje większą stabilność numeryczną niż estymata największej wiarygodności.

Dewiancja dostarcza alternatywnego estymatora dla $\phi$. Biorąc pod uwagę (4.16), widzimy, że średnia $D$ jest w przybliżeniu równa $n-p-1$. To sugeruje użycie estymaty dla dużych prób

$$
\hat{\phi} = \frac{D}{n-p-1}.
$$

## 4.7 Rozkład z próby oszacowanych współczynników regresji

### 4.7.1 Rozkład z próby

Rozkład z próby parametrów modelu należy rozumieć w następujący sposób. Estymatory są wynikami losowego eksperymentu: zbierania danych i analizowania ich za pomocą GLM. Gdyby użyto innego zestawu danych, o tej samej wielkości i wszystkich tych samych podstawowych cechach, ale z innymi wynikami, wynikowe oszacowane współczynniki byłyby inne. Zakres tych różnic zależy od zmienności obserwowanego zjawiska. Rozkład z próby opisuje zachowanie oszacowań uzyskanych z różnych próbek obserwacji, w warunkach powtarzanego próbkowania.

Rozkład z próby można w przybliżeniu wyznaczyć metodą bootstrap (parametryczną lub nieparametryczną), jak wyjaśniono w Rozdziale 3. Niemniej jednak metoda ta jest obliczeniowo intensywna. W przypadku GLM istnieje alternatywne podejście oparte na asymptotycznych właściwościach estymatorów największej wiarygodności. Rzeczywiście, z Rozdziału 3 wiemy, że estymatory uzyskane metodą największej wiarygodności są w przybliżeniu normalnie rozłożone, gdy wielkość próby jest duża. Ta właściwość jest nadal ważna dla wektora $\beta$ oszacowanych współczynników regresji. Biorąc pod uwagę, że w dopasowaniu GLM zaangażowanych jest kilka parametrów $\beta_0, \beta_1, \ldots, \beta_p$, musimy przywołać definicję wielowymiarowego rozszerzenia rozkładu normalnego.

### 4.7.2 Powrót do modelu regresji normalnej liniowej

Przypomnijmy sobie definicję wielowymiarowej wersji rozkładu normalnego. Niech $\boldsymbol{\Sigma}$ będzie macierzą $n \times n$ dodatnio określoną, a $\boldsymbol{\mu}$ wektorem rzeczywistym. Wówczas wektor losowy $\boldsymbol{Z} = (Z_1, Z_2, \ldots, Z_n)^T$ podlega wielowymiarowemu rozkładowi normalnemu o parametrach $(\boldsymbol{\mu}, \boldsymbol{\Sigma})$, co odtąd oznaczamy jako $\boldsymbol{Z} \sim \mathcal{Nor}(\boldsymbol{\mu}, \boldsymbol{\Sigma})$, jeśli jego funkcja gęstości prawdopodobieństwa ma postać

$f_Z(z) = \frac{1}{\sqrt{(2\pi)^n |\boldsymbol{\Sigma}|}} \exp \left( -\frac{1}{2} (z - \boldsymbol{\mu})^T \boldsymbol{\Sigma}^{-1} (z - \boldsymbol{\mu}) \right), \quad z \in (-\infty, \infty)^n, \quad (4.20)$

gdzie $|\boldsymbol{\Sigma}|$ oznacza wyznacznik macierzy $\boldsymbol{\Sigma}$. Tutaj $\boldsymbol{\mu}$ i $\boldsymbol{\Sigma}$ są średnią wektorową i macierzą kowariancji wielowymiarowego rozkładu normalnego $\mathcal{Nor}(\boldsymbol{\mu}, \boldsymbol{\Sigma})$.
Z Rozdziału 2 wiemy, że suma $n$ niezależnych, kwadratowych zmiennych losowych o rozkładzie $\mathcal{Nor}(0, 1)$ podlega rozkładowi chi-kwadrat $\chi_n^2$. Wynik ten rozszerza się na niezależne sumy, w których $\boldsymbol{Z} \sim \mathcal{Nor}(\boldsymbol{\mu}, \boldsymbol{\Sigma})$, gdzie $\boldsymbol{\Sigma}$ ma odwrotność $\boldsymbol{\Sigma}^{-1}$

$$(\boldsymbol{Z} - \boldsymbol{\mu})^T \boldsymbol{\Sigma}^{-1} (\boldsymbol{Z} - \boldsymbol{\mu}) \sim \chi_n^2.$$

Dogodna charakteryzacja wielowymiarowego rozkładu normalnego jest następująca:
$\boldsymbol{Z} \sim \mathcal{Nor}(\boldsymbol{\mu}, \boldsymbol{\Sigma})$, wtedy i tylko wtedy, gdy każda zmienna losowa postaci $\sum_{i=1}^n \alpha_i Z_i$ dla stałych rzeczywistych $\alpha_1, \ldots, \alpha_n$ podlega jednowymiarowemu rozkładowi normalnemu. Mówiąc bardziej ogólnie, wielowymiarowy rozkład normalny ma również następującą użyteczną właściwość stabilności: Niech $\boldsymbol{C}$ będzie daną macierzą $n \times n$ o rzeczywistych wpisach, a $\boldsymbol{b}$ niech będzie $n$-wymiarowym wektorem rzeczywistym. Wtedy,

$\boldsymbol{Z} \sim \mathcal{Nor}(\boldsymbol{\mu}, \boldsymbol{\Sigma}) \Rightarrow \boldsymbol{CZ} + \boldsymbol{b} \sim \mathcal{Nor}(\boldsymbol{C}\boldsymbol{\mu} + \boldsymbol{b}, \boldsymbol{C}\boldsymbol{\Sigma} \boldsymbol{C}^T)$.

Stosując tę właściwość do $\boldsymbol{\hat{\beta}}$ podanego w (4.11), z

$$\boldsymbol{C} = (\boldsymbol{X}^T \boldsymbol{W} \boldsymbol{X})^{-1} \boldsymbol{X}^T \boldsymbol{W}$$
$$\boldsymbol{b} = \boldsymbol{0}$$
$$\boldsymbol{Z}^T = (Y_1, Y_2, \ldots, Y_n)$$
$$\boldsymbol{\mu} = \boldsymbol{X}\boldsymbol{\beta}$$
$$\boldsymbol{\Sigma} = \sigma^2 \boldsymbol{W}^{-1}$$

otrzymujemy

$$\boldsymbol{C}\boldsymbol{\mu} + \boldsymbol{b} = \boldsymbol{\beta} \text{ oraz } \boldsymbol{C}\boldsymbol{\Sigma} \boldsymbol{C}^T = \sigma^2 (\boldsymbol{X}^T \boldsymbol{W} \boldsymbol{X})^{-1}$$

tak, że

$$\boldsymbol{\hat{\beta}} \sim \mathcal{Nor}(\boldsymbol{\beta}, \sigma^2 (\boldsymbol{X}^T \boldsymbol{W} \boldsymbol{X})^{-1}).$$

To daje dokładny rozkład z próby $\boldsymbol{\hat{\beta}}$ w modelu regresji normalnej liniowej.

### 4.7.3 Rozkład z próby w GLM

Rozważmy teraz inne GLM. Ponieważ $\boldsymbol{\hat{\beta}}$ zostało uzyskane metodą największej wiarygodności, jest ono w przybliżeniu normalnie rozłożone, pod warunkiem, że $n$ jest wystarczająco duże. Dokładniej, błąd estymacji spełnia

$$\boldsymbol{\hat{\beta}} - \boldsymbol{\beta} \approx \mathcal{Nor}(0, \hat{\boldsymbol{\Sigma}}(\boldsymbol{\hat{\beta}})).$$

Ponieważ algorytm IRLS używany do dopasowywania GLM (jak wyjaśniono w sekcji 4.4) wielokrotnie dopasowuje normalny model regresji do odpowiedzi roboczych, z różnymi wagami, macierz wariancji-kowariancji wynikającego $\hat{\beta}$ można uzyskać z ostatniego kroku tej procedury, używając wzorów ważnych w przypadku normalnym. Dokładniej, oszacowana macierz wariancji-kowariancji $\boldsymbol{\hat{\beta}}$ dla dużej próby jest dana przez

$$\hat{\boldsymbol{\Sigma}}(\boldsymbol{\hat{\beta}}) = \hat{\phi} (X^T \tilde{W}^{(r_{\text{stop}})} X)^{-1}$$

gdzie $\tilde{W}^{(r_{\text{stop}})}$ jest pobierane z ostatniego kroku algorytmu IRLS produkującego $\boldsymbol{\hat{\beta}}$ (tak, że nie obejmuje parametru dyspersji, a czynnik $\phi$ musi być określony zgodnie z wzorem (4.3)). Błędy standardowe oszacowań parametrów są pierwiastkami kwadratowymi z elementów diagonalnych macierzy wariancji-kowariancji.

### 4.7.4 Przedziały ufności Walda

Podejście Walda opiera się na normalnej aproksymacji rozkładu $\boldsymbol{\hat{\beta}}$. Błąd standardowy jest pierwiastkiem kwadratowym z oszacowanej wariancji $\text{Var}[\hat{\beta}_j]$. Tę wielkość należy rozumieć w następujący sposób. Gdyby pobrano nowe próbki, odchylenie standardowe wynikowych oszacowań $\hat{\beta}_j$ byłoby w przybliżeniu oszacowanym błędem standardowym. Dlatego mały błąd standardowy wskazuje, że $\hat{\beta}_j$ powinien być bliski prawdziwemu $\beta_j$, podczas gdy duży błąd standardowy sugeruje, że szeroki zakres oszacowań można by osiągnąć przez losowość.

Przedział ufności można uznać za rozsądny zakres oszacowań dla współczynnika interesującego. Przedziały ufności Walda opierają się na rozkładzie $\hat{\beta}_j$ dla dużej próby, to jest,

$$\hat{\beta}_j - \beta_j \approx \mathcal{Nor}(0, \hat{\sigma}_{jj}^2)$$

gdzie $\hat{\sigma}_{jj}$ jest elementem $(j, j)$ macierzy $\hat{\boldsymbol{\Sigma}}(\boldsymbol{\hat{\beta}})$, to jest oszacowaną wariancją $\hat{\beta}_j$. Przybliżony przedział ufności na poziomie $1 - \alpha$ jest następnie uzyskiwany w następujący sposób:

$$\begin{align*}
1 - \alpha &\approx \text{P} \left[ -z_{\alpha/2} \sqrt{\hat{\sigma}_{jj}} \leq \hat{\beta}_j - \beta_j \leq z_{\alpha/2} \sqrt{\hat{\sigma}_{jj}} \right] \\
&= \text{P} \left[ \hat{\beta}_j - z_{\alpha/2} \sqrt{\hat{\sigma}_{jj}} \leq \beta_j \leq \hat{\beta}_j + z_{\alpha/2} \sqrt{\hat{\sigma}_{jj}} \right]
\end{align*}$$

tak, że przedział

$$\text{IC}_{1-\alpha}(\beta_j) = \left[ \hat{\beta}_j \pm z_{\alpha/2} \sqrt{\hat{\sigma}_{jj}} \right]$$

oczekuje się, że będzie zawierał prawdziwą wartość parametru z prawdopodobieństwem w przybliżeniu równym $1-\alpha$. Tutaj, prawdopodobieństwo $1-\alpha$ jest (przybliżonym) poziomem ufności, lub pokryciem prawdopodobieństwa odpowiadającego przedziału ufności.

Ponieważ $\boldsymbol{\hat{\beta}}$ jest w przybliżeniu normalny, oszacowana ocena $\hat{s}_i = \boldsymbol{x}_i^T \boldsymbol{\hat{\beta}}$ również jest. Obciążenie jest w przybliżeniu równe 0, a wariancja oszacowanej oceny $\hat{s}_i$ jest dana przez

$$\text{Var}[\hat{s}_i] = \boldsymbol{x}_i^T \hat{\boldsymbol{\Sigma}}(\boldsymbol{\hat{\beta}}) \boldsymbol{x}_i.$$

Przedział ufności na przybliżonym poziomie ufności $1-\alpha$ dla prawdziwej oceny $s_i$ jest następnie dany przez

$$\left[\boldsymbol{x}_i^T \boldsymbol{\hat{\beta}} \pm z_{\alpha/2} \sqrt{\boldsymbol{x}_i^T \hat{\boldsymbol{\Sigma}}(\boldsymbol{\hat{\beta}}) \boldsymbol{x}_i}\right].$$

### 4.7.5 Współczynnik inflacji wariancji

Współliniowość odnosi się do sytuacji, w której dwie lub więcej cech jest silnie predykcyjnych względem trzeciej. Jednak ta ostatnia może nie być silnie skorelowana z żadną z poprzednich, więc zjawisko to nie pojawia się w macierzy korelacji lub macierzy V Craméra w przypadku cech o różnych formatach. Dzieje się tak, ponieważ taka macierz uwzględnia tylko pary cech, a nie większe podzbiory dostępnych cech. W konsekwencji współliniowość jest znacznie trudniejsza do wykrycia.

Gdy istnieją silne zależności liniowe między cechami w analizie regresji, dokładność estymacji dla $\beta_j$ maleje w porównaniu z przypadkiem, w którym cechy są niezależne. Głównym powodem jest to, że macierz $\boldsymbol{X}^T \boldsymbol{W} \boldsymbol{X}$ zaangażowana w każdy krok algorytmu IRLS jest źle uwarunkowana, a zatem prawie nieodwracalna.
Użyteczną statystyką do wykrywania współliniowości jest współczynnik inflacji wariancji (VIF). VIF dla dowolnej cechy mierzy wzrost kwadratu błędu standardowego dla współczynnika regresji z powodu obecności współliniowości w danych. Niech $\rho_j$ oznacza współczynnik determinacji z regresji $j$-tej cechy na pozostałych $p-1$ cechach. Jest on określany przez uruchomienie modelu liniowego dla każdej z pozostałych cech przy użyciu wszystkich innych cech jako danych wejściowych i pomiar jego dokładności predykcyjnej. Oznacza to, że dla każdej cechy $x_{ij}$ z kolei próbujemy zidentyfikować zależność liniową

$$\gamma_0^{(j)} + \sum\limits_{k \neq j} \gamma_k^{(j)} x_{ik}$$

która najlepiej przewiduje $x_{ij}$ z $x_{ik}$, $k \neq j$. Zatem,

$$\hat{\gamma}_0^{(j)} + \sum_{k \neq j} \hat{\gamma}_k^{(j)} x_{ik}$$

może zastąpić $x_{ij}$ w wyniku liniowym. W modelu regresji normalnej liniowej można wykazać, że

$$\text{Var}[\hat{\beta}_j] = \frac{\sigma^2}{(1 - \rho_j^2) \sum_{i=1}^n (x_{ij} - \bar{x}_j)^2}.$$

To daje

$$\widehat{\text{Var}}[\hat{\beta}_j] = \frac{\hat{\sigma}^2}{(n-1) s_j^2} \times \frac{1}{1 - \hat{\rho}_j^2},$$

gdzie $\hat{\sigma}^2$ jest estymowaną wariancją $\sigma^2$, $s_j^2$ jest wariancją próby $x_{1j}, x_{2j}, \ldots, x_{nj}$ oraz $\frac{1}{1 - \hat{\rho}_j^2}$ jest współczynnikiem inflacji wariancji dla estymatora $\hat{\beta}_j$. Ta formuła pokazuje, które składniki wpływają na dokładność estymacji dla współczynników regresji:

- im mniejszy model wariancji, tym mniejsza wariancja $\hat{\beta}_j$, a tym samym dokładniejsza estymacja.
- im mniejsza zależność liniowa między $j$-tą cechą a pozostałymi $p-1$ (mierzona przez $\rho_j^2$), tym mniejsza wariancja $\hat{\beta}_j$.

Wariancja $\hat{\beta}_j$ jest zminimalizowana dla $\rho_j^2 = 0$, to znaczy, gdy cechy są nieskorelowane. Gdy niektóre cechy są silnie skorelowane, estymatory stają się bardzo niedokładne. W skrajnej sytuacji, gdy $\rho_j^2 \rightarrow 1$, wariancja $\hat{\beta}_j$ eksploduje w kierunku nieskończoności (odzwierciedlając doskonałą współliniowość).
- im większa zmienność $j$-tej cechy wokół jej średniej, tym mniejsza wariancja $\hat{\beta}_j$.

Współczynnik inflacji wariancji zdefiniowany jako

$$\text{VIF} = \frac{1}{1 - \hat{\rho}_j^2}$$

określa ilościowo wzrost wariancji $\hat{\beta}_j$ z powodu zależności liniowej $j$-tej cechy od pozostałych $p-1$. VIF jest najprostszym i najbardziej bezpośrednim wskaźnikiem szkód wyrządzonych przez współliniowość. W szczególności pierwiastek kwadratowy z VIF wskazuje, o ile większy jest przedział ufności dla $\hat{\beta}_j$ w porównaniu z podobnymi nieskorelowanymi danymi (takie dane mogą być czysto koncepcyjne). Silne zależności liniowe odpowiadają cechom o dużym VIF. Jako punkt odniesienia, poważny problem współliniowości istnieje, gdy VIF jest większy niż 10. VIF nie stosuje się do zestawów współczynników regresji (odpowiadających różnym poziomom cech kategorialnych, na przykład), ale w tym przypadku dostępne są uogólnione VIF, lub GVIF.

Pomimo ich oczywistego znaczenia w badaniach GLM, obliczanie miar VIF wydaje się być intensywne obliczeniowo w wielu analizach obejmujących wiele cech, biorąc pod uwagę obecną moc obliczeniową. To nieco ogranicza praktyczne zastosowanie tej koncepcji.

### 4.7.6 Testowanie hipotez dotyczących parametrów

#### 4.7.6.1 Hipotezy dotyczące pojedynczego współczynnika regresji

Aby przetestować, czy założenie $\beta_j = b$ jest rozsądne, dla pewnej stałej $b$, możemy obliczyć statystykę Walda

$$z = \frac{\hat{\beta}_j - b}{\sqrt{\widehat{\text{Var}}[\hat{\beta}_j]}}.$$

Możemy następnie porównać $z$ ze standardowymi kwantylami normalnymi, aby zdecydować, czy założenie $\beta_j = b$ jest odrzucone, czy nie. Dokładniej, założenie $\beta_j = b$ jest odrzucane, gdy $\hat{\beta}_j$ jest zbyt daleko od przyjętej wartości $b$, to jest, gdy $|z|$ jest duże. Pracując na poziomie ufności $\alpha$, oznacza to, że $\beta_j = b$ jest odrzucane, jeśli $|z|$ przekracza kwantyl $\Phi^{-1}(1-\alpha/2)$ rozkładu $\mathcal{Nor}(0, 1)$ na poziomie prawdopodobieństwa $\alpha$.

Przyjęcie tej procedury oznacza, że wybrany $\alpha$ kwantyfikuje ryzyko odrzucenia założenia $\beta_j = b$, podczas gdy w rzeczywistości jest ono prawdziwe (nazywane błędem typu 1 w statystyce). Zgodnie z tym założeniem, statystyka testowa $z$ jest w przybliżeniu rozłożona $\mathcal{Nor}(0, 1)$, tak że istnieje prawdopodobieństwo $\alpha$, że $|z|$ przekroczy $\Phi^{-1}(1-\alpha/2)$, prowadząc do odrzucenia. Takie przypadki odpowiadają sytuacjom, w których założenie nie powinno być odrzucane. Stąd $\alpha$ reprezentuje poziom bezpieczeństwa wybrany przez aktuariusza przy odrzucaniu rozważanego założenia. Poziom $\alpha=5\%$ jest rutynowo stosowany w praktyce. Prowadzi to do 1 błędnego wniosku na 20 testów prowadzących do odrzucenia, średnio (pod warunkiem, że testy są przeprowadzane niezależnie). W zależności od wagi wniosku wyciągniętego z analizy, aktuariusz może zmniejszyć ryzyko błędu (do 1%, na przykład), lub być mniej konserwatywny (zwiększając $\alpha$ do 10%, powiedzmy).

Oprogramowanie statystyczne, w tym R, generalnie zgłasza wyniki procedur testowych w postaci p-wartości, aby uniknąć proszenia użytkowników o podanie poziomu ufności $\alpha$, który ma być zastosowany. Na podstawie obliczonej p-wartości, aktuariusz następnie odrzuca założenie, gdy spada ono poniżej wybranego $\alpha$, i zachowuje je jako założenie robocze w przeciwnym razie. Formalnie jednak aktuariusz nigdy nie może zaakceptować założenia (stąd subtelna różnica między akceptacją a nieodrzuceniem). Dzieje się tak dlatego, że inne źródło błędu, zwane błędem typu 2 w statystyce, polegające na akceptowaniu założenia, gdy w rzeczywistości jest ono fałszywe, na ogół nie jest kontrolowane przez procedurę testową.

Zauważ, że standardowe testy dla hipotezy zerowej $H_0: \beta_j = 0$ są automatycznie dostarczane przez oprogramowanie statystyczne. Gdy cechy kategorialne są zaangażowane, $\beta_j = 0$ oznacza, że odpowiedni poziom jest identyczny z poziomem bazowym lub referencyjnym dla tej cechy (która jest uwzględniona w wyrazie wolnym $\beta_0$). Nieodrzucenie $H_0$ sugeruje, że poziom powinien zostać połączony z poziomem bazowym. Ale inne grupowania mogą być sensowne i mogą być wykryte przez testowanie hipotez postaci

$$H_0: \beta_j = \beta_k \Leftrightarrow \hat{\beta}_j - \boldsymbol{\hat{\beta}}_k = 0.$$

Odpowiednie statystyki testowe są uzyskiwane przez standaryzację $\hat{\beta}_j - \boldsymbol{\hat{\beta}}_k,$ odpowiednio, z rozkładem $\mathcal{Nor}(0, 1)$ pod hipotezą zerową $H_0$.

Gdy w ocenie uwzględniony jest wyraz wolny, procedura analizy GLM przetwarza zmienne ryzyka w odniesieniu do poziomu odniesienia (którego efekt odpowiada wyrazowi wolnemu). Przełączanie z jednego poziomu odniesienia na inny nie modyfikuje przewidywań modelu (pomimo różnych oszacowanych współczynników regresji uzyskanych, ponieważ poziomy są teraz w odniesieniu do innego poziomu bazowego). Jednak p-wartości się zmieniają, ponieważ kwestionują różnicę danego poziomu w porównaniu z poziomem bazowym: zmiana poziomu bazowego modyfikuje testowaną hipotezę. Dlatego ważne jest, aby sprawdzić, czy poziom bazowy ma znaczną ekspozycję i nie przyjmować domyślnego poziomu bazowego przypisanego przez oprogramowanie. To jest właśnie wybór dokonany we wszystkich przykładach w tej książce.

**Uwaga 4.7.1** Znaczenie każdej zmiennej jest wyrażone przez wielkość jej wkładu w ocenę. W GLM jest to zasadniczo odzwierciedlone przez wielkość powiązanego $\beta_j$ (zakładając, że wszystkie predyktory zostały znormalizowane). Istotność $j$-tej cechy można następnie przetestować za pomocą hipotezy zerowej $H_0: \beta_j=0$.
Innym sposobem postępowania, który rozszerza się na bardziej wyszukane podejścia do uczenia statystycznego, jest wybranie losowej frakcji $\pi$ oryginalnych danych i wykorzystanie ich do dopasowania GLM. Aktuariusz następnie używa dopasowanego modelu do przewidywania odpowiedzi w pozostałej $1-\pi$ części bazy danych i oblicza dokładność tej predykcji. Wartości $j$-tej cechy są następnie losowo permutowane, a odpowiedź jest ponownie przewidywana na tej podstawie. Chodzi o to, aby porównać dokładność predykcji z i bez permutacji: im ważniejsza cecha, tym większa różnica w tych dwóch miarach, permutacja $j$-tej cechy powodująca duży spadek dokładności predykcji.

#### 4.7.6.2 Hipotezy dotyczące kilku współczynników regresji

Oprócz prostych hipotez dotyczących pojedynczego parametru, można również rozważyć bardziej rozbudowane. Rozważmy model bazowy

$$\mathcal{M}_0: \boldsymbol{\beta} = \boldsymbol{\beta}_0 = (\beta_1, \beta_2, \ldots, \beta_q)^T$$

i model alternatywny

$$\mathcal{M}_1: \boldsymbol{\beta} = \boldsymbol{\beta}_1 = (\beta_1, \beta_2, \ldots, \beta_q, \beta_{q+1}, \ldots, \beta_p)^T$$

zawierający dodatkowe zmienne objaśniające. W ramach $\mathcal{M}_0$ cechy $x_{i, q+1}, \ldots, x_{i,p}$ nie mają wpływu na oczekiwane odpowiedzi. Oznacza to, że $\mathcal{M}_0$ odpowiada hipotezie zerowej

$$H_0: \beta_{q+1} = \ldots = \beta_p = 0$$

podczas gdy hipoteza alternatywna jest taka, że co najmniej jeden współczynnik regresji wśród $\beta_{q+1}, \ldots, \beta_p$ jest niezerowy.
Gdy GLM używa podzbioru cech większego modelu, mniejszy model mówi się, że jest zagnieżdżony w większym. W przypadku $\mathcal{M}_0$ zagnieżdżonego w $\mathcal{M}_1$. Dla GLM bez parametru dyspersji (tj. $\phi = 1$ jak w przypadkach dwumianowym i Poissona), statystyka testu ilorazu wiarygodności jest po prostu różnicą w dewiancjach dla zagnieżdżonych modeli $\mathcal{M}_0$ i $\mathcal{M}_1$. Oznaczając $D_0$ i $D_1$ odpowiednie dewiancje związane z $\mathcal{M}_0$ i $\mathcal{M}_1$, statystyka testowa jest

$$\Delta = D_0 - D_1 = 2(L(\boldsymbol{\hat{\beta}}_1) - L(\boldsymbol{\hat{\beta}}_0))$$

gdzie $\boldsymbol{\hat{\beta}}_0$ i $\boldsymbol{\hat{\beta}}_1$ są estymatorami największej wiarygodności dla $\boldsymbol{\beta}$ w ramach odpowiednio $\mathcal{M}_0$ i $\mathcal{M}_1$. Jeśli ograniczenia nałożone na model $\mathcal{M}_1$ reprezentowane przez $\mathcal{M}_0$ są prawdziwe, to jest, jeśli $H_0$ jest prawdziwe, to statystyka testowa $\Delta$ jest w przybliżeniu rozłożona chi-kwadrat, to jest, $\Delta \approx \chi_{p-q}^2$ pod warunkiem, że wielkość próby jest wystarczająco duża.
Musimy jednak pamiętać, że dodanie cech do modelu zawsze zmniejsza dewiancję (nawet jeśli dodana cecha nie jest związana z odpowiedzią), po prostu dlatego, że w większym modelu zaangażowanych jest więcej parametrów, który może tylko lepiej dopasować dane. Sensownym pytaniem jest zatem, czy dodatkowe cechy znacznie zmniejszają dewiancję, tj. czy nie były one związane z odpowiedzią. Przybliżony rozkład chi-kwadrat $\chi_{p-q}^2$ rządzący $D_0 - D_1$ definiuje próg, powyżej którego różnica w obserwowanych dewiancjach jest wystarczająco duża, aby uzasadnić włączenie dodatkowych cech. Formalnie, jeśli model $\mathcal{M}_1$ jest prawdziwym modelem, to będzie miał tendencję do posiadania znacznie wyższej wiarygodności w porównaniu z modelem $\mathcal{M}_0$, tak że różnica w log-wiarygodnościach będzie zbyt duża. Zatem $H_0$ jest odrzucane, jeśli $\Delta_{obs}$ przekracza statystykę testową $\Delta$ jest "za duża". W dużych próbach $H_0$ jest odrzucane na poziomie prawdopodobieństwa $1-\alpha$, gdzie $\chi_{p-q; 1-\alpha}^2$ oznacza kwantyl rozkładu $\chi_{p-q}^2$.

Dla GLM, w których występuje parametr dyspersji do oszacowania (jak w przypadkach normalnym, gamma i odwrotnym gaussowskim), możemy porównać zagnieżdżone modele za pomocą testu F, opartego na statystyce testowej

$$F = \frac{\frac{D_0-D_1}{p-q}}{\hat{\phi}}$$

obejmując odpowiednie przeskalowane dewiancje $\frac{D_0}{\hat{\phi}}$ i $\frac{D_1}{\hat{\phi}}$. Tutaj, oszacowany parametr dyspersji $\hat{\phi}$ jest pobierany z największego dopasowanego modelu do danych (który niekoniecznie jest modelem $\mathcal{M}_1$). Jeśli największy model ma $k+1$ współczynników regresji, to przy założeniu, że ograniczenia nałożone na model $\mathcal{M}_1$ przez model $\mathcal{M}_0$ są poprawne, $F$ w przybliżeniu podlega rozkładowi Fishera z $p-q$ i $n-k-1$ stopniami swobody.

### 4.7.7 Ilustracje numeryczne

#### 4.7.7.1 Graduacja rocznych prawdopodobieństw zgonu

Wróćmy do problemu graduacji rocznych prawdopodobieństw zgonu przy użyciu formuły (4.15). Przypomnijmy, że odpowiedzią jest liczba zgonów $D_x$ zarejestrowanych wśród $l_x$ osób w wieku $x$ w dniu 1 stycznia. Parametrem zainteresowania jest roczne prawdopodobieństwo zgonu $q_x$. Stosowany jest dwumianowy GLM z kanonicznym łączem, z oceną obejmującą wiek $x$ i jego kwadrat, to jest,

$$\text{logit}(q_x) = \beta_0 + \beta_1 x + \beta_2 x^2.$$

**Tabela 4.8** Wynik funkcji `glm` z R dla dopasowania kwadratowej formuły Perks do danych o śmiertelności wyświetlonych na Rys. 4.1. Liczba iteracji Fisher Scoring: 3

| Współczynnik | Oszacowanie | Bł. stand. | Wartość $z$ | $Pr(>\mid z \mid )$ | |
| --- | --- | --- | --- | --- | --- |
| Wyraz wolny | -3.340 | $4.808 \times 10^{-1}$ | -6.947 | $3.73 \times 10^{-12}$ | \*\*\* |
| Wiek x | -0.1038 | $1.213 \times 10^{-2}$ | -8.558 | $<2 \times 10^{-16}$ | \*\*\* |
| Wiek do kwadratu x² | $1.384 \times 10^{-3}$ | $7.597 \times 10^{-5}$ | 18.214 | $<2 \times 10^{-16}$ | \*\*\* |

Alternatywnie, model można dopasować do obserwowanych surowych prawdopodobieństw zgonu $\tilde{q}_x = D_x / l_x$, pod warunkiem, że wagi $l_x / \text{Var}[\tilde{q}_x]$ są używane jako waga. Oszacowania punktowe $\boldsymbol{\hat{\beta}}$ uzyskane za pomocą algorytmu IRLS zostały już podane wcześniej w tym rozdziale. Teraz możemy uzupełnić szczegółowe wyniki funkcji `glm` o oszacowania i p-wartości dla hipotez zerowych $H_0: \beta_j=0$. Wyniki są podsumowane w Tabeli 4.8.

Widzimy w drugiej kolumnie Tabeli 4.8 (zatytułowanej "Oszacowanie") oszacowania punktowe uzyskane za pomocą algorytmu IRLS. Odpowiednie błędy standardowe, oparte na właściwościach estymacji największej wiarygodności dla dużych prób, są podane w następnej kolumnie. Wartość $z$ jest statystyką Walda do testowania $\beta_j = 0$, otrzymaną jako stosunek wartości pojawiających się w dwóch poprzednich kolumnach. Odpowiednie p-wartości są wyświetlane dalej. Ostatnia kolumna w Tabeli 4.8 ilustruje wnioski wyciągnięte z testu statystycznego za pomocą prostego kodowania. Przypomnijmy, że im mniejsza p-wartość, tym większa statystyka testowa, a tym bardziej wątpliwe jest założenie $\beta_j = 0$. Wtedy prawdopodobne jest, że odpowiednia cecha powinna być zachowana w modelu (ponieważ jej wkład w ocenę GLM jest niezerowy). Jest to reprezentowane przez gwiazdki: trzy gwiazdki (\*\*\*) dla p-wartości mniejszych niż 0.1%, dwie gwiazdki (\*\*) dla p-wartości między 0.1 a 1%, i jedna gwiazdka (\*) dla p-wartości między 1 a 5%. Kropka jest używana dla p-wartości w szarej strefie 5-10%, gdzie aktuariusz jest niepewny co do istotności odpowiedniej cechy. Nic nie pojawia się w ostatniej kolumnie dla p-wartości większych niż 10%, które prawidłowo prowadzą do nieodrzucenia.

Dla właściwej interpretacji ważne jest, aby zdać sobie sprawę, że te gwiazdki odnoszą się do statystycznej istotności, a nie do faktycznego wpływu cechy na oczekiwaną odpowiedź. Możemy mieć większy $\hat{\beta}_j$ związany z większą p-wartością (pomimo pozostałych istotnych). $j$-ta cecha następnie indukuje bardziej premium różnice, nawet jeśli jest mniej istotna w R. Zatem ranking cech oparty na p-wartościach nie jest zatem sensowny bez uwzględnienia oszacowań.
Widzimy w Tabeli 4.8, że wszystkie p-wartości dla testu hipotezy zerowej $H_0: \beta_j = 0$ są mniejsze niż $10^{-6}$, więc istnieją silne dowody przeciwko $H_0: \beta_j=0$ dla każdego parametru (co jest reprezentowane przez trzy gwiazdki w ostatniej kolumnie, jak wyjaśniono powyżej). Dewiancja zerowa wynosi 32.760.878 na 35 stopniach swobody, podczas gdy dewiancja resztowa wynosi 42.942 na 33 stopniach swobody. Odpowiednie AIC wynosi 354.33.

Graficznie, dopasowanie uzyskane za pomocą kwadratowej formuły Perks było wyraźnie lepsze od tego uzyskanego z liniowym odpowiednikiem. Jest to już widoczne z małej p-wartości uzyskanej dla współczynnika regresji związanego ze składnikiem wieku do kwadratu raportowanym w Tabeli 4.8. Formalny test można przeprowadzić przy użyciu funkcji anova, która zwraca analizę tabeli dewiancji. Różnica w odpowiednich dewiancjach modelu liniowego i kwadratowego Perks wynosi 325.62 z 1 stopniem swobody. Wynikowa p-wartość jest mniejsza niż $10^{-6}$, więc hipoteza zerowa, że specyfikacja liniowa oceny jest równoważna kwadratowej, jest wyraźnie odrzucona. Widzimy zatem, że kwadratowa formuła (4.15) znacznie przewyższa swój liniowy odpowiednik (uzyskany przez umieszczenie $\beta_2=0$).

Macierz wariancji-kowariancji $3 \times 3$ $\boldsymbol{\hat{\Sigma}}_{\boldsymbol{\hat{\beta}}}$ z $\boldsymbol{\hat{\beta}}$ można uzyskać za pomocą funkcji R `vcov` dając

$$\hat{\Sigma}_{\boldsymbol{\hat{\beta}}} = \begin{pmatrix} 2.311966 \times 10^{-1} & -5.823715 \times 10^{-3} & 3.631568 \times 10^{-5} \\ -5.823715 \times 10^{-3} & 1.471464 \times 10^{-4} & -9.202314 \times 10^{-7} \\ 3.631568 \times 10^{-5} & -9.202314 \times 10^{-7} & 5.771165 \times 10^{-9} \end{pmatrix}.$$

Ocena $\hat{\beta}_0 + \hat{\beta}_1 x + \hat{\beta}_2 x^2$ jest normalnie rozłożona, ze średnią

$$E[\hat{\beta}_0 + \hat{\beta}_1 x + \hat{\beta}_2 x^2] = \beta_0 + \beta_1 x + \beta_2 x^2$$

i wariancją

$$\text{Var}[\hat{\beta}_0 + \hat{\beta}_1 x + \hat{\beta}_2 x^2] = \text{Var}[(1, x, x^2)\boldsymbol{\hat{\beta}}] = (1, x, x^2) \boldsymbol{\hat{\Sigma}_{\hat{\beta}}} (1, x, x^2)^T.$$

Pozwala to nam uzyskać przedziały ufności dla oceny, a stąd dla prawdopodobieństw zgonu $q_x$.

#### 4.7.7.2 Rezerwacja strat

Przejdźmy teraz do modelowania średnich płatności zgodnie z rokiem wypadku AY i rokiem rozwoju DY, rozważanych wcześniej w tym rozdziale. Współczynniki regresji wyświetlone na Rys. 4.6 sugerują ustrukturyzowanie efektów AY i DY w celu zmniejszenia liczby parametrów i zwiększenia stabilności modelu. Biorąc pod uwagę współczynniki regresji związane z DY, widzimy, że wydają się być wyraźnie liniowe do DY = 7, a następnie łamią, wykazując liniowy trend poza nim. Sugeruje to, aby przedstawić efekt roku rozwoju dwiema cechami DY i I[DY > 7] wchodzącymi w ocenę w sposób liniowy. Biorąc pod uwagę tę specyfikację, różnica w dewiancji wynosi 0.030928 z 8 stopniami swobody. Odpowiednia wartość F wynosi 0.8603, dając p-wartość 55.63%. Nie ma zatem istotnej różnicy między modelami z i bez ustrukturyzowanych efektów DY.
Oszacowane parametry regresji $\hat{\beta}_j$ związane z AY pozostają w dużej mierze niezmienione przez efekty DY. Jest to widoczne, gdy kontynuujemy strukturyzację współczynników regresji $\beta_j$ związanych z 11 latami wypadków. Widzimy, że do AY = 6, są prawie stałe, podczas gdy wykazują liniowy trend poza nim. Sugeruje to zastąpienie nieustrukturyzowanego efektu roku wypadkowego nową cechą AY $\times$ I[AY > 6] wchodzącą w ocenę w sposób liniowy, pozwalając na wyraz wolny. To dalsze uproszczenie nie pogarsza znacząco dopasowania. Dokładniej, w porównaniu z modelem początkowym, różnica w dewiancjach jest równa 0.068996 z 17 stopniami swobody. Wartość F wynosi 0.9032, a odpowiadająca jej p-wartość wynosi 57.85%, więc nie wykryto istotnej różnicy. Porównując z poprzednim modelem, otrzymujemy różnicę w dewiancjach 0.038068, 9 stopni swobody, wartość F 0.8155 i p-wartość 60.43%, co potwierdza poprzednie wyniki. Ponieważ model ustrukturyzowany testowany w stosunku do modelu początkowego przy użyciu analizy tabeli dewiancji i w stosunku do modelu pośredniego nie wykazuje istotnej różnicy (wykrytej testem F), preferowany jest prosty model ustrukturyzowany.
Dopasowanie modelu jest podsumowane w Tabeli 4.9. Ostateczny model obejmuje przekształcone cechy AY $\times$ I[AY > 6], DY i I[DY > 7] wynikające ze strukturyzacji efektów uzyskanych przez traktowanie AY i DY jako cech kategorialnych. Parametr dyspersji dla rodziny odwrotnej-gaussowskiej jest oszacowany na $\hat{\phi} = 0.0051574$. Dewiancja zerowa wynosi 5.48273 na 65 stopniach swobody, podczas gdy dewiancja resztowa jest równa 0.25771 na 62 stopniach swobody. Odpowiednie AIC wynosi 712,569.

**Tabela 4.9** Wynik funkcji `glm` z R dla dopasowania danych rezerwacji strat wyświetlonych w Tabeli 4.6 z ustrukturyzowanymi efektami AY i DY. Liczba iteracji Fisher scoring: 7

| Współczynnik | Oszacowanie | Bł. stand. | Wartość z | $Pr(>\mid z\mid )$ | |
| :--- | :--- | :--- | :--- | :--- | :--- |
| Wyraz wolny | 6.622377 | 0.030958 | 213.912 | $<2 \times 10^{-16}$ | \*\*\* |
| AYI[AY > 6] | 0.035638 | 0.002832 | 12.586 | $<2 \times 10^{-16}$ | \*\*\* |
| DY | 0.427926 | 0.020153 | 21.234 | $<2 \times 10^{-16}$ | \*\*\* |
| I[DY > 7] | -1.126561 | 0.648119 | -1.738 | 0.0871 | . |

## 4.8 Miary Wpływu

### 4.8.1 Definicja Ogólna

Obserwacja wpływowa to taka, która, jeśli zostanie nieznacznie zmieniona lub pominięta, w istotny sposób zmodyfikuje estymowane parametry modelu. Miary wpływu mają na celu ocenę wpływu konkretnej obserwacji na estymowane parametry lub dopasowane wartości. Dźwignia jest jedną z oznak tego, jak duży wpływ ma obserwacja. Ogólną definicją dźwigni $j$-tej obserwacji jest wielkość pochodnej $i$-tej dopasowanej wartości względem $j$-tej wartości odpowiedzi.

### 4.8.2 Miary diagnostyczne oparte na macierzy kapeluszowej (hat matrix)

W przypadku rozkładu normalnego wiemy z (4.11) do (4.12), że dopasowane wartości uzyskuje się poprzez pomnożenie obserwowanych odpowiedzi przez macierz kapeluszową $\boldsymbol{H}$. Zatem element diagonalny $h_{ii}$ macierzy $\boldsymbol{H}$ mierzy wpływ $Y_i$ na $\hat{Y}_i$

$$
\hat{Y_i} = \sum_{j=1}^{n} h_{ij}Y_j = h_{ii}Y_i + \sum_{j \ne i}^{n} h_{ij}Y_j, \quad i=1, 2, \dots, n.
$$

Jest to zgodne z ogólną definicją dźwigni przytoczoną wcześniej. Ponadto, $h_{ii}$ mierzy wpływ $Y_i$ na każdą dopasowaną wartość $\hat{Y}_j$, ponieważ

$$
h_{ii} = \sum_{j=1}^{n} h_{ij}^2, \quad \text{i} \quad h_{ij} = h_{ji}.
$$

Jeśli $h_{ii} = 1$, to $\hat{Y}_i$ jest wyznaczane wyłącznie przez $Y_i$. W zastosowaniach dotyczących rezerw szkodowych jest to zazwyczaj przypadek dla ostatniej komórki w pierwszym wierszu trójkąta run-off, a także dla ostatniej komórki w pierwszej kolumnie, dla modelu traktującego AY i DY jako cechy kategoryczne, który zapewnia idealne dopasowanie. Jeśli $h_{ii} = 0$, to $y_i$ nie ma wpływu na $\hat{y}_i$.

Biorąc pod uwagę ślad macierzy kapeluszowej $\boldsymbol{H}$ podany w (4.18), średnia wartość $h_{ii}$ jest równa $\bar{h} = (p + 1)/n$. Obserwacje wpływowe często mają $h_{ii} > 2\bar{h}$; w małych próbkach, często używa się większego progu $3\bar{h}$. W uogólnionych modelach liniowych (GLM), dźwignia jest kwantyfikowana przez macierz kapeluszową $\boldsymbol{H}$ pochodzącą z ostatniej iteracji algorytmu iteracyjnie ważonych najmniejszych kwadratów (IRLS). Ilość

$$
h_{ii} = \boldsymbol{x}_i^T(\boldsymbol{X}^T\boldsymbol{W}\boldsymbol{X})^{-1}\boldsymbol{x}_i, \quad i=1, 2, \dots, n.
$$

nazywana jest dźwignią lub wartością kapeluszową. W liniowych modelach regresji normalnej jest ona nielosowa (tj. zależy tylko od położeń $\boldsymbol{x}_1, \dots, \boldsymbol{x}_n$), ograniczona do przedziału $[0, 1]$, w zależności od położenia $\boldsymbol{x}_i$ w stosunku do pozostałych $\boldsymbol{x}_k$. Duża wartość $h_{ii}$ odpowiada obserwacji z nietypowym $\boldsymbol{x}_i$, podczas gdy mała $h_{ii}$ odpowiada obserwacji bliskiej centrum danych. W GLM, odpowiedzi wchodzą w obliczenia $h_{ii}$ (poprzez wagi robocze), co czyni ich interpretację bardziej uciążliwą poza wpływem na obliczenie $i$-tej dopasowanej wartości.

### 4.8.3 Estymatory typu "Leave-One-Out" i Odległość Cooka

Innym podejściem do pomiaru wpływu jest zbadanie oszacowanych współczynników regresji uzyskanych poprzez pominięcie każdej obserwacji po kolei. Indeks dolny "$(-i)$" jest używany do wskazania, że odpowiednia wielkość została obliczona na zredukowanym zestawie danych uzyskanym przez usunięcie obserwacji $(y_i, \boldsymbol{x}_i)$ ze zbioru uczącego. Tak więc, $\boldsymbol{\hat{\beta}}_{(-i)}$ daje oszacowane współczynniki regresji na podstawie $(y_1, \boldsymbol{x}_1), \dots, (y_{i-1}, \boldsymbol{x}_{i-1}), (y_{i+1}, \boldsymbol{x}_{i+1}), \dots, (y_n, \boldsymbol{x}_n)$. Nazywa się to estymatorem typu "leave-one-out". 

W przypadku rozkładu normalnego, estymator parametru $\boldsymbol{\hat{\beta}}_{(-i)}$ uzyskany poprzez pominięcie $i$-tej obserwacji jest łatwo uzyskiwany z

$$
\boldsymbol{\hat{\beta}}_{(-i)} = \boldsymbol{\hat{\beta}} - (\boldsymbol{X}^T\boldsymbol{W}\boldsymbol{X})^{-1}\boldsymbol{x}_i \frac{y_i - \hat{y}_i}{1-h_{ii}}.
$$

W związku z tym nie ma potrzeby estymowania $\boldsymbol{\beta}$ ze zredukowanego zbioru danych, ponieważ $\boldsymbol{\hat{\beta}}_{(-i)}$ jest bezpośrednio uzyskiwane z $\boldsymbol{\hat{\beta}}$ za pomocą tej tożsamości. Podobnie dla oszacowanej wariancji,

$$
(n-p-2)\hat{\sigma}_{(-i)}^2 = (n-p-1)\hat{\sigma}^2 - \frac{(y_i - \hat{y}_i)^2}{1-h_{ii}}.
$$

Zmienne losowe $Y_i$ i $\hat{Y}_{(-i)}$ są nieskorelowane, więc

$$
\text{Var}[Y_i - \hat{Y}_{(-i)}] = \text{Var}[Y_i] + \text{Var}[\hat{Y}_{(-i)}] = \sigma^2(1 + \boldsymbol{x}_i^T(\boldsymbol{X}_{(-i)}^T\boldsymbol{X}_{(-i)})^{-1}\boldsymbol{x}_i).
$$

To pokazuje, że w przypadku rozkładu normalnego, estymacja typu "leave-one-out" nie wymaga ponownego dopasowywania modelu $n$ razy, bez $i$-tej obserwacji. Niestety, nie jest to już prawdą w przypadku innych GLM, ale opracowano aproksymacje w celu skrócenia czasu obliczeń dla nienormalnych GLM.

Odległość Cooka mierzy odległość między $\boldsymbol{\hat{\beta}}$ a $\boldsymbol{\hat{\beta}}_{(-i)}$, przy czym duże wartości wskazują na obserwacje, które mają wpływ na oszacowane współczynniki regresji. Odległość Cooka służy do badania, w jaki sposób każda obserwacja wpływa na cały wektor $\boldsymbol{\hat{\beta}}$ oszacowań parametrów. Mierzy ona różnicę $\boldsymbol{\hat{\beta}} - \boldsymbol{\hat{\beta}}_{(-i)}$ we wszystkich $p + 1$ współczynnikach regresji. Inna miara wpływu opiera się na różnicy dopasowanych wartości $\hat{\mu}_i$ przy użyciu wszystkich danych i $\hat{\mu}_{(-i)}$ z pominięciem obserwacji $i$.

Gdy $n$ jest duże (co jest ogólnie prawdą w zastosowaniach ubezpieczeniowych), pominięcie pojedynczej obserwacji $i$ nie ma tak naprawdę wpływu na oszacowane współczynniki regresji, co czyni podejście "leave-one-out" nieatrakcyjnym. Jednakże, gdy jest ono stosowane na danych zgrupowanych lub na problemach o małej skali (takich jak trójkąty run-off w rezerwach szkodowych), podejście to jest mimo wszystko pouczające.

## 4.9 Reszty Surowe, Standaryzowane i Studentyzowane

W tej sekcji omówiono różne rodzaje reszt, które służą do identyfikacji niedoskonałości modelu. Ilustracje numeryczne różnych pojęć zebrano w Sekcji 4.9.4.

### 4.9.1 Powrót do Normalnego Liniowego Modelu Regresji

Reszty reprezentują różnicę między danymi a wartościami dopasowanymi, wytworzonymi przez rozważany model. Są one zatem ważne do sprawdzania adekwatności założenia GLM. W normalnym liniowym modelu regresji reszty surowe $R_i$ są zdefiniowane jako różnica między odpowiedzią $Y_i$ a wartościami dopasowanymi $\boldsymbol{x}_i^\top \boldsymbol{\hat{\beta}}$, tj.

$$
R_i = Y_i - \boldsymbol{x}_i^\top \boldsymbol{\hat{\beta}}.
$$

Mamy wówczas

$$
\text{E}[R_i] = 0, \quad \text{Var}[R_i] = \sigma^2(1 - h_{ii}) \quad \text{i} \quad \text{Cov}[R_i, R_j] = -\sigma^2 h_{ij}
$$

gdzie $h_{ij}$ jest $(i, j)$-tym elementem macierzy kapeluszowej $\boldsymbol{H}$ zdefiniowanej w (4.12). Elementy diagonalne $h_{ii}$ są takie, że nierówności $1/n \le h_{ii} \le 1$ zachodzą dla wszystkich $i$ oraz $\sum_{i=1}^n h_{ii} = p + 1$, jak podano w (4.18). Widzimy, że w przeciwieństwie do obserwacji, reszty surowe są skorelowane. Korelacja ta jest łatwo zrozumiała, ponieważ uzyskuje się je przez odjęcie wartości dopasowanych od obserwacji, a wartości dopasowane zostały wyprodukowane przy użyciu całego zbioru danych. Wariancja reszt jest mniejsza, gdy $h_{ii}$ jest duże; może nawet wynosić zero, jeśli $h_{ii} = 1$. Ale $\boldsymbol{\hat{Y}}$ i $\boldsymbol{R}$ pozostają nieskorelowane. 

Reszty surowe dla obserwacji o dużej dźwigni (tj. dużym $h_{ii}$) mają mniejsze wariancje. Aby skorygować niestałą wariancję, możemy podzielić $R_i$ przez estymację jej odchylenia standardowego. Reszty standaryzowane definiuje się jako

$$
T_i = \frac{y_i - \hat{y}_i}{\hat{\sigma}\sqrt{1 - h_{ii}}} = \frac{R_i}{\hat{\sigma}\sqrt{1 - h_{ii}}}.
$$

Reszty standaryzowane mierzą, o ile odchyleń standardowych obserwacja jest oddalona od dopasowanego modelu. Jednakże, licznik i mianownik w $T_i$ nie są niezależne, co uniemożliwia mu podążanie za rozkładem t-Studenta. Mają one wariancję jednostkową, ale ich rozkład jest nieznany, ponieważ licznik i mianownik są skorelowane ($i$-ta obserwacja wchodzi w skład obu). Z tego powodu zamiast nich często używa się reszt studentyzowanych.

Aby obejść problem zależności, uciekamy się do estymatorów typu "leave-one-out" $\hat{\beta}_{(-i)}$ i $\hat{\sigma}_{(-i)}$, które są oparte na wszystkich obserwacjach z wyjątkiem $i$-tej. Reszty studentyzowane są wówczas dane wzorem

$$
T_i^* = \frac{R_i}{\hat{\sigma}_{(-i)}\sqrt{1 - h_{ii}}}, \quad i = 1, 2, \dots, n.
$$

Można pokazać, że

$$
T_i^* = T_i \sqrt{\frac{n-p-1}{n-p-T_i^2}}
$$

tak więc $T_i^*$ jest łatwo uzyskiwane z $T_i$, co pozwala uniknąć ponownego dopasowywania modelu $n$ razy w normalnym GLM z kanoniczną funkcją łączącą. Reszty studentyzowane $T_i^*$ podążają za rozkładem t-Studenta z $n - p - 2$ stopniami swobody. Często $T_i^*$ jest preferowane nad $T_i$, ponieważ efektywniej identyfikuje obserwacje problematyczne. Dzieje się tak, ponieważ $Y_i$ nie wchodzi do $\hat{\sigma}_{(-i)}$, co czyni tę obserwację bardziej wrażliwą w odniesieniu do $i$-tej obserwacji. Dla dużego $n$ zwykle przyjmuje się, że

$$
T_i \approx T_i^* \approx \frac{R_i}{\sigma} \approx \mathcal{Nor}(0, 1).
$$

Jednak dla mniejszych prób, te przybliżenia niekoniecznie muszą być prawdziwe, a wybór reszt staje się ważny. W zastosowaniach dotyczących rezerw szkodowych opartych na trójkątach, ze względu na stosunkowo małą liczbę obserwacji, aktuariusz musi być ostrożny przy wyborze odpowiednich reszt.

Wykresy reszt mogą ujawnić problemy z dopasowaniem modelu. Istnieje wiele sposobów wyświetlania reszt i powiązanych wielkości. Wykresy reszt względem dopasowanych wartości i każdej cechy zawartej w wyniku należą do najbardziej klasycznych wykresów diagnostycznych. Każda systematyczna zmienność lub wyizolowany punkt odkryty na wykresie na ogół wskazuje na naruszenie jednego lub więcej założeń leżących u podstaw rozważanego modelu.

### 4.9.2 Reszty w GLM

W nienormalnych GLM można zdefiniować kilka typów reszt w analogii do normalnego liniowego modelu regresji. Często odwołują się one do normalnego liniowego modelu regresji używanego w ostatnim kroku algorytmu IRLS. Surowa reszta $r_i$ jest po prostu różnicą $y_i - \hat{\mu}_i$ między obserwowaną odpowiedzią a oszacowaną średnią $\hat{\mu}_i = g^{-1}(\boldsymbol{x}_i^\top \boldsymbol{\hat{\beta}})$. Nazywa się je resztami odpowiedzi dla GLM.

Często aktuariusze dopuszczają większe odchylenia między obserwacjami $y_i$ a dopasowanymi wartościami $\hat{\mu}_i$, gdy średnia wzrasta, ponieważ wariancja jest na ogół rosnącą funkcją średniej. To sprawia, że wykres surowych reszt jest mniej informatywny. Aby obejść ten problem, reszty surowe są normalizowane przez pierwiastek kwadratowy wariancji odpowiedzi. Prowadzi to do reszt Pearsona zdefiniowanych jako

$$
r_i^P = \frac{y_i - \hat{\mu}_i}{\sqrt{V(\hat{\mu}_i)/v_i}}.
$$

Reprezentują one pierwiastek kwadratowy wkładu każdej obserwacji do statystyki $X^2$ Pearsona

$$
\sum_{i=1}^n (r_i^P)^2 = X^2.
$$

To wyjaśnia, dlaczego nazywa się je resztami Pearsona.

Reszty Pearsona są dziedziczone z modeli liniowych i nie uwzględniają kształtu rozkładu. Ponieważ dewiacja koryguje skośność odpowiedzi, w GLM zaproponowano do użycia inny resztę. Reszty dewiacyjne $r_i^D$ są zdefiniowane jako pierwiastek kwadratowy ze znakiem wkładu $i$-tej obserwacji do dewiacji. Pomaga to wykryć obserwacje, które w dużym stopniu zwiększają dewiację, a tym samym unieważniają model. W szczególności, rozłóżmy dewiację na

$$
D(\boldsymbol{y}, \hat{\boldsymbol{\mu}}) = 2 \sum_{i=1}^n (y_i(\tilde{\theta}_i - \hat{\theta}_i) - a(\tilde{\theta}_i) + a(\hat{\theta}_i)) = \sum_{i=1}^n d_i
$$

gdzie $d_i = d(y_i, \hat{\theta}_i)$ zostało wprowadzone w (4.19). Reszty dewiacyjne są wtedy zdefiniowane jako

$$
r_i^D = \text{sign}(y_i - \hat{\mu}_i)\sqrt{d_i}.
$$

Tutaj notacja "sign" oznacza znak wielkości w nawiasach:

$$
\text{sign}(y_i - \hat{\mu}_i) = \begin{cases} 1 & \text{if } y_i > \hat{\mu}_i \\ -1 & \text{w przeciwnym razie}. \end{cases}
$$

Oczywiście, suma kwadratów reszt dewiacyjnych daje dewiację, to znaczy

$$
\sum_{i=1}^n (r_i^D)^2 = D.
$$

Reszty dewiacyjne są bardziej odpowiednie w otoczeniu GLM i często są preferowaną formą reszt. W przypadku rozkładu normalnego reszty Pearsona i dewiacyjne są identyczne. Należy jednak zauważyć, że analityk nie szuka normalności w resztach dewiacyjnych, ponieważ ich rozkład próbkowy pozostaje nieznany. Chodzi o brak dopasowania i szukanie wzorców widocznych na resztach: jeśli aktuariusz wykryje jakiś wzorzec na resztach wykreślonych względem

- dopasowanych wartości,
- cech zawartych w scoringu lub
- cech nieujętych w modelu

wtedy coś jest nie tak i należy podjąć jakieś działania. Na przykład, jeśli aktuariusz wykryje pewną strukturę w resztach wykreślonych względem nieujętej cechy, wówczas cecha ta powinna zostać zintegrowana ze scoringiem. Jeśli istnieją wzorce w resztach względem każdej cechy zawartej w scoringu, wówczas odpowiednia cecha nie jest prawidłowo zintegrowana ze scoringiem (powinna zostać przekształcona, lub aktuariusz powinien porzucić GLM na rzecz modelu addytywnego). Czasami interesujące jest również wykreślenie reszt względem współrzędnych przestrzennych lub czasowych, dla których mogą istnieć wzorce. Wzorce w rozrzucie (wykryte przez wykreślenie reszt względem dopasowanych wartości) mogą wskazywać na naddyspersję lub, bardziej ogólnie, na użycie niewłaściwej relacji średnia-wariancja.

Reszty standaryzowane uwzględniają różne dźwignie. Rozważmy macierz kapeluszową (4.12) pochodzącą z ostatniej iteracji algorytmu IRLS. Wartości dźwigni $h_{ii}$ mierzą dźwignię, czyli potencjał obserwacji do wpływania na dopasowane wartości. Duża wartość $h_{ii}$ wskazuje, że $i$-ta obserwacja jest wrażliwa na odpowiedź $y_i$. Duże dźwignie zazwyczaj oznaczają, że odpowiednie cechy są w jakiś sposób nietypowe. Standaryzowane reszty, zwane również resztami odpowiadającymi, są wtedy dane przez

$$
t_i^P = \frac{r_i^P}{\sqrt{\phi(1 - h_{ii})}} \quad \text{i} \quad t_i^D = \frac{r_i^D}{\sqrt{\phi(1 - h_{ii})}}.
$$

Reszty studentyzowane (zwane również resztami scyzorykowymi lub resztami z usuwania) odpowiadają resztom wynikającym z pominięcia każdej obserwacji po kolei. Ponieważ generalnie nie istnieje prosta zależność między $\boldsymbol{\hat{\beta}}$ a $\boldsymbol{\hat{\beta}}_{(-i)}$ dla GLM, ich obliczenie wymagałoby ponownego dopasowania modelu $n$ razy, co jest poza zasięgiem dla dużych prób. Przybliżenie jest podane przez prawdopodobieństwo reszt zdefiniowane jako ważona średnia standaryzowanych reszt dewiacyjnych i Pearsona, tak że przyjmujemy

$$
t_i^* = \text{sign}(y_i - \hat{\mu}_i)\sqrt{(1-h_{ii})(t_i^D)^2 + h_{ii}(t_i^P)^2}.
$$

W przeciwieństwie do normalnego liniowego modelu regresji, gdzie standaryzowane reszty mają rozkład normalny, rozkład reszt w GLM jest ogólnie nieznany. Dlatego zakres reszt jest trudny do oceny, ale można je badać pod kątem struktury i dyspersji, wykreślając je względem dopasowanych wartości lub niektórych cech. Dźwignia mierzy tylko potencjał wpływu na dopasowanie, podczas gdy miary wpływu bardziej bezpośrednio mierzą efekt każdej obserwacji na dopasowanie. Odbywa się to zazwyczaj poprzez badanie zmiany w dopasowaniu po pominięciu tej obserwacji. W GLM można to zrobić, patrząc na zmiany w oszacowaniach współczynników regresji. Statystyka Cooka mierzy odległość między $\boldsymbol{\hat{\beta}}$ a $\boldsymbol{\hat{\beta}}_{(-i)}$ uzyskaną przez pominięcie $i$-tego punktu danych $(y_i, \boldsymbol{x}_i)$ ze zbioru uczącego.

### 4.9.3 Reszty Indywidualne lub Grupowe

Obliczenie wkładu każdej obserwacji do dewiacji może dostosować się do kształtu, ale nie do dyskrecji obserwacji. Często reszty indywidualne dla odpowiedzi dyskretnych są trudne do zinterpretowania, ponieważ dziedziczą strukturę wartości odpowiedzi. Na przykład, dla odpowiedzi binarnych reszty często skupiają się wokół dwóch krzywych, jednej dla obserwacji "0" i drugiej dla obserwacji "1". Dla zliczeń, reszty indywidualne koncentrują się wokół obserwacji "0", obserwacji "1", obserwacji "2" itd. 

Poprzez agregację reszt indywidualnych na duże grupy podobnych ryzyk, aktuariusz unika pozornej struktury wytworzonej przez całkowitoliczbową naturę odpowiedzi. Dzieje się tak, ponieważ wiemy z Rozdz. 2, że odpowiedzi podążające za rozkładem Poissona z dużą średnią lub rozkładem dwumianowym z dużą liczebnością są w przybliżeniu normalne. Reszty dewiacyjne obliczone na danych zagregowanych dają znacznie lepsze wskazania co do adekwatności modelu dla odpowiedzi.

Przypomnijmy również, że GLM służą do obliczania czystych premii. Stąd analityk jest zainteresowany estymacją wartości oczekiwanych, a nie przewidywaniem konkretnego wyniku. Dlatego dane można grupować przed oceną wyników modelu.

### 4.9.4 Ilustracje Numeryczne

#### 4.9.4.1 Wyrównywanie

Rysunek 4.8 przedstawia reszty dla formuły kwadratowej (4.15). Widzimy, że model jest mniej dokładny dla ostatnich lat życia, co jest w pewnym stopniu oczekiwane, ponieważ ekspozycje są ograniczone w starszych wiekach. Co ważniejsze, reszty nie wykazują żadnej widocznej struktury, gdy są wykreślane względem dopasowanych wartości lub cech wchodzących w skład scoringu (wiek i jego kwadrat).

Ostatni panel Rys. 4.8 łączy reszty, wartości kapeluszowe i odległości Cooka. Dokładniej, pola kół są proporcjonalne do odległości Cooka dla obserwacji. Linie poziome są narysowane na poziomie -2, 0 i 2 na skali reszt studentyzowanych, a linie pionowe na dwu- i trzykrotności średniej wartości kapeluszowej. Obszar krytyczny odpowiada górnej i dolnej prawej części wykresu (łącząc dużą wartość kapeluszową i dużą bezwzględną wartość reszty studentyzowanej), gdzie w obecnym przypadku nie przypada żaden punkt danych. Duże odległości Cooka występują w starszych wiekach (dolna lewa część wykresu), zgodnie z oczekiwaniami ze względu na ograniczoną ekspozycję pod koniec tablicy trwania życia.

#### 4.9.4.2 Rezerwacja Szkodowa

Rysunek 4.9 przedstawia reszty dla średnich kwot płatności względem wartości dopasowanych i zmiennych objaśniających. Poza tym, że więcej obserwacji jest dostępnych dla mniejszych wartości AY i DY oraz dla większych wartości CY, z czterech wykresów reszt na Rys. 4.9 nie wyłania się żadna struktura.

Rysunek 4.10 łączy reszty, wartości kapeluszowe i odległości Cooka. Można go interpretować jak ostatni panel Rys. 4.8. Dokładniej, Rys. 4.10 pokazuje, że żaden punkt danych nie wpada w regiony krytyczne odpowiadające górnej i dolnej prawej ćwiartce wykresu. Niemniej jednak niektóre komórki trójkąta wydają się mieć duże odległości Cooka, związane z niską lub umiarkowaną dźwignią.

## 4.10 Klasyfikacja Ryzyka w Ubezpieczeniach Komunikacyjnych

Aby podsumować wszystkie dotychczas przedstawione pojęcia, przeanalizujmy teraz większy zbiór danych związany z ubezpieczeniami komunikacyjnymi. Odpowiedzią jest tutaj liczba szkód zgłoszonych przez ubezpieczonego kierowcę w ramach ubezpieczenia od odpowiedzialności cywilnej.

### 4.10.1 Opis Zbioru Danych

#### 4.10.1.1 Dostępne Cechy i Odpowiedzi

Przedstawmy pokrótce dane użyte do zilustrowania technik opisanych w tym rozdziale. Zbiór danych odnosi się do portfela belgijskich ubezpieczeń komunikacyjnych OC obserwowanego pod koniec lat 90. Obejmuje on 14 504 umowy z łączną ekspozycją wynoszącą 11 183,68 poliso-lat. Są to pojazdo-lata, ponieważ każda polisa obejmuje pojedynczy pojazd, ale niektóre umowy obowiązują tylko przez część roku.

Zmienne zawarte w pliku są następujące:

*   **AgePh**: Wiek ubezpieczającego w ostatnie urodziny (na 1 stycznia), w 5 grupach od G1 do G5 o rosnącym starszeństwie.
*   **Gender**: Płeć ubezpieczającego, mężczyzna lub kobieta.
*   **PowerCat**: Moc samochodu, w czterech kategoriach od C1 do C4 o rosnącej mocy.
*   **Expor**: Liczba dni obowiązywania polisy w okresie obserwacji.
*   **CitySize**: Wielkość miasta, w którym mieszka ubezpieczający, z trzema kategoriami: duże, średnie lub małe.

Oprócz tych cech, zarejestrowano liczbę szkód **Nclaim** zgłoszonych przez każdego ubezpieczającego w okresie obserwacji. Jest to odpowiedź badana w tej sekcji.

#### 4.10.1.2 Skład Portfela w Odniesieniu do Cech

Górny lewy panel na Rys. 4.11 przedstawia rozkład ekspozycji na ryzyko w portfelu. Około 60% polis było obserwowanych przez cały rok. Biorąc pod uwagę rozkład ekspozycji na ryzyko, widzimy, że wznowienia i wygaśnięcia polis są rozłożone w ciągu roku. Należy zauważyć, że firmy ubezpieczeniowe często tworzą nowy rekord za każdym razem, gdy następuje zmiana cech (na przykład ubezpieczający kupuje nowy samochód lub przeprowadza się w inne miejsce). To wyjaśnia stosunkowo dużą liczbę umów z ekspozycją znacznie mniejszą niż jeden. Struktura wiekowa portfela jest również opisana na Rys. 4.11 (tamże górny prawy panel), podobnie jak skład zbioru danych pod względem płci, wielkości miasta, w którym mieszka ubezpieczający, oraz mocy samochodu. Widzimy, że większość ubezpieczonych kierowców należy do grupy wiekowej G2, około dwie trzecie umów obejmuje kierowcę płci męskiej, rozkład mocy wykazuje kształt litery U, a umowy są równomiernie rozłożone na trzy typy miast.

Dolny prawy panel na Rys. 4.11 przedstawia histogram zaobserwowanych liczby szkód na polisę. Widzimy, że większość polis (dokładnie 12 458) nie wygenerowała żadnej szkody. Około 10% portfela wygenerowało jedną szkodę (1 820 umów). Następnie, 206 umów wygenerowało 2 szkody, 17 umów wygenerowało 3 szkody, 2 umowy 4 szkody i była jedna polisa z 5 szkodami. Daje to globalną częstość szkód równą 20,53% na poziomie portfela (co jest dość wysokie w porównaniu z dzisiejszymi częstościami szkód na poziomie 6-7% na rynku belgijskim).

#### 4.10.1.3 Związek Między Cechami

Użyjmy współczynnika V Craméra do wykrycia cech, które są ze sobą powiązane. Rysunek 4.12 przedstawia wartości V Craméra między cechami, a także między odpowiedzią a cechami. Widzimy, że powiązania są raczej słabe, z wyjątkiem związku między Mocą a Płcią. Ale pozostaje on stosunkowo umiarkowany w każdym przypadku. Ponadto, odpowiedź wydaje się być słabo powiązana z poszczególnymi cechami. Jest to typowe dla danych dotyczących liczby szkód w ubezpieczeniach ze względu na heterogeniczność obecną w danych i dyskretność odpowiedzi.

### 4.10.2 Marginalny Wpływ Cech na Liczbę Szkód

Badania ubezpieczeniowe generalnie zaczynają się od analiz jednoczynnikowych, czyli marginalnych, podsumowujących dane odpowiedzi dla każdej wartości cechy, ale bez uwzględniania wpływu innych cech. Efekty marginalne czterech ciągłych cech na roczne częstości szkód są przedstawione na Rys. 4.13. Widzimy, że oszacowana częstość szkód dla każdego poziomu cech kategorycznych. Najlepsze oszacowania (odpowiadające okręgom) są otoczone przedziałami ufności. Wyniki te można uzyskać za pomocą analizy GLM Poissona obejmującej pojedynczą cechę, dzięki czemu przedziały ufności są łatwo wyprowadzane z rozkładu próby oszacowanych współczynników regresji.

Pomimo stosunkowo niskich wartości V Craméra, widzimy, że częstości szkód wydają się zależeć od cech w zwykły sposób, malejąc wraz z wiekiem ubezpieczającego, większe dla kierowców płci męskiej, rosnące wraz z wielkością miasta, podczas gdy wpływ mocy jest na tym etapie trudniejszy do zinterpretowania. Oczywiście, są to tylko efekty marginalne, które mogą być zniekształcone przez korelację istniejącą między cechami, dlatego potrzebujemy analizy GLM uwzględniającej wszystkie cechy, aby prawidłowo ocenić wpływ każdej cechy na oczekiwaną liczbę szkód.

### 4.10.3 Analiza GLM Poissona dla Liczby Szkód

Rozkład Poissona jest naturalnym kandydatem do modelowania liczby szkód zgłaszanych przez ubezpieczających. Typowym założeniem w tych okolicznościach jest to, że warunkowa średnia częstość szkód może być zapisana jako funkcja wykładnicza wyniku liniowego ze współczynnikami do oszacowania na podstawie danych.

Zaczynamy od modelu uwzględniającego wszystkie cechy. Poziomy odniesienia dla cech binarnych to te najliczniej występujące, zgodnie z wyjaśnionymi powyżej wytycznymi. Używamy

$$
\text{offset}_i = \ln(\text{ExpoR}_i / 365).
$$

Wyniki uzyskane za pomocą funkcji `glm` oprogramowania R są wyświetlone w Tabeli 4.10.

**Tabela 4.10** Wynik funkcji `glm` w R dla dopasowania modelu do danych TPL. Liczba iteracji scoringu Fishera: 6

| Współczynnik    | Oszacowanie | Błąd stand. | wartość z | Pr(>\|z\|)           |   |
|:----------------|:------------|:------------|:----------|:---------------------|:--|
| Intercept       | -1.46605    | 0.05625     | -26.063   | < 2 x 10<sup>-16</sup> | *** |
| Female          | -0.12532    | 0.05567     | -2.209    | 0.02714              | * |
| Age G2          | 0.25232     | 0.08865     | 2.846     | 0.00442              | ** |
| Age G3          | -0.30633    | 0.05999     | -5.106    | 3.29 x 10<sup>-7</sup> | *** |
| Age G4          | -0.32670    | 0.05854     | -5.581    | 2.40 x 10<sup>-8</sup> | *** |
| Age G5          | -0.46584    | 0.06215     | -7.495    | 6.62 x 10<sup>-14</sup>| *** |
| Power C2        | 0.08483     | 0.05430     | 1.562     | 0.11819              |   |
| Power C3        | 0.07052     | 0.06211     | 1.135     | 0.25622              |   |
| Power C4        | 0.07930     | 0.06082     | 1.304     | 0.19230              |   |
| CitySize Large  | 0.24548     | 0.04955     | 4.954     | 7.25 x 10<sup>-7</sup> | *** |
| CitySize Small  | -0.09414    | 0.05340     | -1.763    | 0.07790              |  |

Widzimy, że wpływ wieku, płci i wielkości miasta jest zgodny z wstępną analizą marginalną. Moc nie wydaje się istotnie wpływać na oczekiwaną częstość szkód. Jednak wyniki przedstawione w Tabeli 4.10 nie są wystarczające, aby stwierdzić, że moc nie jest potrzebna w GLM. Powodem jest to, że ta cecha kategoryczna została zakodowana za pomocą trzech zmiennych zero-jedynkowych, więc w Tabeli 4.10 przeprowadzane są trzy testy, po jednym dla każdego poziomu różnego od kategorii odniesienia C1. Ponieważ nie kontrolujemy całkowitego błędu związanego z wykorzystaniem wniosków z tych trzech testów jednocześnie, wartości p-wartości wyświetlane w Tabeli 4.10 nie dają aktuariuszowi odpowiedniego narzędzia do podjęcia decyzji, czy moc można usunąć ze scoringu.

Dewiacja zerowa wynosi 9,508.6 na 14,503 stopniach swobody, a dewiacja resztkowa 9,363.5 na 14,493 stopniach swobody. To pokazuje, że model regresji Poissona wyjaśnia tylko niewielką część dewiacji. AIC wynosi 13,625. Jak omówiono wcześniej, Tabela 4.10 sugeruje, że moc nie wydaje się być czynnikiem ryzyka. Aby formalnie sprawdzić, czy moc można wykluczyć z modelu GLM, porównujemy modele z i bez tej cechy za pomocą tabeli analizy dewiacji. Różnica w dewiacjach modeli z i bez mocy wynosi 3.0654 z 3 stopniami swobody. Odpowiadająca p-wartość wynosi 38.17%, co potwierdza, że moc nie jest istotna dla wyjaśnienia liczby szkód.

Co więcej, wydaje się, że dwa ostatnie poziomy CitySize można połączyć. Sprawdzamy, czy to uproszczenie modelu znacząco pogarsza dopasowanie za pomocą tabeli analizy dewiacji. Różnica w dewiacjach między początkowym modelem uwzględniającym wszystkie cechy a modelem bez Mocy i z dwoma ostatnimi poziomami CitySize połączonymi wynosi 6.2137 z 4 stopniami swobody. Odpowiadająca p-wartość 18.37% wskazuje, że grupowanie nie pogarsza znacząco dopasowania. Po ponownym dopasowaniu modelu odkrywamy, że oszacowane współczynniki regresji dla kategorii wiekowych G3 i G4 są odpowiednio równe -0.30054 i -0.31933, przy błędach standardowych odpowiednio równych 0.05984 i 0.05801. To sugeruje, że poziomy G3 i G4 Wieku można połączyć. Potwierdzono to za pomocą tabeli analizy dewiacji, ponieważ różnica w dewiacjach wynosi 6.2867 z 5 stopniami swobody dla modelu początkowego w porównaniu z uproszczonym.

**Tabela 4.11** Wynik funkcji `glm` w R dla ostatecznego modelu regresji Poissona dla danych TPL. Liczba iteracji scoringu Fishera: 6

| Współczynnik   | Oszacowanie | Błąd stand. | wartość z | Pr(>\|z\|)            |   |
|:---------------|:------------|:------------|:----------|:----------------------|:--|
| Intercept      | -1.76608    | 0.04183     | -42.216   | < 2 x 10<sup>-16</sup>  | *** |
| Female         | -0.11779    | 0.04435     | -2.656    | 0.00791               | ** |
| Age G1         | 0.54798     | 0.08933     | 6.134     | 8.55 x 10<sup>-10</sup> | *** |
| Age G2         | 0.31040     | 0.04756     | 6.527     | 6.71 x 10<sup>-11</sup> | *** |
| Age G5         | -0.14506    | 0.06276     | -2.311    | 0.02081               | * |
| CitySize Large | 0.29026     | 0.04300     | 6.751     | 1.47 x 10<sup>-11</sup> | *** |

P-wartość 27.93% pokazuje, że dalsze grupowanie jest zgodne z danymi. Ponieważ ekspozycja jest teraz w połączonej grupie wiekowej G3-G4, ta ostatnia staje się nowym poziomem odniesienia.

Po tych krokach mamy teraz ostateczny model podsumowany w Tabeli 4.11. Dewiacja zerowa 9,508.6 na 14,503 stopniach swobody jest teraz porównywana z dewiacją resztkową 9,369.8 na 14,498 stopniach swobody. Ta ostatnia jest wyższa niż odpowiadająca wartość dla początkowego modelu obejmującego wszystkie cechy. Uproszczony model mimo to oferuje lepsze dopasowanie, co wskazuje wartość AIC, która zmniejszyła się z 13,625 do 13,621.

Widzimy, że reszty oparte na obserwacjach indywidualnych dziedziczą strukturę odpowiedzi, z dolną krzywą odpowiadającą polisom bez szkód, następną dla polis, które zgłosiły 1 szkodę, 2 szkody itd. Reszty obliczone na podstawie danych zgrupowanych pozwalają uniknąć tej pułapki. Dokładniej, dane indywidualne są grupowane zgodnie z cechami wchodzącymi do ostatecznego modelu (płeć, wiek z 4 poziomami i wielkość miasta) i dodawane są ekspozycje. To tworzy podsumowany zbiór danych z 16 grupami. Oszacowane współczynniki regresji pozostają niezmienione przez grupowanie, ale reszty można teraz ponownie obliczyć (zauważ, że reszty dewiacyjne nie są addytywne). Wynikowe zgrupowane reszty są wyświetlane na dolnym panelu Rys. 4.14. Po wykreśleniu względem wartości kapeluszowych i uzupełnieniu odległościami Cooka na Rys. 4.15, uzyskane reszty wydają się wspierać dopasowanie modelu, ponieważ żadna klasa ryzyka nie wykazuje jednocześnie dużych odległości Cooka i dźwigni. Widzimy na Rys. 4.16, że prognozy modelu są w dużej mierze zgodne z doświadczeniem portfela: klasy ryzyka plasują się bardzo blisko linii 45 stopni, zwłaszcza te najliczniej występujące.

### 4.10.4 Modelowanie Alternatywne

Zamiast rejestrować liczbę $N_i$ szkód w okresie obserwacji $e_i$ (ekspozycja na ryzyko), aktuariusz mógłby równoważnie oprzeć analizę częstości szkód na czasach $T_{i1}, \dots, T_{i, N_i}$, w których wystąpiło tych $N_i$ szkód. Okresy oczekiwania $W_{ik}$ między dwiema kolejnymi szkodami zgłoszonymi przez ubezpieczającego $i$ są zdefiniowane jako

$$
W_{ik} = T_{ik} - T_{i, k-1}, \quad k=1, 2, \dots
$$

gdzie $T_{i0} = 0$, z umowy. Załóżmy, że czasy oczekiwania $W_{ik}$ są niezależne i podążają za rozkładem $\mathcal{E}xp(\lambda_i)$. Jeśli ubezpieczający $i$ nie zgłosi żadnej szkody ($N_i = 0$), to $T_{i1} = W_{i1} > e_i$, a wkład do funkcji wiarygodności wynosi

$$
\text{P}[W_{i1} > e_i] = \exp(-e_i \lambda_i).
$$

Jeśli ubezpieczający $i$ zgłosi pojedynczą szkodę ($N_i = 1$) w czasie $t_1$, to ekspozycję na ryzyko można podzielić na czas $t_1$ do momentu szkody i czas $e_i - t_1$ od $t_1$ do końca okresu obserwacji. Wkład do funkcji wiarygodności jest wtedy dany przez

$$
\lambda_i \exp(-\lambda_i t_1) \text{P}[W_{i2} > e_i - t_1] = \lambda_i \exp(-(t_1 + e_i - t_1)\lambda_i) = \lambda_i \exp(-e_i \lambda_i).
$$

Jeśli ubezpieczający $i$ zgłosi dwie szkody w czasach $t_1$ i $t_2$, to wkład do funkcji wiarygodności wynosi

$$
\lambda_i \exp(-\lambda_i t_1) \lambda_i \exp(-\lambda_i (t_2 - t_1)) \exp(-\lambda_i (e_i - t_2)) = \lambda_i^2 \exp(-e_i \lambda_i).
$$

Ogólnie, funkcja wiarygodności związana z czasami wystąpienia $N_i$ szkód ma postać

$$
\mathcal{L} = \prod_{i=1}^n (\lambda_i^{N_i} \exp(-\lambda_i e_i))
$$

gdzie

$$
N_i = \max\{k | T_{i0} + T_{i1} + \dots + T_{ik} \le e_i\}.
$$

Regresja gamma na okresach oczekiwania $W_{ik}$ jest zatem równoważna regresji Poissona na liczbie szkód.