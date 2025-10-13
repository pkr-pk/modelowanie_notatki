# Rozdział 5: Naddyspersja, Korekty Wiarygodności, Modele Mieszane i Regularyzacja

## 5.1 Wprowadzenie

Z Rozdziału 4 wiemy, że GLM (Uogólnione Modele Liniowe) unifikują metody regresji dla różnorodnych dyskretnych i ciągłych wyników typowo spotykanych w badaniach ubezpieczeniowych. Podstawą GLM jest założenie, że dane pochodzą z prób z rozkładu ED (Exponential Dispersion) oraz że scoring jest liniową kombinacją obserwowanych cech, powiązaną ze średnią odpowiedzią za pomocą monotonicznej transformacji.

Jednakże GLM przypisują pełną wiarygodność (credibility) obserwacjom w tym sensie, że scoring (lub równoważnie, średnia odpowiedź) jest zakładany jako znany dokładnie, z dokładnością do błędu estymacji. W praktyce, z Rozdziału 1 wiemy, że istnieje wiele informacji, do których ubezpieczyciel nie ma dostępu. Oznacza to, że część scoringu pozostaje ogólnie nieznana dla aktuariusza, a to typowo powoduje:

(i) naddyspersję (overdispersion) w danych przekrojowych (czyli danych obserwowanych w jednym okresie) oraz
(ii) korelację szeregową w danych podłużnych lub panelowych (czyli gdy jednostki są obserwowane przez kilka okresów, a odpowiedź jest obserwowana w wielu momentach).

Formalnie, wróćmy do dekompozycji informacji na dostępne cechy $\boldsymbol{X}_i$ i ukryte $\boldsymbol{X}_i^+$ zaproponowanej w Rozdz. 1. Biorąc pod uwagę $\boldsymbol{X}_i=\boldsymbol{x}_i$ oraz $\boldsymbol{X}_i^+=\boldsymbol{x}_i^+$, prawdziwy scoring powinien wynosić

$$
\text{prawdziwy scoring}_i = \boldsymbol{\beta}^T \boldsymbol{x}_i + \boldsymbol{\gamma}^T \boldsymbol{x}_i^+,
$$

przy czym niektóre składowe $\boldsymbol{\beta}$ mogą być równe zero (podobnie jak obserwowalne cechy mogą stać się nieistotne, gdy ukryta informacja staje się dostępna). Oznacza to, że GLM uwzględniają tylko scoring $\boldsymbol{\beta}^T \boldsymbol{x}_i$ oparty na dostępnych czynnikach ryzyka $\boldsymbol{X}_i=\boldsymbol{x}_i$, podczas gdy brakuje części $\boldsymbol{\gamma}^T \boldsymbol{x}_i^+$. Aby uwzględnić brakującą część, jawnie przyznajemy, że istnieje błąd, gdy używamy scoringu $\boldsymbol{\beta}^T \boldsymbol{x}_i$, a model regresji opiera się na scoringu roboczym zdefiniowanym jako

$$
\text{scoring roboczy}_i = \boldsymbol{\beta}^T \boldsymbol{x}_i + \mathcal{E}_i,
$$

gdzie składnik losowy $\mathcal{E}_i$ odpowiada za brakującą część $\gamma^T \boldsymbol{X}_i^+$. Jednakże, i jest to kluczowe dla właściwej interpretacji wyników, uważamy, że $\mathcal{E}_i$ jest tylko resztkowym efektem, tak że oszacowana wartość $\boldsymbol{\beta}$ przy użyciu scoringu roboczego może różnić się od swojej oszacowanej wartości w prawdziwym scoringu, z powodu błędu pominiętej zmiennej (jak wyjaśniono w Rozdz. 4). Zatem GLMM (Uogólnione Liniowe Modele Mieszane) dzielą scoring na dwie części:

* (i) pierwsza $\boldsymbol{x}_i^T \boldsymbol{\beta}$ odpowiada za obserwowalne cechy, jak w Rozdz. 4. Tutaj, $\boldsymbol{x}_i^T \boldsymbol{\beta}$ reprezentuje efekty stałe w scoringu.
* (ii) druga, zwana efektem losowym $\mathcal{E}_i$, reprezentuje brakujący komponent z powodu częściowej informacji.

Prowadzi to do modelu efektów mieszanych, obejmującego zarówno efekty stałe, jak i losowe. Zatem w GLMM występują dwa typy komponentów: efekty stałe i losowe. Efekt stały jest nieznaną stałą, którą chcemy estymować na podstawie dostępnych danych. Jest to przypadek dla wektora $\boldsymbol{\beta}$ współczynników regresji w GLM. W przeciwieństwie do tego, efekt losowy jest zmienną losową. Dlatego nie ma sensu estymować efektu losowego. Zamiast tego, dążymy do estymacji parametrów rządzących rozkładem efektu losowego oraz do rewizji jego rozkładu po zaobserwowaniu odpowiedzi (korekty a posteriori dające rozkład predykcyjny).

Efekty losowe mogą również wychwytywać zależności w czasie, przestrzeni lub pomiędzy różnymi grupami utworzonymi w portfelu (dzielącymi podobne charakterystyki). Na przykład, jeśli odpowiedź jest mierzona wielokrotnie dla tego samego ubezpieczającego, wówczas składnik losowy $\mathcal{E}_i$ dodany do liniowego scoringu odpowiada za zależność szeregową obserwacji związanych z tą samą umową, obserwowaną w czasie. Ta sama konstrukcja może być również użyta do modelowania zależności przestrzennej w ubezpieczeniach katastroficznych lub do radzenia sobie z wielopoziomowymi czynnikami z rzadko obsadzonymi kategoriami. Przykłady cech kategorycznych z wieloma poziomami obejmują klasyfikację modeli samochodów w ubezpieczeniach komunikacyjnych lub sektor działalności w ubezpieczeniach od odpowiedzialności cywilnej pracodawcy, z dużą ilością danych dla niektórych poziomów i ograniczoną ekspozycją dla innych. Bezpośrednie uwzględnienie takiej cechy w GLM jest generalnie niemożliwe, więc są one skutecznie traktowane za pomocą efektów losowych w tym rozdziale. Prowadzi to do GLMM, otwierając drogę do modelowania zależności i korekt wiarygodności (credibility) opartych na przeszłej historii szkodowej.

Warto zauważyć, że modele przedstawione w tym rozdziale oferują potężne narzędzia do implementacji szerokiej gamy modeli wiarygodności. Na przykład, scoring może być uzyskany z innych technik, takich jak metody oparte na drzewach (Trufin i in. 2019) lub sieciach neuronowych (Hainaut i in. 2019). Model mieszany następnie nakłada efekty losowe na te scoringi, które są traktowane jako znane stałe i włączane do offsetu. Rozkład predykcyjny efektów losowych następnie wychwytuje część doświadczenia szkodowego, której nie da się wyjaśnić dostępnymi cechami, i uwzględnia ten efekt przy obliczaniu przyszłych składek.

Jako zastosowanie modeli mieszanych przedstawionych w tym rozdziale, rozważamy systemy Pay-How-You-Drive (PHYD) lub Usage-Based (UB) dla ubezpieczeń komunikacyjnych, które dostarczają aktuariuszom danych behawioralnych czynników ryzyka, takich jak pora dnia, średnie prędkości i inne nawyki jazdy. Dane te są zbierane, gdy umowa jest w mocy, za pomocą urządzeń telematycznych zainstalowanych w pojeździe. Zatem należą one do kategorii informacji a posteriori, które stają się dostępne po zawarciu umowy. Z tego powodu muszą być one włączone do taryfy aktuarialnej za pomocą mechanizmów aktualizacyjnych, zamiast być włączane do scoringu jako zwykłe, obserwowalne a priori cechy. W tym rozdziale pokazujemy, że wielowymiarowe modele mieszane mogą być używane do opisu wspólnej dynamiki danych telematycznych i częstotliwości szkód. Przyszłe składki, uwzględniające przeszłe doświadczenie, mogą być następnie określone przy użyciu predykcyjnego rozkładu charakterystyk szkodowych, biorąc pod uwagę przeszłą historię. Takie podejście pozwala aktuariuszowi radzić sobie z różnorodnymi sytuacjami spotykanymi w praktyce ubezpieczeniowej, od nowych kierowców bez danych telematycznych, po kierowców z różnym stażem i tych, którzy używają swojego pojazdu w różnym stopniu, generując zróżnicowane ilości danych telematycznych.

## 5.2 Rozkłady Mieszane

### 5.2.1 Heterogeniczność i Naddyspersja

Ukryta informacja, jak również zależność szeregowa, mają tendencję do zawyżania wariancji liczby szkód ponad tę zakładaną przez wybrany rozkład ED. Rozważmy następujący prosty przykład wyjaśniający, dlaczego tak się dzieje. Rozważmy liczbę szkód $Y$ zgłoszoną przez ubezpieczającego w danym okresie. Model Poissona narzuca silne założenie ekwidyspersji:

$$
\text{E}[Y] = \text{Var}[Y] = \mu.
$$

Ekwidyspersja jest w praktyce często naruszana z powodu resztkowej heterogeniczności, co sugeruje, że założenie Poissona nie jest odpowiednie i faworyzuje mieszany model Poissona.

Aby zrozumieć, dlaczego naddyspersja jest związana z heterogenicznością, rozważmy dla przykładu dwie klasy ryzyka $\mathcal{C}_1$ i $\mathcal{C}_2$ bez naddyspersji, tzn. ich odpowiednie średnie $\mu_1$ i $\mu_2$ oraz wariancje $\sigma_1^2$ i $\sigma_2^2$ w tych dwóch klasach ryzyka spełniają

$$
\sigma_1^2 = \mu_1 \quad \text{i} \quad \sigma_2^2 = \mu_2.
$$

Teraz załóżmy, że aktuariusz nie jest w stanie rozróżnić między ubezpieczającymi należącymi do $\mathcal{C}_1$ i do $\mathcal{C}_2$. Niech $w_1$ i $w_2$ oznaczają odpowiednie wagi $\mathcal{C}_1$ i $\mathcal{C}_2$ (np. odsetek klas podzielony przez całkowitą ekspozycję). W klasie $\mathcal{C}_1 \cup \mathcal{C}_2$ oczekiwana liczba szkód jest równa

$$\bar{\mu} = w_1\mu_1 + w_2\mu_2.$$

Wariancja w $\mathcal{C}_1 \cup \mathcal{C}_2$ jest sumą ważonych wariancji wewnątrzklasowych i międzyklasowych. Dokładniej, wariancja w $\mathcal{C}_1 \cup \mathcal{C}_2$ jest równa

$$\underbrace{w_1\sigma_1^2 + w_2\sigma_2^2}_{=\bar{\mu}} + \underbrace{w_1(\mu_1 - \bar{\mu})^2}_{>0} + \underbrace{w_2(\mu_2 - \bar{\mu})^2}_{>0} > \bar{\mu}.$$

Stąd pominięcie istotnych zmiennych przy ustalaniu stawek nieuchronnie prowadzi do nadmiernej dyspersji.
Mimo swojej popularności jako punkt wyjścia w analizie danych zliczeniowych, specyfikacja Poissona jest często nieodpowiednia z powodu nieobserwowanej heterogeniczności. Wygodnym sposobem uwzględnienia tego zjawiska jest wprowadzenie do modelu efektu losowego. Jest to zgodne z klasyczną konstrukcją wiarygodności w naukach aktuarialnych. Rysunek 5.1 przedstawia ważone średnie i wariancje dla zbioru danych dotyczących ubezpieczeń komunikacyjnych badanego w rozdziale 4.10. Wyraźnie widzimy, że pary średnia-wariancja dla klas o wysokiej ekspozycji leżą powyżej przekątnej, co sugeruje, że średnia rzeczywiście przekracza wariancję w rozważanym portfelu.
Prawidłowe uwzględnienie nadmiernej dyspersji daje większe błędy standardowe i wartości p w porównaniu ze zwykłą regresją Poissona. Oznacza to, że regresja Poissona jest na ogół zbyt optymistyczna, przypisując zbyt duże znaczenie dostępnym cechom.

W praktyce oznacza to, że cechy uznane za nieistotne przez regresję Poissona mogą być bezpiecznie zignorowane w dalszej analizie. Jednakże niektóre cechy wybrane przez regresję Poissona mogą zostać później odrzucone, gdy zastosuje się bardziej odpowiednie narzędzia inferencyjne.

### 5.2.2 Z jednorodnych ED do mieszanin ED

W tej sekcji wzbogacamy rodzinę rozkładów, aby uwzględnić heterogeniczność danych, wykraczając poza te omówione w rozdziale 2. Rozważmy grupę ubezpieczających, z których każdy ma rozkład podporządkowany pewnemu danemu rozkładowi ED z tym samym parametrem dyspersji $\phi$, ale z różnymi kanonicznymi parametrami $\theta$. Podstawową ideą jest rozważenie ubezpieczającego wybranego losowo z tej grupy. Ta kanoniczna idea jest reprezentowana przez zmienną losową $\Theta$, z rozkładem odzwierciedlającym skład lub strukturę badanej grupy.

Oznacza to, że odpowiedź $Y$ dla tego losowo wybranego ubezpieczającego jest ED tylko wtedy, gdy znamy wartość $\Theta$. Formalnie, dana $\Theta = \theta$, odpowiedź $Y$ podlega rozkładowi ED z kanonicznym parametrem $\theta$ i parametrem dyspersji $\phi$, z masą prawdopodobieństwa lub funkcją gęstości prawdopodobieństwa daną przez (2.3). Oznaczmy jako $f_{\Theta}$ funkcję gęstości prawdopodobieństwa $\Theta$. Bezwarunkowo, to jest, bez znajomości $\Theta$, funkcja gęstości prawdopodobieństwa (lub masa prawdopodobieństwa) $Y$ zapisuje się jako

$$f_Y(y) = \int \exp\left( \frac{y\theta - a(\theta)}{\phi/v} \right) c(y, \phi/v) f_{\Theta}(\theta) d\theta,$$

gdzie całka przebiega po nośniku $\Theta$, przy założeniu, że jest to podzbiór lub zbiór zbieżny ze zbiorem dopuszczalnych wartości dla parametru kanonicznego rozkładu ED branego pod uwagę. Taka odpowiedź $Y$ podlega modelowi mieszanemu, ponieważ jest otrzymywana przez mieszanie kilku rozkładów ED, każdy z własnym parametrem kanonicznym $\theta$, i do wylosowania. Rozkład $Y$ jest wtedy nazywany rozkładem mieszanym, z funkcją mieszającą $f_{\Theta}$, a parametr mieszający, lub efekt $\Theta$.

Tutaj założyliśmy, że $\Theta$ jest ciągłą zmienną losową. Jeśli $\Theta$ jest dyskretną zmienną losową, to ta konstrukcja odpowiada dyskretnemu modelowi mieszanemu, dla którego zachodzi to samo rozumowanie, ale całki zastępuje się sumami. Ponieważ grupy rozważane w zastosowaniach ubezpieczeniowych są na ogół bardzo duże, dyskretne modele mieszane są na ogół aproksymowane przez ciągłe.
Wszystkie obliczenia można przeprowadzić przez warunkowanie na $\Theta$, a następnie stosując wyniki dostępne dla rozkładów ED. Na przykład, średnia odpowiedź jest dana przez

$$E[Y] = E[E[Y|\Theta]] = E[a'(\Theta)].$$

Podobnie, wariancja odpowiedzi jest otrzymywana z

$$\text{Var}[Y] = E[\text{Var}[Y|\Theta]] + \text{Var}[E[Y|\Theta]] = \frac{\phi}{v}E[a''(\Theta)] + \text{Var}[a'(\Theta)].$$

W następnej sekcji badamy mieszaniny Poissona, zdecydowanie najczęściej używane w badaniach aktuarialnych. Inne rozkłady ED można analizować podobnie.

### 5.2.3 Mieszany rozkład Poissona

#### 5.2.3.1 Definicja

Mówiąc słowami, mieszany rozkład Poissona pojawia się, gdy istnieją różne grupy ubezpieczających, z których każda charakteryzuje się określoną średnią Poissona. Jeśli wszyscy członkowie grupy mają tę samą częstotliwość szkód, wówczas rozkład Poissona jest odpowiedni do modelowania liczby szkód. Jednakże, jeśli osoby mają różne poziomy skłonności do wypadków, to rozkład wypadków jest heterogeniczny w odniesieniu do osób, a mieszany rozkład Poissona zastępuje jednorodny rozkład Poissona przy rozważaniu liczby szkód zgłoszonych przez osobę losowo pobraną z grupy.
Niech ubezpieczający zostanie losowo wylosowany z heterogenicznej grupy. Biorąc pod uwagę $\Theta = \theta$, załóżmy, że liczba szkód $Y$ zgłoszonych ubezpieczycielowi ma rozkład Poissona z kanonicznym parametrem $\theta$. Interpretacja, jaką dajemy temu modelowi, jest taka, że nie wszyscy ubezpieczający mają identyczną oczekiwaną częstotliwość szkód $\mu = \exp(\theta)$. Niektórzy z nich mają wyższą oczekiwaną częstotliwość, inni niższą. Zatem używamy modelu efektu losowego, aby uwzględnić tę empiryczną obserwację, zastępując $\theta$ zmienną losową $\Theta$ reprezentującą wynik losowego losowania.
Roczna liczba szkód zgłoszonych przez ubezpieczającego wybranego losowo z portfela podlega mieszanemu prawu Poissona. W tym przypadku prawdopodobieństwo, że losowo wybrany ubezpieczający zgłosi $y$ szkód do towarzystwa, uzyskuje się uśredniając warunkowe prawdopodobieństwa Poissona względem $\Theta$. Funkcja masy prawdopodobieństwa związana z mieszanym rozkładem Poissona jest zdefiniowana jako

$$\begin{align*}
P[Y = y] &= E\left[\frac{\exp(- \exp(\Theta))(\exp(\Theta))^y}{y!}\right] \\
&= \int_{-\infty}^{\infty} \frac{\exp(- \exp(\theta))(\exp(\theta))^y}{y!} f_{\Theta}(\theta) d\theta, \quad y = 0, 1, \ldots \quad (5.1)
\end{align*}$$

gdzie $f_{\Theta}$ oznacza funkcję gęstości prawdopodobieństwa $\Theta$. Rozkład mieszający opisany przez $f_{\Theta}$ reprezentuje heterogeniczność interesującego portfela: $f_{\Theta}$ jest ogólnie nazywany funkcją struktury w literaturze aktuarialnej, ponieważ opisuje strukturę portfela w odniesieniu do rozważanego ryzyka (tutaj, liczby szkód).

Średnia odpowiedź jest wtedy dana przez

$$E[Y] = E[E[Y|\Theta]] = E[\exp(\Theta)]. \quad (5.2) $$

W (5.2), $E[\cdot|\Theta]$ oznacza, że bierzemy wartość oczekiwaną, traktując $\Theta$ jako stałą. Mamy wtedy na myśli wszystkie składniki losowe, z wyjątkiem $\Theta$. W konsekwencji, $E[\cdot|\Theta]$ jest funkcją $\Theta$. Biorąc pod uwagę $\Theta$, $Y$ ma rozkład Poissona ze średnią $\exp(\Theta)$, tak że $E[Y|\Theta] = \exp(\Theta)$. Średnia $Y$ jest ostatecznie uzyskiwana przez uśrednienie $E[Y|\Theta]$ względem $\Theta$.

Każdy mieszany model Poissona (5.1) indukuje nadmierną dyspersję, ponieważ

$$\begin{align*}
\text{Var}[Y] &= E[\text{Var}[Y|\Theta]] + \text{Var}[E[Y|\Theta]] \\
&= E[\exp(\Theta)] + \text{Var}[\exp(\Theta)] \\
&= E[Y] + \text{Var}[\exp(\Theta)]\\
&> E[Y].
\end{align*}$$

To teoretyczne obliczenie rozszerza się na ogólny przypadek, szczególny przykład z dwiema klasami ryzyka $\mathcal{C}_1$ i $\mathcal{C}_2$, które są połączone z powodu braku informacji, jest przykładem wprowadzającym do tej sekcji. Tutaj, $\Theta$ zastępuje etykiety 1 i 2 klas ryzyka, które są mieszane razem.

