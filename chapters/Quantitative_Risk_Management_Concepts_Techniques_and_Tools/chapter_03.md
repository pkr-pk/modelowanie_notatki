# Rozdział 3: Empiryczne własności danych finansowych

## 3.1 Stylizowane fakty dotyczące finansowych szeregów czasowych zwrotów

**Stylizowane fakty (stylized facts)** dotyczące finansowych szeregów czasowych to zbiór obserwacji empirycznych i wniosków z nich płynących, które odnoszą się do wielu szeregów czasowych zmian czynników ryzyka, takich jak logarytmiczne stopy zwrotu z akcji, indeksów, kursów walutowych i cen towarów. Obserwacje te są tak głęboko zakorzenione w doświadczeniu ekonometrycznym, że nadano im status faktów.

Stylizowane fakty, które opisujemy, zazwyczaj dotyczą szeregów czasowych dziennych logarytmicznych stóp zwrotu i często pozostają prawdziwe, gdy rozważamy szeregi o dłuższych interwałach, takie jak tygodniowe czy miesięczne stopy zwrotu, lub krótszych, jak zwroty w ciągu dnia. Większość modeli zarządzania ryzykiem opiera się na danych zbieranych z taką częstotliwością.

Finansowe szeregi czasowe o bardzo wysokiej częstotliwości, takie jak dane tick-by-tick, mają swoje własne stylizowane fakty, ale nie będzie to przedmiotem tego rozdziału. Co więcej, właściwości danych o bardzo niskiej częstotliwości (takich jak roczne stopy zwrotu) są trudniejsze do określenia ze względu na rzadkość takich danych i trudność w założeniu, że są one generowane w długoterminowych warunkach stacjonarnych.

Dla pojedynczego szeregu czasowego zwrotów finansowych, wersja stylizowanych faktów jest następująca:
1.  Szeregi zwrotów nie są niezależne i o identycznym rozkładzie (iid), chociaż wykazują niewielką autokorelację.
2.  Szeregi zwrotów absolutnych lub podniesionych do kwadratu wykazują głęboką autokorelację.
3.  Warunkowe oczekiwane stopy zwrotu są bliskie zera.
4.  Zmienność wydaje się zmieniać w czasie.
5.  Ekstremalne zwroty pojawiają się w skupiskach (klastrach).
6.  Szeregi zwrotów są leptokurtyczne lub grubogonowe (heavy-tailed).

Aby dalej omówić te obserwacje, oznaczmy szereg zwrotów jako $X_1, ..., X_n$ i załóżmy, że zwroty zostały utworzone przez logarytmiczne różnicowanie szeregu cen, indeksu lub kursu walutowego $(S_t)_{t=0,1,...,n}$, a zatem $X_t = \ln(S_t / S_{t-1})$, dla $t = 1, ..., n$.

### 3.1.1 Klastrowanie zmienności

Dowody na pierwsze dwa stylizowane fakty zebrano na Rysunkach 3.1 i 3.2. Rysunek 3.1 (a) przedstawia 2608 dziennych logarytmicznych stóp zwrotu dla indeksu DAX, obejmujących dekadę od 2 stycznia 1985 do 30 grudnia 1994, okres obejmujący zarówno krach giełdowy w 1987 roku, jak i zjednoczenie Niemiec. Części (b) i (c) przedstawiają szeregi symulowanych danych iid z modelu normalnego i modelu t-Studenta; w obu przypadkach parametry modelu zostały ustawione poprzez dopasowanie modelu do rzeczywistych danych zwrotów metodą największej wiarygodności przy założeniu iid. W przypadku rozkładu normalnego oznacza to po prostu symulację danych iid z rozkładem $N(\mu, \sigma^2)$, gdzie $\mu = \bar{X} = n^{-1} \sum_{i=1}^{n} X_i$ a $\sigma^2 = n^{-1} \sum_{i=1}^{n} (X_i - \bar{X})^2$. W przypadku rozkładu t, wiarygodność została zmaksymalizowana numerycznie, a oszacowany parametr stopni swobody wynosi $\nu = 3.8$.

Symulowane dane normalne wyraźnie różnią się od danych zwrotów z indeksu DAX i nie wykazują tego samego zakresu wartości ekstremalnych. Chociaż model t-Studenta może generować porównywalne wartości ekstremalne do danych rzeczywistych, uważniejsza obserwacja ujawnia, że rzeczywiste zwroty wykazują zjawisko znane jako **klastrowanie zmienności**, które nie jest obecne w symulowanych seriach. Klastrowanie zmienności to tendencja do występowania ekstremalnych zwrotów po innych ekstremalnych zwrotach, chociaż niekoniecznie o tym samym znaku. Możemy zaobserwować okresy takie jak krach giełdowy w październiku 1987 roku czy niepewność polityczna w okresie od końca 1989 roku do zjednoczenia Niemiec w 1990 roku w danych DAX: charakteryzują się one dużymi ruchami dodatnimi i ujemnymi.

Na Rysunku 3.2 przedstawiono korelogramy surowych danych i danych absolutnych dla wszystkich trzech zbiorów danych. Korelogram jest graficznym przedstawieniem oszacowań autokorelacji, a jego konstrukcja i interpretacja są omówione w Sekcji 4.1.3. Podczas gdy istnieje bardzo mało dowodów na autokorelację w surowych danych dla wszystkich zbiorów danych, wartości absolutne rzeczywistych danych finansowych wydają się wykazywać dowody na zależność seryjną. Wyraźnie ponad 5% oszacowanych korelacji leży poza liniami przerywanymi, które są 95% przedziałami ufności dla korelacji seryjnych, gdy podstawowy proces składa się z losowych zmiennych iid o skończonej wariancji. Ta zależność seryjna w zwrotach absolutnych byłaby równie widoczna w wartościach zwrotów podniesionych do kwadratu i wydaje się potwierdzać obecność klastrowania zmienności. Wnioskujemy, że chociaż nie ma dowodów przeciwko hipotezie iid dla danych rzeczywiście iid, istnieją silne dowody przeciwko hipotezie iid dla danych zwrotów z indeksu DAX.

**Tabela 3.1.** Testy losowości stóp zwrotu 30 spółek z indeksu Dow Jones w ośmioletnim okresie 1993–2000. Kolumny LBraw i LBabs pokazują wartości *p* dla testów Ljunga-Boxa zastosowanych odpowiednio do surowych i bezwzględnych wartości.

| Nazwa | Symbol | Daily LBraw | Daily LBabs | Monthly LBraw | Monthly LBabs |
|---|---|---|---|---|---|
| Alcoa | AA | 0.00 | 0.00 | 0.23 | 0.02 |
| American Express | AXP | 0.02 | 0.00 | 0.55 | 0.07 |
| AT&T | T | 0.11 | 0.00 | 0.70 | 0.02 |
| Boeing | BA | 0.03 | 0.00 | 0.90 | 0.17 |
| Caterpillar | CAT | 0.28 | 0.00 | 0.73 | 0.07 |
| Citigroup | C | 0.09 | 0.00 | 0.91 | 0.48 |
| Coca-Cola | KO | 0.00 | 0.00 | 0.50 | 0.03 |
| DuPont | DD | 0.03 | 0.00 | 0.75 | 0.00 |
| Eastman Kodak | EK | 0.15 | 0.00 | 0.61 | 0.54 |
| Exxon Mobil | XOM | 0.00 | 0.00 | 0.32 | 0.22 |
| General Electric | GE | 0.00 | 0.00 | 0.25 | 0.09 |
| General Motors | GM | 0.65 | 0.00 | 0.81 | 0.27 |
| Hewlett-Packard | HWP | 0.09 | 0.00 | 0.21 | 0.02 |
| Home Depot | HD | 0.00 | 0.00 | 0.00 | 0.41 |
| Honeywell | HON | 0.44 | 0.00 | 0.07 | 0.30 |
| Intel | INTC | 0.23 | 0.00 | 0.79 | 0.62 |
| IBM | IBM | 0.18 | 0.00 | 0.67 | 0.28 |
| International Paper | IP | 0.15 | 0.00 | 0.01 | 0.09 |
| JPMorgan | JPM | 0.52 | 0.00 | 0.43 | 0.12 |
| Johnson & Johnson | JNJ | 0.00 | 0.00 | 0.11 | 0.91 |
| McDonald's | MCD | 0.28 | 0.00 | 0.72 | 0.68 |
| Merck | MRK | 0.05 | 0.00 | 0.53 | 0.65 |
| Microsoft | MSFT | 0.28 | 0.00 | 0.19 | 0.13 |
| 3M | MMM | 0.00 | 0.00 | 0.57 | 0.33 |
| Philip Morris | MO | 0.01 | 0.00 | 0.68 | 0.82 |
| Procter & Gamble | PG | 0.02 | 0.00 | 0.99 | 0.74 |
| SBC | SBC | 0.05 | 0.00 | 0.13 | 0.00 |
| United Technologies | UTX | 0.00 | 0.00 | 0.12 | 0.01 |
| Wal-Mart | WMT | 0.00 | 0.00 | 0.41 | 0.64 |
| Disney | DIS | 0.44 | 0.00 | 0.01 | 0.51 |

Tabela 3.1 zawiera więcej dowodów przeciwko hipotezie iid dla dziennych danych zwrotów z akcji. Test losowości Ljunga-Boxa (opisany w Sekcji 4.1.3) został przeprowadzony dla akcji wchodzących w skład indeksu Dow Jones 30 w okresie 1993–2000. W dwóch kolumnach dla zwrotów dziennych test jest stosowany odpowiednio do surowych danych zwrotów (LBraw) i ich wartości bezwzględnych (LBabs), a w tabeli podano wartości p; wykazują one silne dowody (szczególnie w przypadku zastosowania do wartości bezwzględnych) przeciwko hipotezie iid. Jeśli logarytmiczne stopy zwrotu nie są iid, jest to sprzeczne z popularną hipotezą błądzenia losowego dla dyskretnego w czasie rozwoju cen logarytmicznych (lub w tym przypadku wartości indeksu). Jeśli logarytmiczne stopy zwrotu nie są ani iid, ani normalne, jest to sprzeczne z hipotezą geometrycznego ruchu Browna dla ciągłego w czasie rozwoju cen, na której opiera się teoria wyceny Blacka-Scholesa-Mertona.

Co więcej, jeśli w danych zwrotów finansowych występuje zależność seryjna, pojawia się pytanie: w jakim stopniu zależność ta może być wykorzystana do przewidywania przyszłości? Jest to tematem trzeciego i czwartego stylizowanego faktu. Bardzo trudno jest przewidzieć zwrot w następnym okresie czasu, opierając się wyłącznie na danych historycznych. Ta trudność w przewidywaniu przyszłych zwrotów jest częścią dowodów na rzecz dobrze znanej **hipotezy rynków efektywnych** w finansach, która mówi, że ceny szybko reagują, aby odzwierciedlić wszystkie dostępne informacje na temat danego aktywa.

Z empirycznego punktu widzenia, brak przewidywalności zwrotów objawia się brakiem autokorelacji w surowych szeregach danych zwrotów. Dla niektórych szeregów czasami obserwujemy dowody na korelacje przy pierwszym (lub kilku pierwszych) opóźnieniu. Mała dodatnia korelacja przy pierwszym opóźnieniu sugerowałaby, że istnieje pewna dostrzegalna tendencja, że po zwrocie o określonym znaku (dodatnim lub ujemnym) w następnym okresie nastąpi zwrot o tym samym znaku. Jednakże nie jest to widoczne w danych DAX, co sugeruje, że nasza najlepsza estymacja jutrzejszego zwrotu, oparta na naszych obserwacjach do dzisiaj, wynosi zero. Idea ta jest wyrażona w stwierdzeniu trzeciego stylizowanego faktu: że **warunkowe oczekiwane stopy zwrotu są bliskie zera**.

Zmienność jest często formalnie modelowana jako warunkowe odchylenie standardowe zwrotów finansowych, biorąc pod uwagę informacje historyczne, i chociaż warunkowe oczekiwane zwroty są konsekwentnie bliskie zeru, obecność klastrowania zmienności sugeruje, że warunkowe odchylenia standardowe ciągle się zmieniają w częściowo przewidywalny sposób. Jeśli wiemy, że zwroty były duże w ostatnich kilku dniach z powodu ożywienia na rynku, istnieje powód, aby sądzić, że rozkład, z którego „losowany” jest jutrzejszy zwrot, powinien mieć dużą wariancję. To właśnie ta idea leży u podstaw modeli szeregów czasowych dla zmieniającej się zmienności, które zbadamy w Rozdziale 4.

Dalsze dowody na klastrowanie zmienności przedstawiono na Rysunku 3.3, gdzie wykreślono szeregi czasowe 100 największych dziennych strat dla zwrotów z indeksu DAX oraz 100 największych wartości dla symulowanych danych t. W Sekcji 5.3.1 podsumowujemy teorię, która sugeruje, że największe wartości w danych iid będą występować jak zdarzenia w procesie Poissona, oddzielone czasami oczekiwania, które są iid o rozkładzie wykładniczym. Części (b) i (d) rysunku przedstawiają wykresy kwantylowe (Q-Q) tych czasów oczekiwania w odniesieniu do referencyjnego rozkładu wykładniczego. Podczas gdy hipoteza o losowym występowaniu wartości ekstremalnych dla danych iid jest potwierdzona, w danych DAX występuje zbyt wiele krótkich i długich czasów oczekiwania spowodowanych klastrowaniem wartości ekstremalnych, aby potwierdzić hipotezę wykładniczą. Piąty stylizowany fakt stanowi zatem kolejny silny dowód przeciwko hipotezie iid dla danych zwrotów.

W Rozdziale 4 wprowadzimy modele szeregów czasowych, które charakteryzują się klastrowaniem zmienności, jakie obserwujemy w rzeczywistych danych zwrotów. W szczególności opiszemy modele ARCH i GARCH, które mogą replikować wszystkie dotychczas omówione ustalone fakty, a także typową nienormalność zwrotów, poruszoną w szóstym stylizowanym fakcie, do którego teraz przechodzimy.

### 3.1.2 Nienormalność i grube ogony

Często obserwuje się, że **rozkład normalny jest słabym modelem** dla dziennych, tygodniowych, a nawet miesięcznych stóp zwrotu. Można to potwierdzić za pomocą różnych dobrze znanych testów normalności, w tym **wykresu kwantylowo-kwantylowego (Q-Q)** w odniesieniu do standardowego normalnego rozkładu odniesienia, a także za pomocą szeregu formalnych testów numerycznych.

Wykres Q-Q (wykres kwantyl-kwantyl) jest standardowym narzędziem wizualnym do pokazywania zależności między kwantylami empirycznymi danych a kwantylami teoretycznymi rozkładu odniesienia. Brak liniowości na wykresie Q-Q interpretuje się jako dowód przeciwko hipotetycznemu rozkładowi odniesienia. Na Rycinie 3.4 pokazano wykres Q-Q dziennych stóp zwrotu z akcji Disneya w latach 1993-2000 w odniesieniu do normalnego rozkładu odniesienia; odwrócona krzywa w kształcie litery S utworzona przez punkty sugeruje, że bardziej skrajne kwantyle empiryczne danych są zazwyczaj większe niż odpowiadające im kwantyle rozkładu normalnego, co wskazuje, że rozkład normalny jest słabym modelem dla tych stóp zwrotu.

Popularne testy numeryczne obejmują testy **Jarque'a i Bery, Andersona i Darlinga, Shapiro i Wilka oraz D’Agostino**. Test Jarque'a-Bery należy do klasy omnibusowych testów momentów, tj. testów, które jednocześnie oceniają, czy **skośność (skewness)** i **kurtoza (kurtosis)** danych są zgodne z modelem Gaussa. Współczynniki skośności i kurtozy z próby są zdefiniowane przez:

$$b = \frac{(1/n)\sum_{i=1}^{n}(X_i - \bar{X})^3}{((1/n)\sum_{i=1}^{n}(X_i - \bar{X})^2)^{3/2}}, \quad k = \frac{(1/n)\sum_{i=1}^{n}(X_i - \bar{X})^4}{((1/n)\sum_{i=1}^{n}(X_i - \bar{X})^2)^2} \quad (3.1)$$

Są one przeznaczone do szacowania teoretycznej skośności i kurtozy, które są zdefiniowane odpowiednio przez $\beta = E(X - \mu)^3/\sigma^3$ oraz $\kappa = E(X - \mu)^4/\sigma^4$, gdzie $\mu = E(X)$ i $\sigma^2 = \text{var}(X)$ oznaczają średnią i wariancję; $\beta$ i $\kappa$ przyjmują wartości zero i trzy dla zmiennej losowej X o rozkładzie normalnym. Statystyka testowa Jarque'a-Bery to:

$$T = \frac{1}{6}n(b^2 + \frac{1}{4}(k-3)^2) \quad (3.2)$$

i ma asymptotyczny rozkład chi-kwadrat z dwoma stopniami swobody przy hipotezie zerowej o normalności; wartości kurtozy z próby znacznie różniące się od trzech oraz wartości skośności znacznie różniące się od zera mogą prowadzić do odrzucenia normalności.

**Tabela 3.2.** Współczynniki skośności ($b$) i kurtozy ($k$) oraz $p$-value dla testów normalności Jarque'a-Bery dla arbitralnie wybranej grupy $n = 10$ spółek z indeksu Dow Jones (zobacz Tabela 3.1 w celu uzyskania nazw spółek).

**Zwroty dzienne $(n = 2020)$ i tygodniowe $(n = 416)$**

| Stock | Daily $b$ | Daily $k$ | Daily $p$-value | Weekly $b$ | Weekly $k$ | Weekly $p$-value |
|---|---|---|---|---|---|---|
| AXP | 0.05 | 5.09 | 0.00 | -0.01 | 3.91 | 0.00 |
| EK | -1.93 | 31.20 | 0.00 | -1.13 | 14.40 | 0.00 |
| BA | -0.34 | 10.89 | 0.00 | -0.26 | 7.54 | 0.00 |
| C | 0.21 | 5.93 | 0.00 | 0.44 | 5.42 | 0.00 |
| KO | -0.02 | 6.36 | 0.00 | -0.21 | 4.37 | 0.00 |
| MSFT | -0.22 | 8.04 | 0.00 | -0.14 | 5.25 | 0.00 |
| HWP | -0.23 | 6.69 | 0.00 | -0.26 | 4.66 | 0.00 |
| INTC | -0.56 | 8.29 | 0.00 | -0.65 | 5.20 | 0.00 |
| JPM | 0.14 | 5.25 | 0.00 | -0.20 | 4.93 | 0.00 |
| DIS | -0.01 | 9.39 | 0.00 | 0.08 | 4.48 | 0.00 |

**Zwroty miesięczne $(n = 96)$ i kwartalne $(n = 32)$**

| Stock | Monthly $b$ | Monthly $k$ | Monthly $p$-value | Quarterly $b$ | Quarterly $k$ | Quarterly $p$-value |
|---|---|---|---|---|---|---|
| AXP | -1.22 | 5.99 | 0.00 | -1.04 | 4.88 | 0.01 |
| EK | -1.52 | 10.37 | 0.00 | -0.63 | 4.49 | 0.08 |
| BA | -0.50 | 4.15 | 0.01 | -0.15 | 6.23 | 0.00 |
| C | -1.10 | 7.38 | 0.00 | -1.61 | 7.13 | 0.00 |
| KO | -0.49 | 3.68 | 0.06 | -1.45 | 5.21 | 0.00 |
| MSFT | -0.40 | 3.90 | 0.06 | -0.56 | 2.90 | 0.43 |
| HWP | -0.33 | 3.47 | 0.27 | -0.38 | 3.64 | 0.52 |
| INTC | -1.04 | 6.50 | 0.00 | -0.42 | 3.10 | 0.62 |
| JPM | -0.51 | 5.40 | 0.00 | -0.78 | 7.26 | 0.00 |
| DIS | 0.04 | 3.26 | 0.87 | -0.49 | 4.32 | 0.16 |

W Tabeli 3.2 testy normalności są zastosowane do arbitralnie wybranej podgrupy dziesięciu spółek wchodzących w skład indeksu Dow Jones. Bierzemy osiem lat danych obejmujących okres 1993–2000 i tworzymy dzienne, tygodniowe, miesięczne i kwartalne logarytmiczne stopy zwrotu. Dla każdej akcji obliczamy skośność i kurtozę z próby oraz stosujemy test Jarque'a-Bery do jednowymiarowego szeregu czasowego.

Dane dotyczące dziennych i tygodniowych stóp zwrotu nie przechodzą żadnego z testów; w szczególności godne uwagi jest występowanie wysokich wartości kurtozy w próbie. Dla danych miesięcznych, hipoteza zerowa o normalności nie jest formalnie odrzucona (to znaczy, **wartość p jest większa niż 0,05**) dla czterech spółek; dla danych kwartalnych, nie jest ona odrzucona dla pięciu spółek, chociaż w tym przypadku wielkość próby jest mała.

Test Jarque'a-Bery (3.2) wyraźnie odrzuca hipotezę o normalności. W szczególności, dane dotyczące dziennych finansowych stóp zwrotu wydają się mieć znacznie wyższą kurtozę niż ta, która jest zgodna z rozkładem normalnym; mówi się, że ich rozkład jest **leptokurtyczny**, co oznacza że jest on węższy od rozkładu normalnego w centrum, ale ma dłuższe i cięższe ogony.

Dalsza analiza empiryczna często sugeruje, że rozkład danych dotyczących dziennych lub innych krótkookresowych stóp zwrotu finansowych ma ogony, które zanikają powoli zgodnie z **prawem potęgowym**, a nie szybciej, w sposób wykładniczy, jak w przypadku ogonów rozkładu normalnego. Oznacza to, że w takich danych o stopach zwrotu mamy tendencję do obserwowania znacznie bardziej **ekstremalnych wartości**, niż można by się spodziewać; zjawisko to omówimy dalej w Rozdziale 5, który jest poświęcony **teorii wartości ekstremalnych**.













### 3.1.3 Szeregi zwrotów o dłuższych interwałach

W miarę stopniowego zwiększania interwału zwrotów, przechodząc od danych dziennych do tygodniowych, miesięcznych, kwartalnych i rocznych, zidentyfikowane przez nas zjawiska stają się mniej wyraźne. Klastrowanie zmienności maleje, a zwroty zaczynają wyglądać zarówno bardziej jak iid, jak i mniej grubogonowe.

Zaczynając od próby $n$ zwrotów mierzonych w pewnym interwale czasowym (powiedzmy dziennym lub tygodniowym), możemy je agregować, tworząc logarytmiczne stopy zwrotu o dłuższym interwale. $H$-okresowa logarytmiczna stopa zwrotu w czasie $t$ jest dana przez

$$X_t^{(h)} = \ln\left(\frac{S_t}{S_{t-h}}\right) = \ln\left(\frac{S_t}{S_{t-1}} \cdots \frac{S_{t-h+1}}{S_{t-h}}\right) = \sum_{j=0}^{h-1} X_{t-j},$$

i z naszej pierwotnej próby możemy utworzyć próbę nienakładających się h-okresowych zwrotów $\{X_t^{(h)} : t = h, 2h, ..., \lfloor n/h \rfloor h\}$, gdzie $\lfloor \cdot \rfloor$ oznacza część całkowitą lub funkcję podłogi; $\lfloor x \rfloor = \max\{k \in \mathbb{Z}: k \le x\}$ jest największą liczbą całkowitą nie większą niż $x$.

Ze względu na sumaryczną strukturę $h$-okresowych zwrotów, można oczekiwać, że nastąpi pewien efekt centralnego twierdzenia granicznego, w wyniku którego ich rozkład stanie się mniej leptokurtyczny i bardziej normalny w miarę wzrostu h. Należy zauważyć, że chociaż podaliśmy w wątpliwość model iid dla danych dziennych, centralne twierdzenie graniczne ma również zastosowanie do wielu stacjonarnych procesów szeregów czasowych, w tym modeli GARCH, które są przedmiotem Rozdziału 4.

W Tabeli 3.1 testy losowości Ljunga-Boxa zostały również zastosowane do nienakładających się miesięcznych danych zwrotów. Dla dwudziestu z trzydziestu akcji hipoteza zerowa o danych iid nie jest odrzucana na poziomie 5% w testach Ljunga-Boxa zastosowanych zarówno do surowych, jak i bezwzględnych zwrotów. Trudniej jest zatem znaleźć dowody na zależność seryjną w takich miesięcznych zwrotach.

Agregacja danych w celu utworzenia nienakładających się $h$-okresowych zwrotów zmniejsza wielkość próby z $n$ do $\lfloor n/h \rfloor$, a dla zwrotów o dłuższym okresie (takich jak kwartalne lub roczne zwroty) może to być bardzo poważne zmniejszenie ilości danych. Alternatywą w tym przypadku jest tworzenie zwrotów nakładających się. Dla $1 \le k < h$ możemy tworzyć zwroty nakładające się, biorąc

$$\{X_t^{(h)} : t = h, h + k, h + 2k, ..., h + \lfloor (n-h)/k \rfloor k\}, \quad (3.4)$$

co daje $1 + \lfloor (n-h)/k \rfloor$ wartości, które nakładają się na siebie o $h-k$. Tworząc zwroty nakładające się, możemy zachować dużą liczbę punktów danych, ale wprowadzamy dodatkową zależność seryjną do danych. Nawet jeśli pierwotne dane były iid, dane nakładające się byłyby głęboko zależne, co może znacznie skomplikować ich analizę.