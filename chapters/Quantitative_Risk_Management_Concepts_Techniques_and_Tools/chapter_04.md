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