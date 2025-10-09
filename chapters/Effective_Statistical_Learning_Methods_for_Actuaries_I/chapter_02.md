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