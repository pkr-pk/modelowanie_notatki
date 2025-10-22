# Rozdział 7: Poza modelowaniem średniej: Podwójne modele GLM i GAM dla położenia, skali i kształtu (GAMLSS)

## 7.1 Wprowadzenie

Wiemy z Rozdziału 4, że modele GLM narzucają stały parametr dyspersji $\phi$. Oznacza to, że wariancja odpowiedzi może zmieniać się wraz z czynnikami ryzyka tylko jako funkcja średniej. Stałe $\phi$ wydaje się być restrykcyjne w przypadku niektórych rozkładów, takich jak te z podklasy Tweedie rodziny ED. Przechodząc do podwójnych modeli GLM, cechy mogą również wchodzić w modelowanie parametru dyspersji, który zależy od określonego wyniku. Oprócz modelowania średniej, tak jak w podejściu GLM, w podwójnych modelach GLM występuje również modelowanie dyspersji, z własną funkcją łączącą (zazwyczaj logarytmiczną, aby zapewnić dodatniość wynikowego parametru dyspersji) i współczynnikami regresji.

Model Gamma GLM jest używany do modelowania dyspersji, uruchamiany na kwadratach reszt dewiacyjnych lub Pearsona. Dopasowanie podwójnego modelu GLM uzyskuje się następnie poprzez iteracyjne dopasowywanie modelu GLM dla średniej, po którym następuje model Gamma GLM dla dyspersji, przy czym każda obserwacja wnosi wkład do dewiacji. Związek między dwoma modelami GLM jest następujący: model dla średniej generuje reszty, które służą jako odpowiedzi dla modelu dyspersji, który z kolei produkuje parametr dyspersji dla modelu średniej.
Ponieważ $\phi$ wchodzi do rozkładu ED poprzez stosunek $\phi/v_i$, gdzie wagi $v_i$ są znanymi stałymi, oznacza to, że wagi a priori $v_i$ nie mogą być modyfikowane przez model. Jeśli jednak parametr dyspersji może się zmieniać w zależności od cech, wagi mogą być dostosowane tak, aby odzwierciedlały aktuarialne doświadczenie każdej polisy w portfelu. Podwójny model GLM może zatem poprawić estymację średniej w przypadku, gdy pewne klasy biznesu wydają się być bardziej zmienne w porównaniu z innymi. W tym ustawieniu model ma na celu nadanie mniejszej wagi bardziej zmiennym obserwacjom z przeszłości i większej wagi stabilniejszemu biznesowi, którego dane są bardziej informatywne. Dlatego model staje się zdolny do ignorowania większego szumu, gdy jest to konieczne, a tym samym do wychwytywania większej ilości sygnału. Podwójny model GLM w pewnym sensie wybiera liniowy wynik w modelu dyspersji, aby zdefiniować optymalne wagi, maksymalizując jakość dopasowania. Czasami cecha, która wydawała się nieistotna w ustawieniu GLM, staje się istotna, gdy aktuariusz przechodzi do ram podwójnego GLM.

W podwójnym modelu GLM cechy wchodzą zarówno w parametry ED $\theta$ i $\phi$, to znaczy, że cechy wchodzą w średnią i parametry dyspersji. Uogólnione modele addytywne dla położenia, skali i kształtu (GAMLSS) rozszerzają tę ideę na bardziej złożone rozkłady, w których nie tylko średnia odpowiedź, ale wiele parametrów jest powiązanych z addytywnymi wynikami za pomocą odpowiednich funkcji łączących. Na przykład rozkłady z zerami, skośne i z zerami mogą być osadzone w tej strukturze jako szczególne przypadki, w których wszystkie występujące parametry są powiązane z określonymi wynikami.

W podejściu GAMLSS założenie o rozkładzie ED dla odpowiedzi jest złagodzone, dzięki czemu analiza aktuarialna nie jest już ograniczona do rozkładów używanych w klasycznym ustawieniu GLM/GAM. Jedynym ograniczeniem jest to, że indywidualny wkład do log-wiarygodności i jego pierwsze dwie pochodne względem każdego z parametrów muszą być obliczalne. Otwiera to drzwi do licznych rozkładów responsywnych, rozpoznających specyficzną naturę danych ubezpieczeniowych.

Po przedstawieniu aspektów metodologicznych podwójnych modeli GLM i GAMLSS, rozdział ten proponuje zastosowanie do podejścia rezerwacyjnego, inspirowanego zbiorowym modelem teorii ryzyka. Zgodnie z paradygmatem zbiorowym, płatności nie są powiązane z konkretnymi roszczeniami lub polisami, ale przyjmuje się ustawienie częstotliwość-szkodowość, z liczbą płatności w każdej komórce trójkąta run-off, wraz z odpowiadającymi im kwotami wypłat. W porównaniu z modelem rezerwacyjnym Tweedie, który można postrzegać jako sumę złożoną z liczbą terminów o rozkładzie Poissona i składnikami o rozkładzie Gamma, można rozważyć bardziej ogólne rozkłady szkodowości, zazwyczaj modele mieszane łączące lekkoogonowy składnik z cięższym ogonem, w tym inflację. Model szkodowości jest dopasowywany do indywidualnych obserwacji, a nie do zagregowanych danych wyświetlanych w trójkątach run-off z pojedynczą wartością w każdej komórce. W tym względzie podejście do modelowania wydaje się być potężną alternatywą zarówno dla tradycyjnego zagregowanego podejścia opartego na trójkątach, jak i dla niezwykle szczegółowego indywidualnego podejścia rezerwacyjnego, rozwijającego każdy pojedynczy łańcuch roszczeń oddzielnie. Studium przypadku oparte na ubezpieczeniu odpowiedzialności cywilnej posiadaczy pojazdów mechanicznych ilustruje znaczenie tego podejścia.

## 7.2 Rozkłady złożone Poissona Tweedie

### 7.2.1 Rozkłady złożone

Czasami aktuariusze dysponują jedynie sumami roszczeń, które są zerowe z dodatnim prawdopodobieństwem, ale w pozostałych przypadkach mają rozkład ciągły. Rozkład złożony jest dobrym kandydatem do modelowania takich odpowiedzi. W szczególności niech $N$ będzie zliczającą zmienną losową reprezentującą liczbę roszczeń, a $C_1, C_2, ..., C_N$ będą odpowiednimi kwotami tych $N$ roszczeń. Całkowita kwota roszczeń zapisuje się zatem

$$
Y = \sum_{k=1}^{N} C_k,
$$

z konwencją, że pusta suma równa się 0, tj.

$$
Y = 
\begin{cases}
0 & \text{if } N=0, \\
C_1 + \dots + C_N & \text{if } N \ge 1.
\end{cases}
$$

Dlatego $\mathrm{P}[Y=0] = \mathrm{P}[N=0] > 0$.
Jeśli wysokości szkód $C_1, C_2, ..., C_N$ są założone jako niezależne i o identycznym rozkładzie, oraz niezależne od $N$, to mówi się, że $Y$ ma rozkład złożony. Średnia i wariancja $Y$ są wówczas odpowiednio dane przez

$$
\mathrm{E}[Y] = \mathrm{E}[\mathrm{E}[Y|N]] = \mathrm{E}[N\mathrm{E}[C_1]] = \mathrm{E}[N]\mathrm{E}[C_1]
$$

i

$$
\begin{aligned}
\mathrm{Var}[Y] &= \mathrm{E}[\mathrm{Var}[Y|N]] + \mathrm{Var}[\mathrm{E}[Y|N]] \\
&= \mathrm{E}[N\mathrm{Var}[C_1]] + \mathrm{Var}[N\mathrm{E}[C_1]] \\
&= \mathrm{E}[N]\mathrm{Var}[C_1] + \mathrm{Var}[N](\mathrm{E}[C_1])^2.
\end{aligned}
$$

### 7.2.2 Rozkłady złożone Poissona

Jeśli $N \sim \mathcal{Poi}(\lambda)$, to $Y$ podlega rozkładowi złożonemu Poissona. W tym przypadku,

$$
\begin{aligned}
\mathrm{E}[Y] &= \lambda \mathrm{E}[C_1] \\
\mathrm{Var}[Y] &= \lambda \mathrm{Var}[C_1] + \lambda(\mathrm{E}[C_1])^2 \\
&= \lambda \mathrm{E}[C_1^2].
\end{aligned}
$$

Rozkłady złożone Poissona odgrywają ważną rolę w naukach aktuarialnych, ponieważ stanowią konserwatywne przybliżenie do indywidualnego modelu w teorii ryzyka.
Funkcję generującą momenty dla rozkładu złożonego Poissona uzyskuje się z

$$
\begin{aligned}
m_Y(t) &= \sum_{k=0}^{\infty} \mathrm{P}[N=k] \mathrm{E} \left[ \exp \left( t \sum_{j=1}^{k} C_j \right) \right] \\
&= \sum_{k=0}^{\infty} \mathrm{P}[N=k] (m_C(t))^k = \mathrm{E}[\exp(tC_1)] \\
&= \mathrm{E}[(m_C(t))^N] = \varphi_N(m_C(t)) \quad \text{gdzie} \quad \varphi_N(t) = \mathrm{E}[t^N].
\end{aligned}
$$

Oczywiście $\varphi_N(t) = \mathrm{E}[t^N] = m_N(\ln t)$. Biorąc pod uwagę funkcję generującą momenty Poissona $m_N$ uzyskaną z Własności 2.4.1, ostatecznie otrzymujemy

$$
m_Y(t) = \exp(\lambda(m_C(t) - 1)).
\tag{7.1}
$$

### 7.2.3 Tweedie i złożony Poisson z składnikami Gamma

Poprzez sprytny dobór rozkładów częstotliwości i wysokości szkód, możemy zobaczyć, że funkcja wariancji potęgowej definiująca rozkład Tweedie odpowiada rozkładowi złożonemu Poissona należącemu do rodziny ED, pod warunkiem $1 < \xi < 2$. W szczególności załóżmy, że

$$
Y = \sum_{k=1}^{N} C_k \quad \text{z} \quad N \sim \mathcal{Poi}(\lambda) \quad \text{i} \quad C_k \sim \mathcal{Gam}(\alpha, \tau),
$$

wszystkie zmienne losowe są niezależne. Aby połączyć ten model złożony Poissona z rodziną ED, zidentyfikujmy odpowiadającą mu funkcję generującą momenty w postaci kanonicznej wyprowadzonej we Własności 2.4.1. W tym celu wstawiamy funkcję generującą momenty Gamma $m_C$ do (7.1), aby otrzymać

$$
m_Y(t) = \exp \left( \lambda \left( \left( 1 - \frac{t}{\tau} \right)^{-\alpha} - 1 \right) \right).
$$

Teraz piszemy

$$
\begin{aligned}
\ln m_Y(t) &= \lambda \left( \left( 1 - \frac{t}{\tau} \right)^{-\alpha} - 1 \right) \\
&= \frac{1}{\phi} (a(\theta + t\phi) - a(\theta))
\end{aligned}
$$

gdzie druga równość jest ważna dla każdego rozkładu z rodziny ED, zgodnie z Własnością 2.4.1. Z funkcją wariancji potęgowej Tweedie $V(\mu)=\mu^\xi$, wiemy z Sekcji 2.5, że

$$
a(\theta) = \frac{(1-\xi)\theta}{2-\xi}^{\frac{2-\xi}{1-\xi}}
$$

tak, że

$$
\begin{aligned}
\ln m_Y(t) &= \frac{1}{\phi} \frac{\left((1-\xi)(\theta + t\phi)\right)^{\frac{2-\xi}{1-\xi}} - \left((1-\xi)\theta\right)^{\frac{2-\xi}{1-\xi}}}{2-\xi} \\
&= \frac{1}{\phi} \left((1-\xi)\theta\right)^{\frac{2-\xi}{1-\xi}} \frac{\left(1+\frac{(1-\xi)t\phi}{(1-\xi)\theta}\right)^{\frac{2-\xi}{1-\xi}} - 1}{2-\xi}.
\end{aligned}
$$

Średnia $\mu$ odpowiadająca podklasie Tweedie rozkładów ED wynosi

$$
a'(\theta) = ((1-\xi)\theta)^{\frac{2-\xi}{1-\xi}-1} = ((1-\xi)\theta)^{\frac{1}{1-\xi}}
$$

tak, że

$$
\ln m_Y(t) = \frac{1}{\phi} \frac{\mu^{2-\xi}}{2-\xi} \left( \left( 1+(t\phi)\mu^{\xi-1} \right)^{\frac{2-\xi}{1-\xi}} - 1 \right).
$$

To ostatecznie prowadzi do

$$
\begin{aligned}
\lambda &= \frac{\mu^{2-\xi}}{\phi(2-\xi)} \\
\alpha &= \frac{2-\xi}{\xi-1} \\
\frac{1}{\tau} &= \phi(\xi-1)\mu^{\xi-1}.
\end{aligned}
$$

Aby zapewnić $\alpha > 0$, musi być spełnione ograniczenie $1 < \xi < 2$, więc wykładnik $\xi$ jest ograniczony do przedziału $(1, 2)$.
Zidentyfikujmy teraz składniki z (2.3) dla takiej odpowiedzi $Y$. Oczywiście,

$$
\mathrm{P}[Y=0] = \mathrm{P}[N=0] = \exp(-\lambda).
$$

Jest to dobrze w postaci (2.3) z

$$
\lambda = \frac{a(\theta)}{\phi}.
$$

Teraz, przy danym $N=k>0$,

$$
Y = C_1 + \dots + C_k \sim \mathcal{Gam}(\alpha k, \tau)
$$

więc funkcja gęstości prawdopodobieństwa $Y$ na $(0, \infty)$ jest dana przez

$$
\begin{aligned}
f_Y(y) &= \sum_{k=1}^{\infty} \mathrm{P}[N=k] f_{C_1+\dots+C_k}(y) \\
&= \exp(-\tau y - \lambda) \sum_{k=1}^{\infty} \frac{\tau^{k\alpha}}{\Gamma(k\alpha)} y^{k\alpha-1} \frac{\lambda^k}{k!}, \quad y > 0.
\end{aligned}
$$

Ponieważ

$$
\lambda \tau^\alpha = \frac{\mu^{2-\xi}}{\phi(2-\xi)} \left( \frac{1}{\phi(\xi-1)\mu^{\xi-1}} \right)^{\frac{2-\xi}{\xi-1}} = \frac{(\phi(\xi-1))^{\frac{2-\xi}{1-\xi}}}{\phi(2-\xi)}
$$

nie zależy od $\mu$, tylko od parametru $\phi$ i stałej $\xi$, szereg zawarty w wyrażeniu na $f_Y$ zależy od $\phi$ i $y$, ale nie od $\mu$. Zdefiniuj $c(y, \phi), y>0$, jako szereg pojawiający się w $f_Y$, tj.

$$
c(y, \phi) = \sum_{k=1}^{\infty} \frac{\tau^{k\alpha}}{\Gamma(k\alpha)} y^{k\alpha-1} \frac{\lambda^k}{k!}
$$

i $c(0, \phi) = 1$. Identyfikując

$$
\exp \left( \frac{y\theta - a(\theta)}{\phi} \right) \quad \text{z} \quad \exp(-\tau y - \lambda),
$$

otrzymujemy

$$
-\tau = \frac{\theta}{\phi} \quad \text{i} \quad \lambda = \frac{a(\theta)}{\phi}
$$

co daje

$$
\theta = -\tau \phi = \frac{1}{(1-\xi)\mu^{\xi-1}} \quad \text{i} \quad a(\theta) = \lambda \phi = \frac{\mu^{2-\xi}}{2-\xi}.
$$

To pokazuje, że rozkład Tweedie należy do rodziny ED dla ustalonej wartości $\xi$. Odzyskujemy również funkcję wariancji potęgowej $V(\mu) = \mu^\xi$ z $1 < \xi < 2$ jako

$$
\begin{aligned}
\mathrm{Var}[Y] &= \mathrm{E}[N]\mathrm{E}[C_1^2] \\
&= \lambda \left( \frac{\alpha}{\tau^2} + \left( \frac{\alpha}{\tau} \right)^2 \right) \\
&= \frac{\lambda}{\tau^2} \alpha(\alpha+1) \\
&= \phi\mu^\xi.
\end{aligned}
$$

### 7.2.4 Parametr wykładnika

W zastosowaniach aktuarialnych wykładnik $\xi$ pojawiający się w funkcji wariancji Tweedie jest generalnie ograniczony do przedziału $(1, 2)$, tak że odpowiadające rozkłady Tweedie odpowiadają złożonym sumom Poissona z szkodami o rozkładzie Gamma. Rozkłady Poissona ($\xi=1$) i Gamma ($\xi=2$) można wtedy uzyskać jako granice tej ograniczonej klasy Tweedie.

Ogólnie $\xi$ jest nieznane i musi być oszacowane na podstawie danych. Z definicji

$$
\xi = \frac{\alpha+2}{\alpha+1}
$$

jest funkcją współczynnika Gamma zmienności. Wartości $\xi$ znalezione w modelowaniu ubezpieczeniowym i badaniach aktuarialnych zazwyczaj mieszczą się w przedziale 1.5 i 1.8. Kiedy używany jest rozkład Tweedie, wybór $\xi = 1.65$ wydaje się być dogodnym punktem wyjścia.

### 7.2.5 Ograniczenie złożonego modelu Poissona Tweedie GLM

Istnieje ukryte ograniczenie przy używaniu złożonego modelu Poissona Tweedie GLM, które jest często pomijane przez analityków. Przypomnijmy z Sekcji 2.5, że w tym przypadku mamy

$$
\mu = \lambda \frac{\alpha}{\tau}, \quad \xi = \frac{\alpha+2}{\alpha+1} \quad \text{i} \quad \phi = \frac{\mu^{2-\xi}}{\lambda(2-\xi)} = \frac{\lambda^{1-\xi}(\frac{\alpha}{\tau})^{2-\xi}}{2-\xi}.
$$

W modelach GLM parametr dyspersji $\phi$ jest stały, tak że

$$
\phi = \text{stała} \implies \frac{(\frac{\alpha}{\tau})^{2-\xi}}{\lambda^{\xi-1}} = \text{stała}.
\tag{7.2}
$$

W związku z tym każdy czynnik ryzyka zwiększający oczekiwaną wysokość szkody $\alpha$ musi również zwiększyć oczekiwaną częstotliwość szkód $\lambda$, aby spełnić (7.2). Podobnie, każdy czynnik ryzyka zmniejszający oczekiwaną wysokość szkody musi również zmniejszyć oczekiwaną częstotliwość szkód, aby (7.2) pozostało w mocy. W szczególności każdy czynnik ryzyka oddziałujący na jedną z tych dwóch zmiennych musi również oddziaływać na drugą w tym samym kierunku.

Jednak często tak nie jest w zastosowaniach ubezpieczeniowych. W rezerwacji szkodowej, czynniki geograficzne zazwyczaj mają przeciwne skutki dla częstotliwości i wysokości szkód, przy wyższych, ale mniejszych szkodach w dużych miastach i odwrotnie na obszarach wiejskich. Tutaj modelowanie strat za pomocą złożonego modelu Poissona Tweedie nie rozpoznaje tych dwóch sprzecznych efektów.
Użycie modelu Tweedie GLM często wymaga zatem przejścia do ustawienia podwójnego GLM, w którym parametr dyspersji $\phi$ zależy również od informacji zawartych w $x_i$. W tym przypadku iterujemy między GLM dla średniej a Gamma GLM dla dyspersji, uruchamianym na resztach dewiacyjnych (zobacz następną sekcję 7.3, aby uzyskać więcej szczegółów).

## 7.3 Modelowanie dyspersji: Podwójne modele GLM

Modele GLM narzucają wymóg, aby parametr dyspersji $\phi$ był taki sam dla każdej obserwacji. W związku z tym, wariancja $\mathrm{Var}[Y_i] = \frac{\phi}{v_i}V(\mu_i)$ może zmieniać się wraz z czynnikami ryzyka tylko jako funkcja średniej $\mu_i$. Podwójne modele GLM pozwalają uniknąć tego ograniczenia. Dokładniej, w podwójnym modelu GLM parametr dyspersji

$$
\phi_i = \phi(\mathbf{x}_i)
$$

również zależy od dostępnych cech poprzez określony wynik (score), co czyni go teraz specyficznym для każdej obserwacji. Zatem, nie tylko średnia odpowiedź zależy od dostępnych cech, ale również współczynnik dyspersji. Dokładniej, oprócz modelowania średniej $g(\mu_i) = \mathbf{x}_i^T \boldsymbol{\beta}$, istnieje również modelowanie dyspersji

$$
g_d(\phi_i) = \mathbf{x}_i^T \boldsymbol{\gamma}
$$

z własną funkcją łączącą $g_d$ i współczynnikami regresji $\boldsymbol{\gamma}$. Często dla $g_d$ używana jest logarytmiczna funkcja łącząca, aby zapewnić, że $\phi_i$ pozostaje dodatnie. Jednak inne funkcje łączące могут być przydatne w niektórych zastosowaniach (zobacz na przykład zastosowanie do graduacji śmiertelności w obecności duplikatów omówione w następnej sekcji).

Współczynniki regresji $\boldsymbol{\gamma}$ są następnie estymowane za pomocą modelu Gamma GLM dopasowanego do kwadratów reszt dewiacyjnych $r_i^D$ lub reszt Pearsona $r_i^P$ zdefiniowanych w Sekcji 4.9. Zaczynając od $\phi_i = 1$ dla wszystkich $i$ w kroku 0, współczynniki regresji $\boldsymbol{\beta}$ i $\boldsymbol{\gamma}$ są następnie estymowane naprzemiennie, aż do osiągnięcia zbieżności. Dokładniej, dopasowanie podwójnego modelu GLM uzyskuje się poprzez iterację następujących kroków:

1.  dopasuj model GLM dla średniej odpowiedzi, ze stałym $\phi$ dla wszystkich obserwacji, to jest,
    $$
    \begin{aligned}
    g(\mathrm{E}[Y_i]) &= \mathbf{x}_i^T \boldsymbol{\beta} \\
    \mathrm{Var}[Y_i] &= \frac{\phi}{v_i} V(\mathrm{E}[Y_i]).
    \end{aligned}
    $$

2.  oblicz wkład każdej obserwacji do dewiacji i oblicz kwadraty reszt Pearsona lub dewiacyjnych $R_i^2$.

3.  dopasuj model GLM dla dyspersji, przyjmując jako odpowiedź wkład $R_i^2$ każdej obserwacji do dewiacji. Rozkład jest Gamma i w tym kroku nie uwzględnia się żadnych wag. Dopasowane wartości są nowym parametrem dyspersji dla każdego rekordu. Dokładniej, modelowanie dyspersji wykorzystuje model Gamma GLM z
    $$
    \begin{aligned}
    g_d(\mathrm{E}[R_i^2]) &= \mathbf{x}_i^T \boldsymbol{\gamma} \\
    \mathrm{Var}[R_i^2] &= \tau (\mathrm{E}[R_i^2])^2 \\
    \phi_i &= g_d^{-1}(\mathbf{x}_i^T \boldsymbol{\gamma}).
    \end{aligned}
    $$

4.  dopasuj model GLM dla średniej, ale tym razem używając specyficznego parametru dyspersji dla każdego rekordu (dzieląc wagę przez specyficzny dla odpowiedzi parametr dyspersji uzyskany w poprzednim kroku), to jest,
    $$
    \begin{aligned}
    g(\mathrm{E}[Y_i]) &= \mathbf{x}_i^T \boldsymbol{\beta} \\
    \mathrm{Var}[Y_i] &= \frac{\phi_i}{v_i} V(\mathrm{E}[Y_i]).
    \end{aligned}
    $$

5.  oblicz kwadraty reszt Pearsona lub dewiacyjnych $R_i^2$ i powtórz poprzednie kroki.

Ten proces iteracyjny jest kontynuowany, aż do spełnienia pewnego warunku zatrzymania. Związek między dwoma modelami GLM jest następujący: model dla średniej generuje odpowiedź $R_i^2$ używaną do dopasowania modelu dyspersji, który z kolei generuje dyspersję dla modelu średniej.

Podwójny model GLM może poprawić estymację średniej w przypadku, gdy pewne klasy biznesu wydają się być bardziej zmienne w porównaniu z innymi. W tym ustawieniu model ma na celu nadanie mniejszej wagi bardziej zmiennym obserwacjom z przeszłości i większej wagi stabilniejszemu biznesowi, którego dane są bardziej informatywne. Dlatego model staje się zdolny do ignorowania większego szumu, gdy jest to konieczne, a tym samym do wychwytywania większej ilości sygnału. Podwójny model GLM w pewnym sensie określa wynik liniowy w celu zdefiniowania optymalnych wag, maksymalizując jakość dopasowania. Czasami cecha, która wydawała się nieistotna w ustawieniu GLM, staje się istotna, gdy aktuariusz przechodzi do ram podwójnego GLM.

Podwójny model GLM jest szczególnie istotny w modelowaniu Tweedie, ze względu na ukryte ograniczenie nieodłącznie związane z tymi modelami, omówione w Sekcji 7.2.5. Rozważmy ponownie zastosowanie do rezerwacji szkodowej, z danymi przedstawionymi w formie trójkątnej, indeksowanymi rokiem wypadku AY i rokiem rozwoju DY. Załóżmy, że kwota pojawiająca się w komórce odpowiadającej AY=j i DY=k ma rozkład Tweedie ze średnią wyrażoną jako funkcja czynników związanych z rokiem wypadku j i rokiem rozwoju k. Dane przedstawione w trójkątach run-off zazwyczaj wykazują następujące dwa przeciwstawne efekty:
*   ogólnie, większość roszczeń jest zgłaszana na wczesnym etapie rozwoju, więc składnik częstotliwości ma malejący trend w trakcie rozwoju;
*   średni koszt na roszczenie wzrasta w latach rozwoju, więc składnik wysokości szkody wykazuje rosnący trend.

Ponieważ częstotliwości i wysokości szkód mają przeciwstawne trendy, modele ze stałą dyspersją są podatne na błędy. W przypadku modelu Tweedie, stałe $\phi$ oznacza, że wpływ $j$ i $k$ na składniki zarówno częstotliwości, jak i wysokości szkody musi iść w tym samym kierunku. Dlatego pożądane jest przejście na podwójny model GLM, w którym zarówno średnia, jak i wariancja zależą od efektów $j$ i $k$.