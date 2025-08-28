# Rozdział 7: Trendy modelujące

## 7.3 Stacjonarność i modele błądzenia losowego

Podstawową kwestią w przypadku procesów, które ewoluują w czasie, jest stabilność procesu. Na przykład: „Czy dojazd do pracy zajmuje mi więcej czasu, odkąd zainstalowano nową sygnalizację świetlną?”, „Czy kwartalne zyski poprawiły się od czasu objęcia stanowiska przez nowego prezesa?”. Mierzymy procesy, aby poprawić ich wydajność, zarządzać nimi i prognozować ich przyszłość. Ponieważ stabilność jest fundamentalną kwestią, będziemy pracować ze szczególnym rodzajem stabilności, zwanym stacjonarnością.

**Definicja.** Stacjonarność to formalne pojęcie matematyczne odpowiadające stabilności szeregu czasowego danych. Mówi się, że szereg jest **(słabo) stacjonarny**, jeśli:
* Średnia $E y_t$ nie zależy od $t$.
* Kowariancja między $y_s$ a $y_t$ zależy tylko od różnicy między jednostkami czasu, $|t - s|$.

Zatem, na przykład, w warunkach słabej stacjonarności $E y_4 = E y_8$, ponieważ średnie nie zależą od czasu i w związku z tym są równe. Ponadto, $\text{Cov}(y_4, y_6) = \text{Cov}(y_6, y_8)$, ponieważ $y_4$ i $y_6$ są oddalone o dwie jednostki czasu, tak samo jak $y_6$ i $y_8$. Inną implikacją drugiego warunku jest to, że $\sigma^2 = \text{Cov}(y_t, y_t) = \text{Cov}(y_s, y_s) = \sigma^2$. Zatem **słabo stacjonarny szereg ma stałą średnią oraz stałą wariancję (jest homoskedastyczny)**. Inny rodzaj stacjonarności, znany jako **ścisła** lub **silna stacjonarność**, wymaga, aby cały rozkład $y_t$ był stały w czasie, a nie tylko średnia i wariancja.

### Biały Szum

Związek między modelami podłużnymi a przekrojowymi można ustalić poprzez pojęcie procesu **białego szumu**. Proces białego szumu to proces stacjonarny, który nie wykazuje widocznych wzorców w czasie. Mówiąc bardziej formalnie, **proces białego szumu to po prostu szereg i.i.d.**, czyli o identycznym i niezależnym rozkładzie. Proces białego szumu jest tylko jednym z rodzajów procesów stacjonarnych – w Rozdziale 8 wprowadzony zostanie inny typ, model autoregresyjny.

Szczególną cechą procesu białego szumu jest to, że prognozy nie zależą od tego, jak daleko w przyszłość chcemy prognozować. Załóżmy, że szereg obserwacji, $y_1, ..., y_T$, został zidentyfikowany jako proces białego szumu. Niech $\bar{y}$ i $s_y$ oznaczają odpowiednio średnią arytmetyczną i odchylenie standardowe próby. Prognoza obserwacji w przyszłości, powiedzmy $y_{T+l}$, dla $l$ jednostek czasu w przyszłość, wynosi $\bar{y}$. Co więcej, przedział prognozy to:

$$\bar{y} \pm t_{T-1,1-\alpha/2} s_y \sqrt{1 + \frac{1}{T}}$$

W zastosowaniach dotyczących szeregów czasowych, ponieważ wielkość próby $T$ jest zazwyczaj stosunkowo duża, używamy przybliżonego 95% przedziału predykcji $\bar{y} \pm 2s_y$. Ten przybliżony przedział prognozy ignoruje niepewność parametrów wynikającą z użycia $\bar{y}$ i $s_y$ do oszacowania średniej $E y$ i odchylenia standardowego $\sigma$ szeregu. Zamiast tego kładzie nacisk na niepewność przyszłych realizacji szeregu (znaną jako niepewność innowacji). Należy zauważyć, że przedział ten nie zależy od wyboru $l$, czyli liczby jednostek czasu, na które prognozujemy w przyszłość.

**Model białego szumu jest jednocześnie najmniej i najważniejszym z modeli szeregów czasowych.** Jest najmniej ważny w tym sensie, że model zakłada, iż obserwacje są ze sobą niepowiązane, co jest mało prawdopodobne w przypadku większości interesujących szeregów. Jest najważniejszy, ponieważ nasze wysiłki modelowania zmierzają do zredukowania szeregu do procesu białego szumu. W analizie szeregów czasowych procedura redukcji szeregu do procesu białego szumu nazywana jest **filtrem**. Po odfiltrowaniu wszystkich wzorców z danych, mówi się, że niepewność jest nieredukowalna.

### Błądzenie Losowe

Wprowadzamy teraz model błądzenia losowego. Dla tego modelu szeregu czasowego pokażemy, jak filtrować dane, po prostu obliczając różnice.

Dla ilustracji, załóżmy, że grasz w prostą grę opartą na rzucie dwiema kostkami. Aby zagrać, musisz zapłacić 7\$ za każdym razem, gdy rzucasz kostkami. Otrzymujesz liczbę dolarów odpowiadającą sumie oczek na obu kostkach, $c_t^*$. Niech $c_t$ oznacza twoją wygraną w każdym rzucie, tak że $c_t = c_t^* - 7$. Zakładając, że rzuty są niezależne i pochodzą z tego samego rozkładu, szereg $\{c_t\}$ jest procesem białego szumu.

Załóżmy, że zaczynasz z kapitałem początkowym $y_0 = \$100$. Niech $y_t$ oznacza sumę kapitału po $t$-tym rzucie. Zauważ, że $y_t$ jest określone rekurencyjnie przez $y_t = y_{t-1} + c_t$. Na przykład, ponieważ w pierwszym rzucie, $t = 1$, wygrałeś 3\$, masz teraz kapitał $y_1 = y_0 + c_1$, czyli $103 = 100 + 3$. Tabela 7.1 pokazuje wyniki dla pierwszych pięciu rzutów. Rysunek 7.6 to wykres szeregu czasowego sum, $y_t$, dla 50 rzutów.

Sumy cząstkowe procesu białego szumu definiują model błądzenia losowego. Na przykład szereg $\{y_1, ..., y_{50}\}$ na rysunku 7.6 jest realizacją modelu błądzenia losowego. Określenie „suma cząstkowa” jest używane, ponieważ każda obserwacja, $y_t$, została utworzona przez zsumowanie wygranych do czasu $t$. W tym przykładzie wygrane, $c_t$, są procesem białego szumu, ponieważ zwrócona kwota, $c_t^*$, ma rozkład i.i.d. W naszym przykładzie twoje wygrane z każdego rzutu kostką są reprezentowane przez proces białego szumu. To, czy wygrasz w jednym rzucie, nie ma wpływu na wynik następnego lub poprzedniego rzutu. W przeciwieństwie do tego, twój kapitał w dowolnym rzucie jest silnie powiązany z kapitałem po następnym lub przed poprzednim rzutem. Twój kapitał po każdym rzucie kostką jest reprezentowany przez model błądzenia losowego.

## 7.4 Wnioskowanie przy użyciu modeli błądzenia losowego

Model błądzenia losowego jest powszechnie stosowanym modelem szeregu czasowego. Aby zobaczyć, jak można go zastosować, najpierw omówimy kilka właściwości modelu. Właściwości te są następnie wykorzystywane do prognozowania i identyfikowania szeregu jako błądzenia losowego. Na koniec, w tej sekcji porównamy model błądzenia losowego z jego konkurentem, modelem liniowego trendu w czasie.

---

### Właściwości modelu

Aby określić właściwości błądzenia losowego, najpierw podsumujmy kilka definicji. Niech $c_1, ..., c_T$ będzie $T$ obserwacjami z procesu białego szumu. Błądzenie losowe można wyrazić rekurencyjnie jako:

$$y_t = y_{t-1} + c_t$$

Poprzez wielokrotne podstawianie otrzymujemy:

$$y_t = c_t + y_{t-1} = c_t + (c_{t-1} + y_{t-2}) = \dots$$

Jeśli jako poziom początkowy użyjemy $y_0$, możemy wyrazić błądzenie losowe jako:

$$y_t = y_0 + c_1 + \dots + c_t. \quad (7.7)$$

Równanie (7.7) pokazuje, że **błądzenie losowe jest sumą cząstkową procesu białego szumu*.

Błądzenie losowe nie jest procesem stacjonarnym, ponieważ zmienność, a być może i średnia, zależą od punktu w czasie, w którym szereg jest obserwowany. Obliczając wartość oczekiwaną i wariancję z równania (7.7) otrzymujemy poziom średniej i zmienność procesu błądzenia losowego:

$$E y_t = y_0 + t\mu_c  \quad \text{oraz} \quad \text{Var } y_t = t\sigma_c^2$$

gdzie $E c_t = \mu_c$ oraz $\text{Var } c_t = \sigma_c^2$. Zatem, o ile w procesie białego szumu występuje pewna zmienność ($\sigma_c^2 > 0$), błądzenie losowe jest niestacjonarne pod względem wariancji. Ponadto, jeśli $\mu_c \neq 0$, błądzenie losowe jest niestacjonarne pod względem średniej. **Błądzenie losowe jest modelem niestacjonarnym**.

---

### Prognozowanie

Jak możemy prognozować szereg obserwacji, $y_1, ..., y_T$, który został zidentyfikowany jako realizacja modelu błądzenia losowego? Technika, której używamy, polega na prognozowaniu różnic, czyli zmian w szeregu, a następnie sumowaniu prognozowanych różnic w celu uzyskania prognozy szeregu. Ta technika jest wykonalna, ponieważ, z definicji modelu błądzenia losowego, różnice mogą być reprezentowane za pomocą procesu białego szumu, czyli procesu, który potrafimy prognozować.

Rozważmy $y_{T+l}$, wartość szeregu o $l$ jednostek czasu w przyszłość. Niech $c_t = y_t - y_{t-1}$ reprezentuje różnice w szeregu, tak że:

$$
\begin{align*}y_{T+l} &= y_{T+l-1} + c_{T+l} = (y_{T+l-2} + c_{T+l-1}) + c_{T+l} = \dots \\
&= y_T + c_{T+1} + \dots + c_{T+l}
\end{align*}$$

Interpretujemy $y_{T+l}$ jako bieżącą wartość szeregu, $y_T$, plus sumę cząstkową przyszłych różnic.

Aby prognozować $y_{T+l}$, ponieważ w czasie $T$ znamy $y_T$, musimy jedynie prognozować zmiany $\{c_{T+1}, ..., c_{T+l}\}$. Ponieważ prognoza przyszłej wartości procesu białego szumu to po prostu średnia tego procesu, prognoza $c_{T+k}$ wynosi $\bar{c}$ dla $k = 1, 2, ..., l$. Łącząc te elementy, prognoza $y_{T+l}$ wynosi $y_T + l\bar{c}$. Na przykład dla $l=1$ interpretujemy prognozę następnej wartości szeregu jako bieżącą wartość szeregu plus średnią zmianę w szeregu.

Używając podobnych pomysłów, otrzymujemy, że przybliżony 95% przedział predykcji dla $y_{T+l}$ wynosi:

$$y_T + l\bar{c} \pm 2s_c\sqrt{l}$$

gdzie $s_c$ to odchylenie standardowe obliczone na podstawie zmian $c_2, c_3, ..., c_T$. Zauważ, że szerokość przedziału predykcji, $4s_c\sqrt{l}$, rośnie wraz ze wzrostem horyzontu prognozy $l$. Ta rosnąca szerokość odzwierciedla naszą malejącą zdolność do przewidywania w przyszłości.

Na przykład, rzuciliśmy kostkami $T = 50$ razy i chcemy prognozować $y_{60}$, czyli naszą sumę kapitału po 60 rzutach. W czasie 50 okazało się, że nasza dostępna suma pieniędzy wynosiła $y_{50} = \$93$. Zaczynając od $y_0 = \$100$, średnia zmiana wynosiła $\bar{c} = -7/50 = -\$0.14$, z odchyleniem standardowym $s_c = \$2.703$. Zatem, prognoza na czas 60 wynosi $93 + 10(-0.14) = 91.6$. Odpowiadający temu 95% przedział predykcji to:

$$91.6 \pm 2(2.703)\sqrt{10} = 91.6 \pm 17.1 = (74.5, 108.7).$$

**Przykład: Wskaźniki aktywności zawodowej.** Wskaźnik aktywności zawodowej (LFPR), w połączeniu z prognozami populacji, dają nam obraz przyszłej siły roboczej narodu. Obraz ten dostarcza wglądu w przyszłe funkcjonowanie całej gospodarki, dlatego prognozy LFPR są przedmiotem zainteresowania wielu agencji rządowych. W Stanach Zjednoczonych wskaźniki LFPR są prognozowane przez Social Security Administration, Bureau of Labor Statistics, Congressional Budget Office oraz Office of Management and Budget. W kontekście ubezpieczeń społecznych (Social Security), decydenci polityczni wykorzystują prognozy siły roboczej do oceny propozycji reformy systemu ubezpieczeń społecznych i do oceny jego przyszłej wypłacalności finansowej.

Wskaźnik aktywności zawodowej to cywilna siła robocza podzielona przez cywilną populację nieinstytucjonalną. Dane te są gromadzone przez Bureau of Labor Statistics. Dla celów ilustracyjnych przyjrzyjmy się konkretnej komórce demograficznej i pokażmy, jak ją prognozować – prognozy dla innych komórek można znaleźć w pracach Fullerton (1999) i Frees (2006). W szczególności badamy lata 1968–98 dla kobiet w wieku 20–44 lat, żyjących w gospodarstwie domowym z obecnym małżonkiem i co najmniej jednym dzieckiem poniżej szóstego roku życia. Rysunek 7.7 pokazuje gwałtowny wzrost LFPR dla tej grupy na przestrzeni $T = 31$ lat.

Aby prognozować LFPR za pomocą błądzenia losowego, zaczynamy od naszej najnowszej obserwacji, $LFPR_{31} = 0.6407$. Oznaczamy zmianę w LFPR przez $c_t$, tak że $c_t = LFPR_t - LFPR_{t-1}$. Okazuje się, że średnia zmiana wynosi $\bar{c} = 0.0121$, a odchylenie standardowe $s_c = 0.0101$. Zatem, używając modelu błądzenia losowego, przybliżony 95% przedział predykcji dla prognozy na $l$ kroków w przód wynosi:

$$0.6407 + 0.0121l \pm 0.0202\sqrt{l}.$$

Rysunek 7.8 ilustruje przedziały predykcji dla lat 1999-2002 włącznie.

---

### Identyfikacja stacjonarności

Widzieliśmy, jak wykonywać użyteczne zadania, takie jak prognozowanie, za pomocą modeli błądzenia losowego. Ale jak zidentyfikować szereg jako realizację błądzenia losowego? Wiemy, że błądzenie losowe jest szczególnym rodzajem modelu niestacjonarnego, więc pierwszym krokiem jest zbadanie szeregu i zdecydowanie, czy jest on stacjonarny.

Stacjonarność określa ilościowo stabilność procesu. Ponieważ proces ściśle stacjonarny ma ten sam rozkład w czasie, powinniśmy móc pobierać kolejne próbki o umiarkowanej wielkości i wykazywać, że mają one w przybliżeniu ten sam rozkład. W przypadku słabej stacjonarności średnia i wariancja są stabilne w czasie, więc jeśli pobierzemy kolejne próbki o umiarkowanej wielkości, oczekujemy, że poziom średniej i wariancja będą w przybliżeniu podobne.

W zastosowaniach zarządzania jakością, podejście to jest kwantyfikowane za pomocą *wykresów kontrolnych*. Wykres kontrolny jest użytecznym narzędziem graficznym do wykrywania braku stacjonarności w szeregu czasowym. Podstawowa idea polega na nałożeniu linii odniesienia, zwanych *limitami kontrolnymi*, na wykres szeregu czasowego danych. Te linie odniesienia pomagają wizualnie wykrywać trendy w danych i identyfikować nietypowe punkty. Mechanizm stojący za limitami kontrolnymi jest prosty. Dla danego szeregu obserwacji oblicza się średnią i odchylenie standardowe szeregu, $\bar{y}$ i $s_y$. Górny limit kontrolny definiuje się jako $UCL = \bar{y} + 3s_y$, a dolny limit kontrolny jako $LCL = \bar{y} - 3s_y$. Wykresy szeregów czasowych z nałożonymi limitami kontrolnymi są znane jako *wykresy kontrolne*.

Czasami z tego typu wykresem kontrolnym kojarzony jest przymiotnik *retrospektywny*. Przymiotnik ten przypomina użytkownikowi, że średnie i odchylenia standardowe są oparte na wszystkich dostępnych danych. W przeciwieństwie do tego, gdy wykres kontrolny jest używana jako bieżące narzędzie zarządcze do wykrywania, czy proces przemysłowy jest „poza kontrolą”, bardziej odpowiednia może być *prospektywny wykres kontrolny*. W tym przypadku, prospektywny oznacza po prostu wykorzystanie tylko wczesnej części procesu, czyli tej „w stanie kontroli”, do obliczenia limitów kontrolnych.

Wykresem kontrolnym, który pomaga nam badać stabilność średniej, jest wykres *Xbar*. Wykres *Xbar* tworzy się, łącząc kolejne obserwacje w grupy o umiarkowanej wielkości, obliczając średnią dla każdej grupy, a następnie tworząc Wykres kontrolny dla średnich grupowych. Dzięki uśrednianiu w grupach, zmienność związana z każdym punktem na wykresie jest mniejsza niż w przypadku Wykresu kontrolnego dla pojedynczych obserwacji. Pozwala to analitykowi danych uzyskać jaśniejszy obraz wszelkich wzorców, które mogą być widoczne w średniej szeregu.

Wykresem kontrolnym, który pomaga nam badać stabilność zmienności, jest wykres *R*. Podobnie jak w przypadku wykresu *Xbar*, zaczynamy od tworzenia kolejnych grup o umiarkowanej wielkości. W przypadku wykresu *R*, dla każdej grupy obliczamy **rozstęp**, czyli różnicę między największą a najmniejszą obserwacją, a następnie tworzymy wykres kontrolny dla rozstępów grupowych. Rozstęp jest miarą zmienności, którą łatwo obliczyć, co jest istotną zaletą w zastosowaniach produkcyjnych.


### Identyfikacja błądzenia losowego

Załóżmy, że podejrzewasz, iż szereg jest niestacjonarny. Jak zidentyfikować, że są to realizacje modelu błądzenia losowego? Przypomnijmy, że wartość oczekiwana błądzenia losowego, $E y_t = y_0 + t\mu_c$, sugeruje, że taki szereg podąża za liniowym trendem w czasie. Wariancja błądzenia losowego, $\text{Var } y_t = t\sigma_c^2$, sugeruje, że zmienność szeregu rośnie wraz z upływem czasu $t$. Po pierwsze, **wykres kontrolny** może pomóc nam wykryć te wzorce, niezależnie od tego, czy jest to liniowy trend w czasie, rosnąca zmienność, czy oba te zjawiska.

Po drugie, jeśli oryginalne dane podążają za modelem błądzenia losowego, to **szereg zróżnicowany podąża za modelem procesu białego szumu**. Jeśli model błądzenia losowego jest modelem kandydującym, należy zbadać różnice szeregu. W tym przypadku wykres szeregu czasowego różnic powinien przedstawiać stacjonarny proces białego szumu, który nie wykazuje widocznych wzorców. Wykresy kontrolne mogą pomóc w wykryciu braku tych wzorców.

Po trzecie, porównaj odchylenia standardowe oryginalnego szeregu i szeregu zróżnicowanego. Oczekujemy, że odchylenie standardowe oryginalnego szeregu będzie większe niż odchylenie standardowe szeregu zróżnicowanego. Zatem, jeśli szereg można przedstawić za pomocą błądzenia losowego, oczekujemy znacznej redukcji odchylenia standardowego po obliczeniu różnic.

**Przykład: Wskaźniki aktywności zawodowej, kontynuacja.** Na Rysunku 7.7 szereg wykazuje wyraźny trend wzrostowy, podczas gdy różnice nie wykazują widocznych trendów w czasie. Ponadto, po obliczeniu różnic każdego szeregu, okazało się, że:

$$0.1197 = SD(\text{szereg}) > SD(\text{różnice}) = 0.0101.$$

Zatem, wydaje się rozsądne, aby wstępnie zastosować model błądzenia losowego dla szeregu wskaźnika aktywności zawodowej.

### Model błądzenia losowego a model liniowego trendu w czasie

Przykład wskaźnika aktywności zawodowej można przedstawić za pomocą modelu błądzenia losowego lub modelu liniowego trendu w czasie. Te dwa modele są ze sobą bardziej powiązane, niż mogłoby się wydawać na pierwszy rzut oka. Aby zobaczyć tę zależność, przypomnijmy, że model liniowego trendu w czasie można zapisać jako

$$ y_t = \beta_0 + \beta_1 t + \epsilon_t, \quad (7.8) $$

gdzie $\{\epsilon_t\}$ jest procesem białego szumu. Jeśli $\{y_t\}$ jest błądzeniem losowym, można go zamodelować jako sumę cząstkową, jak w równaniu (7.7). Możemy również zdekomponować proces białego szumu na średnią $\mu_c$ plus inny proces białego szumu; to jest, $c_t = \mu_c + \epsilon_t$. Łącząc te dwie idee, model błądzenia losowego można zapisać jako

$$ y_t = y_0 + \mu_c t + u_t, \quad (7.9) $$

gdzie $u_t = \sum_{j=1}^t \epsilon_j$. Porównując równania (7.8) i (7.9), widzimy, że oba modele są podobne pod tym względem, że część deterministyczna jest liniową funkcją czasu. Różnica tkwi w składniku błędu. Składnik błędu dla modelu liniowego trendu w czasie jest stacjonarnym procesem białego szumu. Składnik błędu dla modelu błądzenia losowego jest niestacjonarny, ponieważ jest sumą cząstkową procesów białego szumu. Oznacza to, że składnik błędu jest również błądzeniem losowym. Wiele wprowadzających opracowań modelu błądzenia losowego skupia się na przykładzie „gry losowej” i ignoruje składnik dryftu $\mu_c$. Jest to niefortunne, ponieważ porównanie między modelem błądzenia losowego a modelem liniowego trendu w czasie nie jest tak wyraźne, gdy parametr $\mu_c$ jest równy zero.
