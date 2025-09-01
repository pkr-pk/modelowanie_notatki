# Rozdział 3: Drzewa regresyjne

## 3.2 Binarne drzewo regresyjne

### 3.2.3 Zasada określania, kiedy węzeł jest terminalny

Węzeł $t$ nieuchronnie staje się terminalny, gdy $\chi_t$ nie może być dalej dzielone. Dzieje się tak, gdy obserwacje w węźle $t$ mają te same wartości dla wszystkich cech, tj. gdy $x_i = x_j$ dla wszystkich $(y_i, x_i)$ i $(y_j, x_j)$ takich, że $x_i, x_j \in \chi_t$. W takim przypadku podział węzła $t$ wygenerowałby podzbiór bez żadnych obserwacji. Węzeł $t$ jest również z konieczności terminalny, gdy zawiera obserwacje o tej samej wartości odpowiedzi, tj. gdy $y_i = y_j$ dla wszystkich $(y_i, x_i)$ i $(y_j, x_j)$ takich, że $x_i, x_j \in \chi_t$. W szczególności dzieje się tak, gdy węzeł $t$ zawiera tylko jedną obserwację.

Te kryteria zatrzymania są nieodłącznym elementem procedury rekurencyjnego partycjonowania. Takie nieuniknione węzły terminalne prowadzą do największego możliwego drzewa regresyjnego, jakie możemy wyhodować na zbiorze uczącym. Jednak takie drzewo prawdopodobnie wychwytuje szum w zbiorze uczącym i powoduje nadmierne dopasowanie (overfitting).

Aby zmniejszyć rozmiar drzewa i tym samym zapobiec nadmiernemu dopasowaniu, te kryteria zatrzymania, które są nieodłączne dla procedury rekurencyjnego partycjonowania, są uzupełniane o kilka reguł. Trzy powszechnie stosowane reguły zatrzymania można sformułować w następujący sposób:

* Węzeł $t$ jest uznawany za terminalny, gdy zawiera mniej niż ustaloną liczbę obserwacji.
* Węzeł $t$ jest uznawany za terminalny, jeśli co najmniej jeden z jego węzłów potomnych, $t_L$ i $t_R$, który wynika z optymalnego podziału $s_t$, zawiera mniej niż ustaloną liczbę obserwacji.
* Węzeł $t$ jest uznawany za terminalny, gdy jego głębokość jest równa ustalonej maksymalnej głębokości.

Zauważmy, że głębokość węzła $t$ jest równa $d$, jeśli należy on do generacji $d+1$. Na przykład na Rysunku 3.5, $t_0$ ma głębokość zero, $t_1$ i $t_2$ mają głębokość jeden, a węzły terminalne $t_3$, $t_4$, $t_5$ i $t_6$ mają głębokość dwa.

Inna powszechna reguła zatrzymania polega na ustaleniu progu i podjęciu decyzji, że węzeł $t$ jest terminalny, jeśli spadek dewiancji, który uzyskujemy przez podział węzła $t$ optymalnym podziałem $s_t$, jest mniejszy niż ten ustalony próg. Przypomnijmy, że podział węzła $t$ na węzły potomne $t_L$ i $t_R$ skutkuje spadkiem dewiancji danym przez

$$
\Delta D_{\chi_t} = D_{\chi_t}(\hat{c}_t) - (D_{\chi_{t_L}}(\hat{c}_{t_L}) + D_{\chi_{t_R}}(\hat{c}_{t_R})). \quad (3.2.4)
$$

Warto zauważyć, że ta redukcja dewiancji $\Delta D_{\chi_t}$ jest zawsze dodatnia. Rzeczywiście, mamy

$$
D_{\chi_t}(\hat{c}_t) = D_{\chi_{t_L}}(\hat{c}_t) + D_{\chi_{t_R}}(\hat{c}_t) \ge D_{\chi_{t_L}}(\hat{c}_{t_L}) + D_{\chi_{t_R}}(\hat{c}_{t_R})
$$

ponieważ estymatory maksymalnej wiarygodności $\hat{c}_{t_L}$ i $\hat{c}_{t_R}$ minimalizują odpowiednio dewiancje $D_{\chi_{t_L}}(\hat{c}_t)$ i $D_{\chi_{t_R}}(\hat{c}_t)$. W konsekwencji, jeśli dla partycji $\{\chi_t\}_{t \in \mathcal{T}}$ przestrzeni $\chi$ rozważymy dodatkowy standaryzowany podział binarny $S_t$ dla $\chi_t$ dla danego $t \in \mathcal{T}$, co daje nowy zbiór indeksów $\mathcal{T}' = \mathcal{T} \setminus \{t\} \cup \{t_L, t_R\}$ dla węzłów terminalnych, to z konieczności mamy

$$
D((\hat{c}_t)_{t \in \mathcal{T}}) \ge D((\hat{c}_t)_{t \in \mathcal{T}'}).
$$

Zatem dodatkowy standaryzowany podział binarny zawsze zmniejsza dewiancję.

Chociaż przedstawione powyżej reguły zatrzymania mogą dawać dobre wyniki w praktyce, strategia wczesnego zatrzymywania wzrostu drzewa jest ogólnie niezadowalająca. Na przykład, ostatnia reguła daje zbyt duże drzewo, jeśli próg jest ustawiony zbyt nisko. Zwiększenie progu może prowadzić do zbyt małego drzewa, a węzeł $t$ z małą redukcją dewiancji $\Delta D_{\chi_t}$ może mieć węzły potomne $t_L$ i $t_R$ z większymi spadkami dewiancji. Stąd, uznając $t$ za terminalny, dobre podziały w węzłach $t_L$ i $t_R$ nigdy nie zostałyby użyte.

Dlatego preferuje się przycinanie (pruning) drzewa zamiast zatrzymywania jego wzrostu. Przycinanie drzewa polega na pełnym rozwinięciu drzewa, a następnie przycinaniu go w górę, aż do znalezienia optymalnego drzewa. Zostanie to omówione w Sekcji 3.3.

## 3.3 Drzewa o Właściwym Rozmiarze

Widzieliśmy kilka zasad określania, kiedy węzeł $t$ jest terminalny. Cechą wspólną tych zasad jest to, że wcześnie zatrzymują wzrost drzewa. Innym sposobem na znalezienie drzewa o właściwym rozmiarze jest pełne rozwinięcie drzewa, a następnie jego przycięcie.

Odtąd, dla ułatwienia prezentacji, będziemy oznaczać drzewo regresyjne jako $T$. Drzewo $T$ jest zdefiniowane przez zbiór podziałów wraz z kolejnością, w jakiej są używane, oraz predykcje w węzłach terminalnych. W razie potrzeby, aby wyraźnie pokazać związek z drzewem $T$, użyjemy notacji $\mathcal{T}_{(T)}$ dla odpowiadającego mu zbioru indeksów $T$ węzłów terminalnych. Dodatkowo, przez $|T|$ będziemy rozumieć liczbę węzłów terminalnych drzewa $T$, czyli $|\mathcal{T}_{(T)}|$.

Aby zdefiniować proces przycinania, należy określić pojęcie gałęzi drzewa. Gałąź $T^{(t)}$ to część $T$, która składa się z węzła $t$ i wszystkich jego węzłów potomnych. Przycinanie gałęzi $T^{(t)}$ drzewa $T$ oznacza usunięcie z $T$ wszystkich węzłów potomnych węzła $t$. Wynikowe drzewo jest oznaczane jako $T - T^{(t)}$. Mówi się, że $T - T^{(t)}$ jest przyciętym poddrzewem $T$. Na przykład na Rysunku 3.12 przedstawiono drzewo $T$, jego gałąź $T^{(t_2)}$ oraz poddrzewo $T - T^{(t_2)}$ otrzymane z $T$ przez przycięcie gałęzi $T^{(t_2)}$. Ogólniej, drzewo $T^{\prime}$, które jest otrzymywane z $T$ przez kolejne przycinanie gałęzi, nazywane jest przyciętym poddrzewem $T$, lub po prostu poddrzewem $T$, i jest oznaczane jako $T^{\prime} \preceq T$.

Pierwszym krokiem procesu przycinania jest wyhodowanie największego możliwego drzewa $T_{max}$, pozwalając procedurze podziału kontynuować, aż wszystkie węzły terminalne będą zawierać albo obserwacje o identycznych wartościach cech (tj. węzły terminalne $t \in \mathcal{T}$, gdzie $x_i = x_j$ dla wszystkich $(y_i, x_i)$ i $(y_j, x_j)$ takich, że $x_i, x_j \in \chi_t$), albo obserwacje o tej samej wartości odpowiedzi (tj. węzły terminalne $t \in \mathcal{T}$, gdzie $y_i = y_j$ dla wszystkich $(y_i, x_i)$ i $(y_j, x_j)$ takich, że $x_i, x_j \in \chi_t$). 

Zauważmy, że początkowe drzewo $T_{init}$ może być mniejsze niż $T_{max}$. Rzeczywiście, załóżmy, że proces przycinania, zaczynając od największego drzewa $T_{max}$, produkuje poddrzewo $T_{prune}$. Wtedy proces przycinania zawsze doprowadzi do tego samego poddrzewa $T_{prune}$, jeśli zaczniemy od dowolnego poddrzewa $T_{init}$ drzewa $T_{max}$ takiego, że $T_{prune}$ jest poddrzewem $T_{init}$.

Zatem proces przycinania rozpoczynamy od wystarczająco dużego drzewa $T_{init}$. Następnie idea przycinania $T_{init}$ polega na skonstruowaniu sekwencji coraz mniejszych drzew

$$T_{init}, T_{|T_{init}|-1}, T_{|T_{init}|-2}, \dots, T_1,$$

gdzie $T_k$ jest poddrzewem $T_{init}$ z $k$ węzłami terminalnymi, $k=1, \dots, |T_{init}|-1$. W szczególności $T_1$ składa się tylko z węzła głównego $t_0$ drzewa $T_{init}$.

Oznaczmy przez $\mathcal{C}(T_{init}, k)$ klasę wszystkich poddrzew $T_{init}$ mających $k$ węzłów terminalnych. Intuicyjna procedura tworzenia sekwencji drzew $T_{init}, T_{|T_{init}|-1}, \dots, T_1$ polega na wyborze, dla każdego $k=1, \dots, |T_{init}|-1$, drzewa $T_k$, które minimalizuje dewiancję $D((\hat{c}_t)_{t \in \mathcal{T}_{(T)}})$ spośród wszystkich poddrzew $T$ drzewa $T_{init}$ z $k$ węzłami terminalnymi, czyli

$$D((\hat{c}_t)_{t \in \mathcal{T}_{(T_k)}}) = \min_{T \in \mathcal{C}(T_{init}, k)} D((\hat{c}_t)_{t \in \mathcal{T}_{(T)}}).$$

Zatem, dla każdego $k=1, \dots, |T_{init}|-1$, $T_k$ jest najlepszym poddrzewem $T_{init}$ z $k$ węzłami terminalnymi zgodnie z funkcją straty dewiancji.

Naturalne jest używanie dewiancji do porównywania poddrzew o tej samej liczbie węzłów terminalnych. Jednakże dewiancja nie jest pomocna przy porównywaniu poddrzew $T_{init}, T_{|T_{init}|-1}, T_{|T_{init}|-2}, \dots, T_1$. Rzeczywiście, jak zauważono w Sekcji 3.2.3, jeśli $T_k$ jest poddrzewem $T'_{k+1} \in \mathcal{C}(T_{init}, k+1)$, to z konieczności mamy

$$D((\hat{c}_t)_{t \in \mathcal{T}_{(T_k)}}) \ge D((\hat{c}_t)_{t \in \mathcal{T}_{(T'_{k+1})}}) \ge D((\hat{c}_t)_{t \in \mathcal{T}_{(T_{k+1})}}).$$

Dlatego wybór najlepszego poddrzewa $T_{prune}$ spośród sekwencji 

$$T_{init}, T_{|T_{init}|-1}, T_{|T_{init}|-2}, \dots, T_1,$$

który opiera się na dewiancji, czyli na estymatorze błędu generalizacji na próbie uczącej, zawsze doprowadzi do największego drzewa $T_{init}$. 

Dlatego błędy generalizacji przyciętych poddrzew 

$$T_{init}, T_{|T_{init}|-1}, T_{|T_{init}|-2}, \dots, T_1$$

powinny być estymowane na zbiorze walidacyjnym w celu określenia $T_{prune}$. Wybór $T_{prune}$ jest również często dokonywany za pomocą walidacji krzyżowej, jak zobaczymy w dalszej części.

Taka intuicyjna procedura konstruowania sekwencji drzew 

$$T_{init}, T_{|T_{init}|-1}, T_{|T_{init}|-2}, \dots, T_1$$

ma pewne wady. Jedną z nich jest tworzenie poddrzew $T_{init}$, które nie są zagnieżdżone, co oznacza, że poddrzewo $T_k$ niekoniecznie jest poddrzewem $T_{k+1}$. Zatem węzeł $t$ z $T_{init}$ może ponownie pojawić się w drzewie $T_k$, podczas gdy został odcięty w drzewie $T_{k+1}$. 

Dlatego zazwyczaj preferowane jest przycinanie metodą minimalnego kosztu-złożoności, przedstawione poniżej.