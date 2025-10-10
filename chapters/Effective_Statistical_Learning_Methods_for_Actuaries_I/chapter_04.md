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

Ważne jest, aby zdać sobie sprawę, że tutaj nie przekształcamy odpowiedzi $Y_i$, ale raczej jej oczekiwaną wartość $\mu_i$. Z punktu widzenia statystycznego, model, w którym $g(Y_i)$ jest liniowy w $\boldsymbol{x}_i$, nie jest taki sam jak GLM, w którym $g(\mu_i)$ jest liniowy w $\boldsymbol{x}_i$. Aby to zrozumieć, załóżmy, że $\ln Y_i \sim \mathcal{Noror}(\boldsymbol{x}_i^\top \boldsymbol{\beta}, \sigma^2)$. Chociaż model jest dopasowany w skali logarytmicznej, aktuariusz jest zainteresowany odpowiedziami w skali oryginalnej (w jednostkach monetarnych, dla kwot roszczeń). Jeśli estymowane wyniki $\boldsymbol{x}_i^\top \hat{\boldsymbol{\beta}}$ są potęgowane, aby wrócić do oryginalnej skali, $\exp(\boldsymbol{x}_i^\top \hat{\boldsymbol{\beta}})$ daje wartości dla mediany odpowiedzi, a nie dla średniej. Dla średniej, dopasowane wartości wynoszą

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
**Tabela 4.3** Typowe funkcje łączące i ich odwrotności. Tutaj $\mu_i$ to oczekiwana wartość odpowiedzi, $s_i$ to ocena, a $\Phi(\cdot)$ to dystrybuanta standardowego rozkładu normalnego $\mathcal{Noror}(0, 1)$.

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

więc modele Poissona i regresji logistycznej dają podobne wyniki w tym przypadku. Model probit wykorzystuje dystrybuantę $\Phi$ rozkładu $\mathcal{Noror}(0, 1)$ do odwzorowania przedziału jednostkowego $[0, 1]$. Komplementarna funkcja łącząca log-log wykorzystuje rozkład Ekstremalnej (minimum) Wartości w tym celu. Ten link dokładnie łączy GLM-y Poissona i dwumianowe, jak pokazano dalej. Rozważmy $Y \sim \mathcal{Poi}(\mu)$ i zdefiniujmy obciętą odpowiedź $\tilde{Y} = \min\{1, Y\}$.

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
Y_i = \text{offset}_i + \boldsymbol{x}_i^\top \boldsymbol{\beta} + Z_i \quad \text{gdzie } Z_i \sim \mathcal{Noror}(0, \sigma^2).
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
Y_i \sim \mathcal{Nor} \left( \text{score}_i, \frac{\sigma^2}{v_i} \right), \quad i=1, 2, \dots, n, \text{ gdzie } \text{score}_i = \boldsymbol{x}_i^\top \boldsymbol{\beta}.
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
\mathrm{E}[Y_i] = e_i \exp\left( \beta_0 + \sum_{j=1}^p \beta_j x_{ij} + \beta^+ x_i^+ \right).
$$

Jeśli cecha $x_i^+$ jest pominięta i $x_i^+$ jest skorelowana z pozostałymi cechami $x_{i1}, \dots, x_{ip}$ w tym sensie, że przybliżenie

$$
x_i^+ \approx \beta_0^+ + \sum_{j=1}^p \beta_j^+ x_{ij}
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
- z efektem $\beta^+\beta_j^+$ pominiętej cechy $x_i^+$.

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