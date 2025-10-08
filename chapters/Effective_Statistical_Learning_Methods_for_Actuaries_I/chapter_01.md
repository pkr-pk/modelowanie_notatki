# Rozdział 1: Klasyfikacja ryzyka ubezpieczeniowego

## 1.1 Wprowadzenie

W przeciwieństwie do innych branż, działalność ubezpieczeniowa nie polega na sprzedaży produktów jako takich, ale obietnic. Polisa ubezpieczeniowa to nic innego jak obietnica złożona przez ubezpieczyciela ubezpieczającemu, że pokryje on szkody osób trzecich lub wypłaci odszkodowanie za jego własne przyszłe straty spowodowane wystąpieniem niekorzystnych zdarzeń losowych, w zamian za składkę płatną z góry.

W przypadku wypadku, choroby lub śmierci, ubezpieczający lub beneficjent może zgłosić roszczenie do towarzystwa ubezpieczeniowego, prosząc o odszkodowanie w ramach wykonania umowy ubezpieczenia. Roszczenie ubezpieczeniowe można formalnie zdefiniować jako żądanie skierowane do towarzystwa ubezpieczeniowego o objęcie ochroną lub odszkodowanie za finansowe konsekwencje niebezpieczeństwa objętego polisą ubezpieczeniową. Towarzystwo ubezpieczeniowe obsługuje roszczenie od momentu zgłoszenia do ostatecznego rozliczenia (lub zamknięcia) i po zatwierdzeniu, dokonuje wypłaty na rzecz ubezpieczonego lub osoby trzeciej w imieniu ubezpieczonego.

W konsekwencji, w momencie sprzedaży polisy ubezpieczeniowej, dostawca nie zna ostatecznych kosztów tej usługi, ale opiera się na danych historycznych i modelach aktuarialnych, aby przewidzieć zrównoważoną cenę dla swojego produktu. Centralną częścią tej ceny jest składka czysta, czyli najlepsze oszacowanie lub oczekiwana (obecna) wartość przyszłych zobowiązań.

W tym rozdziale wprowadzającym dokonujemy przeglądu podstawowych koncepcji taryfikacji aktuarialnej, które będą używane w całej tej książce. Starannie przygotowujemy grunt i wprowadzamy ogólne zasady leżące u podstaw modelowania szkodowości ubezpieczeniowej i analizy danych szkodowych.

## 1.2 Taryfa Techniczna a Taryfa Komercyjna

### 1.2.1 Cena Sprzedaży a Koszt Operacyjny
Taryfa techniczna ma na celu jak najdokładniejsze oszacowanie składki czystej dla każdego ubezpieczającego i stanowi punkt odniesienia dla wewnętrznej oceny ryzyka. Uwzględnia ona wszystkie dostępne informacje o ryzyku.

Zamiast rozważać obserwowane koszty szkód, które zawierają duży element losowości, aktuariusze biorą pod uwagę łączny koszt operacyjny dla każdej polisy jako cenę techniczną. Zaletą spojrzenia na modelowany koszt zamiast na koszt rzeczywisty jest usunięcie losowych fluktuacji w obserwowanym doświadczeniu. Ponadto, zdecydowana większość obserwowanych kosztów jest zerowa, ponieważ nie zgłoszono żadnych roszczeń w odniesieniu do odpowiednich polis, ale jasne jest, że cena produktu musi być dodatnia. Należy zauważyć, że modelowany koszt może obejmować korekty z taryfikacji opartej na doświadczeniu/wiarygodności, jeśli historia szkodowa wskazuje na przyszłe koszty.

Łączny koszt operacyjny to suma:
- modelowanego oczekiwanego kosztu szkody na polisę;
- kosztów bieżących (personel, budynki itp.);
- dodatku bezpieczeństwa zapewniającego wypłacalność i koszty reasekuracji;
- a także zysku akcjonariuszy.

Łączny wskaźnik operacyjny odpowiada łącznemu kosztowi operacyjnemu podzielonemu przez składkę komercyjną netto (tj. cenę sprzedaży, po odjęciu podatków i prowizji pośredników). Różni się to od wskaźników szkodowości, czyli stosunku wartości rzeczywistych do oczekiwanych, które są generalnie nieinformatywne na poziomie indywidualnej polisy (ale istotne dla bloków biznesu lub w ubezpieczeniach komercyjnych, gdzie pojedyncza polisa może obejmować całą firmę z dużą ekspozycją na ryzyko).

Istnieje zatem różnica między składką techniczną opartą na kosztach a składką komercyjną opartą na popycie, faktycznie pobieraną od ubezpieczających. Ta pierwsza musi prawidłowo oceniać ryzyko dla każdego ubezpieczającego zgodnie z indywidualnymi cechami, podczas gdy ta druga ma na celu optymalizację zysku ubezpieczyciela. Ubezpieczenia są działalnością silnie regulowaną, więc składki komercyjne muszą być zgodne z zasadami narzuconymi przez lokalny organ nadzoru. Takie zasady generalnie mają dwa cele: po pierwsze, zapewnienie wypłacalności ubezpieczyciela (pamiętajmy, że ubezpieczyciele sprzedają obietnice), a po drugie, ochrona klientów poprzez zakaz nieuczciwego traktowania (lub traktowania postrzeganego jako nieuczciwe).

Oprócz zgodności z przepisami prawnymi i regulacyjnymi, ograniczenia IT lub kanały sprzedaży mogą również poważnie ograniczać taryfę komercyjną. Zmiany programistyczne w systemie IT mają swój koszt, nie wspominając o pewnym stopniu konserwatyzmu osób odpowiedzialnych. Ponadto, jeśli produkty są sprzedawane przez brokerów lub agentów, muszą być dla nich zrozumiałe i odpowiadać ich potrzebom.

Taryfy komercyjne silnie zależą również od formy organizacyjnej towarzystwa ubezpieczeniowego, czy jest to ubezpieczyciel wzajemny czy akcyjny/komercyjny. Ubezpieczyciele wzajemni są własnością ubezpieczających i generalnie czują się komfortowo z większą solidarnością wśród ubezpieczonych. Jednak ich wycena techniczna musi być jeszcze bardziej precyzyjna, aby zapobiec negatywnej selekcji.

### 1.2.2 Czynnik Ryzyka a Czynnik Taryfikacyjny

Klasyfikacja ryzyka opiera się na czynnikach ryzyka. Na przykład w ubezpieczeniach komunikacyjnych czynniki ryzyka zazwyczaj obejmują wiek, płeć i zawód ubezpieczającego, typ i sposób użytkowania samochodu, miejsce zamieszkania, a czasami nawet liczbę samochodów w gospodarstwie domowym lub stan cywilny. Te obserwowalne cechy ryzyka są zazwyczaj postrzegane jako nielosowe, a analiza jest prowadzona warunkowo od ich wartości (chociaż rozpoznanie ich losowej natury jest przydatne w niektórych opracowaniach metodologicznych).

Jak wyjaśniono powyżej, składki komercyjne mogą różnić się od składek technicznych opartych na kosztach, obliczanych przez aktuariuszy, z powodu regulacji lub pozycji firmy w stosunku do jej konkurentów rynkowych. Składki komercyjne zależą od zestawu czynników taryfikacyjnych. Te czynniki taryfikacyjne stanowią podzbiór czynników ryzyka, czasami uproszczony. Poniższe przykłady pomagają zrozumieć różnicę między czynnikami ryzyka a czynnikami taryfikacyjnymi.

**Przykład 1.2.1 (Płeć w Unii Europejskiej)** Płeć jest generalnie istotnym czynnikiem ryzyka i dlatego musi być uwzględniona w taryfie technicznej. Jednakże płeć musi być wyłączona z taryfy komercyjnej w Unii Europejskiej, w zastosowaniu dyrektyw europejskich zakazujących wszelkiej dyskryminacji między obywatelami płci męskiej i żeńskiej. Zatem płeć jest czynnikiem ryzyka, ale nie czynnikiem taryfikacyjnym w Unii Europejskiej.

**Przykład 1.2.2 (Status palacza)** Status palacza jest czynnikiem ryzyka w ubezpieczeniach na życie, ponieważ jest dobrze udokumentowane, że oczekiwana dalsza długość życia jest znacznie krótsza dla palaczy w porównaniu z osobami niepalącymi. W przypadku świadczeń na wypadek śmierci, ten czynnik ryzyka jest często używany jako czynnik taryfikacyjny: palacze zazwyczaj płacą wyższą składkę za tę samą kwotę świadczenia z tytułu śmierci w ubezpieczeniu na życie na czas określony, na przykład. Jednak w przypadku świadczeń na dożycie (np. renty kapitałowe lub dożywotnie), palacze nie zawsze otrzymują zniżki składkowe, pomimo ich skróconej oczekiwanej dalszej długości życia. Zatem status palacza może być zarówno czynnikiem ryzyka, jak i czynnikiem taryfikacyjnym, lub tylko czynnikiem ryzyka, w zależności od produktu i decyzji handlowych podjętych przez dostawcę ubezpieczenia.

**Przykład 1.2.3 (Wycena geograficzna)** W ubezpieczeniach mienia, takich jak ubezpieczenie od ognia dla gospodarstw domowych, powszechną praktyką jest, aby składka za ryzyko na jednostkę ekspozycji zmieniała się w zależności od obszaru geograficznego, przy zachowaniu stałości wszystkich innych czynników ryzyka. W ubezpieczeniach komunikacyjnych większość firm przyjęła klasyfikację ryzyka w zależności od strefy geograficznej, w której mieszka ubezpieczający (na przykład miejskiej, podmiejskiej lub wiejskiej). Zmienność przestrzenna może być związana z czynnikami geograficznymi (takimi jak natężenie ruchu lub bliskość dróg głównych w ubezpieczeniach komunikacyjnych) lub czynnikami społeczno-demograficznymi (być może wpływającymi na wskaźniki kradzieży w ubezpieczeniach domów). Składka techniczna odzwierciedla zmienność przestrzenną w doświadczeniu szkodowym, biorąc pod uwagę lokalizację geograficzną zawartą w kodzie pocztowym.³ W związku z tym, efekt geograficzny uzyskany z taryfy technicznej jest zazwyczaj upraszczany przed jego integracją z taryfą komercyjną, co generuje różnicę między lokalizacją geograficzną jako ryzykiem a jako czynnikiem taryfikacyjnym.

W tej książce omawiamy jedynie ustalanie składek technicznych opartych na kosztach, uwzględniających wszystkie istotne informacje w celu:
- przeciwdziałania selekcji negatywnej (antyselakcji),
- monitorowania transferów składek powstających w portfelu w wyniku zastosowania taryfy komercyjnej, oraz
- oceny wartości klienta poprzez porównanie składek technicznych z komercyjnymi.

To tylko trzy przykłady użyteczności śledzenia składek technicznych obok składek komercyjnych.

### 1.2.3 Wycena "od góry do dołu"

Aktuariusze zazwyczaj łączą dwa podejścia:
- analizę przeprowadzaną na poziomie indywidualnej polisy
- uzupełnioną o analizę zbiorową na poziomie portfela.

Wynika to z faktu, że polisy ubezpieczeniowej nigdy nie można rozpatrywać w izolacji, ale zawsze w powiązaniu z portfelem, w którym się znajduje (dywersyfikacja uzyskana poprzez agregację indywidualnych ryzyk w portfele jest sercem działalności ubezpieczeniowej). Reasekuracja lub koasekuracja mogą stanowić substytut dywersyfikacji ryzyka dla portfeli o ograniczonym wolumenie.

Ponieważ ubezpieczyciele sprzedają obietnice, działalność ubezpieczeniowa zawsze była silnie regulowana w celu zapewnienia wypłacalności, tj. zdolności ubezpieczyciela do spłaty przyszłych roszczeń. Dlatego organy regulacyjne nakładają na ubezpieczycieli wymóg posiadania wystarczającego kapitału, aby mogli oni wypełnić swoje obietnice wobec ubezpieczających. Kapitał ten można obliczyć tylko na poziomie portfela, uwzględniając dywersyfikację ryzyka między polisami.

Dokładniej, wycena "od góry do dołu" przebiega następująco. Najpierw na poziomie portfela sporządzana jest prognoza, aby przewidzieć ogólne przyszłe koszty szkód, biorąc pod uwagę cykle rynkowe, inflację specyficzną dla szkód, dynamikę rezerw szkodowych i tak dalej. Za pomocą odpowiedniej miary ryzyka określa się następnie kwotę kapitału ryzyka, co prowadzi do składki portfelowej. Należy zauważyć, że portfel jest w tej analizie traktowany jako zbiorowość, a szczegółowe indywidualne cechy polis zazwyczaj nie są brane pod uwagę.

Następnie przeprowadza się analizę indywidualną w celu zidentyfikowania profili ryzyka. Wynik tego drugiego etapu jest wykorzystywany do alokacji wynikowego całkowitego dochodu ze składek między różne polisy: określa się względne ryzyko każdej polisy (w odniesieniu do pewnej polisy referencyjnej) i stosuje się wynikowy procent do składki portfelowej.

W tej książce rozważamy tylko analizę indywidualną przeprowadzaną na poziomie polisy w celu zidentyfikowania istotnych czynników ryzyka, a nie ocenę odpowiedniego poziomu kapitału potrzebnego do zapewnienia wypłacalności.

### 1.2.4 Prewencja

Oprócz przewidywania kosztów, wycena techniczna jest również pomocna w prewencji. Identyfikując czynniki ryzyka i zachowania podatne na wypadki (na przykład za kierownicą, z pomocą urządzeń telematycznych), ubezpieczyciele mogą informować ubezpieczających i pomagać im zmniejszyć częstotliwość i/lub dotkliwość szkód.

W kilku krajach ubezpieczyciele ujawniają teraz część swojej analizy technicznej, aby pokazać, że ich taryfa odpowiednio odzwierciedla doświadczenie szkodowe. Czasami jest to narzucane przez organy regulacyjne, lub może być robione w celu zwiększenia przejrzystości systemu cenowego i ułatwienia jego akceptacji przez klientów, pokazując, dlaczego dany profil musi płacić więcej za to samo pokrycie w porównaniu z innymi.

## 1.3 Problem Klasyfikacji Ryzyka

### 1.3.1 Dywersyfikacja Ryzyka Ubezpieczeniowego

Towarzystwa ubezpieczeniowe pokrywają ryzyka (czyli losowe straty finansowe) poprzez zbieranie składek. Składki są zazwyczaj płacone z góry. Składka czysta to kwota pobierana przez towarzystwo ubezpieczeniowe, która ma być redystrybuowana jako świadczenia wśród ubezpieczających i osób trzecich w ramach wykonania umowy, bez straty ani zysku. Zgodnie z warunkami ważności prawa wielkich liczb, składka czysta jest oczekiwaną kwotą odszkodowania, która ma być wypłacona przez ubezpieczyciela (czasami dyskontowana do daty wystawienia polisy w przypadku zobowiązań długoterminowych).

Można to łatwo zrozumieć w następujący sposób. Rozważmy niezależne zmienne losowe o jednakowym rozkładzie $Y_1, Y_2, Y_3, \dots$ reprezentujące całkowitą kwotę świadczeń wypłaconych przez ubezpieczyciela w związku z poszczególnymi polisami w jednym okresie. Wtedy 

$$\bar{Y}_n = \frac{1}{n} \sum_{i=1}^{n} Y_i$$

reprezentuje średnią wypłatę na polisę w portfelu o wielkości $n$. 

Niech $F$ będzie wspólną dystrybuantą ryzyk $Y_i$, to znaczy,

$$F(y) = P[Y_1 \le y].$$

Przypomnijmy, że wartość oczekiwaną $Y_1$ oblicza się ze wzoru

$$E[Y_1] = \int_{0}^{\infty} ydF(y).$$

Zakładamy, że $E[Y_1]$ istnieje i jest skończona. Prawo wielkich liczb zapewnia wówczas, że

$$\bar{Y}_n \to E[Y_1] \text{ gdy } n \to \infty, \text{ z prawdopodobieństwem } 1.$$

W związku z tym, oczekiwana kwota świadczenia $E[Y_1]$ może być postrzegana jako średni koszt na polisę w nieskończenie dużym, jednorodnym portfelu i w ten sposób wydaje się być najlepszym kandydatem do obliczenia składki czystej.

Należy zauważyć, że składki czyste są jedynie redystrybuowane wśród ubezpieczających w celu pokrycia ich roszczeń, bez straty ani zysku średnio. Dlatego nie można ich uważać za ceny ubezpieczenia, ponieważ należy dodać narzuty, aby pokryć koszty operacyjne, zapewnić wypłacalność, pokryć ogólne wydatki, płacić prowizje pośrednikom, generować zysk dla akcjonariuszy, nie wspominając o podatkach nałożonych przez lokalne władze.

### **1.3.2 Dlaczego Klasyfikować Ryzyka?**

W praktyce większość portfeli jest niejednorodna: składają się one z osób o różnym poziomie ryzyka. Niektórzy ubezpieczający mają tendencję do częstszego zgłaszania roszczeń lub zgłaszania roszczeń o wyższej wartości, średnio. W niejednorodnym portfelu z jednolitą listą cen, wynik finansowy towarzystwa ubezpieczeniowego zależy od składu portfela, jak pokazano w następnym przykładzie.

**Przykład 1.3.1** Jeśli udowodniono, że młodzi ubezpieczający zgłaszają znacznie mniejsze straty niż starsi, a firma ignoruje ten czynnik ryzyka w swojej komercyjnej taryfikacji i nalicza średnią składkę wszystkim ubezpieczającym niezależnie od wieku, większość jej młodszych klientów będzie skłonna przenieść się do innej firmy, która uwzględnia tę różnicę wieku, oferując lepsze składki w młodym wieku. Poprzednia firma pozostaje wtedy z nieproporcjonalnie dużą liczbą starszych ubezpieczających i niewystarczającym dochodem ze składek na pokrycie zgłoszonych strat (ponieważ składki zostały dostosowane do poprzedniej mieszanki młodych i starych ubezpieczających, dzięki czemu starsi ubezpieczający cieszyli się niezasłużoną zniżką składki kosztem młodszych). Zebrane składki nie są już wystarczające do pokrycia strat. W konsekwencji taryfa musi zostać zmieniona, aby osiągnąć poziom konkurenta.

Należy zauważyć, że uwzględnienie różnicy wieku w składkach może być:
- jawne, w przypadku gdy poprzedni ubezpieczyciel nalicza różne składki w zależności od wieku, przyjmując taką samą klasyfikację ryzyka jak jego konkurent;
- lub ukryte, w przypadku gdy poprzedni ubezpieczyciel podnosi swoją składkę do kwoty odpowiadającej starszym wiekiem, zniechęcając młodszych ubezpieczających do zakupu ubezpieczenia od tego dostawcy.

W tej drugiej sytuacji rynek jako całość również uwzględnił różnicę wieku w składkach, ale poprzednia firma celuje teraz w określony segment ubezpieczonej populacji, podczas gdy segmentowa taryfa konkurenta pozwala mu objąć różne profile wiekowe.

Modyfikacja składu portfela może zatem generować straty dla ubezpieczyciela naliczającego jednolitą składkę dla różnych profili ryzyka, podczas gdy konkurenci rozróżniają składki zgodnie z tymi profilami. Ubezpieczający, którym taryfa ubezpieczeniowa nalicza zawyżoną cenę, opuszczają ubezpieczyciela, aby skorzystać ze zniżek składkowych oferowanych przez konkurentów, podczas gdy ci, którzy wydają się być niedoszacowani, pozostają u ubezpieczyciela. Ta zmiana w składzie portfela generuje systematyczne straty dla ubezpieczyciela stosującego jednolitą taryfę. Teoretycznie za każdym razem, gdy konkurent używa dodatkowego czynnika taryfikacyjnego, aktuariusz musi udoskonalić podział, aby uniknąć utraty mniej ryzykownych ubezpieczających w odniesieniu do tego czynnika. Zjawisko to znane jest jako selekcja negatywna (antyselakcja), ponieważ zakłada się, że ubezpieczający wybierają dostawcę ubezpieczenia oferującego im najlepszą składkę.

To (częściowo) wyjaśnia, dlaczego firmy ubezpieczeniowe używają tak wielu czynników: firmy ubezpieczeniowe muszą stosować strukturę taryfikacyjną, która dopasowuje składki do ryzyka tak dokładnie, jak to robią struktury taryfikacyjne konkurentów. Jeśli tego nie robią, narażają się na ryzyko utraty ubezpieczających, którzy są obecnie przepłaceni zgodnie z ich taryfą, naruszając równowagę między oczekiwanymi stratami a zebranymi składkami. Oczywiście w rzeczywistości sprawy są bardziej skomplikowane, z powodu ograniczonej informacji dostępnej ubezpieczającym, którzy zwracają również uwagę na inne elementy niż tylko wysokość składki, takie jak jakość usług, marka czy postrzegana stabilność finansowa ubezpieczyciela, nie wspominając o naturalnej skłonności do pozostawania u tego samego dostawcy ubezpieczeń.

Podsumowując, firma ubezpieczeniowa może naliczać tę samą kwotę składek ubezpieczającym o różnym profilu ryzyka, ale wtedy jest narażona na ryzyko utraty tych przepłaconych. Jest to jeden z powodów, dla których techniczna lista cen musi być jak najdokładniejsza: tylko w ten sposób ubezpieczyciel jest w stanie skutecznie zarządzać swoim portfelem, wiedząc, które profile są przepłacone, a które subsydiują pozostałe. Innymi słowy, ubezpieczyciel zna wartość każdej polisy w portfelu.

### **1.3.3 Potrzeba Modeli Regresyjnych**

Biorąc pod uwagę, że profile ryzyka w portfelach ubezpieczeniowych są zróżnicowane, teoretycznie wystarczyłoby podzielić cały portfel na jednorodne klasy ryzyka, tj. grupy ubezpieczających o tych samych czynnikach ryzyka, i ustalić wysokość składki czystej specyficznej dla każdej klasy ryzyka. Jednakże, jeśli dane są podzielone na klasy ryzyka określone przez wiele czynników, aktuariusze często mają do czynienia z rzadko obsadzonymi grupami kontraktów. Nawet w dużym portfelu, na przykład polis ubezpieczeń komunikacyjnych, staje się jasne, że każdy ubezpieczający staje się w zasadzie unikalny, gdy poszczególne polisy są dzielone za pomocą:

- czynników demograficznych, takich jak na przykład wiek czy płeć;
- czynników społeczno-ekonomicznych, takich jak na przykład zawód, miejsce zamieszkania czy dochód;
- cech ubezpieczonego ryzyka, takich jak na przykład moc samochodu czy typ pojazdu;
- cech pakietu ubezpieczeniowego, takich jak na przykład tylko obowiązkowe ubezpieczenie OC, lub uzupełnione o inne opcjonalne zakresy, takie jak szkody materialne czy kradzież, lub wybór franszyz redukcyjnych;
- cech behawioralnych zbieranych za pomocą urządzeń telematycznych zainstalowanych w pojeździe;
- zewnętrznych baz danych rejestrujących ocenę kredytową lub informacje bankowe, lub historię wykroczeń drogowych;
- przeszłego doświadczenia w odniesieniu do różnych subskrybowanych gwarancji, czasami łącząc wszystkie produkty na poziomie gospodarstwa domowego.

Dlatego proste średnie stają się bezużyteczne i potrzebne są modele regresyjne.

Modele regresyjne przewidują zmienną odpowiedzi na podstawie funkcji czynników ryzyka i parametrów. Podejście to jest również określane jako uczenie nadzorowane (supervised learning). Poprzez powiązanie różnych profili ryzyka, analiza regresji może poradzić sobie z wysoce posegmentowanymi problemami wynikającymi z ogromnej ilości informacji o ubezpieczających, która stała się teraz dostępna dla ubezpieczycieli.

Należy zauważyć, że zakwestionowano kwestię istnienia branży ubezpieczeniowej w erze „big data”. Jak zostanie pokazane poniżej, rosnąca dokładność predykcyjna średniej kwoty straty wpływa na różnice w składkach między ubezpieczającymi. Jednak ubezpieczyciel wciąż pokrywa indywidualne wahania wokół tej wartości oczekiwanej, a zmienność indywidualnych ryzyk pozostaje ogromna w większości linii ubezpieczeniowych, nawet jeśli w modelu cenowym zintegrowano wiele informacji. W związku z tym, branża ubezpieczeniowa pozostaje rentowna. Jest to szczególnie prawdziwe, gdy czasami występują duże roszczenia, jak w przypadku ubezpieczeń OC, gdzie obrażenia ciała często powodują bardzo kosztowne straty. Innym aspektem czyniącym branżę ubezpieczeniową atrakcyjną dla ubezpieczających jest to, że towarzystwo ubezpieczeniowe nie tylko wypłaca odszkodowanie, zadośćuczyniając ofiarom w imieniu ubezpieczających, ale także zajmuje się procesem likwidacji szkody. W systemie prawa deliktowego usługa ta jest z pewnością ważną wartością dodaną działalności ubezpieczeniowej. Wreszcie, co nie mniej ważne, ubezpieczyciele świadczą również pomoc swoim ubezpieczającym lub inne usługi (na przykład bezpośrednią naprawę objętych ubezpieczeniem szkód).

### **1.3.4 Obserwowalne a Ukryte Czynniki Ryzyka**

Niektóre czynniki ryzyka można łatwo zaobserwować, takie jak wiek ubezpieczającego, stan cywilny czy zawód, typ i sposób użytkowania samochodu, czy też miejsce zamieszkania. Inne można zaobserwować, ale wymaga to pewnego wysiłku lub poniesienia kosztów. Jest to typowe w przypadku cech behawioralnych odzwierciedlonych w danych telematycznych lub informacjach zgromadzonych w zewnętrznych bazach danych, do których ubezpieczyciel może uzyskać dostęp za opłatą uiszczoną dostawcy. Jednak oprócz tych obserwowalnych czynników, zawsze pozostają czynniki ryzyka nieznane ubezpieczycielowi. W ubezpieczeniach komunikacyjnych, na przykład, te ukryte czynniki ryzyka zazwyczaj obejmują temperament i umiejętności, agresywność za kierownicą, poszanowanie przepisów drogowych czy szybkość refleksu (nawet jeśli dane telematyczne pomagają teraz ubezpieczycielom w określeniu tych cech behawioralnych, ale dopiero po rozpoczęciu umowy). 

Odtąd oznaczamy jako:
* $\boldsymbol{X}$ = wektor losowy zbierający obserwowalne czynniki ryzyka używane przez ubezpieczyciela (niekoniecznie w związku przyczynowym z odpowiedzią $Y$)
* $\boldsymbol{X}^+$ = ukryte czynniki ryzyka wpływające na ryzyko, oprócz $\boldsymbol{X}$ (w związku przyczynowym z odpowiedzią $Y$).

Należy zauważyć, że niektóre składowe $\boldsymbol{X}$ mogą stać się nieistotne, gdy $\boldsymbol{X}^+$ jest dostępne. Powód jest następujący. Niektóre składowe $\boldsymbol{X}$ mogą nie być bezpośrednio związane z $Y$, a jedynie skorelowane z $\boldsymbol{X}^+$. Oznacza to, że $X$ dostarcza jedynie pośrednich informacji o $Y$.

**Przykład 1.3.2** Jako przykład, pomyśl o liczbie dzieci lub stanie cywilnym w ubezpieczeniach komunikacyjnych. Bycie w stałym związku lub posiadanie jednego lub kilkorga dzieci nie poprawia ani nie pogarsza umiejętności prowadzenia pojazdu. Ale generalnie zwiększa to jego lub jej stopień awersji do ryzyka, przez co osoba ta zaczyna prowadzić ostrożniej. Oznacza to, że liczba dzieci wchodzi do $\boldsymbol{X}$ i ponieważ stopień awersji do ryzyka jest zazwyczaj zawarty w $\boldsymbol{X}^+$ i wpływa na $Y$, cecha ta wydaje się być pośrednio związana z $Y$. Tego rodzaju zjawisko należy zawsze mieć na uwadze podczas analizy danych ubezpieczeniowych.

### **1.3.5 Dzielenie Ryzyka w Taryfie Segmentowej: Solidarność Losowa a Subsydiująca**

Niech $Y$ będzie całkowitą kwotą świadczenia wypłacanego przez ubezpieczyciela w związku z polisą, której cechy to $(\boldsymbol{X}, \boldsymbol{\boldsymbol{X}}^+)$. Składki czyste odpowiadają wartościom oczekiwanym zgodnie z założeniami prawa wielkich liczb, więc muszą być warunkowe względem informacji używanej do wyceny. Zatem składka czysta obliczona przez ubezpieczyciela to $E[Y|\boldsymbol{X}]$, podczas gdy poprawna składka czysta to $E[Y|\boldsymbol{X}, \boldsymbol{X}^+]$. Oprócz ryzyka ubezpieczeniowego, firma podlega również niedoskonałej klasyfikacji ryzyka, ilościowo określonej przez zmienność prawdziwej składki $E[Y|\boldsymbol{X}, \boldsymbol{X}^+]$ niewyjaśnioną przez $\boldsymbol{X}$. Można to zobaczyć, dekomponując wariancję $Y$ w następujący sposób:

$$
\text{Var}[Y] = E[\text{Var}[Y|\boldsymbol{X}]] + \text{Var}[E[Y|\boldsymbol{X}]]
$$

Pierwszy człon reprezentuje część wariancji pochłanianą przez ubezpieczyciela: po utworzeniu klas ryzyka opartych tylko na $\boldsymbol{X}$, wahania $Y$ wokół składki $E[Y|\boldsymbol{X}]$ wewnątrz każdej klasy ryzyka (tj. wariancja $Y$ przy danym $\boldsymbol{X}$) są kompensowane przez ubezpieczyciela. Ponieważ składka zmienia się w zależności od $\boldsymbol{X}$, drugi człon kwantyfikuje wariancję ponoszoną przez ubezpieczających, którzy obciążani są różnymi kwotami składek w zależności od ich profilu, tj. płacą $E[Y|\boldsymbol{X}]$ zmieniające się w funkcji $\boldsymbol{X}$.

Teraz, prawdziwa składka zależy od $\boldsymbol{X}^+$, a nie tylko od $\boldsymbol{X}$. Pierwszy człon można zatem dalej podzielić, aby otrzymać:

$$
\begin{align*}
\text{Var}[Y] &= \underbrace{\underbrace{E[\text{Var}[Y|\boldsymbol{X}, \boldsymbol{X}^+]]}_{\substack{\text{solidarność losowa} \\ \text{zasada wzajemności}}} + \underbrace{E[\text{Var}[E[Y|\boldsymbol{X}, \boldsymbol{X}^+]|\boldsymbol{X}]]}_{\substack{\text{solidarność subsydiująca} \\ \text{niedoskonała klasyfikacja ryzyka}}}}_{\substack{\rightarrow \text{ towarzystwo ubezpieczeniowe}}} \\

&+ \underbrace{\text{Var}[E[Y|\boldsymbol{X}]]}_{\rightarrow \text{ ubezpieczający}}
\end{align*}
$$

Ważne jest, aby zdać sobie sprawę, że przy danych $(\boldsymbol{X}, \boldsymbol{X}^+)$, wahania w $Y$ są czysto losowe, tj. można je przypisać jedynie przypadkowi. Muszą być one zatem kompensowane przez ubezpieczyciela zgodnie z zasadą wzajemności (która jest samą istotą działalności ubezpieczeniowej). Zasada wzajemności jest doskonale odzwierciedlona w motto „Wkład wielu na nieszczęście nielicznych”: składki zebrane wśród dużej, jednorodnej grupy ubezpieczających są następnie redystrybuowane na rzecz tych nielicznych osób, które doznały pewnych niekorzystnych zdarzeń. Dopóki grupa jest jednorodna pod względem $\boldsymbol{X}^+$ (lub może być uważana za jednorodną, na przykład z powodu braku informacji), ubezpieczyciel po prostu nalicza tę samą składkę każdemu członkowi. Zasada wzajemności jest również czasami określana jako „solidarność losowa”, ponieważ zmienność w kwotach strat można przypisać jedynie przypadkowi lub czystej losowości. Tak byłoby w przypadku, gdyby ubezpieczyciel mógł używać $\boldsymbol{X}^+$.

Ponieważ $\boldsymbol{X}^+$ nie jest dostępne, wahania $Y$ przy danym $\boldsymbol{X}$ częściowo odzwierciedlają resztkową niejednorodność, tj. różnice w profilach ryzyka pomimo posiadania tego samego $\boldsymbol{X}$. Oznacza to, że niektórzy ubezpieczający spośród tych z obserwowanymi cechami $\boldsymbol{X}$ są przepłaceni i w ten sposób subsydiują innych, którzy są niedopłaceni. Tworzy to *solidarność subsydiującą* z powodu niedoskonałej klasyfikacji ryzyka. Dokładniej, prawdziwa składka ubezpieczeniowa $E[Y|\boldsymbol{X}, \boldsymbol{X}^+]$ nie może być naliczona ubezpieczającemu o profilu ryzyka $(\boldsymbol{X}, \boldsymbol{X}^+)$, ponieważ $\boldsymbol{X}^+$ jest ukryte, więc ubezpieczyciel absorbuje również wahania w prawdziwych składkach, które nie mogą być wyjaśnione przez jego strukturę taryfikacyjną opartą na $\boldsymbol{X}$.

Naliczanie $E[Y|\boldsymbol{X}]$ zamiast $E[Y|\boldsymbol{X}, \boldsymbol{X}^+]$ oznacza, że niektórzy ubezpieczający z obserwowanym profilem $\boldsymbol{X}$ płacą za dużo, inni za mało, w zależności od wartości $E[Y|\boldsymbol{X}]$ w porównaniu do $E[Y|\boldsymbol{X}, \boldsymbol{X}^+]$. Dzieje się tak, ponieważ ubezpieczający posiadający ten sam obserwowany profil ryzyka $\boldsymbol{X}$ mogą różnić się pod względem ukrytych czynników ryzyka $\boldsymbol{X}^+$, jak właśnie wyjaśniliśmy. Tworzy to pewien efekt subsydiowania: część składki zapłaconej przez tych, którzy są przepłaceni przez taryfę opartą na $\boldsymbol{X}$, jest transferowana na pokrycie świadczeń przyznanych tym, którzy są niedopłaceni. Ogólny bilans pozostaje jednak w równowadze, jako że

$$
E[E[Y|\boldsymbol{X}, \boldsymbol{X}^+]|\boldsymbol{X}] = E[Y|\boldsymbol{X}]
$$

ale ta tożsamość wymaga, aby skład portfela pod względem $\boldsymbol{X}$ (czyli rozkład $\boldsymbol{X}$ w rozważanym portfelu) pozostawał stabilny w czasie. Musi to być starannie monitorowane przez ubezpieczyciela. Problemy pojawiają się, gdy ubezpieczający zdobywają wiedzę na temat $\boldsymbol{X}^+$, dzięki czemu zyskują przewagę informacyjną nad ubezpieczycielem, lub gdy konkurent uzyskuje dostęp do $\boldsymbol{X}^+$ i celuje w niektóre segmenty rynku, które są przepłacane przy użyciu tylko $\boldsymbol{X}$.

### **1.3.6 Ustalanie Składek Ubezpieczeniowych a Przewidywanie Szkodowości**

Rozważmy odpowiedź $Y$ i zbiór cech $X_1, \dots, X_p$ zebranych w wektorze $\boldsymbol{X}$. Cechy są tutaj traktowane jako zmienne losowe, dlatego oznaczane są wielkimi literami. Oznacza to, że pracujemy z ogólnym ubezpieczającym, wylosowanym (a więc reprezentatywnym) z rozważanego portfela. Kiedy przychodzi do wyceny konkretnej umowy, pracujemy warunkowo do zrealizowanej wartości $\boldsymbol{X}$, czyli przy danym $\boldsymbol{X} = \boldsymbol{x}$.

Struktura zależności wewnątrz wektora losowego $(Y, X_1, \dots, X_p)$ jest wykorzystywana do wydobycia informacji zawartych w $\boldsymbol{X}$ na temat $Y$. W taryfikacji aktuarialnej celem jest jak najdokładniejsze oszacowanie składki czystej. Oznacza to, że celem jest warunkowa wartość oczekiwana $\mu(\boldsymbol{X}) = E[Y|\boldsymbol{X}]$ odpowiedzi $Y$ (liczby szkód lub kwoty szkody) przy dostępnej informacji $\boldsymbol{X}$. Odtąd $\mu(\boldsymbol{X})$ będzie nazywane prawdziwą (czystą) składką.

Należy zauważyć, że funkcja $\boldsymbol{x} \to \mu(\boldsymbol{x}) = E[Y|\boldsymbol{X} = \boldsymbol{x}]$ jest generalnie nieznana aktuariuszowi i może wykazywać złożone zachowanie w $\boldsymbol{x}$. Dlatego ta funkcja jest aproksymowana przez składkę (roboczą lub rzeczywistą) $\boldsymbol{x} \to \pi(\boldsymbol{x})$ o stosunkowo prostej strukturze w porównaniu z nieznaną funkcją regresji $\boldsymbol{x} \to \mu(\boldsymbol{x})$.

Dla wielu modeli rozważanych w badaniach ubezpieczeniowych, cechy $\boldsymbol{X}$ są łączone w celu utworzenia wskaźnika $S$ (tj. funkcji o wartościach rzeczywistych cech $X_1, \dots, X_p$), a składka $\pi$ jest monotoniczną, powiedzmy rosnącą, funkcją $S$. Formalnie,

$$ \pi(\boldsymbol{X}) = h(S) $$

dla pewnej rosnącej funkcji $h$.

### **1.3.7 Zmienne A Priori a A Posteriori**

Aktuariusze muszą rozróżniać zmienne a priori i a posteriori wchodzące do ich analizy. Informacje a priori są dostępne przed (lub w momencie) rozpoczęciem rocznego okresu ochrony. Przykłady obejmują wiek i płeć ubezpieczającego lub cechy pojazdu, które są znane z procesu underwritingu w ubezpieczeniach komunikacyjnych. Informacje a posteriori stają się stopniowo dostępne w okresie ochrony, po wystawieniu umowy. Takie zmienne obejmują doświadczenie szkodowe (liczba i koszt roszczeń) oraz cechy behawioralne (ujawnione przez połączone obiekty lub urządzenia telematyczne zainstalowane w ubezpieczonym pojeździe), a także ich wartości z przeszłości.

Informacje a priori są dostępne dla wszystkich ubezpieczających w ten sam, ustandaryzowany sposób, podczas gdy informacje a posteriori są dostępne w różnych ilościach:
- niedostępne dla nowych ryzyk (na przykład dla nowo uprawnionych kierowców);
- dostępne w skróconej formie dla nowych umów (na przykład zaświadczenie rejestrujące roszczenia z tytułu czynów niedozwolonych w ciągu ostatnich 5 lat w OC komunikacyjnym);
- koszt roszczeń dostępny tylko dla ubezpieczających zgłaszających roszczenia;
- zachowanie za kierownicą mierzone tylko wtedy, gdy ubezpieczający prowadzą pojazd.

Ponieważ modele regresyjne celują w warunkowy rozkład odpowiedzi, przy danych cechach a priori, te ostatnie są traktowane jako znane stałe. Dynamika cech a priori generalnie nie jest modelowana (jeśli, kiedy i gdzie ubezpieczający się przeprowadza, na przykład), ale czasami mogą być potrzebne pewne założenia w tym względzie, na przykład przy obliczaniu wartości klienta.

Zmienne a posteriori są modelowane dynamicznie, wspólnie z doświadczeniem szkodowym. Formalnie, informacja ta jest uwzględniana w taryfikacji za pomocą rozkładów predykcyjnych (tj. korekt wiarygodności, taryfikacji opartej na doświadczeniu), które wydają się być specyficzne dla każdej polisy, uwzględniając objętość i wiarygodność własnego przeszłego doświadczenia.

Jak wyjaśniono w Rozdz. 5, nieobserwowalne cechy ryzyka $\boldsymbol{X}^+$ są reprezentowane jako zmienne ukryte (lub efekty losowe) w aktuarialnych modelach taryfikacyjnych, w duchu teorii wiarygodności. Obserwowane wartości odpowiedzi z przeszłości są następnie wykorzystywane do udoskonalenia klasyfikacji ryzyka a priori, poza różnicami wynikającymi z $\boldsymbol{X}$. Oznaczając jako $Y^{\leftarrow}$ przeszłe doświadczenie szkodowe, składka staje się wtedy $E[Y|\boldsymbol{X}, Y^{\leftarrow}]$. Oczywiście, informacja zawarta w $Y^{\leftarrow}$ różni się w zależności od ubezpieczającego: dla nowych nie ma dostępnych informacji, podczas gdy ci objęci ubezpieczeniem od kilku lat mają losowe wektory $Y^{\leftarrow}$ o różnej długości w zależności od czasu spędzonego w portfelu (tj. stażu polisy ubezpieczeniowej). Zatem format $Y^{\leftarrow}$ różni się w zależności od ubezpieczającego. Dlatego $Y^{\leftarrow}$ nie jest traktowane jak inne cechy, ale wymaga specjalnego potraktowania w drugim etapie klasyfikacji ryzyka. Jest to temat teorii wiarygodności, którą można zaimplementować za pomocą modeli mieszanych opracowanych w literaturze statystycznej i dostępnych w oprogramowaniu statystycznym.