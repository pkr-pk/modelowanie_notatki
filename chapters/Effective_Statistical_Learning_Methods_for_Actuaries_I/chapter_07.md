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

## 7.4 Modelowanie dyspersji w graduacji śmiertelności z duplikatami

### 7.4.1 Duplikaty

W praktyce często zdarza się, że osoby fizyczne posiadają więcej niż jedną polisę, a zatem pojawiają się w liczbie osób narażonych na ryzyko lub zgonów więcej niż raz. W takim przypadku mówi się, że portfel zawiera duplikaty: obejmuje on kilka polis dotyczących tych samych osób.

Zjawisko to można łatwo skorygować na poziomie każdej firmy ubezpieczeniowej, ale pozostaje problematyczne, gdy dane rynkowe są gromadzone i zestawiane przez jakąś agencję centralną. Dzieje się tak za sprawą Biura Ciągłych Badań Śmiertelności (Continuous Mortality Investigation - CMI) w Wielkiej Brytanii lub organów regulacyjnych (takich jak Bank Narodowy w Belgii). Nawet jeśli dane są deduplikowane przez każdego uczestnika przed przekazaniem statystyk śmiertelności do agencji centralnej, to jest, wszystkie polisy posiadane przez tę samą osobę są konsolidowane w jedną obserwację, nie można tego zrobić w odniesieniu do firm, ponieważ agencja centralna odpowiedzialna za gromadzenie danych (ponieważ dane są anonimizowane przez uczestniczące firmy przed ich przekazaniem) nie jest w stanie tego zrobić. Dane rynkowe generalnie zawierają wiele duplikatów, tak że zgony są mylone z roszczeniami z polis: śmierć ubezpieczającego posiadającego $m$ polis jest liczona jako $m$ zgonów w danych. Wobec braku informacji o rozkładzie polis na ubezpieczoną osobę, oszacowanie prawdopodobieństw zgonu staje się trudniejsze.

Gdy w portfelu występują duplikaty, aktuariusz zna liczbę roszczeń $c_x$, tj. liczbę polis, których posiadacz zmarł w wieku $x$ w okresie obserwacji, a nie rzeczywistą liczbę zgonów $d_x$ spośród $l_x$ osób w wieku $x$. Wiemy, że gdy w danych o śmiertelności występują duplikaty, nierówność $c_x \ge d_x$ jest prawdziwa. Ponadto, niech $n_x$ oznacza liczbę polis, których posiadacz ma wiek $x$ odnotowany w bazie danych, przy czym $n_x > l_x$, dolna granica odpowiada brakowi duplikatów.

### 7.4.2 Liczba umów, żyć, zgonów i roszczeń

Formalnie, oznaczmy przez $l_x$ liczbę ubezpieczających w wieku $x$ i przez $D_x$ liczbę zgonów odnotowanych wśród nich. Dla ułatwienia prezentacji załóżmy, że każdy ubezpieczający był objęty ochroną przez cały rok. Liczba zgonów $D_x$ ma zatem rozkład dwumianowy o wielkości $l_x$ i prawdopodobieństwie $q_x$.
Ponadto, zdefiniujmy zmienne losowe

$$
\begin{aligned}
N_{xi} &= \text{liczba umów posiadanych przez ubezpieczającego } i, \text{ w wieku } x \\
N_x &= \text{liczba umów posiadanych przez wszystkich ubezpieczających w wieku } x \\
&= \sum_{i=1}^{l_x} N_{xi}.
\end{aligned}
$$

Oznaczmy przez $C_{xi}$ liczbę roszczeń odpowiadającą ubezpieczającemu $i$, w wieku $x$. Ta zmienna losowa jest równa zero, jeśli ubezpieczający $i$ przeżyje, i jest równa $N_{xi}$ w przypadku jego lub jej śmierci.

### 7.4.3 Zależność między średnią a wariancją

Zakładamy, że zmienne losowe $C_{xi}$ są niezależne i mają identyczny rozkład z

$$
\mathrm{P}[C_{xi} = 0] = p_x
$$

i dla $k \ge 1$,

$$
\mathrm{P}[C_{xi} = k] = q_x \psi_x(k)
$$

gdzie $\psi_x(k) = \mathrm{P}[N_{xi}=k]$ oznacza prawdopodobieństwo, że osoba $i$ w wieku $x$ posiada $k$ polis, $k=1, 2, \dots$, z

$$
1 = \sum_{k=1}^\infty \psi_x^{(k)}.
$$

Innymi słowy, $\psi_x(k)$ odpowiada prawdopodobieństwu, że pojedynczy zgon zostanie zarejestrowany jako $k$ roszczeń.
Zdefiniujmy

$$
\bar{\psi}_x^{[j]} = \sum_{k=1}^\infty k^j \psi_x(k) \quad \text{dla } j \in \{1, 2\}.
$$

Wówczas,

$$
\mathrm{E}[C_{xi}] = q_x \sum_{k=1}^\infty k \psi_x(k) = q_x \bar{\psi}_x^{[1]}
$$

i

$$
\begin{aligned}
\mathrm{Var}[C_{xi}] &= \mathrm{E}[C_{xi}^2] - (\mathrm{E}[C_{xi}])^2 \\
&= q_x \sum_{k=1}^\infty k^2 \psi_x(k) - \left(q_x \sum_{k=1}^\infty k \psi_x(k)\right)^2 \\
&= q_x (\bar{\psi}_x^{[2]} - q_x(\bar{\psi}_x^{[1]})^2).
\end{aligned}
$$

Rozważmy całkowitą liczbę roszczeń odpowiadającą ubezpieczającym w wieku $x$, daną przez

$$
C_x = \sum_{i=1}^{l_x} C_{xi}.
$$

Mamy

$$
\mathrm{E}[C_x] = l_x \mathrm{E}[C_{x1}] = r_x q_x
$$

z

$$
r_x = l_x \bar{\psi}_x^{[1]}.
$$

Tutaj $r_x$ jest oczekiwaną liczbą polis posiadanych przez $l_x$ osób w wieku $x$. Ponadto,

$$
\mathrm{Var}[C_x] = l_x \mathrm{Var}[C_{x1}] = \phi_x r_x q_x (1-q_x)
$$

z

$$
\phi_x = \frac{1}{1-q_x \bar{\psi}_x^{[1]}} \frac{\bar{\psi}_x^{[2]}}{\bar{\psi}_x^{[1]}} \left(1 - \frac{(\bar{\psi}_x^{[1]})^2}{\bar{\psi}_x^{[2]}}q_x\right).
$$

### 7.4.4 Naddyspersyjny rozkład dwumianowy

Gdy w zbiorze danych nie ma duplikatów, $\psi_x(1)=1$ i $\psi_x(k)=0$ dla wszystkich $k \ge 2$. Wtedy mamy

$$
\phi_x=1, \quad r_x = l_x \quad \text{i} \quad C_x = D_x \sim \mathcal{Bin}(l_x, q_x).
$$

Gdy występują duplikaty, $\psi_x(k)>0$ dla pewnego $k \ge 2$. Wariancja liczby umów na ubezpieczoną osobę w wieku $x$ wynosi $\bar{\psi}_x^{[2]} - (\bar{\psi}_x^{[1]})^2$. Im większa zmienność w tym zakresie, tym większa wariancja i mniejszy stosunek $(\bar{\psi}_x^{[1]})^2 / \bar{\psi}_x^{[2]}$. W przypadku granicznym, gdy każdy ubezpieczający ma tylko jedną umowę, otrzymujemy $\phi_x=1$. Sugeruje to użycie przybliżenia

$$
\phi_x \approx \frac{\bar{\psi}_x^{[2]}}{\bar{\psi}_x^{[1]}} > 1
\tag{7.3}
$$

które wydaje się być dokładne, o ile $q_x$ pozostaje małe. Ponieważ jest to prawdą z wyjątkiem najstarszych grup wiekowych, przybliżenie (7.3) jest skuteczne w badaniach ubezpieczeniowych. Zgodnie z (7.3), $C_x$ podlega naddyspersyjnemu rozkładowi dwumianowemu.

### 7.4.5 Modelowanie dyspersji

Niech $\hat{\psi}_x(k)$ będzie proporcją osób w wieku $x$ posiadających $k$ polis. Wariancja $vr_x$, zdefiniowana jako

$$
vr_x = \frac{\sum_{k \ge 1} k^2 \hat{\psi}_x(k)}{\sum_{k \ge 1} k \hat{\psi}_x(k)}
$$

może być użyta jako estymator dla $\phi_x$. Badania przeprowadzone na rynku brytyjskim ujawniają wskaźniki wariancji w przedziale $(1, 2)$.
W przypadku braku informacji o wielokrotnym posiadaniu polis, nierealistycznym podejściem jest założenie, że $vr_x$ jest stałe w odniesieniu do $x$ (co jest oczywiście nierealistyczne) i użycie naddyspersyjnego modelu dwumianowego ze stałym parametrem dyspersji $\phi > 1$. Bardziej skutecznym podejściem jest uzupełnienie modelu dwumianowego GLM/GAM dla liczby zgonów o modelowanie dyspersji, w którym $\phi_x$ jest wyuczane z danych.
Gdy występują duplikaty, graduacja śmiertelności przebiega w dwóch etapach, w ramach podwójnego modelu GLM. W pierwszym etapie (modelowanie średniej), liczba roszczeń $C_x$ jest modelowana zgodnie z naddyspersyjnym rozkładem dwumianowym z

$$
\mathrm{E}[C_x] = r_x q_x \quad \text{i} \quad \mathrm{Var}[C_x] = \phi_x r_x q_x (1-q_x).
$$

Alternatywnie, dla otwartych grup, przydatne może być przybliżenie Poissona, z odpowiednią ekspozycją na ryzyko opartą na polisach $n_x$ i $q_x$ zastąpionym siłą śmiertelności $\mu_x$. W drugim etapie parametr dyspersji $\phi_x$ jest estymowany w modelu regresji Gamma z kwadratami reszt dewiacyjnych jako zmiennymi odpowiedzi. Ponieważ $\phi_x \in [1, 1+\kappa)$ dla pewnego $\kappa$ (gdzie $\kappa=1$ jest wspierane przez badania przeprowadzone na rynku brytyjskim), funkcja łącząca drugiego etapu $g_d$ powinna uwzględniać ten konkretny rynek. Dla $\kappa=1$ osiąga się to za pomocą przesuniętej komplementarnej funkcji log-log

$$
\phi_x = 2 - \exp(-\exp(\text{score}_x)),
$$

przesuniętej funkcji logitowej

$$
\phi_x = \frac{1+2\exp(\text{score}_x)}{1+\exp(\text{score}_x)},
$$

lub przesuniętej funkcji probitowej

$$
\phi_x = 1 + \Phi(\text{score}_x)
$$

gdzie, jak poprzednio, $\Phi(\cdot)$ oznacza dystrybuantę rozkładu $\mathcal{N}(0,1)$. Wynik (score) zaangażowany w modelowanie dyspersji jest zazwyczaj kwadratową funkcją wieku $x$, to jest,

$$
\text{score}_x = \gamma_0 + \gamma_1 x + \gamma_2 x^2,
$$

lub funkcją liniową $r_x$, to jest,

$$
\text{score}_x = \gamma_0 + \gamma_1 r_x.
$$

Wielką zaletą tego podejścia jest to, że znajomość prawdopodobieństw wielokrotnego posiadania polis $\psi_x(\cdot)$ nie jest koniecznie potrzebna do przeprowadzenia graduacji śmiertelności. Obecność duplikatów jest uwzględniana przez użycie specyficznych dla wieku parametrów dyspersji $\phi_x$, zastępujących empiryczne wskaźniki wariancji $vr_x$, w ramach podwójnego modelu GLM.
Na zakończenie wspomnijmy, że to samo podejście można zastosować do graduacji kwot w momencie zgonu, to jest, rozważając kwotę świadczeń wypłacanych przez towarzystwa ubezpieczeniowe zamiast rzeczywistej liczby zgonów.