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

Rysunek 8.6 ma zaskakującą cechę: niektóre podziały dają dwa węzły końcowe, które mają tę samą przewidywaną wartość. Rozważmy na przykład podział `RestECG < 1` w prawym dolnym rogu nieprzyciętego drzewa. Niezależnie od wartości `RestECG`, dla tych obserwacji przewidywana jest wartość `Yes`. Dlaczego więc w ogóle dokonuje się tego podziału? Podział jest dokonywany, ponieważ prowadzi do zwiększenia czystości węzła. To znaczy, wszystkie 9 obserwacji odpowiadających prawemu liściowi ma wartość odpowiedzi `Yes`, podczas gdy 7/11 tych odpowiadających lewemu liściowi ma wartość odpowiedzi `Yes`. Dlaczego czystość węzła jest ważna? Załóżmy, że mamy obserwację testową, która należy do regionu określonego przez ten prawy liść. Wtedy możemy być prawie pewni, że jej wartość odpowiedzi to `Yes`. W przeciwieństwie do tego, jeśli obserwacja testowa należy do regionu określonego przez lewy liść, jej wartość odpowiedzi to prawdopodobnie `Yes`, ale jesteśmy znacznie mniej pewni. Mimo że podział `RestECG < 1` nie zmniejsza błędu klasyfikacji, poprawia indeks Giniego i entropię, które są bardziej wrażliwe na czystość węzła.

## 8.2 Bagging, Lasy Losowe, Boosting i Bayesowskie Addytywne Drzewa Regresyjne

**Metoda zespołowa** (ensemble method) to podejście, które łączy wiele prostych modeli "budulcowych" w celu uzyskania jednego, potencjalnie bardzo potężnego modelu. Te proste modele budulcowe są czasami nazywane **słabymi uczniami** (weak learners), ponieważ same w sobie mogą prowadzić do przeciętnych predykcji.

Teraz omówimy **bagging**, **lasy losowe**, **boosting** i **Bayesowskie addytywne drzewa regresyjne**. Są to metody zespołowe, dla których prostym modelem budulcowym jest drzewo regresyjne lub klasyfikacyjne.

---

### 8.2.1 Bagging

Bootstrap, wprowadzony w Rozdziale 5, jest niezwykle potężną ideą. Stosuje się go w wielu sytuacjach, w których trudno lub nawet niemożliwe jest bezpośrednie obliczenie odchylenia standardowego interesującej nas wielkości. Zobaczymy tutaj, że bootstrap można wykorzystać w zupełnie innym kontekście, w celu ulepszenia metod uczenia statystycznego, takich jak drzewa decyzyjne.

Drzewa decyzyjne omówione w Sekcji 8.1 cierpią z powodu wysokiej wariancji. Oznacza to, że jeśli losowo podzielimy dane treningowe na dwie części i dopasujemy drzewo decyzyjne do obu połówek, wyniki, które uzyskamy, mogą być zupełnie inne. W przeciwieństwie do tego, procedura o niskiej wariancji da podobne wyniki, jeśli zostanie wielokrotnie zastosowana do różnych zbiorów danych; regresja liniowa ma tendencję do niskiej wariancji, jeśli stosunek n do p jest umiarkowanie duży. **Agregacja bootstrapowa**, czyli **bagging**, jest procedurą ogólnego przeznaczenia do redukcji wariancji metody uczenia statystycznego; wprowadzamy ją tutaj, ponieważ jest szczególnie użyteczna i często stosowana w kontekście drzew decyzyjnych.

Przypomnijmy, że dla zbioru n niezależnych obserwacji $Z_1,...,Z_n$, każda z wariancją $\sigma^2$, wariancja średniej $\bar{Z}$ z obserwacji jest dana przez $\sigma^2/n$. Innymi słowy, uśrednianie zbioru obserwacji zmniejsza wariancję. Stąd naturalnym sposobem na zmniejszenie wariancji i zwiększenie dokładności na zbiorze testowym metody uczenia statystycznego jest pobranie wielu zbiorów treningowych z populacji, zbudowanie osobnego modelu predykcyjnego na każdym zbiorze treningowym i uśrednienie wyników predykcji. Innymi słowy, moglibyśmy obliczyć $\hat{f}^1(x), \hat{f}^2(x),..., \hat{f}^B(x)$ używając B oddzielnych zbiorów treningowych, a następnie uśrednić je, aby uzyskać pojedynczy model uczenia statystycznego o niskiej wariancji, dany przez

$$\hat{f}_{avg}(x) = \frac{1}{B} \sum_{b=1}^{B} \hat{f}^b(x).$$

Oczywiście, jest to niepraktyczne, ponieważ generalnie nie mamy dostępu do wielu zbiorów treningowych. Zamiast tego możemy zastosować bootstrap, pobierając wielokrotnie próbki z (jednego) zbioru danych treningowych. W tym podejściu generujemy B różnych bootstrapowych zbiorów danych treningowych. Następnie trenujemy naszą metodę na b-tym bootstrapowym zbiorze treningowym, aby uzyskać $\hat{f}^{*b}(x)$, i na koniec uśredniamy wszystkie predykcje, aby uzyskać

$$\hat{f}_{bag}(x) = \frac{1}{B} \sum_{b=1}^{B} \hat{f}^{*b}(x).$$

To się nazywa **bagging**.

Chociaż bagging może poprawić predykcje dla wielu metod regresji, jest szczególnie użyteczny dla drzew decyzyjnych. Aby zastosować bagging do drzew regresyjnych, po prostu konstruujemy B drzew regresyjnych, używając B bootstrapowych zbiorów treningowych, i uśredniamy wynikowe predykcje. Te drzewa są głęboko rozwijane i nie są przycinane. Stąd każde pojedyncze drzewo ma wysoką wariancję, ale niską obciążenie. Uśrednianie tych B drzew zmniejsza wariancję. Wykazano, że bagging daje imponującą poprawę dokładności, łącząc setki, a nawet tysiące drzew w jedną procedurę.

Do tej pory opisaliśmy procedurę baggingu w kontekście regresji, aby przewidzieć ilościową zmienną wynikową Y. Jak można rozszerzyć bagging na problem klasyfikacyjny, gdzie Y jest jakościowe? W tej sytuacji istnieje kilka możliwych podejść, ale najprostsze jest następujące. Dla danej obserwacji testowej możemy zapisać klasę przewidzianą przez każde z B drzew i przeprowadzić **głosowanie większościowe**: ogólną predykcją jest najczęściej występująca klasa spośród B predykcji.

Rysunek 8.8 pokazuje wyniki z zastosowania baggingu drzew na danych Heart. Stopa błędu testowego jest pokazana jako funkcja B, liczby drzew skonstruowanych przy użyciu bootstrapowych zbiorów danych treningowych. Widzimy, że w tym przypadku stopa błędu testowego baggingu jest nieco niższa niż stopa błędu testowego uzyskana z pojedynczego drzewa. Liczba drzew B nie jest krytycznym parametrem w baggingu; użycie bardzo dużej wartości B nie doprowadzi do przeuczenia. W praktyce używamy wartości B wystarczająco dużej, aby błąd się ustabilizował. Użycie B = 100 jest wystarczające, aby osiągnąć dobrą wydajność w tym przykładzie.

**Szacowanie Błędu Out-of-Bag**

Okazuje się, że istnieje bardzo prosty sposób na oszacowanie błędu testowego modelu z baggingiem, bez konieczności przeprowadzania walidacji krzyżowej czy podejścia z walidacją na oddzielnym zbiorze. Przypomnijmy, że kluczem do baggingu jest to, że drzewa są wielokrotnie dopasowywane do bootstrapowych podzbiorów obserwacji. Można wykazać, że średnio każde drzewo z baggingu wykorzystuje około dwóch trzecich obserwacji. Pozostała jedna trzecia obserwacji, które nie zostały użyte do dopasowania danego drzewa z baggingu, nazywana jest obserwacjami **out-of-bag (OOB)**. Możemy przewidzieć odpowiedź dla i-tej obserwacji, używając każdego z drzew, w których ta obserwacja była OOB. Da to około B/3 predykcji dla i-tej obserwacji. Aby uzyskać pojedynczą predykcję dla i-tej obserwacji, możemy uśrednić te przewidywane odpowiedzi (jeśli celem jest regresja) lub przeprowadzić głosowanie większościowe (jeśli celem jest klasyfikacja). Prowadzi to do pojedynczej predykcji OOB dla i-tej obserwacji. Predykcję OOB można uzyskać w ten sposób dla każdej z n obserwacji, z których można obliczyć ogólny błąd średniokwadratowy OOB (dla problemu regresji) lub błąd klasyfikacji (dla problemu klasyfikacji). Wynikowy błąd OOB jest prawidłowym oszacowaniem błędu testowego dla modelu z baggingiem, ponieważ odpowiedź dla każdej obserwacji jest przewidywana przy użyciu tylko tych drzew, które nie były dopasowywane przy użyciu tej obserwacji. Rysunek 8.8 pokazuje błąd OOB na danych Heart. Można wykazać, że przy wystarczająco dużym B, błąd OOB jest praktycznie równoważny błędowi walidacji krzyżowej typu leave-one-out. Podejście OOB do szacowania błędu testowego jest szczególnie wygodne podczas przeprowadzania baggingu na dużych zbiorach danych, dla których walidacja krzyżowa byłaby obliczeniowo uciążliwa.

**Miary Ważności Zmiennych**

Jak już wspomniano, bagging zazwyczaj skutkuje poprawą dokładności w porównaniu z predykcją przy użyciu pojedynczego drzewa. Niestety, interpretacja wynikowego modelu może być trudna. Przypomnijmy, że jedną z zalet drzew decyzyjnych jest atrakcyjny i łatwy do interpretacji diagram, taki jak ten przedstawiony na rysunku 8.1. Jednakże, gdy stosujemy bagging z dużą liczbą drzew, nie jest już możliwe przedstawienie wynikowej procedury uczenia statystycznego za pomocą pojedynczego drzewa, i nie jest już jasne, które zmienne są najważniejsze dla procedury. Zatem bagging poprawia dokładność predykcji kosztem interpretowalności.

Chociaż zbiór drzew z baggingu jest znacznie trudniejszy do zinterpretowania niż pojedyncze drzewo, można uzyskać ogólne podsumowanie ważności każdego predyktora, używając RSS (dla drzew regresyjnych z baggingiem) lub indeksu Giniego (dla drzew klasyfikacyjnych z baggingiem). W przypadku drzew regresyjnych z baggingiem możemy zapisać całkowitą wielkość, o jaką RSS (8.1) jest zmniejszany przez podziały na danym predyktorze, uśrednioną po wszystkich B drzewach. Duża wartość wskazuje na ważny predyktor. Podobnie, w kontekście drzew klasyfikacyjnych z baggingiem, możemy zsumować całkowitą wielkość, o jaką indeks Giniego (8.6) jest zmniejszany przez podziały na danym predyktorze, uśrednioną po wszystkich B drzewach.

Graficzne przedstawienie **ważności zmiennych** w danych `Heart` pokazano na Rysunku 8.9. Widzimy średni spadek indeksu Giniego dla każdej zmiennej, w odniesieniu do największego. Zmiennymi o największym średnim spadku indeksu Giniego są `Thal`, `Ca` i `ChestPain`.

