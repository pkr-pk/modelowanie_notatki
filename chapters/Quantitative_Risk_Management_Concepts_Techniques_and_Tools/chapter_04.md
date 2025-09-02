# Rozdział 4: Finansowe szeregi czasowe

## 4.1 Podstawy analizy szeregów czasowych

### 4.1.4 Analiza statystyczna szeregów czasowych

W praktyce, analiza statystyczna danych szeregów czasowych $X_1,...,X_n$ przebiega według programu składającego się z następujących etapów.

*Analiza wstępna.* Dane są przedstawiane na wykresie i rozważana jest zasadność zastosowania pojedynczego modelu stacjonarnego. Na tym etapie można również przeprowadzić szereg formalnych testów numerycznych stacjonarności.

Ponieważ koncentrujemy się tutaj na zróżnicowanych logarytmicznych szeregach wartości, założymy, że nasze dane wymagają co najwyżej niewielkiej wstępnej obróbki. Klasyczna analiza szeregów czasowych dysponuje wieloma technikami usuwania trendów i sezonowości z danych "niestacjonarnych". Chociaż pewne rodzaje finansowych szeregów czasowych, takie jak szeregi czasowe zysków, z pewnością wykazują wzorce sezonowe, założymy, że takie efekty są stosunkowo niewielkie w przypadku dziennych lub tygodniowych szeregów zwrotów, które stanowią podstawę metod zarządzania ryzykiem. Gdybyśmy opierali nasze zarządzanie ryzykiem na danych o wysokiej częstotliwości, wstępne oczyszczanie danych byłoby ważniejszą kwestią, ponieważ wykazują one wyraźne cykle dobowe i inne cechy deterministyczne.

Oczywiście, założenie o stacjonarności staje się bardziej wątpliwe, jeśli przyjmiemy długie okna danych lub jeśli wybierzemy okna, w których miały miejsce dobrze znane zmiany w polityce gospodarczej. Chociaż rynki stale się zmieniają, zawsze będzie istniało napięcie między naszym pragnieniem korzystania z najbardziej aktualnych danych a potrzebą uwzględnienia wystarczającej ilości danych, aby uzyskać precyzję w estymacji statystycznej. To, czy odpowiednie będzie pół roku danych, rok, pięć lat czy dziesięć lat, zależy od sytuacji. Z pewnością dobrym pomysłem jest przeprowadzenie szeregu analiz z różnymi oknami danych i zbadanie wrażliwości wnioskowania statystycznego na ilość danych.

*Analiza w dziedzinie czasu.* Po ustaleniu danych do gry wchodzą techniki z sekcji 4.1.3. Poprzez zastosowanie korelogramów i testów portmanteau, takich jak test Ljunga-Boxa, zarówno do surowych danych, jak i ich wartości bezwzględnych, oceniana jest hipoteza o ścisłym białym szumie. Jeśli nie można jej odrzucić dla danych, formalna analiza szeregów czasowych jest zakończona, a zamiast modelowania dynamicznego można zastosować proste dopasowanie rozkładu.

W przypadku dziennych szeregów zwrotów czynników ryzyka spodziewamy się szybkiego odrzucenia hipotezy o ścisłym białym szumie. Pomimo faktu, że korelogramy surowych danych mogą wykazywać niewielkie dowody na korelację seryjną, korelogramy danych bezwzględnych prawdopodobnie wykażą dowody silnej zależności seryjnej. Innymi słowy, dane mogą wspierać model białego szumu, ale nie model ścisłego białego szumu. W tym przypadku modelowanie ARMA nie jest wymagane, ale modele zmienności z sekcji 4.2 mogą być użyteczne.

Jeśli korelogram dostarcza dowodów na istnienie wzorców korelacji seryjnej typowych dla procesów ARMA, możemy spróbować dopasować procesy ARMA do danych.

*Dopasowanie modelu.* Tradycyjne podejście do dopasowania modelu polega najpierw na próbie identyfikacji rzędu odpowiedniego procesu ARMA za pomocą korelogramu i dodatkowego narzędzia znanego jako korelogram cząstkowy. Na przykład, obecność odcięcia na opóźnieniu $q$ w korelogramie jest traktowana jako diagnostyka dla czystego zachowania średniej ruchomej rzędu $q$ (a podobne zachowanie w korelogramie cząstkowym wskazuje na czyste zachowanie AR). Dzięki nowoczesnej mocy obliczeniowej jest teraz dość łatwo dopasować różne modele MA, AR i ARMA i użyć kryterium wyboru modelu, takiego jak kryterium Akaike, aby wybrać "najlepszy" model. Istnieją również zautomatyzowane procedury wyboru modelu, takie jak metoda Tsay i Tiao.

Czasami istnieją a priori powody, aby oczekiwać, że pewne rodzaje modeli będą najbardziej odpowiednie. Na przykład, załóżmy, że analizujemy zwroty z dłuższych okresów, które się nakładają, jak w (3.4). Rozważmy przypadek, w którym surowe dane to zwroty dzienne, a my budujemy zwroty tygodniowe. W (3.4) ustawiamy $h = 5$ (aby uzyskać zwroty tygodniowe) i $k = 1$ (aby uzyskać jak najwięcej danych). Zakładając, że dane bazowe pochodzą z procesu białego szumu $(X_t)_{t \in Z} \sim WN(0, \sigma^2)$, zagregowane zwroty tygodniowe w czasie $t$ i $t + l$ spełniają

$$\text{cov}(X_t^{(5)}, X_{t+l}^{(5)}) = \text{cov}(\sum_{i=0}^{4} X_{t-i}, \sum_{j=0}^{4} X_{t+l-j}) = 
\begin{cases}(5 - l)\sigma^2, & l = 0,..., 4,\\0, & l \ge 5,
\end{cases}$$

więc nakładające się zwroty mają strukturę korelacji procesu MA(4) i byłby to naturalny wybór modelu szeregów czasowych dla nich.

Po wybraniu modelu do dopasowania istnieje szereg możliwych metod dopasowania, w tym specjalistyczne metody dla procesów AR, takie jak Yule-Walkera, które czynią minimalne założenia dotyczące rozkładu innowacji białego szumu. W sekcji 4.2.4 omawiamy metodę (warunkowej) największej wiarygodności, która może być użyta do dopasowania modeli ARMA z (lub bez) błędami GARCH do danych.

*Analiza reszt i porównanie modeli.* Przypomnijmy reprezentację przyczynowego i odwracalnego procesu ARMA w (4.11) i załóżmy, że dopasowaliśmy taki proces i oszacowaliśmy parametry $\phi_i$ i $\theta_j$. Reszty to wywnioskowane wartości $\hat{\epsilon}_t$ dla nieobserwowanych innowacji $\epsilon_t$ i są obliczane rekurencyjnie z danych i dopasowanego modelu za pomocą równań

$$\hat{\epsilon}_t = X_t - \hat{\mu}_t, \quad \hat{\mu}_t = \hat{\mu} + \sum_{i=1}^{p} \hat{\phi}_i(X_{t-i} - \hat{\mu}) + \sum_{j=1}^{q} \hat{\theta}_j \hat{\epsilon}_{t-j}, \quad (4.13)$$

gdzie wartości $\hat{\mu}_t$ są czasami nazywane wartościami dopasowanymi. Oczywiście mamy problem z obliczeniem pierwszych kilku wartości $\hat{\epsilon}_t$ z powodu skończoności naszej próbki danych i nieskończonej natury rekurencji (4.13). Jednym z wielu możliwych rozwiązań jest ustawienie $\hat{\epsilon}_{-q+1} = \hat{\epsilon}_{-q+2} = \cdots = \hat{\epsilon}_0 = 0$ oraz $X_{-p+1} = X_{-p+2} = \cdots = X_0 = \bar{X}$, a następnie użycie (4.13) dla $t = 1,...,n$. Ponieważ na pierwsze kilka wartości będą miały wpływ te wartości początkowe, można je zignorować w późniejszych analizach.

Reszty $(\hat{\epsilon}_t)$ powinny zachowywać się jak realizacja procesu białego szumu, ponieważ takie jest nasze założenie modelowe dla innowacji, a to można ocenić, konstruując ich korelogram. Jeśli w korelogramie nadal widoczne są dowody korelacji seryjnej, sugeruje to, że dobry model ARMA nie został jeszcze znaleziony. Co więcej, możemy użyć testów portmanteau, aby formalnie przetestować, czy reszty zachowują się jak realizacja procesu ścisłego białego szumu. Jeśli reszty zachowują się jak ścisły biały szum, dalsze modelowanie szeregów czasowych nie jest wymagane; jeśli zachowują się jak biały szum, ale nie ścisły biały szum, mogą być wymagane modele zmienności z sekcji 4.2.

Zazwyczaj możliwe jest znalezienie więcej niż jednego rozsądnego modelu ARMA dla danych, a do podjęcia decyzji o ogólnie najlepszym modelu lub modelach mogą być wymagane formalne techniki porównywania modeli. Można zastosować kryterium informacyjne Akaike lub jedną z wielu wariacji tego kryterium, które są często preferowane dla szeregów czasowych.

### 4.1.5 Predykcja

Istnieje wiele podejść do prognozowania lub przewidywania szeregów czasowych, a my podsumujemy dwa, które łatwo rozszerzają się na przypadek modeli GARCH. Pierwsza strategia wykorzystuje dopasowane modele ARMA (lub ARIMA) i jest czasami nazywana podejściem Boxa-Jenkinsa. Druga strategia to bezmodelowe podejście do prognozowania, znane jako wygładzanie wykładnicze, które jest powiązane z techniką wykładniczo ważonej średniej kroczącej do przewidywania zmienności.

*Prognozowanie przy użyciu modeli ARMA.* Rozważmy odwracalny model ARMA i jego reprezentację w (4.11). Niech $\mathcal{F}_t$ oznacza historię procesu aż do czasu $t$ włącznie, jak poprzednio, i załóżmy, że innowacje $(\epsilon_t)_{t \in Z}$ mają własność różnicy martyngałowej względem $(\mathcal{F}_t)_{t \in Z}$.

Dla problemu prognozowania wygodnie będzie oznaczyć naszą próbkę *n* danych jako $X_{t-n+1},...,X_t$. Zakładamy, że są to realizacje zmiennych losowych podążających za określonym modelem ARMA. Naszym celem jest przewidzenie $X_{t+1}$ lub, ogólniej, $X_{t+h}$, a naszą prognozę oznaczymy jako $P_tX_{t+h}$. Opisana metoda zakłada, że mamy dostęp do nieskończonej historii procesu do czasu *t* i wyprowadza wzór, który jest następnie aproksymowany dla naszej skończonej próbki.

Jako predyktor $X_{t+h}$ używamy warunkowej wartości oczekiwanej $E(X_{t+h} | \mathcal{F}_t)$. Spośród wszystkich prognoz $P_tX_{t+h}$ opartych na nieskończonej historii procesu do czasu *t*, ten predyktor minimalizuje średniokwadratowy błąd prognozy $E((X_{t+h} - P_tX_{t+h})^2)$.

Podstawowa idea polega na tym, że dla $h \ge 1$, prognoza $E(X_{t+h} | \mathcal{F}_t)$ jest rekurencyjnie oceniana w kategoriach $E(X_{t+h-1} | \mathcal{F}_t)$. Wykorzystujemy fakt, że $E(\epsilon_{t+h} | \mathcal{F}_t) = 0$ (własność różnicy martyngałowej innowacji) oraz że zmienne losowe $(X_s)_{s \le t}$ i $(\epsilon_s)_{s \le t}$ są "znane" w czasie *t*. Założenie odwracalności (4.10) zapewnia, że innowacja $\epsilon_t$ może być zapisana jako funkcja nieskończonej historii procesu $(X_s)_{s \le t}$. Aby zilustrować to podejście, wystarczy rozważyć model ARMA(1, 1), a uogólnienie na modele ARMA(p, q) jest proste.

**Przykład 4.15 (prognozowanie dla modelu ARMA(1, 1)).** Załóżmy, że do danych dopasowano model ARMA(1, 1) w postaci (4.11), a jego parametry $\mu$, $\phi_1$ i $\theta_1$ zostały określone. Nasza prognoza jednokrokowa dla $X_{t+1}$ to

$$E(X_{t+1} | \mathcal{F}_t) = \mu_{t+1} = \mu + \phi_1(X_t - \mu) + \theta_1\epsilon_t,$$

ponieważ $E(\epsilon_{t+1} | \mathcal{F}_t) = 0$. Dla prognozy dwukrokowej otrzymujemy

$$\begin{align*}
E(X_{t+2} | \mathcal{F}_t) = E(\mu_{t+2} | \mathcal{F}_t) &= \mu + \phi_1(E(X_{t+1} | \mathcal{F}_t) - \mu) \\
&= \mu + \phi_1^2(X_t - \mu) + \phi_1\theta_1\epsilon_t,
\end{align*}$$

a ogólnie mamy

$$E(X_{t+h} | \mathcal{F}_t) = \mu + \phi_1^h(X_t - \mu) + \phi_1^{h-1}\theta_1\epsilon_t.$$

Nie znając wszystkich historycznych wartości $(X_s)_{s \le t}$, tego predyktora nie można ocenić dokładnie, ponieważ nie znamy dokładnie $\epsilon_t$, ale można go dokładnie przybliżyć, jeśli *n* jest odpowiednio duże. Najprostszym sposobem jest podstawienie reszty modelu $\hat{\epsilon}_t$ obliczonej z (4.13) w miejsce $\epsilon_t$. Zauważmy, że $\lim_{h \to \infty} E(X_{t+h} | \mathcal{F}_t) = \mu$, prawie na pewno, więc prognoza zbiega do estymacji bezwarunkowej średniej procesu dla dłuższych horyzontów czasowych.

*Wygładzanie wykładnicze.* Jest to popularna technika stosowana zarówno do prognozowania szeregów czasowych, jak i estymacji trendu. Tutaj niekoniecznie zakładamy, że dane pochodzą z modelu stacjonarnego, chociaż zakładamy, że w modelu nie ma deterministycznego składnika sezonowego. Ogólnie rzecz biorąc, metoda ta jest mniej odpowiednia dla szeregów zwrotów z często zmieniającymi się znakami i lepiej nadaje się do niezróżnicowanych szeregów cen lub wartości. Stanowi ona podstawę bardzo powszechnej metody prognozowania zmienności.

Załóżmy, że nasze dane reprezentują realizacje zmiennych losowych $Y_{t-n+1},...,Y_t$, rozważane bez odniesienia do żadnego konkretnego modelu parametrycznego. Jako prognozę dla $Y_{t+1}$ używamy prognozy w postaci

$$P_tY_{t+1} = \sum_{i=0}^{n-1} \lambda(1 - \lambda)^i Y_{t-i}, \quad  \text{gdzie} \quad 0 < \lambda < 1.$$

W ten sposób ważymy dane od najnowszych do najstarszych za pomocą sekwencji wykładniczo malejących wag, które sumują się do prawie jedności. Łatwo można obliczyć, że

$$\begin{align*}
P_tY_{t+1} = \sum_{i=0}^{n-1} \lambda(1 - \lambda)^i Y_{t-i} & = \lambda Y_t + (1 - \lambda) \sum_{j=0}^{n-2} \lambda(1 - \lambda)^j Y_{t-1-j} \\
&= \lambda Y_t + (1 - \lambda)P_{t-1}Y_t, \quad (4.14)
\end{align*}$$

więc prognoza w czasie $t$ jest otrzymywana z prognozy w czasie $t - 1$ za pomocą prostego schematu rekurencyjnego. Wybór $\lambda$ jest subiektywny; im większa wartość, tym większą wagę przywiązuje się do najnowszej obserwacji. Badania walidacji empirycznej na różnych zbiorach danych mogą być użyte do określenia wartości $\lambda$, która daje dobre wyniki; Chatfield (2003) podaje, że w praktyce powszechnie stosowane są wartości od 0.1 do 0.3.

Zauważmy, że chociaż metoda ta jest powszechnie postrzegana jako technika prognozowania bezmodelowego, można wykazać, że jest to naturalna metoda prognozowania oparta na warunkowej wartości oczekiwanej dla niestacjonarnego modelu ARIMA(0, 1, 1).

## 4.2 Modele GARCH dla Zmiennej Wariancji

Najważniejsze modele dla dziennych szeregów czasowych zmian czynników ryzyka zostały omówione w tej sekcji. Podajemy definicje modeli ARCH (autoregresyjnej warunkowej heteroskedastyczności) i GARCH (uogólnionej ARCH) oraz omawiamy niektóre z ich właściwości matematycznych, zanim przejdziemy do ich praktycznego zastosowania.

### 4.2.1 Procesy ARCH

**Definicja 4.16.** Niech $(Z_t)_{t \in \mathbb{Z}}$ będzie SWN(0, 1). Proces $(X_t)_{t \in \mathbb{Z}}$ jest procesem ARCH(p), jeśli jest ściśle stacjonarny i jeśli spełnia, dla wszystkich $t \in \mathbb{Z}$ oraz dla pewnego procesu o wartościach ściśle dodatnich $(\sigma_t)_{t \in \mathbb{Z}}$, równania

$$X_t = \sigma_t Z_t, \quad (4.15)$$

$$\sigma_t^2 = \alpha_0 + \sum_{i=1}^{p} \alpha_i X_{t-i}^2, \quad (4.16)$$

gdzie $\alpha_0 > 0$ i $\alpha_i \ge 0$, $i = 1,...,p$.

Niech $\mathcal{F}_t = \sigma({X_s : s \le t})$ ponownie oznacza algebrę sigma reprezentującą historię procesu do czasu t, tak że $(\mathcal{F}_t)_{t \in \mathbb{Z}}$ jest filtracją naturalną. Konstrukcja (4.16) zapewnia, że $\sigma_t$ jest mierzalna względem $\mathcal{F}_{t-1}$, a proces $(\sigma_t)_{t \in \mathbb{Z}}$ jest nazywany przewidywalnym. Pozwala nam to obliczyć, pod warunkiem, że $E(|X_t|) < \infty$,

$$E(X_t | \mathcal{F}_{t-1}) = E(\sigma_t Z_t | \mathcal{F}_{t-1}) = \sigma_t E(Z_t | \mathcal{F}_{t-1}) = \sigma_t E(Z_t) = 0, \quad (4.17)$$

tak że proces ARCH ma właściwość ciągu różnic martyngałowych względem $(\mathcal{F}_t)_{t \in \mathbb{Z}}$. Jeśli proces jest stacjonarny w sensie kowariancji, jest to po prostu biały szum, jak omówiono w Sekcji 4.1.1.

**Uwaga 4.17.** Należy zauważyć, że niezależność $Z_t$ i $F_{t-1}$, którą założyliśmy powyżej, wynika z faktu, że proces ARCH musi być przyczynowy, tzn. równania (4.15) i (4.16) muszą mieć rozwiązanie postaci $X_t = f(Z_t, Z_{t-1},...)$ dla pewnej funkcji $f$, tak że $Z_t$ jest niezależne od poprzednich wartości procesu. Kontrastuje to z modelami ARMA, gdzie równania mogą mieć rozwiązania nieprzyczynowe.

Jeśli po prostu założymy, że proces jest stacjonarnym w sensie kowariancji białym szumem (dla którego warunek podamy w Stwierdzeniu 4.18), to $E(X_t^2) < \infty$ i
$$\text{var}(X_t | \mathcal{F}_{t-1}) = E(\sigma_t^2 Z_t^2 | \mathcal{F}_{t-1}) = \sigma_t^2 \text{var}(Z_t) = \sigma_t^2.$$
Zatem model ma interesującą właściwość, że jego warunkowe odchylenie standardowe $\sigma_t$, czyli zmienność, jest ciągle zmieniającą się funkcją poprzednich kwadratów wartości procesu. Jeśli jedna lub więcej z wartości $|X_{t-1}|,..., |X_{t-p}|$ jest szczególnie duża, to $X_t$ jest efektywnie losowane z rozkładu o dużej wariancji i samo może być duże; w ten sposób model generuje klastry zmienności. Nazwa ARCH odnosi się do tej struktury: model jest autoregresyjny, ponieważ $X_t$ wyraźnie zależy od poprzednich $X_{t-i}$, i warunkowo heteroskedastyczny, ponieważ wariancja warunkowa ciągle się zmienia.

Rozkład innowacji $(Z_t)_{t \in \mathbb{Z}}$ może w zasadzie być dowolnym rozkładem o zerowej średniej i jednostkowej wariancji. Do celów dopasowania statystycznego możemy, ale nie musimy, specyfikować rozkładu, w zależności od tego, czy implementujemy metodę największej wiarygodności (ML), quasi-największej wiarygodności (QML) czy nieparametryczną metodę dopasowania (patrz Sekcja 4.2.4). Dla ML najczęstszymi wyborami są standardowe innowacje normalne lub skalowane innowacje t. Przez te ostatnie rozumiemy, że $Z_t \sim t_1(\nu, 0, (\nu - 2)/\nu)$, w notacji z Przykładu 6.7, tak że wariancja rozkładu wynosi jeden.

**Model ARCH(1).** W pozostałej części tej sekcji analizujemy niektóre właściwości modelu ARCH(1). Właściwości te rozszerzają się na całą klasę modeli ARCH i GARCH, ale najłatwiej jest je wprowadzić w najprostszym przypadku. Symulowana realizacja procesu ARCH(1) z innowacjami gaussowskimi oraz odpowiadająca jej realizacja procesu zmienności są pokazane na Rysunku 4.2.

Używając $X_t^2 = \sigma_t^2 Z_t^2$ i (4.16) w przypadku p = 1, wnioskujemy, że kwadrat procesu ARCH(1) spełnia

$$X_t^2 = \alpha_0 Z_t^2 + \alpha_1 Z_t^2 X_{t-1}^2. \quad (4.18)$$

Szczegółowa analiza matematyczna modelu ARCH(1) obejmuje badanie równania (4.18), które jest stochastycznym równaniem rekurencyjnym (SRE). Podobnie jak w przypadku modelu AR(1) w Przykładzie 4.11, chcielibyśmy wiedzieć, kiedy to równanie ma stacjonarne rozwiązania wyrażone w terminach nieskończonej historii innowacji, tzn. rozwiązania postaci $X_t^2 = f(Z_t, Z_{t-1},...)$.

Dla modeli ARCH musimy starannie odróżnić rozwiązania, które są stacjonarne w sensie kowariancji, od rozwiązań, które są tylko ściśle stacjonarne. Możliwe jest istnienie modeli ARCH(1) z nieskończoną wariancją, które oczywiście nie mogą być stacjonarne w sensie kowariancji.

**Stochastyczne równania rekurencyjne.** Równanie (4.18) jest szczególnym przykładem klasy równań rekurencyjnych postaci
$$Y_t = A_t Y_{t-1} + B_t, $$
gdzie $(A_t)_{t \in \mathbb{Z}}$ i $(B_t)_{t \in \mathbb{Z}}$ są ciągami iid (niezależnych o identycznym rozkładzie) zmiennych losowych. Wystarczającymi warunkami na istnienie rozwiązania są:
$$E(\ln^+ |B_t|) < \infty \quad \text{oraz} \quad E(\ln |A_t|) < 0, $$
gdzie $\ln^+ x = \max(0, \ln x)$. Jedyne rozwiązanie jest dane przez

$$Y_t = B_t + \sum_{i=1}^{\infty} B_{t-i} \prod_{j=0}^{i-1} A_{t-j}, \quad (4.21)$$

gdzie suma zbiega bezwzględnie, prawie na pewno.

Możemy rozwinąć pewną intuicję dla warunków (4.20) i postaci rozwiązania (4.21), iterując równanie (4.19) $k$ razy, aby otrzymać

$$Y_t = A_t(A_{t-1}Y_{t-2} + B_{t-1}) + B_t$$

$$= B_t + \sum_{i=1}^{k} B_{t-i} \prod_{j=0}^{i-1} A_{t-j} + Y_{t-k-1} \prod_{i=0}^{k} A_{t-i}.$$

Warunki (4.20) zapewniają, że środkowy wyraz po prawej stronie jest zbieżny bezwzględnie, a ostatni wyraz zanika. W szczególności zauważmy, że

$$\frac{1}{k+1} \sum_{i=0}^{k} \ln|A_{t-i}| \xrightarrow{\text{p.n.}} E(\ln|A_t|) < 0$$

z mocnego prawa wielkich liczb. Zatem

$$\prod_{i=0}^{k} |A_{t-i}| = \exp\left(\sum_{i=0}^{k} \ln|A_{t-i}|\right) \xrightarrow{\text{p.n.}} 0,$$

co pokazuje znaczenie warunku $E(\ln|A_t|) < 0$. Rozwiązanie (4.21) SRE jest procesem ściśle stacjonarnym (będąc funkcją zmiennych iid $(A_s, B_s)_{s \le t}$), a warunek $E(\ln|A_t|) < 0$ okazuje się kluczowy dla ścisłej stacjonarności modeli ARCH i GARCH.

**Stacjonarność ARCH(1).** Kwadrat modelu ARCH(1) (4.18) jest SRE postaci (4.19) z $A_t = \alpha_1 Z_t^2$ i $B_t = \alpha_0 Z_t^2$. Zatem warunki w (4.20) przekładają się na wymagania, że $E(\ln^+ |\alpha_0 Z_t^2|) < \infty$, co jest automatycznie prawdą dla procesu ARCH(1), jak go zdefiniowaliśmy, oraz $E(\ln(\alpha_1 Z_t^2)) < 0$. Jest to warunek na istnienie ściśle stacjonarnego rozwiązania równań ARCH(1) i można pokazać, że jest to w rzeczywistości warunek konieczny i wystarczający dla ścisłej stacjonarności. Z (4.21) rozwiązanie równania (4.18) przyjmuje postać

$$X_t^2 = \alpha_0 \sum_{i=0}^{\infty} \alpha_1^i \prod_{j=0}^{i} Z_{t-j}^2. \quad (4.22)$$

Jeśli $(Z_t)$ są standardowymi innowacjami normalnymi, to warunek na istnienie ściśle stacjonarnego rozwiązania wynosi w przybliżeniu $\alpha_1 < 3.562$; być może nieco zaskakująco, jeśli $(Z_t)$ są skalowanymi innowacjami t z czterema stopniami swobody i wariancją 1, warunek wynosi $\alpha_1 < 5.437$. Ścisła stacjonarność zależy od rozkładu innowacji, ale stacjonarność w sensie kowariancji nie; warunkiem koniecznym i wystarczającym dla stacjonarności w sensie kowariancji jest zawsze $\alpha_1 < 1$, co teraz udowodnimy.

**Stwierdzenie 4.18.** Proces ARCH(1) jest stacjonarnym w sensie kowariancji białym szumem wtedy i tylko wtedy, gdy $\alpha_1 < 1$. Wariancja procesu stacjonarnego w sensie kowariancji jest dana przez $\alpha_0/(1 - \alpha_1)$.

*Dowód.* Przy założeniu stacjonarności kowariancyjnej, z (4.18) oraz $E(Z_t^2) = 1$ wynika, że

$$\sigma_x^2 = E(X_t^2) = \alpha_0 + \alpha_1 E(X_{t-1}^2) = \alpha_0 + \alpha_1 \sigma_x^2.$$

Oczywiście, $\sigma_x^2 = \alpha_0/(1 - \alpha_1)$ oraz musi zachodzić $\alpha_1 < 1$.

Odwrotnie, jeśli $\alpha_1 < 1$, to z nierówności Jensena,

$$E(\ln(\alpha_1 Z_t^2)) \le \ln(E(\alpha_1 Z_t^2)) = \ln(\alpha_1) < 0,$$

i możemy użyć (4.22) do obliczenia, że

$$E(X_t^2) = \alpha_0 \sum_{i=0}^{\infty} \alpha_1^i = \frac{\alpha_0}{1 - \alpha_1}.$$

Proces $(X_t)_{t \in \mathbb{Z}}$ jest procesem różnic martyngałowych ze skończonym, niezależnym od czasu momentem drugiego rzędu. Zatem jest to proces białego szumu.

Na Rysunku 4.3 przedstawiono przykłady niestacjonarnych kowariancyjnie modeli ARCH(1), jak również przykład niestacjonarnego (wybuchowego) procesu generowanego przez równania ARCH(1). Proces na Rysunku 4.2 jest stacjonarny kowariancyjnie.

**O stacjonarnym rozkładzie $X_t$.** Z (4.22) jasno wynika, że rozkład $(X_t)$ w modelu ARCH(1) ma złożony związek z rozkładem innowacji $(Z_t)$. Nawet jeśli innowacje są gaussowskie, stacjonarny rozkład szeregu czasowego nie jest gaussowski, ale raczej rozkładem leptokurtycznym z wolniej zanikającymi ogonami.

**Stwierdzenie 4.19.** Dla $m \ge 1$, ściśle stacjonarny proces ARCH(1) ma skończone momenty rzędu $2m$ wtedy i tylko wtedy, gdy $E(Z_t^{2m}) < \infty$ i $\alpha_1 < (E(Z_t^{2m}))^{-1/m}$.

*Dowód.* Zapisujemy ponownie (4.22) w postaci $X_t^2 = Z_t^2 \sum_{i=0}^{\infty} Y_{t,i}$ dla dodatnich zmiennych losowych $Y_{t,i} = \alpha_0 \alpha_1^i \prod_{j=1}^i Z_{t-j}^2, i \ge 1$, oraz $Y_{t,0} = \alpha_0$. Dla $m \ge 1$ zachodzą następujące nierówności (druga z nich to nierówność Minkowskiego):

$$E(Y_{t,1}^m) + E(Y_{t,2}^m) \le E((Y_{t,1} + Y_{t,2})^m) \le ((E(Y_{t,1}^m))^{1/m} + (E(Y_{t,2}^m))^{1/m})^m.$$

Ponieważ

$$E(X_t^{2m}) = E(Z_t^{2m})E\left(\left(\sum_{i=0}^{\infty} Y_{t,i}\right)^m\right),$$

wynika, że

$$E(Z_t^{2m})\sum_{i=0}^{\infty} E(Y_{t,i}^m) \le E(X_t^{2m}) \le E(Z_t^{2m})\left(\sum_{i=0}^{\infty} (E(Y_{t,i}^m))^{1/m}\right)^m.$$

Ponieważ $E(Y_{t,i}^m) = \alpha_0^m \alpha_1^{im} (E(Z_t^{2m}))^i$, można wywnioskować, że wszystkie trzy wielkości są skończone wtedy i tylko wtedy, gdy $E(Z_t^{2m}) < \infty$ oraz $\alpha_1^m E(Z_t^{2m}) < 1$.

Na przykład, dla skończonego momentu czwartego rzędu ($m=2$) wymagamy, aby $\alpha_1 < 1/\sqrt{3}$ w przypadku innowacji gaussowskich oraz $\alpha_1 < 1/\sqrt{6}$ w przypadku innowacji *t*-Studenta z sześcioma stopniami swobody; dla innowacji *t*-Studenta z czterema stopniami swobody moment czwartego rzędu jest niezdefiniowany.

Zakładając istnienie skończonego momentu czwartego rzędu, łatwo jest obliczyć jego wartość, a także kurtozę procesu. Podnosimy obie strony (4.18) do kwadratu, obliczamy wartość oczekiwaną obu stron, a następnie rozwiązujemy względem $E(X_t^4)$, aby otrzymać

$$E(X_t^4) = \frac{\alpha_0^2 E(Z_t^4)(1 - \alpha_1^2)}{(1 - \alpha_1^2)^2(1 - \alpha_1^2 E(Z_t^4))}.$$

Kurtozę stacjonarnego rozkładu $\kappa_X$ można następnie obliczyć jako

$$\kappa_X = \frac{E(X_t^4)}{E(X_t^2)^2} = \frac{\kappa_Z(1 - \alpha_1^2)}{(1 - \alpha_1^2 \kappa_Z)},$$

gdzie $\kappa_Z = E(Z_t^4)$ oznacza kurtozę innowacji. Oczywiście, gdy $\kappa_Z > 1$, kurtoza rozkładu stacjonarnego jest zawyżona w porównaniu z kurtozą rozkładu innowacji; dla innowacji gaussowskich lub *t*-Studenta, $\kappa_X > 3$, więc rozkład stacjonarny jest leptokurtyczny. Kurtoza procesu na Rysunku 4.2 wynosi 9.

**Paralele z procesem AR(1).** Zwracamy teraz uwagę na strukturę zależności seryjnej kwadratu szeregu w przypadku stacjonarności kowariancyjnej ($\alpha_1 < 1$). Możemy zapisać kwadrat procesu jako

$$X_t^2 = \sigma_t^2 Z_t^2 = \sigma_t^2 + \sigma_t^2(Z_t^2 - 1). \quad (4.23)$$

Przyjmując $V_t = \sigma_t^2(Z_t^2 - 1)$, zauważamy, że $(V_t)_{t \in \mathbb{Z}}$ tworzy szereg różnic martyngałowych, ponieważ $E|V_t| < \infty$ oraz $E(V_t | \mathcal{F}_{t-1}) = \sigma_t^2 E(Z_t^2 - 1) = 0$. Teraz zapisujemy ponownie (4.23) jako $X_t^2 = \alpha_0 + \alpha_1 X_{t-1}^2 + V_t$ i zauważamy, że to bardzo przypomina proces AR(1) dla $X_t^2$, z tym wyjątkiem, że $V_t$ niekoniecznie jest procesem białego szumu. Jeśli ograniczymy naszą uwagę do procesów, w których $E(X_t^4)$ jest skończone, wtedy $V_t$ ma skończony i stały drugi moment i jest procesem białego szumu. Przy tym założeniu, $X_t^2$ jest procesem AR(1), zgodnie z Definicją 4.7, postaci

$$\left(X_t^2 - \frac{\alpha_0}{1 - \alpha_1}\right) = \alpha_1 \left(X_{t-1}^2 - \frac{\alpha_0}{1 - \alpha_1}\right) + V_t.$$

Ma on średnią $\alpha_0/(1 - \alpha_1)$ i możemy wykorzystać Przykład 4.11, aby stwierdzić, że funkcja autokorelacji to $\rho(h) = \alpha_1^{|h|}, h \in \mathbb{Z}$. Rysunek 4.2 pokazuje przykład procesu ARCH(1) ze skończonym momentem czwartego rzędu, dla którego kwadraty wartości tworzą proces AR(1).

