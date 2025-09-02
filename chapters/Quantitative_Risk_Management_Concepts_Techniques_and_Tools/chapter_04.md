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

### 4.2.2 Procesy GARCH

**Definicja 4.20.** Niech $(Z_t)_{t \in \mathbb{Z}}$ będzie SWN(0, 1). Proces $(X_t)_{t \in \mathbb{Z}}$ jest procesem GARCH(p, q), jeśli jest ściśle stacjonarny i jeśli spełnia, dla wszystkich $t \in \mathbb{Z}$ oraz dla pewnego procesu o wartościach ściśle dodatnich $(\sigma_t)_{t \in \mathbb{Z}}$, równania

$$X_t = \sigma_t Z_t, \quad \sigma_t^2 = \alpha_0 + \sum_{i=1}^{p} \alpha_i X_{t-i}^2 + \sum_{j=1}^{q} \beta_j \sigma_{t-j}^2, \quad (4.24)$$

gdzie $\alpha_0 > 0$, $\alpha_i \ge 0$, $i = 1,...,p$, oraz $\beta_j \ge 0$, $j = 1,...,q$. 

Procesy GARCH są uogólnionymi procesami ARCH w tym sensie, że kwadrat zmienności $\sigma_t^2$ może zależeć od poprzednich kwadratów zmienności, a także od poprzednich kwadratów wartości procesu. 

**Model GARCH(1, 1).** W praktyce najczęściej używane są modele GARCH niskiego rzędu, dlatego skoncentrujemy się na modelu GARCH(1, 1).  W tym modelu okresy wysokiej zmienności mają tendencję do utrzymywania się, ponieważ $|X_t|$ ma szansę być duże, jeśli albo $|X_{t-1}|$ jest duże, albo $\sigma_{t-1}$ jest duże. 

**Stacjonarność.** Z (4.24) wynika, że dla modelu GARCH(1, 1) mamy

$$\sigma_t^2 = \alpha_0 + (\alpha_1 Z_{t-1}^2 + \beta_1)\sigma_{t-1}^2, \quad (4.25)$$

co jest SRE postaci $Y_t = A_t Y_{t-1} + B_t$, tak jak w (4.19).  Tym razem jest to SRE dla $Y_t = \sigma_t^2$, ale jego analiza wynika łatwo z przypadku ARCH(1).

Warunek $E(\ln|A_t|) < 0$ dla ściśle stacjonarnego rozwiązania (4.19) przekłada się na warunek $E(\ln(\alpha_1 Z_t^2 + \beta_1)) < 0$ dla (4.25), a ogólne rozwiązanie (4.21) przyjmuje postać

$$\sigma_t^2 = \alpha_0 + \alpha_0 \sum_{i=1}^{\infty} \prod_{j=1}^{i} (\alpha_1 Z_{t-j}^2 + \beta_1). \quad (4.26)$$

Jeśli $(\sigma_t^2)_{t \in \mathbb{Z}}$ jest procesem ściśle stacjonarnym, to $(X_t)_{t \in \mathbb{Z}}$ również nim jest, ponieważ $X_t = \sigma_t Z_t$ oraz $(Z_t)_{t \in \mathbb{Z}}$ jest po prostu ścisłym białym szumem. Rozwiązanie równań definiujących GARCH(1,1) jest wtedy następujące

$$X_t = Z_t \sqrt{\alpha_0 \left(1 + \sum_{i=1}^{\infty} \prod_{j=1}^{i} (\alpha_1 Z_{t-j}^2 + \beta_1)\right)}, \quad (4.27)$$

i możemy to wykorzystać do wyprowadzenia warunku stacjonarności kowariancyjnej.

**Stwierdzenie 4.21.** Proces GARCH(1, 1) jest stacjonarnym w sensie kowariancji białym szumem wtedy i tylko wtedy, gdy $\alpha_1 + \beta_1 < 1$. Wariancja procesu stacjonarnego w sensie kowariancji jest dana przez $\alpha_0/(1 - \alpha_1 - \beta_1)$.

*Dowód.* Używamy podobnego argumentu jak w Stwierdzeniu 4.18 i korzystamy z (4.27).

*Momenty czwartego rzędu i kurtoza.* Stosując podobne podejście jak w Stwierdzeniu 4.19, możemy użyć (4.27) do wyprowadzenia warunków na istnienie wyższych momentów kowariancyjnie stacjonarnego procesu GARCH(1, 1). Dla istnienia momentu czwartego rzędu, warunkiem koniecznym i wystarczającym jest, aby $E((\alpha_1 Z_t^2 + \beta_1)^2) < 1$, lub alternatywnie

$$(\alpha_1 + \beta_1)^2 < 1 - (\kappa_Z - 1)\alpha_1^2.$$

Zakładając, że jest to prawdą, obliczamy moment czwartego rzędu i kurtozę $X_t$. Podnosimy obie strony (4.25) do kwadratu i obliczamy wartości oczekiwane, aby otrzymać

$$E(\sigma_t^4) = \alpha_0^2 + (\alpha_1^2 \kappa_Z + \beta_1^2 + 2\alpha_1 \beta_1)E(\sigma_t^4) + 2\alpha_0(\alpha_1 + \beta_1)E(\sigma_t^2).$$

Rozwiązując względem $E(\sigma_t^4)$, przypominając, że $E(\sigma_t^2) = E(X_t^2) = \alpha_0/(1 - \alpha_1 - \beta_1)$, i podstawiając $E(X_t^4) = \kappa_Z E(\sigma_t^4)$, otrzymujemy

$$E(X_t^4) = \frac{\alpha_0^2 \kappa_Z (1 - (\alpha_1 + \beta_1)^2)}{(1 - \alpha_1 - \beta_1)^2(1 - \alpha_1^2 \kappa_Z - \beta_1^2 - 2\alpha_1 \beta_1)},$$

z czego wynika, że

$$\kappa_X = \frac{\kappa_Z(1 - (\alpha_1 + \beta_1)^2)}{(1 - (\alpha_1 + \beta_1)^2) - (\kappa_Z - 1)\alpha_1^2}.$$

Ponownie jest jasne, że kurtoza $X_t$ jest większa niż kurtoza $Z_t$, gdy tylko $\kappa_Z > 1$, tak jak w przypadku innowacji gaussowskich i skalowanych *t*-Studenta. Kurtoza modelu GARCH(1, 1) na Rysunku 4.4 wynosi 3.77.

**Paralele z procesem ARMA(1, 1).** Używając tej samej reprezentacji co w równaniu (4.23), stacjonarny w sensie kowariancji proces GARCH(1, 1) można zapisać jako
$$X_t^2 = \alpha_0 + \alpha_1 X_{t-1}^2 + \beta_1 \sigma_{t-1}^2 + V_t,$$
gdzie $V_t$ jest ciągiem różnic martyngałowych. Ponieważ $\sigma_{t-1}^2 = X_{t-1}^2 - V_{t-1}$, możemy zapisać

$$X_t^2 = \alpha_0 + (\alpha_1 + \beta_1)X_{t-1}^2 - \beta_1 V_{t-1} + V_t, \quad (4.28)$$

co zaczyna przypominać proces ARMA(1, 1) dla $X_t^2$. Jeśli dalej założymy, że $E(X_t^4) < \infty$, wtedy, przypominając, że $\alpha_1 + \beta_1 < 1$, formalnie mamy, że

$$\left(X_t^2 - \frac{\alpha_0}{1 - \alpha_1 - \beta_1}\right) = (\alpha_1 + \beta_1) \left(X_{t-1}^2 - \frac{\alpha_0}{1 - \alpha_1 - \beta_1}\right) - \beta_1 V_{t-1} + V_t$$

jest procesem ARMA(1, 1). Rysunek 4.4 pokazuje przykład procesu GARCH(1, 1) ze skończonym momentem czwartego rzędu, którego kwadraty wartości tworzą proces ARMA(1, 1).

*Model GARCH(p, q).* Modele ARCH i GARCH wyższego rzędu mają takie samo ogólne zachowanie jak ARCH(1) i GARCH(1, 1), ale ich analiza matematyczna staje się bardziej żmudna. Warunek dla ściśle stacjonarnego rozwiązania definiującego SRE został wyprowadzony przez Bougerola i Picarda (1992), ale jest on skomplikowany. Warunkiem koniecznym i wystarczającym, aby to rozwiązanie było stacjonarne kowariancyjnie, jest $\sum_{i=1}^p \alpha_i + \sum_{j=1}^q \beta_j < 1$.

Proces kwadratów GARCH(p, q) ma strukturę

$$X_t^2 = \alpha_0 + \sum_{i=1}^{\max(p,q)} (\alpha_i + \beta_i)X_{t-i}^2 - \sum_{j=1}^q \beta_j V_{t-j} + V_t,$$

gdzie $\alpha_i = 0$ dla $i = p+1, \dots, q$ jeśli $q > p$, lub $\beta_j = 0$ dla $j = q+1, \dots, p$ jeśli $p > q$. Przypomina to proces ARMA(max(p, q), q) i formalnie jest takim procesem, pod warunkiem, że $E(X_t)^4 < \infty$.

*Zintegrowany GARCH.* Badania nad zintegrowanymi procesami GARCH (lub IGARCH) były motywowane faktem, że w niektórych zastosowaniach modelowania GARCH do dziennych lub o wyższej częstotliwości szeregów zwrotów z czynników ryzyka, obserwuje się, że estymowane współczynniki ARCH i GARCH $(\alpha_1, \dots, \alpha_p, \beta_1, \dots, \beta_q)$ sumują się do liczby bardzo bliskiej 1, a czasami nawet nieznacznie większej niż 1. W modelu, w którym $\sum_{i=1}^p \alpha_i + \sum_{j=1}^q \beta_j \ge 1$, proces ma *nieskończoną wariancję* i w związku z tym jest niestacjonarny kowariancyjnie. Szczególny przypadek, w którym $\sum_{i=1}^p \alpha_i + \sum_{j=1}^q \beta_j = 1$, jest znany jako IGARCH i poświęcono mu pewną uwagę.

Dla uproszczenia, rozważmy model IGARCH(1, 1). Używamy (4.28), aby stwierdzić, że proces kwadratów musi spełniać

$$\nabla X_t^2 = X_t^2 - X_{t-1}^2 = \alpha_0 - (1-\alpha_1)V_{t-1} + V_t,$$

gdzie $V_t$ jest sekwencją szumu zdefiniowaną przez $V_t = \sigma_t^2(Z_t^2 - 1)$ oraz $\sigma_t^2 = \alpha_0 + \alpha_1 X_{t-1}^2 + (1-\alpha_1)\sigma_{t-1}^2$. To równanie przypomina model ARIMA(0, 1, 1) (zobacz (4.12)) dla $X_t^2$, chociaż szum $V_t$ nie jest białym szumem, ani, ściśle mówiąc, nie jest różnicą martyngałową zgodnie z Definicją 4.6. $E(V_t | \mathcal{F}_{t-1})$ jest niezdefiniowane, ponieważ $E(\sigma_t^2) = E(X_t^2) = \infty$, i w związku z tym $E|V_t|$ jest niezdefiniowane.

### 4.2.3 Proste Rozszerzenia Modelu GARCH

Zaproponowano wiele wariantów i rozszerzeń podstawowego modelu GARCH. Wspominamy tylko o kilku.

**Modele ARMA z błędami GARCH.** Ustawiamy błąd ARMA $\epsilon_t$ równy $\sigma_t Z_t$, gdzie $\sigma_t$ podlega specyfikacji zmienności GARCH w terminach historycznych wartości $\epsilon_t$. Daje nam to elastyczną rodzinę modeli ARMA z błędami GARCH, która łączy cechy obu klas modeli.

**Definicja 4.22.** Proces $(X_t)_{t \in \mathbb{Z}}$ jest procesem ARMA($p_1$, $q_1$) z błędami GARCH($p_2$, $q_2$), jeśli jest stacjonarny w sensie kowariancji i spełnia równania różnicowe postaci

$$X_t = \mu_t + \sigma_t Z_t,$$
$$\mu_t = \mu + \sum_{i=1}^{p_1} \phi_i(X_{t-i} - \mu) + \sum_{j=1}^{q_1} \theta_j(X_{t-j} - \mu_{t-j}),$$
$$\sigma_t^2 = \alpha_0 + \sum_{i=1}^{p_2} \alpha_i(X_{t-i} - \mu_{t-i})^2 + \sum_{j=1}^{q_2} \beta_j \sigma_{t-j}^2. $$

gdzie $\alpha_0 > 0$, $\alpha_i \ge 0$, $i = 1, \dots, p_2$, $\beta_j \ge 0$, $j=1, \dots, q_2$, oraz $\sum_{i=1}^{p_2} \alpha_i + \sum_{j=1}^{q_2} \beta_j < 1$.

Aby zachować spójność z poprzednią definicją procesu ARMA, wbudowujemy warunek stacjonarności kowariancyjnej dla błędów GARCH w definicję. Aby proces ARMA był przyczynowym i odwracalnym procesem liniowym, tak jak poprzednio, wielomiany $\tilde{\phi}(z) = 1 - \phi_1 z - \dots - \phi_{p_1} z^{p_1}$ oraz $\tilde{\theta}(z) = 1 + \theta_1 z + \dots + \theta_{q_1} z^{q_1}$ nie powinny mieć wspólnych pierwiastków ani pierwiastków wewnątrz koła jednostkowego.

Niech $(\mathcal{F}_t)_{t \in \mathbb{Z}}$ oznacza naturalną filtrację $(X_t)_{t \in \mathbb{Z}}$ i załóżmy, że model ARMA jest odwracalny. Odwracalność procesu ARMA zapewnia, że $\mu_t$ jest $\mathcal{F}_{t-1}$-mierzalne, tak jak w (4.11). Ponadto, ponieważ $\sigma_t$ zależy od nieskończonej historii $(X_s - \mu_s)_{s \le t-1}$, odwracalność ARMA zapewnia również, że $\sigma_t$ jest $\mathcal{F}_{t-1}$-mierzalne. Proste obliczenia pokazują, że $\mu_t = E(X_t | \mathcal{F}_{t-1})$ oraz $\sigma_t^2 = \text{var}(X_t | \mathcal{F}_{t-1})$, więc $\mu_t$ i $\sigma_t^2$ są warunkową średnią i wariancją nowego procesu.

*GARCH z dźwignią.* Jednym z głównych zarzutów wobec standardowych modeli ARCH i GARCH jest sztywno symetryczny sposób, w jaki zmienność reaguje na ostatnie stopy zwrotu, niezależnie od ich znaku. Teoria ekonomii sugeruje, że informacje rynkowe powinny mieć asymetryczny wpływ na zmienność, według którego złe wiadomości prowadzące do spadku wartości kapitału własnego firmy mają tendencję do zwiększania zmienności. Zjawisko to zostało nazwane *efektem dźwigni*, ponieważ spadek wartości kapitału własnego powoduje wzrost wskaźnika zadłużenia do kapitału własnego, czyli tak zwanej dźwigni finansowej firmy, i w konsekwencji powinien sprawić, że akcje staną się bardziej zmienne. Na mniej teoretycznym poziomie wydaje się rozsądne, że spadające wartości akcji mogą prowadzić do wyższego poziomu nerwowości inwestorów niż wzrosty wartości o tej samej wielkości.

Jedną z metod dodania efektu dźwigni do modelu GARCH(1, 1) jest wprowadzenie dodatkowego parametru do równania zmienności (4.24), aby otrzymać

$$\sigma_t^2 = \alpha_0 + \alpha_1(X_{t-1} + \delta|X_{t-1}|)^2 + \beta_1\sigma_{t-1}^2. \quad (4.29)$$

Zakładamy, że $\delta \in [-1, 1]$ oraz $\alpha_1 \ge 0$, tak jak w modelu GARCH(1, 1). Zauważmy, że (4.29) można zapisać jako

$$\sigma_t^2 = \begin{cases} \alpha_0 + \alpha_1(1 + \delta)^2 X_{t-1}^2 + \beta_1\sigma_{t-1}^2, & X_{t-1} \ge 0, \\ \alpha_0 + \alpha_1(1 - \delta)^2 X_{t-1}^2 + \beta_1\sigma_{t-1}^2, & X_{t-1} < 0, \end{cases}$$

a stąd, że

$$\frac{\partial \sigma_t^2}{\partial X_{t-1}^2} = \begin{cases} \alpha_1(1 + \delta)^2, & X_{t-1} \ge 0, \\ \alpha_1(1 - \delta)^2, & X_{t-1} < 0. \end{cases}$$

Reakcja zmienności na wielkość ostatniej stopy zwrotu zależy od znaku tej stopy zwrotu i generalnie oczekujemy, że $\delta < 0$, więc złe wiadomości mają większy wpływ.

*Progowy GARCH.* Zauważmy, że (4.29) można łatwo przepisać w postaci

$$\sigma_t^2 = \alpha_0 + \tilde{\alpha}_1 X_{t-1}^2 + \tilde{\delta} I_{\{X_{t-1} < 0\}} X_{t-1}^2 + \beta_1 \sigma_{t-1}^2, \quad (4.30)$$

gdzie $\tilde{\alpha}_1 = \alpha_1(1+\delta)^2$ oraz $\tilde{\delta} = -4\delta\alpha_1$. Równanie (4.30) przedstawia najczęstszą wersję progowego modelu GARCH (lub TGARCH). W efekcie, ustalono próg na poziomie zerowym, i w czasie *t* dynamika zależy od tego, czy poprzednia wartość procesu $X_{t-1}$ (lub innowacji $Z_{t-1}$) była poniżej czy powyżej tego progu. Jednakże, możliwe jest również ustawienie niezerowych progów w modelach TGARCH, więc reprezentuje to bardziej ogólną klasę modeli niż GARCH z dźwignią.

W mniej popularnej wersji progowego GARCH, współczynniki efektów GARCH zależą od znaków poprzednich wartości procesu; daje to proces pierwszego rzędu postaci

$$\sigma_t^2 = \alpha_0 + \alpha_1 X_{t-1}^2 + \beta_1 \sigma_{t-1}^2 + \delta I_{\{X_{t-1} < 0\}} \sigma_{t-1}^2. \quad (4.31)$$

**Uwaga 4.23.** Należy również zauważyć, że kolejnym sposobem na wprowadzenie asymetrii do modelu GARCH jest jawne użycie asymetrycznego rozkładu innowacji (aczkolwiek znormalizowanego tak, aby miał średnią 0 i wariancję 1). Rozkłady kandydujące mogą pochodzić z uogólnionej rodziny hiperbolicznej z Sekcji 6.2.3.

### 4.2.4 Dopasowywanie Modeli GARCH do Danych

*Konstruowanie funkcji wiarygodności.* W praktyce, najczęściej stosowanym podejściem do dopasowywania modeli GARCH do danych jest metoda największej wiarygodności. Rozważymy kolejno dopasowanie modeli ARCH(1) i GARCH(1, 1), z czego łatwo wynika dopasowanie ogólnych modeli ARCH(p) i GARCH(p, q).

Dla modeli ARCH(1) i GARCH(1, 1), załóżmy, że mamy łącznie n + 1 wartości danych $X_0, X_1, \dots, X_n$. Warto przypomnieć, że możemy zapisać łączną gęstość odpowiednich zmiennych losowych jako

$$f_{X_0, \dots, X_n}(x_0, \dots, x_n) = f_{X_0}(x_0) \prod_{t=1}^{n} f_{X_t|X_{t-1}, \dots, x_0}(x_t | x_{t-1}, \dots, x_0). \quad (4.32)$$

Dla czystego procesu ARCH(1), który jest markowowski pierwszego rzędu, gęstości warunkowe $f_{X_t|X_{t-1}, \dots, x_0}$ w (4.32) zależą od przeszłości jedynie poprzez wartość $\sigma_t$ lub, równoważnie, $X_{t-1}$. Gęstość warunkowa jest łatwa do obliczenia jako

$$f_{X_t|X_{t-1}, \dots, x_0}(x_t | x_{t-1}, \dots, x_0) = f_{X_t|X_{t-1}}(x_t | x_{t-1}) = \frac{1}{\sigma_t} f_Z\left(\frac{x_t}{\sigma_t}\right), \quad (4.33)$$

gdzie $\sigma_t = (\alpha_0 + \alpha_1 x_{t-1}^2)^{1/2}$, a $f_Z(z)$ oznacza gęstość innowacji $(Z_t)_{t \in \mathbb{Z}}$. Przypomnijmy, że musi ona mieć średnią 0 i wariancję 1, a typowymi wyborami byłyby standardowa gęstość normalna lub gęstość rozkładu *t*-Studenta przeskalowana tak, aby miała jednostkową wariancję.

Jednakże, gęstość brzegowa $f_{X_0}$ w (4.32) nie jest znana w jawnej, analitycznej postaci dla modeli ARCH i GARCH, i stanowi to problem przy opieraniu funkcji wiarygodności na (4.32). Rozwiązaniem stosowanym w praktyce jest skonstruowanie warunkowej funkcji wiarygodności przy danym $X_0$, którą oblicza się z

$$f_{X_1, \dots, X_n|X_0}(x_1, \dots, x_n | x_0) = \prod_{t=1}^{n} f_{X_t|X_{t-1}, \dots, x_0}(x_t | x_{t-1}, \dots, x_0). \quad (4.34)$$

Dla modelu ARCH(1) wynika to z (4.33) i ma postać

$$L(\alpha_0, \alpha_1; \mathbf{X}) = f_{X_1, \dots, X_n|X_0}(X_1, \dots, X_n | X_0) = \prod_{t=1}^{n} \frac{1}{\sigma_t} f_Z\left(\frac{X_t}{\sigma_t}\right),$$

gdzie $\sigma_t = (\alpha_0 + \alpha_1 X_{t-1}^2)^{1/2}$. Dla modelu ARCH(p) użylibyśmy analogicznych argumentów, aby zapisać funkcję wiarygodności warunkową względem pierwszych *p* wartości.

W modelu GARCH(1, 1), $\sigma_t$ jest zdefiniowane rekurencyjnie w zależności od $\sigma_{t-1}$, i tutaj, zamiast używać (4.34), konstruujemy łączną gęstość $X_1, \dots, X_n$ warunkową względem zrealizowanych wartości zarówno $X_0$ jak i $\sigma_0$, która ma postać

$$f_{X_1, \dots, X_n|X_0, \sigma_0}(x_1, \dots, x_n | x_0, \sigma_0) = \prod_{t=1}^{n} f_{X_t|X_{t-1}, \dots, x_0, \sigma_0}(x_t | x_{t-1}, \dots, x_0, \sigma_0).$$

Gęstości warunkowe $f_{X_t|X_{t-1}, \dots, X_0, \sigma_0}$ zależą od przeszłości tylko poprzez wartość $\sigma_t$, która jest dana rekurencyjnie z $\sigma_0, X_0, \dots, X_{t-1}$ używając $\sigma_t^2 = \alpha_0 + \alpha_1 X_{t-1}^2 + \beta_1 \sigma_{t-1}^2$. Daje nam to warunkową funkcję wiarygodności

$$L(\alpha_0, \alpha_1, \beta_1; \mathbf{X}) = \prod_{t=1}^{n} \frac{1}{\sigma_t} f_Z\left(\frac{X_t}{\sigma_t}\right), \quad \sigma_t = \sqrt{\alpha_0 + \alpha_1 X_{t-1}^2 + \beta_1 \sigma_{t-1}^2}.$$

Pozostaje problem, że wartość $\sigma_0^2$ nie jest w rzeczywistości obserwowana, i jest to zwykle rozwiązywane przez wybór wartości początkowej, takiej jak wariancja z próby dla $X_1, \dots, X_n$, lub po prostu zero.

Dla modelu GARCH(p, q) założylibyśmy, że mamy n + p wartości danych oznaczonych jako $X_{-p+1}, \dots, X_0, X_1, \dots, X_n$. Obliczylibyśmy warunkową funkcję wiarygodności na podstawie (obserwowanych) wartości $X_0, X_1, \dots, X_n$ oraz $X_{-p+1}, \dots, X_0$ jak również (nieobserwowanych) wartości $\sigma_{-q+1}, \dots, \sigma_0$, dla których wartości początkowe musiałyby zostać użyte jak powyżej. Na przykład, jeśli p = 1 i q = 3, wymagamy wartości początkowych dla $\sigma_0, \sigma_{-1}$ i $\sigma_{-2}$.

Podobne podejście można zastosować do opracowania funkcji wiarygodności dla modelu ARMA z błędami GARCH. W tym przypadku otrzymalibyśmy warunkową funkcję wiarygodności postaci

$$L(\theta; \mathbf{X}) = \prod_{t=1}^{n} \frac{1}{\sigma_t} f_Z\left(\frac{X_t - \mu_t}{\sigma_t}\right),$$

gdzie $\sigma_t$ jest zgodne ze specyfikacją GARCH z Definicji 4.22, a $\mu_t$ jest zgodne ze specyfikacją ARMA, a wszystkie nieznane parametry (w tym ewentualnie nieznane parametry rozkładu innowacji) zostały zebrane w wektorze $\theta$. Moglibyśmy oczywiście rozważać również modele z efektem dźwigni lub progowymi.