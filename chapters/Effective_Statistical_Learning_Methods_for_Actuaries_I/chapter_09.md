# Rozdział 9: Modele Wartości Ekstremalnych

## 9.1 Wprowadzenie

Podobnie jak w innych dziedzinach wrażliwych na obserwacje ekstremalne, na przykład w hydrologii i klimatologii, standardowe techniki statystyczne zawodzą przy analizie dużych szkód znajdujących się daleko w ogonie rozkładu ciężkości szkód. Dzieje się tak, ponieważ aktuariusz chce wnioskować o ekstremalnym zachowaniu rozkładów szkód, czyli w obszarze próby, w którym jest bardzo mało punktów danych, jeśli w ogóle.

Teoria Wartości Ekstremalnych (w skrócie **EVT**), wraz z modelem „nadwyżek ponad próg” oraz Uogólnionym Rozkładem Pareto, oferuje zunifikowane podejście do modelowania ogona rozkładu szkód. Metoda ta nie opiera się wyłącznie na dostępnych danych, ale zawiera probabilistyczny argument dotyczący zachowania ekstremalnych wartości z próby, co pozwala na ekstrapolację poza zakres danych, czyli w obszary, w których w ogóle nie ma obserwacji.

Omówmy krótko typowe sytuacje, w których aktuariusze zajmują się wartościami ekstremalnymi. W ubezpieczeniach odpowiedzialności cywilnej aktuariusze często napotykają trudności podczas analizy ciężkości szkód, ponieważ:

* występuje ograniczona liczba obserwacji, jako że tylko polisy, z których zgłoszono szkody, dostarczają informacji o ich kosztach.
* likwidacja szkody może trwać długo, przez co ostateczny koszt szkody jest w tym okresie nieznany firmie.
* kwoty wypłacane przez firmę w dużej mierze zależą od cech charakterystycznych strony trzeciej, które są nieznane w momencie obliczania składki.

Co więcej, kilka dużych szkód w portfelu często stanowi znaczną część świadczeń ubezpieczeniowych wypłacanych przez firmę. Te ekstremalne zdarzenia są zatem przedmiotem szczególnego zainteresowania aktuariuszy. Stanowią one również materiał statystyczny do:

* wyceny umów reasekuracyjnych, takich jak traktaty **reasekuracji nadwyżki szkody** (w ramach których reasekurator musi zapłacić za nadwyżkę szkody ponad ustalony próg, zwany franszyzą redukcyjną).
* szacowania wysokich kwantyli (nazywanych również **Wartością Narażoną na Ryzyko** w zarządzaniu ryzykiem finansowym).
* wyznaczania **prawdopodobnej szkody maksymalnej** (w skrócie PML), która ma być używana jako robocza górna granica wielkości szkody w obliczaniu miar ryzyka.

Gdy głównym przedmiotem zainteresowania jest ogon rozkładu ciężkości szkód, kluczowe jest posiadanie dobrego modelu dla największych szkód. Rozkłady, które zapewniają dobre ogólne dopasowanie, takie jak rozkłady z rodziny wykładniczej (ED), mogą być szczególnie nieodpowiednie do modelowania ogonów. Teoria Wartości Ekstremalnych (EVT) koncentruje się właśnie na ogonach, opierając się na solidnych podstawach teoretycznych. Zasada leżąca u podstaw EVT polega na przeprowadzeniu analizy w oparciu o tę część próby, która niesie informację o zachowaniu ekstremalnym, czyli wyłącznie na największych wartościach z próby. W tym rozdziale przypomniano podstawy EVT, przydatne w zastosowaniach aktuarialnych.

## 9.2 Podstawy EVT

### 9.2.1 Średnie z próby i maksima

Rozważając ciąg niezależnych zmiennych losowych o jednakowym rozkładzie (powiedzmy, wysokości szkód lub pozostałe długości życia) $Y_1, Y_2, Y_3, \dots$, większość klasycznych wyników z rachunku prawdopodobieństwa i statystyki, które są istotne dla ubezpieczeń, opiera się na sumach $S_n = \sum_{i=1}^n Y_i$. Przywołajmy na przykład prawo wielkich liczb i centralne twierdzenie graniczne.

Centralne twierdzenie graniczne określa rzeczywiste ciągi $a_n$ i $b_n$ takie, że znormalizowane sumy częściowe $\frac{S_n - a_n}{b_n}$ zbiegają do standardowego rozkładu normalnego, gdy $n$ rośnie. W szczególności, oznaczając $\mu = E[Y_1]$ i $\sigma^2 = \text{Var}[Y_1] < \infty$, wiemy, że tożsamość

$$
\lim_{n \to \infty} P\left[\frac{\sum_{i=1}^n Y_i - a_n}{b_n} \le x\right] = \Phi(x)
$$

zachodzi dla $a_n = n\mu$ i $b_n = \sqrt{n}\sigma$.
Inną interesującą, choć mniej standardową statystyką dla aktuariusza jest

$$
M_n = \max\{Y_1, \dots, Y_n\}
$$

maksimum z $n$ zmiennych losowych $Y_1, \dots, Y_n$. EVT odnosi się do następującego pytania: jak zachowuje się $M_n$ w dużych próbach (tj. gdy $n$ dąży do nieskończoności)? Oczywiście, bez dalszych ograniczeń, $M_n$ dąży do $\omega$, gdzie $\omega$ jest prawym krańcem nośnika $Y_1$, precyzyjniej zdefiniowanym jako

$$
\omega = \sup\{y \in (-\infty, \infty) | F(y) < 1\}, \text{ być może nieskończone,}
$$

gdzie $F$ oznacza dystrybuantę zmiennych losowych $Y_1, Y_2, Y_3, \dots$ objętych badaniem. Wiemy, że

$$
P[M_n \le y] = P[Y_i \le y \text{ dla wszystkich } i=1, \dots, n] = (F(y))^n.
$$

W praktyce jednak dokładny rozkład $M_n$ ma niewielkie znaczenie dla wszystkich $y < \omega$, ponieważ

$$
\lim_{n \to \infty} P[M_n \le y] = \lim_{n \to \infty} (F(y))^n = 0.
$$

Jeśli $\omega < \infty$, to dla wszystkich $y \ge \omega$

$$
\lim_{n \to \infty} P[M_n \le y] = \lim_{n \to \infty} (F(y))^n = 1.
$$

Co więcej, mały błąd oszacowania $F$ może mieć dramatyczne konsekwencje dla estymacji wysokich kwantyli $M_n$.

### 9.2.2 Rozkład skrajnych wartości

Gdy $M_n$ jest odpowiednio wycentrowane i znormalizowane, może jednak zbiegać do pewnego konkretnego rozkładu granicznego (jednego z trzech różnych typów, w zależności od grubości ogonów $F$). Wraz z stałymi normalizującymi, ten rozkład graniczny określa asymptotyczne zachowanie maksimum z próby $M_n$. Jeśli istnieją ciągi liczb rzeczywistych $c_n > 0$ i $d_n \in (-\infty, \infty)$ takie, że znormalizowana sekwencja $(M_n - d_n)/c_n$ zbiega w rozkładzie, tj.

$$
\lim_{n \to \infty} P\left[\frac{M_n - d_n}{c_n} \le y\right] = \lim_{n \to \infty} (F(c_n y + d_n))^n = H(y) \quad (9.1)
$$

dla wszystkich punktów ciągłości $H$, to $H$ jest uogólnionym rozkładem wartości ekstremalnych. Zauważ, że (9.1) jest odpowiednikiem centralnego twierdzenia granicznego dla sum $S_n$.
Uogólniona funkcja rozkładu wartości ekstremalnych (GEV) z indeksem ogona $\xi$ pojawiającym się jako granica w (9.1) ma postać

$$
H_\xi(y) = 
\begin{cases} 
\exp\left(-(1+\xi y)_+^{-1/\xi}\right) & \text{jeśli } \xi \ne 0, \\
\exp(-\exp(-y)) & \text{jeśli } \xi = 0, 
\end{cases}
$$

gdzie $z_+ = \max\{z, 0\}$ jest dodatnią częścią $z$. Nośnik $H_\xi$ jest

$$
\begin{cases} 
(-1/\xi, \infty) & \text{jeśli } \xi > 0 \\
(-\infty, -1/\xi) & \text{jeśli } \xi < 0 \\
(-\infty, \infty) & \text{jeśli } \xi = 0. 
\end{cases}
$$

Tutaj parametr $\xi$ kontrolujący prawy ogon rozkładu nazywany jest indeksem ogona lub indeksem wartości ekstremalnej. Trzy klasyczne rozkłady wartości ekstremalnych są szczególnymi przypadkami rodziny GEV:

* jeśli $\xi > 0$, mamy rozkład Frecheta,
* jeśli $\xi < 0$, mamy rozkład Weibulla, a
* jeśli $\xi = 0$, mamy rozkład Gumbela.

Rozkład GEV wydaje się być jedynym niezdegenerowanym rozkładem granicznym dla odpowiednio znormalizowanych maksimów z próby, co formalnie stwierdzono w następnej sekcji.

### 9.2.3 Twierdzenie Fishera-Tippetta

Jeśli odpowiednio znormalizowane maksima z próby $M_n$ zbiegają w rozkładzie do niezdegenerowanego rozkładu granicznego $H$ w (9.1), mówi się, że funkcja rozkładu $F$ należy do dziedziny przyciągania rozkładu GEV $H$. Klasa rozkładów, dla których zachowuje się takie asymptotyczne zachowanie, jest duża: obejmuje wszystkie powszechnie spotykane rozkłady ciągłe. Wyniki szeroko omówione do tej pory można podsumować w następującym twierdzeniu.

**Twierdzenie 9.2.1 (Twierdzenie Fishera-Tippetta)** Jeśli istnieją ciągi stałych rzeczywistych $c_n$ i $d_n$ takie, że (9.1) zachodzi dla jakiegoś niezdegenerowanego rozkładu $H$ (tj. nieskoncentrowanego w jednym punkcie), to $H$ musi być rozkładem GEV, tj.

$$
H = H_\xi \text{ dla pewnego } \xi.
$$

Parametr $\xi$ jest znany jako indeks wartości ekstremalnej (lub indeks Pareto, lub indeks ogona).
Rozkład Pareto jest typowym przykładem elementu należącego do klasy Frecheta, jak pokazano w następnym przykładzie.

**Przykład 9.2.2 ($\mathcal{Par}(\alpha, \tau) \Rightarrow \xi > 0$)** Załóżmy, że $Y_1, Y_2, Y_3, \dots$ są niezależne i wszystkie podlegają rozkładowi $\mathcal{Par}(\alpha, \tau)$, z dystrybuantą

$$
F(y) = 1 - \left(\frac{\tau}{\tau+y}\right)^\alpha, \quad \alpha, \tau > 0, \quad y \ge 0.
$$

Biorąc pod uwagę stałe normalizujące

$$
c_n = \frac{\tau}{n^{1/\alpha}} \quad \text{i} \quad d_n = \tau n^{1/\alpha} - \tau
$$

otrzymujemy dla

$$
c_n y + d_n \ge 0 \iff 1 + \frac{y}{n^{1/\alpha}} \ge n^{-1/\alpha}
$$

że

$$
\begin{align*}
\left(F(c_n y + d_n)\right)^n &= \left(1 - \left(\frac{\tau}{\tau + y\frac{\tau n^{1/\alpha}}{\alpha} + \tau n^{1/\alpha} - \theta}\right)^\alpha\right)^n \\
&= \left(1 - \frac{1}{n}\left(1 + \frac{y}{\alpha}\right)^{-\alpha}\right)^n \\
&\to \exp\left(-\left(1 + \frac{y}{\alpha}\right)^{-\alpha}\right) = H_{1/\alpha}(y) \text{ as } n \to \infty.
\end{align*}
$$

Zatem indeks wartości ekstremalnej rozkładu $\mathcal{Par}(\alpha, \tau)$ wynosi $\xi = 1/\alpha$. Zauważ, że ograniczenie $1 + \frac{y}{n^{1/\alpha}} \ge n^{-1/\alpha}$ staje się $1 + \frac{y}{\alpha} > 0$ w granicy, więc dziedzina jest dobrze dopasowana do postaci $(-1/\xi, \infty)$, jak zapowiedziano. To pokazuje, że rozkład $\mathcal{Par}(\alpha, \tau)$ należy do dziedziny przyciągania rozkładu Frecheta z indeksem wartości ekstremalnej $\xi = 1/\alpha$, który jest dodatni.

Oczywiście, klasa Frecheta ($\xi > 0$) nie ogranicza się do rozkładu Pareto. Można ją opisać w szerokim ujęciu w następujący sposób. Twierdzenie Fishera-Tippetta zachodzi z $H_\xi, \xi > 0$, wtedy i tylko wtedy, gdy reprezentacja

$$
1 - F(y) = y^{-1/\xi} \ell(y) \quad (9.2)
$$

zachodzi dla pewnej wolno zmieniającej się funkcji $\ell(\cdot)$, takiej że

$$
\lim_{y \to \infty} \frac{\ell(ty)}{\ell(y)} = 1 \text{ dla wszystkich } t > 0.
$$

Innymi słowy, zasadniczo oznacza to, że jeśli $1-F$ zanika jak funkcja potęgowa, to rozkład należy do dziedziny przyciągania rozkładu Frecheta. Wzór (9.2) jest często przyjmowany jako definicja rozkładów o grubych ogonach. Oprócz rozkładu Pareto, którego ogony maleją wielomianowo, przykłady rozkładów należących do klasy Frecheta obejmują rozkłady Burr, Log-Gamma, Cauchy'ego i Studenta $t$. Nie wszystkie momenty są skończone.
Stosowność rozkładów o grubych ogonach w klasie Frecheta do modelowania ryzyka katastroficznego przejawia się w relacji asymptotycznej

$$
\lim_{t \to \infty} \frac{P[M_n > t]}{P[S_n > t]} = 1.
$$

Ta relacja opisuje sytuację, w której suma $S_n$ roszczeń staje się duża wtedy i tylko wtedy, gdy maksymalne roszczenie $M_n$ staje się duże. Rozkłady spełniające ten warunek są zwykle określane w literaturze aktuarialnej jako sub-wykładnicze.
Rozkłady o grubych ogonach są najczęściej spotykane w zastosowaniach ubezpieczeń majątkowych. Rozkłady o lżejszych ogonach (zazwyczaj z wykładniczym spadkiem w ogonach) należą do klasy Gumbela, jak pokazano w następnym przykładzie.

**Przykład 9.2.3 ($\mathcal{E}xp(\tau) \Rightarrow \xi = 0$)** Załóżmy, że $Y_1, Y_2, \dots$ są niezależne i wszystkie podlegają rozkładowi $\mathcal{E}xp(\tau)$, z dystrybuantą

$$
F(y) = 1 - \exp(-\tau y), \quad \tau > 0, \quad y \ge 0.
$$

Z $c_n = \frac{1}{\tau}$ i $d_n = \frac{\ln n}{\tau}$, otrzymujemy dla

$$
c_n y + d_n \ge 0 \iff y \ge -\ln n
$$

że

$$
\begin{aligned}
(F(c_n y + d_n))^n &= \left(1 - \exp\left(-\tau\left(\frac{y}{\tau} + \frac{\ln n}{\tau}\right)\right)\right)^n \\
&= \left(1 - \frac{1}{n}\exp(-y)\right)^n \\
&\to \exp(-\exp(-y)) = H_0(y), \quad y \in (-\infty, \infty) \text{ gdy } n \to \infty.
\end{aligned}
$$

Zauważ, że ograniczenie $y \ge -\ln n$ nie jest już wiążące w granicy, więc dziedzina rozszerza się na całą prostą rzeczywistą $(-\infty, \infty)$. Zatem rozkład $\mathcal{E}xp(\tau)$ należy do dziedziny przyciągania rozkładu Gumbela z zerowym indeksem wartości ekstremalnej.
Charakteryzacja rozkładów Gumbela $\xi = 0$ jest bardziej skomplikowana. Z grubsza mówiąc, ona zawiera rozkłady, których ogony zanikają wykładniczo do 0 (rozkłady o lekkich ogonach). Wszystkie momenty są skończone. Przykłady obejmują rozkłady Normalny, LogNormalny i Gamma (w szczególności ujemny rozkład wykładniczy).
Klasa Weibulla ($\xi < 0$) wydaje się być szczególnie użyteczna w ubezpieczeniach na życie, ponieważ jej elementy mają ograniczony nośnik. To zachowanie będzie badane w następnych sekcjach poświęconych modelowaniu pozostałych długości życia w starszym wieku.

### 9.2.4 Ekstremalne długości życia

Przeformułujmy teraz centralny wynik EVT w kontekście ubezpieczeń na życie, dla modelowania śmiertelności w najstarszych grupach wiekowych. Odpowiedzi $Y$ zazwyczaj reprezentują długości życia w kontekście ubezpieczeń na życie i są w związku z tym oznaczane jako $T$. Rozważmy ciąg niezależnych indywidualnych długości życia $T_1, T_2, T_3, \dots$ z wspólną dystrybuantą

$$
F(x) = {}_xq_0 = P[T_i \le x], \quad x \ge 0,
$$

dla $i=1, \dots, n$, spełniającą $F(0) = 0$. Tutaj $T_i$ reprezentuje całkowitą długość życia, od urodzenia do śmierci. W wielu zastosowaniach ubezpieczeniowych może również reprezentować pozostałą długość życia po osiągnięciu danego wieku $\alpha$, ze wspólną dystrybuantą $F_\alpha(x) = {}_xq_\alpha$. W słowach, $M_n$ reprezentuje w tym ujęciu wiek najstarszej osoby zmarłej w jednorodnej grupie $n$ osób podlegających tej samej tablicy trwania życia $x \mapsto {}_xq_0$. EVT bada asymptotyczne zachowanie $M_n$, gdy $n$ dąży do nieskończoności, pod warunkiem spełnienia pewnych warunków technicznych dotyczących $x \mapsto {}_xq_0$. Oczywiście, bez dalszych ograniczeń, $M_n$ zbliża się do górnego krańca nośnika

$$
\omega = \sup\{x \ge 0 | {}_xq_0 < 1\}, \text{ być może nieskończonego.}
$$

W ubezpieczeniach na życie $\omega$ jest określane jako ostateczny wiek w tablicy trwania życia. Można to łatwo zauważyć z

$$
P[M_n \le x] = ({}_xq_0)^n \to 
\begin{cases} 
0 & \text{jeśli } x < \omega \\
1 & \text{jeśli } x \ge \omega 
\end{cases} 
\text{ gdy } n \to \infty.
$$

Gdy $M_n$ jest odpowiednio wycentrowane i znormalizowane, może jednak zbiegać do pewnego konkretnego rozkładu granicznego, jak w (9.1). Gdy $\xi > 0$, mamy do czynienia z rozkładami o grubych ogonach, co jest sprzeczne z danymi empirycznymi dla ludzkich długości życia (w tym przypadku siły śmiertelności zmniejszałyby się wraz z wiekiem). Zatem przypadki $\xi=0$ i $\xi < 0$ są interesujące dla zastosowań w ubezpieczeniach na życie. Zauważ, że jeśli (9.1) zachodzi z $\xi < 0$, to $\omega < \infty$, więc ujemna wartość $\xi$ potwierdza istnienie skończonego ostatecznego wieku $\omega$.
Przypomnijmy, że

$$
\mu_x = \lim_{\Delta x \searrow 0} \frac{P[x < T \le x + \Delta x | T > x]}{\Delta x} = \frac{\frac{d}{dx}{}_xq_0}{{}_xp_0}
$$

jest siłą śmiertelności w wieku $x$. Określa ona ryzyko natychmiastowej śmierci dla osoby żyjącej w wieku $x$. Warunkiem wystarczającym, aby (9.1) zachodziło jest

$$
\lim_{x \to \omega} \frac{d}{dx}\left(\frac{1}{\mu_x}\right) = \xi. \quad (9.3)
$$

Intuicyjnie, $\frac{1}{\mu_x}$ można uznać za siłę oporu wobec śmiertelności, lub siłę witalności w wieku $x$. Opór wobec śmiertelności musi się stabilizować, gdy $\xi = 0$ lub staje się ostatecznie liniowy. Ujemna wartość $\xi$ wskazuje, że opór ostatecznie maleje w starszym wieku. Dla $\xi < 0$ mamy $\omega < \infty$ i warunek (9.3) implikuje

$$
\lim_{x \to \omega} (\omega - x)\mu_x = -\frac{1}{\xi}.
$$

## 9.3 Nadwyżka ponad próg i funkcja średniej nadwyżki

### 9.3.1 Rozkład nadwyżki

Biorąc pod uwagę ciąg niezależnych zmiennych losowych o identycznym rozkładzie $Y_1, Y_2, \dots$ oraz próg $u$, uważany za duży, aktuariusze są często zainteresowani nadwyżkami $Y_i - u$ ponad $u$ dla $Y_i > u$. Zauważmy, że tutaj ograniczamy się do tych elementów $Y_i$, dla których spełniony jest warunek $Y_i > u$, tzn. $Y_i$ osiąga próg $u$ będący przedmiotem rozważań.

Niech $F_u$ oznacza dystrybuantę nadwyżki losowej zmiennej $Y$ ponad próg $u$, pod warunkiem, że próg $u$ jest osiągnięty, to znaczy,

$$
\begin{align*}
F_u(y) &= P[Y - u \le y | Y > u] \\
&= \frac{F(y+u) - F(u)}{1 - F(u)} \\
&= 1 - \frac{\bar{F}(y+u)}{\bar{F}(u)}, \quad y \ge 0,
\end{align*}
$$

gdzie $\bar{F} = 1 - F$ jest funkcją nadwyżki (lub komplementarną do dystrybuanty, lub funkcją przeżycia) powiązaną z (skumulowaną) funkcją dystrybuanty $F$.

### 9.3.2 Funkcja średniej nadwyżki

Pod warunkiem, że $E[Y] < \infty$, funkcja średniej nadwyżki $e(\cdot)$ powiązana z $Y$ jest zdefiniowana jako

$$
e(u) = E[Y - u | Y > u] = \int_0^\infty \bar{F}_u(t)dt
$$

gdzie $\bar{F}_u = 1 - F_u$. Zatem $e(u)$ reprezentuje oczekiwaną nadwyżkę ponad próg $u$, pod warunkiem, że próg ten jest osiągnięty. Łatwo jest udowodnić, że jeśli $Y$ ma rozkład wykładniczy, jego funkcja średniej nadwyżki jest stała, to znaczy

$$
Y \sim \mathcal{E}xp(\tau) \iff e(u) = E[Y] = \frac{1}{\tau}.
$$

Jest to bezpośrednia konsekwencja właściwości braku pamięci charakteryzującej Ujemny Rozkład Wykładniczy. W konsekwencji, wykres $e(u)$ w funkcji $u$ będzie linią poziomą. Rozkłady o cienkich ogonach będą wykazywać trend spadkowy. Z drugiej strony, trend wzrostowy będzie wskazówką zachowania gruboogonowego. Trend spadkowy jest typowy dla rozkładów o skończonych nośnikach.

Zazwyczaj funkcja średniej nadwyżki $e(\cdot)$ nie jest znana, ale można ją łatwo oszacować na podstawie próby losowej, a jej empiryczny estymator $\hat{e}_n$ może być wykreślony. Dokładniej, funkcja średniej nadwyżki z próby losowej $\{y_1, y_2, \dots, y_n\}$ jest określona przez

$$
\hat{e}_n(u) = \frac{\sum_{i=1}^n Y_i I[Y_i > u]}{\#\{i | Y_i > u\}} - u = \frac{\sum_{i=1}^n (Y_i - u)I[Y_i > u]}{\#\{i | Y_i > u\}}
$$

gdzie $\#\{i | y_i > u\}$ oznacza liczbę obserwacji przekraczających $u$, a $I[\cdot]$ jest funkcją wskaźnikową. Innymi słowy, $e(u)$ jest szacowane przez sumę wszystkich nadwyżek ponad próg $u$ podzieloną przez liczbę punktów danych przekraczających próg $u$. Zazwyczaj funkcja średniej nadwyżki jest oceniana w obserwacjach z próby. Dokładniej, oznaczmy obserwacje z próby w porządku rosnącym (statystyki pozycyjne) jako

$$
y_{(1)} \le y_{(2)} \le \dots \le y_{(n)}.
$$

Mamy wtedy

$$
\hat{e}_n(y_{(k)}) = \frac{1}{n-k} \sum_{j=1}^{n-k} (y_{(k+j)} - y_{(k)}).
$$

### 9.3.3 Ilustracja w ubezpieczeniach majątkowych

Rozważmy wysokości szkód w belgijskim portfelu ubezpieczeń OC komunikacyjnych rozważanym w Rozdz. 6. Informacje o kwotach szkód w bazie danych zostały zarejestrowane 6 miesięcy po zakończeniu okresu obserwacji. Stąd większość "małych" szkód została rozliczona, a ich ostateczny koszt jest znany. Jednak dla dużych szkód pracowaliśmy tutaj ze szkodami poniesionymi (wypłacone odszkodowania plus rezerwy). Wszystkie koszty są wyrażone we frankach belgijskich, walucie obowiązującej w Belgii pod koniec lat 90. (jedno euro to około czterdzieści franków belgijskich).

Statystyki opisowe dla kosztów szkód są przedstawione w Tabeli 9.1. Mamy do dyspozycji 18 042 obserwowane indywidualne koszty szkód, w zakresie od 1 do ponad 80 000 000 franków belgijskich, ze średnią 72 330,7 franków belgijskich. Widzimy w Tabeli 9.1, że 25% zarejestrowanych kosztów szkód jest mniejszych niż 5 850 franków belgijskich, a 90% z nich jest mniejszych niż 121 902 franków belgijskich. Pozostałe 10% obserwowanych wysokości szkód rozciąga się zatem do ponad 80 milionów, co sugeruje, że zbiór danych zawiera bardzo duże kwoty w ogonie rozkładu.

Ponieważ dane są znacznie skośne w prawo, zdecydowaliśmy się na reprezentację logarytmiczną, aby przedstawić wysokości szkód. Histogram wyświetlony na Rys. 9.1 pokazuje, że prawa część rozkładu ujawnia obecność bardzo dużych strat nawet w skali logarytmicznej. Zachowanie ogona danych jest dalej badane za pomocą wykresu $\hat{e}_n$. Empiryczna funkcja średniej nadwyżki wyświetlona na Rys. 9.2 pokazuje gruboogonowe zachowanie wysokości szkód, które materializuje się w widocznym trendzie wzrostowym. Ten wykres został uzyskany za pomocą funkcji `mrlplot` z pakietu R `POT`.

Wiemy, że funkcja średniej nadwyżki jest stała, jeśli wysokości szkód podlegają Ujemnemu Rozkładowi Wykładniczemu. Rozkłady o cienkich ogonach wykazują trend spadkowy, podczas gdy trend wzrostowy wskazuje na zachowanie gruboogonowe. Rosnący trend $\hat{e}_n$, który jest wyraźnie widoczny na Rys. 9.2, wspiera zachowanie gruboogonowe. Dlatego oczekuje się dodatniej wartości indeksu ogona $\xi$.

Ujemny Rozkład Wykładniczy służy również jako punkt odniesienia na wykresie kwantylowo-kwantylowym (QQ-plot) wykładniczym. Interpretacja takiego wykresu jest prosta. Jeśli dane są zgodne z Ujemnym Rozkładem Wykładniczym, punkty powinny leżeć w przybliżeniu wzdłuż linii prostej. Wypukłe odchylenie od kształtu liniowego wskazuje na rozkład o grubszym ogonie w tym sensie, że kwantyle empiryczne rosną szybciej niż teoretyczne.

---
**Tabela 9.1** Statystyki opisowe kosztów szkód (tylko wartości ściśle dodatnie) dla belgijskiego portfela ubezpieczeń OC komunikacyjnych obserwowanego pod koniec lat 90., wszystkie wartości kosztów wyrażone we frankach belgijskich.

| | |
| :--- | :--- |
| **Liczba obserwacji** | 18 042 |
| **Minimum** | 1 |
| **Maksimum** | 80 258 970 |
| **Średnia** | 72 331 |
| **Odchylenie standardowe** | 710 990,5 |
| **25. percentyl** | 5 850 |
| **Mediana** | 23 242 |
| **75. percentyl** | 58 465 |
| **90. percentyl** | 121 902 |
| **95. percentyl** | 170 530,9 |
| **99. percentyl** | 794 900,9 |

Przeciwnie, wklęsłość wskazuje na rozkład o cieńszym ogonie. Wypukły wzór wykresu kwantylowo-kwantylowego wykładniczego przedstawionego na Rys. 9.3 potwierdza gruboogonowe zachowanie rozkładu wysokości szkód.

### 9.3.4 Ilustracja w ubezpieczeniach na życie

Techniki EVT są również przydatne do badania samego końca tablic trwania życia. Ubezpieczyciele muszą być w stanie oszacować śmiertelność w najstarszych grupach wiekowych, aby wycenić renty dożywotnie i odwrócone hipoteki, na przykład. Do dopasowania statystyk śmiertelności często używano modeli parametrycznych, w tym:

- Model Gompertza:
  $$ \mu_x = a \exp(bx) \text{ z } a, b > 0; $$
- Model Makehama:
  $$ \mu_x = c + a \exp(bx) \text{ z } a, b, c > 0. $$

Wykładniczy wzrost siły śmiertelności w modelach Gompertza i Makehama jest ogólnie odpowiedni dla dorosłych i osób wczesnej starości, ale śmiertelność ma tendencję do spowalniania w starszych grupach wiekowych, zazwyczaj około 85. roku życia, więc modele te zwykle zawodzą w opisie końca życia (przeszacowując śmiertelność w najstarszych grupach wiekowych, co wydaje się być problematyczne na przykład dla rent dożywotnich). Zaproponowano alternatywne modele w celu uwzględnienia spowolnienia śmiertelności obserwowanego w starszym wieku w uprzemysłowionych krajach. Płaskowyż, który pojawia się w tych zaawansowanych grupach wiekowych, jest często przypisywany wcześniejszej selekcji bardziej odpornych osób w heterogenicznych kohortach. Ten empiryczny dowód wspiera model logistyczny uzyskany przez włączenie losowego współczynnika kruchości (frailty) o rozkładzie Gamma w celu uwzględnienia heterogeniczności w modelu Gompertza, jak wyjaśniono dalej.

Kruchość jest zdefiniowana jako nieujemna zmienna losowa, której wartość wyraża nieobserwowalne czynniki ryzyka wpływające na indywidualną śmiertelność. Podstawową ideą jest to, że osoby o wyższej kruchości umierają średnio wcześniej niż inne. Niech $Z$ oznacza kruchość odpowiadającą dożywotniej $T$. Biorąc pod uwagę $Z=z$, siła śmiertelności w multiplikatywnym modelu kruchości jest zdefiniowana jako
$$ \mu_x(z) = z \mu_x $$
gdzie $\mu_x$ reprezentuje siłę śmiertelności dla osoby ze standardowym profilem $Z=1; \mu_x$ jest uważana za standardową siłę śmiertelności. Jeśli $z < 1$, wtedy $\mu_x(z) < \mu_x$, co sugeruje, że osoba jest w dobrej kondycji; odwrotnie, jeśli $z > 1$. Teraz załóżmy, że $Z \sim \mathcal{Gam}(\delta, \theta)$. Odnosząc się do dorosłych, możemy założyć model Gompertza do opisu standardowej siły śmiertelności, tak że
$$ \mu_x(z) = z a \exp(bx). $$
Populacyjna siła śmiertelności jest wtedy dana przez
$$ \frac{a \delta \exp(bx)}{(\theta - \frac{a}{b}) + \frac{a}{b} \exp(bx)} = \frac{1}{\theta - \frac{a}{b}} \frac{a \delta \exp(bx)}{1 + \frac{a}{b\theta - a} \exp(bx)}. $$

---
**Tabela 9.2** Statystyki opisowe dla obserwowanego wieku w chwili śmierci powyżej 95 lat, kohorty urodzone w Belgii w latach 1886-1904.

| | Mężczyźni | Kobiety |
| :--- | :--- | :--- |
| **Liczba obserwacji** | 10 050 | 36 616 |
| **Średnia** | 97,35 | 97,75 |
| **Odchylenie standardowe** | 2,05 | 2,37 |
| **25. percentyl** | 95,77 | 95,91 |
| **Mediana** | 96,79 | 97,12 |
| **75. percentyl** | 98,36 | 98,99 |
| **Maksimum** | 111,47 | 112,58 |

Definiując $\frac{a \delta}{\theta - \frac{a}{b}} = a'$ i $\frac{a}{b\theta - a} = \delta'$, otrzymujemy model logistyczny

$$ \frac{a' \exp(bx)}{1 + \delta' \exp(bx)} $$

dla populacyjnej siły śmiertelności, który tworzy wcześniej wspomniany płaskowyż śmiertelności.

Gdy głównym celem jest śmiertelność w najstarszych grupach wiekowych, wnioskowanie prowadzone jest na podstawie próby, w której zakres danych jest bardzo mały. Co więcej, ekstrapolacja poza zakres danych jest często pożądaną procedurą. W takiej sytuacji niezbędne jest posiadanie dobrego modelu dla najdłuższych okresów życia. Zatem EVT wydaje się naturalnym kandydatem do analizy śmiertelności w najstarszych grupach wiekowych. Ponadto, EVT jest ściśle powiązane z granicznymi rozkładami resztkowymi życia i oferuje jednolite podejście do modelowania prawego ogona rozkładu czasu życia. Na przykład funkcja średniej nadwyżki odpowiada oczekiwanej pozostałej długości życia jako funkcji osiągniętego wieku.

Biorąc pod uwagę analizowane dane, mamy do dyspozycji zgony w każdym indywidualnym wieku dla kobiet i mężczyzn, którzy zmarli w wieku 95 lat lub więcej, a także listę osób, które przeżyły wiek 95 lat i zmarły na końcu okresu obserwacji. Jest 46 666 obserwacji dotyczących osób urodzonych w Belgii, które zmarły po 1981 roku. Baza danych zawiera 22% mężczyzn i 78% kobiet. Dla każdej osoby znamy dokładną datę urodzenia i śmierci.

Podstawowe statystyki opisowe zbiorów danych podano w Tabeli 9.2. Średni wiek śmierci dla kobiet jest większy niż dla mężczyzn, zgodnie z oczekiwaniami. Różnice są jednak raczej umiarkowane, w porównaniu do różnic w całkowitej długości życia od urodzenia, z wyjątkiem znacznie wyższej liczby kobiet osiągających wiek 95 lat w porównaniu z mężczyznami.

Dane kohortowe są przedstawione w Tabeli 9.3, oddzielnie dla każdej płci. Możemy tam odczytać początkową liczbę osób włączonych do analizy, dla każdej kohorty urodzonej w latach 1886 i 1904 (czyli liczbę $L_{95}$ osób w kalendarzowym roku $c \in \{1886, \dots, 1904\}$ wciąż żyjących w wieku 95 lat) oraz wiek śmierci $M_{L_{95}}$ ostatniej osoby, która przeżyła dla każdej kohorty. Widzimy, że $L_{95}$ wzrasta w miarę upływu czasu, zgodnie z oczekiwaniami połączonego efektu rosnącej populacji i malejącej śmiertelności w młodszych grupach wiekowych w czasie. Ponadto liczba osób

---
**Tabela 9.3** Początkowa wielkość dla każdej kohorty $L_{95}$, obserwowany najwyższy wiek śmierci $M_{L_{95}}$ oraz średni wiek śmierci $95 + \hat{e}(95)$ dla każdej kohorty urodzonej w roku kalendarzowym $c$ od 1886 do 1904.

| Kohorta | Mężczyźni | | | Kobiety | | |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **c** | **$L_{95}$** | **$M_{L_{95}}$** | **$95 + \hat{e}(95)$** | **$L_{95}$** | **$M_{L_{95}}$** | **$95 + \hat{e}(95)$** |
| 1886 | 359 | 108.17 | 97.35 | 1045 | 107.78 | 97.70 |
| 1887 | 418 | 105.13 | 97.41 | 1130 | 110.45 | 97.64 |
| 1888 | 412 | 106.33 | 97.20 | 1242 | 110.32 | 97.68 |
| 1889 | 462 | 105.58 | 97.53 | 1208 | 110.16 | 97.69 |
| 1890 | 425 | 107.70 | 97.50 | 1311 | 112.58 | 97.60 |
| 1891 | 428 | 105.81 | 97.35 | 1368 | 109.72 | 97.70 |
| 1892 | 444 | 105.44 | 97.29 | 1510 | 110.89 | 97.72 |
| 1893 | 492 | 110.29 | 97.46 | 1544 | 107.75 | 97.63 |
| 1894 | 498 | 106.19 | 97.22 | 1760 | 107.41 | 97.71 |
| 1895 | 526 | 106.62 | 97.24 | 1852 | 109.38 | 97.86 |
| 1896 | 545 | 106.27 | 97.43 | 1894 | 109.79 | 97.85 |
| 1897 | 568 | 106.43 | 97.33 | 2009 | 109.85 | 97.72 |
| 1898 | 572 | 105.74 | 97.26 | 2149 | 110.89 | 97.71 |
| 1899 | 630 | 106.88 | 97.35 | 2301 | 111.60 | 97.72 |
| 1900 | 576 | 111.47 | 97.27 | 2460 | 111.70 | 97.78 |
| 1901 | 635 | 103.77 | 97.23 | 2787 | 110.36 | 97.89 |
| 1902 | 632 | 106.79 | 97.42 | 2829 | 112.36 | 97.82 |
| 1903 | 669 | 104.71 | 97.46 | 2980 | 109.96 | 97.81 |
| 1904 | 759 | 106.15 | 97.32 | 3237 | 110.18 | 97.74 |
---
wchodzących do bazy danych jest większa dla kobiet w porównaniu z mężczyznami z powodu niższych wskaźników śmiertelności dla kobiet w porównaniu z mężczyznami we wszystkich grupach wiekowych. Maksima $M_{L_{95}}$ są stosunkowo stabilne w kohortach i są wyższe dla kobiet w porównaniu z mężczyznami. Zauważ, że dla każdej kohorty ostatnią osobą, która przeżyła, była kobieta. Najwyższy wiek w chwili śmierci obserwowany w Belgii dla tych kohort to 112,58 lat dla kobiet i 111,47 dla mężczyzn. Przypomnijmy, że Jeanne Calment zmarła w wieku 122 lat w 1997 roku. Po tym rekordzie tylko jedna osoba, Sarah Knauss, żyła 119 lat i zmarła w 1999 roku. Od tego czasu trzy kobiety żyły ponad 117 lat, a trzy inne wciąż żyją w wieku 116 lat. Biorąc pod uwagę ograniczony rozmiar populacji belgijskiej, maksima zawarte w bazie danych są zgodne z tymi światowymi rekordami.

Widzimy również z Tabeli 9.3, że nie ma widocznego trendu w średnim wieku w chwili śmierci, ani oczekiwanej długości życia $95 + \hat{e}(95)$ dla osób żyjących w wieku 95 lat. Pozostaje on nawet stabilny na poziomie około 97 lat zarówno dla kohort męskich, jak i żeńskich. Obserwacja, że $\hat{e}(95)$ nie poprawiła się w 19 kohortach urodzonych jest dość niezwykła. Wydaje się to sugerować, że nie ma potencjalnego zysku w ekstremalnej długowieczności przez pokolenia w tych zaawansowanych grupach wiekowych. Można to potwierdzić za pomocą formalnego testu (jak wykonano w Gbari i in. 2017a).

Ograniczmy teraz zbiór danych do wieku 98 lat i więcej. Powód jest następujący: statystyki śmiertelności rynkowej są dostępne w Belgijskim Banku Narodowym, działającym jako regulator ubezpieczeniowy, do wieku 98 lat (ostatnia kategoria jest otwarta i obejmuje wiek 99 lat i więcej). Ponieważ celem jest dostarczenie aktuariuszom ekstrapolacji rynkowych tablic trwania życia na starsze grupy wiekowe, naturalne jest skoncentrowanie się na grupach wiekowych nieobjętych rynkowymi tablicami trwania życia. Histogramy są wyświetlone na Rys. 9.4. Wykresy te zostały uzyskane z obserwowanych wieków w chwili śmierci $\{t_1, t_2, \dots, t_n\}$ z $t_i \ge 98$ zawartych w bazie danych. Widzimy, że te histogramy sugerują, że pozostałe okresy życia powyżej 98 roku życia mają malejącą funkcję gęstości prawdopodobieństwa, z pewnymi obserwacjami znajdującymi się daleko w ogonie rozkładów.

Empiryczna wersja oczekiwanej pozostałej długości życia $\hat{e}(x)$ jako funkcja osiągniętego wieku $x$ jest przedstawiona na Rys. 9.5. Wiemy, że jeśli pozostałe okresy życia podlegają Ujemnemu Rozkładowi Wykładniczemu, to funkcja średniej nadwyżki jest stała. W konsekwencji wykres $e(x)$ w funkcji wieku $x$ będzie linią poziomą. Rozkłady o cienkich ogonach będą wykazywać trend spadkowy. Przeciwnie, trend wzrostowy będzie wskazówką zachowania gruboogonowego. Malejący kształt $\hat{e}(x)$ widoczny na Rys. 9.5 zaprzecza zachowaniu Ujemnego Rozkładu Wykładniczego dla pozostałego czasu życia. Trend spadkowy jest wyraźnie widoczny na Rys. 9.5, wspierając zachowanie o cienkim ogonie. Dlatego oczekuje się ujemnej wartości indeksu ogona $\xi$, co skutkuje skończonym ostatecznym wiekiem $\omega$.

Porównanie z rozkładem Ujemnym Wykładniczym długości życia można przeprowadzić za pomocą wykresu kwantylowo-kwantylowego (QQ-plot) wykładniczego, jak wyjaśniono wcześniej przy omawianiu wysokości szkód w ubezpieczeniach komunikacyjnych. Jeśli dane są niezależną i identycznie rozłożoną próbą z rozkładu Ujemnego Wykładniczego, powinien być widoczny trend liniowy. Wypukłe odchylenie od tego kształtu wskazuje na rozkład o grubszym ogonie w tym sensie, że kwantyle empiryczne rosną szybciej niż teoretyczne. Przeciwnie, wklęsłość wskazuje na rozkład o cieńszym ogonie. Wklęsły wzór wykresu kwantylowo-kwantylowego wykładniczego widoczny na Rys. 9.6 potwierdza zachowanie o cienkim ogonie rozkładu czasu życia dla obu płci.














## 9.5 Podejście POT (Peak Over Threshold)

### 9.5.1 Zasada

Tradycyjne podejście do EVT opiera się na granicznych rozkładach wartości ekstremalnych. W tym ujęciu model dla ekstremalnych strat bazuje na możliwej parametrycznej formie granicznego rozkładu maksimów. Bardziej elastyczny model jest znany jako metoda „Peak Over Threshold” (w skrócie POT). To podejście jest alternatywą dla analizy maksimów w badaniu zachowań ekstremalnych. Zasadniczo, POT analizuje serię nadwyżek ponad wysoki próg $u$. 

Okazuje się, że Uogólniony Rozkład Pareto stanowi dla aktuariusza przybliżenie rozkładu nadwyżek *Fu* ponad dostatecznie wysokie progi. 

**Twierdzenie 9.5.1 (Twierdzenie Pickandsa-Balkemy-de Haana)**
*Twierdzenie Fishera-Tippetta zachodzi z $H_\xi$ wtedy i tylko wtedy, gdy możemy znaleźć dodatnią funkcję $\tau(u)$ taką, że*

$$ \lim_{u \to \omega} \sup_{0 \le y \le \omega - u} |F_u(y) - G_{\xi, \tau(u)}(y)| = 0. $$

Twierdzenie 9.5.1 wskazuje, że rozkład $GPar(\xi, \tau)$ stanowi dobre przybliżenie rozkładu nadwyżek ponad dostatecznie wysokie progi $u$. W praktyce, dla pewnej funkcji $\tau(u)$ i pewnego indeksu Pareto $\xi$ zależnego od $F$, możemy użyć przybliżenia

$$ \bar{F}_u(y) \approx 1 - G_{\xi; \tau(u)}(y), \quad y \ge 0, $$

pod warunkiem, że $u$ jest wystarczająco duże. Otrzymujemy wtedy użyteczne przybliżenie

$$ \bar{F}(u + z) \approx \bar{F}(u) [1 - G_{\hat{\xi}; \hat{\tau}}(z)] $$

które wydaje się być dokładne dla wystarczająco dużego progu $u$ i dowolnego $z \ge 0$. Wzory na ogon rozkładu oparte na Uogólnionym Rozkładzie Pareto, takie jak te wyprowadzone w następnej sekcji, okazują się więc użyteczne, gdy aktuariusz ma do czynienia z ekstremalnymi wartościami zmiennej odpowiedzi. 

---
### 9.5.2 Wzory na Ogon Rozkładu

Jeśli ogon rozkładu Y jest Uogólnionym Rozkładem Pareto, aktuariusz jest w stanie wyprowadzić szereg użytecznych tożsamości, jak pokazano poniżej. Jeśli $F_u(y) = G_{\xi, \tau}(y)$ dla $0 \le y < \omega − u$, jak sugeruje twierdzenie Pickandsa-Balkemy-de Haana, to dla $y \ge u$,

$$ P[Y > y] = P[Y > u]P[Y > y|Y > u] \quad \text{dla} \quad x \ge u $$

$$ = \bar{F}(u)P[Y - u > y - u|Y > u] $$

$$ = \bar{F}(u)\bar{F}_u(y - u) $$

$$ = \bar{F}(u) \left( 1 + \xi \frac{y - u}{\tau} \right)^{-1/\xi}. $$

Ten wzór jest użyteczny do obliczania wysokich kwantyli zmiennej odpowiedzi. Funkcja kwantylowa rozkładu $GPar(\xi, \tau)$ jest dana wzorem

$$ G^{-1}_{\xi;\tau}(p) = \frac{\tau}{\xi} \left[ (1 - p)^{-\xi} - 1 \right], \quad 0 < p < 1. $$

Teraz, dla $p \ge F(u)$, kwantyl $Y$ na poziomie prawdopodobieństwa $p$ (często nazywany Wartością Narażoną na Ryzyko w zarządzaniu ryzykiem) otrzymuje się jako rozwiązanie $z$ równania

$$ 1 - p = \bar{F}(u) \left( 1 + \xi \frac{z - u}{\tau} \right)^{-1/\xi}. $$

To daje

$$ F^{-1}_Y(p) = u + \frac{\tau}{\xi} \left[ \left( \frac{1 - p}{\bar{F}(u)} \right)^{-\xi} - 1 \right]. $$

Średnia wartość odpowiedzi, gdy Wartość Narażona na Ryzyko została przekroczona, jest również bardzo użytecznym wskaźnikiem w zarządzaniu ryzykiem, do kwantyfikacji strat w niekorzystnych scenariuszach. Wskaźnik ten jest znany jako warunkowa wartość oczekiwana ogona. Jeśli $\xi < 1$, to dla $p \ge F(u)$, warunkowa wartość oczekiwana ogona jest dana wzorem

$$ E[Y | Y > F^{-1}_Y(p)] = F^{-1}_Y(p) + E[Y - F^{-1}_Y(p) | Y > F^{-1}_Y(p)] $$

$$ = F^{-1}_Y(p) + E[Y - u - (F^{-1}_Y(p) - u) | Y - u > F^{-1}_Y(p) - u] $$

$$ = F^{-1}_Y(p) + \frac{\tau + \xi(F^{-1}_Y(p) - u)}{1 - \xi} $$

$$ = \frac{F^{-1}_Y(p)}{1 - \xi} + \frac{\tau - \xi u}{1 - \xi} $$

gdzie $F^{-1}_Y(p)$ zostało wyprowadzone we wcześniejszym wzorze. 

---
### 9.5.3 Estymatory Ogona

Twierdzenie Pickandsa-Balkemy-de Haana pokazuje, że (pod warunkiem, że $u$ jest wystarczająco duże) potencjalnym estymatorem dla rozkładu nadwyżki szkody $F_u(x)$ jest $G_{\hat{\xi};\hat{\tau}}(x)$. Wybór odpowiedniego progu $u$ zostanie omówiony w następnych sekcjach. Zatem, $G_{\hat{\xi};\hat{\tau}}(x)$ przybliża warunkowy rozkład strat, pod warunkiem, że przekraczają one próg $u$. Estymatory kwantyli wyprowadzone z tej krzywej są warunkowymi estymatorami kwantyli, które wskazują na skalę strat, jakich można doświadczyć, gdyby próg $u$ został przekroczony. Gdy interesują nas estymatory bezwarunkowych kwantyli, konieczne jest powiązanie bezwarunkowej dystrybuanty $F$ z $G_{\hat{\xi};\hat{\tau}}$ poprzez $F_u$. 

Oznaczmy liczbę szkód powyżej progu $u$ jako

$$ N_u = \sum_{i=1}^n I[Y_i > u] \sim \text{Bin}(n, \bar{F}(u)). $$

Pod warunkiem, że mamy wystarczająco dużą próbę, możemy dokładnie oszacować $\bar{F}(u)$ za pomocą jego empirycznego odpowiednika $N_u/n$. Dla $x > u$, $\bar{F}(x) = \bar{F}(u)\bar{F}_u(x − u)$, więc możemy oszacować $\bar{F}(x)$ za pomocą

$$ \hat{\bar{F}}(x) = \frac{N_u}{n} \left( 1 + \hat{\xi} \frac{x - u}{\hat{\tau}} \right)^{-1/\hat{\xi}}. $$

Wysokie kwantyle zawierają użyteczne informacje dla ubezpieczycieli na temat rozkładu kwot szkód. Zazwyczaj kwantyle można oszacować za pomocą ich empirycznych odpowiedników, ale gdy interesują nas bardzo wysokie kwantyle, to podejście przestaje być ważne, ponieważ estymacja oparta na niewielkiej liczbie dużych obserwacji byłaby bardzo nieprecyzyjna. Twierdzenie Pickandsa-Balkemy-de Haana sugeruje następujące estymatory. Dla $p \ge F(u)$, kwantyl na poziomie prawdopodobieństwa $p$ można oszacować z

$$ \hat{F}^{-1}(p) = u + \frac{\hat{\tau}}{\hat{\xi}} \left[ \left( \frac{n(1 - p)}{N_u} \right)^{-\hat{\xi}} - 1 \right]. $$

Jeśli $\xi < 1$, to warunkową wartość oczekiwaną ogona można oszacować z

$$ \hat{E}[Y | Y > F^{-1}(p)] = \frac{\hat{F}^{-1}(p)}{1 - \hat{\xi}} + \frac{\hat{\tau} - \hat{\xi}u}{1 - \hat{\xi}} $$

gdzie wstawiamy wyżej wymienione wyrażenie dla $\hat{F}^{-1}(p)$. 

---
### 9.5.4 Zastosowania do Pozostałego Czasu Życia

Analiza pozostałego czasu życia w starszym wieku jest zgodna z metodą POT, gdzie próg $u$ odpowiada pewnemu zaawansowanemu wiekowi $x$. Przenosząc to na grunt ubezpieczeń na życie, pozostały czas życia $T − x$ w wieku $x$, pod warunkiem $T > x$, ma rozkład zgodny z

$$ s \to {}_s q_x = P[T - x \le s | T > x]. $$

Może się zdarzyć, że dla wysokich osiągniętych wieków $x$, ten warunkowy rozkład prawdopodobieństwa stabilizuje się po normalizacji, tzn. istnieje dodatnia funkcja $\tau(\cdot)$ taka, że

$$ \lim_{x \to \omega} P\left[\frac{T - x}{\tau(x)} > s \bigg| T > x\right] = 1 - G(s), \quad s > 0, $$

gdzie $G$ jest niezdegenerowaną dystrybuantą. Tylko ograniczona klasa dystrybuant jest dopuszczalna w (9.4), mianowicie Uogólnione Rozkłady Pareto $G_{\xi,\tau}$. Gdy $\xi = 0$, pozostały czas życia w starszym wieku staje się ostatecznie ujemnie wykładniczy, tak że siły wymierania stabilizują się. 

Dla pewnej odpowiedniej funkcji $\tau(\cdot)$, przybliżenie

$$ {}_s q_x \approx G_{\xi; \tau(x)}(s) \quad \text{dla } \quad s \ge 0 $$

zachodzi dla wystarczająco dużego $x$. Przybliżenie (9.5) jest uzasadnione twierdzeniem Pickandsa-Balkemy-de Haana. W świetle (9.5) pozostały czas życia w wieku $x$ można traktować jako losową próbę z Uogólnionego Rozkładu Pareto, pod warunkiem że $x$ jest wystarczająco duże. 

Jeśli $\xi < 0$, tak że $\omega < \infty$, to odpowiednia transformacja indeksu wartości ekstremalnej $\xi$ ma intuicyjną interpretację. Przypomnijmy, że funkcja średniej nadwyżki

$$ e(x) = E[T - x | T > x] $$

pokrywa się z oczekiwanym dalszym trwaniem życia w wieku $x$, oznaczanym jako $ex$. Można wykazać, że dla $\xi < 0$, (9.1) jest równoważne z

$$ \lim_{x \to \omega} E\left[\frac{T - x}{\omega - x} \bigg| T > x\right] = \lim_{x \to \omega} \frac{e(x)}{\omega - x} = -\frac{\xi}{1 - \xi} = \alpha $$

Parametr $\alpha = \alpha(\xi)$ jest określany jako parametr perseweracji. Intuicyjna interpretacja parametru perseweracji jest następująca. Rozważmy osobę, która jest wciąż żywa w pewnym zaawansowanym wieku $x$. Stosunek $(T - x) / (\omega - x)$ reprezentuje procent faktycznego pozostałego czasu życia $T − x$ do maksymalnego pozostałego czasu życia $\omega − x$. Ten procent stabilizuje się średnio, gdy $x \to \omega$ i zbiega do $\alpha$, które zatem jawi się jako oczekiwany procent maksymalnego możliwego pozostałego czasu życia efektywnie wykorzystanego przez osobę. 

Pod warunkiem, że wybrany próg wiekowy $x^*$ jest wystarczająco duży, potencjalnym estymatorem dla rozkładu dalszego trwania życia ${}_s q_{x^*}$ jest $G_{\hat{\xi};\hat{\tau}}(s)$. Estymatory kwantyli wyprowadzone z tej krzywej są warunkowymi estymatorami kwantyli, które wskazują na potencjalne przeżycie ponad próg wiekowy $x^*$, gdy jest on osiągnięty. Jeśli $\hat{\xi} < 0$, to osoby mają ograniczony dalszy czas życia z oszacowanym wiekiem granicznym

$$ \hat{\omega} = x^* - \frac{\hat{\tau}}{\hat{\xi}}. $$

Gdy interesują nas estymatory kwantyli bezwarunkowych, możemy powiązać bezwarunkową dystrybuantę ${}_x q_0$ z $G_{\hat{\xi};\hat{\tau}}$ dla

$$ x^* < x \le \hat{\omega} = x^* - \frac{\hat{\tau}}{\hat{\xi}}, $$

poprzez

$$ {}_x q_0 = 1 - {}_x p_0 $$

$$ = 1 - {}_{x^*}p_0 \cdot {}_{x-x^*}p_{x^*} $$

$$ \approx 1 - {}_{x^*}p_0 [1 - G_{\hat{\xi};\hat{\tau}}(x - x^*)]. $$

Wysokie kwantyle odpowiadają wtedy rozwiązaniom $z$ równań $ {}_z q_0 = 1 - \epsilon $ gdzie przybliżenie właśnie wyprowadzone jest użyteczne dla małych poziomów prawdopodobieństwa $\epsilon$. 

Pod warunkiem, że wielkość próby jest wystarczająco duża, możemy oszacować ${}_{x^*}p_0$ za pomocą jego empirycznego odpowiednika. W przykładzie rozważanym w tym rozdziale zaczynamy od osób w wieku 95 lat. Prawdopodobieństwo przeżycia do wieku $x$ takiego, że $x^* - \hat{\tau}/\hat{\xi} \ge x > x^*$, można wtedy oszacować przez

$$ {}_x \hat{p}_{95} = \frac{L_{x^*}}{L_{95}} [1 - G_{\hat{\xi};\hat{\tau}}(x - x^*)] $$

gdzie $L_{x^*}$ i $L_{95}$ to odpowiednio liczba osób, które przeżyły do wieku progowego $x^*$ i do wieku 95 lat (tj. całkowita liczba osób objętych badaniem dla danej kohorty). Wysokie kwantyle są wtedy szacowane jako

$$ \hat{F}^{-1}(\epsilon) = x^* + \frac{\hat{\tau}}{\hat{\xi}} \left[ \left( \frac{L_{95}}{L_{x^*}}(1 - \epsilon) \right)^{-\hat{\xi}} - 1 \right]. $$

Ponadto, dla wieków $x$ takich, że $x^* - \hat{\tau}/\hat{\xi} \ge x > x^*$, siłę wymierania można uzyskać ze wzoru Uogólnionego Rozkładu Pareto

$$ \hat{\mu}_x = \frac{1}{\hat{\tau} + \hat{\xi}(x - x^*)}. $$

Należy zauważyć, że $\hat{\mu}_x$ dąży do $\infty$, gdy wiek $x$ zbliża się do skończonego punktu końcowego $\hat{\omega}$, ponieważ żadna osoba nie może przeżyć poza wiek $\omega$. 

---
### 9.5.5 Wybór Progu dla Uogólnionego Rozkładu Pareto

#### 9.5.5.1 Zasada
Wybór odpowiedniego progu $u$, powyżej którego przybliżenie Uogólnionym Rozkładem Pareto ma zastosowanie, jest z pewnością bardzo trudnym zadaniem. W tej sekcji przedstawiamy czytelnikowi pewne ogólne zasady w tym zakresie, odsyłając do literatury po szczegółowe podejścia stosowane w konkretnych sytuacjach. Często techniki wyboru progu dostarczają jedynie zakresu rozsądnych wartości, dlatego zaleca się jednoczesne stosowanie kilku z nich w celu uzyskania bardziej wiarygodnych wyników. 

Przy wyborze optymalnego progu $u$, powyżej którego przybliżenie Uogólnionym Rozkładem Pareto jest słuszne dla rozkładu nadwyżek, należy wziąć pod uwagę dwa czynniki: 

* Zbyt duża wartość $u$ skutkuje małą liczbą nadwyżek, a w konsekwencji niestabilnymi estymatami górnych kwantyli. Aktuariusz traci również możliwość szacowania mniejszych kwantyli. 
* Zbyt mała wartość $u$ oznacza, że Uogólniony Rozkład Pareto nie jest właściwy dla umiarkowanych obserwacji, co prowadzi do obciążonych estymat kwantyli. Obciążenie to może być znaczne, ponieważ umiarkowane obserwacje zazwyczaj stanowią największą część próby. 

Naszym celem jest zatem określenie minimalnej wartości progu, powyżej którego Uogólniony Rozkład Pareto staje się rozsądnym przybliżeniem ogona rozkładu będącego przedmiotem badania. 

Wiemy, że Uogólniony Rozkład Pareto posiada wygodną właściwość stabilności progowej, która zapewnia, że

$$ Y \sim \text{GPar}(\xi, \tau) \implies P[Y - u > t | Y > u] = 1 - G_{\xi, \tau + \xi u}(t) $$

z tym samym parametrem indeksu $\xi$, dla dowolnego $u > 0$. Mówiąc prościej, jeśli $Y$ ma Uogólniony Rozkład Pareto, to nadwyżki $Y$ ponad dowolny próg nadal mają Uogólniony Rozkład Pareto. Ta właściwość jest wykorzystywana przez większość procedur wyboru optymalnego progu $u$, powyżej którego przybliżenie Uogólnionym Rozkładem Pareto ma zastosowanie. 

Właściwość stabilności (9.7) Uogólnionego Rozkładu Pareto zapewnia, że wykres estymatorów $\hat{\xi}$ obliczonych przy rosnących progach ujawnia estymacje, które stabilizują się, gdy osiągnięty zostanie najmniejszy próg, dla którego zachowanie Uogólnionego Rozkładu Pareto jest słuszne. Można to sprawdzić graficznie i pozwala to aktuariuszowi określić najmniejszy próg $u$, powyżej którego Uogólniony Rozkład Pareto stanowi dobre przybliżenie ogona. 

#### 9.5.5.2 Zastosowanie do Ciężkości Szkód
Zgodnie z praktyką rynkową, koncentrujemy się na ciężkościach szkód przekraczających 1 000 000 franków belgijskich (około 25 000 €), co odpowiada 13.83-krotności obserwowanej średniej ciężkości szkody. Kwantyl 97.5% rozkładu Gamma o średniej i wariancji odpowiadających ich empirycznym odpowiednikom wynosi 360 481.7 franków belgijskich, podczas gdy kwantyle 99% i 99.5% wynoszą odpowiednio 1 937 007 i 4 004 094 franków belgijskich. Odpowiednie wartości dla rozkładu Odwrotnego Gaussa to 409 671.3 franków belgijskich, 1 403 344 franków belgijskich i 2 952 146 franków belgijskich. Widzimy więc, że rozkłady ED używane do modelowania umiarkowanych strat znacznie wykraczają poza najniższy rozważany próg, jeśli analiza POT rozpoczyna się od 1 000 000 franków belgijskich. 

Wykres indeksu Pareto dla ciężkości szkód w ubezpieczeniach komunikacyjnych przedstawiono na Rys. 9.7. Został on wygenerowany za pomocą funkcji `tcplot` z pakietu R `POT`. Widzimy tam, że oszacowane indeksy wydają się być względnie stabilne, co jest zgodne z twierdzeniem Pickandsa-Balkemy-de Haana. Przedziały ufności są jednak dość szerokie. Ogólnie rzecz biorąc, wykres ten nie dostarcza aktuariuszowi wielu wskazówek co do wyboru progu. 

Wykres Gertensgarbe'a to kolejne narzędzie graficzne, które można wykorzystać do oszacowania progu definiującego duże straty. Opiera się on na założeniu, że optymalny próg można znaleźć jako punkt zwrotny w serii odstępów między uporządkowanymi kosztami szkód. Kluczową ideą jest to, że można rozsądnie oczekiwać, iż zachowanie różnic odpowiadających obserwacjom ekstremalnym będzie inne niż to odpowiadające obserwacjom nieekstremalnym. Zatem powinien istnieć punkt zwrotny, jeśli analiza POT ma zastosowanie, a ten punkt zwrotny można zidentyfikować za pomocą sekwencyjnej wersji testu Manna-Kendalla jako punkt przecięcia znormalizowanych statystyk rang progresywnych i retrogradywnych. Jest on zaimplementowany w funkcji `ggplot` pakietu R `tea`. Dokładniej, biorąc pod uwagę serię różnic $\Delta_i = y_{(i)} − y_{(i−1)}$, punkt początkowy regionu ekstremalnego zostanie wykryty jako punkt zwrotny serii $\{\Delta_i, i = 2, 3,..., n\}$. W tym teście określa się dwie znormalizowane serie $U_p$ i $U_r$, najpierw na podstawie serii $\Delta_1, \Delta_2,...$ a następnie na podstawie serii różnic od końca do początku, $\Delta_n, \Delta_{n−1},...$, zamiast od początku do końca. Punkt przecięcia tych dwóch serii określa kandydata na punkt zwrotny, który będzie istotny, jeśli przekroczy wysoki percentyl rozkładu normalnego. 

Wykres Gertensgarbe'a przedstawiono na Rys. 9.8. Wskazuje on próg 2 471 312 franków belgijskich (co oznacza, że 29 szkód kwalifikuje się jako duże). Wartość $p$-value testu Manna-Kendalla wynosi $1.299018 \times 10^{−4}$, więc wynik jest istotny. Jest to zgodne z wnioskiem wyciągniętym z wykresu indeksu Pareto przedstawionego na Rys. 9.7, dlatego wybieramy próg $u = 2 471 312$ dla ciężkości szkód w ubezpieczeniach komunikacyjnych. 

Teraz, gdy wartość progu została wybrana, istnieje kilka metod szacowania odpowiednich parametrów Uogólnionego Rozkładu Pareto. Wymienić można metodę największej wiarygodności (z karą lub bez), (nieobciążone) ważone momenty probabilistyczne, momenty, estymatory Pickandsa, minimalnej dywergencji potęgowej gęstości, mediany i maksymalnej dobroci dopasowania, wszystkie dostępne w funkcji `fitgpd` pakietu R `POT`. Tutaj przyjmujemy podejście największej wiarygodności. Aby dopasować model Uogólnionego Rozkładu Pareto do nadwyżek ponad próg $u$, maksymalizujemy funkcję wiarygodności

$$ \mathcal{L}(\xi, \tau) = \prod_{i|y_i > u} \frac{1}{\tau} \left( 1 + \frac{\xi}{\tau}(y_i - u) \right)^{-1/\xi - 1} $$

lub odpowiadającą jej funkcję log-wiarygodności

$$ L(\xi, \tau) = \ln \mathcal{L}(\xi, \tau) = -N_u \ln \tau - \left( 1 + \frac{1}{\xi} \right) \sum_{i|y_i > u} \ln \left( 1 + \frac{\xi}{\tau}(y_i - u) \right) $$

gdzie $N_u = \text{\#} {y_i\mid y_i > u}$ jest liczbą dużych szkód zaobserwowanych w portfelu, jak wprowadzono wcześniej. 

Ten problem optymalizacyjny wymaga algorytmów numerycznych i odpowiednich wartości początkowych dla parametrów $\xi$ i $\tau$. Średnia i wariancja Uogólnionego Rozkładu Pareto wynoszą odpowiednio $\tau/(1 - \xi)$ pod warunkiem $\xi < 1$, oraz $\tau^2/((1 - \xi)^2(1 - 2\xi))$ pod warunkiem $\xi < 1/2$. Wartości początkowe metodą momentów to

$$ \hat{\xi}_0 = \frac{1}{2} \left( 1 - \frac{\bar{y}^2}{s^2} \right) \quad \text{i} \quad \hat{\tau}_0 = \frac{1}{2} \bar{y} \left( \frac{\bar{y}^2}{s^2} + 1 \right), $$

gdzie $\bar{y}$ i $s^2$ są średnią i wariancją z próby. Można również wykorzystać wartości $\tau$ i $\xi$ pochodzące z dopasowania liniowego do prawej części wykresu empirycznej funkcji średniej nadwyżki. 

Przy progu ustalonym na 2 471 312 franków belgijskich, otrzymujemy $\hat{\xi} = 0.2718819$, co oznacza, że mamy do czynienia z rozkładem o grubym ogonie. Odpowiadające mu $\tau = 7 655 438$. Należy zauważyć, że $\hat{\xi}$ rzeczywiście odpowiada plateau widocznemu na wykresie indeksu Pareto przedstawionym na Rys. 9.7. 

#### 9.5.5.3 Zastosowanie w Ubezpieczeniach na Życie
Przejdźmy teraz do modelowania skrajnych czasów życia. Wykresy indeksu Pareto dla czasów życia przedstawiono na Rys. 9.9, oddzielnie dla mężczyzn i kobiet. Narzędzia graficzne są trudniejsze do interpretacji w porównaniu z ciężkościami szkód. Odsyłamy czytelnika do Gbari i in. (2017a) w celu określenia progu wiekowego $x^*$ takiego że przybliżenie (9.5) jest wystarczająco dokładne dla $x \ge x^*$ za pomocą kilku zautomatyzowanych procedur. Zgodnie z ich wnioskami, możemy ustawić $x^*$ na 98.89 dla mężczyzn i 100.89 dla kobiet. Wartości te wydają się rozsądne, biorąc pod uwagę wykresy indeksu Pareto przedstawione na Rys. 9.9. Estymaty największej wiarygodności parametrów Uogólnionego Rozkładu Pareto można znaleźć w Tabeli 9.4 wraz z błędami standardowymi. 

Oszacowany wiek graniczny ω jest dany wzorem

$$ \hat{\omega} = x^* - \frac{\hat{\tau}}{\hat{\xi}} = \begin{cases} 114,82 & \text{dla mężczyzn,} \\ 122,73 & \text{dla kobiet.} \end{cases} $$

Populacja kobiet ma najwyższy oszacowany wiek graniczny. Estymacje są zgodne z najwyższymi zaobserwowanymi wiekami zgonu 112.58 dla kobiet i 111.47 dla mężczyzn dla rozważanych kohort urodzonych w Belgii. Wyniki te są spójne z maksymalnymi obserwowanymi na świecie wiekami zgonu. Co ciekawe, uzyskany wiek graniczny dla kobiet jest bliski rekordowi Jeanne Calment wynoszącemu 122.42 (122 lata i 164 dni, urodzona w Arles we Francji 21 lutego 1875 r., zmarła w tym samym miejscu 4 sierpnia 1997 r.). 

Każda miara dobroci dopasowania może być użyta w celu sprawdzenia, czy dane są zgodne z Uogólnionym Rozkładem Pareto powyżej wybranego progu. W tym celu często używa się wykresu kwantylowo-kwantylowego (QQ-plot) dla Uogólnionego Rozkładu Pareto. Na tym wykresie kwantyle empiryczne są przedstawione w odniesieniu do oszacowanych kwantyli Uogólnionego Rozkładu Pareto. Jeśli Uogólniony Rozkład Pareto skutecznie dopasowuje się do rozważanych danych, wykreślone pary powinny znajdować się blisko linii o nachyleniu 45 stopni. Funkcja `qqgpd` z pakietu R `tea` może być użyta do wykreślenia obserwacji empirycznych powyżej danego progu w odniesieniu do teoretycznych kwantyli Uogólnionego Rozkładu Pareto. Wykresy QQ dla Uogólnionego Rozkładu Pareto dla skrajnych czasów życia przedstawiono na Rys. 9.10. Wyraźnie liniowy wzorzec na wykresie QQ potwierdza, że model Uogólnionego Rozkładu Pareto adekwatnie opisuje rozkład pozostałego czasu życia powyżej $x^*$. 

Na zakończenie tego zastosowania w ubezpieczeniach na życie, omówmy różnicę w stosunku do analiz przeprowadzonych na zagregowanych danych o śmiertelności. W demografii poziomy śmiertelności są zwykle oceniane na podstawie danych statystycznych zagregowanych według osiągniętego wieku, a nie indywidualnych wieków zgonu. Dzieje się tak zwłaszcza w przypadku danych dotyczących populacji ogólnej. Aktuariusz zna jedynie obserwowane liczby $L_x$ osób osiągających wiek $x$, odpowiadające im liczby zgonów $D_x = L_x − L_{x+1}$ oraz odpowiadającą im ekspozycję $E_x$ w tym wieku. Dostępne dane są zatem takie, jak przedstawiono w Tabeli 9.5. 

Model Uogólnionego Rozkładu Pareto można estymować na danych zagregowanych $(L_x, D_x), x \ge \lceil x^* \rceil$, gdzie $\lceil x^*\rceil$ jest najniższą liczbą całkowitą większą lub równą $x^*$. Biorąc pod uwagę wiek $x \ge x^*$, jednoroczne prawdopodobieństwa przeżycia uzyskuje się z przybliżenia (9.5), co daje

$$ p_x = p_x(\xi, \tau) = \left( 1 + \frac{\xi}{\tau + \xi(x - x^*)} \right)^{-1/\xi}. $$

Parametry Uogólnionego Rozkładu Pareto można oszacować w modelu warunkowym

$$D_x \sim Bin(L_x, q_x), \quad \text{gdzie} \quad q_x = q_x(\xi, \tau) = 1 − p_x(\xi, \tau). $$ 

Dokładniej, maksymalizujemy funkcję log-wiarygodności rozkładu dwumianowego

$$ L(\xi, \tau) = \sum_{x \ge x^*} [L_x \ln p_x(\xi, \tau) + D_x \ln q_x(\xi, \tau)]. $$

To daje

$$ \hat{\xi} = \begin{cases} -0.131 & \text{z błędem standardowym 0.016 dla mężczyzn,} \\ -0.096 & \text{z błędem standardowym 0.015 dla kobiet} \end{cases} $$

oraz

$$ \hat{\tau} = \begin{cases} 2.100 & \text{z błędem standardowym 0.061 dla mężczyzn,} \\ 2.034 & \text{z błędem standardowym 0.046 dla kobiet.} \end{cases} $$

Powyższe wartości są bliskie wartościom w Tabeli 9.4. Estymowana wartość $\omega$ wynosi teraz

$$ \hat{\omega} = \begin{cases} 114.90 & \text{dla mężczyzn,} \\ 122.13 & \text{dla kobiet,} \end{cases} $$

co jest zgodne z wartościami w (9.8) uzyskanymi przy użyciu indywidualnych wieków zgonu. 

Główną trudnością przy pracy z danymi zagregowanymi jest wybór odpowiedniego wieku progowego, powyżej którego zachowanie Uogólnionego Rozkładu Pareto staje się widoczne.