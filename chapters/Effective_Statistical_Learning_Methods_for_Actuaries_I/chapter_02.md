# Rozdział 2: Wykładnicze rozkłady dyspersji (ED)

## 2.1 Wprowadzenie

Wszystkie modele w tej książce mają na celu analizę związku między zmienną, której wynik należy przewidzieć, a jedną lub kilkoma potencjalnymi zmiennymi objaśniającymi. Interesująca nas zmienna nazywana jest odpowiedzią i oznaczana jest jako $Y$. Analitycy ubezpieczeniowi zazwyczaj spotykają się z odpowiedziami innymi niż rozkład normalny, takimi jak

$$
Y = \begin{cases} 
N & \text{liczba roszczeń} \\
I & \text{skłonność do roszczeń } I[N \ge 1] \\
C_k & \text{koszt na roszczenie} \\
C & \text{średni koszt na roszczenie} \\
S & \text{roczny łączny koszt roszczeń} \\
T & \text{czas do zdarzenia, taki jak czas życia} \\
& \text{itd.}
\end{cases}
$$

Liczby roszczeń, wielkości roszczeń lub skłonności do roszczeń w ramach jednej polisy nie podlegają rozkładowi normalnemu, nawet w przybliżeniu. Dzieje się tak albo dlatego, że rozkład prawdopodobieństwa jest skoncentrowany na niewielkim zbiorze wartości dodatnich, albo dlatego, że funkcja gęstości prawdopodobieństwa nie jest symetryczna. Ogólnie rzecz biorąc, wariancja wzrasta wraz ze średnią odpowiedzi w zastosowaniach ubezpieczeniowych. Aby uwzględnić te specyfiki danych, aktuariusze często wybierają rozkład odpowiedzi z rodziny wykładniczej dyspersji (lub ED).

Przed przystąpieniem do szczegółowego badania rodziny ED, przypomnijmy sobie kilka podstawowych pojęć z zakresu prawdopodobieństwa. Liczby roszczeń są modelowane za pomocą nieujemnych całkowitoliczbowych zmiennych losowych (często nazywanych zliczającymi zmiennymi losowymi). Takie zmienne losowe są opisywane przez ich funkcję masy prawdopodobieństwa: dla danej zmiennej losowej $Y$ o wartościach w zbiorze $\{0, 1, 2, \dots\}$ nieujemnych liczb całkowitych, jej funkcja masy prawdopodobieństwa $p_Y$ jest zdefiniowana jako

$$
y \mapsto p_Y(y) = P[Y=y], \quad y = 0, 1, 2, \dots
$$

i ustalamy $p_Y(y) > 0$. Nośnik $\mathcal{S}$ jest zdefiniowany jako zbiór wszystkich $y$ takich, że $p_Y(y) > 0$. Wartość oczekiwana i wariancja są odpowiednio dane przez

$$
E[Y] = \sum_{y=0}^{\infty} y p_Y(y) \quad \text{i} \quad \text{Var}[Y] = \sum_{y=0}^{\infty} (y - E[Y])^2 p_Y(y).
$$

Roszczenia są modelowane przez nieujemne ciągłe zmienne losowe posiadające funkcję gęstości prawdopodobieństwa. Dokładniej, funkcja gęstości prawdopodobieństwa $f_Y$ takiej zmiennej losowej $Y$ jest zdefiniowana jako

$$
y \mapsto f_Y(y) = \frac{d}{dy} P[Y \le y], \quad y \in (-\infty, \infty).
$$

W tym przypadku,

$$
P[Y \approx y] = P\left[y - \frac{\Delta}{2} \le Y \le y + \frac{\Delta}{2}\right] \approx f_Y(y)\Delta
$$

dla dostatecznie małego $\Delta > 0$, więc $f_Y$ wskazuje również region, w którym $Y$ najprawdopodobniej wystąpi. W szczególności, $f_Y = 0$ tam, gdzie $Y$ nie może przyjąć żadnych wartości. Nośnik $\mathcal{S}$ zmiennej $Y$ jest zdefiniowany jako zbiór wszystkich $y$ takich, że $f_Y(y) > 0$. Wartość oczekiwana i wariancja są odpowiednio dane przez

$$
E[Y] = \int_{-\infty}^{\infty} y f_Y(y) dy \quad \text{i} \quad \text{Var}[Y] = \int_{-\infty}^{\infty} (y - E[Y])^2 f_Y(y) dy.
$$

Nawet jeśli sam rozkład normalny nie jest dobrym kandydatem do modelowania odpowiedzi interesujących w badaniach ubezpieczeniowych, wciąż odgrywa on centralną rolę w analizach przeprowadzanych przy użyciu rozkładów ED. Dlatego zaczynamy od tego podstawowego modelu prawdopodobieństwa, aby przejść do definicji rodziny ED i badania jej właściwości.

## 2.2 Od rozkładów normalnych do rozkładów ED

### 2.2.1 Rozkład normalny

#### 2.2.1.1 Funkcja gęstości prawdopodobieństwa rozkładu normalnego

Najstarszym rozkładem dla błędów w ustawieniach regresji jest z pewnością rozkład normalny, zwany także rozkładem Gaussa lub Gaussa-Laplace'a od nazwisk jego wynalazców.

Rodzina rozkładów ED w rzeczywistości rozszerza tę (ładną) strukturę tego prawa prawdopodobieństwa na bardziej ogólne błędy.

Przypomnijmy, że odpowiedź $Y$ o wartościach w $\mathcal{S} = (-\infty, \infty)$ ma rozkład normalny z parametrami $\mu \in (-\infty, \infty)$ i $\sigma^2 > 0$, oznaczany jako $Y \sim \mathcal{Nor}(\mu, \sigma^2)$, jeśli jej funkcja gęstości prawdopodobieństwa $f_Y$ jest

$$
f_Y(y) = \frac{1}{\sigma\sqrt{2\pi}} \exp\left(-\frac{1}{2\sigma^2}(y-\mu)^2\right), \quad y \in (-\infty, \infty). \quad (2.1)
$$

Biorąc pod uwagę (2.1), widzimy, że odpowiedzi o rozkładzie normalnym mogą przyjmować dowolną wartość rzeczywistą, dodatnią lub ujemną, ponieważ $f_Y > 0$ na całej prostej rzeczywistej $(-\infty, \infty)$.

Rysunek 2.1 przedstawia funkcję gęstości prawdopodobieństwa (2.1) dla różnych wartości parametrów. Funkcja gęstości prawdopodobieństwa $\mathcal{Nor}(\mu, \sigma^2)$ ma kształt symetrycznej krzywej dzwonowej wyśrodkowanej na $\mu$, przy czym $\sigma^2$ kontroluje rozrzut rozkładu. Ponieważ funkcja gęstości prawdopodobieństwa $f_Y$ jest symetryczna względem $\mu$, odchylenia dodatnie lub ujemne od średniej $\mu$ mają takie samo prawdopodobieństwo wystąpienia. Aby analiza oparta na rozkładzie normalnym była skuteczna, funkcja gęstości prawdopodobieństwa danych musi mieć kształt podobny do jednego z tych widocznych na Rys. 2.1, co rzadko ma miejsce w zastosowaniach ubezpieczeniowych.

#### 2.2.1.2 Momenty

Jeśli $Y \sim \mathcal{Nor}(\mu, \sigma^2)$, to $E[Y] = \mu$ i $\text{Var}[Y] = \sigma^2$. Widzimy, że wariancja $\sigma^2$ nie jest związana ze średnią $\mu$. Jest to rodzina lokalizacji-skali, ponieważ

$$
Y \sim \mathcal{Nor}(\mu, \sigma^2) \Leftrightarrow Z = \frac{Y-\mu}{\sigma} \sim \mathcal{Nor}(0, 1).
$$

Cała rodzina rozkładów normalnych może być zatem uzyskana ze standardowego rozkładu normalnego $\mathcal{Nor}(0, 1)$. Odpowiednia funkcja dystrybucji jest odtąd oznaczana jako $\Phi$, tj.

$$
\Phi(y) = P[Z \le y] = \int_{-\infty}^{y} \frac{1}{\sqrt{2\pi}} \exp\left(-\frac{1}{2}z^2\right)dz, \quad y \in (-\infty, \infty).
$$

Jeśli $Y \sim \mathcal{Nor}(\mu, \sigma^2)$, to

$$
P[Y \le y] = P\left[\frac{Y-\mu}{\sigma} \le \frac{y-\mu}{\sigma}\right] = \Phi\left(\frac{y-\mu}{\sigma}\right), \quad y \in (-\infty, \infty).
$$

Rozkład normalny cieszy się wygodną właściwością stabilności splotu: suma niezależnych, normalnie rozłożonych zmiennych losowych pozostaje normalna. Wynik ten pozostaje ważny, o ile obie zmienne losowe są łącznie normalne (co zostanie omówione w rozdz. 4).

#### 2.2.1.3 Twierdzenie graniczne centralne

Rozkład normalny naturalnie wynika z twierdzenia granicznego centralnego, jako przybliżony rozkład sumy wystarczająco wielu niezależnych i identycznie rozłożonych zmiennych losowych ze skończonymi wariancjami (i to podstawowe twierdzenie wyjaśnia, dlaczego rozkład normalny pozostaje użyteczny w kontekście rodziny ED). Przypomnijmy, że zgodnie z twierdzeniem granicznym centralnym, mając niezależne i identycznie rozłożone zmienne losowe $Y_1, Y_2, Y_3, \dots$ ze skończoną średnią $\mu$ i wariancją $\sigma^2$, standaryzowana średnia

$$
\frac{\bar{Y}_n - \mu}{\sigma/\sqrt{n}} = \frac{\sum_{i=1}^n Y_i - n\mu}{\sigma\sqrt{n}}
$$

w przybliżeniu podlega rozkładowi $\mathcal{Nor}(0, 1)$ pod warunkiem, że $n$ jest wystarczająco duże. Formalnie,

$$
P\left[\frac{\sum_{i=1}^n Y_i - n\mu}{\sigma\sqrt{n}} \le z\right] \to \Phi(z) \text{ dla wszystkich } z, \text{ gdy } n \to \infty.
$$

### 2.2.2 Rozkłady ED

Pozwól nam teraz przepisać funkcję gęstości prawdopodobieństwa $\mathcal{Nor}(\mu, \sigma^2)$, aby zademonstrować, jak rozszerzyć ją na większą klasę rozkładów prawdopodobieństwa o podobnych wygodnych właściwościach: rodzinę ED. Chodzi o to, jak następuje. Parametrem interesującym w cenach ubezpieczeń jest średnia $\mu$ zaangażowana w obliczenia składek czystych. Dlatego izolujemy składniki funkcji gęstości prawdopodobieństwa rozkładu normalnego, w których pojawia się $\mu$. Odbywa się to poprzez rozwinięcie kwadratu pojawiającego się wewnątrz funkcji wykładniczej w (2.1), co daje

$$
\begin{aligned}
f_Y(y) &= \frac{1}{\sigma\sqrt{2\pi}} \exp\left(-\frac{1}{2\sigma^2}(y^2 - 2y\mu + \mu^2)\right) \\
&= \exp\left(\frac{y\mu - \mu^2/2}{\sigma^2}\right) \frac{\exp\left(-\frac{y^2}{2\sigma^2}\right)}{\sigma\sqrt{2\pi}}. \quad (2.2)
\end{aligned}
$$

Drugi czynnik pojawiający się w (2.2) nie obejmuje $\mu$, więc ważny składnik jest pierwszy. Widzimy, że ma on bardzo prostą formę, będąc wykładnikiem (stąd przymiotnik „wykładniczy” w ED) stosunku z wariancją $\sigma^2$, tj. parametrem dyspersji, pojawiającym się w mianowniku. Licznik to różnica między iloczynem odpowiedzi $y$ i kanonicznego parametru normalnego $\mu$ a funkcją $\mu$, jedyną. Zauważ, że pochodna tego drugiego członu $\frac{\mu^2}{2}$ jest właśnie średnią $\mu$. Taka dekompozycja pozwala nam zdefiniować całą klasę ED rozkładów w następujący sposób.

Definicja 2.2.1 Rozważ odpowiedź $Y$ o wartościach w podzbiorze $\mathcal{S}$ prostej rzeczywistej $(-\infty, \infty)$. Mówi się, że jej rozkład należy do rodziny ED, jeśli $Y$ podlega funkcji masy prawdopodobieństwa $p_Y$ lub funkcji gęstości prawdopodobieństwa $f_Y$ w postaci

$$
\left.
\begin{aligned}
p_Y(y) \\
f_Y(y)
\end{aligned}
\right\} = \exp\left(\frac{y\theta - a(\theta)}{\phi/v}\right) c(y, \phi/v), \quad y \in \mathcal{S}, \quad (2.3)
$$

gdzie

*   $\theta$ = parametr lokalizacji o wartościach rzeczywistych, zwany parametrem kanonicznym
*   $\phi$ = dodatni parametr skali, zwany parametrem dyspersji
*   $v$ = znana dodatnia stała, zwana wagą
*   $a(\cdot)$ = monotoniczna funkcja wypukła $\theta$
*   $c(\cdot)$ = dodatnia funkcja normalizująca.

W większości zastosowań aktuarialnych waga odpowiada pewnej mierze wolumenu, stąd notacja $v$.

Parametry $\theta$ i $\phi$ są zasadniczo wskaźnikami lokalizacji i skali, rozszerzającymi średnią wartość $\mu$ i wariancję $\sigma^2$ na całą rodzinę rozkładów ED. Biorąc pod uwagę (2.2), widzimy, że jest to rzeczywiście forma (2.3) z

$$
\begin{aligned}
\theta &= \mu \\
a(\theta) &= \frac{\mu^2}{2} = \frac{\theta^2}{2} \\
\phi &= \sigma^2 \\
v &= 1 \\
c(y, \phi) &= \frac{\exp\left(-\frac{y^2}{2\sigma^2}\right)}{\sigma\sqrt{2\pi}}.
\end{aligned}
$$

_Uwaga 2.2.2_ Czasami (2.3) jest zastępowane bardziej ogólną formą

$$
\exp\left(\frac{y\theta - a(\theta)}{b(\phi, v)}\right) c(y, \phi, v).
$$

Szczególny przypadek, w którym $\phi$ i $v$ są połączone w $\phi/v$, tj.

$$
b(\phi, v) = \frac{\phi}{v} \quad \text{oraz} \quad c(y, \phi, v) = c\left(y, \frac{\phi}{v}\right)
$$

wydaje się być wystarczający do zastosowań aktuarialnych i jest zachowany w tej książce.

_Uwaga 2.2.3_ Funkcja $a(\cdot)$ jest czasami nazywana funkcją kumulant. Tę terminologię można wyjaśnić w następujący sposób. Rozważmy ciągłą, powiedzmy, zmienną losową $Z$ o wartościach w $\mathcal{S}$ z funkcją gęstości prawdopodobieństwa $f_Z(z) = c(z)$ i funkcją generującą momenty

$$
m_Z(t) = E[\exp(tZ)] = \int_\mathcal{S} \exp(tz) c(z) dz.
$$

Zdefiniujmy $a(\theta) = \ln m_Z(\theta)$, ta ostatnia funkcja jest określana jako funkcja generująca kumulanty $Z$. Wtedy,

$$
\int_\mathcal{S} \exp(\theta z) c(z) dz = \exp(a(\theta))
$$

lub

$$
\int_\mathcal{S} \exp(\theta z - a(\theta)) c(z) dz = 1 \text{ dla wszystkich } \theta.
$$

Stąd,

$$
z \mapsto \exp(\theta z - a(\theta)) c(z)
$$

definiuje rozkład ED z kanonicznym parametrem $\theta$. Taka konstrukcja jest często używana w stosowanym prawdopodobieństwie i nazywana jest wykładniczym pochyleniem $Z$. Zauważ, że dla $\theta=0$ po prostu odzyskujemy $f_Z$.

### 2.2.3 Niektóre rozkłady związane z rozkładem normalnym

Przed zbadaniem rodziny ED, przypomnijmy sobie krótko definicję niektórych standardowych rozkładów ściśle związanych z rozkładem normalnym.

#### 2.2.3.1 Rozkład log-normalny

W zastosowaniach aktuarialnych rozkład normalny jest często stosowany w skali logarytmicznej, tzn. zakładając, że $\ln Y \sim \mathcal{Nor}(\mu, \sigma^2)$. Mówi się wówczas, że odpowiedź $Y$ ma rozkład log-normalny. Odpowiednia funkcja gęstości prawdopodobieństwa jest dana wzorem

$$
f_Y(y) = \frac{1}{y\sigma\sqrt{2\pi}} \exp\left(-\frac{1}{2\sigma^2}(\ln y - \mu)^2\right), \quad y > 0.
$$

Rozkład log-normalny jest często używany do modelowania wielkości roszczeń. Jednak wcześniejsza transformacja danych do skali logarytmicznej może wpłynąć na predykcję, chyba że zostaną wprowadzone odpowiednie korekty. Głównym celem stosowania rodziny ED jest uniknięcie tej wstępnej transformacji i praca z oryginalną, nieprzekształconą odpowiedzią $Y$.

Jeśli $\ln Y \sim \mathcal{Nor}(\mu, \sigma^2)$, to $\exp(\mu)$ nie jest średnią, lecz medianą, jako że

$$
P[Y \le \exp(\mu)] = P[\ln Y \le \mu] = \frac{1}{2}.
$$

Średnia i wariancja są odpowiednio dane przez

$$
E[Y] = \exp\left(\mu + \frac{\sigma^2}{2}\right),
$$

oraz

$$
\text{Var}[Y] = (\exp(\sigma^2) - 1) \exp(2\mu + \sigma^2) = (\exp(\sigma^2) - 1)(E[Y])^2.
$$

Stąd widzimy, że wariancja jest kwadratową funkcją średniej. Ponieważ $\exp(\sigma^2) > 1$, wariancja rośnie wraz ze średnią.

Współczynnik skośności $\gamma$ zmiennej losowej jest zdefiniowany w następujący sposób: dla danej zmiennej losowej $Y$ o średniej $\mu$ i wariancji $\sigma^2$,

$$
\gamma[Y] = \frac{E[(Y - \mu)^3]}{\sigma^3}.
$$

Jeśli funkcja gęstości prawdopodobieństwa $Y$ jest symetryczna (jak w przypadku rozkładu normalnego), to $\gamma[Y] = 0$. Im większe $|\gamma[Y]|$, tym bardziej asymetryczny jest rozkład. Dla rozkładu log-normalnego, współczynnik skośności

$$
\gamma[Y] = (\exp(\sigma^2) + 2)\sqrt{\exp(\sigma^2) - 1}
$$

jest zawsze dodatni i zależy tylko od $\sigma^2$, a nie od $\mu$.

#### 2.2.3.2 Rozkład chi-kwadrat

Rozkład chi-kwadrat z $k$ stopniami swobody odpowiada rozkładowi sumy

$$
Y = Z_1^2 + Z_2^2 + \dots + Z_k^2
$$

niezależnych zmiennych losowych $Z_1, Z_2, \dots, Z_k$ o rozkładzie $\mathcal{Nor}(0, 1)$, podniesionych do kwadratu. Odtąd taki przypadek oznaczamy jako $Y \sim \chi_k^2$.

Podajmy funkcję gęstości prawdopodobieństwa rozkładu chi-kwadrat. Przypomnijmy, że funkcja Gamma $\Gamma(\cdot)$ jest zdefiniowana dla $\alpha \ge 0$ jako całka

$$
\Gamma(\alpha) = \int_0^\infty \exp(-z)z^{\alpha-1}dz.
$$

Oczywiście $\Gamma(1) = 1$, a całkowanie przez części pokazuje, że

$$
\Gamma(\alpha) = (\alpha-1)\Gamma(\alpha-1) \text{ dla każdego } \alpha > 1.
$$

Zatem funkcja Gamma może być postrzegana jako ciągła wersja silni, w tym sensie, że gdy $\alpha$ jest nieujemną liczbą całkowitą,

$$
\Gamma(\alpha + 1) = \alpha! = \prod_{j=0}^{\alpha-1} (\alpha - j).
$$

Funkcja gęstości prawdopodobieństwa rozkładu chi-kwadrat z $k$ stopniami swobody jest dana wzorem

$$
f_Y(y) = \frac{y^{k/2 - 1}\exp(-y/2)}{2^{k/2}\Gamma(k/2)}, \quad y > 0.
$$

Rozkład chi-kwadrat jest używany głównie w testowaniu hipotez, a nie do bezpośredniego modelowania odpowiedzi. Pojawia się w teście ilorazu wiarygodności dla zagnieżdżonych modeli, na przykład.

#### 2.2.3.3 Rozkład t-Studenta

Rozkład t-Studenta (lub po prostu rozkład t) jest związany ze średnią z normalnie rozłożonych odpowiedzi w sytuacjach, gdy wielkość próby jest mała, a odchylenie standardowe populacji jest nieznane. W szczególności, niech $Y_1, \dots, Y_n$ będą niezależnymi zmiennymi losowymi podlegającymi rozkładowi $\mathcal{Nor}(\mu, \sigma^2)$ i zdefiniujmy

$$
\bar{Y} = \frac{1}{n}\sum_{i=1}^n Y_i \quad \text{i} \quad S^2 = \frac{1}{n-1}\sum_{i=1}^n (Y_i - \bar{Y})^2.
$$

Wielkości te nazywane są średnią z próby i wariancją z próby. Wtedy,

$$
\frac{\bar{Y} - \mu}{\sigma/\sqrt{n}} \sim \mathcal{Nor}(0, 1),
$$

podczas gdy zmienna losowa

$$
T = \frac{\bar{Y} - \mu}{S/\sqrt{n}}
$$

ma rozkład t-Studenta z $n-1$ stopniami swobody. Odpowiednia funkcja gęstości prawdopodobieństwa to

$$
f_T(t) = \frac{\Gamma(n/2)}{\sqrt{(n-1)\pi}\Gamma(\frac{n-1}{2})} \left(1 + \frac{t^2}{n-1}\right)^{-n/2}, \quad t \in (-\infty, \infty).
$$

Funkcja gęstości prawdopodobieństwa rozkładu t-Studenta jest symetryczna, a jej ogólny kształt przypomina dzwonowaty kształt normalnie rozłożonej zmiennej losowej o średniej 0 i wariancji 1, z tą różnicą, że ma grubsze ogony. Staje się ona coraz bliższa rozkładowi normalnemu wraz ze wzrostem liczby stopni swobody.

Rozkład t-Studenta z $d$ stopniami swobody można ogólnie zdefiniować jako rozkład zmiennej losowej

$$
T = \frac{Z}{\sqrt{C/d}}
$$

gdzie $Z \sim \mathcal{Nor}(0, 1)$, $C \sim \chi_d^2$, przy czym $Z$ i $C$ są niezależne.

#### 2.2.3.4 Rozkład Fishera

Rozważmy dwie niezależne zmienne losowe o rozkładzie chi-kwadrat $C_1$ i $C_2$ z odpowiednio $d_1$ i $d_2$ stopniami swobody. Rozkład F z parametrami $d_1$ i $d_2$ odpowiada stosunkowi

$$
\frac{C_1/d_1}{C_2/d_2}.
$$

Rozkład ten naturalnie pojawia się w pewnych procedurach testowych.

### 2.3 Niektóre rozkłady z rodziny ED

#### 2.3.1 Rozkład Gamma

##### 2.3.1.1 Funkcja gęstości prawdopodobieństwa rozkładu Gamma

Rozkład Gamma jest prawostronnie skośny, z ostrym szczytem i długim ogonem po prawej stronie. Cechy te są często widoczne w empirycznych rozkładach kwot roszczeń. To sprawia, że rozkład Gamma jest naturalnym kandydatem do modelowania świadczeń z tytułu roszczeń wypłacanych przez ubezpieczyciela.

Dokładniej, mówi się, że zmienna losowa $Y$ o wartościach w $\mathcal{S} = (0, \infty)$ ma rozkład Gamma z parametrami $\alpha > 0$ i $\tau > 0$, co odtąd będziemy oznaczać jako $Y \sim \mathcal{Gam}(\alpha, \tau)$, jeśli jej funkcja gęstości prawdopodobieństwa jest dana wzorem

$$
f_Y(y) = \frac{\tau^\alpha y^{\alpha-1} \exp(-\tau y)}{\Gamma(\alpha)}, \quad y > 0. \quad (2.4)
$$

Parametr $\alpha$ jest często nazywany parametrem kształtu rozkładu Gamma, podczas gdy $\tau$ jest określany jako parametr skali.

##### 2.3.1.2 Momenty

Średnia i wariancja $Y \sim \mathcal{Gam}(\alpha, \tau)$ są odpowiednio dane przez

$$
E[Y] = \frac{\alpha}{\tau} \quad \text{i} \quad \text{Var}[Y] = \frac{\alpha}{\tau^2} = \frac{1}{\alpha}(E[Y])^2. \quad (2.5)
$$

Widzimy więc, że wariancja jest kwadratową funkcją średniej, podobnie jak w przypadku rozkładu log-normalnego. Rozkład Gamma jest użyteczny do modelowania dodatniej, ciągłej odpowiedzi, gdy wariancja rośnie wraz ze średnią, ale gdzie współczynnik zmienności

$$
\text{CV}[Y] = \frac{\sqrt{\text{Var}[Y]}}{E[Y]} = \frac{1}{\sqrt{\alpha}}
$$

pozostaje stały. Jak sugerują ich nazwy, parametr skali w rodzinie Gamma wpływa głównie na rozrzut (a pośrednio na położenie), ale nie na kształt rozkładu, podczas gdy parametr kształtu kontroluje skośność rozkładu. Dla $Y \sim \mathcal{Gam}(\alpha, \tau)$ mamy

$$
\gamma[Y] = \frac{2}{\sqrt{\alpha}}
$$

więc rozkład Gamma jest dodatnio skośny. W miarę wzrostu parametru kształtu $\alpha$, rozkład staje się bardziej symetryczny.

Rysunek 2.2 przedstawia funkcje gęstości prawdopodobieństwa Gamma dla różnych wartości parametrów. Ustalono tu średnią $\mu = \frac{\alpha}{\tau}$ na 1 i przyjęto $\tau \in \{1, 2, 4\}$, tak że wariancja jest równa 1, 0.5, i 0.25. W przeciwieństwie do rozkładu normalnego (którego funkcja gęstości prawdopodobieństwa przypomina kształt dzwonu wyśrodkowanego na $\mu$ niezależnie od wariancji $\sigma^2$), kształt funkcji gęstości prawdopodobieństwa Gamma zmienia się wraz z parametrem $\alpha$. Dla $\alpha \le 1$, funkcja gęstości prawdopodobieństwa ma maksimum w początku układu, podczas gdy dla $\alpha > 1$ jest jednomodalna, ale skośna. Skośność maleje wraz ze wzrostem $\alpha$.

Rozkłady Gamma cieszą się wygodną właściwością stabilności splotu dla ustalonego parametru skali $\tau$. W szczególności,

$$
\left.
\begin{aligned}
Y_1 &\sim \mathcal{Gam}(\alpha_1, \tau) \\
Y_2 &\sim \mathcal{Gam}(\alpha_2, \tau) \\
Y_1 &\text{ i } Y_2 \text{ niezależne}
\end{aligned}
\right\} \Rightarrow Y_1 + Y_2 \sim \mathcal{Gam}(\alpha_1 + \alpha_2, \tau). \quad (2.6)
$$

##### 2.3.1.3 Przypadki szczególne: Rozkład wykładniczy ujemny, Erlanga i chi-kwadrat

Gdy $\alpha = 1$, rozkład Gamma redukuje się do rozkładu wykładniczego ujemnego z funkcją gęstości prawdopodobieństwa

$$
f_Y(y) = \tau \exp(-\tau y), \quad y > 0.
$$

W tym przypadku piszemy $Y \sim \mathcal{Exp}(\tau)$. Rozkład ten cieszy się niezwykłą właściwością braku pamięci:

$$
P[Y > s+t | Y > s] = \frac{\exp(-\tau(s+t))}{\exp(-\tau s)} = \exp(-\tau t) = P[Y > t].
$$

Odpowiada to stałej intensywności śmierci lub awarii: $\frac{f_Y(t)}{1-F_Y(t)} = \tau$; osoba z czasem życia $Y \sim \mathcal{Exp}(\tau)$ nie starzeje się, podlegając stałej intensywności śmierci $\tau$ przez całe życie.

Gdy $\alpha$ jest dodatnią liczbą całkowitą, odpowiednia dystrybuanta rozkładu Gamma jest dana przez

$$
F(y) = 1 - \sum_{j=0}^{\alpha-1} \exp(-y\tau) \frac{(y\tau)^j}{j!}, \quad y \ge 0.
$$

Ten szczególny przypadek jest określany jako rozkład Erlanga. Rozkład Erlanga odpowiada rozkładowi sumy

$$
Y = Z_1 + Z_2 + \dots + Z_\alpha
$$

niezależnych zmiennych losowych $Z_1, Z_2, \dots, Z_\alpha$ o wspólnym rozkładzie $\mathcal{Exp}(\tau)$, na mocy (2.6). Stąd, gdy $\alpha = 1$, rozkład Erlanga redukuje się do rozkładu wykładniczego ujemnego.

Rozkład chi-kwadrat z $k$ stopniami swobody okazuje się być szczególnym przypadkiem rozkładu $\mathcal{Gam}(\alpha, \tau)$ z $\alpha = k/2$ i $\tau = 1/2$.

##### 2.3.1.4 Postać ED

Ustalmy teraz, że rozkład Gamma należy do rodziny ED. W tym celu wprowadźmy parametr średniej $\mu = \alpha/\tau$ do wyrażenia funkcji gęstości prawdopodobieństwa i przepiszmy funkcję gęstości prawdopodobieństwa $\mathcal{Gam}(\alpha, \tau)$ (2.4) w następujący sposób:

$$
\begin{aligned}
f_Y(y) &= \frac{\tau^\alpha}{\Gamma(\alpha)} y^{\alpha-1} \exp(-\tau y) \\
&= \frac{\alpha^\alpha \mu^{-\alpha}}{\Gamma(\alpha)} y^{\alpha-1} \exp\left(-\frac{y\alpha}{\mu}\right) \quad \text{z} \quad \mu = \frac{\alpha}{\tau} \Leftrightarrow \tau = \frac{\alpha}{\mu} \\
&= \exp\left(\alpha\left(-\frac{y}{\mu} - \ln\mu\right)\right) \frac{\alpha^\alpha}{\Gamma(\alpha)} y^{\alpha-1}
\end{aligned}
$$

co jest zgodne z postacią (2.3) z

$$
\begin{aligned}
\theta &= -\frac{1}{\mu} \\
a(\theta) &= \ln \mu = -\ln(-\theta) \\
\phi &= \frac{1}{\alpha} \\
c(y, \phi) &= \frac{\alpha^\alpha}{\Gamma(\alpha)}y^{\alpha-1}.
\end{aligned}
$$

Rozkład Gamma należy więc do rodziny ED.

##### 2.3.1.5 Rozkłady potęgowo-gamma, czyli Weibulla

Rozkład Weibulla jest często używany w badaniach niezawodności, ponieważ odpowiada rozkładowi minimum z dużej liczby niezależnych zmiennych losowych, z których każda odpowiada czasowi do wystąpienia innego rodzaju awarii. Taki element psuje się, gdy tylko jeden z jego komponentów zawiedzie. Rozkład Weibulla był również stosowany w ubezpieczeniach na życie, biorąc pod uwagę ludzkie ciało jako system szeregowy.

Rozważmy $Y = Z^{1/\alpha}$ dla pewnego $\alpha > 0$, gdzie $Z \sim \mathcal{Exp}(\tau)$. Wtedy $Y$ podlega rozkładowi Weibulla z funkcją gęstości prawdopodobieństwa

$$
f_Y(y) = \alpha \tau y^{\alpha-1} \exp(-\tau y^\alpha), \quad y > 0.
$$

Średnia rozkładu Weibulla wynosi

$$
E[Y] = \tau^{-1/\alpha} \Gamma\left(1 + \frac{1}{\alpha}\right)
$$

a wariancja

$$
\text{Var}[Y] = \tau^{-2/\alpha}\left(\Gamma\left(1+\frac{2}{\alpha}\right) - \left(\Gamma\left(1+\frac{1}{\alpha}\right)\right)^2\right).
$$

Rozkład Weibulla jest ściśle związany z modelowaniem przyspieszonego czasu życia. Zauważmy również, że jeśli $Y$ jest rozkładem Weibulla, to $\ln Y = \frac{\ln Z}{\alpha}$ podlega rozkładowi wartości ekstremalnych.

##### 2.3.1.6 Rozkłady log-gamma, czyli Pareto

Rozkłady log-gamma są przydatne do modelowania odpowiedzi o ciężkich ogonach. Biorąc pod uwagę $\ln Y \sim \mathcal{Exp}(\xi)$, mamy

$$
P[Y \le y] = 
\begin{cases} 
0, & \text{jeśli } y \le 1, \\
1 - y^{-\xi}, & \text{jeśli } y > 1. 
\end{cases}
$$