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

Model ten różni się od poprzednich tym, że funkcja gęstości prawdopodobieństwa maleje do 0 proporcjonalnie, a nie wykładniczo. To wolniejsze dążenie do zera oznacza, że więcej masy prawdopodobieństwa może znajdować się w górnym ogonie rozkładu. Taka odpowiedź może odchylać się od swojej średniej w znacznie większym stopniu (ta ostatnia może nawet stać się nieskończona, gdy $\xi \le 1$).

Aby otrzymać rozkład Pareto, wystarczy rozważyć

$$
Y = \tau(\exp(Z) - 1) \quad \text{z} \quad Z \sim \mathcal{Exp}(\xi)
$$

tak że

$$
\begin{aligned}
P[Y > y] &= P\left[Z > \ln\left(1 + \frac{y}{\tau}\right)\right] \\
&= \left(1 + \frac{y}{\tau}\right)^{-\xi} \\
&= \left(\frac{\tau}{y+\tau}\right)^\xi.
\end{aligned}
$$

Odtąd oznaczamy jako $\mathcal{Par}(\alpha, \tau)$ rozkład Pareto z dodatnimi parametrami $\alpha$ i $\tau$. Jak wspomniano wcześniej w przypadku rozkładu log-normalnego, wcześniejsza transformacja do skali logarytmicznej nie jest optymalna z punktu widzenia statystycznego.

Średnia rozkładu Pareto jest nieskończona, jeśli $\alpha \le 1$. Dla $\alpha > 1$ średnia jest równa $\frac{\tau}{\alpha-1}$. Wariancja jest zdefiniowana dla $\alpha > 2$ i jest równa $\frac{\alpha\tau^2}{(\alpha-2)(\alpha-1)^2}$.

### 2.3.2 Odwrotny rozkład Gaussa

#### 2.3.2.1 Funkcja gęstości prawdopodobieństwa odwrotnego rozkładu Gaussa

Odwrotny rozkład Gaussa pojawia się czasami w literaturze aktuarialnej jako kolejny dobry kandydat do modelowania dodatnich, prawostronnie skośnych danych. Jego właściwości przypominają właściwości rozkładów Gamma i log-normalnego. Jego nazwa wywodzi się z odwrotnej relacji, jaka istnieje między funkcją generującą kumulanty odwrotnego rozkładu Gaussa a funkcją rozkładu normalnego (lub Gaussa).

Przypomnijmy, że zmienna losowa $Y$ o wartościach w $\mathcal{S}=(0, \infty)$ ma odwrotny rozkład Gaussa z parametrami $\mu>0$ i $\alpha>0$, co odtąd będziemy oznaczać jako $Y \sim \mathcal{IGau}(\mu, \alpha)$, jeśli jej funkcja gęstości prawdopodobieństwa jest dana wzorem

$$
f_Y(y) = \sqrt{\frac{\alpha}{2\pi y^3}} \exp\left(-\frac{\alpha(y-\mu)^2}{2y\mu^2}\right), \quad y>0. \quad (2.7)
$$

Odwrotny rozkład Gaussa z $\mu=1$ jest znany jako rozkład Walda.

#### 2.3.2.2 Momenty

Jeśli $Y \sim \mathcal{IGau}(\mu, \alpha)$, to momenty są dane przez

$$
E[Y] = \mu \quad \text{i} \quad \text{Var}[Y] = \frac{\mu^3}{\alpha} = \frac{1}{\alpha}(E[Y])^3. \quad (2.8)
$$

Podobnie jak w przypadku rozkładu Gamma, wariancja odwrotnego rozkładu Gaussa rośnie wraz ze średnią, ale w szybszym tempie (sześciennym zamiast kwadratowego).

Odwrotny rozkład Gaussa zyskał na znaczeniu w opisywaniu i analizowaniu danych prawostronnie skośnych. Główną zaletą modeli odwrotnego rozkładu Gaussa jest fakt, że mogą one przyjmować różne kształty, od wysoce skośnych do prawie normalnych. W prawdopodobieństwie stosowanym odwrotny rozkład Gaussa pojawia się jako rozkład czasu pierwszego przejścia do bariery absorbującej zlokalizowanej w jednostkowej odległości od początku w procesie Wienera (ruch Browna).

Rysunek 2.3 przedstawia funkcje gęstości prawdopodobieństwa odwrotnego rozkładu Gaussa dla różnych wartości parametrów. Przyjmujemy tutaj tę samą średnią i wariancje jak w przypadku Gamma powyżej, tj. $\mu=1$ i $\alpha=1, 2, 4$.

#### 2.3.2.3 Postać ED

Aby pokazać, że odwrotny rozkład Gaussa należy do rodziny ED, przepiszmy funkcję gęstości prawdopodobieństwa $\mathcal{IGau}(\mu, \alpha)$ (2.7) w następujący sposób

$$
\begin{aligned}
f_Y(y) &= \sqrt{\frac{\alpha}{2\pi y^3}} \exp\left(-\frac{\alpha(y-\mu)^2}{2y\mu^2}\right) \\
&= \exp\left(-\frac{\alpha(y^2-2y\mu+\mu^2)}{2y\mu^2}\right) \sqrt{\frac{\alpha}{2\pi y^3}} \\
&= \exp\left(\frac{y\left(-\frac{1}{2\mu^2}\right) - \left(-\frac{1}{\mu}\right)}{1/\alpha}\right) \exp\left(-\frac{\alpha}{2y}\right)\sqrt{\frac{\alpha}{2\pi y^3}}
\end{aligned}
$$

co jest zgodne z postacią (2.3) z

$$
\begin{aligned}
\theta &= -\frac{1}{2\mu^2} \\
a(\theta) &= -\frac{1}{\mu} = -\sqrt{-2\theta} \\
\phi &= \frac{1}{\alpha} \\
c(y, \phi) &= \exp\left(-\frac{\alpha}{2y}\right)\sqrt{\frac{\alpha}{2\pi y^3}}.
\end{aligned}
$$

Odwrotny rozkład Gaussa należy więc do rodziny ED.

#### 2.3.2.4 Porównanie rozkładów log-normalnego, Gamma i odwrotnego Gaussa

Naturalne jest chęć porównania prawostronnie skośnych funkcji gęstości prawdopodobieństwa odpowiadających rozkładom log-normalnemu, Gamma i odwrotnemu Gaussa. W tym celu aktuariusze wykonują porównanie dla identycznej średniej i wariancji. W szczególności rozważmy zmienne losowe $X$ podlegające rozkładowi Gamma, $Y$ podlegające odwrotnemu rozkładowi Gaussa i $Z$ podlegające rozkładowi log-normalnemu, tak że

$$
E[X] = E[Y] = E[Z] \quad \text{i} \quad \text{Var}[X] = \text{Var}[Y] = \text{Var}[Z].
$$

Wówczas Kaas i Hesselager (1995, Twierdzenie 3.1) ustalili, że nierówności

$$
\begin{aligned}
E[(X-t)_+^2] &\le E[(Y-t)_+^2] \\
E[(X-t)_+^3] &\le E[(Z-t)_+^3] \\
E[(Y-t)_+^3] &\le E[(Z-t)_+^3]
\end{aligned}
$$

zachodzą dla wszystkich $t \ge 0$. Zatem nierówności

$$
E[(X-t)_+^3] \le E[(Y-t)_+^3] \le E[(Z-t)_+^3] \text{ zachodzą dla wszystkich } t \ge 0,
$$

co pokazuje, że ogony rozkładu log-normalnego są cięższe w porównaniu z ogonami odwrotnego rozkładu Gaussa, które z kolei są cięższe w porównaniu z ogonami rozkładu Gamma.

Jako przykład, Rys. 2.4 przedstawia trzy funkcje gęstości prawdopodobieństwa z tą samą średnią 1 i wariancją 0.5. Widzimy, że te trzy funkcje gęstości prawdopodobieństwa rzeczywiście zgadzają się z tym rankingiem dla dostatecznie dużych wartości ich argumentu.

### 2.3.3 Rozkład dwumianowy

#### 2.3.3.1 Próba Bernoulliego

Wiele rozkładów zliczających jest definiowanych za pomocą prób Bernoulliego. W próbie Bernoulliego dany eksperyment prowadzący do sukcesu lub porażki jest powtarzany w identycznych warunkach (więc poprzednie wyniki nie wpływają na kolejne). Oznacza to, że pewne zdarzenie $E$ jest dychotomiczne, np. sukces lub porażka, żywy lub martwy, obecność lub brak. Wynik próby Bernoulliego to zatem 0 lub 1, tak lub nie. Zmienną interesującą jest liczba sukcesów. W próbach Bernoulliego analityk jest zainteresowany prawdopodobieństwem $q$, że zdarzenie wystąpi. Jest to w istocie skłonność lub prawdopodobieństwo zapadalności.

#### 2.3.3.2 Rozkład Bernoulliego

Odpowiedź $Y$ o wartościach w $\mathcal{S}=\{0,1\}$ ma rozkład Bernoulliego z prawdopodobieństwem sukcesu $q$, co oznaczamy jako $Y \sim \mathcal{Ber}(q)$. Odpowiednia funkcja masy prawdopodobieństwa jest dana przez

$$
p_Y(y) = 
\begin{cases}
1-q & \text{jeśli } y=0 \\
q & \text{jeśli } y=1 \\
0 & \text{w przeciwnym razie}.
\end{cases}
$$

Jest tu tylko jeden parametr: prawdopodobieństwo sukcesu $q$. Średnia i wariancja $Y \sim \mathcal{Ber}(q)$ są dane przez

$$
E[Y] = q \quad \text{i} \quad \text{Var}[Y] = q(1-q). \quad (2.9)
$$

Taka odpowiedź $Y$ jest zmienną wskaźnikową: dla pewnego zdarzenia $E$ o prawdopodobieństwie $q$,

$$
Y = I[E] = 
\begin{cases}
1 & \text{jeśli } E \text{ wystąpi} \\
0 & \text{w przeciwnym razie}.
\end{cases}
$$

Bardziej ogólnie, oznaczamy przez $I[\cdot]$ funkcję wskaźnikową równą 1, jeśli warunek w nawiasach jest spełniony, i 0 w przeciwnym razie.

Aby sprawdzić, czy rozkład Bernoulliego należy do rodziny ED, musimy pokazać, że odpowiednia funkcja masy prawdopodobieństwa $p_Y$ ma postać (2.3). W tym celu napiszmy

$$
\begin{aligned}
p_Y(y) &= q^y(1-q)^{1-y} \\
&= \exp\left(y\ln\frac{q}{1-q} + \ln(1-q)\right),
\end{aligned}
$$

co odpowiada funkcji masy prawdopodobieństwa ED (2.3) z

$$
\begin{aligned}
\theta &= \ln\frac{q}{1-q} \\
a(\theta) &= -\ln(1-q) = \ln(1+\exp(\theta)) \\
\phi &= 1 \\
v &= 1 \\
c(y, \phi) &= 1.
\end{aligned}
$$

To pokazuje, że rozkład Bernoulliego rzeczywiście należy do rodziny ED.

#### 2.3.3.3 Rozkład dwumianowy

Rozkład dwumianowy odpowiada liczbie sukcesów odnotowanych w sekwencji $m$ niezależnych prób Bernoulliego, każda z tym samym prawdopodobieństwem sukcesu $q$. Oznaczając przez $Y$ liczbę sukcesów w zbiorze $\mathcal{S}=\{0, 1, \dots, m\}$, prawdopodobieństwo, że wynik prób wynosi dokładnie $y$ jest

$$
p_Y(y) = \binom{m}{y} q^y (1-q)^{m-y}, \quad y=0,1,\dots,m, \quad (2.10)
$$

i 0 w przeciwnym razie, gdzie współczynnik dwumianowy jest zdefiniowany jako

$$
\binom{m}{y} = \frac{m!}{y!(m-y)!}
$$

jest liczbą sposobów wyboru $y$ sukcesów spośród $m$ prób. Mamy teraz dwa parametry: liczbę prób $m$ (zwana także wykładnikiem lub rozmiarem) i prawdopodobieństwo sukcesu $q$. Często $m$ jest znane aktuariuszowi, podczas gdy $q$ jest parametrem do oszacowania. Piszemy $Y \sim \mathcal{Bin}(m,q)$, aby wskazać, że $Y$ ma rozkład dwumianowy, stąd $m$ i prawdopodobieństwo sukcesu $q$. Zauważ, że definicja sukcesu w odniesieniu do porażki jest arbitralna, w tym sensie, że

$$
Y \sim \mathcal{Bin}(m,q) \Leftrightarrow m-Y \sim \mathcal{Bin}(m, 1-q).
$$

Kształt funkcji masy prawdopodobieństwa rozkładu dwumianowego jest przedstawiony na wykresach na Rys. 2.5 dla $m=100$ i zmieniających się wartości $q$. Dokładniej, Rys. 2.5 przedstawia wykresy słupkowe, gdzie wysokość jest równa masie prawdopodobieństwa zlokalizowanej w odpowiedniej liczbie całkowitej. Dla małych $q$ widzimy, że funkcja masy prawdopodobieństwa jest wysoce asymetryczna, koncentrując masę prawdopodobieństwa na mniejszych wynikach. Gdy prawdopodobieństwo sukcesu $q$ staje się większe, funkcja masy prawdopodobieństwa rozkładu dwumianowego staje się bardziej symetryczna, a jej kształt ostatecznie wydaje się podobny do (dyskretnej wersji) rozkładu normalnego. Jest to konsekwencja centralnego twierdzenia granicznego, ponieważ rozkład dwumianowy odpowiada sumie niezależnych prób Bernoulliego. Zbieżność do granicznego rozkładu normalnego jest szybsza, gdy wyniki Bernoulliego stają się bardziej symetryczne, to znaczy, gdy $q$ zbliża się do 0.5.

Oczywiście,

$$
\left.
\begin{aligned}
Y_1 &\sim \mathcal{Bin}(m_1, q) \\
Y_2 &\sim \mathcal{Bin}(m_2, q) \\
Y_1 &\text{ i } Y_2 \text{ niezależne}
\end{aligned}
\right\} \Rightarrow Y_1 + Y_2 \sim \mathcal{Bin}(m_1 + m_2, q).
$$

Z (2.9) łatwo wywnioskować, że dla $Y \sim \mathcal{Bin}(m,q)$,

$$
E[Y] = mq \quad \text{i} \quad \text{Var}[Y] = mq(1-q). \quad (2.11)
$$

Łatwo zauważyć, że wariancja rozkładu dwumianowego jest maksymalna, gdy $q = \frac{1}{2}$. Natychmiast obserwujemy, że rozkład dwumianowy jest niedyspersyjny, tzn. jego wariancja jest mniejsza niż jego średnia:

$$
\text{Var}[Y] = mq(1-q) \le mq = E[Y].
$$

Trzeci moment centralny, mierzący skośność, jest dany przez $mq(1-q)(1-2q)$. Stąd rozkład dwumianowy jest symetryczny, gdy $q=\frac{1}{2}$. Widać to z ostatniego panelu na Rys. 2.5. Dla wszystkich innych wartości $q$, funkcja masy prawdopodobieństwa rozkładu dwumianowego jest skośna (dodatnio skośna dla $q$ mniejszego niż 0.5, ujemnie skośna dla $q$ większego niż 0.5).

_Uwaga 2.3.1_ Rozważmy niezależne zmienne losowe $Y_i \sim \mathcal{Ber}(q_i), i=1,2,\dots,m$, i zdefiniujmy $Y_\bullet = \sum_{i=1}^m Y_i$. Zmienna losowa $Y_\bullet$ nie podlega rozkładowi dwumianowemu z powodu nierównych prawdopodobieństw sukcesu w próbach. Pierwsze momenty $Y_\bullet$ można uzyskać w następujący sposób:

$$
E[Y_\bullet] = \sum_{i=1}^m q_i = m\bar{q} \quad \text{z} \quad \bar{q} = \frac{1}{m}\sum_{i=1}^m q_i
$$

oraz

$$
\begin{aligned}
\text{Var}[Y_\bullet] &= \sum_{i=1}^m q_i(1-q_i) \\
&= m\bar{q}(1-\bar{q}) - m\sigma_q^2
\end{aligned}
$$

gdzie

$$
\sigma_q^2 = \frac{1}{m}\sum_{i=1}^m q_i^2 - (\bar{q})^2
$$

jest wariancją $q_i$. Oznacza to, że wariancja $Y_\bullet$ jest mniejsza niż wariancja $m\bar{q}(1-\bar{q})$ rozkładu dwumianowego o rozmiarze $m$ i prawdopodobieństwie sukcesu $\bar{q}$ (mającym taką samą średnią jak $Y_\bullet$). Innymi słowy, dopuszczenie indywidualnego sukcesu wariancji daje mniej niż standardowa wariancja dwumianowa. Inaczej mówiąc, rozkład dwumianowy maksymalizuje wariancję sumy niezależnych zmiennych losowych Bernoulliego przy stałej średniej.

Aby pokazać, że rozkład dwumianowy $\mathcal{Bin}(m,q)$ należy do rodziny ED, musimy przepisać jego funkcję masy prawdopodobieństwa w następujący sposób:

$$
\begin{aligned}
p_Y(y) &= \binom{m}{y} q^y (1-q)^{m-y} \\
&= \exp\left(y\ln\frac{q}{1-q} + m\ln(1-q)\right)\binom{m}{y}
\end{aligned}
$$

gdzie rozpoznajemy funkcję masy prawdopodobieństwa ED (2.3) z

$$
\begin{aligned}
\theta &= \ln\frac{q}{1-q} \\
a(\theta) &= -m\ln(1-q) = m\ln(1+\exp(\theta)) \\
\phi &= 1 \\
v &= 1 \\
c(y, \phi) &= \binom{m}{y}.
\end{aligned}
$$

#### 2.3.3.4 Rozkłady geometryczny i Pascala

Rozkład ten powstaje z prób Bernoulliego, rozważając liczbę $Y$ porażek przed uzyskaniem $m$ sukcesów, dla pewnej ustalonej dodatniej wartości całkowitej $m$. Teraz odpowiedź $Y$ nie jest już ograniczona z góry, tj. $\mathcal{S}=\{0,1,2,\dots\}$. Jej funkcja masy prawdopodobieństwa to

$$
p_Y(y) = \binom{m+y-1}{y}q^m(1-q)^y, \quad y=0,1,2,\dots \quad (2.12)
$$

Rozkład ten jest odtąd określany jako rozkład Pascala, od nazwiska jego wynalazcy. Jest oznaczany jako $Y \sim \mathcal{Pas}(m,q)$. W zastosowaniach ubezpieczeniowych rozkład Pascala może być użyty do opisania liczby akt szkodowych w trakcie dochodzenia przed wykryciem danej liczby $m$ fałszywych.

Dla $m=1$ otrzymujemy dany rozkład geometryczny $\mathcal{Geo}(q)$, dla którego

$$
p_Y(y) = q(1-q)^y, \quad y=0,1,2,\dots \quad (2.13)
$$

Rozkład geometryczny można postrzegać jako wynik dyskretyzacji rozkładu wykładniczego. W szczególności, rozważmy $Z \sim \mathcal{Exp}(\tau)$ i zdefiniujmy jego część całkowitą jako

$$
Y = [Z] = \max\{\text{integer } k \text{ such that } k \le Y\}.
$$

Wtedy,

$$
P[Y=0] = P[Z \le 1] = 1 - \exp(-\tau)
$$

i dla $y \in \{1,2,3,\dots\}$,

$$
\begin{aligned}
P[Y=y] &= P[Z < y+1] - P[Z \le y] \\
&= \exp(-y\tau) - \exp(-(y+1)\tau) \\
&= \exp(-y\tau)(1-\exp(-\tau))
\end{aligned}
$$

gdzie rozpoznajemy rozkład $\mathcal{Geo}(q)$ z

$$
q = 1 - \exp(-\tau).
$$

Podobnie jak jego ciągły odpowiednik, rozkład geometryczny cieszy się właściwością braku pamięci. Widać to z dyskretnej stopy awarii

$$
P[Y=y|Y \ge y] = 1-q \text{ niezależnie od } y \in \{0,1,2,\dots\},
$$

która wydaje się być stała.

Średnia i wariancja $Y \sim \mathcal{Pas}(m,q)$ są dane przez

$$
E[Y] = \frac{m(1-q)}{q} \quad \text{i} \quad \text{Var}[Y] = \frac{m(1-q)}{q^2}.
$$

Wariancja jest zatem zawsze większa niż średnia. Trzeci moment centralny wynosi $(2-q)\sqrt{m(1-q)}/q$ tak, że rozkład Pascala jest zawsze dodatnio skośny, ale staje się bardziej symetryczny w miarę wzrostu $m$. Kształt funkcji masy prawdopodobieństwa Pascala jest przedstawiony na wykresach na Rys. 2.6. Widzimy, że funkcja masy prawdopodobieństwa $\mathcal{Pas}(m,q)$ jest rzeczywiście zawsze dodatnio skośna, ale staje się bardziej symetryczna dla dużych wartości $m$.

Z definicji łatwo zauważyć, że rozkłady Pascala cieszą się wygodną właściwością stabilności splotu dla ustalonego $q$, tj.

$$
\left.
\begin{aligned}
Y_1 &\sim \mathcal{Pas}(m_1, q) \\
Y_2 &\sim \mathcal{Pas}(m_2, q) \\
Y_1 &\text{ i } Y_2 \text{ niezależne}
\end{aligned}
\right\} \Rightarrow Y_1 + Y_2 \sim \mathcal{Pas}(m_1 + m_2, q). \quad (2.14)
$$

Każdą odpowiedź $Y$ podlegającą rozkładowi $\mathcal{Pas}(m,q)$ można zatem postrzegać jako sumę $m$ niezależnych składników o rozkładzie $\mathcal{Geo}(q)$.

Aby pokazać, że rozkład Pascala należy do rodziny ED, przepiszmy funkcję masy prawdopodobieństwa (2.12) jako

$$
p_Y(y) = \exp\left(y\ln(1-q) + m\ln(q)\right)\binom{m+y-1}{y}
$$

co ma postać (2.3) z

$$
\begin{aligned}
\theta &= \ln(1-q) \\
a(\theta) &= -m\ln(q) = -m\ln(1-\exp(\theta)) \\
\phi &= 1 \\
v &= 1 \\
c(y, \phi) &= \binom{m+y-1}{y}.
\end{aligned}
$$

Zatem rozkład Pascala należy do rodziny ED (przypomnijmy, że $m$ nie jest parametrem do oszacowania, ale znaną dodatnią liczbą całkowitą).

### 2.3.4 Rozkład Poissona

#### 2.3.4.1 Graniczne formy rozkładów dwumianowych

Dwa ważne rozkłady pojawiają się jako aproksymacje rozkładów dwumianowych. Jeśli $m$ jest duże, a skośność rozkładu nie jest zbyt duża (tzn. $q$ nie jest zbyt bliskie 0 lub 1), to rozkład dwumianowy jest dobrze aproksymowany przez rozkład normalny. Jest to bezpośrednia konsekwencja centralnego twierdzenia granicznego i jest to wyraźnie widoczne na Rys. 2.5.

Gdy liczba prób $m$ jest duża, a prawdopodobieństwo sukcesu $q$ jest małe, odpowiedni rozkład dwumianowy jest dobrze aproksymowany przez rozkład Poissona ze średnią $\lambda = mq$. Aby zobaczyć, dlaczego tak jest, rozważmy $Y_m \sim \mathcal{Bin}(m, \frac{\lambda}{m})$ dla pewnego $\lambda > 0$. Wtedy,

$$
P[Y_m = 0] = \left(1-\frac{\lambda}{m}\right)^m \to \exp(-\lambda) \text{ gdy } m \to \infty
$$

oraz

$$
\frac{P[Y_m = y+1]}{P[Y_m = y]} = \frac{m-y}{y+1}\frac{\lambda/m}{1-\lambda/m} \to \frac{\lambda}{y+1} \text{ gdy } m \to \infty. \quad (2.15)
$$

Traktując (2.15) jako zależność rekurencyjną między kolejnymi prawdopodobieństwami, zaczynając od $\exp(-\lambda)$, otrzymujemy

$$
\lim_{m \to \infty} P[Y_m = y] = \exp(-\lambda)\frac{\lambda^y}{y!}
$$

co definiuje rozkład Poissona. Stąd rozkład $\mathcal{Bin}(m, \frac{\lambda}{m})$ jest w przybliżeniu rozkładem Poissona z parametrem $\lambda$ dla dużego $m$. Formalniej, jeśli $m \to \infty$ i $\lambda_m \to 0$ w taki sposób, że $m\lambda_m \to \lambda$, to rozkład $\mathcal{Bin}(m, \lambda_m)$ zbiega do rozkładu Poissona ze średnią $\lambda$. Rozkład Poissona jest zatem czasami nazywany prawem małych liczb, ponieważ jest to rozkład prawdopodobieństwa liczby wystąpień zdarzenia, które zdarza się rzadko, ale ma wiele okazji do wystąpienia.

Zazwyczaj zmienna losowa Poissona zlicza liczbę zdarzeń występujących w określonym przedziale czasowym lub obszarze przestrzennym. Na przykład liczba samochodów przejeżdżających przez stały punkt w pięciominutowym przedziale czasu lub liczba roszczeń zgłoszonych firmie ubezpieczeniowej przez ubezpieczonego kierowcę w danym okresie. Chociaż rozkład Poissona jest często nazywany prawem małych liczb, nie ma potrzeby, aby $\lambda = mq$ było małe. Ważna jest duża liczba $m$ i małość $q = \frac{\lambda}{m}$.

#### 2.3.4.2 Rozkład Poissona

Odpowiedź $Y$ o rozkładzie Poissona przyjmuje wartości w $\mathcal{S}=\{0,1,2,\dots\}$ i ma funkcję masy prawdopodobieństwa

$$
p_Y(y) = \exp(-\lambda)\frac{\lambda^y}{y!}, \quad y=0,1,2,\dots. \quad (2.16)
$$

Mając zliczającą zmienną losową $Y$, oznaczamy jako $Y \sim \mathcal{Poi}(\lambda)$ fakt, że $Y$ ma rozkład Poissona z parametrem $\lambda$. Parametr $\lambda$ jest często nazywany intensywnością, w odniesieniu do procesu Poissona (patrz poniżej).

Jeśli $Y \sim \mathcal{Poi}(\lambda)$, to graniczny argument podany wcześniej pokazuje, że

$$
E[Y] = \lambda \quad \text{i} \quad \text{Var}[Y] = \lambda. \quad (2.17)
$$

Biorąc pod uwagę (2.17), widzimy, że zarówno średnia, jak i wariancja rozkładu Poissona są równe $\lambda$, zjawisko to nazywane jest ekwidyspersją. Współczynnik skośności rozkładu Poissona wynosi

$$
\gamma[Y] = \frac{1}{\sqrt{\lambda}}.
$$

W miarę wzrostu $\lambda$, rozkład Poissona staje się bardziej symetryczny i ostatecznie jest dobrze aproksymowany przez rozkład normalny, przy czym aproksymacja staje się całkiem dobra dla $\lambda > 20$. Jeśli $Y \sim \mathcal{Poi}(\lambda)$, to $\sqrt{Y}$ zbiega znacznie szybciej do rozkładu $\mathcal{Nor}(\lambda, \frac{1}{4})$. Stąd transformacja pierwiastkowa była często zalecana jako transformacja stabilizująca wariancję dla danych zliczających w klasycznych metodach statystycznych zakładających normalność (i stałą wariancję).

Kształt funkcji masy prawdopodobieństwa Poissona jest przedstawiony na Rys. 2.7. Dla małych wartości $\lambda$ widzimy, że funkcja masy prawdopodobieństwa $\mathcal{Poi}(\lambda)$ jest wysoce asymetryczna. W miarę wzrostu $\lambda$ staje się bardziej symetryczna i ostatecznie wygląda jak krzywa dzwonowa.

Rozkład Poissona cieszy się wygodną właściwością stabilności splotu, tj.

$$
\left.
\begin{aligned}
Y_1 &\sim \mathcal{Poi}(\lambda_1) \\
Y_2 &\sim \mathcal{Poi}(\lambda_2) \\
Y_1 &\text{ i } Y_2 \text{ niezależne}
\end{aligned}
\right\} \Rightarrow Y_1 + Y_2 \sim \mathcal{Poi}(\lambda_1 + \lambda_2). \quad (2.18)
$$

Ta właściwość jest użyteczna, ponieważ czasami aktuariusz ma dostęp tylko do danych zagregowanych. Zakładając, że dane indywidualne mają rozkład Poissona, to zsumowany rachunek i modelowanie Poissona nadal mają zastosowanie.

Aby ustalić, że rozkład Poissona należy do rodziny ED, przepiszmy funkcję masy prawdopodobieństwa $\mathcal{Poi}(\lambda)$ (2.16) w następujący sposób:

$$
\begin{aligned}
p_Y(y) &= \exp(-\lambda)\frac{\lambda^y}{y!} \\
&= \exp(y\ln\lambda - \lambda)\frac{1}{y!}
\end{aligned}
$$

gdzie rozpoznajemy funkcję masy prawdopodobieństwa (2.3) z

$$
\begin{aligned}
\theta &= \ln\lambda \\
a(\theta) &= \lambda = \exp(\theta) \\
\phi &= 1 \\
v &= 1 \\
c(y, \phi) &= \frac{1}{y!}.
\end{aligned}
$$

Zatem rozkład Poissona rzeczywiście należy do rodziny ED.

#### 2.3.4.3 Ekspozycja na ryzyko

Liczba obserwowanych zdarzeń na ogół zależy od zmiennej rozmiaru, która określa liczbę okazji do wystąpienia zdarzenia. Ta zmienna rozmiaru jest często czasem, ponieważ liczba roszczeń oczywiście zależy od długości okresu ubezpieczenia. Możliwe są jednak inne wybory, na przykład odległość przebyta w ubezpieczeniach komunikacyjnych (jak zobaczymy w Rozdz. 5).

Ustawienie procesu Poissona jest przydatne, gdy aktuariusz chce analizować dane dotyczące roszczeń od ubezpieczonych, którzy byli obserwowani w nierównych okresach. W tym ustawieniu zakłada się, że roszczenia występują losowo i niezależnie w czasie zgodnie z procesem Poissona o intensywności $\lambda$. Oznaczając przez $T_1, T_2, \dots$ czasy między dwoma kolejnymi zdarzeniami, oznacza to, że te zmienne losowe są niezależne i podlegają rozkładowi $\mathcal{Exp}(\lambda)$, jedynemu, który cieszy się właściwością braku pamięci. Stąd, $k$-te roszczenie występuje w czasie

$$
\sum_{j=1}^k T_j \sim \mathcal{Gam}(k, \lambda)
$$

gdzie rozpoznajemy rozkład Erlanga.

Rozważmy ubezpieczonego objętego ochroną przez firmę przez okres o długości $e$, to znaczy ubezpieczony naraża ubezpieczyciela na ryzyko zarejestrowania roszczenia w ciągu $e$ jednostek czasu. Wówczas liczba $Y$ zgłoszonych roszczeń jest taka, że

$$
P[Y \ge k] = P\left[\sum_{j=1}^k T_j \le e\right] = 1 - \sum_{j=0}^{k-1}\exp(-\lambda e)\frac{(\lambda e)^j}{j!}
$$

więc ma funkcję masy prawdopodobieństwa

$$
P[Y=k] = P[Y \ge k] - P[Y \ge k+1] = \exp(-\lambda e)\frac{(\lambda e)^k}{k!}, \quad k=0,1,\dots,
$$

to jest, $Y \sim \mathcal{Poi}(\lambda e)$.

W badaniach aktuarialnych długość $e$ okresu obserwacji jest ogólnie określana jako ekspozycja na ryzyko (stąd litera $e$). Pozwala to analitykowi uwzględnić fakt, że niektóre polisy są obserwowane przez cały okres, podczas gdy inne właśnie weszły do portfela lub opuściły go wkrótce po rozpoczęciu okresu rocznego. Widzimy, że ekspozycja na ryzyko $e$ po prostu mnoży roczną oczekiwaną częstotliwość roszczeń $\lambda$ w modelu Poissona.

#### 2.3.4.4 Obcięty rozkład Poissona

Rozważmy odpowiedź Poissona $Y^*$ i załóżmy, że wartości 0 nie są obserwowane. Jest to zazwyczaj przypadek, gdy aktuariusz rozważa liczbę roszczeń zgłoszonych przez tych ubezpieczonych, którzy zgłosili co najmniej jedno roszczenie. Oznacza to, że aktuariusz pracuje z obserwowaną odpowiedzią $Y$ zdefiniowaną jako $Y^*$ pod warunkiem $Y^* \ge 1$, z funkcją masy prawdopodobieństwa

$$
p_Y(y) = P[Y^*=y|Y^* \ge 1] = \frac{\exp(-\lambda)\lambda^y}{y!(1-\exp(-\lambda))}, \quad y=1,2,\dots.
$$

Przepisując $p_Y$ jako

$$
p_Y(y) = \exp\left(y\ln\lambda - \lambda - \ln(1-\exp(-\lambda))\right)\frac{1}{y!}
$$

pokazuje, że obcięty rozkład Poissona ma funkcję masy prawdopodobieństwa w postaci (2.3) z

$$
\begin{aligned}
\theta &= \ln\lambda \\
a(\theta) &= \lambda + \ln(1-\exp(-\lambda)) \\
&= \exp(\theta) + \ln(1-\exp(-\exp(\theta))) \\
\phi &= 1 \\
v &= 1 \\
c(y, \phi) &= \frac{1}{y!}
\end{aligned}
$$

więc należy do rodziny ED. Ta właściwość jest interesująca, gdy aktuariusz rozważa użycie modeli Hurdle Poisson (badanych w Rozdz. 7).

#### 2.3.4.5 Rozkład wielomianowy

Schemat wielomianowy rozszerza próbę Bernoulliego w tym sensie, że dla każdego eksperymentu możliwe jest kilka wyników (a nie tylko sukces/porażka). W szczególności załóżmy, że istnieje $b$ typów prób, z odpowiednimi prawdopodobieństwami $q_1, \dots, q_b$ takimi, że $\sum_{i=1}^b q_i = 1$. Dla prób Bernoulliego, $b=2$ i $q_2 = 1-q_1$.

Oznaczmy przez $Y_i$ liczbę wyników typu $i$ w $m$ powtórzeniach tego eksperymentu. Zmienne losowe $Y_1, \dots, Y_b$ są ujemnie skorelowane, ponieważ zwiększenie liczby wyników jednego typu może tylko zmniejszyć liczbę wyników innych typów, ponieważ liczba prób $m$ jest stała. Oznacza to, że wektor losowy $\boldsymbol{Y} = (Y_1, \dots, Y_b)^\mathrm{T}$ musi być rozważany, a nie każda $Y_j$ z osobna.

Wektor losowy $\boldsymbol{Y}$ ma rozkład wielomianowy, ze wspólną funkcją masy prawdopodobieństwa $p_Y$ daną przez

$$
p_Y(y_1, \dots, y_b) = P[Y_1=y_1, \dots, Y_b=y_b] = \frac{m!}{y_1!\dots y_b!}q_1^{y_1}\dots q_b^{y_b}
$$

jeśli $y_1+\dots+y_b=m$ i 0 w przeciwnym razie, gdzie $m$ jest liczbą prób. Jest to odtąd oznaczane jako $\boldsymbol{Y} \sim \mathcal{Mult}(m, q_1, \dots, q_b)$. Oczywiście,

$$
\boldsymbol{Y} \sim \mathcal{Mult}(m, q_1, \dots, q_b) \Rightarrow Y_i \sim \mathcal{Bin}(m, q_i) \text{ dla } i=1,\dots,b.
$$

Zainteresowanie rozkładem wielomianowym w zastosowaniach ubezpieczeniowych wynika z następującego wyniku.

**Własność 2.3.2** Niech $Y$ będzie całkowitą liczbą roszczeń, taką że $Y \sim \mathcal{Poi}(\lambda)$. Załóżmy, że roszczenia $Y$ mogą być sklasyfikowane w $b$ kategoriach, zgodnie z wielomianowym schematem partycjonowania z prawdopodobieństwami $q_1, \dots, q_b$. Niech $Y_i$ reprezentuje liczbę roszczeń typu $i$, $i=1,\dots,b$. Wtedy, $Y_1, \dots, Y_b$ są niezależne i

$$
Y_i \sim \mathcal{Poi}(\lambda q_i), \quad i=1,\dots,b.
$$

_Dowód_ Aby pokazać, że $Y_i \sim \mathcal{Poi}(\lambda q_i)$, napiszmy

$$
\begin{aligned}
P[Y_i=y] &= \sum_{z=y}^{\infty} P[Y_i=y|Y=z]P[Y=z] \\
&= \sum_{z=y}^{\infty} \binom{z}{y}q_i^y(1-q_i)^{z-y} \exp(-\lambda)\frac{\lambda^z}{z!} \\
&= \exp(-\lambda)\frac{(\lambda q_i)^y}{y!} \sum_{z=y}^{\infty} \frac{((1-q_i)\lambda)^{z-y}}{(z-y)!} \\
&= \exp(-\lambda q_i)\frac{(\lambda q_i)^y}{y!}.
\end{aligned}
$$

Wzajemna niezależność $Y_1, \dots, Y_b$ wynika z

$$
\begin{aligned}
P[Y_1=y_1, \dots, Y_b=y_b] &= P[Y_1=y_1, \dots, Y_b=y_b|Y=y_1+\dots+y_b] \\
&\cdot P[Y=y_1+\dots+y_b] \\
&= \frac{(y_1+\dots+y_b)!}{y_1!\dots y_b!}q_1^{y_1}\dots q_b^{y_b} \exp(-\lambda)\frac{\lambda^{y_1+\dots+y_b}}{(y_1+\dots+y_b)!} \\
&= \prod_{j=1}^b \frac{\exp(-\lambda q_j)(\lambda q_j)^{y_j}}{y_j!} \\
&= \prod_{j=1}^b P[Y_j=y_j].
\end{aligned}
$$

To kończy dowód ogłoszonego wyniku.

W szczególności, mając niezależne $Y_i \sim \mathcal{Poi}(\lambda_i)$, widzimy, że rozkład dowolnej $Y_i$ pod warunkiem sumy $\sum_{j=1}^n Y_j = m$ jest $\mathcal{Bin}(m, q_i)$ z

$$
q_i = \frac{\lambda_i}{\sum_{j=1}^n \lambda_j}.
$$

### 2.4 Niektóre użyteczne właściwości

Tabela 2.1 podsumowuje główne wyniki uzyskane do tej pory. Dla każdego rozważanego rozkładu podajemy kanoniczny parametr $\theta$, funkcję kumulanty $a(\cdot)$, i parametr dyspersji $\phi$ wchodzący w ogólną definicję (2.3) masy prawdopodobieństwa lub funkcji gęstości prawdopodobieństwa rodziny ED. Podajemy również pierwsze dwa momenty. W tej sekcji ustalamy kilka właściwości rozkładów ED, które wydają się być szczególnie użyteczne w analizie danych ubezpieczeniowych.

Tabela 2.1 Przykłady rozkładów z rodziny ED (z $v=1$)

| Rozkład            | $\theta$                        | $a(\theta)$                             | $\phi$               | $\mu = E[Y]$                                | $\text{Var}[Y]$                                        |
| ------------------ | ------------------------------- | --------------------------------------- | -------------------- | ------------------------------------------- | ------------------------------------------------------ |
| $\mathcal{Ber}(q)$    | $\ln\frac{q}{1-q}$              | $\ln(1+\exp(\theta))$                   | $1$                  | $q$                                         | $\mu(1-\mu)$                                               |
| $\mathcal{Bin}(m,q)$  | $\ln\frac{q}{1-q}$              | $m\ln(1+\exp(\theta))$                  | $1$                  | $mq$                                        | $\mu(1-\frac{\mu}{m})$                                              |
| $\mathcal{Geo}(q)$    | $\ln(1-q)$                      | $-\ln(1-\exp(\theta))$                  | $1$                  | $\frac{1-q}{q}$                             | $\mu(1+\mu)$                                      |
| $\mathcal{Pas}(m,q)$  | $\ln(1-q)$                      | $-m\ln(1-\exp(\theta))$                 | $1$                  | $m\frac{1-q}{q}$                            | $\mu(1+\frac{\mu}{m})$                                      |
| $\mathcal{Poi}(\mu)$  | $\ln\mu$                        | $\exp(\theta)$                          | $1$                  | $\mu$                                       | $\mu$                                                  |
| $\mathcal{Nor}(\mu, \sigma^2)$ | $\mu$ | $-\frac{\theta^2}{2}$ | $\sigma^2$               | $\mu$                                       | $\phi$                              |
| $\mathcal{Exp}(\mu)$  | $-\frac{1}{\mu}$                | $-\ln(-\theta)$                         | $1$                  | $\mu$                                       | $\mu^2$                                                |
| $\mathcal{Gam}(\mu, \alpha)$ | $-\frac{1}{\mu}$          | $-\ln(-\theta)$                         | $\frac{1}{\alpha}$   | $\mu$                                       | $\phi\mu^2$                                            |
| $\mathcal{IGau}(\mu, \alpha)$ | $-\frac{1}{2\mu^2}$      | $-\sqrt{-2\theta}$                      | $\frac{1}{\alpha}$   | $\mu$                                       | $\phi\mu^3$                                            |