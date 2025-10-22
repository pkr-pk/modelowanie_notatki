# Rozdział 6 Uogólnione Modele Addytywne (GAM)

## 6.1 Wprowadzenie

Widzieliśmy w rozdziale 4, że GLM oferują skuteczne rozwiązanie wielu problemów w naukach aktuarialnych. Jednakże założenie o liniowości dotyczące scoringu jest czasami wątpliwe: nieliniowe efekty ciągłych cech, takich jak wiek ubezpieczającego, mogą wymagać uwzględnienia w scoringu, aby odzwierciedlić rzeczywiste doświadczenie. Cechy ciągłe mogą efektywnie wchodzić do GLM tylko wtedy, gdy są odpowiednio przekształcone, aby odzwierciedlić ich prawdziwy wpływ na scoring. Niestety, rzadko jest jasne, jak zmienne powinny być przekształcone przed włączeniem ich do scoringu. Powszechną praktyką w firmach ubezpieczeniowych jest modelowanie nieliniowych efektów za pomocą wielomianów. Jednak wielomiany niskiego stopnia często nie są wystarczająco elastyczne, aby uchwycić strukturę obecną w danych, podczas gdy zwiększanie ich stopnia powoduje niestabilne oszacowania, zwłaszcza dla ekstremalnych wartości cech.

Inne podejście powszechnie stosowane w praktyce polega na zastąpieniu cechy ciągłej cechą kategorialną, uzyskaną przez podzielenie jej dziedziny na umiarkowaną liczbę podprzedziałów i założenie, że cecha ma stały wpływ na scoring w każdym przedziale. Oznacza to, że funkcja dająca efekt cechy ciągłej na scoring jest zakładana jako odcinkowo stała. Takie grupowanie oczywiście skutkuje utratą informacji. Ponadto nie ma ogólnej zasady określającej optymalne punkty odcięcia, więc grupowanie może obciążać ocenę ryzyka.

Uogólnione Modele Addytywne (GAM) weszły do zestawu narzędzi aktuariusza, aby radzić sobie z cechami ciągłymi w sposób elastyczny. W tym ujęciu cechy ciągłe wchodzą do modelu w semi-parametrycznym predyktorze addytywnym. W ubezpieczeniach majątkowych i osobowych, wpływ wieku ubezpieczającego, mocy pojazdu lub sumy ubezpieczonej można modelować za pomocą GAM. GAM pozwalają również aktuariuszowi analizować zmiany ryzyka według obszaru geograficznego, uwzględniając możliwą interakcję między cechami ciągłymi (szerokość i długość geograficzna dające położenie przestrzenne, w tym przypadku). Inne interakcje ogólnie obecne w danych obejmują wiek i moc, a także płeć i wiek w ubezpieczeniach komunikacyjnych. Mogą one być również uchwycone przez GAM.

W ubezpieczeniach na życie statystyki śmiertelności są często agregowane dla grup ubezpieczonych o tych samych cechach (takich jak wiek, płeć czy rodzaj produktu), aby przeprowadzić analizę GLM przy użyciu regresji Poissona. Liczby zgonów pochodzące z danych ekspozycji mogą być następnie modelowane przy użyciu regresji Poissona. Kluczowym argumentem teoretycznym legitymizującym regresję Poissona dla GLM jest to, że przy łagodnych założeniach prawdopodobieństwo Poissona wydaje się być proporcjonalne do prawdziwego prawdopodobieństwa. Dlatego wnioskowanie statystyczne oparte na prawdopodobieństwie Poissona nie jest restrykcyjne. Jest to szczególnie interesujące z praktycznego punktu widzenia, ponieważ pozwala aktuariuszom uciekać się do dostępnego oprogramowania statystycznego wykonującego regresję Poissona do budowy tablic trwania życia.

Gdy cechy ciągłe są uwzględniane w analizie przeżycia, takie jak suma ubezpieczona dla ubezpieczonych, agregacja według grup wymaga wstępnego, subiektywnego grupowania. Sprytniejsze wykorzystanie GAM pozwala uniknąć tego problematycznego kroku i pozwala aktuariuszowi analizować indywidualne dane o śmiertelności za pomocą regresji Poissona. Niezbędna baza danych zawiera jeden rekord na polisę i rok (ten sam format co w ubezpieczeniach komunikacyjnych, na przykład) z \mathcal{bin}arną zmienną wskazującą zgon i numerem referencyjnym łączącym wszystkie rekordy związane z tą samą polisą. Czasami wymaga to rozszerzenia bazy danych poprzez podzielenie każdej indywidualnej obserwacji na zestaw niezależnych realizacji zliczeń Poissona o tych samych niezmiennych w czasie zmiennych objaśniających, lub pozwolenie, aby ewoluowały one w czasie, gdy są dynamiczne (takie jak osiągnięty wiek, na przykład). To sprawia, że podejście GAM jest równie skuteczne w badaniach ubezpieczeń na życie i majątkowych.

## 6.2 Struktura GAM

### 6.2.1 Rozkład odpowiedzi

Podobnie jak w przypadku GLM, rozważamy odpowiedzi $Y_1, Y_2, \ldots, Y_n$ mierzone na $n$ osobach. Dla każdej odpowiedzi aktuariusz ma wektor $\boldsymbol{x}_i = (1, x_{i1}, \ldots, x_{ip})^{\text{T}}$ o wymiarze $p+1$ zawierający odpowiednie cechy (uzupełnione o 1 z przodu, gdy w scoringu uwzględniony jest wyraz wolny). Cechy są teraz podzielone na $p_{\text{cat}}$ zmiennych kategorialnych $x_{i1}, \ldots, x_{ip_{\text{cat}}}$ i $p_{\text{cont}} = p - p_{\text{cat}}$ zmiennych ciągłych $x_{i, p_{\text{cat}}+1}, \ldots, x_{ip}$.
Biorąc pod uwagę warunkową informację zawartą w $x_i$, zakładamy, że odpowiedzi $Y_i$ są niezależne z rozkładem warunkowym w rodzinie ED gruntownie przestudiowanej w rozdziale 2. W szczególności, $Y_i$ podlega rozkładowi (2.3) z kanonicznym parametrem $\theta_i$ i tym samym parametrem dyspersji $\phi$. Sposób, w jaki $\theta_i$ jest powiązany z informacją zawartą w $\boldsymbol{x}_i$, zostanie wyjaśniony w następnej kolejności.

### 6.2.2 Addytywne scoringi

W praktyce wiele cech jest kategorialnych i efektywnie wchodzi do GLM po zakodowaniu za pomocą zmiennych \mathcal{bin}arnych. Jednak niektóre ważne zmienne scoringowe są ciągłe: w taryfikacji ubezpieczeń komunikacyjnych, wiek lub moc samochodu są tego przykładem. W ubezpieczeniach na życie i zdrowie, wiek i suma ubezpieczona są cechami ciągłymi. W ogólności, ciągłość odnosi się tutaj do cechy ilościowej z wieloma odrębnymi, uporządkowanymi wartościami. Na przykład wiek jest generalnie rejestrowany jako wiek w ostatnie urodziny, więc przyjmuje tylko wartości całkowite (18, 19, 20, ... w ubezpieczeniach komunikacyjnych, na przykład). Niemniej jednak jest uważany za cechę ciągłą, ponieważ oczekujemy płynnego postępu ryzyka wraz z wiekiem, bez nagłych skoków.

Wyjaśnijmy teraz ograniczenia analizy GLM, jeśli chodzi o rozpoznanie, że ciągła cecha $x^*$ ma nieliniowy wpływ na scoring. Wprowadzenie $x^*$ bezpośrednio do scoringu GLM sprowadza się do założenia liniowego wpływu $x^*$ na skali scoringu: z funkcją log-link, oznacza to, że średnia odpowiedź jest ograniczona do wykładniczego wzrostu wraz z $x^*$. Jednak takie monotoniczne zachowanie wykładnicze może nie być poparte danymi, jak pokazano w następnym przykładzie.

**Przykład 6.2.1** Rozważmy na przykład wiek w taryfikacji ubezpieczeń komunikacyjnych. Wprowadzenie wieku bezpośrednio do scoringu GLM oznacza liniowy wpływ wieku na skalę scoringu. Rozważmy na przykład liczbę szkód $Y_i$ zgłoszonych przez ubezpieczającego $i$ i model Poissona GLM

$$
Y_i \sim \mathcal{Poi} \left( e_i \exp \left( \beta_0 + \sum_{j=1}^{p} \beta_j x_{ij} + \beta_{\text{age}} \text{age}_i \right) \right),
$$

gdzie $e_i$ to ekspozycja na ryzyko, a $x_i$ podsumowuje wszystkie dostępne informacje o ubezpieczającym $i$, oprócz osiągniętego wieku. Widzimy, że oczekiwana liczba szkód

$$
\mu_i = e_i \exp \left( \beta_0 + \sum_{j=1}^{p} \beta_j x_{ij} + \beta_{\text{age}} \text{age}_i \right)
$$

albo rośnie (jeśli $\beta_{\text{age}} > 0$) albo maleje (jeśli $\beta_{\text{age}} < 0$) wraz z wiekiem w sposób wykładniczy. Jednak wpływ wieku na oczekiwaną liczbę szkód w ubezpieczeniach komunikacyjnych ma zazwyczaj kształt litery U (co może być zniekształcone, ponieważ dzieci pożyczają samochód rodziców i powodują wypadki), co jest sprzeczne z wykładniczym zachowaniem implicite zakładanym przez GLM.

Wprowadzenie ciągłej cechy $x^*$ w sposób liniowy na skali scoringu może prowadzić do błędnej oceny ryzyka, podczas gdy traktowanie jej jako cechy kategorialnej z wieloma poziomami może
- wprowadzić dużą liczbę dodatkowych parametrów regresji;
- nie rozpoznać oczekiwanej płynnej zmiany średniej odpowiedzi w $x^*$ (ponieważ parametry regresji odpowiadające każdemu poziomowi nie są ze sobą powiązane po zakodowaniu za pomocą zmiennych \mathcal{bin}arnych).

Ogólnie rzecz biorąc, powoduje to dużą zmienność w oszacowanych współczynnikach regresji związanych z różnymi poziomami cechy ciągłej $x^*$, gdy jest ona traktowana jako cecha kategorialna.
Zanim GAM weszły do zestawu narzędzi aktuariusza, klasyczne podejście do radzenia sobie z cechami ciągłymi było czysto parametryczne: każda cecha ciągła $x^*$ była przekształcana przed włączeniem do scoringu. Ogólnie rzecz biorąc, funkcja dająca wpływ $x^*$ na scoring jest znana, liniowa, wielomianowa lub odcinkowo stała. Tak więc funkcja jest znana z dokładnością do skończonej liczby parametrów do oszacowania na podstawie danych i wracamy do przypadku GLM. Jednakże, bez względu na to, jaką formę parametryczną określimy, zawsze wykluczamy wiele możliwych funkcji.
Aktuariusze często oczekują płynnej zmiany średniej odpowiedzi wraz z $x^*$. Oczekuje się, że oczekiwana liczba szkód w ubezpieczeniach komunikacyjnych lub wskaźnik śmiertelności w ubezpieczeniach na życie będzie się zmieniać bardzo płynnie wraz z wiekiem, bez nagłych skoków z jednego wieku do drugiego. W tym rozdziale wyjaśniamy, jak dopasować model z addytywnym scoringiem pozwalającym na nieliniowe efekty ciągłych cech na skali scoringu, które są wyuczone z dostępnych danych. Poniższy przykład w ubezpieczeniach komunikacyjnych wyjaśnia wartość dodaną GAM w tym zakresie.

**Przykład 6.2.2** Kontynuujmy z przykładem 6.2.1. Rozważmy dane $(Y_i, e_i, x_i)$. Możemy zobaczyć ekspozycje na ryzyko $e_i$ (w polisach-latach), a także wynik uzyskany przez traktowanie wieku liniowo na skali scoringu (linia prosta) lub jako cechy kategorialnej, z określonym współczynnikiem regresji dla każdego wieku (okręgi). Widzimy, że żadne z tych podejść nie jest zadowalające: efekt liniowy pomija efekt wieku w kształcie litery U, podczas gdy współczynniki regresji specyficzne dla wieku nie są wystarczająco gładkie.
To jest powód, dla którego przechodzimy na

$$
\beta_0 + \sum_{j=1}^{p} \beta_j x_{ij} + b(\text{age}_i)
$$

dla pewnej nieznanej funkcji $b(\cdot)$. Nie zakłada się niczego o $b(\cdot)$ poza tym, że jest to gładka funkcja wieku. W istocie, $x \mapsto \hat{b}(x)$ jest wygładzoną wersją współczynników regresji $x \mapsto \hat{\beta}_x$ uzyskanych przez traktowanie wieku $x$ jako cechy kategorialnej (tak, że wiek jest zakodowany przez zestaw zmiennych \mathcal{bin}arnych, po jednej dla każdego obserwowanego wieku w bazie danych z wyjątkiem referencyjnego). W naszym przykładzie $\hat{\beta}_x$ są okręgami widocznymi w prawym górnym panelu Rys. 6.1. W szczególności wyjaśniamy, jak dopasować model

$$
Y_i \sim \mathcal{Poi} \left( e_i \exp \left( \beta_0 + \sum_{j=1}^{p} \beta_j x_{ij} + b(\text{age}_i) \right) \right).
$$

Wynik jest wyświetlony w prawym dolnym panelu Rys. 6.1. Widzimy, że $\hat{b}(\cdot)$ ładnie wygładza $\hat{\beta}_x$ z wyjątkiem starszych grup wiekowych, gdzie występuje duża zmienność (z powodu małych ekspozycji dla każdego wieku, jak widać na lewym górnym panelu Rys. 6.1).

Kryterium używane do wyboru stopnia gładkości na podstawie danych wyświetlonych w lewym dolnym panelu, zwane LCV od Likelihood Cross Validation, zostanie wyjaśnione w następnej sekcji.

Ta sama sytuacja występuje w ubezpieczeniach na życie. Biorąc pod uwagę Rys. 4.2, widzimy, że prawdopodobieństwa zgonu nie zależą liniowo od wieku w całym zakresie wiekowym. Prawdopodobieństwa zgonu są raczej wysokie dla noworodków, następnie gwałtownie spadają do niższego poziomu śmiertelności dla dzieci i nastolatków. Dla wieku 20-25 lat obserwuje się tak zwany "wzgórek wypadkowy", który jest szczególnie wyraźny u mężczyzn. Od 30 roku życia prawdopodobieństwa zgonu rosną niemal liniowo wraz z wiekiem (na skali scoringu). W starszym wieku roczne prawdopodobieństwa zgonu pozostają rosnące, ale o innym kształcie. Surowe $\hat{q}_x = D_x/l_x$ wyświetlone na Rys. 4.2 można postrzegać jako wynik \mathcal{Bin}omialnego GLM, gdzie wiek $x$ jest traktowany jako cecha kategorialna. Wynikowe oszacowane prawdopodobieństwa zgonu nie są połączone, więc wyraźnie widoczne są tam nieregularne zmiany.

Tradycyjnie wiek jest uwzględniany w modelu za pomocą wielomianu. Zostało to zrobione w rozdziale 4 przy użyciu kwadratowej formuły Perksa. Aby odtworzyć funkcjonalną formę wieku dla wskaźników śmiertelności, zwykle wymagany jest wielomian rzędu szóstego lub wyższego, jeśli brany jest pod uwagę cały okres życia. Jednak wielomiany wyższego rzędu mają tendencję do pokazywania dzikich fluktuacji w regionach ogonowych (to jest dla bardzo młodych i bardzo starych grup wiekowych), a pojedyncze obserwacje w regionach ogonowych mogą silnie wpływać na krzywą.

Inne podejście polega na oszacowaniu funkcjonalnej formy wieku w sposób nieparametryczny. W przeciwieństwie do podejścia parametrycznego, aktuariusz nie określa z góry określonej funkcji, ale pozwala danym określić optymalną formę. Metody estymacji nieparametrycznej obejmują wygładzanie splajnami lub lokalne metody GLM, jak wyjaśniono w następnych sekcjach.

### 6.2.3 Funkcja łącząca

Jesteśmy teraz gotowi, aby podać definicję GAM. Podobnie jak w przypadku GLM, odpowiedź $Y_i$ ma rozkład z rodziny ED. Cechy kategorialne $x_{i1}, \ldots, x_{i,p_{\text{cat}}}$ są łączone z cechami ciągłymi $X_i, p_{\text{cat}}+1, \ldots, x_{ip}$, tworząc addytywny scoring

$$
\text{score}_i = \beta_0 + \sum_{j=1}^{p_{\text{cat}}} \beta_j x_{ij} + \sum_{j=p_{\text{cat}}+1}^{p} b_j(x_{ij}).
$$

Niech $g$ będzie funkcją łączącą. Średnia $\mu_i$ z $Y_i$ jest powiązana z nieliniowym scoringiem przez zależność

$$
g(\mu_i) = \beta_0 + \sum_{j=1}^{p_{\text{cat}}} \beta_j x_{ij} + \sum_{j=p_{\text{cat}}+1}^{p} b_j(x_{ij}).
$$

Gładkie, nieokreślone funkcje $b_j$ oraz współczynniki regresji $\beta_0, \beta_1, \ldots, \beta_{p_{\text{cat}}}$ są szacowane na podstawie dostępnych danych przy użyciu technik przedstawionych w następnej sekcji (lokalne GLM lub splajny).
GLM zakładają określoną formę dla funkcji $b_j$, taką jak liniowa lub kwadratowa, która może być oszacowana parametrycznie. Przeciwnie, GAM nie określają żadnej sztywnej formy parametrycznej dla $b_j$, ale pozostawiają je nieokreślone. Jedyne założenie dotyczące $b_j$ to gładkość. Przekładając to na terminy matematyczne, oznacza to, że $b_j$ jest ciągła z ciągłymi pierwszymi, drugimi, ..., pochodnymi. To ograniczenie jest znacznie mniej restrykcyjne niż podejście parametryczne. Ta elastyczność jest często pożądana w badaniach ubezpieczeniowych, gdzie skoki lub gwałtowne przerwy rzadko występują. I to proste założenie wydaje się być wystarczające do oszacowania $b_j$ za pomocą podejść semi-parametrycznych, takich jak lokalne GLM lub splajny.
Ponieważ funkcja $b_j$ wymaga niewielkiej ilości informacji, musimy narzucić $E[b_j(X_{ij})] = 0$, aby mieć model identyfikowalny. W przeciwnym razie moglibyśmy dodawać i odejmować stałe do jednowymiarowych funkcji $b_j$, pozostawiając scoring niezmieniony. Ograniczenie to jest generalnie uwzględniane po ostatnim kroku estymacji, poprzez centrowanie wszystkich oszacowanych funkcji $b_j$ (odejmując średnią wartość przekształconej cechy).
Oprócz funkcji jednowymiarowych, do scoringu mogą również wchodzić terminy interakcji w postaci $b_{jk}(x_{ij}, x_{ik})$ lub $b_{jkl}(x_{ij}, x_{ik}, x_{il})$. Dla więcej niż trzech cech jest generalnie niemożliwe dopasowanie terminów interakcji wyższego rzędu. Czasami przydatne jest uwzględnienie ograniczonych terminów w postaci $b_{jk}(x_{ij}, x_{ik})$ z argumentem, że produkt cech $j$-tej z $k$-tą, zwłaszcza gdy wielkość próby jest umiarkowana. Poniższe przykłady ilustrują użycie funkcji z dwoma argumentami w analizie GAM.

**Przykład 6.2.3** Obecnie stało się powszechną praktyką, aby składka za ryzyko na jednostkę ekspozycji zmieniała się w zależności od obszaru geograficznego, gdy utrzymywane są wszystkie inne czynniki ryzyka. Na przykład w ubezpieczeniach komunikacyjnych większość firm przyjęła klasyfikację ryzyka w zależności od strefy geograficznej, w której mieszka ubezpieczający (np. miasto vs. wieś). Jeśli ubezpieczyciel chce wdrożyć dokładniejsze podziały kraju zgodnie z kodami pocztowymi, wówczas zmienność przestrzenna związana z czynnikami geograficznymi (tj. czynnikami społeczno-demograficznymi) może być uchwycona na mapie, reprezentowanej przez szerokość i długość geograficzną. Główne założenie tego podejścia jest takie, że charakterystyki roszczeń mają tendencję do bycia podobnymi w sąsiednich obszarach kodów pocztowych (po uwzględnieniu innych czynników ryzyka). GAM wykorzystują tę przestrzenną gładkość, pozwalając na transfer informacji do i z sąsiednich regionów.
Efekt geograficzny można uchwycić poprzez lokalizację miejsca zamieszkania ubezpieczającego, reprezentowaną przez szerokość i długość geograficzną. Scoring zawiera wtedy termin postaci

$$
\ldots + b_{\text{spat}}(\text{szerokość}_i, \text{długość}_i) + \ldots
$$

gdzie funkcja $b_{\text{spat}}(\cdot, \cdot)$ jest zakładana jako gładka i szacowana na podstawie danych.

**Przykład 6.2.4** Powszechnie spotykana interakcja w ubezpieczeniach komunikacyjnych obejmuje wiek i moc samochodu. Potencjalnie niebezpieczne mogą być potężne samochody prowadzone przez niedoświadczonych kierowców, podczas gdy wpływ mocy zmienia się wraz z wiekiem. Oznacza to, że scoring zawiera wtedy termin postaci

$$
\ldots + b(\text{moc}_i, \text{wiek}_i) + \ldots
$$

gdzie funkcja $b(\cdot, \cdot)$ jest zakładana jako gładka i szacowana na podstawie danych.

Interakcje między cechami kategorialnymi i ciągłymi mogą być również uwzględnione w scoringu, jak pokazano w następnym przykładzie.

**Przykład 6.2.5** Aby uwzględnić możliwą interakcję między wiekiem ubezpieczającego a płcią ($\text{płeć}_i = 1$, jeśli ubezpieczający $i$ jest mężczyzną, a $\text{płeć}_i=0$ w przeciwnym razie), predyktor zawiera

$$
b_1(\text{wiek}_i) + b_2(\text{wiek}_i)\text{płeć}_i
$$

W tym modelu funkcja $b_1$ odpowiada (nieliniowemu) wpływowi wieku dla kobiet, a $b_2$ odchyleniu od tego wpływu dla mężczyzn. Wpływ dla mężczyzn jest zatem dany przez sumę $b_1 + b_2$.

W porównaniu z metodami opartymi na drzewach (Trufin i in. 2019) i sieciami neuronowymi (Hainaut i in. 2019), interakcje muszą być ustrukturyzowane przed włączeniem do GAM. Zatem drzewa i sieci neuronowe są pomocne w eksploracji możliwych efektów interakcji obecnych w danych, aby kierować aktuariuszem przy określaniu odpowiednich addytywnych scoringów w GAM.

Podsumowując, modele addytywne są znacznie bardziej elastyczne niż modele liniowe, ale pozostają interpretowalne, ponieważ oszacowaną funkcję $b_j$ można wykreślić, aby zwizualizować marginalną zależność między cechą ciągłą $x_{ij}$ a odpowiedzią. W porównaniu z parametrycznymi przekształceniami cech, takimi jak wielomiany, modele addytywne określają najlepsze przekształcenie $b_j$ bez żadnych sztywnych założeń parametrycznych poza gładkością. GAM oferują zatem podejście oparte na danych, mające na celu odkrycie odrębnego wpływu każdej cechy na scoring, przy założeniu, że jest on addytywny.

## 6.3 Wnioskowanie w modelach GAM

### 6.3.1 Lokalne Wielomianowe Modele GLM

#### 6.3.1.1 Idea Intuicyjna

Na początek załóżmy, że mamy pojedynczą ciągłą cechę $x$ do modelowania odpowiedzi $Y$. W zastosowaniu, które mamy na myśli w tym rozdziale, lokalna regresja jest używana do modelowania związku między cechą $x$ a odpowiedzią $Y$. Zazwyczaj $x$ reprezentuje wiek, podczas gdy $Y$ może być związane ze śmiertelnością lub zachorowalnością, albo liczbą lub wysokością szkód. Ponieważ istnieje unikalna cecha, funkcja łącząca zapewnia jedynie, że estymowana średnia należy do zbioru dopuszczalnych wartości, i możemy po prostu zrównać kanoniczny parametr z nieznaną funkcją, którą chcemy oszacować, tj. praca z funkcją $x \mapsto \theta(x)$. Centralnym założeniem jest gładkość: zakłada się, że funkcja $x \mapsto \theta(x)$ jest ciągła, z ciągłymi pochodnymi. Oznacza to, że oczekujemy, iż gładki postęp średniej odpowiedzi w ramach cechy $x$. Gdy założono, że funkcja do estymacji jest gładka, można ją rozwinąć za pomocą wzoru Taylora, aby uzyskać przybliżenie

$$
\theta(x) \approx \beta_0 + \beta_1 x + \dots + \beta_k x^k.
$$

Jednak takie wielomianowe przybliżenie zwykle działa bardzo słabo, zwłaszcza na krańcach (w "ogonach"). Dlatego opracowano inne algorytmy.

W przypadku wielomianów lokalnych, przybliżenie wielomianowe nie jest stosowane globalnie, ale tylko w sąsiedztwie interesującego nas punktu. Oznacza to, że uznajemy, iż zachowanie wielomianowe sugerowane przez rozwinięcie Taylora obowiązuje tylko lokalnie, więc współczynniki aproksymującego wielomianu mogą się różnić w zależności od wartości $x$. W swojej najprostszej formie, metoda estymacji wykorzystuje dobrze znany wynik, że każda funkcja różniczkowalna może być lokalnie aproksymowana przez swoją linię styczną.

Mając obserwacje $(x_1, Y_1), (x_2, Y_2), \dots, (x_n, Y_n)$, zakładamy, że odpowiedź $Y_i$ ma rozkład masy prawdopodobieństwa/gęstości z rodziny ED z parametrem kanonicznym $\theta_i$ i wagą jednostkową, gdzie $\theta_i = \theta(x_i)$ jest funkcją $x_i$ (przy kanonicznej funkcji łączącej). W GLM zakłada się, że $\theta(x)$ ma specyficzną formę parametryczną, na przykład $\theta(x) = \beta_0 + \beta_1 x$ z tymi samymi punktami przecięcia $\beta_0$ i nachyleniem $\beta_1$ niezależnie od wieku $x$. W przypadku GAM nie czyni się silnych założeń co do funkcji $x \mapsto \theta(x)$, poza tym, że jest to gładka funkcja, którą można lokalnie przybliżyć za pomocą prostych funkcji parametrycznych.

#### 6.3.1.2 Okno Wygładzania

Podejście lokalnego GLM nie zakłada już, że $\theta(x)$ ma sztywną formę parametryczną, ale dopasowuje model wielomianowy lokalnie w oknie wygładzania

$$
\mathcal{V}(x) = (x - h(x), x + h(x)).
$$

Przypomnijmy z twierdzenia Taylora, że każda funkcja różniczkowalna może być lokalnie aproksymowana przez linię prostą, a każda dwukrotnie różniczkowalna funkcja może być lokalnie aproksymowana przez kwadratowe przybliżenie. Wewnątrz okna wygładzania $\mathcal{V}(x)$, nieznana funkcja $\theta(\cdot)$ jest następnie aproksymowana przez taki wielomian niskiego stopnia. Zmieniając okno, prowadzi to do aproksymacji $\theta(\cdot)$ przez różne lokalne wielomiany. Współczynniki tych lokalnych wielomianów są następnie estymowane metodą największej wiarygodności w odpowiednim GLM.

Tylko obserwacje w oknie wygładzania $\mathcal{V}(x)$ odgrywają rolę w estymacji $\theta$ w punkcie $x$. Powszechnie stosowana jest szerokość pasma oparta na najbliższym sąsiedztwie: okno wygładzania jest tak dobrane, aby zawierało określoną liczbę punktów. Dokładniej, $\lambda$ najbliższych sąsiadów $x$ jest zbieranych w zadanym zbiorze $\mathcal{V}(x)$, tj.

$$
\#\mathcal{V}(x) = \lambda.
$$

Liczba $\lambda$ obserwacji jest często wyrażana jako proporcja $\alpha = \frac{\lambda}{n}$ zbioru danych ($\alpha$ reprezentuje proporcję obserwacji uwzględnionych w każdym wygładzaniu). Wtedy $h(x)$ jest odległością od $x$ do najdalszego z jego $\lambda$ najbliższych sąsiadów, tj.

$$
h(x) = \max_{i \in \mathcal{V}(x)} |x - x_i|.
$$

Szerokość pasma $h(x)$ ma krytyczny wpływ na regresję lokalną. Jeśli $h(x)$ jest zbyt małe, w oknie wygładzania znajdzie się niewystarczająca ilość danych i dopasowanie może nie dać dobrych wyników. Z drugiej strony, jeśli $h(x)$ jest zbyt duże, lokalny wielomian może nie pasować dobrze do danych w oknie wygładzania, a ważne cechy funkcji średniej mogą zostać zniekształcone lub utracone.

#### 6.3.1.3 Lokalnie Ważone Dopasowanie GLM

Estymacja $\theta(x)$ jest uzyskiwana z lokalnego GLM, na podstawie obserwacji w $\mathcal{V}(x)$. W celu oszacowania $\theta$ w pewnym punkcie $x$, obserwacje w $\mathcal{V}(x)$ są ważone w taki sposób, że największe wagi są przypisywane obserwacjom bliskim $x$, a mniejsze tym, które są dalej. W wielu przypadkach waga $v_i(x)$ przypisana do $(x_i, Y_i)$ w celu oszacowania $\theta(x)$ jest uzyskiwana z pewnej funkcji wagowej $v(\cdot)$ wybranej jako ciągła, symetryczna, z maksimum w 0 i zdefiniowana na przedziale $[-1, 1]$. Dokładnie, wagi uzyskuje się ze wzoru

$$
v_i(x) = v\left(\frac{x_i - x}{h(x)}\right), \quad x_i \in \mathcal{V}(x).
$$

Tylko obserwacje w oknie wygładzania $\mathcal{V}(x)$ otrzymują niezerowe wagi i odgrywają rolę w estymacji $\theta(\cdot)$ w punkcie $x$.

Powszechnym wyborem jest funkcja wagowa trójsześcienna (tricube) zdefiniowana jako

$$
v(u) = \begin{cases} (1 - |u|^3)^3 & \text{dla } -1 < u < 1, \\ 0 & \text{w przeciwnym razie}. \end{cases}
$$

Pod warunkiem, że wielkość próby jest wystarczająco duża, wybór funkcji wagowej nie jest zbyt krytyczny, a funkcja wagowa trójsześcienna wydaje się wygodnym wyborem. Zgodnie z (6.1), wagi są wówczas dane przez

$$
v_i(x) = \begin{cases} \left(1 - \left(\frac{|x_i - x|}{\max_{i \in \mathcal{V}(x)} |x_i - x|}\right)^3\right)^3 & \text{dla } i \text{ takiego, że } x_i \in \mathcal{V}(x), \\ 0 & \text{w przeciwnym razie}. \end{cases}
$$

Oczywiście dostępne są inne alternatywy dla funkcji wagowej trójsześciennej, na przykład prostokątna (lub jednostajna), gaussowska lub Epanechnikova. W celu sprawdzenia spójności, aktuariusz może zmieniać funkcję wagową, aby sprawdzić, czy wynikowe dopasowanie pozostaje stabilne.

Wielomian o wysokim stopniu może zawsze zapewnić lepsze przybliżenie nieznanej funkcji $\theta(\cdot)$ niż wielomian o niskim stopniu. Jednak wielomiany wysokiego rzędu mają większą liczbę współczynników do oszacowania, a wynikająca z tego zwiększona wariancja w estymacji. W pewnym stopniu efekty stopnia wielomianu i szerokości pasma są mylone. Często wystarcza wybrać wielomian niskiego stopnia i skoncentrować się na wyborze szerokości pasma w celu uzyskania zadowalającego dopasowania. To jest powód, dla którego wewnątrz okna wygładzania $\mathcal{V}(x)$, nieznana funkcja $\theta(\cdot)$ jest generalnie aproksymowana przez wielomian niskiego stopnia:

$$
\theta(x) \approx \begin{cases} \beta_0(x) & \text{(lokalna regresja stała)} \\ \beta_0(x) + \beta_1(x)x & \text{(lokalna regresja liniowa)} \\ \beta_0(x) + \beta_1(x)x + \beta_2(x)x^2 & \text{(lokalna regresja kwadratowa)} \end{cases}
$$

ze współczynnikami specyficznymi dla $x$ (jak sugeruje rozwinięcie Taylora). Lokalne współczynniki regresji $\beta_j(x)$ są estymowane metodą największej wiarygodności w odpowiednim GLM, z wagami $v_i(x)$ specyficznymi dla $x$. Zauważ, że lokalna stała regresja uogólnia podejście KNN (lub najbliższych sąsiadów).

W swojej podstawowej formie, lokalny GLM zakłada, że $\theta_i = \theta(x_i)$ jest liniowe w $x_i$, z punktem przecięcia i nachyleniem specyficznym dla $x_i$. Dokładnie, wiarygodność jest maksymalizowana względem tych współczynników. Precyzyjnie,

$$
\theta(x_i) = \beta_0(x_i) + \beta_1(x_i)x_i
$$

a lokalna log-wiarygodność w $x$ wynosi

$$
L_x(\beta_0(x), \beta_1(x)) = \sum_{i=1}^n v_i(x) l(y_i, \beta_0(x) + \beta_1(x)x_i)
$$

gdzie $l$ to logarytm gęstości ED podany przez (2.3)

$$
l(y_i, \beta_0(x) + \beta_1(x)x_i) = \frac{1}{\phi}(y_i(\beta_0(x) + \beta_1(x)x_i) - a(\beta_0(x) + \beta_1(x)x_i)) + \text{stała}.
$$

Te wzory można łatwo dostosować do przypadku lokalnej stałej i lokalnej kwadratowej.

**Przykład 6.3.1** Rozważmy na przykład przypadek Poissona z kanonicznym łączem logarytmicznym. Załóżmy, że dla odpowiedzi $Y$ z cechą $x$ i wagą jednostkową, nieznana funkcja $\theta(\cdot)$ jest dana przez lokalny model liniowy.

$$
\theta(x) = \beta_0(x) + \beta_1(x)x.
$$

Średnia odpowiedź wynosi wtedy

$$
\exp(\theta(x)) = \exp(\beta_0(x) + \beta_1(x)x).
$$

Estymowany lokalny punkt przecięcia $\hat{\beta}_0(x)$ i lokalne nachylenie $\hat{\beta}_1(x)$ maksymalizują lokalną log-wiarygodność Poissona w $x$ daną przez

$$
L_x(\beta_0(x), \beta_1(x)) = \sum_{i=1}^n v_i(x) (-e_i \exp(\beta_0(x) + \beta_1(x)x_i) + y_i(\ln e_i + \beta_0(x) + \beta_1(x)x_i))
$$

na podstawie danych $(Y_i, x_i, e_i), i=1, 2, \dots, n$, gdzie $e_i$ to ekspozycja na ryzyko.

Tutaj, $\hat{\beta}_0(x)$ i $\hat{\beta}_1(x)$ rozwiązują

$$
\frac{\partial}{\partial \beta_0(x)} L_x = \sum_{i=1}^n v_i(x)(y_i - e_i \exp(\beta_0(x) + \beta_1(x)x_i)) = 0
$$

$$
\frac{\partial}{\partial \beta_1(x)} L_x = \sum_{i=1}^n v_i(x)x_i(y_i - e_i \exp(\beta_0(x) + \beta_1(x)x_i)) = 0.
$$

Ich wartość numeryczną można uzyskać za pomocą algorytmu IRLS, dokładnie tak jak w przypadku GLM. Zauważ, że w przypadku GAM, ponieważ wagi są przypisane do obserwacji w zależności od wartości $x$ będącej przedmiotem zainteresowania, algorytm IRLS musi być uruchamiany wielokrotnie dla każdej obserwacji o wartości $x$ będącej przedmiotem zainteresowania. Zatem może być uruchamiany wiele razy, a nie tylko raz, jak w GLM.

#### 6.3.1.4 Przykład z Wygładzaniem Śmiertelności

Podajmy przykład z ubezpieczeń na życie. Rozważmy statystyki śmiertelności modelowane za pomocą dwumianowego GLM opartego na formule Perks'a (4.15). Zamiast tylko jednego roku, rozważmy trzy kolejne lata i całą długość życia, od urodzenia do najstarszych wieków. Odpowiadające surowe roczne prawdopodobieństwa zgonu $\hat{q}_x$ są wyświetlane na Rys. 6.2, w trzech różnych kolorach dla trzech lat objętych badaniem.
Nawet jeśli ogólny kształt krzywej śmiertelności jest stabilny przez trzy kolejne lata objęte badaniem, roczne wahania pozostają. Ponieważ te nieregularne wahania nie ujawniają niczego o podstawowym wzorcu śmiertelności, powinny być usunięte przed wykonaniem jakichkolwiek obliczeń aktuarialnych. Wygładzanie (graduation) jest stosowane do usuwania losowych odchyleń od podstawowej gładkiej krzywej $x \mapsto q_x$. Ponieważ nie ma prostego modelu parametrycznego zdolnego do uchwycenia struktury wieku-śmiertelności, używamy modelu regresji GAM

$$
D_x \sim \mathcal{Bin}(L_x, q_x) \quad \text{z} \quad \ln \frac{q_x}{1 - q_x} = \theta(x)
$$

w celu wygładzenia surowych oszacowań $\hat{q}_x$, gdzie funkcja $\theta(\cdot)$ jest nieokreślona, ale zakłada się, że jest gładka.
Wkład obserwacji w wieku $x$ do log-wiarygodności wynosi

$$
l(D_x, l_x) = D_x \ln q_x + (l_x - D_x) \ln(1 - q_x) = D_x \ln \frac{q_x}{1 - q_x} + l_x \ln(1 - q_x).
$$

Lokalne wielomianowe przybliżenie dla $q_x$ jest trudne, ponieważ musi być spełniony warunek $0 \le q_x \le 1$. Dlatego wolimy pracować na skali logit, definiując nowy parametr za pomocą transformacji logit

$$
\theta(x) = \ln \frac{q_x}{1 - q_x}.
$$

Teraz $\theta(x)$ może przyjmować dowolną wartość rzeczywistą, gdy $q_x$ zmienia się od 0 do 1. Lokalna wielomianowa wiarygodność w $x$ wynosi wtedy

$$
\sum_{i=0}^{100} v_i(x) (D_i(\beta_0(x) + \beta_1(x)i) - l_i \ln(1 + \exp(\beta_0(x) + \beta_1(x)i))).
$$

Estymacja $q_x$ jest następnie uzyskiwana przez odwrócenie (6.4), czyli

$$
\hat{q}_x = \frac{\exp(\hat{\beta}_0(x) + \hat{\beta}_1(x)x)}{1 + \exp(\hat{\beta}_0(x) + \hat{\beta}_1(x)x)}.
$$

Rysunek 6.3 ilustruje tę procedurę. Przede wszystkim, globalne dopasowanie liniowe GLM nie oddaje struktury wiekowej tablicy trwania życia, co jest wyraźnie zilustrowane na lewym górnym panelu. Lokalny GLM postępuje w następujący sposób. Aby oszacować $q_5$, najpierw zbierzmy obserwacje bliskie wiekowi 5 (czerwona kropka na Rys. 6.3), które zostaną użyte w lokalnej estymacji $q_5$. Tutaj przyjmujemy $\lambda = 7$, więc

$$
\mathcal{V}(5) = \{2, 3, 4, 5, 6, 7, 8\}.
$$

Są to zielone kropki na Rys. 6.3. Następnie dopasowujemy dwumianowy GLM do 7 obserwacji w $\mathcal{V}(5)$, z których każda otrzymuje równą, jednostkową wagę. Daje to prostą linię, którą można zobaczyć jako linię styczną do prawdziwej tablicy trwania życia w wieku 5 lat. Ponieważ linia styczna dotyka krzywej w punkcie $x=5$, otrzymujemy dopasowane $\hat{q}_5$ reprezentowane jako niebieska kropka na Rys. 6.3.
Powtarzanie tej procedury w różnych wiekach dostarcza nam wyników przedstawionych na Rys. 6.4. Górny panel powtarza procedurę przeprowadzoną dla wieku 5 na Rys. 6.3 dla kilku innych wieków, podczas gdy dolny panel pokazuje wynikowe dopasowanie (z wyjątkiem trzech najmłodszych i trzech najstarszych wieków, ponieważ nie ma wystarczającej liczby obserwacji po lewej lub po prawej stronie, aby zastosować naszą podstawową strategię). Widzimy na dolnym panelu na Rys. 6.4, że to bardzo proste podejście już daje dość rozsądne wyniki, z wyjątkiem śmiertelności w wieku 0-15 lat, gdzie występuje duża zmienność w obserwacjach.

#### 6.3.1.5 Optymalna Szerokość Okna

Celem przy wyborze szerokości pasma jest uzyskanie estymacji, która jest jak najbardziej gładka, bez zniekształcania podstawowego wzorca zależności odpowiedzi od zmiennych objaśniających. Na przykład w zastosowaniu do wygładzania śmiertelności, oznacza to, że aktuariusz chce usunąć wszystkie nieregularne wahania prawdopodobieństw zgonu, ujawniając podstawową tablicę trwania życia. Oczywiście, szerokość pasma i stopień lokalnego wielomianu oddziałują na siebie. Dlatego ustalamy stopień lokalnego wielomianu i koncentrujemy się na wyborze szerokości pasma.
Liczba równoważnych stopni swobody dopasowania lokalnego stanowi uogólnienie liczby parametrów w podejściu parametrycznym. Istnieje kilka możliwych definicji (patrz Rozdz. 4.5.5) dla możliwego podejścia do obliczania równoważnej liczby stopni swobody), ale wszystkie wydają się mierzyć złożoność modelu. Intuicyjnie, reprezentują one liczbę parametrów potrzebnych do uzyskania tego samego dopasowania za pomocą parametrycznego GLM.

Optymalny parametr wygładzania $\lambda$ (lub $\alpha$) jest wybierany za pomocą walidacji krzyżowej. W przypadku normalnym, z odpowiedziami

$$
Y_i \sim \mathcal{Nor}(b(x_i), \sigma^2), \quad i=1, 2, \dots, n,
$$

dla pewnej nieznanej funkcji $b(\cdot)$, wydajność dopasowania jest oceniana za pomocą średniego kwadratowego błędu predykcji dla obserwacji spoza próby $(Y^*, x^*)$, czyli za pomocą empirycznej wersji

$$
MSEP(\hat{b}) = E[(Y^* - \hat{b}(x^*))^2].
$$

Ten wskaźnik został już rozłożony na trzy składniki w Rozdz. 4.5.5.

$$
MSEP(\hat{b}) = \text{Var}[\hat{b}(x^*)] + (b(x^*) - E[\hat{b}(x^*)])^2 + \text{Var}[Y^*]
$$

gdzie
- składnik obciążenia (bias)

$$
b(x^*) - E[\hat{b}(x^*)]
$$

mierzy błąd wprowadzony przez przybliżenie skomplikowanej funkcji $b$ za pomocą prostszego. Jest to różnica między prawdą a tym, co można odkryć za pomocą rozważanego modelu. Obciążenie staje się wysokie, gdy dane są niedopasowane.
- składnik wariancji

$$
\text{Var}[\hat{b}(x^*)]
$$

mierzy zmianę w estymacji, jeśli użyjemy innego zestawu danych treningowych. Jeśli model ma wysoką wariancję, to małe modyfikacje w danych treningowych mogą skutkować dużymi zmianami w dopasowaniu. Duża wariancja sugeruje nadmierne dopasowanie danych.
- ostatni składnik $\text{Var}[Y^*]$ często nazywany ryzykiem procesowym w ubezpieczeniach, jest stałą niezależną od procedury estymacji. Odpowiada za zmienność nieodłączną w badanych danych.

MSEP pojawia się zatem jako kompromis między obciążeniem a wariancją, powszechnie określany jako kompromis obciążenie-wariancja.

#### 6.3.1.6 Walidacja Krzyżowa

Estymacja MSEP metodą walidacji krzyżowej to

$$
CV(\hat{b}) = \frac{1}{n} \sum_{i=1}^n (y_i - \hat{b}^{(-i)}(x_i))^2
$$

gdzie $\hat{b}^{(-i)}(x_i)$ oznacza estymację $b(x_i)$ z pominięciem $x_i$. Optymalny parametr wygładzania $\alpha$ minimalizuje CV.

W praktyce każda $x_i$ jest kolejno usuwana ze zbioru danych, a estymacja regresji lokalnej $\hat{b}^{(-i)}$ jest obliczana na podstawie pozostałych $n-1$ punktów danych:

$$
\begin{array}{ccc}
\{ (x_2, y_2), (x_3, y_3), \dots, (x_n, y_n) \} & \rightarrow & \hat{b}^{(-1)} \\ & \rightarrow & \text{predykcja } \hat{y}_1 = \hat{b}^{(-1)}(x_1) \\
\{ (x_1, y_1), (x_3, y_3), \dots, (x_n, y_n) \} & \rightarrow & \hat{b}^{(-2)} \\ & \rightarrow & \text{predykcja } \hat{y}_2 = \hat{b}^{(-2)}(x_2) \\
\vdots & & \vdots \\
\{ (x_1, y_1), (x_2, y_2), \dots, (x_{n-1}, y_{n-1}) \} & \rightarrow & \hat{b}^{(-n)} \\ & \rightarrow & \text{predykcja } \hat{y}_n = \hat{b}^{(-n)}(x_n)
\end{array}
$$

Aby ułatwić obliczenia w dużych zbiorach danych, dostępne są aproksymacje kryterium CV, unikając ponownego dopasowywania modelu $n$ razy. Optymalny parametr wygładzania $\alpha$ jest następnie uzyskiwany poprzez przeszukiwanie siatki (grid search).

Dla odpowiedzi podlegających innym rozkładom z rodziny ED, przechodzimy do walidacji krzyżowej opartej na wiarygodności (lub dewiancji), zastępując estymację z pominięciem $x_i$ $\hat{\theta}^{(-i)}(x_i)$ w log-wiarygodności, czyli

$$
LCV(\hat{\theta}) = -2 \sum_{i=1}^n l(Y_i, \hat{\theta}^{(-i)}(x_i)),
$$

gdzie $\hat{\theta}(\cdot)$ obejmuje nieznaną funkcję $b(\cdot)$. Obliczanie estymacji z pominięciem $n$ obserwacji $\hat{\theta}^{(-i)}(x_i)$ może być czasochłonne. Opracowano aproksymacje dla $\hat{\theta}^{(-i)}(x_i)$, oparte na odpowiedniej generalizacji wpływów każdej obserwacji.

W praktyce (L)CV dostarcza graficznej pomocy w wyborze parametrów wygładzania. Konkretnie, (L)CV jest obliczane dla zakresu kandydatów na parametry wygładzania. Wykres (L)CV przedstawia liczbę równoważnych stopni swobody (edf) jako oś poziomą, a statystykę (L)CV jako oś pionową. Liczba edf dopasowania lokalnego stanowi uogólnienie liczby parametrów zaangażowanych w dopasowanie parametryczne. Odsyłamy czytelnika do Loader (1999, Rozdz. 4.4) w celu uzyskania precyzyjnej definicji edf w lokalnym estymowaniu wiarygodności. Pozwolenie na pojawienie się edf wzdłuż osi poziomej pomaga w interpretacji (zbyt mała liczba edf wskazuje na gładki model z bardzo małą elastycznością, zbyt duża liczba edf reprezentuje zaszumiony model) i można dodać porównywalność (do wykresu (L)CV można dodać różne stopnie wielomianów lub metody wygładzania). Optymalny parametr wygładzania $\alpha$ odpowiada minimum na wykresie (L)CV.
Należy podkreślić, że płaskie wykresy występują często; każdy model z wynikiem (L)CV bliskim minimum prawdopodobnie będzie miał podobną moc predykcyjną. W takim przypadku samo minimalizowanie (L)CV odrzuca istotne informacje dostarczane przez cały profil (L)CV. Jeśli (L)CV osiąga plateau po gwałtownym spadku, to parametr wygładzania odpowiadający początkowi plateau jest dobrym kandydatem na optymalną procentową liczbę punktów danych uwzględnionych w każdym lokalnym sąsiedztwie, dla którego szacowany jest przykładowy GLM.

### 6.3.2 Regresja Poissona dla danych zliczeniowych

Niech $Y_i$ będzie liczbą roszczeń zgłoszonych przez posiadacza polisy w wieku $i$, $i=1, 2, \dots, n$. Dane składają się z $(y_i, x_i, e_i)$, gdzie $e_i$ to odpowiednia ekspozycja na ryzyko. Model

$$
Y_i \sim \mathcal{Poi}(e_i \exp(b(x_i)))
$$

wydaje się dobrym punktem wyjścia do analizy danych zliczeniowych. Zakłada się, że funkcja $b$ jest gładka, ale w przeciwnym razie nieokreślona. Dla $t$ dostatecznie bliskiego $x$, $b$ jest aproksymowane przez funkcję liniową, tj. $b(t) \approx \beta_0(x) + \beta_1(x)t$. Wtedy $\beta_0(x)$ i $\beta_1(x)$ są estymowane za pomocą ważonej (lub lokalnej) metody największej wiarygodności, przypisując większe wagi $v_i(x)$ obserwacjom $(y_i, x_i, e_i)$ odpowiadającym wiekom $x_i$ bliskim $x$. Zauważ, że to, co jest tu istotne, to nie bliskość $x$ do obserwowanych $x_i$, ale do tych, które są dostępne. W rzeczywistości ta sama procedura ma zastosowanie, niezależnie od tego, czy obserwacja dla faktu $x$ jest zawarta w dostępnej bazie danych, czy nie. Najpierw identyfikujemy obserwacje bliskie interesującemu nas wiekowi $x$, przypisujemy im wagi i dopasowujemy lokalny GLM.

Walidacja krzyżowa pozwala aktuariuszowi wybrać optymalne $\alpha$, faworyzując jakość predykcji w stosunku do dobroci dopasowania. Niech $\hat{\beta}_0^{(-i)}(x_i)$ i $\hat{\beta}_1^{(-i)}(x_i)$ będą estymowanymi $\beta_0(x_i)$ i $\beta_1(x_i)$ na podstawie zredukowanych danych $(y_j, x_j, e_j), j \neq i$. Optymalna szerokość pasma jest wybierana na podstawie walidacji krzyżowej wiarygodności

$$\begin{align*}
LCV =& -2 \sum_{i=1} (-e_i \exp(\hat{\beta}_0^{(-i)}(x_i) + \hat{\beta}_1^{(-i)}(x_i)x_i) \\
&+ y_i(\ln e_i + \hat{\beta}_0^{(-i)}(x_i) + \hat{\beta}_1^{(-i)}(x_i)x_i)) \\
&+ \text{stała}.
\end{align*}$$

Jest to procedura z Przykładu 6.2.2. Lewy dolny panel na Rys. 6.1 wyświetla wykres LCV. Wybieramy optymalne $\alpha$ z plateau widocznego w prawej części wykresu LCV, odpowiadające $\lambda = 19$ (co oznacza, że bierzemy 9 lat na lewo i 9 lat na prawo od wieku będącego przedmiotem zainteresowania, w centralnej części danych).

#### 6.3.1.7 Zastosowanie do Wygładzania Śmiertelności

Nasze naiwne podejście dało dopasowanie widoczne na dolnym panelu Rys. 6.4. Dane pary $(y_i, l_i)$, gdzie $y_i$ to obserwowana liczba zgonów w wieku $i$ wśród $l_i$ osób w wieku $i$, są modelowane w następujący sposób:

$$
Y_i \sim \mathcal{Bin}(l_i, q_i) \quad \text{gdzie} \quad q_i = \frac{\exp(b(i))}{1 + \exp(b(i))}
$$

dla pewnej gładkiej, nieokreślonej funkcji $b$. Aby uchwycić dużą krzywiznę obserwowanych danych dla wieków 0-15, używamy lokalnego dwumianowego GLM. Oznacza to, że dla $t$ dostatecznie bliskiego $x$, aproksymujemy $b$ za pomocą wielomianu kwadratowego:

$$
b(t) \approx \beta_0(x) + \beta_1(x)t + \beta_2(x)t^2.
$$

Następnie współczynniki regresji $\beta_0(x), \beta_1(x)$ i $\beta_2(x)$ są estymowane za pomocą ważonej (lub lokalnej) metody największej wiarygodności, przypisując większe wagi $w_i(x)$ obserwacjom $(y_i, l_i)$ odpowiadającym wiekom $i$ bliskim $x$.

Użyjmy LCV do wyboru optymalnego $\alpha$. Niech $\hat{\beta}_0^{(-i)}(i), \hat{\beta}_1^{(-i)}(i)$ i $\hat{\beta}_2^{(-i)}(i)$ będą estymowanymi $\beta_0(i), \beta_1(i)$ i $\beta_2(i)$ na podstawie zredukowanych danych $\{(y_j, l_j), j \neq i\}$. Optymalna szerokość pasma jest wybierana na podstawie walidacji krzyżowej wiarygodności

$$\begin{align*}
LCV =& -2 \sum_{i=0}^{100} (y_i(\hat{\beta}_0^{(-i)}(i) + \hat{\beta}_1^{(-i)}(i)i + \hat{\beta}_2^{(-i)}(i)i^2) \\
&- l_i \ln(1 + \exp(\hat{\beta}_0^{(-i)}(i) + \hat{\beta}_1^{(-i)}(i)i + \hat{\beta}_2^{(-i)}(i)i^2))) \\
&+ \text{stała}.
\end{align*}$$

Rysunek 6.5, lewy górny panel, wyświetla wykres LCV dla różnych edf odpowiadających wartościom $\alpha$ między 5% a 50%, co 3%. Optymalna wartość dla $\alpha$ znajduje się na początku plateau. Wynosi ona około 20%. Wynikowe dopasowanie jest wyświetlane na prawym górnym panelu. Widzimy, że lokalny kwadratowy dwumianowy GLM jest w stanie uchwycić kształt śmiertelności we wszystkich wiekach. Wyświetlamy również reszty dewiancji na lewym dolnym panelu. Widzimy, że z wyjątkiem bardzo młodych wieków, model dość dokładnie oddaje strukturę danych. Wreszcie, wynikowa tablica trwania życia otoczona 95% przedziałami predykcji jest wyświetlana na prawym dolnym panelu. Widzimy, że margines błędu staje się dość znaczny w starszych wiekach (w młodych wiekach prawdopodobieństwa zgonu są tak małe, że trudno cokolwiek zobaczyć).

**Uwaga 6.3.3** Można by również użyć adaptacyjnego wygładzania, aby uzyskać odpowiedni stopień wygładzenia w różnych wiekach (zazwyczaj zmienność jest wyższa w młodym i starym wieku, co wymaga większego wygładzenia w porównaniu do wieków średnich, gdzie śmiertelność jest znacznie bardziej stabilna). W przypadku adaptacyjnego wygładzania, okno wygładzania (lub procent $\alpha$) jest dostosowywane do lokalnej zmienności obecnej w danych, zamiast być stałe dla wszystkich wieków.

#### 6.3.1.8 Zastosowanie do Taryfikacji Geograficznej

Modele przestrzenne do taryfikacji ubezpieczeniowej dostarczają aktuariuszom metody analizy zróżnicowania ryzyka według obszaru geograficznego. Są one obecnie powszechne w ubezpieczeniach majątkowych, w tym ubezpieczeniach komunikacyjnych i domowych. Zmienność przestrzenna może być związana z czynnikami geograficznymi (np. bliskość remizy strażackiej) lub społeczno-demograficznymi (być może wpływającymi na wskaźniki włamań w ubezpieczeniach domowych). Ubezpieczenie za potencjalne polisy zależy od ich lokalizacji geograficznej (zawartej w kodzie pocztowym) oprócz standardowych czynników.
Co więcej, charakterystyka roszczeń ma tendencję do bycia podobną w sąsiednich obszarach kodów pocztowych (po uwzględnieniu innych czynników). Idea modeli geograficznych polega na wykorzystaniu tej przestrzennej gładkości i umożliwieniu transferu informacji do i z sąsiednich regionów. Wygodnym rozwiązaniem do estymacji geograficznych wariacji jest uciekanie się do regresji lokalnej GLM w celu oszacowania efektu geograficznego dla każdego obszaru, w oparciu o doświadczenie ubezpieczyciela.

Jako przykład, rozważmy dane przedstawione na Rys. 6.6. Lewy górny panel pokazuje średnią liczbę roszczeń obserwowanych w każdym dystrykcie, po uwzględnieniu wszystkich innych cech dostępnych w bazie danych. Na płaszczyźnie naniesiono punkty danych w zależności od szerokości i długości geograficznej środka każdego dystryktu. Bliskość jest oceniana na podstawie odległości euklidesowej. Wykres LCV jest trudniejszy do zinterpretowania, ale sugeruje użycie raczej małej proporcji $\alpha$ dla lokalnego dopasowania wielomianowego. Wynikowe dopasowanie jest wyświetlone na lewym dolnym panelu Rys. 6.6. Ujawnia on typowe geograficzne wariacje oczekiwanych roszczeń z tytułu OC komunikacyjnego. Wygładzone efekty przestrzenne wyraźnie uwidaczniają wyższe oczekiwane częstotliwości roszczeń w obszarach miejskich: Brukseli (w środku), Antwerpii (na północy) i Liège (na zachodzie), trzech największych miast w Belgii. Reszty dewiancji są również mapowane i nie ujawniają żadnego pozostałego wzorca przestrzennego.

#### 6.3.1.9 Zastosowanie do Powierzchni Śmiertelności

Dynamiczna analiza śmiertelności często opiera się na modelowaniu powierzchni śmiertelności przedstawionej na Rys. 6.7, górny panel. Śmiertelność jest tam badana w wymiarze wiekowo-okresowym. Oznacza to, że używane są dwie wymiary: wiek i czas kalendarzowy. Obie, wiek i czas kalendarzowy, są zmiennymi dyskretnymi. W dyskretnych terminach, osoba w wieku $x, x=0, 1, 2, \dots$, ma dokładny wiek zawarty między $x$ a $x+1$. Jest to również znane jako „wiek w dniu ostatnich urodzin” (tj. wiek w latach, zaokrąglony w dół do najbliższej liczby całkowitej). Podobnie, zdarzenie, które ma miejsce w roku kalendarzowym $t$, ma miejsce w przedziale czasu $[t, t+1)$. Teraz, $q_x(t)$ jest prawdopodobieństwem, że $x$-letnia osoba w roku kalendarzowym $t$ umrze przed osiągnięciem wieku $x+1$.

Niech $l_{xt}$ będzie liczbą osób w wieku $x$ w dniu ostatnich urodzin na dzień 1 stycznia roku $t$, a $Y_{xt}$ będzie liczbą zgonów zarejestrowanych w wieku $x$ w dniu ostatnich urodzin w ciągu roku kalendarzowego $t$, wśród tych $l_{xt}$ osób. Jednoroczne prawdopodobieństwo zgonu $q_x(t)$ można wtedy oszacować jako $Y_{xt}/l_{xt}$.
Powierzchnia śmiertelności składa się z trójwymiarowego wykresu logitu $\hat{q}_x(t)$ widzianego jako funkcja zarówno wieku $x$, jak i czasu $t$. Ustalając wartość $t$, rozpoznajemy klasyczny kształt krzywej śmiertelności widoczny na Rys. 6.2. W szczególności, wzdłuż przekrojów powierzchni śmiertelności, gdy $t$ jest stałe (lub wzdłuż przekątnych, gdy śledzone są kohorty), obserwuje się stosunkowo wysoką śmiertelność w okolicach urodzenia, dobrze znaną obecność dołka w wieku około 10 lat, grzbietu we wczesnych latach 20. (który jest mniej wyraźny dla kobiet) oraz wzrostu w średnim i starszym wieku.
W celu usunięcia nieregularnych wahań z surowej powierzchni śmiertelności, wykonujemy lokalne dopasowanie dwumianowe GLM. Stopień gładkości jest wybierany na podstawie wykresu LCV, który wyraźnie pokazuje plateau. Odpowiadające dopasowanie wyświetlone na dolnym panelu ujawnia podstawowy, gładki wzorzec śmiertelności.

### 6.3.2 Splajny (Funkcje Sklejane)

Oprócz lokalnych GLM, splajny (funkcje sklejane) stanowią drugą klasyczną metodę włączania nieliniowych efektów ciągłych czynników ryzyka w skali punktowej. Splajny to wielomiany kawałkami określone przez wybór zestawu węzłów.

#### 6.3.2.1 Wielomiany Kawałkami

Elastycznym podejściem do modelowania nieliniowych efektów $b_j$ zawartych w wynikach GAM jest stosowanie wielomianów kawałkami. Przypomnijmy, że grupowanie polega na zastąpieniu nieznanej funkcji $b_j$ przez stałą kawałkami, aby ułatwić estymację. Odbywa się to poprzez podział dziedziny $j$-tej cechy na pod-przedziały i zakładając, że funkcja do oszacowania jest stała na każdym z nich. W praktyce oznacza to, że aktuariusz dopasowuje wielomian 0-stopnia oddzielnie na każdym pod-przedziale. Podział jest określony przez interwały graniczne, zwane węzłami. Splajny rozszerzają to podstawowe podejście, pozwalając na wielomiany wyższego stopnia, które ładnie łączą się w każdym z pod-przedziałów (w węzłach), a tym samym wybierając odpowiednie węzły partycjonujące dziedzinę cechy ciągłej.
Dokładnie, nieznane funkcje $b_j$ są aproksymowane przez wielomiany splajnowe, które mogą być uważane za wielomiany kawałkami z dodatkowymi warunkami regularności. W przypadku splajnów, ideą jest uwzględnienie reszty z rozwinięcia Taylora. Zaczynając od wzoru

$$
b(x) = b(0) + b'(0)x + b''(0)\frac{x^2}{2} + b'''(0)\frac{x^3}{6} + \int_0^x b''''(t)\frac{(x-t)^3}{6}dt
$$

naturalne jest użycie przybliżenia

$$
b(x) \approx \beta_0 + \beta_1 x + \beta_2 \frac{x^2}{2} + \beta_3 \frac{x^3}{6} + \sum_{j=1}^{p_\xi} \beta_j \frac{(x-\xi_j)_+^3}{6}.
$$

$\xi_j$ wchodzące w ostatni wzór nazywane są węzłami. Przybliżenie po prawej stronie (6.7) wydaje się być wielomianem ciągłym między dwoma kolejnymi węzłami $\xi_j$ i $\xi_{j+1}$. Jest ciągłe i posiada ciągłe pierwsze i drugie pochodne w każdym węźle $\xi_j$.
Ideą jest zastąpienie nieznanego wkładu $b(\cdot)$ do wyniku jego przybliżeniem (6.7). W ten sposób odzyskujemy wynik GLM z początkową cechą $x$ i jej transformacjami $x^2, x^3$ oraz $(x-\xi_j)_+^3, j=1, 2, \dots, p_\xi$. Algorytm IRLS może być następnie użyty do oszacowania nieznanych parametrów $\beta_j$ wchodzących w (6.7) w odpowiednim GLM.
Przybliżenie (6.7) do $b$ jest funkcją wielomianową kawałkami: dziedzina $x$ jest podzielona na pod-przedziały przez węzły $\xi_j$, a przybliżenie polega na dopasowaniu innego wielomianu sześciennego na każdym przedziale. Wynikowa funkcja, znana jako splajn sześcienny, jest ciągła i ma ciągłe pierwsze i drugie pochodne w węzłach. Zauważ, że narzucenie jeszcze jednego rzędu ciągłości doprowadziłoby do globalnego wielomianu sześciennego. Ponieważ splajny sześcienne są splajnami najniższego rzędu, dla których nieciągłość węzłów nie jest widoczna dla ludzkiego oka, rzadko jest dobry powód, aby wykraczać poza splajny sześcienne.

#### 6.3.2.2 Splajny Sześcienne (Kubiczne)

Ograniczone rozwinięcie Taylora uzasadnia użycie splajnów: splajny są wielomianami kawałkami połączonymi w węzłach, aby stworzyć jedną gładką krzywą. Podstawową ideą splajnów sześciennych jest zastąpienie ciągłej cechy $x$ nowymi, które są transformacjami $x$, a mianowicie $x, x^2, x^3$ oraz $(x-\xi_j)_+^3$, w oparciu o rozwinięcie Taylora. Maszyneria GLM stosuje się następnie do tego nowego zestawu cech.
Gdy są używane do dopasowywania danych, wielomiany mają tendencję do bycia nieregularnymi w pobliżu granic, a ekstrapolacja może być bezsensowna. To niepożądane zachowanie jest jeszcze bardziej zaostrzone w przypadku splajnów. Dlatego naturalne splajny sześcienne dodają dalsze ograniczenia, a mianowicie, że funkcja jest liniowa poza granicznymi węzłami. Rozważmy model regresji normalnej

$$
Y_i \sim \mathcal{Nor}(b(x_i), \sigma^2), \quad i=1, 2, \dots, n,
$$

dla pewnej nieznanej funkcji $b(\cdot)$. Naturalne splajny sześcienne z węzłami w każdej obserwacji $x_i$, to znaczy funkcja (6.7) z $p_\xi=n$ i $\xi_j=x_j$, są rozwiązaniami następującego problemu optymalizacji: spośród wszystkich funkcji $b$ z ciągłymi drugimi pochodnymi, znaleźć tę, która minimalizuje ukarane resztkowe sumy kwadratów

$$
\sum_{i=1}^n (y_i - b(x_i))^2 + \lambda \int (b''(x))^2 dx
$$

gdzie wartości cechy ciągłej są zakładane w porządku rosnącym, tj. $x_1 < x_2 < \dots < x_n$. Penalizowana wersja celu najmniejszych kwadratów równoważy dobroć dopasowania i gładkość estymowanego $b$: pierwszy składnik narzuca zgodność z danymi, podczas gdy drugi penalizuje krzywiznę (zauważ, że $b''=0$, jeśli $b$ jest liniowe). Tutaj $\lambda \ge 0$ jest parametrem wygładzania kontrolującym kompromis między dwoma terminami, dobrocią dopasowania i karą za chropowatość. Dwa skrajne przypadki to:
- $\lambda=0$ nie narzuca żadnych ograniczeń, a wynikowa estymacja interpoluje dane (w rzeczywistości $b$ jest dowolną funkcją interpolującą dane).
- $\lambda \rightarrow \infty$ sprawia, że krzywizna jest niemożliwa, więc wracamy do regresji liniowej $b(x) = \beta_0 + \beta_1 x$, która spełnia $b''=0$.

Zatem duża $\lambda$ odpowiada gładkiej funkcji $b$.

Co ciekawe, minimalizator jest skończenie wymiarowy, chociaż cel jest minimum w nieskończenie wymiarowej przestrzeni funkcji. Dokładnie, unikalnym minimalizatorem $\hat{b}_\lambda$ jest naturalny splajn sześcienny z węzłami w wartościach $x_i$, to znaczy jest to forma (6.7) z $p_\xi=n$ i $\xi_j=x_j$. Oznacza to, że $\hat{b}_\lambda$ jest wielomianem sześciennym na każdym przedziale $[x_i, x_{i+1}]$ takim, że $\hat{b}''_\lambda(x_i) = \hat{b}''_\lambda(x_n) = 0$. Wiedząc, że rozwiązaniem jest naturalny splajn sześcienny, można to ułatwić, korzystając z pomocy GLM, używając reprezentacji

$$
\hat{b}_\lambda(x) = \sum_{j=1}^n \beta_j B_j(x)
$$

gdzie funkcje $B_j(\cdot)$ tworzą bazę dla takich splajnów. Problem sprowadza się wtedy do dopasowania zwykłej liniowej regresji z przekształconymi cechami $B_j(x)$. Oznaczając jako $\boldsymbol{B}$ macierz projektu z kolumnami $(B_j(x_1), \dots, B_j(x_n))$ i definiując macierz $\boldsymbol{\Omega}$ z elementem $(j,k)$ danym przez

$$
\Omega_{jk} = \int B_j''(z)B_k''(z)dz,
$$

rozwiązanie $\boldsymbol{\beta}$ minimalizuje $\|\boldsymbol{Y} - \boldsymbol{B}\boldsymbol{\beta}\|^2 + \lambda \boldsymbol{\beta}^T \boldsymbol{\Omega} \boldsymbol{\beta}$, więc

$$
\hat{\boldsymbol{\beta}} = (\boldsymbol{B}^T \boldsymbol{B} + \lambda \boldsymbol{\Omega})^{-1} \boldsymbol{B}^T \boldsymbol{Y}.
$$

W porównaniu do wzoru (4.11), widzimy, że $\lambda \boldsymbol{\Omega}$ jest dodawane do $\boldsymbol{B}^T \boldsymbol{B}$ w wyrażeniu $\boldsymbol{\hat{\beta}}$. Jest to ściśle związane z regresją grzbietową (omówioną w Rozdz. 5). Macierz $\lambda \boldsymbol{\Omega}$ służy jako grzbiet, lub macierz kurcząca, tak że estymacje $\boldsymbol{\hat{\beta}}$ są skurczone do zera. Wynika to z faktu, że dla dużej $\lambda$, macierz $(\boldsymbol{B}^T \boldsymbol{B} + \lambda \boldsymbol{\Omega})^{-1}$ staje się mała. Ponadto, suma kwadratów reszt może być zastąpiona log-wiarygodnością dowolnego rozkładu ED, gdy rozważane są odpowiedzi nienormalne.
Dopasowane wartości $\hat{Y}_i = \hat{b}_\lambda(x_i)$ można uzyskać jako

$$
\boldsymbol{\hat{Y}} = \boldsymbol{S}_\lambda \boldsymbol{Y} \quad \text{z} \quad \boldsymbol{S}_\lambda = \boldsymbol{B}(\boldsymbol{B}^T \boldsymbol{B} + \lambda \boldsymbol{\Omega})^{-1} \boldsymbol{B}^T
$$

złożoność dopasowania splajnu można wywnioskować z równoważnych stopni swobody $\text{edf} = \text{Tr}(\boldsymbol{S}_\lambda)$. Intuicyjnie, dopasowanie splajnu z edf=5 jest tak złożone, jak globalny wielomian stopnia 4 (który ma 5 parametrów, wliczając punkt przecięcia).

#### 6.3.2.3 P-Splajny, Kary Wygładzające

Gdy węzły $\zeta_j$ są ustalone, mamy splajn regresyjny: aktuariusz musi wybrać liczbę i lokalizację tych węzłów, a następnie maszyneria GLM ma zastosowanie. Węzły są generalnie umieszczane w kwantylach cechy $x$, a nie przy każdej obserwowanej wartości $x_i$. W prostym podejściu do splajnów regresyjnych, współczynniki regresji $\boldsymbol{\beta}_j$ są estymowane przy użyciu standardowych algorytmów dla maksymalnej wiarygodności w GLM, takich jak algorytm IRLS.
Kluczowym punktem jest wybór liczby węzłów. Ponownie stajemy przed kompromisem między obciążeniem a wariancją, używając niewielu węzłów skutkuje restrykcyjną klasą estymatorów (a więc potencjalnie z wysokim obciążeniem), podczas gdy użycie wielu węzłów może prowadzić do nadmiernie dopasowanych danych (a więc niestabilnych estymacji, tj. estymatora o dużej wariancji). Przy małej liczbie węzłów, wynikowy splajn może nie być wystarczająco elastyczny, aby uchwycić zmienność danych. Przy dużej liczbie węzłów, estymowane krzywe mają tendencję do nadmiernego dopasowywania danych i, w rezultacie, uzyskuje się zbyt chropowate dopasowania.

Aby przezwyciężyć trudności związane z prostymi splajnami regresyjnymi, stało się powszechną praktyką narzucanie kar wygładzających na współczynniki regresji zaangażowane w dekompozycję splajnu. To jest tak zwane podejście P(enalized)-splines. Kluczowa idea stojąca za P-splajnami jest następująca. Po pierwsze, zdefiniuj umiarkowanie dużą liczbę równo rozmieszczonych węzłów (zwykle między 20 a 40), aby zapewnić wystarczającą elastyczność w dopasowaniu. Po drugie, zdefiniuj karę za chropowatość opartą na sumie kwadratów różnic drugiego rzędu współczynników $\beta_j$ powiązanych z funkcjami bazowymi $B_j(\cdot)$, aby zagwarantować wystarczającą gładkość dopasowanej krzywej. Prowadzi to do podejścia z ukaranej wiarygodności, w którym funkcja celu jest kompromisem między dobrocią dopasowania a gładkością. Dokładnie, łączymy log-wiarygodność $L$ odpowiadającą pewnej dystrybucji ED z karą za chropowatość, aby otrzymać

$$
PL(\beta_0, \beta_1, \dots, \beta_m) = L(\beta_0, \beta_1, \dots, \beta_m) - \lambda \boldsymbol{\beta}^T \boldsymbol{\Delta} \boldsymbol{\beta}
$$

gdzie $\lambda$ jest parametrem wygładzania kontrolującym kompromis między dopasowaniem do danych a gładkością, podczas gdy $\boldsymbol{\beta}^T \boldsymbol{\Delta} \boldsymbol{\beta}$ jest karą za chropowatość. Różnice między sąsiednimi wartościami $\beta_j$ uzyskuje się z

$$
\boldsymbol{P}\boldsymbol{\beta} = \begin{pmatrix} \beta_2 - \beta_1 \\ \beta_3 - \beta_2 \\ \vdots \end{pmatrix} \quad \text{z} \quad \boldsymbol{P} = \begin{pmatrix} -1 & 1 & 0 & \dots \\ 0 & -1 & 1 & 0 \dots \\ \vdots & \vdots & \vdots & \ddots \end{pmatrix}.
$$

Stąd,

$$
\sum_{j \ge 1} (\beta_{j+1} - \beta_j)^2 = \boldsymbol{\beta}^T \boldsymbol{P}^T \boldsymbol{P} \boldsymbol{\beta}
$$

jako

$$
\boldsymbol{P}^T \boldsymbol{P} = \begin{pmatrix} 1 & -1 & 0 & 0 \dots \\ -1 & 2 & -1 & 0 \dots \\ 0 & -1 & 2 & -1 \dots \\ \vdots & \vdots & \vdots & \ddots \end{pmatrix}.
$$

Algorytm IRLS można dostosować do uzyskania estymowanych współczynników regresji $\beta_0, \beta_1, \dots, \beta_m$, optymalizując ukarana log-wiarygodność. Walidacja krzyżowa może być następnie użyta do wyboru optymalnego $\lambda$. Warto zauważyć, że ukarana log-wiarygodność $\lambda \boldsymbol{\beta}^T \boldsymbol{\Delta} \boldsymbol{\beta}$ może być postrzegana jako log-wiarygodność uzyskana przez przyjęcie, że $\beta$ jest wielowymiarowym rozkładem normalnym o zerowej średniej i macierzy wariancji-kowariancji $(\lambda \boldsymbol{\Delta})^{-1}$. Jest to w istocie model mieszany, taki jak te rozważane w Rozdz. 5.

### 6.3.3 Rozszerzenie na Wiele Cech Ciągłych

#### 6.3.3.1 Algorytm Backfitting

Parametry regresji $\beta_0, \beta_1, \dots, \beta_{p_{cat}}$ oraz funkcje $b_j$ muszą być oszacowane na podstawie dostępnych danych. Dopasowanie GLM zostało wykonane za pomocą algorytmu IRLS. Podobnie, dopasowanie GAM jest wykonywane jako sekwencja dopasowań w normalnym addytywnym modelu regresji, oparta na działających odpowiedziach. W każdym kroku algorytm backfitting może być użyty do oszacowania nieznanych funkcji $b_j$.
Backfitting przebiega w następujący sposób. Wszystkie funkcje $b_k, k \neq j$, są umieszczane w offsecie, a pozostała funkcja $b_j$ jest estymowana za pomocą lokalnej dekompozycji wiarygodności lub splajnu, jak wyjaśniono powyżej. Algorytm iteracyjny można opisać w następujący sposób. Najpierw ustawiamy wszystkie funkcje $b_j$ na zero i następnie powtarzamy następujące kroki aż do zbieżności:

Krok 1: oszacuj $\beta_0, \beta_1, \dots, \beta_{p_{cat}}$ w GLM ze średnią odpowiedzią

$$
g^{-1}\left( \beta_0 + \sum_{j=1}^{p_{cat}} x_{ij} \right),
$$

to jest, tylko z cechami kategorialnymi i offsetami

$$
\text{offset}_i = \sum_{j=p_{cat}+1}^p b_j(x_{ij}).
$$

Krok 2: dla $j = p_{cat}+1, \dots, p$, oszacuj $b_j$ w GAM ze średnią odpowiedzią

$$
g^{-1}(b_j(x_{ij})),
$$

to jest, tylko z $j$-tą cechą ciągłą i offsetem

$$
\text{offset}_i = \beta_0 + \sum_{j=1}^{p_{cat}} x_{ij} + \sum_{k=p_{cat}+1, k \neq j}^p b_k(x_{ik}).
$$

Czasami funkcje są zmuszone do bycia liniowymi w kroku początkowym, tak że dopasowanie GAM zaczyna się od odpowiadającego dopasowania GLM. Ponadto, liniowy efekt każdej cechy może być włączony do części GLM wyniku i ponownie estymowany w kroku 1. W takim przypadku funkcje $b_j$ szacują odchylenie od liniowości dla każdej cechy ciągłej włączonej do wyniku.
Ważne jest, aby zauważyć, że możemy używać różnych wygładzaczy dla każdej cechy ciągłej zaangażowanej w wynik addytywny, takich jak wielomiany lokalne lub splajny, z różnymi ilościami wygładzania.

#### 6.3.3.2 Estymacja Największej Wiarygodności z Karą

W podejściu maksymalizacji wiarygodności z karą, log-wiarygodność jest uzupełniana o karę dla każdej gładkiej funkcji $b_j$, karząc jej nieregularne zachowanie. Aby kontrolować kompromis między gładkością a przyleganiem do danych, każda kara jest mnożona przez powiązany parametr wygładzania, który należy wybrać z danych.
Zdefiniuj partycję zakresu $j$-tej cechy na $r_j$ nienakładających się interwałów. Wtedy $b_j$ jest zapisywane w postaci kom\mathcal{bin}acji liniowej $m_j = r_j + l_j$ funkcji bazowych $B_{jk}^l$, tj.

$$
b_j(x_j) = \sum_{k=1}^{m_j} \beta_{jk} B_{jk}^l(x_j).
$$

Wektor ewaluacji funkcji

$$
\boldsymbol{b}_j = (b_j(x_{1j}), \dots, b_j(x_{nj}))^T
$$

można zapisać jako iloczyn macierzowy macierzy projektu $\boldsymbol{X}_j$ i wektora współczynników regresji $\boldsymbol{\beta}_j = (\beta_{j1}, \dots, \beta_{jm_j})^T$, tj.

$$
\boldsymbol{f}_j = \boldsymbol{X}_j \boldsymbol{\beta}_j.
$$

Macierz $n \times m_j$ projektu $\boldsymbol{X}_j$ składa się z funkcji bazowych $B_{jk}^l$ ocenianych w obserwacjach. Dokładnie, element w wierszu $i$ i kolumnie $k$ macierzy $\boldsymbol{X}_j$ jest dany przez $B_{jk}^l(x_{ij})$.
Parametry regresji są następnie estymowane przez maksymalizację ukaranej log-wiarygodności postaci

$$
PL(\boldsymbol{\beta}_1, \dots, \boldsymbol{\beta}_p) = L(\boldsymbol{\beta}_1, \dots, \boldsymbol{\beta}_p) - \lambda_1 \sum_{k=d+1}^{m_1} (\Delta^d \beta_{1k})^2 - \dots - \lambda_p \sum_{k=d+1}^{m_p} (\Delta^d \beta_{pk})^2.
$$

Tutaj $PL$ jest maksymalizowane względem $\boldsymbol{\beta}_1, \dots, \boldsymbol{\beta}_p$. Indeks $d$ wskazuje rząd różnic. Zwykle używa się $d=1$ lub $d=2$, co prowadzi odpowiednio do pierwszych różnic $\beta_{j,k} - \beta_{j,k-1}$ lub drugich różnic $\beta_{j,k} - 2\beta_{j,k-1} + \beta_{j,k-2}$. Kompromis między wiernością danych (rządzoną przez składnik wiarygodności) a gładkością (rządzoną przez składniki kary $\lambda_j$) jest kontrolowany przez parametry wygładzania $\lambda_j$. Im większe parametry wygładzania, tym gładsze wynikowe dopasowanie. W granicy ($\lambda_j \rightarrow \infty$) otrzymujemy wielomian, którego stopień zależy od rzędu kary różnicowej i stopnia splajnu. Na przykład dla $d=2$ i $l_j=3$ (najczęściej używana kom\mathcal{bin}acja) granicą jest dopasowanie liniowe. Wybór parametrów wygładzania jest kluczowy, ponieważ możemy uzyskać zupełnie inne dopasowania, zmieniając parametry wygładzania $\lambda_j$. Parametry wygładzania można wybrać za pomocą walidacji krzyżowej lub za pomocą kryteriów, takich jak uogólniona walidacja krzyżowa (GCV), nieobciążona estymacja ryzyka (UBRE) lub AIC, które unikają ponownego dopasowywania modelu kilka razy w celu przewidywania punktów danych spoza próby.