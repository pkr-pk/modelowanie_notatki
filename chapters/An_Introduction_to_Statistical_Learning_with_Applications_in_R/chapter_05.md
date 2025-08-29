# Rozdział 5: Resampling Methods

## 5.1 Cross-Validation

### 5.1.2 Leave-One-Out Cross-Validation

Sprawdzian krzyżowy z pominięciem jednej obserwacji (Leave-One-Out Cross-Validation, **LOOCV**) jest ściśle powiązany z podejściem zbioru walidacyjnego z sekcji 5.1.1, ale próbuje zaradzić wadom tej metody.

Podobnie jak podejście ze zbiorem walidacyjnym, LOOCV polega na podziale zbioru obserwacji na dwie części. Jednak zamiast tworzyć dwa podzbiory o porównywalnej wielkości, jako zbiór walidacyjny używana jest pojedyncza obserwacja $(x_1, y_1)$, a pozostałe obserwacje $\{(x_2, y_2),...,(x_n, y_n)\}$ tworzą zbiór uczący. Metoda uczenia statystycznego jest dopasowywana na $n − 1$ obserwacjach uczących, a dla wykluczonej obserwacji dokonywana jest predykcja $\hat{y}_1$, używając jej wartości $x_1$. Ponieważ $(x_1, y_1)$ nie była używana w procesie dopasowywania, $MSE_1 = (y_1 − \hat{y}_1)^2$ dostarcza w przybliżeniu nieobciążonej estymacji błędu testowego. Jednakże, chociaż $MSE_1$ jest nieobciążony dla błędu testowego, jest to słaba estymacja, ponieważ ma dużą zmienność, jako że opiera się na pojedynczej obserwacji $(x_1, y_1)$.

Możemy powtórzyć procedurę, wybierając $(x_2, y_2)$ jako dane walidacyjne, ucząc procedurę statystyczną na $n − 1$ obserwacjach $\{(x_1, y_1),(x_3, y_3),...,(x_n, y_n)\}$ i obliczając $MSE_2 = (y_2−\hat{y}_2)^2$. Powtarzanie tego podejścia $n$ razy daje $n$ błędów kwadratowych, $MSE_1,..., MSE_n$. Estymacja LOOCV dla testowego MSE jest średnią z tych $n$ estymacji błędu testowego:

$$CV_{(n)} = \frac{1}{n}\sum_{i=1}^{n} MSE_i. \quad (5.1)$$

Schemat podejścia LOOCV jest zilustrowany na rysunku 5.3.

LOOCV ma kilka istotnych zalet w porównaniu z podejściem zbioru walidacyjnego. Po pierwsze, ma znacznie mniejsze obciążenie. W LOOCV wielokrotnie dopasowujemy metodę uczenia statystycznego, używając zbiorów uczących zawierających $n − 1$ obserwacji, czyli prawie tyle samo, co w całym zbiorze danych. Jest to przeciwieństwo podejścia ze zbiorem walidacyjnym, w którym zbiór uczący stanowi zazwyczaj około połowy oryginalnego zbioru danych. W konsekwencji podejście LOOCV ma tendencję do niezawyżania stopy błędu testowego w takim stopniu, jak robi to podejście ze zbiorem walidacyjnym. Po drugie, w przeciwieństwie do podejścia walidacyjnego, które da różne wyniki przy wielokrotnym stosowaniu z powodu losowości w podziałach na zbiory uczące/walidacyjne, wielokrotne wykonanie LOOCV zawsze da te same wyniki: nie ma losowości w podziałach na zbiory uczące/walidacyjne.

Użyliśmy LOOCV na zbiorze danych `Auto`, aby uzyskać estymację testowego MSE wynikającego z dopasowania modelu regresji liniowej do przewidywania `mpg` przy użyciu funkcji wielomianowych `horsepower`. Wyniki są pokazane na lewym panelu rysunku 5.4.

LOOCV może być kosztowny obliczeniowo do wdrożenia, ponieważ model musi być dopasowywany $n$ razy. Może to być bardzo czasochłonne, jeśli $n$ jest duże, a dopasowanie każdego indywidualnego modelu jest powolne. W przypadku regresji liniowej lub wielomianowej metodą najmniejszych kwadratów, niesamowity skrót sprawia, że koszt LOOCV jest taki sam, jak dopasowanie pojedynczego modelu! Prawdziwa jest następująca formuła:

$$CV_{(n)} = \frac{1}{n}\sum_{i=1}^{n}\left(\frac{y_i - \hat{y}_i}{1 - h_i}\right)^2, \quad (5.2)$$

gdzie $\hat{y}_i$ to i-ta dopasowana wartość z oryginalnego dopasowania metodą najmniejszych kwadratów, a $h_i$ to dźwignia (leverage) zdefiniowana w (3.37) na stronie 99. To jest jak zwykłe MSE, z tym że i-ta reszta jest dzielona przez $1 − h_i$. Dźwignia leży między $1/n$ a 1 i odzwierciedla, w jakim stopniu obserwacja wpływa na własne dopasowanie. Stąd reszty dla punktów o dużej dźwigni są w tej formule zawyżone o dokładnie taką wielkość, aby ta równość była zachowana.

LOOCV jest bardzo ogólną metodą i może być używana z każdym rodzajem modelowania predykcyjnego. Na przykład, moglibyśmy użyć jej z regresją logistyczną lub liniową analizą dyskryminacyjną, lub z dowolną z metod omówionych w późniejszych rozdziałach. Magiczna formuła (5.2) nie jest ogólnie prawdziwa, w którym to przypadku model musi być dopasowywany $n$ razy.