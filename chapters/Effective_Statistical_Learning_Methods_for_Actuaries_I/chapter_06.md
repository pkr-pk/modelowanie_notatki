# Rozdział 6: Uogólnione modele addytywne (GAMs)

## 6.1 Wprowadzenie

Widzieliśmy w Rozdziale 4, że modele GLM oferują skuteczne rozwiązanie wielu problemów w naukach aktuarialnych. Jednak założenie liniowości dotyczące scoringu jest czasami wątpliwe: nieliniowe efekty cech ciągłych, takich jak wiek posiadacza polisy, mogą wymagać uwzględnienia w skali scoringu, aby odzwierciedlić rzeczywiste doświadczenie. Cechy ciągłe mogą skutecznie wchodzić do modeli GLM tylko wtedy, gdy są odpowiednio przekształcone, aby odzwierciedlić ich prawdziwy wpływ na skalę scoringu. Niestety, nie zawsze jest jasne, jak zmienne powinny być przekształcone przed ich włączeniem. W firmach ubezpieczeniowych stało się powszechną praktyką modelowanie potencjalnie nieliniowych efektów za pomocą wielomianów. Jednak wielomiany niskiego stopnia często nie są wystarczająco elastyczne, aby uchwycić strukturę obecną w danych, podczas gdy zwiększanie ich stopnia prowadzi do niestabilnych oszacowań, zwłaszcza dla skrajnych wartości cech.

Innym podejściem powszechnie stosowanym w praktyce jest zastąpienie cechy ciągłej cechą kategorialną, uzyskaną przez podzielenie jej dziedziny na umiarkowaną liczbę podprzedziałów i założenie, że cecha ma stały wpływ na scoring w każdym przedziale. Oznacza to, że funkcja dająca efekt cechy ciągłej na skali scoringu jest zakładana jako odcinkowo stała. Takie grupowanie oczywiście skutkuje utratą informacji. Co więcej, nie ma ogólnej zasady określającej optymalny wybór punktów cięcia, więc grupowanie może obciążać ocenę ryzyka.

Uogólnione modele addytywne (GAM) weszły do zestawu narzędzi aktuariusza, aby radzić sobie z cechami ciągłymi w elastyczny sposób. W tym ujęciu cechy ciągłe wchodzą do modelu w postaci półparametrycznego predyktora addytywnego. W ubezpieczeniach majątkowych i osobowych (Property and Casualty), wpływ wieku posiadacza polisy, mocy pojazdu lub sumy ubezpieczenia może być modelowany za pomocą modeli GAM. GAM pozwalają również aktuariuszowi analizować wariacje ryzyka według obszaru geograficznego, uwzględniając możliwą interakcję między cechami ciągłymi (szerokość i długość geograficzna podające lokalizację przestrzenną, w tym przypadku). Inne interakcje, ogólnie obecne w danych, obejmują wiek i moc w ubezpieczeniach komunikacyjnych. One również mogą być uchwycone przez modele GAM.

W ubezpieczeniach na życie statystyki śmiertelności są często agregowane dla grup ubezpieczonych o tych samych cechach (takich jak wiek, płeć czy rodzaj produktu) w celu przeprowadzenia analizy GLM. Liczba zgonów wynikająca z danych ekspozycji może być następnie badana przy użyciu technik regresji Poissona. Kluczowym argumentem teoretycznym legitymizującym Poissona GLM jest to, że przy łagodnych założeniach, prawdopodobieństwo Poissona wydaje się być proporcjonalne do prawdziwego prawdopodobieństwa. Dlatego wnioskowanie statystyczne oparte na prawdopodobieństwie Poissona nie jest restrykcyjne. Jest to szczególnie interesujące z praktycznego punktu widzenia, ponieważ pozwala aktuariuszom korzystać z dostępnego oprogramowania statystycznego wykonującego regresję Poissona do budowy tablic trwania życia.

Gdy w analizie przeżycia uwzględnione są cechy ciągłe, takie jak suma ubezpieczenia, agregacja przed analizą GLM Poissona wymaga wstępnego, subiektywnego grupowania. Sprytne wykorzystanie modeli GAM pozwala uniknąć tego problematycznego kroku i umożliwia aktuariuszowi analizę indywidualnych danych o śmiertelności za pomocą regresji Poissona. Niezbędna baza danych zawiera jeden rekord na polisę i na rok (ten sam format jak na przykład w ubezpieczeniach komunikacyjnych) ze zmienną binarną wskazującą zgon i numerem referencyjnym łączącym wszystkie rekordy związane z tą samą polisą. Czasami wymaga to powiększenia bazy danych poprzez podzielenie każdej indywidualnej obserwacji na zbiór niezależnych realizacji Poissona, które dzielą te same niezmienne w czasie zmienne objaśniające, lub pozwalając tym cechom ewoluować w czasie, tak jak są one dynamiczne (na przykład osiągnięty wiek). To sprawia, że podejście GAM jest równie skuteczne w badaniach ubezpieczeń na życie i majątkowych.

## 6.3 Inference in GAMs

### 6.3.2 Splines

#### 6.3.2.2 Splajny Sześcienne

Ograniczone rozwinięcie Taylora wyjaśnia zatem użycie splajnów: splajny to wielomiany odcinkowe połączone w celu utworzenia jednej gładkiej krzywej. Główną ideą splajnów sześciennych jest zastąpienie ciągłej cechy $x$ nowymi cechami, które są transformacjami $x$, postaci $x$, $x^2$, $x^3$ oraz $(x − c_j)^3_+$, w oparciu o rozwinięcie Taylora. Mechanizm GLM stosuje się następnie do tego nowego zestawu cech.

Gdy są dopasowywane do danych, wielomiany mają tendencję do błędnego zachowania w pobliżu granic, a ekstrapolacja może być pozbawiona sensu. To nieprzyjemne zachowanie jest zaostrzane przez splajny. Dlatego naturalne splajny sześcienne dodają dalsze ograniczenia, tak aby funkcja była liniowa poza węzłami granicznymi. Rozważmy model regresji normalnej

$$Y_i \sim Nor(b(x_i), \sigma^2), \quad i = 1, 2,...,n,$$

dla pewnej nieznanej funkcji $b(\cdot)$. Naturalne splajny sześcienne z węzłami w każdej obserwacji $x_i$, to jest z $p_\zeta = n$ i $\zeta_j* = x_j$, są rozwiązaniami następującego problemu optymalizacyjnego: spośród wszystkich funkcji $b$ z ciągłymi drugimi pochodnymi, znajdź tę, która minimalizuje penalizowaną resztkową sumę kwadratów

$$
\sum_{i=1}^{n} (y_i - b(x_i))^2 + \lambda \int (b''(x))^2 dx
$$

gdzie $\lambda \ge 0$ jest parametrem wygładzającym, a wartości cechy ciągłej $x$ są założone jako uporządkowane rosnąco, tj. $x_1 < x_2 < ... < x_n$. Ta penalizowana wersja kryterium najmniejszych kwadratów równoważy dobroć dopasowania i gładkość oszacowania $b$; pierwszy składnik wymusza zgodność z danymi, podczas gdy drugi penalizuje krzywiznę (zauważ, że $b'' = 0$, jeśli $b$ jest liniowe). Tutaj $\lambda$ jest parametrem wygładzającym kontrolującym kompromis między dwoma składnikami: dobrocią dopasowania a karą za chropowatość. Dwa skrajne przypadki są następujące:

$\lambda = 0$ nie nakłada żadnych ograniczeń, a wynikowe oszacowanie interpoluje dane (w rzeczywistości jest to dowolna funkcja interpolująca dane).

$\lambda \to \infty$ sprawia, że krzywizna jest niemożliwa, więc wracamy do modelu regresji liniowej $b(x) = \beta_0 + \beta_1x$, który spełnia $b'' = 0$.

Zatem duże $\lambda$ odpowiada gładkiej funkcji $b$.

Co ciekawe, minimalizator jest funkcją skończenie wymiarową, chociaż cel jest minimum z nieskończenie wymiarowej przestrzeni funkcyjnej. Dokładniej, unikalny minimalizator $\hat{b}_\lambda$ jest naturalnym splajnem sześciennym z węzłami zlokalizowanymi w wartościach $x_i$, to jest funkcją z (6.7) z $p_\zeta = n$ i $\zeta_j = x_j$. Oznacza to, że $\hat{b}_\lambda$ jest wielomianem sześciennym na każdym przedziale $[x_i, x_{i+1})$, takim że $\hat{b}_\lambda$, $\hat{b}'_\lambda$ i $\hat{b}''_\lambda$ są ciągłe wszędzie, a $b''_\lambda(x_1) = \hat{b}''_\lambda(x_n) = 0$. Wiedząc, że rozwiązanie jest naturalnym splajnem sześciennym, można je oszacować za pomocą reprezentacji

$$
\hat{b}_{\lambda}(x) = \sum_{j=1}^{n} \beta_j B_j(x)
$$

gdzie funkcje $B_j(\cdot)$ tworzą bazę dla takich splajnów. Problem sprowadza się do dopasowania zwykłego modelu regresji liniowej z przekształconymi cechami $B_j(x)$. Oznaczając jako $\mathbf{B}$ macierz projektu z kolumnami $(B_j(x_1), ..., B_j(x_n))$ i definiując macierz $\mathbf{\Omega}$ z elementem $(j, k)$ danym przez

$$
\Omega_{jk} = \int B''_j(z) B''_k(z) dz,
$$

rozwiązanie $\hat{\beta}$ minimalizuje $||\mathbf{Y} - \mathbf{B}\beta||^2 + \lambda\beta^T\mathbf{\Omega}\beta$, tak że

$$
\hat{\beta} = (\mathbf{B}^T \mathbf{B} + \lambda \mathbf{\Omega})^{-1} \mathbf{B}^T \mathbf{Y}.
$$

W porównaniu ze wzorem (4.11), widzimy, że $\lambda\mathbf{\Omega}$ jest dodane do $\mathbf{B}^T\mathbf{B}$ w wyrażeniu na $\hat{\beta}$. Jest to ściśle związane z regresją grzbietową (omówioną w Rozdz. 5). Macierz $\lambda\mathbf{\Omega}$ służy jako macierz grzbietowa lub kurcząca (shrinkage matrix), tak że oszacowania $\hat{\beta}$ są kurczone w kierunku zera. Wynika to z faktu, że dla dużego $\lambda$, wyrażenie $(\mathbf{B}^T\mathbf{B} + \lambda\mathbf{\Omega})^{-1}$ staje się małe. Ponadto, suma kwadratów reszt może być zastąpiona logarytmem prawdopodobieństwa dowolnego rozkładu ED, gdy rozważane są odpowiedzi nienormalne.

Ponieważ dopasowane wartości $\hat{y}_i = \hat{b}_{\lambda}(x_i)$ można uzyskać z

$$
\hat{\mathbf{Y}} = \mathbf{S}_{\lambda} \mathbf{Y} \quad \text{z} \quad \mathbf{S}_{\lambda} = \mathbf{B}(\mathbf{B}^T \mathbf{B} + \lambda \mathbf{\Omega})^{-1} \mathbf{B}^T
$$

złożoność oszacowanej funkcji splajnu można wywnioskować z równoważnych stopni swobody $\text{edf} = \text{Tr}(\mathbf{S}_{\lambda})$. Intuicyjnie rzecz biorąc, splajn wygładzający z $\text{edf} = 5$ jest tak samo złożony jak globalny wielomian stopnia 4 (który ma 5 parametrów, wliczając wyraz wolny).

## 6.4 Risk Classification in Motor Insurance

### 6.4.2 Modeling Claim Counts

#### 6.4.2.2 Model Regresji GAM dla Liczby Roszczeń

Rozkład Poissona jest naturalnym kandydatem do modelowania liczby roszczeń zgłaszanych przez ubezpieczających. Typowym założeniem w tych okolicznościach jest to, że warunkowa średnia częstość roszczeń może być zapisana jako funkcja wykładnicza addytywnego scoringu, który ma być oszacowany na podstawie danych.

Zaczynamy od modelu uwzględniającego wszystkie dostępne informacje, pozwalającego na możliwą interakcję między wiekiem a płcią ubezpieczającego. Poziomy odniesienia dla cech kategorycznych są następujące: Benzyna dla Paliwa, Mężczyzna dla Płci, C2 dla Mocy, OC.Tylko dla Zakresu, Rocznie dla Podziału i Prywatne dla Użytkowania. Jest to zgodne z najliczniej reprezentowanymi kategoriami widocznymi na Rys. 6.8 i 6.9. Domyślnie R porządkuje poziomy alfabetycznie, ale można to zmienić za pomocą polecenia C, określając poziom bazowy, czyli odniesienia.

Dopasowanie wykonuje się za pomocą algorytmu **backfitting**. Procedura ta zbiegła się szybko: potrzebne były tylko trzy iteracje, z kryterium zatrzymania w postaci względnej różnicy w log-prawdopodobieństwach mniejszej niż $10^{-5}$. Rysunek 6.15 porównuje wartości parametrów uzyskane w różnych iteracjach tego algorytmu dla cech kategorycznych. Rysunek 6.16 przedstawia oszacowane efekty dla cech ciągłych. Widzimy, że początkowe oszacowania są już bardzo bliskie ostatecznym. Rysunek 6.17 przedstawia różnicę między krokiem początkowym a ostatecznymi szacunkami dla belgijskich dystryktów. Ponownie widzimy, że zróżnicowanie między początkowymi a końcowymi oszacowaniami jest raczej niewielkie.

Małe do umiarkowanych różnice między poszczególnymi krokami algorytmu backfitting legitymizują procedurę krok po kroku, preferowaną przez wielu praktyków, którzy często zatrzymują się po pierwszym cyklu algorytmu backfitting i po prostu włączają efekty w sposób progresywny, zamrażając poprzednie oszacowania w offsecie.

Skomentujmy krótko wynikowe dopasowanie. Zaczynamy od wpływu cech kategorycznych na scoring. Widzimy, że wykupienie większej liczby gwarancji niż tylko obowiązkowe OC ma tendencję do zmniejszania oczekiwanej liczby roszczeń (co widać po ujemnych oszacowanych współczynnikach regresji). Ponadto, jazda pojazdem na benzynę wydaje się bezpieczniejsza. Analizując nieliniową część scoringu (patrz Rys. 6.16), odkrywamy, że interakcja wiek-płeć jest istotna. Młodzi mężczyźni (poniżej 35 lat) mają tendencję do zgłaszania większej liczby wypadków niż młode kobiety, a także starsi mężczyźni (powyżej 80 lat). Pomiędzy tymi wiekami mężczyźni wydają się mniej niebezpieczni. Można to przypisać faktowi, że z powodu wysokich składek nakładanych na młodych ubezpieczonych, w Belgii powszechną praktyką było w tamtym czasie proszenie starszych krewnych (najczęściej matki) o zakup polisy. Szczyt efektu wieku około 45 lat jest generalnie przypisywany wypadkom spowodowanym przez dzieci za kierownicą. Nowe samochody wydają się bardziej niebezpieczne niż starsze po 4 latach. Pamiętajmy, że w Belgii samochód nie musi przechodzić corocznego przeglądu technicznego organizowanego przez państwo przez pierwsze trzy lata. Zatem kierowcy z wysokim rocznym przebiegiem często trzymają swój samochód tylko przez trzy lata, a następnie kupują nowy. Efekt geograficzny przedstawiony na Rys. 6.18 jest zgodny z naszymi oczekiwaniami, wskazując, że duże miasta są bardziej niebezpieczne pod względem częstości roszczeń.

Przeprowadźmy teraz analizę za pomocą funkcji `gam` zawartej w pakiecie R `mgcv`. Zaimplementowano tam penalizowane prawdopodobieństwo, a optymalna wartość parametrów wygładzania jest wybierana przy użyciu kryterium uogólnionej walidacji krzyżowej (GCV) (danego przez $nD/(n - \text{edf})^2$, gdzie $D$ to dewiancja, a edf to równoważna liczba stopni swobody) lub przeskalowanego AIC.

Oszacowania liniowej części scoringu, obejmującej cechy kategoryczne, są przedstawione w Tabeli 6.1. Widzimy, że sposób użytkowania pojazdu nie wpływa znacząco na oczekiwaną liczbę roszczeń. Dlatego wykluczamy tę cechę z modelu i ponownie dopasowujemy Poissona GAM. Wynikowe oszacowania liniowej części scoringu są przedstawione w Tabeli 6.2. Widzimy, że pominięcie sposobu użytkowania pozostawia pozostałe oszacowania niemal niezmienione.

**Tabela 6.1** Wynik funkcji `gam` z pakietu R `mgcv` dla dopasowania liczby roszczeń w ubezpieczeniu OC komunikacyjnym.
| Coefficient | Estimate | Std. Error | z value | $Pr(> \mid z\mid )$ | |
|---|---|---|---|---|---|
| **Intercept** | –2.12986 | 0.01641 | –129.812 | < 2 x 10⁻¹⁶ | *** |
| **Fuel diesel** | 0.19219 | 0.01566 | 12.271 | < 2 x 10⁻¹⁶ | *** |
| **Gender female** | 0.07499 | 0.01718 | 4.366 | 1.27 x 10⁻⁵ | *** |
| **Cover comprehensive** | –0.16264 | 0.02540 | –6.403 | 1.52 x 10⁻¹⁰ | *** |
| **Cover Limited.MD** | –0.15170 | 0.01796 | –8.444 | < 2 x 10⁻¹⁶ | *** |
| **Split half-yearly** | 0.16614 | 0.01784 | 9.311 | < 2 x 10⁻¹⁶ | *** |
| **Split monthly**| 0.30639 | 0.02212 | 13.854 | < 2 x 10⁻¹⁶ | *** |
| **Split quarterly**| 0.36560 | 0.02545 | 14.365 | < 2 x 10⁻¹⁶ | *** |
| **Use professional**| 0.03206 | 0.03349 | 0.957 | 0.338413 | |
| **PowerCat C1** | –0.07913 | 0.01620 | –4.885 | 1.04 x 10⁻⁶ | *** |
| **PowerCat C3** | 0.08000 | 0.02555 | 3.131 | 0.001741 | ** |
| **PowerCat C4** | 0.18747 | 0.04877 | 3.844 | 0.000121 | *** |
| **PowerCat C5** | 0.47399 | 0.18631 | 2.544 | 0.010955 | * |

**Tabela 6.2** Wynik funkcji `gam` z pakietu R `mgcv` dla dopasowania liczby roszczeń w ubezpieczeniu OC komunikacyjnym.
| Coefficient | Estimate | Std. Error | z value | $Pr(> \mid z\mid )$ | |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **Intercept** | –2.12876 | 0.01637 | –130.075 | < 2 x 10⁻¹⁶ | *** |
| **Fuel diesel** | 0.19320 | 0.01563 | 12.364 | < 2 x 10⁻¹⁶ | *** |
| **Gender female** | 0.07484 | 0.01718 | 4.357 | 1.32 x 10⁻⁵ | *** |
| **Cover comprehensive** | –0.16073 | 0.02532 | –6.348 | 2.18 x 10⁻¹⁰ | *** |
| **Cover Limited.MD** | –0.15142 | 0.01796 | –8.430 | < 2 x 10⁻¹⁶ | *** |
| **Split half-yearly** | 0.16565 | 0.01784 | 9.288 | < 2 x 10⁻¹⁶ | *** |
| **Split monthly**| 0.30594 | 0.02211 | 13.837 | < 2 x 10⁻¹⁶ | *** |
| **Split quarterly**| 0.36548 | 0.02545 | 14.360 | < 2 x 10⁻¹⁶ | *** |
| **PowerCat C1** | –0.07956 | 0.01619 | –4.914 | 8.95 x 10⁻⁷ | *** |
| **PowerCat C3** | 0.08150 | 0.02550 | 3.196 | 0.00139 | ** |
| **PowerCat C4** | 0.19092 | 0.04864 | 3.926 | 8.65 x 10⁻⁵ | *** |
| **PowerCat C5** | 0.47702 | 0.18628 | 2.561 | 0.01045 | * |

Zauważmy, że niektóre poziomy można by połączyć. Na przykład, oszacowane współczynniki regresji dla Limited.MD i Comprehensive nie różnią się znacząco, biorąc pod uwagę związane z nimi błędy standardowe, więc moglibyśmy zdefiniować nową cechę Cover2 z tylko dwoma poziomami, TPL.Only i TPL+, przy czym ten drugi łączy poziomy Limited.MD i Comprehensive. Podobne grupowania można by osiągnąć dla Split.

Rozważmy teraz dopasowanie nieliniowej części scoringu. Efekt wieku dla kierowców płci żeńskiej ma edf równe 5.238 i jest wysoce istotny z *p*-value mniejszym niż $2 \times 10^{-16}$, podczas gdy edf związane z wiekiem dla kierowców płci męskiej jest równe 6.132 z *p*-value mniejszym niż $2 \times 10^{-16}$. Pozostałe dwie cechy ciągłe są również wysoce istotne, z edf równym 7.878 i *p*-value $1.14 \times 10^{-13}$ dla wieku samochodu oraz edf równym 26.281 z *p*-value mniejszym niż $2 \times 10^{-16}$ dla efektu geograficznego. Oszacowane funkcje są przedstawione na Rys. 6.19. Widzimy, że uzyskane wyniki są bardzo podobne do tych z Rys. 6.16, z tą różnicą, że tutaj wartości są podane w skali logarytmicznej, a wynik dla efektu geograficznego nie jest narysowany na mapie z `gam`.