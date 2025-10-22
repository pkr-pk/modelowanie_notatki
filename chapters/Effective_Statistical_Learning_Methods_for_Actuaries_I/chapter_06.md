# Rozdział 6 Uogólnione Modele Addytywne (GAM)

## 6.1 Wprowadzenie

Widzieliśmy w rozdziale 4, że GLM oferują skuteczne rozwiązanie wielu problemów w naukach aktuarialnych. Jednakże założenie o liniowości dotyczące scoringu jest czasami wątpliwe: nieliniowe efekty ciągłych cech, takich jak wiek ubezpieczającego, mogą wymagać uwzględnienia w scoringu, aby odzwierciedlić rzeczywiste doświadczenie. Cechy ciągłe mogą efektywnie wchodzić do GLM tylko wtedy, gdy są odpowiednio przekształcone, aby odzwierciedlić ich prawdziwy wpływ na scoring. Niestety, rzadko jest jasne, jak zmienne powinny być przekształcone przed włączeniem ich do scoringu. Powszechną praktyką w firmach ubezpieczeniowych jest modelowanie nieliniowych efektów za pomocą wielomianów. Jednak wielomiany niskiego stopnia często nie są wystarczająco elastyczne, aby uchwycić strukturę obecną w danych, podczas gdy zwiększanie ich stopnia powoduje niestabilne oszacowania, zwłaszcza dla ekstremalnych wartości cech.

Inne podejście powszechnie stosowane w praktyce polega na zastąpieniu cechy ciągłej cechą kategorialną, uzyskaną przez podzielenie jej dziedziny na umiarkowaną liczbę podprzedziałów i założenie, że cecha ma stały wpływ na scoring w każdym przedziale. Oznacza to, że funkcja dająca efekt cechy ciągłej na scoring jest zakładana jako odcinkowo stała. Takie grupowanie oczywiście skutkuje utratą informacji. Ponadto nie ma ogólnej zasady określającej optymalne punkty odcięcia, więc grupowanie może obciążać ocenę ryzyka.

Uogólnione Modele Addytywne (GAM) weszły do zestawu narzędzi aktuariusza, aby radzić sobie z cechami ciągłymi w sposób elastyczny. W tym ujęciu cechy ciągłe wchodzą do modelu w semi-parametrycznym predyktorze addytywnym. W ubezpieczeniach majątkowych i osobowych, wpływ wieku ubezpieczającego, mocy pojazdu lub sumy ubezpieczonej można modelować za pomocą GAM. GAM pozwalają również aktuariuszowi analizować zmiany ryzyka według obszaru geograficznego, uwzględniając możliwą interakcję między cechami ciągłymi (szerokość i długość geograficzna dające położenie przestrzenne, w tym przypadku). Inne interakcje ogólnie obecne w danych obejmują wiek i moc, a także płeć i wiek w ubezpieczeniach komunikacyjnych. Mogą one być również uchwycone przez GAM.

W ubezpieczeniach na życie statystyki śmiertelności są często agregowane dla grup ubezpieczonych o tych samych cechach (takich jak wiek, płeć czy rodzaj produktu), aby przeprowadzić analizę GLM przy użyciu regresji Poissona. Liczby zgonów pochodzące z danych ekspozycji mogą być następnie modelowane przy użyciu regresji Poissona. Kluczowym argumentem teoretycznym legitymizującym regresję Poissona dla GLM jest to, że przy łagodnych założeniach prawdopodobieństwo Poissona wydaje się być proporcjonalne do prawdziwego prawdopodobieństwa. Dlatego wnioskowanie statystyczne oparte na prawdopodobieństwie Poissona nie jest restrykcyjne. Jest to szczególnie interesujące z praktycznego punktu widzenia, ponieważ pozwala aktuariuszom uciekać się do dostępnego oprogramowania statystycznego wykonującego regresję Poissona do budowy tablic trwania życia.

Gdy cechy ciągłe są uwzględniane w analizie przeżycia, takie jak suma ubezpieczona dla ubezpieczonych, agregacja według grup wymaga wstępnego, subiektywnego grupowania. Sprytniejsze wykorzystanie GAM pozwala uniknąć tego problematycznego kroku i pozwala aktuariuszowi analizować indywidualne dane o śmiertelności za pomocą regresji Poissona. Niezbędna baza danych zawiera jeden rekord na polisę i rok (ten sam format co w ubezpieczeniach komunikacyjnych, na przykład) z binarną zmienną wskazującą zgon i numerem referencyjnym łączącym wszystkie rekordy związane z tą samą polisą. Czasami wymaga to rozszerzenia bazy danych poprzez podzielenie każdej indywidualnej obserwacji na zestaw niezależnych realizacji zliczeń Poissona o tych samych niezmiennych w czasie zmiennych objaśniających, lub pozwolenie, aby ewoluowały one w czasie, gdy są dynamiczne (takie jak osiągnięty wiek, na przykład). To sprawia, że podejście GAM jest równie skuteczne w badaniach ubezpieczeń na życie i majątkowych.

## 6.2 Struktura GAM

### 6.2.1 Rozkład odpowiedzi

Podobnie jak w przypadku GLM, rozważamy odpowiedzi $Y_1, Y_2, \ldots, Y_n$ mierzone na $n$ osobach. Dla każdej odpowiedzi aktuariusz ma wektor $\boldsymbol{x}_i = (1, x_{i1}, \ldots, x_{ip})^{\text{T}}$ o wymiarze $p+1$ zawierający odpowiednie cechy (uzupełnione o 1 z przodu, gdy w scoringu uwzględniony jest wyraz wolny). Cechy są teraz podzielone na $p_{\text{cat}}$ zmiennych kategorialnych $x_{i1}, \ldots, x_{ip_{\text{cat}}}$ i $p_{\text{cont}} = p - p_{\text{cat}}$ zmiennych ciągłych $x_{i, p_{\text{cat}}+1}, \ldots, x_{ip}$.
Biorąc pod uwagę warunkową informację zawartą w $x_i$, zakładamy, że odpowiedzi $Y_i$ są niezależne z rozkładem warunkowym w rodzinie ED gruntownie przestudiowanej w rozdziale 2. W szczególności, $Y_i$ podlega rozkładowi (2.3) z kanonicznym parametrem $\theta_i$ i tym samym parametrem dyspersji $\phi$. Sposób, w jaki $\theta_i$ jest powiązany z informacją zawartą w $\boldsymbol{x}_i$, zostanie wyjaśniony w następnej kolejności.

### 6.2.2 Addytywne scoringi

W praktyce wiele cech jest kategorialnych i efektywnie wchodzi do GLM po zakodowaniu za pomocą zmiennych binarnych. Jednak niektóre ważne zmienne scoringowe są ciągłe: w taryfikacji ubezpieczeń komunikacyjnych, wiek lub moc samochodu są tego przykładem. W ubezpieczeniach na życie i zdrowie, wiek i suma ubezpieczona są cechami ciągłymi. W ogólności, ciągłość odnosi się tutaj do cechy ilościowej z wieloma odrębnymi, uporządkowanymi wartościami. Na przykład wiek jest generalnie rejestrowany jako wiek w ostatnie urodziny, więc przyjmuje tylko wartości całkowite (18, 19, 20, ... w ubezpieczeniach komunikacyjnych, na przykład). Niemniej jednak jest uważany za cechę ciągłą, ponieważ oczekujemy płynnego postępu ryzyka wraz z wiekiem, bez nagłych skoków.

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
- nie rozpoznać oczekiwanej płynnej zmiany średniej odpowiedzi w $x^*$ (ponieważ parametry regresji odpowiadające każdemu poziomowi nie są ze sobą powiązane po zakodowaniu za pomocą zmiennych binarnych).

Ogólnie rzecz biorąc, powoduje to dużą zmienność w oszacowanych współczynnikach regresji związanych z różnymi poziomami cechy ciągłej $x^*$, gdy jest ona traktowana jako cecha kategorialna.
Zanim GAM weszły do zestawu narzędzi aktuariusza, klasyczne podejście do radzenia sobie z cechami ciągłymi było czysto parametryczne: każda cecha ciągła $x^*$ była przekształcana przed włączeniem do scoringu. Ogólnie rzecz biorąc, funkcja dająca wpływ $x^*$ na scoring jest znana, liniowa, wielomianowa lub odcinkowo stała. Tak więc funkcja jest znana z dokładnością do skończonej liczby parametrów do oszacowania na podstawie danych i wracamy do przypadku GLM. Jednakże, bez względu na to, jaką formę parametryczną określimy, zawsze wykluczamy wiele możliwych funkcji.
Aktuariusze często oczekują płynnej zmiany średniej odpowiedzi wraz z $x^*$. Oczekuje się, że oczekiwana liczba szkód w ubezpieczeniach komunikacyjnych lub wskaźnik śmiertelności w ubezpieczeniach na życie będzie się zmieniać bardzo płynnie wraz z wiekiem, bez nagłych skoków z jednego wieku do drugiego. W tym rozdziale wyjaśniamy, jak dopasować model z addytywnym scoringiem pozwalającym na nieliniowe efekty ciągłych cech na skali scoringu, które są wyuczone z dostępnych danych. Poniższy przykład w ubezpieczeniach komunikacyjnych wyjaśnia wartość dodaną GAM w tym zakresie.

**Przykład 6.2.2** Kontynuujmy z przykładem 6.2.1. Rozważmy dane $(Y_i, e_i, x_i)$. Możemy zobaczyć ekspozycje na ryzyko $e_i$ (w polisach-latach), a także wynik uzyskany przez traktowanie wieku liniowo na skali scoringu (linia prosta) lub jako cechy kategorialnej, z określonym współczynnikiem regresji dla każdego wieku (okręgi). Widzimy, że żadne z tych podejść nie jest zadowalające: efekt liniowy pomija efekt wieku w kształcie litery U, podczas gdy współczynniki regresji specyficzne dla wieku nie są wystarczająco gładkie.
To jest powód, dla którego przechodzimy na

$$
\beta_0 + \sum_{j=1}^{p} \beta_j x_{ij} + b(\text{age}_i)
$$

dla pewnej nieznanej funkcji $b(\cdot)$. Nie zakłada się niczego o $b(\cdot)$ poza tym, że jest to gładka funkcja wieku. W istocie, $x \mapsto \hat{b}(x)$ jest wygładzoną wersją współczynników regresji $x \mapsto \hat{\beta}_x$ uzyskanych przez traktowanie wieku $x$ jako cechy kategorialnej (tak, że wiek jest zakodowany przez zestaw zmiennych binarnych, po jednej dla każdego obserwowanego wieku w bazie danych z wyjątkiem referencyjnego). W naszym przykładzie $\hat{\beta}_x$ są okręgami widocznymi w prawym górnym panelu Rys. 6.1. W szczególności wyjaśniamy, jak dopasować model

$$
Y_i \sim \mathcal{Poi} \left( e_i \exp \left( \beta_0 + \sum_{j=1}^{p} \beta_j x_{ij} + b(\text{age}_i) \right) \right).
$$

Wynik jest wyświetlony w prawym dolnym panelu Rys. 6.1. Widzimy, że $\hat{b}(\cdot)$ ładnie wygładza $\hat{\beta}_x$ z wyjątkiem starszych grup wiekowych, gdzie występuje duża zmienność (z powodu małych ekspozycji dla każdego wieku, jak widać na lewym górnym panelu Rys. 6.1).

Kryterium używane do wyboru stopnia gładkości na podstawie danych wyświetlonych w lewym dolnym panelu, zwane LCV od Likelihood Cross Validation, zostanie wyjaśnione w następnej sekcji.

Ta sama sytuacja występuje w ubezpieczeniach na życie. Biorąc pod uwagę Rys. 4.2, widzimy, że prawdopodobieństwa zgonu nie zależą liniowo od wieku w całym zakresie wiekowym. Prawdopodobieństwa zgonu są raczej wysokie dla noworodków, następnie gwałtownie spadają do niższego poziomu śmiertelności dla dzieci i nastolatków. Dla wieku 20-25 lat obserwuje się tak zwany "wzgórek wypadkowy", który jest szczególnie wyraźny u mężczyzn. Od 30 roku życia prawdopodobieństwa zgonu rosną niemal liniowo wraz z wiekiem (na skali scoringu). W starszym wieku roczne prawdopodobieństwa zgonu pozostają rosnące, ale o innym kształcie. Surowe $\hat{q}_x = D_x/l_x$ wyświetlone na Rys. 4.2 można postrzegać jako wynik Binomialnego GLM, gdzie wiek $x$ jest traktowany jako cecha kategorialna. Wynikowe oszacowane prawdopodobieństwa zgonu nie są połączone, więc wyraźnie widoczne są tam nieregularne zmiany.

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