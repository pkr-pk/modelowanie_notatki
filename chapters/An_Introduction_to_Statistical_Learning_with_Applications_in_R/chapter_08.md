# Rozdział 8: Metody oparte na drzewach

## 8.1 Podstawy drzew decyzyjnych

### 8.1.2 Drzewa klasyfikacyjne

**Drzewo klasyfikacyjne** jest bardzo podobne do drzewa regresyjnego, z tą różnicą, że służy do przewidywania odpowiedzi jakościowej, a nie ilościowej. Przypomnijmy, że w przypadku drzewa regresyjnego przewidywana odpowiedź dla obserwacji jest podawana jako średnia odpowiedź obserwacji uczących, które należą do tego samego węzła końcowego. W przeciwieństwie do tego, w przypadku drzewa klasyfikacyjnego przewidujemy, że każda obserwacja należy do **najczęściej występującej klasy** obserwacji uczących w regionie, do którego należy. Interpretując wyniki drzewa klasyfikacyjnego, często interesuje nas nie tylko predykcja klasy odpowiadająca danemu regionowi węzła końcowego, ale także **proporcje klas** wśród obserwacji uczących, które do tego regionu wpadają.

Zadanie budowy drzewa klasyfikacyjnego jest bardzo podobne do zadania budowy drzewa regresyjnego. Podobnie jak w przypadku regresji, używamy **rekurencyjnego dzielenia binarnego** do budowy drzewa klasyfikacyjnego. Jednak w przypadku klasyfikacji, RSS nie może być użyte jako kryterium do dokonywania podziałów binarnych. Naturalną alternatywą dla RSS jest **współczynnik błędu klasyfikacji**. Ponieważ planujemy przypisać obserwację w danym regionie do najczęściej występującej klasy obserwacji uczących w tym regionie, współczynnik błędu klasyfikacji jest po prostu ułamkiem obserwacji uczących w tym regionie, które nie należą do najczęściej występującej klasy:

$$E = 1 - \max_{k}(\hat{p}_{mk})$$ 

Tutaj $\hat{p}_{mk}$ reprezentuje proporcję obserwacji uczących w $m$-tym regionie, które pochodzą z $k$-tej klasy. Okazuje się jednak, że błąd klasyfikacji nie jest wystarczająco czuły do budowy drzewa, a w praktyce preferowane są dwie inne miary.

**Indeks Giniego** jest zdefiniowany przez

$$G = \sum_{k=1}^{K} \hat{p}_{mk}(1 - \hat{p}_{mk})$$ 

będąc miarą całkowitej wariancji dla wszystkich $K$ klas. Nietrudno zauważyć, że indeks Giniego przyjmuje małą wartość, jeśli wszystkie $\hat{p}_{mk}$ są bliskie zeru lub jedności. Z tego powodu indeks Giniego jest określany jako miara **czystości węzła** — mała wartość wskazuje, że węzeł zawiera głównie obserwacje z jednej klasy.

Alternatywą dla indeksu Giniego jest **entropia**, dana przez

$$D = - \sum_{k=1}^{K} \hat{p}_{mk} \log \hat{p}_{mk}$$ 

Ponieważ $0 \le \hat{p}_{mk} \le 1$, wynika z tego, że $0 \le -\hat{p}_{mk} \log \hat{p}_{mk}$. Można pokazać, że entropia przyjmie wartość bliską zeru, jeśli $\hat{p}_{mk}$ są wszystkie bliskie zeru lub jedności. Dlatego, podobnie jak indeks Giniego, entropia przyjmie małą wartość, jeśli $m$-ty węzeł jest czysty. W rzeczywistości okazuje się, że indeks Giniego i entropia są liczbowo dość podobne.

Podczas budowy drzewa klasyfikacyjnego do oceny jakości danego podziału zwykle używa się indeksu Giniego lub entropii, ponieważ te dwa podejścia są bardziej wrażliwe na czystość węzła niż współczynnik błędu klasyfikacji. Każde z tych trzech podejść może być użyte podczas przycinania drzewa, ale współczynnik błędu klasyfikacji jest preferowany, jeśli celem jest dokładność predykcji końcowego przyciętego drzewa.

Rysunek 8.6 przedstawia przykład na zbiorze danych `Heart`. Dane te zawierają binarny wynik `HD` dla 303 pacjentów, którzy zgłosili się z bólem w klatce piersiowej. Wartość `Yes` wskazuje na obecność choroby serca na podstawie testu angiograficznego, podczas gdy `No` oznacza brak choroby serca. Jest 13 predyktorów, w tym `Age`, `Sex`, `Chol` (pomiar cholesterolu) oraz inne pomiary funkcji serca i płuc. Walidacja krzyżowa skutkuje drzewem z sześcioma węzłami końcowymi.

W naszej dotychczasowej dyskusji zakładaliśmy, że zmienne predykcyjne przyjmują wartości ciągłe. Jednak drzewa decyzyjne można budować nawet w obecności jakościowych zmiennych predykcyjnych. Na przykład w danych `Heart` niektóre predyktory, takie jak `Sex`, `Thal` (test wysiłkowy z talem) i `ChestPain`, są jakościowe. Dlatego podział na jednej z tych zmiennych polega na przypisaniu niektórych wartości jakościowych do jednej gałęzi, a pozostałych do drugiej gałęzi. Na rysunku 8.6 niektóre z węzłów wewnętrznych odpowiadają podziałowi zmiennych jakościowych. Na przykład górny węzeł wewnętrzny odpowiada podziałowi `Thal`. Tekst `Thal:a` wskazuje, że lewa gałąź wychodząca z tego węzła składa się z obserwacji z pierwszą wartością zmiennej `Thal` (normalna), a prawy węzeł składa się z pozostałych obserwacji (wady stałe lub odwracalne). Tekst `ChestPain:bc` dwa podziały niżej po lewej stronie wskazuje, że lewa gałąź wychodząca z tego węzła składa się z obserwacji z drugą i trzecią wartością zmiennej `ChestPain`, gdzie możliwe wartości to typowa dławica piersiowa, nietypowa dławica piersiowa, ból niedławicowy i bezobjawowy.

Rysunek 8.6 ma zaskakującą cechę: niektóre podziały dają dwa węzły końcowe, które mają tę samą przewidywaną wartość. Rozważmy na przykład podział `RestECG<1` w prawym dolnym rogu nieprzyciętego drzewa. Niezależnie od wartości `RestECG`, dla tych obserwacji przewidywana jest wartość `Yes`. Dlaczego więc w ogóle dokonuje się tego podziału? Podział jest dokonywany, ponieważ prowadzi do zwiększenia czystości węzła. To znaczy, wszystkie 9 obserwacji odpowiadających prawemu liściowi ma wartość odpowiedzi `Yes`, podczas gdy 7/11 tych odpowiadających lewemu liściowi ma wartość odpowiedzi `Yes`. Dlaczego czystość węzła jest ważna? Załóżmy, że mamy obserwację testową, która należy do regionu określonego przez ten prawy liść. Wtedy możemy być prawie pewni, że jej wartość odpowiedzi to `Yes`. W przeciwieństwie do tego, jeśli obserwacja testowa należy do regionu określonego przez lewy liść, jej wartość odpowiedzi to prawdopodobnie `Yes`, ale jesteśmy znacznie mniej pewni. Mimo że podział `RestECG<1` nie zmniejsza błędu klasyfikacji, poprawia indeks Giniego i entropię, które są bardziej wrażliwe na czystość węzła.