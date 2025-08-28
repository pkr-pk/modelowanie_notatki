# Rozdział 7: Trendy modelujące

## 7.3 Stacjonarność i modele błądzenia losowego

Podstawową kwestią w przypadku procesów, które ewoluują w czasie, jest stabilność procesu. Na przykład: „Czy dojazd do pracy zajmuje mi więcej czasu, odkąd zainstalowano nową sygnalizację świetlną?”, „Czy kwartalne zyski poprawiły się od czasu objęcia stanowiska przez nowego prezesa?”. Mierzymy procesy, aby poprawić ich wydajność, zarządzać nimi i prognozować ich przyszłość. Ponieważ stabilność jest fundamentalną kwestią, będziemy pracować ze szczególnym rodzajem stabilności, zwanym stacjonarnością.

**Definicja.** Stacjonarność to formalne pojęcie matematyczne odpowiadające stabilności szeregu czasowego danych. Mówi się, że szereg jest **(słabo) stacjonarny**, jeśli:
* Średnia $E y_t$ nie zależy od $t$.
* Kowariancja między $y_s$ a $y_t$ zależy tylko od różnicy między jednostkami czasu, $|t - s|$.

Zatem, na przykład, w warunkach słabej stacjonarności $E y_4 = E y_8$, ponieważ średnie nie zależą od czasu i w związku z tym są równe. Ponadto, $\text{Cov}(y_4, y_6) = \text{Cov}(y_6, y_8)$, ponieważ $y_4$ i $y_6$ są oddalone o dwie jednostki czasu, tak samo jak $y_6$ i $y_8$. Inną implikacją drugiego warunku jest to, że $\sigma^2 = \text{Cov}(y_t, y_t) = \text{Cov}(y_s, y_s) = \sigma^2$. Zatem **słabo stacjonarny szereg ma stałą średnią oraz stałą wariancję (jest homoskedastyczny)**. Inny rodzaj stacjonarności, znany jako **ścisła** lub **silna stacjonarność**, wymaga, aby cały rozkład $y_t$ był stały w czasie, a nie tylko średnia i wariancja.

### Biały Szum

Związek między modelami podłużnymi a przekrojowymi można ustalić poprzez pojęcie procesu **białego szumu**. Proces białego szumu to proces stacjonarny, który nie wykazuje widocznych wzorców w czasie. Mówiąc bardziej formalnie, **proces białego szumu to po prostu szereg i.i.d.**, czyli o identycznym i niezależnym rozkładzie. Proces białego szumu jest tylko jednym z rodzajów procesów stacjonarnych – w Rozdziale 8 wprowadzony zostanie inny typ, model autoregresyjny.

Szczególną cechą procesu białego szumu jest to, że prognozy nie zależą od tego, jak daleko w przyszłość chcemy prognozować. Załóżmy, że szereg obserwacji, $y_1, ..., y_T$, został zidentyfikowany jako proces białego szumu. Niech $\bar{y}$ i $s_y$ oznaczają odpowiednio średnią arytmetyczną i odchylenie standardowe próby. Prognoza obserwacji w przyszłości, powiedzmy $y_{T+l}$, dla $l$ jednostek czasu w przyszłość, wynosi $\bar{y}$. Co więcej, przedział prognozy to:

$$\bar{y} \pm t_{T-1,1-\alpha/2} s_y \sqrt{1 + \frac{1}{T}}$$

W zastosowaniach dotyczących szeregów czasowych, ponieważ wielkość próby $T$ jest zazwyczaj stosunkowo duża, używamy przybliżonego 95% przedziału predykcji $\bar{y} \pm 2s_y$. Ten przybliżony przedział prognozy ignoruje niepewność parametrów wynikającą z użycia $\bar{y}$ i $s_y$ do oszacowania średniej $E y$ i odchylenia standardowego $\sigma$ szeregu. Zamiast tego kładzie nacisk na niepewność przyszłych realizacji szeregu (znaną jako niepewność innowacji). Należy zauważyć, że przedział ten nie zależy od wyboru $l$, czyli liczby jednostek czasu, na które prognozujemy w przyszłość.

**Model białego szumu jest jednocześnie najmniej i najważniejszym z modeli szeregów czasowych.** Jest najmniej ważny w tym sensie, że model zakłada, iż obserwacje są ze sobą niepowiązane, co jest mało prawdopodobne w przypadku większości interesujących szeregów. Jest najważniejszy, ponieważ nasze wysiłki modelowania zmierzają do zredukowania szeregu do procesu białego szumu. W analizie szeregów czasowych procedura redukcji szeregu do procesu białego szumu nazywana jest **filtrem**. Po odfiltrowaniu wszystkich wzorców z danych, mówi się, że niepewność jest nieredukowalna.

### Błądzenie Losowe

Wprowadzamy teraz model błądzenia losowego. Dla tego modelu szeregu czasowego pokażemy, jak filtrować dane, po prostu obliczając różnice.

Dla ilustracji, załóżmy, że grasz w prostą grę opartą na rzucie dwiema kostkami. Aby zagrać, musisz zapłacić 7\$ za każdym razem, gdy rzucasz kostkami. Otrzymujesz liczbę dolarów odpowiadającą sumie oczek na obu kostkach, $c_t^*$. Niech $c_t$ oznacza twoją wygraną w każdym rzucie, tak że $c_t = c_t^* - 7$. Zakładając, że rzuty są niezależne i pochodzą z tego samego rozkładu, szereg $\{c_t\}$ jest procesem białego szumu.

Załóżmy, że zaczynasz z kapitałem początkowym $y_0 = \$100$. Niech $y_t$ oznacza sumę kapitału po $t$-tym rzucie. Zauważ, że $y_t$ jest określone rekurencyjnie przez $y_t = y_{t-1} + c_t$. Na przykład, ponieważ w pierwszym rzucie, $t = 1$, wygrałeś 3\$, masz teraz kapitał $y_1 = y_0 + c_1$, czyli $103 = 100 + 3$. Tabela 7.1 pokazuje wyniki dla pierwszych pięciu rzutów. Rysunek 7.6 to wykres szeregu czasowego sum, $y_t$, dla 50 rzutów.

Sumy cząstkowe procesu białego szumu definiują model błądzenia losowego. Na przykład szereg $\{y_1, ..., y_{50}\}$ na rysunku 7.6 jest realizacją modelu błądzenia losowego. Określenie „suma cząstkowa” jest używane, ponieważ każda obserwacja, $y_t$, została utworzona przez zsumowanie wygranych do czasu $t$. W tym przykładzie wygrane, $c_t$, są procesem białego szumu, ponieważ zwrócona kwota, $c_t^*$, ma rozkład i.i.d. W naszym przykładzie twoje wygrane z każdego rzutu kostką są reprezentowane przez proces białego szumu. To, czy wygrasz w jednym rzucie, nie ma wpływu na wynik następnego lub poprzedniego rzutu. W przeciwieństwie do tego, twój kapitał w dowolnym rzucie jest silnie powiązany z kapitałem po następnym lub przed poprzednim rzutem. Twój kapitał po każdym rzucie kostką jest reprezentowany przez model błądzenia losowego.
