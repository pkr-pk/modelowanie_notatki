# Rozdział 7: Poza modelowanie średniej: Podwójne modele GLM i GAM dla położenia, skali i kształtu (GAMLSS)

### **7.3 Modelowanie dyspersji: podwójne GLM**

GLM narzucają, że parametr dyspersji $\phi$ musi być taki sam dla każdej osoby. Stąd wariancja odpowiedzi $Var[Y_i] = \phi\nu_iV(\mu_i)$ może zmieniać się wraz z czynnikami ryzyka tylko jako funkcja średniej $\mu_i$. Podwójne GLM unikają tego ograniczenia. Dokładniej, w podwójnym GLM parametr dyspersji

$$\phi_i = \phi(x_i)$$

również zależy od dostępnych cech poprzez specyficzną ocenę (score), co czyni go specyficznym dla każdej obserwacji. Zatem nie tylko średnia odpowiedź zależy od dostępnych cech, ale także współczynnik dyspersji. Dokładniej, oprócz modelowania średniej $g(\mu_i) = x_i^T \beta$, istnieje również modelowanie dyspersji 

$$g_d (\phi_i) = x_i^T \gamma$$ 

z własną funkcją łączącą $g_d$ i współczynnikami regresji $\gamma$. Często dla $g_d$ używana jest logarytmiczna funkcja łącząca, aby zapewnić, że $\hat{\phi}_i$ pozostaje dodatnie. Jednak inne funkcje łączące mogą być przydatne w niektórych zastosowaniach (zobacz na przykład zastosowanie do wyrównywania śmiertelności w obecności duplikatów omówione w następnej sekcji).

Współczynniki regresji $\gamma$ są następnie szacowane za pomocą Gamma GLM przeprowadzonego na kwadratach reszt dewiacji $r_{Di}$ lub reszt Pearsona $r_{Pi}$ zdefiniowanych w Sekcji 4.9. Zaczynając od $\phi_i = 1$ dla wszystkich $i$ w kroku 0, współczynniki regresji $\beta$ i $\gamma$ są następnie szacowane naprzemiennie, aż do osiągnięcia zbieżności. Dokładniej, dopasowanie podwójnego GLM uzyskuje się poprzez iteracyjne wykonywanie następujących kroków:

* dopasuj GLM dla średniej odpowiedzi, ze stałym $\phi$ dla wszystkich obserwacji, to jest,

    $$g(E[Y_i]) = x_i^T \beta$$

    $$Var[Y_i] = \frac{\phi}{\nu_i} V(E[Y_i]).$$

* oblicz wkład każdej obserwacji do dewiacji i oblicz kwadraty reszt Pearsona lub reszt dewiacji $R_i^2$.
* dopasuj GLM dla dyspersji, przyjmując jako odpowiedź wkład $R_i^2$ każdej obserwacji do dewiacji. Rozkładem jest Gamma i w tym kroku nie uwzględnia się żadnych wag. Dopasowane wartości są nowym parametrem dyspersji dla każdego rekordu. Dokładniej, modelowanie dyspersji wykorzystuje Gamma GLM z

    $$g_d (E[R_i^2 ]) = x_i^T \gamma$$

    $$Var[R_i^2 ] = \tau (E[R_i^2 ])^2$$

    $$\phi_i = g_d^{-1} (x_i^T \gamma ).$$

* dopasuj GLM dla średniej, ale tym razem używając specyficznego parametru dyspersji dla każdego rekordu (dzieląc wagę przez parametr dyspersji specyficzny dla odpowiedzi uzyskany z poprzedniego kroku), to jest,

    $$g(E[Y_i]) = x_i^T \beta$$

    $$Var[Y_i] = \frac{\phi_i}{\nu_i} V(E[Y_i]).$$

* oblicz kwadraty reszt Pearsona lub reszt dewiacji $R_i^2$ i powtórz poprzednie kroki.

Ten proces iteracyjny jest kontynuowany, aż zostanie spełniony pewien warunek zatrzymania. Związek między dwoma modelami GLM jest następujący: model dla średniej generuje odpowiedź $R_i^2$ używaną do dopasowania modelu dyspersji, który z kolei generuje dyspersję dla modelu średniej.

Podwójny GLM może poprawić estymację średniej w przypadku, gdy pewne klasy biznesu wydają się być bardziej zmienne w porównaniu z innymi. W tym ustawieniu, model ma możliwość nadawania mniejszej wagi przeszłym obserwacjom zmiennego biznesu i większej wagi stabilnemu biznesowi, którego dane są bardziej informacyjne. W ten sposób model staje się zdolny do ignorowania większej ilości szumu, gdy jest to konieczne, a tym samym do wychwytywania większej ilości sygnału. Podwójny GLM w pewnym sensie wybiera ocenę liniową (linear score) w modelu dyspersji, aby zdefiniować optymalne wagi, maksymalizując dobroć dopasowania. Czasami cecha, która wydaje się nieistotna w ustawieniu GLM, staje się istotna, gdy aktuariusz przechodzi do ram podwójnego GLM.

Podwójny GLM jest szczególnie istotny dla modelowania Tweedie, ze względu na ukryte ograniczenie nieodłącznie związane z tymi modelami, omówione w Sekcji 7.2.5. Rozważmy ponownie zastosowanie do rezerwacji szkodowej, z danymi przedstawionymi w formie trójkąta indeksowanego rokiem wypadku AY i rokiem rozwoju DY. Załóżmy, że kwota w komórce odpowiadającej AY= j i DY= k ma rozkład Tweedie ze średnią wyrażoną jako funkcja czynników związanych z rokiem wypadku j i rozwojem k. Dane przedstawione w trójkątach runoff generalnie wykazują następujące dwa przeciwstawne efekty:

* generalnie, większość roszczeń jest zgłaszana na wczesnym etapie rozwoju, więc składnik częstości wykazuje tendencję spadkową w całym okresie rozwoju;
* średni koszt na roszczenie często wzrasta w kolejnych latach rozwoju, więc składnik dotkliwości wykazuje tendencję wzrostową.

Ponieważ częstości i dotkliwości mają przeciwstawne tendencje, modele ze stałą dyspersją są podatne na błędy. W przypadku modelu Tweedie, stałe $\phi$ implikuje, że wpływ $j$ i $k$ na zarówno składnik częstości, jak i dotkliwości musi iść w tym samym kierunku. Dlatego pożądane jest przejście na podwójny GLM, w którym zarówno średnia, jak i wariancja zależą od efektów $j$ i $k$.