# Notatki

## 1.1 Wprowadzenie

W przeciwieństwie do innych branż, działalność ubezpieczeniowa nie polega na sprzedaży produktów jako takich, ale obietnic. Polisa ubezpieczeniowa to nic innego jak obietnica złożona przez ubezpieczyciela ubezpieczającemu, że pokryje on szkody osób trzecich lub wypłaci odszkodowanie za jego własne przyszłe straty spowodowane wystąpieniem niekorzystnych zdarzeń losowych, w zamian za składkę płatną z góry.

W przypadku wypadku, choroby lub śmierci, ubezpieczający lub beneficjent może zgłosić roszczenie do towarzystwa ubezpieczeniowego, prosząc o odszkodowanie w ramach wykonania umowy ubezpieczenia. Roszczenie ubezpieczeniowe można formalnie zdefiniować jako żądanie skierowane do towarzystwa ubezpieczeniowego o objęcie ochroną lub odszkodowanie za finansowe konsekwencje niebezpieczeństwa objętego polisą ubezpieczeniową. Towarzystwo ubezpieczeniowe obsługuje roszczenie od momentu zgłoszenia do ostatecznego rozliczenia (lub zamknięcia) i po zatwierdzeniu, dokonuje wypłaty na rzecz ubezpieczonego lub osoby trzeciej w imieniu ubezpieczonego.

W konsekwencji, w momencie sprzedaży polisy ubezpieczeniowej, dostawca nie zna ostatecznych kosztów tej usługi, ale opiera się na danych historycznych i modelach aktuarialnych, aby przewidzieć zrównoważoną cenę dla swojego produktu. Centralną częścią tej ceny jest składka czysta, czyli najlepsze oszacowanie lub oczekiwana (obecna) wartość przyszłych zobowiązań.

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

**Przykład 1.2.3 (Wycena geograficzna)** W ubezpieczeniach mienia, takich jak ubezpieczenie od ognia dla gospodarstw domowych, powszechną praktyką jest, aby składka za ryzyko na jednostkę ekspozycji zmieniała się w zależności od obszaru geograficznego, przy zachowaniu stałości wszystkich innych czynników ryzyka. W ubezpieczeniach komunikacyjnych większość firm przyjęła klasyfikację ryzyka w zależności od strefy geograficznej, w której mieszka ubezpieczający (na przykład miejskiej, podmiejskiej lub wiejskiej). Zmienność przestrzenna może być związana z czynnikami geograficznymi (takimi jak natężenie ruchu lub bliskość dróg głównych w ubezpieczeniach komunikacyjnych) lub czynnikami społeczno-demograficznymi (być może wpływającymi na wskaźniki kradzieży w ubezpieczeniach domów). Składka techniczna odzwierciedla zmienność przestrzenną w doświadczeniu szkodowym, biorąc pod uwagę lokalizację geograficzną zawartą w kodzie pocztowym. W związku z tym, efekt geograficzny uzyskany z taryfy technicznej jest zazwyczaj upraszczany przed jego integracją z taryfą komercyjną, co generuje różnicę między lokalizacją geograficzną jako ryzykiem a jako czynnikiem taryfikacyjnym.

W tej książce omawiamy jedynie ustalanie składek technicznych opartych na kosztach, uwzględniających wszystkie istotne informacje w celu:
- przeciwdziałania selekcji negatywnej (antyselakcji),
- monitorowania transferów składek powstających w portfelu w wyniku zastosowania taryfy komercyjnej, oraz
- oceny wartości klienta poprzez porównanie składek technicznych z komercyjnymi.

To tylko trzy przykłady użyteczności śledzenia składek technicznych obok składek komercyjnych.

### 1.2.3 Wycena "od góry do dołu" (top-down pricing)

Aktuariusze zazwyczaj łączą dwa podejścia:
- analizę przeprowadzaną na poziomie indywidualnej polisy
- uzupełnioną o analizę zbiorową na poziomie portfela.

Wynika to z faktu, że polisy ubezpieczeniowej nigdy nie można rozpatrywać w izolacji, ale zawsze w powiązaniu z portfelem, w którym się znajduje (dywersyfikacja uzyskana poprzez agregację indywidualnych ryzyk w portfele jest sercem działalności ubezpieczeniowej). Reasekuracja lub koasekuracja mogą stanowić substytut dywersyfikacji ryzyka dla portfeli o ograniczonym wolumenie.

Ponieważ ubezpieczyciele sprzedają obietnice, działalność ubezpieczeniowa zawsze była silnie regulowana w celu zapewnienia wypłacalności, tj. zdolności ubezpieczyciela do spłaty przyszłych roszczeń. Dlatego organy regulacyjne nakładają na ubezpieczycieli wymóg posiadania wystarczającego kapitału, aby mogli oni wypełnić swoje obietnice wobec ubezpieczających. Kapitał ten można obliczyć tylko na poziomie portfela, uwzględniając dywersyfikację ryzyka między polisami.

Dokładniej, wycena "od góry do dołu" przebiega następująco. Najpierw na poziomie portfela sporządzana jest prognoza, aby przewidzieć ogólne przyszłe koszty szkód, biorąc pod uwagę cykle rynkowe, inflację specyficzną dla szkód, dynamikę rezerw szkodowych i tak dalej. Za pomocą odpowiedniej miary ryzyka określa się następnie kwotę kapitału ryzyka, co prowadzi do składki portfelowej. Należy zauważyć, że portfel jest w tej analizie traktowany jako zbiorowość, a szczegółowe indywidualne cechy polis zazwyczaj nie są brane pod uwagę.

Następnie przeprowadza się analizę indywidualną w celu zidentyfikowania profili ryzyka. Wynik tego drugiego etapu jest wykorzystywany do alokacji wynikowego całkowitego dochodu ze składek między różne polisy: określa się względne ryzyko każdej polisy (w odniesieniu do pewnej polisy referencyjnej) i stosuje się wynikowy procent do składki portfelowej.

### 1.2.4 Prewencja

Oprócz przewidywania kosztów, wycena techniczna jest również pomocna w prewencji. Identyfikując czynniki ryzyka i zachowania podatne na wypadki (na przykład za kierownicą, z pomocą urządzeń telematycznych), ubezpieczyciele mogą informować ubezpieczających i pomagać im zmniejszyć częstotliwość i/lub dotkliwość szkód.

W kilku krajach ubezpieczyciele ujawniają teraz część swojej analizy technicznej, aby pokazać, że ich taryfa odpowiednio odzwierciedla doświadczenie szkodowe. Czasami jest to narzucane przez organy regulacyjne, lub może być robione w celu zwiększenia przejrzystości systemu cenowego i ułatwienia jego akceptacji przez klientów, pokazując, dlaczego dany profil musi płacić więcej za to samo pokrycie w porównaniu z innymi.

## 1.3 Problem Klasyfikacji Ryzyka

### 1.3.1 Dywersyfikacja Ryzyka Ubezpieczeniowego

Towarzystwa ubezpieczeniowe pokrywają ryzyka (czyli losowe straty finansowe) poprzez zbieranie składek. Składki są zazwyczaj płacone z góry. Składka czysta to kwota pobierana przez towarzystwo ubezpieczeniowe, która ma być redystrybuowana jako świadczenia wśród ubezpieczających i osób trzecich w ramach wykonania umowy, bez straty ani zysku. Zgodnie z warunkami ważności prawa wielkich liczb, składka czysta jest oczekiwaną kwotą odszkodowania, która ma być wypłacona przez ubezpieczyciela (czasami dyskontowana do daty wystawienia polisy w przypadku zobowiązań długoterminowych).

Można to łatwo zrozumieć w następujący sposób. Rozważmy niezależne zmienne losowe o jednakowym rozkładzie $Y_1, Y_2, Y_3, \dots$ reprezentujące całkowitą kwotę świadczeń wypłaconych przez ubezpieczyciela w związku z poszczególnymi polisami w jednym okresie. Wtedy 

$$\bar{Y}_n = \frac{1}{n} \sum_{i=1}^{n} Y_i$$

reprezentuje średnią wypłatę na polisę w portfelu o wielkości $n$.

Zakładamy, że $E[Y_1]$ istnieje i jest skończona. Prawo wielkich liczb zapewnia wówczas, że

$$\bar{Y}_n \to E[Y_1] \text{ gdy } n \to \infty, \text{ z prawdopodobieństwem } 1.$$

W związku z tym, oczekiwana kwota świadczenia $E[Y_1]$ może być postrzegana jako średni koszt na polisę w nieskończenie dużym, jednorodnym portfelu i w ten sposób wydaje się być najlepszym kandydatem do obliczenia składki czystej.

Należy zauważyć, że składki czyste są jedynie redystrybuowane wśród ubezpieczających w celu pokrycia ich roszczeń, bez straty ani zysku średnio. Dlatego nie można ich uważać za ceny ubezpieczenia, ponieważ należy dodać narzuty, aby pokryć koszty operacyjne, zapewnić wypłacalność, pokryć ogólne wydatki, płacić prowizje pośrednikom, generować zysk dla akcjonariuszy, nie wspominając o podatkach nałożonych przez lokalne władze.

### 1.3.2 Dlaczego Klasyfikować Ryzyka?

W praktyce większość portfeli jest niejednorodna: składają się one z osób o różnym poziomie ryzyka. Niektórzy ubezpieczający mają tendencję do częstszego zgłaszania roszczeń lub zgłaszania roszczeń o wyższej wartości, średnio. W niejednorodnym portfelu z jednolitą listą cen, wynik finansowy towarzystwa ubezpieczeniowego zależy od składu portfela, jak pokazano w następnym przykładzie.

**Przykład 1.3.1** Jeśli udowodniono, że młodzi ubezpieczający zgłaszają znacznie mniejsze straty niż starsi, a firma ignoruje ten czynnik ryzyka w swojej komercyjnej taryfikacji i nalicza średnią składkę wszystkim ubezpieczającym niezależnie od wieku, większość jej młodszych klientów będzie skłonna przenieść się do innej firmy, która uwzględnia tę różnicę wieku, oferując lepsze składki w młodym wieku. Poprzednia firma pozostaje wtedy z nieproporcjonalnie dużą liczbą starszych ubezpieczających i niewystarczającym dochodem ze składek na pokrycie zgłoszonych strat (ponieważ składki zostały dostosowane do poprzedniej mieszanki młodych i starych ubezpieczających, dzięki czemu starsi ubezpieczający cieszyli się niezasłużoną zniżką składki kosztem młodszych). Zebrane składki nie są już wystarczające do pokrycia strat. W konsekwencji taryfa musi zostać zmieniona, aby osiągnąć poziom konkurenta.

Należy zauważyć, że uwzględnienie różnicy wieku w składkach może być:
- jawne, w przypadku gdy poprzedni ubezpieczyciel nalicza różne składki w zależności od wieku, przyjmując taką samą klasyfikację ryzyka jak jego konkurent;
- lub ukryte, w przypadku gdy poprzedni ubezpieczyciel podnosi swoją składkę do kwoty odpowiadającej starszym wiekiem, zniechęcając młodszych ubezpieczających do zakupu ubezpieczenia od tego dostawcy.

W tej drugiej sytuacji rynek jako całość również uwzględnił różnicę wieku w składkach, ale poprzednia firma celuje teraz w określony segment ubezpieczonej populacji, podczas gdy segmentowa taryfa konkurenta pozwala mu objąć różne profile wiekowe.

Modyfikacja składu portfela może zatem generować straty dla ubezpieczyciela naliczającego jednolitą składkę dla różnych profili ryzyka, podczas gdy konkurenci rozróżniają składki zgodnie z tymi profilami. Ubezpieczający, którym taryfa ubezpieczeniowa nalicza zawyżoną cenę, opuszczają ubezpieczyciela, aby skorzystać ze zniżek składkowych oferowanych przez konkurentów, podczas gdy ci, którzy wydają się być niedoszacowani, pozostają u ubezpieczyciela. Ta zmiana w składzie portfela generuje systematyczne straty dla ubezpieczyciela stosującego jednolitą taryfę. Teoretycznie za każdym razem, gdy konkurent używa dodatkowego czynnika taryfikacyjnego, aktuariusz musi udoskonalić podział, aby uniknąć utraty mniej ryzykownych ubezpieczających w odniesieniu do tego czynnika. Zjawisko to znane jest jako selekcja negatywna (antyselakcja), ponieważ zakłada się, że ubezpieczający wybierają dostawcę ubezpieczenia oferującego im najlepszą składkę.

To (częściowo) wyjaśnia, dlaczego firmy ubezpieczeniowe używają tak wielu czynników: firmy ubezpieczeniowe muszą stosować strukturę taryfikacyjną, która dopasowuje składki do ryzyka tak dokładnie, jak to robią struktury taryfikacyjne konkurentów. Jeśli tego nie robią, narażają się na ryzyko utraty ubezpieczających, którzy są obecnie przepłaceni zgodnie z ich taryfą, naruszając równowagę między oczekiwanymi stratami a zebranymi składkami. Oczywiście w rzeczywistości sprawy są bardziej skomplikowane, z powodu ograniczonej informacji dostępnej ubezpieczającym, którzy zwracają również uwagę na inne elementy niż tylko wysokość składki, takie jak jakość usług, marka czy postrzegana stabilność finansowa ubezpieczyciela, nie wspominając o naturalnej skłonności do pozostawania u tego samego dostawcy ubezpieczeń.

Podsumowując, firma ubezpieczeniowa może naliczać tę samą kwotę składek ubezpieczającym o różnym profilu ryzyka, ale wtedy jest narażona na ryzyko utraty tych przepłaconych. Jest to jeden z powodów, dla których techniczna lista cen musi być jak najdokładniejsza: tylko w ten sposób ubezpieczyciel jest w stanie skutecznie zarządzać swoim portfelem, wiedząc, które profile są przepłacone, a które subsydiują pozostałe. Innymi słowy, ubezpieczyciel zna wartość każdej polisy w portfelu.

### 1.3.3 Potrzeba Modeli Regresyjnych

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

### 1.3.4 Obserwowalne a Ukryte Czynniki Ryzyka

Niektóre czynniki ryzyka można łatwo zaobserwować, takie jak wiek ubezpieczającego, stan cywilny czy zawód, typ i sposób użytkowania samochodu, czy też miejsce zamieszkania. Inne można zaobserwować, ale wymaga to pewnego wysiłku lub poniesienia kosztów. Jest to typowe w przypadku cech behawioralnych odzwierciedlonych w danych telematycznych lub informacjach zgromadzonych w zewnętrznych bazach danych, do których ubezpieczyciel może uzyskać dostęp za opłatą uiszczoną dostawcy. Jednak oprócz tych obserwowalnych czynników, zawsze pozostają czynniki ryzyka nieznane ubezpieczycielowi. W ubezpieczeniach komunikacyjnych, na przykład, te ukryte czynniki ryzyka zazwyczaj obejmują temperament i umiejętności, agresywność za kierownicą, poszanowanie przepisów drogowych czy szybkość refleksu (nawet jeśli dane telematyczne pomagają teraz ubezpieczycielom w określeniu tych cech behawioralnych, ale dopiero po rozpoczęciu umowy). 

Odtąd oznaczamy jako:
* $\boldsymbol{X}$ = wektor losowy zbierający obserwowalne czynniki ryzyka używane przez ubezpieczyciela (niekoniecznie w związku przyczynowym z odpowiedzią $Y$)
* $\boldsymbol{X}^+$ = ukryte czynniki ryzyka wpływające na ryzyko, oprócz $\boldsymbol{X}$ (w związku przyczynowym z odpowiedzią $Y$).

Należy zauważyć, że niektóre składowe $\boldsymbol{X}$ mogą stać się nieistotne, gdy $\boldsymbol{X}^+$ jest dostępne. Powód jest następujący. Niektóre składowe $\boldsymbol{X}$ mogą nie być bezpośrednio związane z $Y$, a jedynie skorelowane z $\boldsymbol{X}^+$. Oznacza to, że $X$ dostarcza jedynie pośrednich informacji o $Y$.

**Przykład 1.3.2** Jako przykład, pomyśl o liczbie dzieci lub stanie cywilnym w ubezpieczeniach komunikacyjnych. Bycie w stałym związku lub posiadanie jednego lub kilkorga dzieci nie poprawia ani nie pogarsza umiejętności prowadzenia pojazdu. Ale generalnie zwiększa to jego lub jej stopień awersji do ryzyka, przez co osoba ta zaczyna prowadzić ostrożniej. Oznacza to, że liczba dzieci wchodzi do $\boldsymbol{X}$ i ponieważ stopień awersji do ryzyka jest zazwyczaj zawarty w $\boldsymbol{X}^+$ i wpływa na $Y$, cecha ta wydaje się być pośrednio związana z $Y$. Tego rodzaju zjawisko należy zawsze mieć na uwadze podczas analizy danych ubezpieczeniowych.

### 1.3.5 Dzielenie Ryzyka w Taryfie Segmentowej: Solidarność Losowa a Subsydiująca

Niech $Y$ będzie całkowitą kwotą świadczenia wypłacanego przez ubezpieczyciela w związku z polisą, której cechy to $(\boldsymbol{X}, \boldsymbol{\boldsymbol{X}}^+)$. Składki czyste odpowiadają wartościom oczekiwanym zgodnie z założeniami prawa wielkich liczb, więc muszą być warunkowe względem informacji używanej do wyceny. Zatem składka czysta obliczona przez ubezpieczyciela to $E[Y|\boldsymbol{X}]$, podczas gdy poprawna składka czysta to $E[Y|\boldsymbol{X}, \boldsymbol{X}^+]$. Oprócz ryzyka ubezpieczeniowego, firma podlega również niedoskonałej klasyfikacji ryzyka, ilościowo określonej przez zmienność prawdziwej składki $E[Y|\boldsymbol{X}, \boldsymbol{X}^+]$ niewyjaśnioną przez $\boldsymbol{X}$. Można to zobaczyć, dekomponując wariancję $Y$ w następujący sposób:

$$
\text{Var}[Y] = E[\text{Var}[Y|\boldsymbol{X}]] + \text{Var}[E[Y|\boldsymbol{X}]]
$$

Pierwszy człon reprezentuje część wariancji pochłanianą przez ubezpieczyciela: po utworzeniu klas ryzyka opartych tylko na $\boldsymbol{X}$, wahania $Y$ wokół składki $E[Y|\boldsymbol{X}]$ wewnątrz każdej klasy ryzyka (tj. wariancja $Y$ przy danym $\boldsymbol{X}$) są kompensowane przez ubezpieczyciela. Ponieważ składka zmienia się w zależności od $\boldsymbol{X}$, drugi człon kwantyfikuje wariancję ponoszoną przez ubezpieczających, którzy obciążani są różnymi kwotami składek w zależności od ich profilu, tj. płacą $E[Y|\boldsymbol{X}]$ zmieniające się w funkcji $\boldsymbol{X}$.

Teraz, prawdziwa składka zależy od $\boldsymbol{X}^+$, a nie tylko od $\boldsymbol{X}$. Pierwszy człon można zatem dalej podzielić, aby otrzymać:

$$
\begin{align*}
\text{Var}[Y] &= \underbrace{\underbrace{E[\text{Var}[Y|\boldsymbol{X}, \boldsymbol{X}^+]]}_{\substack{\text{solidarność losowa} \\ \text{zasada wzajemności}}} + \underbrace{E[\text{Var}[E[Y|\boldsymbol{X}, \boldsymbol{X}^+]|\boldsymbol{X}]]}_{\substack{\text{solidarność subsydiująca} \\ \text{niedoskonała klasyfikacja ryzyka}}}}_{\substack{\rightarrow \text{ towarzystwo ubezpieczeniowe}}} \\
&+ \underbrace{\text{Var}[E[Y|\boldsymbol{X}]]}_{\substack{\rightarrow \text{ ubezpieczający}}}
\end{align*}
$$

Ważne jest, aby zdać sobie sprawę, że przy danych $(\boldsymbol{X}, \boldsymbol{X}^+)$, wahania w $Y$ są czysto losowe, tj. można je przypisać jedynie przypadkowi. Muszą być one zatem kompensowane przez ubezpieczyciela zgodnie z zasadą wzajemności (która jest samą istotą działalności ubezpieczeniowej). Zasada wzajemności jest doskonale odzwierciedlona w motto „Wkład wielu na nieszczęście nielicznych”: składki zebrane wśród dużej, jednorodnej grupy ubezpieczających są następnie redystrybuowane na rzecz tych nielicznych osób, które doznały pewnych niekorzystnych zdarzeń. Dopóki grupa jest jednorodna pod względem $\boldsymbol{X}^+$ (lub może być uważana za jednorodną, na przykład z powodu braku informacji), ubezpieczyciel po prostu nalicza tę samą składkę każdemu członkowi. Zasada wzajemności jest również czasami określana jako „solidarność losowa”, ponieważ zmienność w kwotach strat można przypisać jedynie przypadkowi lub czystej losowości. Tak byłoby w przypadku, gdyby ubezpieczyciel mógł używać $\boldsymbol{X}^+$.

Ponieważ $\boldsymbol{X}^+$ nie jest dostępne, wahania $Y$ przy danym $\boldsymbol{X}$ częściowo odzwierciedlają resztkową niejednorodność, tj. różnice w profilach ryzyka pomimo posiadania tego samego $\boldsymbol{X}$. Oznacza to, że niektórzy ubezpieczający spośród tych z obserwowanymi cechami $\boldsymbol{X}$ są przepłaceni i w ten sposób subsydiują innych, którzy są niedopłaceni. Tworzy to *solidarność subsydiującą* z powodu niedoskonałej klasyfikacji ryzyka. Dokładniej, prawdziwa składka ubezpieczeniowa $E[Y|\boldsymbol{X}, \boldsymbol{X}^+]$ nie może być naliczona ubezpieczającemu o profilu ryzyka $(\boldsymbol{X}, \boldsymbol{X}^+)$, ponieważ $\boldsymbol{X}^+$ jest ukryte, więc ubezpieczyciel absorbuje również wahania w prawdziwych składkach, które nie mogą być wyjaśnione przez jego strukturę taryfikacyjną opartą na $\boldsymbol{X}$.

Naliczanie $E[Y|\boldsymbol{X}]$ zamiast $E[Y|\boldsymbol{X}, \boldsymbol{X}^+]$ oznacza, że niektórzy ubezpieczający z obserwowanym profilem $\boldsymbol{X}$ płacą za dużo, inni za mało, w zależności od wartości $E[Y|\boldsymbol{X}]$ w porównaniu do $E[Y|\boldsymbol{X}, \boldsymbol{X}^+]$. Dzieje się tak, ponieważ ubezpieczający posiadający ten sam obserwowany profil ryzyka $\boldsymbol{X}$ mogą różnić się pod względem ukrytych czynników ryzyka $\boldsymbol{X}^+$, jak właśnie wyjaśniliśmy. Tworzy to pewien efekt subsydiowania: część składki zapłaconej przez tych, którzy są przepłaceni przez taryfę opartą na $\boldsymbol{X}$, jest transferowana na pokrycie świadczeń przyznanych tym, którzy są niedopłaceni. Ogólny bilans pozostaje jednak w równowadze, jako że

$$
E[E[Y|\boldsymbol{X}, \boldsymbol{X}^+]|\boldsymbol{X}] = E[Y|\boldsymbol{X}]
$$

ale ta tożsamość wymaga, aby skład portfela pod względem $\boldsymbol{X}$ (czyli rozkład $\boldsymbol{X}$ w rozważanym portfelu) pozostawał stabilny w czasie. Musi to być starannie monitorowane przez ubezpieczyciela. Problemy pojawiają się, gdy ubezpieczający zdobywają wiedzę na temat $\boldsymbol{X}^+$, dzięki czemu zyskują przewagę informacyjną nad ubezpieczycielem, lub gdy konkurent uzyskuje dostęp do $\boldsymbol{X}^+$ i celuje w niektóre segmenty rynku, które są przepłacane przy użyciu tylko $\boldsymbol{X}$.

### 1.3.6 Ustalanie Składek Ubezpieczeniowych a Przewidywanie Szkodowości

Rozważmy odpowiedź $Y$ i zbiór cech $X_1, \dots, X_p$ zebranych w wektorze $\boldsymbol{X}$. Cechy są tutaj traktowane jako zmienne losowe, dlatego oznaczane są wielkimi literami. Oznacza to, że pracujemy z ogólnym ubezpieczającym, wylosowanym (a więc reprezentatywnym) z rozważanego portfela. Kiedy przychodzi do wyceny konkretnej umowy, pracujemy warunkowo do zrealizowanej wartości $\boldsymbol{X}$, czyli przy danym $\boldsymbol{X} = \boldsymbol{x}$.

Struktura zależności wewnątrz wektora losowego $(Y, X_1, \dots, X_p)$ jest wykorzystywana do wydobycia informacji zawartych w $\boldsymbol{X}$ na temat $Y$. W taryfikacji aktuarialnej celem jest jak najdokładniejsze oszacowanie składki czystej. Oznacza to, że celem jest warunkowa wartość oczekiwana $\mu(\boldsymbol{X}) = E[Y|\boldsymbol{X}]$ odpowiedzi $Y$ (liczby szkód lub kwoty szkody) przy dostępnej informacji $\boldsymbol{X}$. Odtąd $\mu(\boldsymbol{X})$ będzie nazywane prawdziwą (czystą) składką.

Należy zauważyć, że funkcja $\boldsymbol{x} \to \mu(\boldsymbol{x}) = E[Y|\boldsymbol{X} = \boldsymbol{x}]$ jest generalnie nieznana aktuariuszowi i może wykazywać złożone zachowanie w $\boldsymbol{x}$. Dlatego ta funkcja jest aproksymowana przez składkę (roboczą lub rzeczywistą) $\boldsymbol{x} \to \pi(\boldsymbol{x})$ o stosunkowo prostej strukturze w porównaniu z nieznaną funkcją regresji $\boldsymbol{x} \to \mu(\boldsymbol{x})$.

Dla wielu modeli rozważanych w badaniach ubezpieczeniowych, cechy $\boldsymbol{X}$ są łączone w celu utworzenia wskaźnika $S$ (tj. funkcji o wartościach rzeczywistych cech $X_1, \dots, X_p$), a składka $\pi$ jest monotoniczną, powiedzmy rosnącą, funkcją $S$. Formalnie,

$$ \pi(\boldsymbol{X}) = h(S) $$

dla pewnej rosnącej funkcji $h$.

### 1.3.7 Zmienne A Priori a A Posteriori

Aktuariusze muszą rozróżniać zmienne a priori i a posteriori wchodzące do ich analizy. Informacje a priori są dostępne przed (lub w momencie) rozpoczęciem rocznego okresu ochrony. Przykłady obejmują wiek i płeć ubezpieczającego lub cechy pojazdu, które są znane z procesu underwritingu w ubezpieczeniach komunikacyjnych. Informacje a posteriori stają się stopniowo dostępne w okresie ochrony, po wystawieniu umowy. Takie zmienne obejmują doświadczenie szkodowe (liczba i koszt roszczeń) oraz cechy behawioralne (ujawnione przez połączone obiekty lub urządzenia telematyczne zainstalowane w ubezpieczonym pojeździe), a także ich wartości z przeszłości.

Informacje a priori są dostępne dla wszystkich ubezpieczających w ten sam, ustandaryzowany sposób, podczas gdy informacje a posteriori są dostępne w różnych ilościach:
- niedostępne dla nowych ryzyk (na przykład dla nowo uprawnionych kierowców);
- dostępne w skróconej formie dla nowych umów (na przykład zaświadczenie rejestrujące roszczenia z tytułu czynów niedozwolonych w ciągu ostatnich 5 lat w OC komunikacyjnym);
- koszt roszczeń dostępny tylko dla ubezpieczających zgłaszających roszczenia;
- zachowanie za kierownicą mierzone tylko wtedy, gdy ubezpieczający prowadzą pojazd.

Ponieważ modele regresyjne celują w warunkowy rozkład odpowiedzi, przy danych cechach a priori, te ostatnie są traktowane jako znane stałe. Dynamika cech a priori generalnie nie jest modelowana (jeśli, kiedy i gdzie ubezpieczający się przeprowadza, na przykład), ale czasami mogą być potrzebne pewne założenia w tym względzie, na przykład przy obliczaniu wartości klienta.

Zmienne a posteriori są modelowane dynamicznie, wspólnie z doświadczeniem szkodowym. Formalnie, informacja ta jest uwzględniana w taryfikacji za pomocą rozkładów predykcyjnych (tj. korekt wiarygodności, taryfikacji opartej na doświadczeniu), które wydają się być specyficzne dla każdej polisy, uwzględniając objętość i wiarygodność własnego przeszłego doświadczenia.

Jak wyjaśniono w Rozdz. 5, nieobserwowalne cechy ryzyka $\boldsymbol{X}^+$ są reprezentowane jako zmienne ukryte (lub efekty losowe) w aktuarialnych modelach taryfikacyjnych, w duchu teorii wiarygodności. Obserwowane wartości odpowiedzi z przeszłości są następnie wykorzystywane do udoskonalenia klasyfikacji ryzyka a priori, poza różnicami wynikającymi z $\boldsymbol{X}$. Oznaczając jako $Y^{\leftarrow}$ przeszłe doświadczenie szkodowe, składka staje się wtedy $E[Y|\boldsymbol{X}, Y^{\leftarrow}]$. Oczywiście, informacja zawarta w $Y^{\leftarrow}$ różni się w zależności od ubezpieczającego: dla nowych nie ma dostępnych informacji, podczas gdy ci objęci ubezpieczeniem od kilku lat mają losowe wektory $Y^{\leftarrow}$ o różnej długości w zależności od czasu spędzonego w portfelu (tj. stażu polisy ubezpieczeniowej). Zatem format $Y^{\leftarrow}$ różni się w zależności od ubezpieczającego. Dlatego $Y^{\leftarrow}$ nie jest traktowane jak inne cechy, ale wymaga specjalnego potraktowania w drugim etapie klasyfikacji ryzyka. Jest to temat teorii wiarygodności, którą można zaimplementować za pomocą modeli mieszanych opracowanych w literaturze statystycznej i dostępnych w oprogramowaniu statystycznym.

## 1.4 Dane Ubezpieczeniowe

### 1.4.1 Dane Szkodowe

Z powodu selekcji negatywnej (antyselakcji), większość badań aktuarialnych opiera się na danych specyficznych dla ubezpieczeń, generalnie składających się z danych szkodowych. Ważne jest, aby zdać sobie sprawę, że roszczenia są zgłaszane według uznania ubezpieczającego. Czasami wypadek nie jest zgłaszany ubezpieczycielowi, ponieważ warunki polisy obejmują mechanizm taryfikacji opartej na doświadczeniu (experience rating): jeśli zgłoszenie roszczenia zwiększa przyszłe składki, ubezpieczający może uznać, że taniej jest pokryć drobne szkody, aby uniknąć kar finansowych nałożonych przez ubezpieczyciela (nie wspominając o możliwym wypowiedzeniu polisy). Czasami, nawet jeśli roszczenie jest zgłoszone, może zostać odrzucone przez firmę z powodu warunków polisy. Dzieje się tak na przykład, gdy kwota roszczenia jest niższa od franszyzy redukcyjnej określonej w polisie. Zazwyczaj takie zdarzenia nie są rejestrowane w bazie danych (lub mogą być rejestrowane jako roszczenie o wartości zerowej, ale często nie systematycznie, więc takie przypadki należy traktować z dużą ostrożnością, jeśli są uwzględnione w analizie).

Praca z danymi szkodowymi oznacza zatem, że dostępna jest tylko ograniczona informacja o zdarzeniach, które faktycznie miały miejsce. Analizując dane ubezpieczeniowe, aktuariusz jest w stanie wyciągnąć wnioski na temat liczby roszczeń zgłoszonych przez ubezpieczających podlegających określonemu mechanizmowi ustalania składek (np. zasady bonus-malus lub franszyzy redukcyjne), a nie na temat rzeczywistej liczby wypadków. Wnioski z analizy aktuarialnej są ważne tylko wtedy, gdy istniejące zasady pozostają niezmienione. Efekt rozszerzenia zakresu ochrony (np. zmniejszenie kwoty franszyz redukcyjnych) jest niezwykle trudny do oceny.

Ponadto niektórzy ubezpieczający mogą nie zgłaszać swoich roszczeń natychmiast, z różnych powodów (na przykład, ponieważ nie byli świadomi wystąpienia zdarzenia ubezpieczeniowego), co wpływa na dostępne dane. Z powodu opóźnionego zgłaszania, obserwowana liczba roszczeń może być mniejsza niż rzeczywista liczba roszczeń dla ostatnich okresów obserwacji. Po zgłoszeniu, roszczenia wymagają pewnego czasu na likwidację. Jest to szczególnie prawdziwe w systemach prawa deliktowego w przypadku roszczeń z tytułu odpowiedzialności cywilnej. Oznacza to, że może minąć wiele lat, zanim ostateczny koszt szkody będzie znany ubezpieczycielowi.

Informacje zarejestrowane w bazach danych zazwyczaj obejmują jeden lub kilka lat kalendarzowych. Dane są przedstawiane na dzień ekstrakcji (powiedzmy, 6 miesięcy po zakończeniu okresu obserwacji). W związku z tym większość „małych” roszczeń jest zlikwidowana, a ich ostateczny koszt jest znany. Jednak w przypadku dużych roszczeń aktuariusze mogą pracować tylko ze szkodami poniesionymi (wypłaty dokonane plus rezerwa, przy czym ta ostatnia stanowi prognozę ostatecznego kosztu, który jeszcze ma być zapłacony, zgodnie z oceną likwidatora szkód). Szkody poniesione są rutynowo używane w praktyce, ale lepszym podejściem byłoby uznanie, że aktuariusze mają tylko częściową wiedzę na temat kwoty szkody.

### 1.4.2 Skłonności

Modele skłonności (propensity models), zwane również modelami prawdopodobieństwa zakupu (likelihood-to-buy), mają na celu przewidywanie prawdopodobieństwa określonego rodzaju zachowania klienta, na przykład czy klient przeglądający stronę internetową prawdopodobnie coś kupi. W zastosowaniach ubezpieczeniowych modele skłonności pomagają aktuariuszom analizować wskaźnik sukcesu w pozyskiwaniu nowego biznesu. W tym celu wykorzystuje się plik zawierający rekord dla każdej oferty złożonej nowym klientom oraz informację, czy polisa została wystawiona. Identyfikacja czynników wyjaśniających tę binarną odpowiedź pomaga ubezpieczycielowi zidentyfikować segmenty, w których jego składki nie są konkurencyjne.

Modele skłonności są również przydatne do mierzenia wskaźników utrzymania (retention rates). Analiza wskaźników utrzymania przy odnowieniu może dostarczyć wiedzy na temat wrażliwości cenowej (lub elastyczności) utrzymania biznesu. W tym przypadku odpowiedź jest równa 1, jeśli polisa jest odnawiana, i 0, jeśli nie jest. Analiza opiera się na pliku zawierającym jeden rekord dla każdej zaoferowanej odnowy.

W tej książce używamy terminu skłonność w odniesieniu do binarnej zmiennej losowej 0–1, gdzie 0 koduje niepowodzenie lub brak, a 1 odpowiada sukcesowi lub obecności. W zastosowaniach ubezpieczeniowych takie binarne wyniki odpowiadają wystąpieniu lub niewystąpieniu pewnych zdarzeń, takich jak:
- czy ubezpieczający zgłosił co najmniej jedno roszczenie w okresie ochrony,
- czy ubezpieczający zmarł w okresie ochrony,
- czy zgłoszone roszczenie jest oszustwem,

na przykład. Dla takich odpowiedzi analityk jest zainteresowany prawdopodobieństwem, że zdarzenie wystąpi. Modele skłonności tego typu są również czasami nazywane modelami incydencji.

### 1.4.3 Dekompozycja Częstotliwość-Dotkliwość

#### 1.4.3.1 Liczba Szkód

Nawet jeśli aktuariusz chce modelować całkowitą kwotę szkody $Y$ wygenerowaną przez polisę w portfelu w jednym okresie (zazwyczaj jeden rok), ta zmienna losowa generalnie nie jest celem modelowania. Rzeczywiście, modelowanie $Y$ nie pozwala na badanie wpływu franszyz redukcyjnych na szkodę ani zasad bonus-malus, na przykład. Zamiast tego, całkowita kwota szkody $Y$ jest rozkładana na:

$$ Y = \sum_{k=1}^{N} C_k $$

gdzie

$N$ = liczba szkód

$C_k$ = koszt (lub dotkliwość) $k$-tej szkody $C_1, C_2, \dots$ są zmiennymi losowymi o identycznym rozkładzie

wszystkie te zmienne losowe są niezależne. Zgodnie z konwencją, pusta suma jest równa zero, to znaczy,

$$ N = 0 \Rightarrow Y = 0. $$

Składowa częstotliwości $Y$ odnosi się do liczby $N$ roszczeń zgłoszonych przez każdego ubezpieczającego w jednym okresie. Biorąc pod uwagę liczbę roszczeń zgłoszonych przez ubezpieczającego w ubezpieczeniach majątkowych i osobowych, model Poissona i jego rozszerzenia są generalnie przyjmowane jako punkt wyjścia. W ubezpieczeniach na życie model dwumianowy jest przydatny do estymacji ogólnych wskaźników umieralności populacji. Modele te są szczegółowo przedstawione w Rozdz. 2.

Zazwyczaj różne składniki rocznych strat ubezpieczeniowych $Y$ są modelowane oddzielnie. Koszty mogą mieć różną skalę, w zależności od rodzaju roszczenia: standardowe lub małe szkody (attritional claims) o umiarkowanych kosztach w porównaniu z dużymi szkodami o znacznie wyższych kosztach. Jeśli mogą wystąpić duże szkody, to mieszanina tych dwóch rodzajów szkód jest jawnie uwzględniana przez:

$$ C_k = \begin{cases} \text{koszt dużej szkody, z prawdopodobieństwem } p, \\ \text{koszt małej szkody, z prawdopodobieństwem } 1-p. \end{cases} $$

#### 1.4.3.2 Kwoty Szkód

Mając do dyspozycji indywidualne koszty każdej szkody, aktuariusz często chce modelować ich odpowiednie kwoty (zwane również wielkościami szkód lub dotkliwościami szkód w literaturze aktuarialnej). Przed przystąpieniem do analizy aktuariusz musi najpierw wykluczyć ewentualne duże szkody, zachowując jedynie te standardowe, czyli zwykłe. Tradycyjnie, słynny przypadek z Bar-le-Duc, który miał miejsce 18 marca 1976 roku we Francji, był uważany za prototyp dużej szkody.

Tego dnia pociąg towarowy zderzył się z samochodem 2CV Citroën na przejeździe kolejowym, jadąc z pełną prędkością, całkowicie niszcząc samochód i wykolejając się na odcinku 100 m. Na szczęście nie było ofiar śmiertelnych, ponieważ pasażerom samochodu udało się opuścić pojazd przed zderzeniem, ale szkody wyniosły rekordowe 30 milionów franków francuskich. Roszczenia zostały zgłoszone przez:
- francuskie przedsiębiorstwo kolejowe SNCF za jedną lokomotywę typu BB15011, 21 wagonów kolejowych i około 100 m zniszczonych torów.
- strony trzecie za tysiące puszek piwa Kronenbourg i opakowań zupy Knorr.
- oraz z tytułu przerwy w działalności gospodarczej: połączenie kolejowe między Paryżem a Strasburgiem zostało przerwane na 10 dni, objazd o długości 200 km musiało pokonywać codziennie 60 autokarów i ciężarówek. Ruch na pobliskim kanale również został wstrzymany na pewien czas.
- Nawet klub wędkarski „Bar-le-Duc” zgłosił utratę setek kilogramów ryb różnych gatunków z powodu przekarmienia piwem Kronenbourg i zupą Knorr.

Nawet jeśli to zdarzenie przez dziesięciolecia było uważane za prototyp dużej szkody, obecnie towarzystwa ubezpieczeniowe stają w obliczu jeszcze większych szkód o bardziej dramatycznych konsekwencjach. Sposób postępowania z dużymi szkodami opisano w ramach Teorii Wartości Ekstremalnych w Rozdz. 9. Ogólnie rzecz biorąc, szkody zwykłe mają funkcję gęstości prawdopodobieństwa, która maleje wykładniczo do 0, gdy ich argument rośnie, podczas gdy w przypadku dużych szkód maleje ona wielomianowo (tj. w znacznie wolniejszym tempie) do 0. Przykładem tego drugiego zachowania jest rozkład Pareto, który jest często używany w badaniach aktuarialnych.

Ogólnie rzecz biorąc, modelowanie kwot szkód jest trudniejsze niż częstotliwości szkód. Jest na to kilka powodów. Po pierwsze i najważniejsze, szkody czasami wymagają kilku lat na likwidację, jak wyjaśniono wcześniej. W ewidencji ubezpieczyciela pojawiają się tylko szacunki ostatecznego kosztu, dopóki szkoda nie zostanie zamknięta. Co więcej, statystyki dostępne do dopasowania modelu dla dotkliwości szkód są znacznie rzadsze, ponieważ generalnie tylko 5–10% polis w portfelu generuje szkody. Wreszcie, niewyjaśniona niejednorodność jest czasami bardziej wyraźna dla kosztów niż dla częstotliwości. Koszt wypadku drogowego jest na przykład w dużej mierze poza kontrolą ubezpieczającego, ponieważ wypłaty towarzystwa ubezpieczeniowego są determinowane przez cechy osób trzecich. Stopień ostrożności wykazywany przez kierowcę w największym stopniu wpływa na liczbę wypadków, ale w znacznie mniejszym stopniu na koszt tych wypadków.

### 1.4.4 Dane Obserwacyjne

Analizy statystyczne przeprowadzane są na danych pochodzących z badań eksperymentalnych lub obserwacyjnych. W pierwszym przypadku losowe przypisanie jednostek indywidualnych (ludzi lub zwierząt, na przykład) do zabiegów eksperymentalnych odgrywa fundamentalną rolę w wyciąganiu wniosków o związkach przyczynowych (na przykład, aby wykazać użyteczność nowego leku). Nie jest to jednak przypadek danych ubezpieczeniowych, które składają się z obserwacji zarejestrowanych na podstawie przeszłych umów zawartych przez ubezpieczyciela.

Jako przykład rozważmy ubezpieczenie komunikacyjne. Ubezpieczeni objęci przez daną firmę ubezpieczeniową generalnie nie stanowią losowej próby z całej populacji kierowców w kraju. Każda firma celuje w określony segment tej populacji (za pomocą kampanii reklamowych lub specyficznego projektu produktu, na przykład) i przyciąga określone profile. Może to być spowodowane postrzeganiem przez konsumentów produktów ubezpieczyciela, kanałami sprzedaży (brokerzy, agenci lub sprzedaż bezpośrednia), nie wspominając o selekcji dokonywanej przez ubezpieczyciela, który sprawdza kandydatów przed zaakceptowaniem ich ryzyka.

W badaniach ubezpieczeniowych uważamy, że portfel jest reprezentatywny dla przyszłych ubezpieczających, tych, którzy pozostaną ubezpieczeni w firmie lub później dołączą do portfela. Założenie, że nowi ubezpieczeni odpowiadają profilom już istniejącym w portfelu, musi być starannie ocenione, ponieważ każda zmiana warunków ochrony lub cenników konkurentów może przyciągnąć nowe profile o różnych poziomach ryzyka (mimo że są identyczni pod względem $\boldsymbol{X}$, mogą różnić się pod względem $\boldsymbol{X}^+$, z powodu selekcji negatywnej (antyselakcji) przeciwko ubezpieczycielowi).

Aktuariusz musi zawsze pamiętać o istotnej różnicy między związkami przyczynowymi a zwykłymi korelacjami istniejącymi między czynnikami ryzyka a liczbą lub dotkliwością szkód. Takie korelacje mogły powstać w wyniku związku przyczynowego, ale mogły również wynikać z efektów zakłócających. Poniższy prosty przykład ilustruje tę sytuację.

**Przykład 1.4.1** Rozważmy ubezpieczenie komunikacyjne i załóżmy, że posiadanie dzieci (obserwowana cecha z dwoma poziomami „bez dziecka” i „$\ge$ 1 dziecko”) nie wpływa na skłonność do szkód, ale na stopień awersji do ryzyka, czyli ostrożność za kierownicą (cecha ukryta). Aby uprościć sytuację, załóżmy, że istnieją tylko dwie kategorie kierowców: ci, którzy są awersyjni do ryzyka, i ci, którzy są skłonni do ryzyka lub poszukujący ryzyka. Teraz załóżmy, że liczba szkód $N$ spowodowanych przez ubezpieczonego kierowcę jest taka, że:

$$\begin{align*}
& P[N \ge 1|\text{skłonny do ryzyka, } \ge 1 \text{ dziecko}] \\
&= P[N \ge 1|\text{skłonny do ryzyka, bez dziecka}] \\
&= P[N \ge 1|\text{skłonny do ryzyka}] \\
&= 0.15 
\end{align*}$$

i

$$ \begin{align*}
& P[N \ge 1|\text{awersyjny do ryzyka, } \ge 1 \text{ dziecko}] \\
&= P[N \ge 1|\text{awersyjny do ryzyka, bez dziecka}] \\
&= P[N \ge 1|\text{awersyjny do ryzyka}] \\
&= 0.05 
\end{align*}$$

Formalnie, posiadanie dzieci jest niezależne od zdarzenia „$N \ge 1$”, przy danym stopniu awersji do ryzyka. To stwierdzenie dotyczy tylko niezależności warunkowej: bez informacji o awersji do ryzyka te dwie wielkości mogą być skorelowane, jak pokazano dalej.

Aby wygenerować zapowiedzianą korelację, załóżmy, że rozkład kierowców awersyjnych do ryzyka w portfelu różni się w zależności od liczby dzieci. W szczególności,

$$ \begin{align*}
& P[\text{skłonny do ryzyka}|\text{bez dziecka}] \\
&= 1 - P[\text{awersyjny do ryzyka}|\text{bez dziecka}] \\
&= 0.9 \end{align*}$$

$$ \begin{align*}
& P[\text{skłonny do ryzyka}|\ge 1 \text{ dziecko}] \\
&= 1 - P[\text{awersyjny do ryzyka}|\ge 1 \text{ dziecko}] \\
&= 0.1 
\end{align*}$$

To indukuje pewną korelację między stopniem awersji do ryzyka a liczbą dzieci. Innymi słowy, posiadanie dzieci dostarcza teraz pewnych informacji o stopniu awersji do ryzyka. Bez wiedzy o stopniu awersji do ryzyka, liczba dzieci pośrednio wpływa na skłonność do szkód. Jest to wyraźnie widoczne z

$$ \begin{align*}
& P[N \ge 1|\text{bez dziecka}] \\
&= P[N \ge 1|\text{skłonny do ryzyka, bez dziecka}]P[\text{skłonny do ryzyka}|\text{bez dziecka}] \\
& + P[N \ge 1|\text{awersyjny do ryzyka, bez dziecka}]P[\text{awersyjny do ryzyka}|\text{bez dziecka}] \\
&= 0.15 \times 0.9 + 0.05 \times 0.1 \\
&= 0.14
\end{align*}$$

podczas gdy

$$\begin{align*}
& P[N \ge 1|\ge 1 \text{ dziecko}] \\
&= P[N \ge 1|\text{skłonny do ryzyka, } \ge 1 \text{ dziecko}]P[\text{skłonny do ryzyka}|\ge 1 \text{ dziecko}] \\
&+ P[N \ge 1|\text{awersyjny do ryzyka, } \ge 1 \text{ dziecko}]P[\text{awersyjny do ryzyka}|\ge 1 \text{ dziecko}] \\
&= 0.15 \times 0.1 + 0.05 \times 0.9 \\
&= 0.06 
\end{align*}$$

Nawet jeśli obecność dzieci nie wpływa na skłonność do szkód po uwzględnieniu stopnia awersji do ryzyka za kierownicą, bezwarunkowo istnieje pewna zależność między tymi dwiema zmiennymi.

Dlatego nie możemy na podstawie analizy marginalnej wnioskować, że brak dzieci powoduje wzrost skłonności do szkód. W rzeczywistości te dwie zmienne są obie związane ze stopniem awersji do ryzyka za kierownicą. Oczywiście, sytuacja jest w rzeczywistości znacznie bardziej skomplikowana, ponieważ posiadanie dzieci może również zwiększyć przejechany dystans (aby odwieźć je do szkoły lub na różne zajęcia, odbierając je późno w nocy z imprez). Nie wspominając o tym, że wśród dzieci mogą być młodzi kierowcy, którzy czasami pożyczają samochód rodziców, powodując wzrost częstotliwości szkód. Żaden z tych efektów nie jest kontrolowany, więc wszystkie stają się pomieszane w badaniu przeprowadzonym przez aktuariusza.

Z powodu możliwych efektów zakłócających, jak zilustrowano w poprzednim przykładzie, aktuariusz musi zawsze pamiętać, że na podstawie danych obserwacyjnych generalnie nie jest możliwe rozdzielenie:
- prawdziwego efektu czynnika ryzyka
- od pozornego efektu wynikającego z korelacji z ukrytymi cechami

na bazie obserwowalnych danych. Ponadto, efekt oszacowany na podstawie statystyk portfelowych jest dominujący: różne historie mogą dotyczyć różnych ubezpieczających, podczas gdy wszystkie są uśredniane w szacunkach uzyskanych przez aktuariusza.

Należy zauważyć, że korelacja z ukrytymi czynnikami ryzyka może nawet odwrócić wpływ dostępnego czynnika ryzyka na odpowiedź. Dzieje się tak na przykład, gdy cecha jest ujemnie skorelowana z odpowiedzią, ale dodatnio skorelowana z ukrytą cechą, przy czym ta ostatnia jest dodatnio związana z odpowiedzią. Aktuariusz może wtedy zaobserwować dodatni związek między tą cechą a odpowiedzią, mimo że prawdziwa korelacja jest ujemna. To jest istota paradoksu Simpsona, który pokazuje, że znak związku między czynnikiem ryzyka a odpowiedzią może się zmienić, gdy aktuariusz skoryguje efekt innego czynnika ryzyka. Zjawisko to zostanie rozważone w następnych rozdziałach, przy omawianiu efektów błędu pominiętej zmiennej.

**Uwaga 1.4.2** Nawet jeśli istnieje korelacja między czynnikiem taryfikacyjnym a ryzykiem objętym przez ubezpieczyciela, może nie być związku przyczynowego między tym czynnikiem a ryzykiem. Wymaganie, aby firmy ubezpieczeniowe ustaliły taki związek przyczynowy, aby mogły używać czynnika taryfikacyjnego, może drastycznie zmienić schematy klasyfikacji ryzyka. Teraz, gdy konsumenci wymagają wysokiego poziomu przejrzystości i gdy każda forma dyskryminacji jest zakazywana w Unii Europejskiej, może to stać się poważnym problemem w przyszłości, jeśli organy regulacyjne zaczną uważać, że korelacja nie wystarcza, aby pozwolić ubezpieczycielom na wdrożenie segmentacji składek, ale że należy w tym celu ustalić przyczynowość.

### 1.4.5 Format Danych

Dane wymagane do przeprowadzenia analiz opisanych w tej książce generalnie składają się z połączonych informacji o polisach i szkodach na poziomie indywidualnego ryzyka. Odpowiednia definicja indywidualnego poziomu ryzyka różni się w zależności od linii biznesowej i rodzaju badania. Na przykład, indywidualne ryzyko generalnie odpowiada pojazdowi w ubezpieczeniach komunikacyjnych lub budynkowi w ubezpieczeniach od ognia.

Baza danych musi zawierać jeden rekord dla każdego okresu, w którym polisa była narażona na ryzyko zgłoszenia roszczenia i w którym wszystkie czynniki ryzyka pozostały niezmienione. Nowy rekord musi być tworzony za każdym razem, gdy zmieniają się czynniki ryzyka, z poprzednią ekspozycją skróconą w momencie zmiany. Numer polisy pozwala wtedy aktuariuszowi śledzić doświadczenie indywidualnych ryzyk w czasie. Anulowanie polis i nowe umowy również skutkują skróceniem okresu ekspozycji. Dla każdego rekordu baza danych rejestruje cechy polisy wraz z liczbą roszczeń i całkowitymi poniesionymi stratami. Oprócz tego pliku polis, istnieje plik szkodowy rejestrujący wszystkie informacje o każdej szkodzie osobno (połączenie między tymi dwoma plikami odbywa się za pomocą numeru polisy). Ten drugi plik zawiera również specyficzne cechy każdej szkody, takie jak obecność obrażeń ciała, liczba ofiar i tak dalej. Ten drugi plik jest interesujący do budowy modeli predykcyjnych dla kosztu szkód na podstawie informacji o okolicznościach każdego zdarzenia ubezpieczeniowego. Pozwala to ubezpieczycielowi lepiej oceniać poniesione straty.

Informacje dostępne do przeprowadzenia klasyfikacji ryzyka są podsumowane w zestawie cech $x_{ij}$, $j=1, \dots, p$, dostępnych dla każdego ryzyka $i$. Cechy te mogą mieć różne formaty:
- kategorialne (takie jak płeć, z dwoma poziomami, męska i żeńska);
- całkowitoliczbowe lub dyskretne (takie jak liczba pojazdów w gospodarstwie domowym);
- ciągłe (takie jak wiek ubezpieczającego).

Kowariancje kategorialne mogą być uporządkowane (gdy poziomy można uporządkować w sensowny sposób, np. poziom wykształcenia) lub nie (gdy poziomów nie można uszeregować, pomyśl na przykład o stanie cywilnym, z poziomami singiel, żonaty, konkubinat, rozwiedziony lub wdowiec).

Należy zauważyć, że cechy ciągłe są generalnie dostępne z ograniczoną precyzją, więc w rzeczywistości są to zmienne dyskretne z dużą liczbą wartości liczbowych. Na przykład, wiek w ukończonych latach (tj. wiek zaokrąglony w dół) jest często rejestrowany w bazie danych i używany w taryfikacji, więc wiek można uznać za uporządkowaną cechę kategorialną. Jednakże, ponieważ liczba kategorii wiekowych jest znaczna i oczekuje się płynnego wzrostu strat z wiekiem, wiek w ukończonych latach jest generalnie traktowany jako cecha ciągła przy użyciu metod przedstawionych w Rozdz. 6. Modele dla danych ciągłych zwykle ignorują tę precyzję pomiaru.

Gdy $x_{ij}$ są numeryczne, całkowite lub ciągłe, mogą bezpośrednio wejść do obliczeń. Jednakże dane jakościowe muszą być najpierw przetworzone przed wejściem do obliczeń. Dokładniej, cechy kategorialne są przekształcane w jedną lub kilka zmiennych zero-jedynkowych (dummy), wskaźnikowych lub binarnych, jak pokazano w następnym przykładzie.

**Przykład 1.4.3** Załóżmy, że obszar zamieszkania ubezpieczającego jest uważany za potencjalny czynnik ryzyka w modelu częstotliwości szkód. Jeśli istnieją trzy obszary, powiedzmy A, B i C, to potrzebne są dwie zmienne wskaźnikowe (dummy) $x_{i1}$ i $x_{i2}$, aby przedstawić dostępne informacje dotyczące ubezpieczającego $i$. W szczególności, $x_{i1}$ i $x_{i2}$ są zdefiniowane jako:

$$ x_{i1} = \begin{cases} 1 & \text{jeśli ubezpieczający } i \text{ mieszka w obszarze A,} \\ 0 & \text{w przeciwnym razie,} \end{cases} $$

i

$$ x_{i2} = \begin{cases} 1 & \text{jeśli ubezpieczający } i \text{ mieszka w obszarze B,} \\ 0 & \text{w przeciwnym razie.} \end{cases} $$

Kodowanie cechy kategorialnej można podsumować w następującej tabeli:

| | $x_{i1}$ | $x_{i2}$ |
| :--- | :---: | :---: |
| Ubezpieczający $i$ mieszka w A | 1 | 0 |
| Ubezpieczający $i$ mieszka w B | 0 | 1 |
| Ubezpieczający $i$ mieszka w C | 0 | 0 |

Nie jest potrzebny żaden wskaźnik dla C, ponieważ odpowiada on $x_{i1} = x_{i2} = 0$: pominięta kategoria C jest nazywana poziomem bazowym lub referencyjnym. Odpowiada ona najliczniej reprezentowanemu obszarowi (z powodów, które staną się jasne w następnych rozdziałach).

To kodowanie jest automatycznie wykonywane przez oprogramowanie statystyczne podczas pracy z cechami kategorialnymi, więc aktuariusz nie musi ręcznie definiować powiązanych zmiennych zero-jedynkowych. Jednak ważne jest, aby pamiętać, że każda cecha kategorialna wchodzi do modelu jako zbiór niepowiązanych zmiennych zero-jedynkowych, aby prawidłowo interpretować wyniki analiz statystycznych.

### 1.4.6 Kwestie Jakości Danych

Jak w większości podręczników aktuarialnych, zakładamy tutaj, że dostępne dane są wiarygodne i dokładne. To założenie ukrywa czasochłonny krok w każdym badaniu aktuarialnym, podczas którego dane są zbierane, sprawdzane pod kątem spójności, czyszczone w razie potrzeby, a czasami łączone z zewnętrznymi bazami danych w celu zwiększenia ilości informacji. Przygotowanie bazy danych często zajmuje najwięcej czasu i nie wygląda na bardzo satysfakcjonujące. Przygotowanie danych ma jednak kluczowe znaczenie, ponieważ, jak mówi przysłowie, „śmieci na wejściu, śmieci na wyjściu”: nie ma nadziei na uzyskanie wiarygodnej technicznej listy cen z bazy danych cierpiącej na wiele ograniczeń.

Po zebraniu danych ważne jest, aby poświęcić wystarczająco dużo czasu na eksploracyjną analizę danych. Ta część analizy ma na celu odkrycie, które cechy wydają się wpływać na odpowiedź, a także podzbiory silnie skorelowanych cech. Ten tradycyjny, pozornie staromodny pogląd może być w konflikcie z nowoczesnym podejściem do nauki o danych, gdzie praktycy czasami są kuszeni, aby wrzucić wszystkie cechy do modelu „czarnej skrzynki” bez poświęcania czasu na zrozumienie, co one oznaczają. Ale jesteśmy głęboko przekonani, że taka ślepa strategia może czasami prowadzić do katastrofalnych wniosków w taryfikacji ubezpieczeniowej, dlatego zdecydowanie zalecamy poświęcenie wystarczającej ilości czasu na odkrycie rodzaju informacji zarejestrowanych w badanej bazie danych.

Eksploracja danych przy użyciu odpowiednich wykresów i tabelaryzacji jest pierwszym krokiem w budowie modelu, mającym na celu odkrycie:
* (i) związków między odpowiedzią a cechami, sugerujących zmienne objaśniające do włączenia do analizy i ich prawdopodobny wpływ na odpowiedź.
* (ii) związków między cechami, podkreślając, które cechy są powiązane i w ten sposób kodują tę samą informację.

Zrozumienie struktury korelacji między dostępnymi cechami jest kluczowe dla budowy modelu, ponieważ silnie powiązane cechy są włączane do modelu z ostrożnością.

Eksploracyjna analiza danych opiera się zazwyczaj na analizach jednokierunkowych (czasami uzupełnianych o dwukierunkowe, w celu wykrycia możliwych interakcji). W analizie jednokierunkowej wpływ określonej cechy na szkody jest badany bez uwzględniania wpływu innych cech zawartych w bazie danych. Główną wadą analiz jednokierunkowych jest zatem to, że mogą być one zniekształcone przez korelacje: różnice w składkach odzwierciedlone przez analizy jednokierunkowe mogą podwójnie liczyć wpływ niektórych cech.

Ten pierwszy krok można zautomatyzować, tworząc standaryzowane raporty opisujące główny wpływ każdej cechy. Może to nasuwać interesujące pytania dotyczące faktycznego znaczenia ujemnych roszczeń (gdy strona trzecia jest odpowiedzialna za wypadek, na przykład) lub zerowych roszczeń (tj. roszczeń zamkniętych bez wypłaty). Ważne jest, aby sprawdzić, czy takie roszczenia były konsekwentnie rejestrowane w bazie danych. Nawet jeśli te roszczenia wydają się bezkosztowe pod względem świadczeń ubezpieczeniowych, ważne jest ich modelowanie w celu alokacji kosztów operacyjnych. Takie roszczenia powinny być wyłączone z analizy opartej na kosztach.

Inną częstą sytuacją jest nieproporcjonalna liczba roszczeń o stałej kwocie. Mogą one wynikać z określonych zasad likwidacji, na przykład z automatycznego przypisywania stałej kwoty poniesionej straty do dopiero co zgłoszonych roszczeń. Mogą być również przypisywane szczególnym mechanizmom rynkowym mającym na celu przyspieszenie likwidacji małych roszczeń: ubezpieczyciele czasami zgadzają się płacić sobie nawzajem stałą, średnią kwotę w przypadku, gdy roszczenie spełnia warunki traktowania go w ten prosty i skuteczny sposób. Może to być również spowodowane cechami biznesowymi. Na przykład, roszczenia dotyczące szyb samochodowych mogą mieć mniej więcej te same koszty.

Dodatkowe cechy mogą być dodane do bazy danych, aby uwzględnić pewne specyficzne warunki, jeśli to konieczne. Na przykład, jeśli aktuariusz łączy dane z kilku firm lub z kilku kanałów sprzedaży, można użyć zmiennych wskaźnikowych do rozróżnienia tych podgrup. Takie wskaźniki mogą być również użyte do pochłonięcia pewnych tymczasowych zjawisk, których powtórzenie w przyszłości nie jest oczekiwane. Oczywiście zawsze lepiej jest używać danych pozbawionych takich zakłócających efektów.

Bazy danych są zazwyczaj oparte na jednym lub kilku okresach rocznych kalendarzowo-wypadkowych. Doświadczenie zebrane na temat polis w tych latach wypadkowych jest rejestrowane w bazie danych. Na przykład, jeśli baza danych odnosi się do lat kalendarzowych 2017–2018, oznacza to, że wszystkie polisy z rocznicą między pierwszym stycznia 2016 r. a 31 grudnia 2019 r., które obowiązywały co najmniej jeden dzień w latach kalendarzowych 2017–2018, pojawiają się w bazie danych. Rozważane roszczenia to te, które wystąpiły między pierwszym stycznia 2017 r. a 31 grudnia 2018 r. Straty poniesione są wyceniane na połowę 2019 r. (zazwyczaj koniec czerwca 2019 r.).

Traktowanie roku kalendarzowego jako dodatkowej cechy pozwala aktuariuszowi śledzić ewentualne trendy w czasie, gdy rozważane są dłuższe okresy obserwacji. Należy zauważyć, że wskaźnik roku kalendarzowego może również uwzględniać pewne opóźnienia w zgłaszaniu roszczeń: w przypadku, gdy niektóre roszczenia są zgłaszane dopiero później, zazwyczaj będzie to miało większy wpływ na ostatnie okresy obserwacji i zostanie odzwierciedlone we wskaźniku roku kalendarzowego. Warto wspomnieć, że zmienne wskaźnikowe odpowiadające kwartałom lub miesiącom mogą zawierać element sezonowości. Na przykład w ubezpieczeniach komunikacyjnych wypadki zdarzają się częściej w miesiącach zimowych z powodu warunków pogodowych, które mogą się różnić z roku na rok.

Przypomnijmy, że straty poniesione sumują kwoty wypłacone i prognozę przyszłych kwot do zapłacenia w związku z danym roszczeniem. Dlatego właściwe jest odczekanie pewnego czasu po zakończeniu okresu obserwacji, aby umożliwić zgłoszenie roszczeń i rozwinięcie się szacunków. Straty poniesione powinny opierać się na najnowszych szacunkach rezerw, czekając do ostatniego przeglądu szacunków przed zbudowaniem bazy danych. Straty poniesione nie powinny być na tym etapie obcinane według żadnego dużego progu, aby umożliwić aktuariuszowi przeprowadzenie analizy wrażliwości testującej różne progi dużych strat.

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

## 2.2 Od rozkładów normalnych do rozkładów ED

### 2.2.2 Rozkłady ED

Pozwól nam teraz przepisać funkcję gęstości prawdopodobieństwa $\mathcal{Nor}(\mu, \sigma^2)$, aby zademonstrować, jak rozszerzyć ją na większą klasę rozkładów prawdopodobieństwa o podobnych wygodnych właściwościach: rodzinę ED. Chodzi o to, jak następuje. Parametrem interesującym w cenach ubezpieczeń jest średnia $\mu$ zaangażowana w obliczenia składek czystych. Dlatego izolujemy składniki funkcji gęstości prawdopodobieństwa rozkładu normalnego, w których pojawia się $\mu$. Odbywa się to poprzez rozwinięcie kwadratu pojawiającego się wewnątrz funkcji wykładniczej w (2.1), co daje

$$
\begin{aligned}
f_Y(y) &= \frac{1}{\sigma\sqrt{2\pi}} \exp\left(-\frac{1}{2\sigma^2}(y^2 - 2y\mu + \mu^2)\right) \\
&= \exp\left(\frac{y\mu - \mu^2/2}{\sigma^2}\right) \frac{\exp\left(-\frac{y^2}{2\sigma^2}\right)}{\sigma\sqrt{2\pi}}. \quad (2.2)
\end{aligned}
$$

Drugi czynnik pojawiający się w (2.2) nie obejmuje $\mu$, więc ważny składnik jest pierwszy. Widzimy, że ma on bardzo prostą formę, będąc wykładnikiem (stąd przymiotnik „wykładniczy” w ED) stosunku z wariancją $\sigma^2$, tj. parametrem dyspersji, pojawiającym się w mianowniku. Licznik to różnica między iloczynem odpowiedzi $y$ i kanonicznego parametru normalnego $\mu$ a funkcją $\mu$, jedyną. Zauważ, że pochodna tego drugiego członu $\frac{\mu^2}{2}$ jest właśnie średnią $\mu$. Taka dekompozycja pozwala nam zdefiniować całą klasę ED rozkładów w następujący sposób.

## 2.4 Niektóre użyteczne własności

### 2.4.4 Funkcja wariancji

Funkcja wariancji $V(\cdot)$ wskazuje na związek między średnią a wariancją rozkładu z rodziny ED. Zauważmy, że brak związku między średnią a wariancją jest możliwy tylko dla odpowiedzi o wartościach rzeczywistych (takich jak normalnie rozłożone, gdzie wariancja $\sigma^2$ nie zależy od średniej $\mu$). Rzeczywiście, jeśli $Y$ jest nieujemne (tj. $Y \ge 0$), to intuicyjnie wariancja $Y$ dąży do zera, gdy średnia z $Y$ dąży do zera. Oznacza to, że wariancja jest funkcją średniej dla odpowiedzi nieujemnych.

Funkcja wariancji $V(\cdot)$ jest formalnie zdefiniowana jako

$$
V(\mu) = \frac{d^2}{d\theta^2}a(\theta) = \frac{d}{d\theta}\mu(\theta).
$$

Funkcja wariancji odpowiada zatem zmianie średniej odpowiedzi $\mu(\theta)$ postrzeganej jako funkcja kanonicznego parametru $\theta$. W przypadku rozkładu normalnego, $\mu(\theta)=\theta$ i $V(\mu)=1$. Inne rozkłady ED mają niestałe funkcje wariancji. Ponownie widzimy, że funkcja kumulanty $a(\cdot)$ determinuje właściwości rozkładu w rodzinie ED.

Wariancję odpowiedzi można zatem zapisać jako

$$
\text{Var}[Y] = \frac{\phi}{v}V(\mu).
$$

Ważne jest, aby pamiętać, że funkcja wariancji nie jest wariancją odpowiedzi, ale funkcją średniej wchodzącą w to wyrażenie (do pomnożenia przez $\phi/v$). Funkcja wariancji jest traktowana jako funkcja średniej $\mu$, nawet jeśli pojawia się jako funkcja $\theta$; jest to możliwe przez odwrócenie relacji między $\theta$ a $\mu$, ponieważ wiemy z Własności 2.4.4, że $\mu = E[Y] = a'(\theta)$. Wypukłość $a(\cdot)$ zapewnia, że średnia funkcja $a'$ jest rosnąca, więc jej odwrotność jest dobrze zdefiniowana. Stąd możemy wyrazić kanoniczny parametr w kategoriach średniej odpowiedzi $\mu$ przez relację

$$
\theta = (a')^{-1}(\mu).
$$

Funkcje wariancji odpowiadające zwykłym rozkładom ED są wymienione w Tabeli 2.2. Zauważmy, że

$$
V(\mu) = \mu^\xi \quad \text{z} \quad \xi = 
\begin{cases}
0 & \text{dla rozkładu normalnego} \\
1 & \text{dla rozkładu Poissona} \\
2 & \text{dla rozkładu Gamma} \\
3 & \text{dla odwrotnego rozkładu Gaussa}.
\end{cases}
$$

Ci członkowie rodziny ED mają więc potęgowe funkcje wariancji. Cała rodzina rozkładów ED z potęgowymi funkcjami wariancji jest określana jako rodzina Tweedie, która zostanie zbadana później w Sekcji 2.5.

## 2.5 Rozkłady Tweedie

### 2.5.1 Potęgowa funkcja wariancji

Modele Tweedie są zdefiniowane w ramach rodziny ED przez potęgową funkcję wariancji postaci

$$
V(\mu) = \mu^\xi
$$

gdzie parametr potęgowy $\xi$ kontroluje kształt rozkładu. Rozkłady: normalny ($\xi=0$), Poissona ($\xi=1$), Gamma ($\xi=2$) i odwrotny Gaussa ($\xi=3$) wszystkie należą do podklasy Tweedie w rodzinie ED. Można wykazać, że rozkład ED z taką potęgową funkcją wariancji zawsze istnieje, z wyjątkiem przypadku, gdy $0 < \xi < 1$. Gdy $v=1$, implikuje to wariancję równą

$$
\text{Var}[Y] = \phi\mu^\xi.
$$

## 3.7 Bootstrap

W niektórych zastosowaniach, analityczne wyniki są trudne do uzyskania, a metody numeryczne muszą być zastosowane. W tym względzie szczególnie użyteczne wydaje się przedstawione w tej sekcji podejście bootstrap.

### 3.7.3 Estymata Bootstrapowa

Idea nieparametrycznego bootstrapu polega na symulowaniu zbiorów niezależnych zmiennych losowych

$$
Y_1^{(b)}, Y_2^{(b)}, \dots, Y_n^{(b)}
$$

podlegających dystrybuancie $\hat{F}_n$, $b=1,2,\dots,B$. Można to zrobić, symulując $U_i \sim \mathcal{Uni}(0,1)$ i ustawiając

$$
Y_i^{(b)} = y_I \text{ gdzie } I = [nU_i] + 1.
$$

### 3.7.4 Bootstrap Nieparametryczny a Parametryczny

W ujęciu nieparametrycznym, ponowne próbkowanie jest wykonywane z $\hat{F}_n$, czyli próbkowanie z obserwacji $y_1, \dots, y_n$ z powtórzeniami. Prowadzi to do rozkładu interesującej nas statystyki, z której można oszacować takie wielkości jak błędy standardowe. W bootstrapie parametrycznym, podstawowy rozkład jest szacowany na podstawie danych w modelu parametrycznym, to znaczy,

$$
F = F_{\theta}.
$$

Próbki bootstrapowe są następnie generowane przez próbkowanie z $F_{\hat{\theta}}$, a nie z $\hat{F}_n$.

### 3.7.5 Zastosowania

Użyjmy podejścia bootstrapowego, aby uzyskać 95% przedział ufności dla prawdopodobieństwa zgonu w ciągu jednego roku $q$. Przypomnijmy, że obserwacje składają się z 30 zgonów i 70 ocalałych, zebranych w wektorze wskaźników zgonów $Y_i = I[T_i \leq 1]$ (z 1 pojawiającą się 30 razy i 0 pojawiającą się 70 razy). Bez utraty ogólności, możemy uporządkować $Y_1, \dots, Y_{100}$ tak, aby wartości 1 pojawiły się najpierw w wektorze.

Następnie losujemy 10 000 razy z powtórzeniami z tego wektora za pomocą funkcji `sample` dostępnej w oprogramowaniu statystycznym R (losowanie 10 000 razy próbek każda po 100 obserwacji). Dla każdej próbki, obliczamy prawdopodobieństwo sukcesu, uśredniając wartości ponownie próbkowanego wektora i przechowujemy wyniki. Przedział mieszczący się od 2.5% do 97.5% percentyla jest równy [0.21, 0.39]. Otrzymane wyniki okazały się być bardzo zbliżone do tych opartych na aproksymacji rozkładem normalnym dla dużych prób i dokładnym dwumianowym przedziale ufności, o których mowa wcześniej. Zauważmy, że zamiast ponownego próbkowania ze 100 obserwowanych wartości, inną możliwością jest generowanie liczby zgonów z rozkładu dwumianowego z parametrami 100 i $\hat{q} = 0.3$ za pomocą funkcji R `rbinom` i dzielenie wynikowych 10 000 realizacji przez 100. Rysunek 3.3 (górny panel) przedstawia histogram 10 000 wartości bootstrapowych dla rocznego prawdopodobieństwa zgonu, który wydaje się być zgodny z aproksymacją rozkładem normalnym dla dużych prób.

Funkcja `boot` z biblioteki R `boot` pozwala nam uzyskać różne wersje podejścia bootstrapowego. Średnia z dostępnej próby wynosi $\hat{q} = 0.3$. Różnica między $\hat{q}$ a średnią $\hat{q}^b$ z 10 000 próbek bootstrapowych wynosi

$$
\hat{q} - \frac{1}{10000} \sum_{b=1}^{10000} \hat{q}^b = -0.000279
$$

podczas gdy odpowiadający błąd standardowy wynosi 0.04546756. Histogram 10 000 wartości jest przedstawiony na dolnym panelu Rys. 3.3. Nawet jeśli funkcja `boot` nie zapewnia różnych wyników w porównaniu z bezpośrednim wykorzystaniem funkcji `sample`, oferuje zaawansowane możliwości bootstrapowe, takie jak przedział ufności oparty na aproksymacji rozkładem normalnym [0.2112, 0.3894], przedział ufności uzyskany z kwantyli 2.5 i 97.5% z 10 000 wartości bootstrapowych [0.21, 0.39] oraz skorygowany o obciążenie i przyspieszony (bias-corrected accelerated) percentylowy przedział ufności [0.21, 0.38], uważany za najdokładniejszy (nawet w tym przykładzie, wszystkie przedziały są bardzo zgodne).

## 4.1 Wprowadzenie

GLM jest definiowany przez określenie dwóch komponentów:
- odpowiedź musi podlegać rozkładowi ED przedstawionemu w Rozdz. 2, oraz
- funkcja łącząca opisuje, jak średnia odpowiedź jest powiązana z liniową kombinacją dostępnych cech (ogólnie nazywaną oceną (score) w zastosowaniach ubezpieczeniowych, nie mylić z oceną Fishera w statystyce przywołaną w Rozdz. 3).

GLM-y zostały pierwotnie wprowadzone w praktyce aktuarialnej jako metoda poprawy dokładności wyceny ubezpieczeń komunikacyjnych. Ich zastosowanie zostało następnie gwałtownie rozszerzone na większość linii biznesowych. Dziś są rutynowo stosowane w underwritingu, wycenie lub zarządzaniu szkodami. Oto kilka typowych zastosowań GLM-ów w ubezpieczeniach:

- **Modelowanie liczby szkód.** Doświadczenie związane z konkretną umową ubezpieczeniową jest generalnie rozkładane na liczbę szkód wraz z odpowiednimi kosztami tych szkód. Rozkład Poissona jest naturalnym kandydatem do modelowania liczby szkód zgłaszanych przez ubezpieczających. Typowe założenie w tych okolicznościach jest takie, że warunkowa średnia częstotliwość szkód może być zapisana jako funkcja wykładnicza liniowej oceny estymowanej na podstawie danych.
- **Modelowanie wysokości szkód.** Ponieważ koszty szkód są nieujemne i dodatnio skośne, rozkłady Gamma i Odwrotny Gaussowski są naturalnymi kandydatami do ich modelowania. Tutaj również aktuariusze generalnie zakładają, że warunkowa średnia wysokość szkody może być zapisana jako wykładnicza funkcja liniowej oceny.
- **Graduacja umieralności i chorobowości.** Aktuariusze są świadomi, że w przeżywalności ubezpieczających występuje duża heterogeniczność. Czynniki ryzyka wpływające na śmiertelność obejmują wiek, płeć, wykształcenie, dochód, zawód, stan cywilny i zachowania zdrowotne. Ogólnie rzecz biorąc, osoby o wyższym statusie społeczno-ekonomicznym mają tendencję do życia dłużej niż te z niższych grup społeczno-ekonomicznych. GLM-y dwumianowe i Poissona pozwalają aktuariuszowi na przeprowadzanie tego rodzaju klasyfikacji ryzyka w ubezpieczeniach na życie i zdrowotnych. Podejście zależy od rodzaju dostępnych danych: grupa otwarta lub zamknięta, dane indywidualne lub dane pogrupowane w przedziały według niektórych cech (takich jak np. suma ubezpieczenia).
- **Modelowanie elastyczności.** Modele elastyczności dla nowych i wznowionych transakcji pomagają firmom ubezpieczeniowym przewidywać wpływ różnych działań (i podwyżek składek) na udział w rynku. Rentowność i modele elastyczności można następnie połączyć w celu podjęcia optymalnych decyzji cenowych.
- **Rezerwacja strat.** Oprócz nadmiernie dyspersyjnego GLM Poissona odpowiadającego zagregowanej metodzie rezerwacji Chain-Ladder, GLM-y mogą być również stosowane w indywidualnej rezerwacji szkód w celu przewidywania czasu do rozliczenia w zależności od niektórych cech szkody, a także ostatecznej kwoty szkody. Pozwala to likwidatorom szkód na tworzenie bardziej odpowiednich rezerw i wczesną identyfikację roszczeń, które mogą być oszukańcze lub najprawdopodobniej zakończą się procesem sądowym.
- **Analiza konkurencji.** GLM-y mogą być wykorzystane do odtworzenia komercyjnych cenników stosowanych przez konkurentów. Odbywa się to poprzez inżynierię wsteczną ich cennika. Pozwala to aktuariuszom na ekstrapolację składek premium na inne profile ryzyka poza te, dla których dostępne są oferty.

Jak wspomniano powyżej, GLM-y opierają się na wyborze rozkładu (wyborze członka rodziny ED do modelowania losowych odchyleń od średniej), liniowej ocenie obejmującej dostępne cechy oraz założonej funkcji łączącej oczekiwaną odpowiedź i ocenę. Gdy aktuariusz wybierze te składniki zgodnie z rozważanymi danymi, wnioskowanie statystyczne jest przeprowadzane za pomocą przeglądanej w Rozdz. 3 zasady największej wiarygodności. Miary diagnostyczne pozwalają następnie aktuariuszowi zbadać względne zalety rozważanych modeli w celu wybrania optymalnego dla rozważanych danych ubezpieczeniowych. Całą analizę można przeprowadzić przy użyciu istniejącego oprogramowania, w tym swobodnie dostępnego, otwartego oprogramowania R.

## 4.2 Struktura GLM-ów

Podejście GLM jest przykładem modelowania regresyjnego lub nadzorowanego uczenia się. Model regresyjny ma na celu wyjaśnienie niektórych cech odpowiedzi, z pomocą cech działających jako zmienne objaśniające. W zastosowaniach ubezpieczeniowych aktuariusze zazwyczaj chcą wyjaśnić średnią wartość odpowiedzi wchodzącą w skład czystych składek: na przykład oczekiwaną liczbę szkód i odpowiadającą jej oczekiwaną składkę za szkodę.

GLM-y oddzielają systematyczne cechy danych od losowych wariacji, które muszą być kompensowane przez ubezpieczenie. Cechy systematyczne są reprezentowane przez funkcję regresji, podczas gdy losowe wariacje są reprezentowane przez rozkład prawdopodobieństwa w rodzinie ED. Dokładniej, GLM składa się z trzech komponentów:
- rozkładu odpowiedzi (losowy komponent) wybranego z rodziny ED,
- liniowej oceny (komponent systematyczny), oraz
- funkcji łączącej, która wiąże średnią odpowiedź z oceną obejmującą dostępne cechy.

Komponent systematyczny podaje czystą składkę odpowiadającą obserwowalnym profilom ubezpieczających, podczas gdy komponent losowy reprezentuje zmienność, która ma być skompensowana przez ubezpieczyciela na mocy wzajemności lub solidarności losowej. Używając GLM, aktuariusz domyślnie zakłada, że wszystkie niezbędne informacje są dostępne (to znaczy, że wektory losowe $\boldsymbol{X}$ i $\boldsymbol{X}^{+}$ pokrywają się, zobacz notację w Sekcji 1.3.4). Jeśli tak nie jest, należy zamiast tego użyć modeli mieszanych (takich jak te badane w Rozdz. 5).

### 4.2.1 Rozkład Odpowiedzi

Rozważmy odpowiedzi $Y_1, Y_2, \dots, Y_n$ mierzone na $n$ osobach. Dla każdej odpowiedzi, aktuariusz ma informacje podsumowane w wektorze $\boldsymbol{x}_i = (1, x_{i1}, \dots, x_{ip})^\top$ wymiaru $p+1$ zawierającym odpowiadające mu cechy (uzupełnione o 1 z przodu, gdy w ocenie uwzględniony jest wyraz wolny, jak w większości zastosowań ubezpieczeniowych). Cechy zazwyczaj składają się z terminów polisy lub cech ubezpieczającego. Pomyśl o rodzaju typu pojazdu, wieku lub zakresie ubezpieczenia w ubezpieczeniach komunikacyjnych, lub rodzaju konstrukcji, dacie budowy lub kwocie ubezpieczenia w ubezpieczeniach domów. Ubezpieczyciele mają teraz dostęp do wielu zmiennych klasyfikacyjnych, które są oparte na warunkach polisy lub informacjach dostarczonych przez ubezpieczających, lub które zawarte są w zewnętrznych bazach danych. Czasami aktuariusz musi uzupełnić ubezpieczenie o dane bankowe lub telematyczne, znacznie rozszerzając dostępne informacje. Odtąd zakładamy, że cechy $x_{ij}$ zostały zarejestrowane bez błędów i brakujących wartości.

Oprócz odpowiedzi, mamy cel, tj. interesującą nas wielkość. W taryfikacji celem jest czysta składka

$$
\mu(\boldsymbol{x}) = \mathrm{E}[Y | \boldsymbol{X} = \boldsymbol{x}],
$$

zwana także funkcją regresji. Zauważ, że celem nie jest odpowiedź $Y$ sama w sobie (jak byłoby w przypadku predykcji). Dzieje się tak, ponieważ aktuariusz chce określić czyste składki, czyli oczekiwane straty. Do celów taryfikacji aktuariusz nie jest zainteresowany przewidywaniem rzeczywistych strat jakiegokolwiek ubezpieczającego, ale raczej średnimi stratami dla jednorodnej grupy ubezpieczonych osób.

Aktuariusze generalnie mają do czynienia z danymi obserwacyjnymi, tak że same cechy są zmiennymi losowymi. Analiza jest następnie przeprowadzana na próbie $(y_i, \boldsymbol{x}_i)$ obserwacji, które można rozumieć jako realizacje wektora losowego łączącego odpowiedź z wektorem cech. Jednak nie próbujemy modelować łącznego rozkładu tego wektora losowego. Zamiast tego, biorąc pod uwagę informacje zawarte w $\boldsymbol{x}_i$, odpowiedzi $Y_i$ zakłada się, że są niezależne z rozkładem warunkowym w rodzinie ED. Warto zauważyć, że odpowiedzi nie muszą mieć identycznych rozkładów.

Można uwzględnić określone wagi $\nu_i$ w celu uwzględnienia różnych ilości informacji, np. gdy $Y_i$ reprezentuje średnią wyników dla grupy jednorodnych ryzyk, a nie wynik pojedynczych ryzyk. Na przykład, odpowiedź może być średnią kwotą straty dla kilku roszczeń, wszystkie z tymi samymi cechami. Waga $\nu_i$ jest wtedy przyjmowana jako liczba strat wchodzących w skład średniej, w zastosowaniu Właściwości 2.4.2. Pomimo tej niezwykłej właściwości GLM-ów, pozwalającej aktuariuszowi na przeprowadzanie analiz na danych pogrupowanych, ważne jest, aby pamiętać, że zawsze jest preferowane nie agregowanie danych i zachowywanie jak największej liczby indywidualnych rekordów.

W praktyce aktuarialnej wagi czasami odzwierciedlają również wiarygodność przypisaną danej obserwacji. Wagi pozwalają aktuariuszowi zidentyfikować odpowiedzi niosące więcej informacji. Ponieważ obserwacje o wyższych wagach mają mniejszą wariancję, dopasowanie modelu będzie bardziej pod wpływem tych punktów danych.

### 4.2.2 Ocena Liniowa

Ocena GLM dla odpowiedzi $Y_i$ jest zdefiniowana jako

$$
\text{score}_i = \boldsymbol{x}_i^\top \boldsymbol{\beta} = \beta_0 + \sum_{j=1}^p \beta_j x_{ij}, \quad i=1, 2, \dots, n,
$$

gdzie $\boldsymbol{\beta} = (\beta_0, \beta_1, \dots, \beta_p)^\top$ jest wektorem wymiaru $p+1$ zawierającym nieznane współczynniki regresji. Zatem $\beta_j$ są parametrami do estymacji na podstawie danych. Jak wskazano w Rozdz. 3, ocena odnosi się do liniowej kombinacji $\boldsymbol{x}_i^\top \boldsymbol{\beta}$ cech, a nie do oceny Fishera.

Ilość $\beta_0$ odpowiada wszystkim wynikom. Nazywa się to wyrazem wolnym, ponieważ odpowiada ocenie dla osoby, dla której $x_{ij} = 0$ dla wszystkich $j=1, \dots, p$. Następnie, każda wielkość $\beta_j$ kwantyfikuje wpływ na ocenę wzrostu o jedną jednostkę w odpowiadającej jej cesze $x_{ij}$. Obecność wyrazu wolnego $\beta_0$ wyjaśnia, dlaczego pierwszy wpis wektora $\boldsymbol{x}_i$ to 1.

Wyjaśnijmy znaczenie liniowości oceny. Słowo „liniowy” w GLM oznacza, że zmienne objaśniające są łączone liniowo, aby przewidzieć (funkcję) średniej. Liniowość w GLM-ach odnosi się do liniowości w współczynnikach $\beta_j$, a nie w cechach. Na przykład, oceny

$$
\text{score}_i = \beta_0 + \beta_1 x_{i1} + \beta_2 x_{i1}^2 \quad \text{i} \quad \text{score}_i = \beta_0 + \beta_1 x_{i1} + \beta_2 x_{i2} + \beta_3 x_{i1}x_{i3}
$$

są obie „liniowe” w sensie GLM, pomimo kwadratu $x_{i1}^2$ i iloczynu $x_{i1}x_{i3}$. W rzeczywistości w GLM zawsze dozwolone jest przekształcanie cech $x_{ij}$, tworząc nowe, przekształcone cechy, takie jak $x_{ij}^2$ lub $x_{ij}x_{ik}$ (chociaż GAM-y oferują lepsze podejście do radzenia sobie z nieliniowymi efektami w skali oceny, jak zobaczymy w Rozdz. 6). Jednak ocena

$$
\text{score}_i = \beta_0 + \beta_1 x_{i1} + \exp(\beta_2 x_{i2})
$$

nie jest liniowa (i nie może być brana pod uwagę w ustawieniu GLM).

### 4.2.3 Macierz Projektowa

Wprowadźmy macierz $\boldsymbol{X}$, często nazywaną macierzą projektową w statystyce, otrzymaną przez powiązanie $\boldsymbol{x}_i$, wiersz po wierszu. Dokładniej, $i$-ty wiersz $\boldsymbol{X}$ jest $\boldsymbol{x}_i^\top$, tak że

$$
\boldsymbol{X} = \begin{pmatrix} \boldsymbol{x}_1^\top \\ \boldsymbol{x}_2^\top \\ \vdots \\ \boldsymbol{x}_n^\top \end{pmatrix} = \begin{pmatrix} 1 & x_{11} & \dots & x_{1p} \\ 1 & x_{21} & \dots & x_{2p} \\ \vdots & \vdots & \ddots & \vdots \\ 1 & x_{n1} & \dots & x_{np} \end{pmatrix}.
$$

Gdy model regresji zawiera wyraz wolny, macierz projektowa $\boldsymbol{X}$ zawiera kolumnę jedynek. Wektor $\boldsymbol{s}$ zbierający wyniki $s_1, \dots, s_n$ dla wszystkich $n$ odpowiedzi jest następnie otrzymywany z

$$
\boldsymbol{s} = (s_1, \dots, s_n)^\top = \boldsymbol{X}\boldsymbol{\beta}.
$$

W całym tym rozdziale zawsze zakładamy, że macierz projektowa $\boldsymbol{X}$ ma pełny rząd. To założenie jest naruszone, jeśli jedna z cech jest liniową kombinacją niektórych innych cech, co implikuje nadmiarowość informacji. Wrócimy do tego zagadnienia, omawiając współliniowość.

---
**Tabela 4.1** Obserwowana liczba szkód wraz z odpowiadającymi jej ekspozycjami na ryzyko (w osobolatach) pojawiającymi się w nawiasach. Ubezpieczenia komunikacyjne, dane hipotetyczne.

| | Tylko TPL | Ograniczone szkody | Kompleksowe |
|---|---|---|---|
| **Mężczyźni** | 1 683 | 3 403 | 626 |
|               | (10 000) | (30 000) | (5 000) |
| **Kobiety**   | 873 | 2 423 | 766 |
|               | (6 000) | (24 000) | (7 000) |

Jako przykład, rozważmy zbiór danych wyświetlony w Tabeli 4.1 sklasyfikowany krzyżowo według płci posiadaczy polis (z dwoma poziomami, mężczyzna i kobieta) oraz zakresu ubezpieczenia (z trzema poziomami, obowiązkowe ubezpieczenie od odpowiedzialności cywilnej (zwane dalej TPL), ograniczone szkody i kompleksowe). Gdy cechy $x_{ij}$ są numeryczne, całkowite lub ciągłe, mogą one bezpośrednio wejść do oceny: ich wkład jest wtedy otrzymywany przez pomnożenie $x_{ij}$ przez odpowiadający mu współczynnik $\beta_j$. Jednakże, jakościowe lub kategoryczne dane muszą być najpierw przetworzone przed wejściem do obliczenia oceny. GLM-y efektywnie uwzględniają efekty cech kategorycznych przez „dummyfikację” włączenia do oceny.

Dwie cechy w Tabeli 4.1 są kategoryczne. Można je zakodować za pomocą cech binarnych. Dokładniej, każdego ubezpieczającego można reprezentować za pomocą wektora $\boldsymbol{x}_i = (1, x_{i1}, x_{i2}, x_{i3})^\top$ z trzema cechami binarnymi:

$$
\begin{aligned}
x_{i1} &= \begin{cases} 1 & \text{jeśli posiadacz polisy } i \text{ jest kobietą} \\ 0 & \text{w przeciwnym razie} \end{cases} \\
x_{i2} &= \begin{cases} 1 & \text{jeśli posiadacz polisy } i \text{ ma tylko TPL} \\ 0 & \text{w przeciwnym razie} \end{cases} \\
x_{i3} &= \begin{cases} 1 & \text{jeśli posiadacz polisy } i \text{ ma ubezpieczenie kompleksowe} \\ 0 & \text{w przeciwnym razie}. \end{cases}
\end{aligned}
$$

Tutaj wybraliśmy poziomy bazowe odpowiadające najbardziej zaludnionym kategoriom, a dokładniej kategorii o największej całkowitej ekspozycji: mężczyzna dla płci ($x_{i1} = 0$) i ograniczony zakres ubezpieczenia ($x_{i2} = x_{i3} = 0$). Powody tego wyboru staną się jaśniejsze później.

---
**Tabela 4.2** Dane z Tabeli 4.1 ułożone w formacie wejściowym do analizy GLM za pomocą oprogramowania statystycznego, takiego jak R.

| $X_1$ | $X_2$ | $X_3$ | # szkód | Ekspozycja |
|-------|-------|-------|---------|------------|
| 0     | 1     | 0     | 1,683   | 10,000     |
| 0     | 0     | 0     | 3,403   | 30,000     |
| 0     | 0     | 1     | 626     | 5,000      |
| 1     | 1     | 0     | 873     | 6,000      |
| 1     | 0     | 0     | 2,423   | 24,000     |
| 1     | 0     | 1     | 766     | 7,000      |

Dane można następnie wyświetlić w formacie używanym do analizy statystycznej, jak pokazano w Tabeli 4.2. Zobaczymy w Sekcji 4.3.5, dlaczego analiza GLM może być przeprowadzona na danych zagregowanych w formie tabelarycznej zamiast na indywidualnych rekordach, bez utraty informacji. Dlatego indeks $i$ może odnosić się do pojedynczej polisy lub całej klasy ryzyka (jak to ma miejsce tutaj).

Macierz projektowa dla danych wyświetlonych w Tabeli 4.1 to

$$
\boldsymbol{X} = \begin{pmatrix}
1 & 0 & 1 & 0 \\
1 & 0 & 0 & 0 \\
1 & 0 & 0 & 1 \\
1 & 1 & 1 & 0 \\
1 & 1 & 0 & 0 \\
1 & 1 & 0 & 1
\end{pmatrix}.
$$

Odpowiadający wektor obserwacji to

$$
\boldsymbol{Y} = \begin{pmatrix}
1,683 \\
3,403 \\
626 \\
873 \\
2,423 \\
766
\end{pmatrix}.
$$

### 4.2.4 Funkcja Łącząca

#### 4.2.4.1 Łączenie Średniej z Oceną Liniową

Zamiast utożsamiać średnią odpowiedź z oceną, jak w modelu regresji normalnej, istnieje jednoznaczne, różniczkowalne odwzorowanie mapujące średnią na ocenę liniową, zwane w żargonie GLM funkcją łączącą. W szczególności, średnia

$$
\mu_i = \mu(\boldsymbol{x}_i)
$$

odpowiedzi $Y_i, i = 1, 2, \dots, n$, jest powiązana z oceną obejmującą zmienne objaśniające za pomocą gładkiej i odwracalnej transformacji linearyzującej, to jest,

$$
g(\mu_i) = \text{score}_i.
$$

Funkcja $g$ jest monotoniczna, różniczkowalna i nazywana jest funkcją łączącą. Ponieważ funkcja łącząca $g$ jest jednoznaczna, możemy ją odwrócić, aby uzyskać

$$
g(\mu_i) = \text{score}_i \iff \mu_i = g^{-1}(\text{score}_i).
$$

Zauważ, że GLM jest modelem warunkowym, który określa rozkład odpowiedzi $Y_i$ przy danym wektorze cech $\boldsymbol{X}_i = (X_{i1}, X_{i2}, \dots, X_{ip})$. W szczególności, $Y_i$ przy danym $\boldsymbol{X}_i = \boldsymbol{x}_i$ podlega rozkładowi z rodziny ED z

$$
\mu(\boldsymbol{x}_i) = g^{-1}\left(\beta_0 + \sum_{j=1}^p \beta_j x_{ij}\right).
$$

Niemniej jednak ważne jest, aby zdać sobie sprawę, że losowy wektor $\boldsymbol{X}_i^\top = (X_{i1}, X_{i2}, \dots, X_{ip})$ może mieć złożoną strukturę zależności, która wpływa na oszacowanie $\boldsymbol{\beta}$, jak zobaczymy później. Ważne jest również, aby pamiętać, że bezwarunkowo, $Y_i$ nie podlega temu samemu rozkładowi. Na przykład, jeśli $Y_i$ przy danym $\boldsymbol{X}_i = \boldsymbol{x}_i$ podlega $\mathcal{Poi}(\exp(\boldsymbol{\beta}^\top \boldsymbol{x}_i))$, to bezwarunkowo, $Y_i$ oznacza losową zmienną $\exp(\boldsymbol{\beta}^\top \boldsymbol{X}_i)$ i mamy do czynienia z mieszanym rozkładem Poissona.

#### 4.2.4.2 Typowe Funkcje Łączące

Oczywiście, nie chodzi o to, aby wybór funkcji łączącej był całkowicie determinowany przez zakres zmiennej odpowiedzi. Wybór funkcji łączącej musi być również oparty na danych. Podejście SIM pozwala aktuariuszowi oszacować $g$ na podstawie danych, bez wcześniejszego określania go. Ta nieparametryczna estymacja może służyć jako punkt odniesienia do wyboru odpowiedniej formy funkcjonalnej. Alternatywnie, niektóre wykresy diagnostyczne mogą być użyte do sprawdzenia adekwatności wybranej funkcji łączącej po dopasowaniu modelu. Oznacza to, że wybór $g$ nie jest czysto arbitralny i że w tym zakresie mogą pomóc niektóre procedury oparte na danych.

---
**Tabela 4.3** Typowe funkcje łączące i ich odwrotności. Tutaj $\mu_i$ to oczekiwana wartość odpowiedzi, $s_i$ to ocena, a $\Phi(\cdot)$ to dystrybuanta standardowego rozkładu normalnego $\mathcal{Nor}(0, 1)$.

| Funkcja łącząca | $s_i = g(\mu_i)$ | $\mu_i = g^{-1}(s_i)$ |
|----|----|----|
| Tożsamościowa (Identity) | $\mu_i$ | $s_i$ |
| Logarytmiczna (Log) | $\ln \mu_i$ | $\exp(s_i)$ |
| Odwrotność (Inverse) | $\mu_i^{-1}$ | $s_i^{-1}$ |
| Odwrotność kwadratu (Inverse-square) | $\mu_i^{-2}$ | $s_i^{-1/2}$ |
| Pierwiastek kwadratowy (Square-root) | $\sqrt{\mu_i}$ | $s_i^2$ |
| Logit | $\ln \frac{\mu_i}{1-\mu_i}$ | $\frac{\exp(s_i)}{1+\exp(s_i)}$ |
| Probit | $\Phi^{-1}(\mu_i)$ | $\Phi(s_i)$ |
| Log-log | $-\ln(-\ln \mu_i)$ | $\exp(-\exp(-s_i))$ |
| Komplementarna log-log | $\ln(-\ln(1-\mu_i))$ | $1 - \exp(-\exp(s_i))$ |

Tabela 4.3 wymienia zwykłe funkcje łączące i ich odwrotności. Te funkcje łączące narzucają warunki na średnią $\mu_i$, a tym samym na zakres odpowiedzi $Y$. Na przykład, funkcje łączące kwadrat, pierwiastek kwadratowy i logarytm stosuje się tylko do wartości nieujemnych i dodatnich, odpowiednio. Ostatnie cztery funkcje łączące wymienione w Tabeli 4.3 są przeznaczone dla danych dwumianowych, gdzie $\mu_i$ reprezentuje prawdopodobieństwo sukcesu, a zatem należy do przedziału jednostkowego $[0, 1]$.

#### 4.2.4.3 Łącze Logarytmiczne

Jak wyjaśniono wcześniej, obiecujący link usuwa ograniczenie na zakres oczekiwanej odpowiedzi. Na przykład, odpowiedź jest nieujemna w wielu zastosowaniach ubezpieczeniowych, gdzie reprezentuje częstotliwość szkód lub ciężkość szkody, tak że $\mu_i \ge 0$. Funkcja łącząca logarytmiczna usuwa wtedy to ograniczenie, ponieważ specyfikacja

$$
\ln \mu_i = \text{score}_i \iff \mu_i = \exp(\text{score}_i)
$$

zapewnia, że warunek ten jest zawsze spełniony.

#### 4.2.4.4 Łącza Logit, Probit i Log-Log

Dla proporcji dwumianowych, średnia odpowiedź odpowiada prawdopodobieństwu sukcesu, tak że $\mu_i$ jest ograniczona do przedziału jednostkowego $[0, 1]$. Funkcje rozkładu są naturalnymi kandydatami do transformacji wyniku o wartościach rzeczywistych na średnią odpowiedź. Często stosowano rozkłady logistyczne i normalne, co dało początek tak zwanym funkcjom łączącym logit i probit. Komplementarna funkcja łącząca log-log jest dziedziczona z rozkładu Ekstremalnej Wartości.

Funkcje łączące logit, probit i komplementarna log-log odwzorowują przedział jednostkowy $[0, 1]$ na całą oś rzeczywistą, usuwając ograniczenie na średnią odpowiedź. Łącze logit

$$
\ln \frac{\mu_i}{1-\mu_i} = \text{score}_i \iff \mu_i = \frac{\exp(\text{score}_i)}{1+\exp(\text{score}_i)} = \frac{1}{1+\exp(-\text{score}_i)}
$$

jest często stosowane dla prawdopodobieństw $\mu_i$ w zastosowaniach aktuarialnych.

#### 4.2.4.5 Łącza Kanoniczne

---
**Tabela 4.4** Przykłady kanonicznych funkcji łączących. Zauważ, że kanoniczne łącze to $\mu^\xi$ dla rozkładów Gamma i Odwrotnego Gaussa, ale znak minus jest generalnie usuwany, a także dzielenie przez 2 w przypadku Odwrotnego Gaussa.

| Funkcja łącząca | $g(\mu)$ | Kanoniczne łącze dla |
|---|---|---|
| Tożsamościowa | $\mu$ | Normalny |
| Logarytmiczna | $\ln \mu$ | Poisson |
| Potęgowa | $\mu^\xi$ | Gamma ($\xi = -1$) |
| | | Odwrotny Gauss ($\xi = -2$) |
| Logit | $\ln \frac{\mu}{1-\mu}$ | Dwumianowy |

Kanoniczny parametr znacznie upraszcza analizę opartą na GLM, ale inne funkcje łączące mogą być również używane. Jest to właśnie wielka siła podejścia GLM. Często w przeszłości aktuariusze przekształcali odpowiedź przed normalną analizą regresji liniowej (taką jak modelowanie logarytmiczno-normalne).

### 4.2.5 Interakcje

Interakcja powstaje, gdy wpływ określonego czynnika ryzyka zależy od wartości innego. Przykładem w ubezpieczeniach komunikacyjnych jest zależność wieku kierowcy od płci: często młode kobiety-kierowcy powodują mniej roszczeń w porównaniu z młodymi mężczyznami, podczas gdy ta różnica płci zanika (a czasem nawet odwraca się) w starszym wieku. Stąd wpływ wieku zależy od płci. W przeciwieństwie do technik regresji rozważanych w kolejnych tomach, takich jak metody oparte na drzewach, GLM-y nie uwzględniają automatycznie interakcji, a takie efekty muszą być ręcznie wprowadzane do oceny.

Rozważmy model z dwiema ciągłymi cechami i oceną postaci

$$
\text{score}_i = \beta_0 + \beta_1 x_{i1} + \beta_2 x_{i2} + \beta_3 x_{i1} x_{i2}.
$$

Ocena liniowa obejmuje trzecią cechę $x_{i3} = x_{i1} x_{i2}$, zwaną terminem interakcji, zbudowaną z dwóch pierwszych. Włączenie iloczynu $x_{i1} x_{i2}$ do oceny indukuje interakcje, ponieważ wzrost o jednostkę $x_{i1}$ zwiększa ocenę liniową o $\beta_1 + \beta_3 x_{i2}$, a zatem wpływ pierwszej cechy zależy od drugiej. Podobnie, wpływ wzrostu o jednostkę $x_{i2}$ wynosi $\beta_2 + \beta_3 x_{i1}$, co zależy od $x_{i1}$. Te dwie cechy oddziałują na siebie. Zmienne $x_{i1}$ i $x_{i2}$ nazywane są efektami głównymi i odróżnia się je od terminu interakcji $x_{i3} = x_{i1} x_{i2}$.

Interakcja zwykłej cechy ilościowej z cechą binarną jest również często przydatna w zastosowaniach ubezpieczeniowych. Załóżmy, że $x_{i1}$ jest ciągła, a $x_{i2}$ jest wskaźnikiem przyjmującym wartości 0 i 1. Wtedy ocena jest dana przez

$$
\begin{aligned}
\text{score}_i &= \beta_0 + \beta_1 x_{i1} + \beta_2 x_{i2} + \beta_3 x_{i1} x_{i2} \\
&= \begin{cases} \beta_0 + \beta_1 x_{i1} & \text{jeśli } x_{i2} = 0 \\ \beta_0 + \beta_2 + (\beta_1 + \beta_3) x_{i1} & \text{jeśli } x_{i2} = 1. \end{cases}
\end{aligned}
$$

Wpływ $x_{i1}$ (to jest, wyraz wolny i nachylenie) zależy od grupy wskazanej przez $x_{i2}$.

### 4.2.6 Podsumowanie Założeń dla Regresji z GLM-ami

Podsumowując, podejście GLM do modelowania danych ubezpieczeniowych wychodzi z następujących założeń:

- **Odpowiedź $Y_i$ podlegająca rozkładowi ED** z funkcją masy prawdopodobieństwa lub funkcją gęstości prawdopodobieństwa postaci
    $$
    f_{\theta_i}(y) = \exp\left(\frac{y\theta_i - a(\theta_i)}{\phi/v_i}\right) c(y, \phi/v_i)
    $$

    ze wspólnym parametrem dyspersji $\phi$ i określonymi wagami $v_i$.

- **Ocena liniowa**
    $$
    \text{score}_i = \text{score}(\boldsymbol{x}_i) = \boldsymbol{x}_i^\top \boldsymbol{\beta} = \beta_0 + \sum_{j=1}^p \beta_j x_{ij}.
    $$

- **Modelowanie średniej:** oczekiwana odpowiedź $\mu_i = \mu(\boldsymbol{x}_i)$ postaci
    $$
    \begin{aligned}
    a'(\theta_i) &= \mu_i \\
    g(\mu_i) &= \text{score}_i \iff \mu_i = g^{-1}(\text{score}_i) \\
    g &\in \{\text{ln, logit, \dots}\}.
    \end{aligned}
    $$

- **Monotoniczna funkcja łącząca $g$** jest określana przez aktuariusza, a nie estymowana na podstawie danych.

## 4.3 Równania Wiarygodności w GLM

### 4.3.1 Postać Ogólna

Gdy GLM zostanie określony pod względem funkcji łączącej i rozkładu ED, lub równoważnie, gdy funkcja wariancji zostanie wybrana przez aktuariusza, estymowane parametry są uzyskiwane metodą największej wiarygodności (przypomnianą w Rozdz. 3). Dostarcza to aktuariuszowi nie tylko estymat współczynników regresji $\beta_j$, ale także estymowanych błędów standardowych dla dużych prób. W istocie, to podejście dąży do estymacji GLM, który ma najwyższe prawdopodobieństwo wygenerowania rzeczywistych danych.

Niektóre z rozkładów ED, na których opierają się GLM, zawierają nieznany parametr dyspersji $\phi$. Chociaż ten parametr mógłby być również estymowany metodą największej wiarygodności, zamiast tego generalnie stosuje się estymator momentów, w drugim kroku, gdy $\boldsymbol{\beta}$ zostanie już oszacowane. W rzeczywistości okaże się, że znajomość $\phi$ nie jest potrzebna do oszacowania $\boldsymbol{\beta}$, więc te dwa zbiory parametrów można traktować oddzielnie. Dlatego tutaj postrzegamy funkcję wiarygodności jako funkcję tylko $\boldsymbol{\beta}$.

Funkcja wiarygodności jest iloczynem prawdopodobieństw zaobserwowania wartości każdej odpowiedzi, z funkcją gęstości prawdopodobieństwa używaną w miejsce funkcji masy prawdopodobieństwa dla odpowiedzi ciągłych. Jak wyjaśniono w Rozdz. 3, zwykle przełącza się na logarytm wiarygodności, ponieważ obejmuje on sumę, a nie iloczyn (a dowolna wartość parametru maksymalizująca logarytm wiarygodności maksymalizuje również samą wiarygodność).

### 4.3.3 Wpływ Funkcji Wariancji

Rozważając równanie wiarygodności GLM (4.1), ważne jest, aby zdać sobie sprawę, jak wybór funkcji wariancji $V(\cdot)$ wpływa na dopasowanie modelu. Widzimy, że im większa wariancja $V(\mu_i)$, tym mniejszy wkład obserwacji $i$ do sumy pojawiającej się w równaniach wiarygodności. Innymi słowy, dopuszczalne są większe różnice $y_i - \mu_i$, gdy $\mu_i$ jest duże.

Z Rozdziału 2 wiemy, że typowe funkcje wariancji mają postać Tweediego $V(\mu) = \mu^\xi$. W rodzinie Tweediego:
- Dla $\xi=0$ (przypadek normalny), każda obserwacja ma taką samą ustaloną wariancję niezależnie od jej średniej, a dopasowane wartości są jednakowo przyciągane do obserwowanych punktów danych.
- Dla $\xi=1$ (przypadek Poissona), wariancja jest proporcjonalna do oczekiwanej wartości każdej obserwacji: obserwacje z mniejszą oczekiwaną wartością będą miały mniejszą założoną wariancję, co skutkuje większą wagą (lub wiarygodnością) przy estymacji parametrów. Dopasowanie modelu jest zatem bardziej pod wpływem obserwacji o mniejszej oczekiwanej wartości niż o większej oczekiwanej wartości (a więc większej założonej wariancji).
- Gdy $\xi=2$ (przypadek Gamma) i $\xi=3$ (przypadek odwrotny Gaussa), dopasowanie GLM jest jeszcze bardziej pod wpływem obserwacji o mniejszych oczekiwanych wartościach, tolerując większe błędy dla obserwacji o większych oczekiwanych wartościach.

Wagi a priori $v_i$ również działają na wariancję i skutkują tym, że punkty danych mają mniejszy wpływ na dopasowanie modelu. Rzeczywiście, zwiększenie wagi zmniejsza wariancję, a odpowiadająca jej obserwacja odgrywa ważniejszą rolę w dopasowaniu modelu.

Wybór funkcji wariancji jest zatem sprawą najwyższej wagi, aby uzyskać dokładne dopasowanie modelu. Jak zobaczymy, stosując techniki quasi-wiarygodności, techniki oparte na GLM wydają się być dość odporne na błędy w rozkładzie, o ile funkcja wariancji została poprawnie określona.

### 4.3.7 Przesunięcia (Offsets)

#### 4.3.7.1 Definicja

Przesunięcia są używane w zdecydowanej większości badań aktuarialnych. Nie należy ich mylić z wagami, przesunięcia wchodzą do oceny addytywnie, bez mnożenia przez współczynnik regresji do oszacowania na podstawie danych. Przesunięcia reprezentują zatem znane wkłady i a priori informacje do oceny addytywnej. W przeciwieństwie do wyrazu wolnego $\beta_0$ wchodzącego do wszystkich ocen, przesunięcia zwykle różnią się w zależności od osoby.

W szczególności, przesunięcie jest wielkością dodawaną do oceny, bez mnożenia przez znany lub wstępnie określony współczynnik, który nie jest estymowany na podstawie danych. Zatem, przesunięcie można postrzegać jako dodatkową cechę, której współczynnik jest ograniczony, powiedzmy, do 1. Ocena staje się wtedy

$$
\text{score}_i = \text{offset}_i + \boldsymbol{x}_i^\top \boldsymbol{\beta}
$$

gdzie $\text{offset}_i$ oznacza przesunięcie specyficzne dla $i$-tej osoby.

#### 4.3.7.2 Zliczenia Poissona i Wskaźniki Poissona

Typowym zastosowaniem przesunięcia jest włączenie miary ekspozycji przy modelowaniu stawek. Na przykład, jeśli niektóre rekordy w ubezpieczeniach komunikacyjnych odpowiadają polisom obowiązującym przez 6 miesięcy, podczas gdy inne odpowiadają polisom obowiązującym przez cały rok, właściwe jest użycie z łączem logarytmicznym logarytmu okresu ubezpieczenia jako przesunięcia, jak wyjaśniono dalej. W modelu regresji Poissona, ekspozycja jest generalnie liczbą pojazdo-lat ubezpieczonych, a odpowiedzią jest liczba $Y_i$ zgłoszonych roszczeń. Biorąc pod uwagę proces Poissona z Rozdziału 2, wiemy, że ta ekspozycja mnoży roczną stopę szkód. Ponieważ ten przesunięcie musi być dodane w skali oceny, logarytm ekspozycji jest używany jako przesunięcie: oznaczając ekspozycję jako $e_i$,

$$
\ln \mu_i(\boldsymbol{x}_i) = \ln e_i + \boldsymbol{x}_i^\top \boldsymbol{\beta} \implies \text{offset}_i = \ln e_i.
\quad\quad\text{(4.7)}
$$

#### 4.3.7.3 Inne Zastosowania Przesunięć

Innym przydatnym zastosowaniem przesunięcia jest ograniczenie niektórych czynników ratingowych do wartości wstępnie określonych przy przechodzeniu z technicznego na komercyjny cennik. Takie ograniczenia są często motywowane konkurencją i względami marketingowymi, pozwalając na optymalne dostosowanie współczynników pozostałych czynników ratingowych do tych ograniczeń.

Przesunięcie może nawet pomóc w przejrzeniu lub udoskonaleniu istniejącego cennika. Ten ostatni jest wtedy umieszczany w przesunięciu, a oszacowane współczynniki regresji wskazują wtedy korekty potrzebne do poprawy obecnego cennika.

W ubezpieczeniach na życie, ekspozycje portfela są czasami ograniczone do rynku (lub innego odniesienia) tablicy życia. Sugeruje to pracę w kategoriach względnych, w odniesieniu do znaczącej referencyjnej tablicy życia. Odbywa się to poprzez wprowadzenie do wyniku regresji Poissona logarytmu oczekiwanej liczby zgonów zgodnie z tą tablicą życia, aby wyjaśnić specyficzną dla portfela siłę śmiertelności. To samo podejście dotyczy stawek chorobowości w ubezpieczeniach zdrowotnych.

## 4.4 Rozwiązywanie Równań Wiarygodności w GLM

### 4.4.2 Algorytm IRLS

Równanie wiarygodności (4.1) nie dopuszcza jawnych rozwiązań (z wyjątkiem przypadku normalnego, jak pokazano powyżej) i dlatego musi być rozwiązywane iteracyjnie. Jedną z przyczyn, które przyczyniły się do sukcesu GLM, jest możliwość użycia jednego algorytmu do rozwiązywania równania wiarygodności (4.1) we wszystkich przypadkach. Zaczynając od odpowiedniej wartości początkowej dla $\boldsymbol{\beta}$, do uzyskania rozwiązań można użyć algorytmu Newtona-Raphsona. Algorytm Newtona-Raphsona wykorzystuje macierz Hessego, czyli drugie pochodne funkcji celu. W odniesieniu do maksymalizacji funkcji log-wiarygodności, hesjan odpowiada obserwowanej informacji Fishera, ze zmianą znaku (jak przypomniano w Rozdz. 3). W porównaniu z algorytmem Newtona-Raphsona, algorytm scoringu Fishera zastępuje obserwowaną macierz informacyjną jej oczekiwanym odpowiednikiem, czyli informacją Fishera. Gdy obie macierze informacyjne pokrywają się, scoring Fishera odpowiada metodzie Newtona-Raphsona. Oczekiwane i obserwowane macierze informacyjne są równe, gdy używane są kanoniczne funkcje łączące (pamiętaj, że funkcja log-wiarygodności jest zawsze wklęsła w tym przypadku, więc estymator największej wiarygodności jest zawsze unikalny, jeśli istnieje). Zazwyczaj potrzeba tylko kilku kroków, aby uzyskać $\boldsymbol{\beta}$, gdy używane są łącza kanoniczne. Każdą iterację algorytmu Newtona-Raphsona można zinterpretować jako dopasowanie normalnego modelu regresji liniowej na odpowiedziach roboczych, co daje początek podejściu iteracyjnie ważonych najmniejszych kwadratów (IRLS) wyjaśnionemu dalej. IRLS działa zatem poprzez sekwencję problemów najmniejszych kwadratów, dla których dostępne są dobrze zbadane techniki numeryczne.

W GLM średnia odpowiedź jest powiązana z oceną liniową przez funkcję łączącą.

W większości przypadków zbieżność algorytmu IRLS jest bardzo szybka. Jednak czasami pojawiają się trudności. Gdy zbieżność nie występuje, może to być spowodowane problemem z rozważanymi danymi. Na przykład, gdy dwie grupy są liniowo rozdzielalne w przestrzeni cech z odpowiedziami binarnymi, dane mogą być dopasowane idealnie, a niestabilne estymaty parametrów z wysokimi błędami standardowymi są typowo uzyskiwane. Powoduje to problemy ze zbieżnością w algorytmie IRLS.

### 4.4.3 Wybór Poziomu Bazowego dla Cech Kategorycznych

Oprogramowanie GLM automatycznie zastępuje każdą cechę kategoryczną zbiorem wskaźników, po jednym dla każdego poziomu innego niż poziom bazowy. Ten ostatni musi być wybrany z rozwagą, ponieważ cała analiza jest wykonywana w kategoriach względnych, w odniesieniu do wybranego poziomu bazowego.

Rozważmy Przykład 1.4.3, gdzie obszar zamieszkania posiadacza polisy jest uważany za potencjalną zmienną objaśniającą dla częstotliwości szkód $Y$. Istnieją trzy obszary, A, B i C, zakodowane za pomocą dwóch wskaźników (zmiennych zero-jedynkowych) $x_{i1}$ (równe 1, jeśli posiadacz polisy mieszka w obszarze A, a 0 w przeciwnym razie) i $x_{i2}$ (równe 1, jeśli posiadacz polisy mieszka w obszarze B, a 0 w przeciwnym razie). Nie jest potrzebny żaden wskaźnik dla poziomu referencyjnego C. Wtedy, wyraz wolny $\beta_0$ odpowiada za efekt poziomu referencyjnego, tutaj obszaru C. Następnie, $\beta_1$ kwantyfikuje różnicę między obszarami A i C, podczas gdy $\beta_2$ kwantyfikuje różnicę między obszarami B i C. Zauważ, że aktuariusz nie jest ograniczony do porównywania tylko z poziomem bazowym: różnice między obszarami A i B są kwantyfikowane przez $\beta_1 - \beta_2$.

Wybór poziomu bazowego należy do aktuariusza, ale nie powinien on być rzadki. Załóżmy na przykład, że nie ma posiadaczy polis w obszarze C. Wtedy dla każdego posiadacza polisy albo $x_{i1}$ lub $x_{i2}$ jest równe 1, a drugi jest równy 0. To z kolei oznacza, że $x_{i1} + x_{i2} = 1$ dla wszystkich $i$ i stąd macierz projektowa $\boldsymbol{X}$ jest osobliwa: suma dwóch ostatnich kolumn jest równa pierwszej (kolumna jedynek odpowiadająca wyrazowi wolnemu). To sprawia, że oszacowanie $\boldsymbol{\beta}$ jest niemożliwe, ponieważ algorytm IRLS musi odwrócić macierz $\boldsymbol{X}^\top \boldsymbol{X}$. Intuicyjnie, jedna z dwóch cech binarnych w takiej sytuacji jest redundantna, a model nie może rozwikłać ich odpowiednich współczynników regresji $\beta_1$ i $\beta_2$: każda modyfikacja w $\beta_1$ może być skompensowana przez odpowiednią zmianę w $\beta_2$ i odwrotnie.

Bardziej realistycznie, jeśli poziom bazowy ma bardzo mało przypadków, wtedy kolumny $\boldsymbol{X}$ są prawie zależne, co sprawia, że obliczenie $\boldsymbol{\beta}$ jest numerycznie niestabilne. Nie zawsze jest to brane pod uwagę w zautomatyzowanym kodowaniu implementowanym w oprogramowaniu, więc domyślny wybór musi być czasami modyfikowany przez aktuariusza.

Zauważ, że nie jest sensowne definiowanie cechy

$$
\text{area}_i = \begin{cases}
2 & \text{jeśli posiadacz polisy } i \text{ mieszka w obszarze A} \\
1 & \text{jeśli posiadacz polisy } i \text{ mieszka w obszarze B} \\
0 & \text{jeśli posiadacz polisy } i \text{ mieszka w obszarze C.}
\end{cases}
$$

Odpowiadający wynik GLM jest wtedy równy

$$
\beta_0 + \beta_{\text{area}}\text{area}_i = \begin{cases}
\beta_0 + 2\beta_{\text{area}} & \text{jeśli posiadacz polisy } i \text{ mieszka w obszarze A,} \\
\beta_0 + \beta_{\text{area}} & \text{jeśli posiadacz polisy } i \text{ mieszka w obszarze B,} \\
\beta_0 & \text{jeśli posiadacz polisy } i \text{ mieszka w obszarze C,}
\end{cases}
$$

więc ta specyfikacja implikuje równe „odstępy” między A, B i C: wynik $\beta_0 + \beta_{\text{area}}\text{area}_i$ daje różnicę $\beta_{\text{area}}$ między obszarami A i B, i taką samą różnicę $\beta_{\text{area}}$ między obszarami B i C. Niekoniecznie jest to zgodne z danymi i może obciążać analizę.

Nawet jeśli analiza GLM jest przeprowadzana warunkowo na wartościach $x_{ij}$ cech $X_{ij}$, struktura korelacji wektorów losowych $(X_{i1}, \dots, X_{ip})$ może wpłynąć na oszacowanie współczynników regresji. Korelacja między losowymi zmiennymi $X_{ij}$ oznacza, że niektóre cechy mogą służyć jako substytuty dla innych w obliczaniu wyniku.

### 4.4.4 Skorelowane Cechy, Współliniowość

Większość czasu, cechy $X_{ij}$ są skorelowane w badaniach obserwacyjnych. Gdy te korelacje są umiarkowane, GLM są w stanie wyizolować wpływ każdego czynnika ryzyka na średnią odpowiedź. Jest to jedna z wielkich zalet GLM w porównaniu z analizami jednowymiarowymi lub wielowymiarowymi, które są często zniekształcone, ponieważ ten sam efekt jest liczony wielokrotnie. Jednakże, gdy korelacja między niektórymi cechami staje się duża, GLM mogą napotkać problemy. Takie wysokie korelacje oznaczają, że ta sama informacja jest zakodowana w kilku cechach, co prowadzi do nadmiarowości informacji, jak wyjaśniono dalej.

Aliasing występuje, gdy istnieje liniowa zależność między cechami. Na przykład, jedna cecha może być identyczna z jakąś liniową kombinacją niektórych innych. Rozważmy następujące zmienne objaśniające:

$x_{i1}$ = wiek, w którym rozpoczęto prowadzenie pojazdu

$x_{i2}$ = staż prawa jazdy

$x_{i3}$ = obecny wiek posiadacza polisy $i$.

Oczywiście, tożsamość

$$
x_{i3} = x_{i1} + x_{i2}
$$

zachodzi dla wszystkich $i$, więc $x_{i3}$ jest redundantny w wyjaśnianiu odpowiedzi. Wynik można zapisać jako funkcję tylko dwóch cech. Na przykład, możemy napisać

$$
\beta_0 + \beta_1 x_{i1} + \beta_2 x_{i2} + \beta_3 x_{i3} = \beta_0 + (\beta_1 + \beta_3) x_{i1} + (\beta_2 + \beta_3) x_{i2}.
$$

Stąd, trzy zmienne $x_{i1}, x_{i2}$ i $x_{i3}$ wyjaśniają to samo co dowolne dwie z tych zmiennych. Zatem, indywidualne efekty każdej z trzech zmiennych nie mogą być ocenione.

Takie zmienne są doskonale współliniowe lub aliasowane. Aliasing odpowiada zatem liniowej zależności między kolumnami macierzy projektowej $\boldsymbol{X}$. Gdy dwie cechy są doskonale skorelowane, mówi się, że są aliasowane, a estymaty parametrów nie są unikalne. W tych przypadkach, macierz projektowa $\boldsymbol{X}$ ma dokładną zależność liniową między swoimi kolumnami i stąd $\boldsymbol{X}^\top \boldsymbol{X}$ jest osobliwa, co unieważnia podejście IRLS.

Oprócz dokładnej współliniowości zilustrowanej powyżej, częściej spotyka się sytuacje bliskiej współliniowości (lub prawie aliasowania). Na przykład, w zbiorze danych, w którym obszar zamieszkania, poziom dochodów i poziom wykształcenia posiadacza polisy są silnie skorelowane, włączenie trzech zmiennych objaśniających sprawia, że $\boldsymbol{X}^\top \boldsymbol{X}$ jest bliska osobliwości i powoduje trudności w obliczaniu odwrotności $(\boldsymbol{X}^\top \boldsymbol{X})^{-1}$.

Jeśli odwrotność jest obliczalna, zazwyczaj będzie miała duże wpisy diagonalne, co implikuje duże błędy standardowe dla jednego lub więcej współczynników regresji (z powodów wyjaśnionych w następnej sekcji). Współliniowość może również wprowadzać w błąd analityka w wyborze predykcyjnych zmiennych: jeśli 2 zmienne objaśniające są silnie skorelowane i obie są predykcyjne dla odpowiedzi, to każda z nich, przy obecności drugiej, nie wniesie wiele dodatkowych informacji do odpowiedzi. Testy hipotez dla każdej zmiennej, przy założeniu, że druga jest w modelu, pokażą brak istotności. Rozwiązaniem jest włączenie tylko jednej z tych dwóch zmiennych objaśniających do modelu. To wyjaśnia, dlaczego wstępne badanie struktury korelacji cech jest przydatne przed rozpoczęciem analizy GLM.

**Uwaga 4.4.1** (Współliniowość a Interakcja). Współliniowość pochodzi z korelacji między czynnikami ryzyka, z których kilka koduje te same informacje. Interakcja nie ma nic wspólnego ze współliniowością/korelacją. Na przykład, rozważmy portfel ubezpieczeń komunikacyjnych z taką samą strukturą wiekową dla mężczyzn i kobiet (tak, że czynniki ryzyka wiek i płeć są wzajemnie niezależne). Jeśli młodzi mężczyźni są bardziej niebezpieczni w porównaniu z młodymi kobietami, podczas gdy ta różnica zanika lub odwraca się w starszym wieku, to wiek i płeć oddziałują na siebie pomimo bycia niezależnymi.

### 4.4.5 V Craméra

Siłę zależności między dwiema zmiennymi ciągłymi można ocenić za pomocą współczynnika korelacji liniowej Pearsona (tj. kowariancji podzielonej przez iloczyn odchyleń standardowych) lub, preferencyjnie, za pomocą współczynników korelacji rang, takich jak rho Spearmana lub tau Kendalla. Jednak te klasyczne miary zależności nie są odpowiednie do oceny korelacji między cechami nieciągłymi. Biorąc pod uwagę różne formaty cech wchodzących do GLM (binarne, kategoryczne, dyskretne lub ciągłe), istnieje potrzeba miary korelacji mającej zastosowanie do wszystkich tych przypadków. Dlatego V Craméra jest często używane do mierzenia siły powiązania między cechami w badaniach ubezpieczeniowych. To podejście opiera się na danych wyświetlanych w formie tabelarycznej, tak że cechy dyskretne lub kategoryczne mogą być traktowane bezpośrednio. Cechy ciągłe muszą być skutecznie traktowane (lub grupowane) przez podział ich dziedziny na rozłączne przedziały i stąd traktowane jako dyskretne.

Rozważmy na przykład wiek i płeć posiadacza polisy. Wiek jest najpierw kategoryzowany w kilku klasach: na przykład, kategoria „młody kierowca” grupująca posiadaczy polis w wieku poniżej 25 lat, kategoria „w średnim wieku” grupująca posiadaczy polis w wieku od 26 do 50 lat i kategoria „senior” grupująca posiadaczy polis w wieku 51 lat i starszych. Portfel można następnie podzielić na 6 klas, krzyżując 3 wspomniane kategorie wiekowe z 2 płciami. W każdej komórce aktuariusz rejestruje obserwowane odpowiedzi i związaną z nimi ekspozycję. Jednak takie wstępne grupowanie zmiennej ciągłej (jak wiek w naszym przykładzie) jest subiektywne (wybór punktów odcięcia jest generalnie nieco arbitralny przez analityka) i może prowadzić do możliwej utraty informacji. Nie są dostępne ogólne optymalne punkty odcięcia.

V Craméra opiera się na tablicy kontyngencji klasyfikującej krzyżowo dane według pary cech w badaniu. Przypomnijmy, że statystyka Chi-Kwadrat dla testowania niezależności między dwiema cechami w tablicy kontyngencji, oznaczona jako $\chi^2$, jest otrzymywana przez zsumowanie kwadratów różnic między obserwowaną częstotliwością a oczekiwaną częstotliwością w każdej komórce tabeli, podzielonych przez oczekiwaną częstotliwość. Oczekiwana częstotliwość w każdej komórce odpowiada niezależności między dwiema cechami. Jest ona otrzymywana przez pomnożenie sumy wiersza przez sumę kolumny, podzieloną przez sumę całkowitą. Zauważ, że ekspozycje muszą być uwzględnione w obliczeniach.

Teraz, biorąc pod uwagę dwie cechy $X_{ij_1}$ i $X_{ij_2}$ przyjmujące odpowiednio $k_1$ i $k_2$ wartości dyskretnych, V Craméra jest zdefiniowane jako

$$
V = \sqrt{\frac{\chi^2/n}{\min\{k_1-1, k_2-1\}}} \in [0, 1]
$$

gdzie $\chi^2$ jest statystyką testu Chi-Kwadrat Pearsona dla niezależności cech $X_{ij_1}$ i $X_{ij_2}$. Dzielenie $\chi^2$ przez liczbę obserwacji sprawia, że statystyka jest niezależna od liczby obserwacji, tj. mnożenie każdej komórki tablicy kontyngencji przez dodatnią liczbę całkowitą nie zmienia wartości V Craméra. Co więcej, V Craméra przyjmuje wartości w przedziale jednostkowym [0, 1], przy czym 0 i 1 odpowiadają niezależności i doskonałej zależności. Będąc opartym na danych wyświetlanych w formie tabelarycznej, V Craméra jest szeroko stosowane.

Co więcej, maksimum $\chi^2/n$ jest osiągane, gdy istnieje całkowita zależność, tj. każdy wiersz (lub kolumna, w zależności od tego, na której stronie tabeli) wykazuje tylko jeden ściśle dodatni wiersz. Oznacza to, że wartość $X_{ij_1}$ determinuje wartości $X_{ij_2}$. Ta maksymalna wartość $\chi^2/n$ jest równa najmniejszemu wymiarowi minus 1. Zatem, dzielenie $\chi^2/n$ przez $\min\{k_1-1, k_2-1\}$ zapewnia, że V przyjmuje wartości w przedziale jednostkowym [0, 1]. Wyciągnięcie pierwiastka kwadratowego gwarantuje, że V Craméra i współczynnik korelacji liniowej Pearsona pokrywają się w tabelach 2x2 (tj. dla dwóch cech binarnych, z $k_1=k_2=2$).

**Przykład 4.4.2** Rozważmy ponownie dane wyświetlone w Tabeli 4.5. Ten hipotetyczny zbiór danych jest sklasyfikowany krzyżowo według dwóch zmiennych kategorycznych (z dwoma kategoriami zdefiniowanymi w odniesieniu do progu 20 000 km). V Craméra jest równe 0.533, co wskazuje na dość silną dodatnią korelację między płcią a przejechanym dystansem, mężczyźni posiadający polisy mają tendencję do przejeżdżania dłuższych dystansów. Dla danych w Tabeli 4.1, stwierdzamy, że V Craméra jest równe 0.123, więc powiązanie jest słabsze.

### 4.4.6 Błąd Pominiętej Zmiennej

Z Rozdziału 2 wiemy, że aktuariusz nie może pominąć ważnych informacji o ryzyku posiadaczy polis. Jeśli ukryte czynniki ryzyka są skorelowane zarówno z odpowiedzią, jak i z jedną lub więcej dostępnymi cechami, będą one obciążać estymatę odpowiedniego współczynnika regresji. Zjawisko to jest powszechnie znane jako błąd pominiętej zmiennej i można je wyjaśnić w następujący sposób.

Zilustrujmy teraz efekt pominiętej zmiennej na prostym przykładzie. Rozważmy ponownie zbiór danych wyświetlony w Tabeli 4.5 sklasyfikowany krzyżowo według płci i rocznego przebiegu (z punktem odcięcia 20 000 km). Ponieważ obie cechy są kategoryczne z dwoma poziomami, można je zakodować za pomocą dwóch cech binarnych. Dokładniej, $x_{i1}$ rejestruje roczny przebieg, z

$$
x_{i1} = \begin{cases}
0 & \text{jeśli posiadacz polisy } i \text{ przejeżdża więcej niż 20,000 km rocznie} \\
1 & \text{w przeciwnym razie}.
\end{cases}
$$

Podobnie, $x_{i2}$ rejestruje płeć, z

$$
x_{i2} = \begin{cases}
0 & \text{jeśli posiadacz polisy } i \text{ jest mężczyzną} \\
1 & \text{w przeciwnym razie}.
\end{cases}
$$

Tutaj wybraliśmy poziomy bazowe odpowiadające najbardziej zaludnionym kategoriom: mężczyzna dla płci i ponad 20 000 km rocznie dla przejechanego dystansu.

Włączając obie binarne cechy kodujące informacje w odniesieniu do tej polisy ($x_{i1}$ dla płci i $x_{i2}$ dla rocznego przebiegu), IRLS daje następujące wyniki po trzech iteracjach:

$$
\begin{aligned}
\hat{\beta}_0 &= -2.20597 \\
\hat{\beta}_1 &= -0.54753 \\
\hat{\beta}_2 &= -0.26383.
\end{aligned}
$$

Teraz, jeśli płeć zostanie usunięta z oceny, zachowując tylko przejechany dystans, otrzymujemy po trzech iteracjach:

$$
\begin{aligned}
\hat{\beta}_0 &= -2.24904 \\
\hat{\beta}_1 &= -0.69552.
\end{aligned}
$$

Zatem widzimy, że oszacowane współczynniki regresji są modyfikowane w porównaniu z oszacowaniami uzyskanymi z płcią w modelu. W szczególności, oszacowany współczynnik regresji $\hat{\beta}_1$ związany z przejechanym dystansem staje się bardziej ujemny, zmieniając się z -0.55 na -0.70. Można to wyjaśnić korelacją istniejącą między dwiema cechami. Wiemy, że korelacja jest dodatnia, na co wskazuje stosunkowo wysoka wartość V Craméra uzyskana w Przykładzie 4.4.2. Słownie, mężczyźni posiadający polisy mają tendencję do przejeżdżania więcej niż 20 000 km rocznie, podczas gdy kobiety posiadające polisy mają tendencję do przejeżdżania mniej. Stąd, jeśli wiemy, że posiadacz polisy przejeżdża mniej niż 20 000 km rocznie, jest prawdopodobne, że jest to kobieta. Formalnie,

$$
\text{P}[\text{kobieta} | < 20,000 \text{ km}] = \frac{6,000}{6,000+2,000} = \frac{3}{4}.
$$

Stąd, $\hat{\beta}_1$ staje się bardziej ujemny, gdy płeć jest pominięta w ocenie, ponieważ przejmuje negatywny wpływ bycia kobietą na ocenę.

Wstępne badanie powiązań istniejących między dostępnymi cechami wskazuje, że włączenie lub przesunięcie (offsetting) jednej z nich będzie miało największy wpływ na te cechy, które są silnie skorelowane z pominiętą.

## 4.5 Dewiancja

### 4.5.1 Model Zerowy

Model zerowy odpowiada przypadkowi, w którym cechy nie wnoszą żadnych informacji o odpowiedzi. Wobec braku związku między cechami a odpowiedzią, dopasowujemy wspólną średnią $\mu$ do wszystkich obserwacji. Jest to więc pojedynczy parametr $\beta_0$ dla wszystkich obserwacji, taki że

$$
a'(\theta_i) = \bar{y} \iff \hat{\mu}_i = \bar{y} \text{ dla } i=1, 2, \dots, n \iff \hat{\beta}_0 = g^{-1}(\bar{y}).
$$

Wynik ten bezpośrednio wynika z właściwości estymat największej wiarygodności dla rozkładów ED ustalonych w Rozdz. 3.

W modelu zerowym dane są reprezentowane w całości jako losowe wahania wokół wspólnej średniej $\mu$. Jeśli model zerowy ma dobre dopasowanie, dane są jednorodne i nie ma powodu, aby naliczać różne składki premium podgrupom ubezpieczonych.

### 4.5.2 Model Pełny, czyli Nasycony

Dla wybranego rozkładu ED, model o najlepszym możliwym dopasowaniu liczy tyle samo parametrów co obserwacji. Dopasowanie jest więc idealne, a model estymuje $\mu_i$ dla średniej odpowiedzi jako odpowiadającą jej obserwację $y_i$. Ten model nazywany jest modelem pełnym lub nasyconym. Jest on używany jako odniesienie do oceny dobroci dopasowania dowolnego GLM opartego na tym samym rozkładzie ED.

Model pełny zapewnia całkowitą elastyczność w dopasowaniu, tak że odpowiadająca estymata największej wiarygodności $\hat{\theta}_i$ rozwiązuje

$$
a'(\tilde{\theta}_i) = y_i \iff \tilde{\mu}_i = y_i \text{ dla } i=1, 2, \dots, n,
$$

jak ustalono w Rozdz. 2. Rozwiązanie jest oznaczone jako

$$
\tilde{\theta}_i = (a')^{-1}(y_i), \quad i=1, 2, \dots, n.
$$

Zatem każda dopasowana wartość jest równa obserwacji, a model pełny idealnie pasuje. Jednakże model ten nie wydobywa z danych żadnej struktury, a jedynie powtarza dostępne obserwacje, nie kondensując ich.

### 4.5.3 Dewiancja

#### 4.5.3.1 Statystyka Ilorazu Wiarygodności

Model statystyczny opisuje, jak aktuariusz dzieli zmienność obecną w danych na strukturę systematyczną (reprezentowaną w wyniku GLM) i losowe odchylenia od wartości oczekiwanych (wahania wywołane przez założony rozkład ED, lub relację średnia-wariancja). Model zerowy reprezentuje jedną skrajność, gdzie dane są czysto losowe, podczas gdy model nasycony lub pełny reprezentuje dane jako całkowicie systematyczne. Model pełny zapewnia aktuariuszowi miarę tego, jak dobrze jakikolwiek model oparty na rozważanym rozkładzie ED może dopasować dane. Ponieważ model pełny daje najwyższą osiągalną log-wiarygodność z rozkładem ED w ramach rozważań, różnica między log-wiarygodnością $L_{\text{full}}$ modelu pełnego a log-wiarygodnością $L(\hat{\boldsymbol{\beta}})$ uzyskaną z tego GLM mierzy dobroć dopasowania. Prowadzi to do statystyki ilorazu wiarygodności odpowiadającej dwukrotności tej różnicy.

#### 4.5.3.2 Dewiancja

Podczas pracy z GLM, przydatne jest posiadanie wielkości, którą można interpretować jako sumę kwadratów błędów (lub reszt) w modelu regresji normalnej. Ta wielkość nazywana jest dewiancją, czyli dewiancją resztową modelu i jest zdefiniowana jako

$$
\begin{aligned}
D(\boldsymbol{y}, \boldsymbol{\hat{\mu}}) &= \phi LR \\
&= 2\phi(L_{\text{full}} - L(\hat{\boldsymbol{\beta}}))
\end{aligned}
$$

Jest to miara odległości między konkretnym modelem a obserwowanymi danymi zdefiniowanymi za pomocą modelu nasyconego. Podobnie do sumy kwadratów błędów, kwantyfikuje ona wahania w danych, które nie są wyjaśnione przez rozważany model.

---
**Tabela 4.7** Dewiancja związana z GLM opartymi na niektórych członkach rodziny rozkładów ED.

| Rozkład | Dewiancja |
| :--- | :--- |
| Poissona | $2 \sum_{i=1}^n (y_i \ln \frac{y_i}{\hat{\mu}_i})$ gdzie $y \ln y = 0$ jeśli $y=0$ |
| Normalny | $\sum_{i=1}^n (y_i - \hat{\mu}_i)^2$ |
---

Ponieważ model nasycony musi pasować do danych co najmniej tak samo dobrze jak każdy inny model, dewiancja resztowa nigdy nie jest ujemna. Im większa dewiancja, tym większe różnice między rzeczywistymi danymi a dopasowanymi wartościami. Dewiancja modelu zerowego nazywana jest dewiancją zerową.

Tabela 4.7 przedstawia dewiancję związaną z GLM opartymi na niektórych członkach rodziny rozkładów ED. Drugi składnik $\sum_{i=1}^n (y_i - \hat{\mu}_i)$ w dewiancji Poissona jest zwykle równy 0, gdy w ocenie uwzględniony jest wyraz wolny, więc dewiancja Poissona redukuje się do $2\sum_{i=1}^n y_i \ln \frac{y_i}{\hat{\mu}_i}$. W przypadku Poissona, dewiancja jest również nazywana statystyką G.

#### 4.5.3.3 Dewiancja Skalowana

Dla rodzin z znanym parametrem dyspersji $\phi$, takich jak dwumianowy lub Poissona, dla których $\phi=1$, dewiancja stanowi podstawę do testowania braku dopasowania modelu lub do porównywania zagnieżdżonych modeli. Jeśli $\phi$ musi być oszacowane na podstawie danych, wtedy należy użyć dewiancji skalowanej. Dokładniej, dewiancja skalowana jest dana przez

$$
\tilde{D}(\boldsymbol{y}, \boldsymbol{\hat{\mu}}) = \frac{1}{\phi} D(\boldsymbol{y}, \boldsymbol{\hat{\mu}}).
$$

Zauważ, że dewiancja skalowana zależy od parametru dyspersji $\phi$. Gdy $\phi=1$ (podobnie jak w przypadkach dwumianowym i Poissona), dewiancja i dewiancja skalowana są takie same. Powszechną praktyką jest po prostu podłączenie oszacowania $\hat{\phi}$ w celu obliczenia $\tilde{D}$.

#### 4.5.3.4 Rozkład Próbkowy Dewiancji Skalowanej

Koncepcja dewiancji rozciąga się na wszystkich członków rodziny ED znaną sumę kwadratów reszt używaną w regresji liniowej normalnej. Liczba stopni swobody związana z dewiancją skalowaną jest równa liczbie $n$ obserwacji minus liczba $p+1$ parametrów regresji $\beta_0, \beta_1, \dots, \beta_p$ do oszacowania. W dużych próbach, dewiancja skalowana ma w przybliżeniu rozkład Chi-kwadrat z $n-(p+1)$ stopniami swobody, tj.

$$
\tilde{D} \approx \chi^2_{n-p-1} \text{ pod warunkiem, że } n \text{ jest wystarczająco duże.}
\quad\quad\text{(4.16)}
$$

Duża dewiancja wskazuje na źle dopasowany model. Dokładniej, jeśli GLM pasuje do danych rozsądnie dobrze, to skalowana dewiancja $\tilde{D}$ powinna być bliska liczbie resztowych stopni swobody, tj. $n-p-1$.

Zauważ, że do aproksymacji Chi-kwadrat w (4.16) należy podchodzić z pewną ostrożnością. Istnieją sytuacje, w których nie jest ona w ogóle zachowana, na przykład w przypadku odpowiedzi binarnych (jak pokazano w następnej sekcji).

### 4.5.4 Przypadek Bernoulliego

Maksymalizacja log-wiarygodności lub minimalizacja dewiancji jest równoważna z punktu widzenia numerycznego, z wyjątkiem niektórych szczególnych przypadków, jak pokazano dalej. Załóżmy, że $Y_i \sim \mathcal{Ber}(q_i)$. 

$$
D = -2 \sum_{i=1}^n (\hat{q}_i \text{logit}(\hat{q}_i) + \ln(1 - \hat{q}_i)).
$$

Widzimy, że dewiancja $D$ zależy tylko od dopasowanych wartości $\hat{q}_i$ z $q_i$, a nie od rzeczywistych obserwacji $y_i$. Dla dewiancji, aby zmierzyć dobroć dopasowania, musi ona porównywać dopasowane wartości $\hat{q}_i$ z danymi $y_i$, ale tutaj mamy tylko funkcję $\hat{q}_i$. To pokazuje, że dewiancja nie wnosi żadnych informacji o dobroci dopasowania, gdy $Y_i \sim \mathcal{Ber}(q_i)$ i używana jest funkcja łącząca logit. Stąd nie można jej użyć do pomiaru adekwatności modelu w takim przypadku.

### 4.5.5 Kary Kowariancyjne i AIC

#### 4.5.5.1 Pomiar Dobroci Dopasowania

Zagnieżdżone modele można porównywać za pomocą dewiancji. Jednakże, gdy modele nie są zagnieżdżone lub postulują różne rozkłady dla odpowiedzi, bezpośrednie porównanie staje się problematyczne. W takim przypadku, kryteria informacyjne pozwalają aktuariuszowi na porównywanie różnych modeli. Klasyczne kryteria obejmują Kryterium Informacyjne Akaikego

$$
AIC = -2L + 2\#\text{parametrów},
$$

i Bayesowskie Kryterium Informacyjne

$$
BIC = -2L + \ln(n)\#\text{parametrów}.
$$

Zauważ, że liczba parametrów uwzględnia wszystkie parametry zawarte w modelu (parametr dyspersji $\phi$ jest również liczony jako parametr w oparciu o log-wiarygodność $L$ i odpowiada za miarę złożoności modelu). Preferowane są mniejsze wartości AIC lub BIC.

Powszechną praktyką jest odrzucanie części wiarygodności, które nie są funkcjami parametrów. To jest wspólne do porównywania modeli zakładających ten sam rozkład dla odpowiedzi, ponieważ odrzucone części są równe w takim przypadku. Dla odpowiedzi o różnych rozkładach, istotne jest jednak, aby wszystkie części wiarygodności zostały zachowane.

Skorygowana wersja AIC (oznaczona jako AICC) została opracowana do użytku w małych próbach lub gdy liczba parametrów $p+1$ jest umiarkowaną do dużej częścią wielkości próby $n$.

## 4.6 Estymacja Parametru Dyspersji

Niektóre rozkłady z rodziny ED zawierają nieznany parametr dyspersji $\phi$. Dzieje się tak na przykład w przypadku rozkładów Gamma lub Odwrotnego Gaussa. Równania wiarygodności dla $\boldsymbol{\beta}$ nie zawierają parametru dyspersji $\phi$, więc nie jest konieczne szacowanie $\phi$, aby uzyskać $\boldsymbol{\hat{\beta}}$. Jednakże, oszacowany parametr dyspersji jest potrzebny do przeprowadzenia wnioskowania na temat współczynników regresji. Chociaż $\phi$ w zasadzie może być oszacowane metodą największej wiarygodności, częściej stosuje się alternatywny estymator wyprowadzony z metody momentów, który jest zdefiniowany w następujący sposób.

Gdy algorytm IRLS wygeneruje $\boldsymbol{\hat{\beta}}$, parametr dyspersji $\phi$ można oszacować, dzieląc statystykę dobroci dopasowania Pearsona

$$
X^2 = \sum_{i=1}^n \frac{(y_i - \hat{\mu}_i)^2}{V(\hat{\mu}_i)/v_i}
$$

przez resztowe stopnie swobody modelu, to jest,

$$
\hat{\phi} = \frac{1}{n-p-1} \sum_{i=1}^n \frac{v_i(y_i - \hat{\mu}_i)^2}{V(\hat{\mu}_i)} = \frac{X^2}{n-p-1}.
$$

Estymator ten wykorzystuje fakt, że $X^2$ Pearsona ma asymptotycznie rozkład Chi-kwadrat z $n-p-1$ stopniami swobody. Jest on znacznie prostszy do obliczenia i w niektórych przypadkach oferuje większą stabilność numeryczną niż estymata największej wiarygodności.

Dewiancja dostarcza alternatywnego estymatora dla $\phi$. Biorąc pod uwagę (4.16), widzimy, że średnia $D$ jest w przybliżeniu równa $n-p-1$. To sugeruje użycie estymaty dla dużych prób

$$
\hat{\phi} = \frac{D}{n-p-1}.
$$

## 4.7 Rozkład z próby oszacowanych współczynników regresji

### 4.7.4 Przedziały ufności Walda

Podejście Walda opiera się na normalnej aproksymacji rozkładu $\boldsymbol{\hat{\beta}}$. Błąd standardowy jest pierwiastkiem kwadratowym z oszacowanej wariancji $\text{Var}[\hat{\beta}_j]$. Tę wielkość należy rozumieć w następujący sposób. Gdyby pobrano nowe próbki, odchylenie standardowe wynikowych oszacowań $\hat{\beta}_j$ byłoby w przybliżeniu oszacowanym błędem standardowym. Dlatego mały błąd standardowy wskazuje, że $\hat{\beta}_j$ powinien być bliski prawdziwemu $\beta_j$, podczas gdy duży błąd standardowy sugeruje, że szeroki zakres oszacowań można by osiągnąć przez losowość.

Przedział ufności można uznać za rozsądny zakres oszacowań dla współczynnika interesującego. Przedziały ufności Walda opierają się na rozkładzie $\hat{\beta}_j$ dla dużej próby, to jest,

$$\hat{\beta}_j - \beta_j \approx \mathcal{Nor}(0, \hat{\sigma}_{jj}^2)$$

gdzie $\hat{\sigma}_{jj}$ jest elementem $(j, j)$ macierzy $\hat{\boldsymbol{\Sigma}}(\boldsymbol{\hat{\beta}})$, to jest oszacowaną wariancją $\hat{\beta}_j$. Przybliżony przedział ufności na poziomie $1 - \alpha$ jest następnie uzyskiwany w następujący sposób:

$$\begin{align*}
1 - \alpha &\approx \text{P} \left[ -z_{\alpha/2} \sqrt{\hat{\sigma}_{jj}} \leq \hat{\beta}_j - \beta_j \leq z_{\alpha/2} \sqrt{\hat{\sigma}_{jj}} \right] \\
&= \text{P} \left[ \hat{\beta}_j - z_{\alpha/2} \sqrt{\hat{\sigma}_{jj}} \leq \beta_j \leq \hat{\beta}_j + z_{\alpha/2} \sqrt{\hat{\sigma}_{jj}} \right]
\end{align*}$$

tak, że przedział

$$\text{IC}_{1-\alpha}(\beta_j) = \left[ \hat{\beta}_j \pm z_{\alpha/2} \sqrt{\hat{\sigma}_{jj}} \right]$$

oczekuje się, że będzie zawierał prawdziwą wartość parametru z prawdopodobieństwem w przybliżeniu równym $1-\alpha$. Tutaj, prawdopodobieństwo $1-\alpha$ jest (przybliżonym) poziomem ufności, lub pokryciem prawdopodobieństwa odpowiadającego przedziału ufności.

### 4.7.5 Współczynnik inflacji wariancji

Współliniowość odnosi się do sytuacji, w której dwie lub więcej cech jest silnie predykcyjnych względem trzeciej. Jednak ta ostatnia może nie być silnie skorelowana z żadną z poprzednich, więc zjawisko to nie pojawia się w macierzy korelacji lub macierzy V Craméra w przypadku cech o różnych formatach. Dzieje się tak, ponieważ taka macierz uwzględnia tylko pary cech, a nie większe podzbiory dostępnych cech. W konsekwencji współliniowość jest znacznie trudniejsza do wykrycia.

Gdy istnieją silne zależności liniowe między cechami w analizie regresji, dokładność estymacji dla $\beta_j$ maleje w porównaniu z przypadkiem, w którym cechy są niezależne. Głównym powodem jest to, że macierz $\boldsymbol{X}^T \boldsymbol{W} \boldsymbol{X}$ zaangażowana w każdy krok algorytmu IRLS jest źle uwarunkowana, a zatem prawie nieodwracalna.
Użyteczną statystyką do wykrywania współliniowości jest współczynnik inflacji wariancji (VIF). VIF dla dowolnej cechy mierzy wzrost kwadratu błędu standardowego dla współczynnika regresji z powodu obecności współliniowości w danych. Niech $\rho_j$ oznacza współczynnik determinacji z regresji $j$-tej cechy na pozostałych $p-1$ cechach. Jest on określany przez uruchomienie modelu liniowego dla każdej z pozostałych cech przy użyciu wszystkich innych cech jako danych wejściowych i pomiar jego dokładności predykcyjnej.

Współczynnik inflacji wariancji zdefiniowany jako

$$\text{VIF} = \frac{1}{1 - \hat{\rho}_j^2}$$

określa ilościowo wzrost wariancji $\hat{\beta}_j$ z powodu zależności liniowej $j$-tej cechy od pozostałych $p-1$. VIF jest najprostszym i najbardziej bezpośrednim wskaźnikiem szkód wyrządzonych przez współliniowość. W szczególności pierwiastek kwadratowy z VIF wskazuje, o ile większy jest przedział ufności dla $\hat{\beta}_j$ w porównaniu z podobnymi nieskorelowanymi danymi (takie dane mogą być czysto koncepcyjne). Silne zależności liniowe odpowiadają cechom o dużym VIF. Jako punkt odniesienia, poważny problem współliniowości istnieje, gdy VIF jest większy niż 10. VIF nie stosuje się do zestawów współczynników regresji (odpowiadających różnym poziomom cech kategorialnych, na przykład), ale w tym przypadku dostępne są uogólnione VIF, lub GVIF.

Pomimo ich oczywistego znaczenia w badaniach GLM, obliczanie miar VIF wydaje się być intensywne obliczeniowo w wielu analizach obejmujących wiele cech, biorąc pod uwagę obecną moc obliczeniową. To nieco ogranicza praktyczne zastosowanie tej koncepcji.

### 4.7.6 Testowanie hipotez dotyczących parametrów

#### 4.7.6.1 Hipotezy dotyczące pojedynczego współczynnika regresji

Aby przetestować, czy założenie $\beta_j = b$ jest rozsądne, dla pewnej stałej $b$, możemy obliczyć statystykę Walda

$$z = \frac{\hat{\beta}_j - b}{\sqrt{\widehat{\text{Var}}[\hat{\beta}_j]}}.$$

Przyjęcie tej procedury oznacza, że wybrany $\alpha$ kwantyfikuje ryzyko odrzucenia założenia $\beta_j = b$, podczas gdy w rzeczywistości jest ono prawdziwe (nazywane błędem typu 1 w statystyce). Zgodnie z tym założeniem, statystyka testowa $z$ jest w przybliżeniu rozłożona $\mathcal{Nor}(0, 1)$, tak że istnieje prawdopodobieństwo $\alpha$, że $|z|$ przekroczy $\Phi^{-1}(1-\alpha/2)$, prowadząc do odrzucenia. Takie przypadki odpowiadają sytuacjom, w których założenie nie powinno być odrzucane. Stąd $\alpha$ reprezentuje poziom bezpieczeństwa wybrany przez aktuariusza przy odrzucaniu rozważanego założenia. Poziom $\alpha=5\%$ jest rutynowo stosowany w praktyce. Prowadzi to do 1 błędnego wniosku na 20 testów prowadzących do odrzucenia, średnio (pod warunkiem, że testy są przeprowadzane niezależnie). W zależności od wagi wniosku wyciągniętego z analizy, aktuariusz może zmniejszyć ryzyko błędu (do 1%, na przykład), lub być mniej konserwatywny (zwiększając $\alpha$ do 10%, powiedzmy).

Oprogramowanie statystyczne, w tym R, generalnie zgłasza wyniki procedur testowych w postaci p-wartości, aby uniknąć proszenia użytkowników o podanie poziomu ufności $\alpha$, który ma być zastosowany. Na podstawie obliczonej p-wartości, aktuariusz następnie odrzuca założenie, gdy spada ono poniżej wybranego $\alpha$, i zachowuje je jako założenie robocze w przeciwnym razie. Formalnie jednak aktuariusz nigdy nie może zaakceptować założenia (stąd subtelna różnica między akceptacją a nieodrzuceniem). Dzieje się tak dlatego, że inne źródło błędu, zwane błędem typu 2 w statystyce, polegające na akceptowaniu założenia, gdy w rzeczywistości jest ono fałszywe, na ogół nie jest kontrolowane przez procedurę testową.

Zauważ, że standardowe testy dla hipotezy zerowej $H_0: \beta_j = 0$ są automatycznie dostarczane przez oprogramowanie statystyczne. Gdy cechy kategorialne są zaangażowane, $\beta_j = 0$ oznacza, że odpowiedni poziom jest identyczny z poziomem bazowym lub referencyjnym dla tej cechy (która jest uwzględniona w wyrazie wolnym $\beta_0$). Nieodrzucenie $H_0$ sugeruje, że poziom powinien zostać połączony z poziomem bazowym. Ale inne grupowania mogą być sensowne i mogą być wykryte przez testowanie hipotez postaci

$$H_0: \beta_j = \beta_k \Leftrightarrow \hat{\beta}_j - \boldsymbol{\hat{\beta}}_k = 0.$$

Gdy w ocenie uwzględniony jest wyraz wolny, procedura analizy GLM przetwarza zmienne ryzyka w odniesieniu do poziomu odniesienia (którego efekt odpowiada wyrazowi wolnemu). Przełączanie z jednego poziomu odniesienia na inny nie modyfikuje przewidywań modelu (pomimo różnych oszacowanych współczynników regresji uzyskanych, ponieważ poziomy są teraz w odniesieniu do innego poziomu bazowego). Jednak p-wartości się zmieniają, ponieważ kwestionują różnicę danego poziomu w porównaniu z poziomem bazowym: zmiana poziomu bazowego modyfikuje testowaną hipotezę. Dlatego ważne jest, aby sprawdzić, czy poziom bazowy ma znaczną ekspozycję i nie przyjmować domyślnego poziomu bazowego przypisanego przez oprogramowanie. To jest właśnie wybór dokonany we wszystkich przykładach w tej książce.

**Uwaga 4.7.1** Znaczenie każdej zmiennej jest wyrażone przez wielkość jej wkładu w ocenę. W GLM jest to zasadniczo odzwierciedlone przez wielkość powiązanego $\beta_j$ (zakładając, że wszystkie predyktory zostały znormalizowane). Istotność $j$-tej cechy można następnie przetestować za pomocą hipotezy zerowej $H_0: \beta_j=0$.
Innym sposobem postępowania, który rozszerza się na bardziej wyszukane podejścia do uczenia statystycznego, jest wybranie losowej frakcji $\pi$ oryginalnych danych i wykorzystanie ich do dopasowania GLM. Aktuariusz następnie używa dopasowanego modelu do przewidywania odpowiedzi w pozostałej $1-\pi$ części bazy danych i oblicza dokładność tej predykcji. Wartości $j$-tej cechy są następnie losowo permutowane, a odpowiedź jest ponownie przewidywana na tej podstawie. Chodzi o to, aby porównać dokładność predykcji z i bez permutacji: im ważniejsza cecha, tym większa różnica w tych dwóch miarach, permutacja $j$-tej cechy powodująca duży spadek dokładności predykcji.

#### 4.7.6.2 Hipotezy dotyczące kilku współczynników regresji

Oprócz prostych hipotez dotyczących pojedynczego parametru, można również rozważyć bardziej rozbudowane. Rozważmy model bazowy

$$\mathcal{M}_0: \boldsymbol{\beta} = \boldsymbol{\beta}_0 = (\beta_1, \beta_2, \ldots, \beta_q)^T$$

i model alternatywny

$$\mathcal{M}_1: \boldsymbol{\beta} = \boldsymbol{\beta}_1 = (\beta_1, \beta_2, \ldots, \beta_q, \beta_{q+1}, \ldots, \beta_p)^T$$

zawierający dodatkowe zmienne objaśniające. W ramach $\mathcal{M}_0$ cechy $x_{i, q+1}, \ldots, x_{i,p}$ nie mają wpływu na oczekiwane odpowiedzi. Oznacza to, że $\mathcal{M}_0$ odpowiada hipotezie zerowej

$$H_0: \beta_{q+1} = \ldots = \beta_p = 0$$

podczas gdy hipoteza alternatywna jest taka, że co najmniej jeden współczynnik regresji wśród $\beta_{q+1}, \ldots, \beta_p$ jest niezerowy.
Gdy GLM używa podzbioru cech większego modelu, mniejszy model mówi się, że jest zagnieżdżony w większym. W przypadku $\mathcal{M}_0$ zagnieżdżonego w $\mathcal{M}_1$. Dla GLM bez parametru dyspersji (tj. $\phi = 1$ jak w przypadkach dwumianowym i Poissona), statystyka testu ilorazu wiarygodności jest po prostu różnicą w dewiancjach dla zagnieżdżonych modeli $\mathcal{M}_0$ i $\mathcal{M}_1$. Oznaczając $D_0$ i $D_1$ odpowiednie dewiancje związane z $\mathcal{M}_0$ i $\mathcal{M}_1$, statystyka testowa jest

$$\Delta = D_0 - D_1 = 2(L(\boldsymbol{\hat{\beta}}_1) - L(\boldsymbol{\hat{\beta}}_0))$$

gdzie $\boldsymbol{\hat{\beta}}_0$ i $\boldsymbol{\hat{\beta}}_1$ są estymatorami największej wiarygodności dla $\boldsymbol{\beta}$ w ramach odpowiednio $\mathcal{M}_0$ i $\mathcal{M}_1$. Jeśli ograniczenia nałożone na model $\mathcal{M}_1$ reprezentowane przez $\mathcal{M}_0$ są prawdziwe, to jest, jeśli $H_0$ jest prawdziwe, to statystyka testowa $\Delta$ jest w przybliżeniu rozłożona chi-kwadrat, to jest, $\Delta \approx \chi_{p-q}^2$ pod warunkiem, że wielkość próby jest wystarczająco duża.
Musimy jednak pamiętać, że dodanie cech do modelu zawsze zmniejsza dewiancję (nawet jeśli dodana cecha nie jest związana z odpowiedzią), po prostu dlatego, że w większym modelu zaangażowanych jest więcej parametrów, który może tylko lepiej dopasować dane. Sensownym pytaniem jest zatem, czy dodatkowe cechy znacznie zmniejszają dewiancję, tj. czy nie były one związane z odpowiedzią. Przybliżony rozkład chi-kwadrat $\chi_{p-q}^2$ rządzący $D_0 - D_1$ definiuje próg, powyżej którego różnica w obserwowanych dewiancjach jest wystarczająco duża, aby uzasadnić włączenie dodatkowych cech. Formalnie, jeśli model $\mathcal{M}_1$ jest prawdziwym modelem, to będzie miał tendencję do posiadania znacznie wyższej wiarygodności w porównaniu z modelem $\mathcal{M}_0$, tak że różnica w log-wiarygodnościach będzie zbyt duża. Zatem $H_0$ jest odrzucane, jeśli $\Delta_{obs}$ przekracza statystykę testową $\Delta$ jest "za duża". W dużych próbach $H_0$ jest odrzucane na poziomie prawdopodobieństwa $1-\alpha$, gdzie $\chi_{p-q; 1-\alpha}^2$ oznacza kwantyl rozkładu $\chi_{p-q}^2$.

Dla GLM, w których występuje parametr dyspersji do oszacowania (jak w przypadkach normalnym, gamma i odwrotnym gaussowskim), możemy porównać zagnieżdżone modele za pomocą testu F, opartego na statystyce testowej

$$F = \frac{\frac{D_0-D_1}{p-q}}{\hat{\phi}}$$

obejmując odpowiednie przeskalowane dewiancje $\frac{D_0}{\hat{\phi}}$ i $\frac{D_1}{\hat{\phi}}$. Tutaj, oszacowany parametr dyspersji $\hat{\phi}$ jest pobierany z największego dopasowanego modelu do danych (który niekoniecznie jest modelem $\mathcal{M}_1$). Jeśli największy model ma $k+1$ współczynników regresji, to przy założeniu, że ograniczenia nałożone na model $\mathcal{M}_1$ przez model $\mathcal{M}_0$ są poprawne, $F$ w przybliżeniu podlega rozkładowi Fishera z $p-q$ i $n-k-1$ stopniami swobody.

### 4.7.7 Ilustracje numeryczne

#### 4.7.7.1 Graduacja rocznych prawdopodobieństw zgonu

Wróćmy do problemu graduacji rocznych prawdopodobieństw zgonu przy użyciu formuły (4.15). Przypomnijmy, że odpowiedzią jest liczba zgonów $D_x$ zarejestrowanych wśród $l_x$ osób w wieku $x$ w dniu 1 stycznia. Parametrem zainteresowania jest roczne prawdopodobieństwo zgonu $q_x$. Stosowany jest dwumianowy GLM z kanonicznym łączem, z oceną obejmującą wiek $x$ i jego kwadrat, to jest,

$$\text{logit}(q_x) = \beta_0 + \beta_1 x + \beta_2 x^2.$$

**Tabela 4.8** Wynik funkcji `glm` z R dla dopasowania kwadratowej formuły Perks do danych o śmiertelności wyświetlonych na Rys. 4.1. Liczba iteracji Fisher Scoring: 3

| Współczynnik | Oszacowanie | Bł. stand. | Wartość $z$ | $Pr(>\mid z \mid )$ | |
| --- | --- | --- | --- | --- | --- |
| Wyraz wolny | -3.340 | $4.808 \times 10^{-1}$ | -6.947 | $3.73 \times 10^{-12}$ | \*\*\* |
| Wiek x | -0.1038 | $1.213 \times 10^{-2}$ | -8.558 | $<2 \times 10^{-16}$ | \*\*\* |
| Wiek do kwadratu x² | $1.384 \times 10^{-3}$ | $7.597 \times 10^{-5}$ | 18.214 | $<2 \times 10^{-16}$ | \*\*\* |

Alternatywnie, model można dopasować do obserwowanych surowych prawdopodobieństw zgonu $\tilde{q}_x = D_x / l_x$, pod warunkiem, że wagi $l_x / \text{Var}[\tilde{q}_x]$ są używane jako waga. Oszacowania punktowe $\boldsymbol{\hat{\beta}}$ uzyskane za pomocą algorytmu IRLS zostały już podane wcześniej w tym rozdziale. Teraz możemy uzupełnić szczegółowe wyniki funkcji `glm` o oszacowania i p-wartości dla hipotez zerowych $H_0: \beta_j=0$. Wyniki są podsumowane w Tabeli 4.8.

Widzimy w drugiej kolumnie Tabeli 4.8 (zatytułowanej "Oszacowanie") oszacowania punktowe uzyskane za pomocą algorytmu IRLS. Odpowiednie błędy standardowe, oparte na właściwościach estymacji największej wiarygodności dla dużych prób, są podane w następnej kolumnie. Wartość $z$ jest statystyką Walda do testowania $\beta_j = 0$, otrzymaną jako stosunek wartości pojawiających się w dwóch poprzednich kolumnach. Odpowiednie p-wartości są wyświetlane dalej. Ostatnia kolumna w Tabeli 4.8 ilustruje wnioski wyciągnięte z testu statystycznego za pomocą prostego kodowania. Przypomnijmy, że im mniejsza p-wartość, tym większa statystyka testowa, a tym bardziej wątpliwe jest założenie $\beta_j = 0$. Wtedy prawdopodobne jest, że odpowiednia cecha powinna być zachowana w modelu (ponieważ jej wkład w ocenę GLM jest niezerowy). Jest to reprezentowane przez gwiazdki: trzy gwiazdki (\*\*\*) dla p-wartości mniejszych niż 0.1%, dwie gwiazdki (\*\*) dla p-wartości między 0.1 a 1%, i jedna gwiazdka (\*) dla p-wartości między 1 a 5%. Kropka jest używana dla p-wartości w szarej strefie 5-10%, gdzie aktuariusz jest niepewny co do istotności odpowiedniej cechy. Nic nie pojawia się w ostatniej kolumnie dla p-wartości większych niż 10%, które prawidłowo prowadzą do nieodrzucenia.

Dla właściwej interpretacji ważne jest, aby zdać sobie sprawę, że te gwiazdki odnoszą się do statystycznej istotności, a nie do faktycznego wpływu cechy na oczekiwaną odpowiedź. Możemy mieć większy $\hat{\beta}_j$ związany z większą p-wartością (pomimo pozostałych istotnych). $j$-ta cecha następnie indukuje bardziej premium różnice, nawet jeśli jest mniej istotna w R. Zatem ranking cech oparty na p-wartościach nie jest zatem sensowny bez uwzględnienia oszacowań.
Widzimy w Tabeli 4.8, że wszystkie p-wartości dla testu hipotezy zerowej $H_0: \beta_j = 0$ są mniejsze niż $10^{-6}$, więc istnieją silne dowody przeciwko $H_0: \beta_j=0$ dla każdego parametru (co jest reprezentowane przez trzy gwiazdki w ostatniej kolumnie, jak wyjaśniono powyżej). Dewiancja zerowa wynosi 32.760.878 na 35 stopniach swobody, podczas gdy dewiancja resztowa wynosi 42.942 na 33 stopniach swobody. Odpowiednie AIC wynosi 354.33.

Graficznie, dopasowanie uzyskane za pomocą kwadratowej formuły Perks było wyraźnie lepsze od tego uzyskanego z liniowym odpowiednikiem. Jest to już widoczne z małej p-wartości uzyskanej dla współczynnika regresji związanego ze składnikiem wieku do kwadratu raportowanym w Tabeli 4.8. Formalny test można przeprowadzić przy użyciu funkcji anova, która zwraca analizę tabeli dewiancji. Różnica w odpowiednich dewiancjach modelu liniowego i kwadratowego Perks wynosi 325.62 z 1 stopniem swobody. Wynikowa p-wartość jest mniejsza niż $10^{-6}$, więc hipoteza zerowa, że specyfikacja liniowa oceny jest równoważna kwadratowej, jest wyraźnie odrzucona. Widzimy zatem, że kwadratowa formuła (4.15) znacznie przewyższa swój liniowy odpowiednik (uzyskany przez umieszczenie $\beta_2=0$).

#### 4.7.7.2 Rezerwacja strat

Przejdźmy teraz do modelowania średnich płatności zgodnie z rokiem wypadku AY i rokiem rozwoju DY, rozważanych wcześniej w tym rozdziale. Współczynniki regresji wyświetlone na Rys. 4.6 sugerują ustrukturyzowanie efektów AY i DY w celu zmniejszenia liczby parametrów i zwiększenia stabilności modelu. Biorąc pod uwagę współczynniki regresji związane z DY, widzimy, że wydają się być wyraźnie liniowe do DY = 7, a następnie łamią, wykazując liniowy trend poza nim. Sugeruje to, aby przedstawić efekt roku rozwoju dwiema cechami DY i I[DY > 7] wchodzącymi w ocenę w sposób liniowy. Biorąc pod uwagę tę specyfikację, różnica w dewiancji wynosi 0.030928 z 8 stopniami swobody. Odpowiednia wartość F wynosi 0.8603, dając p-wartość 55.63%. Nie ma zatem istotnej różnicy między modelami z i bez ustrukturyzowanych efektów DY.

Oszacowane parametry regresji $\hat{\beta}_j$ związane z AY pozostają w dużej mierze niezmienione przez efekty DY. Jest to widoczne, gdy kontynuujemy strukturyzację współczynników regresji $\beta_j$ związanych z 11 latami wypadków. Widzimy, że do AY = 6, są prawie stałe, podczas gdy wykazują liniowy trend poza nim. Sugeruje to zastąpienie nieustrukturyzowanego efektu roku wypadkowego nową cechą AY $\times$ I[AY > 6] wchodzącą w ocenę w sposób liniowy, pozwalając na wyraz wolny. To dalsze uproszczenie nie pogarsza znacząco dopasowania. Dokładniej, w porównaniu z modelem początkowym, różnica w dewiancjach jest równa 0.068996 z 17 stopniami swobody. Wartość F wynosi 0.9032, a odpowiadająca jej p-wartość wynosi 57.85%, więc nie wykryto istotnej różnicy. Porównując z poprzednim modelem, otrzymujemy różnicę w dewiancjach 0.038068, 9 stopni swobody, wartość F 0.8155 i p-wartość 60.43%, co potwierdza poprzednie wyniki. Ponieważ model ustrukturyzowany testowany w stosunku do modelu początkowego przy użyciu analizy tabeli dewiancji i w stosunku do modelu pośredniego nie wykazuje istotnej różnicy (wykrytej testem F), preferowany jest prosty model ustrukturyzowany.

Dopasowanie modelu jest podsumowane w Tabeli 4.9. Ostateczny model obejmuje przekształcone cechy AY $\times$ I[AY > 6], DY i I[DY > 7] wynikające ze strukturyzacji efektów uzyskanych przez traktowanie AY i DY jako cech kategorialnych. Parametr dyspersji dla rodziny odwrotnej-gaussowskiej jest oszacowany na $\hat{\phi} = 0.0051574$. Dewiancja zerowa wynosi 5.48273 na 65 stopniach swobody, podczas gdy dewiancja resztowa jest równa 0.25771 na 62 stopniach swobody. Odpowiednie AIC wynosi 712,569.

**Tabela 4.9** Wynik funkcji `glm` z R dla dopasowania danych rezerwacji strat wyświetlonych w Tabeli 4.6 z ustrukturyzowanymi efektami AY i DY. Liczba iteracji Fisher scoring: 7

| Współczynnik | Oszacowanie | Bł. stand. | Wartość z | $Pr(>\mid z\mid )$ | |
| :--- | :--- | :--- | :--- | :--- | :--- |
| Wyraz wolny | 6.622377 | 0.030958 | 213.912 | $<2 \times 10^{-16}$ | \*\*\* |
| AYI[AY > 6] | 0.035638 | 0.002832 | 12.586 | $<2 \times 10^{-16}$ | \*\*\* |
| DY | 0.427926 | 0.020153 | 21.234 | $<2 \times 10^{-16}$ | \*\*\* |
| I[DY > 7] | -1.126561 | 0.648119 | -1.738 | 0.0871 | . |

## 4.8 Miary Wpływu

### 4.8.1 Definicja Ogólna

Obserwacja wpływowa to taka, która, jeśli zostanie nieznacznie zmieniona lub pominięta, w istotny sposób zmodyfikuje estymowane parametry modelu. Miary wpływu mają na celu ocenę wpływu konkretnej obserwacji na estymowane parametry lub dopasowane wartości. Dźwignia jest jedną z oznak tego, jak duży wpływ ma obserwacja. Ogólną definicją dźwigni $j$-tej obserwacji jest wielkość pochodnej $i$-tej dopasowanej wartości względem $j$-tej wartości odpowiedzi.

### 4.8.2 Miary diagnostyczne oparte na macierzy kapeluszowej (hat matrix)

W przypadku rozkładu normalnego wiemy z (4.11) do (4.12), że dopasowane wartości uzyskuje się poprzez pomnożenie obserwowanych odpowiedzi przez macierz kapeluszową $\boldsymbol{H}$. Zatem element diagonalny $h_{ii}$ macierzy $\boldsymbol{H}$ mierzy wpływ $Y_i$ na $\hat{Y}_i$

$$
\hat{Y_i} = \sum_{j=1}^{n} h_{ij}Y_j = h_{ii}Y_i + \sum_{j \ne i}^{n} h_{ij}Y_j, \quad i=1, 2, \dots, n.
$$

Jest to zgodne z ogólną definicją dźwigni przytoczoną wcześniej. Ponadto, $h_{ii}$ mierzy wpływ $Y_i$ na każdą dopasowaną wartość $\hat{Y}_j$, ponieważ

$$
h_{ii} = \sum_{j=1}^{n} h_{ij}^2, \quad \text{i} \quad h_{ij} = h_{ji}.
$$

Jeśli $h_{ii} = 1$, to $\hat{Y}_i$ jest wyznaczane wyłącznie przez $Y_i$. Jeśli $h_{ii} = 0$, to $y_i$ nie ma wpływu na $\hat{y}_i$.

Biorąc pod uwagę ślad macierzy kapeluszowej $\boldsymbol{H}$ podany w (4.18), średnia wartość $h_{ii}$ jest równa $\bar{h} = (p + 1)/n$. Obserwacje wpływowe często mają $h_{ii} > 2\bar{h}$; w małych próbkach, często używa się większego progu $3\bar{h}$.

### 4.8.3 Estymatory typu "Leave-One-Out" i Odległość Cooka

Innym podejściem do pomiaru wpływu jest zbadanie oszacowanych współczynników regresji uzyskanych poprzez pominięcie każdej obserwacji po kolei. Indeks dolny "$(-i)$" jest używany do wskazania, że odpowiednia wielkość została obliczona na zredukowanym zestawie danych uzyskanym przez usunięcie obserwacji $(y_i, \boldsymbol{x}_i)$ ze zbioru uczącego. Tak więc, $\boldsymbol{\hat{\beta}}_{(-i)}$ daje oszacowane współczynniki regresji na podstawie $(y_1, \boldsymbol{x}_1), \dots, (y_{i-1}, \boldsymbol{x}_{i-1}), (y_{i+1}, \boldsymbol{x}_{i+1}), \dots, (y_n, \boldsymbol{x}_n)$. Nazywa się to estymatorem typu "leave-one-out".

To pokazuje, że w przypadku rozkładu normalnego, estymacja typu "leave-one-out" nie wymaga ponownego dopasowywania modelu $n$ razy, bez $i$-tej obserwacji. Niestety, nie jest to już prawdą w przypadku innych GLM, ale opracowano aproksymacje w celu skrócenia czasu obliczeń dla nienormalnych GLM.

Odległość Cooka mierzy odległość między $\boldsymbol{\hat{\beta}}$ a $\boldsymbol{\hat{\beta}}_{(-i)}$, przy czym duże wartości wskazują na obserwacje, które mają wpływ na oszacowane współczynniki regresji. Odległość Cooka służy do badania, w jaki sposób każda obserwacja wpływa na cały wektor $\boldsymbol{\hat{\beta}}$ oszacowań parametrów. Mierzy ona różnicę $\boldsymbol{\hat{\beta}} - \boldsymbol{\hat{\beta}}_{(-i)}$ we wszystkich $p + 1$ współczynnikach regresji. Inna miara wpływu opiera się na różnicy dopasowanych wartości $\hat{\mu}_i$ przy użyciu wszystkich danych i $\hat{\mu}_{(-i)}$ z pominięciem obserwacji $i$.

Gdy $n$ jest duże (co jest ogólnie prawdą w zastosowaniach ubezpieczeniowych), pominięcie pojedynczej obserwacji $i$ nie ma tak naprawdę wpływu na oszacowane współczynniki regresji, co czyni podejście "leave-one-out" nieatrakcyjnym. Jednakże, gdy jest ono stosowane na danych zgrupowanych lub na problemach o małej skali (takich jak trójkąty run-off w rezerwach szkodowych), podejście to jest mimo wszystko pouczające.

## 4.9 Reszty Surowe, Standaryzowane i Studentyzowane

W tej sekcji omówiono różne rodzaje reszt, które służą do identyfikacji niedoskonałości modelu.

### 4.9.1 Powrót do Normalnego Liniowego Modelu Regresji

Reszty reprezentują różnicę między danymi a wartościami dopasowanymi, wytworzonymi przez rozważany model. Są one zatem ważne do sprawdzania adekwatności założenia GLM. W normalnym liniowym modelu regresji reszty surowe $R_i$ są zdefiniowane jako różnica między odpowiedzią $Y_i$ a wartościami dopasowanymi $\boldsymbol{x}_i^\top \boldsymbol{\hat{\beta}}$, tj.

$$
R_i = Y_i - \boldsymbol{x}_i^\top \boldsymbol{\hat{\beta}}.
$$

Mamy wówczas

$$
\text{E}[R_i] = 0, \quad \text{Var}[R_i] = \sigma^2(1 - h_{ii}) \quad \text{i} \quad \text{Cov}[R_i, R_j] = -\sigma^2 h_{ij}
$$

gdzie $h_{ij}$ jest $(i, j)$-tym elementem macierzy kapeluszowej $\boldsymbol{H}$ zdefiniowanej w (4.12).

Reszty surowe dla obserwacji o dużej dźwigni (tj. dużym $h_{ii}$) mają mniejsze wariancje. Aby skorygować niestałą wariancję, możemy podzielić $R_i$ przez estymację jej odchylenia standardowego. Reszty standaryzowane definiuje się jako

$$
T_i = \frac{y_i - \hat{y}_i}{\hat{\sigma}\sqrt{1 - h_{ii}}} = \frac{R_i}{\hat{\sigma}\sqrt{1 - h_{ii}}}.
$$

Reszty standaryzowane mierzą, o ile odchyleń standardowych obserwacja jest oddalona od dopasowanego modelu. Jednakże, licznik i mianownik w $T_i$ nie są niezależne, co uniemożliwia mu podążanie za rozkładem t-Studenta. Mają one wariancję jednostkową, ale ich rozkład jest nieznany, ponieważ licznik i mianownik są skorelowane ($i$-ta obserwacja wchodzi w skład obu). Z tego powodu zamiast nich często używa się reszt studentyzowanych.

Aby obejść problem zależności, uciekamy się do estymatorów typu "leave-one-out" $\hat{\beta}_{(-i)}$ i $\hat{\sigma}_{(-i)}$, które są oparte na wszystkich obserwacjach z wyjątkiem $i$-tej. Reszty studentyzowane są wówczas dane wzorem

$$
T_i^* = \frac{R_i}{\hat{\sigma}_{(-i)}\sqrt{1 - h_{ii}}}, \quad i = 1, 2, \dots, n.
$$

Reszty studentyzowane $T_i^*$ podążają za rozkładem t-Studenta z $n - p - 2$ stopniami swobody. Często $T_i^*$ jest preferowane nad $T_i$, ponieważ efektywniej identyfikuje obserwacje problematyczne.

Wykresy reszt mogą ujawnić problemy z dopasowaniem modelu. Istnieje wiele sposobów wyświetlania reszt i powiązanych wielkości. Wykresy reszt względem dopasowanych wartości i każdej cechy zawartej w wyniku należą do najbardziej klasycznych wykresów diagnostycznych. Każda systematyczna zmienność lub wyizolowany punkt odkryty na wykresie na ogół wskazuje na naruszenie jednego lub więcej założeń leżących u podstaw rozważanego modelu.

### 4.9.2 Reszty w GLM

W nienormalnych GLM można zdefiniować kilka typów reszt w analogii do normalnego liniowego modelu regresji. Często odwołują się one do normalnego liniowego modelu regresji używanego w ostatnim kroku algorytmu IRLS. Surowa reszta $r_i$ jest po prostu różnicą $y_i - \hat{\mu}_i$ między obserwowaną odpowiedzią a oszacowaną średnią $\hat{\mu}_i = g^{-1}(\boldsymbol{x}_i^\top \boldsymbol{\hat{\beta}})$. Nazywa się je resztami odpowiedzi dla GLM.

Często aktuariusze dopuszczają większe odchylenia między obserwacjami $y_i$ a dopasowanymi wartościami $\hat{\mu}_i$, gdy średnia wzrasta, ponieważ wariancja jest na ogół rosnącą funkcją średniej. To sprawia, że wykres surowych reszt jest mniej informatywny. Aby obejść ten problem, reszty surowe są normalizowane przez pierwiastek kwadratowy wariancji odpowiedzi. Prowadzi to do reszt Pearsona zdefiniowanych jako

$$
r_i^P = \frac{y_i - \hat{\mu}_i}{\sqrt{V(\hat{\mu}_i)/v_i}}.
$$

Reszty Pearsona są dziedziczone z modeli liniowych i nie uwzględniają kształtu rozkładu. Ponieważ dewiacja koryguje skośność odpowiedzi, w GLM zaproponowano do użycia inny resztę. Reszty dewiacyjne $r_i^D$ są zdefiniowane jako pierwiastek kwadratowy ze znakiem wkładu $i$-tej obserwacji do dewiacji. Pomaga to wykryć obserwacje, które w dużym stopniu zwiększają dewiację, a tym samym unieważniają model. W szczególności, rozłóżmy dewiację na

$$
D(\boldsymbol{y}, \hat{\boldsymbol{\mu}}) = 2 \sum_{i=1}^n (y_i(\tilde{\theta}_i - \hat{\theta}_i) - a(\tilde{\theta}_i) + a(\hat{\theta}_i)) = \sum_{i=1}^n d_i
$$

gdzie $d_i = d(y_i, \hat{\theta}_i)$ zostało wprowadzone w (4.19). Reszty dewiacyjne są wtedy zdefiniowane jako

$$
r_i^D = \text{sign}(y_i - \hat{\mu}_i)\sqrt{d_i}.
$$

Tutaj notacja "sign" oznacza znak wielkości w nawiasach:

$$
\text{sign}(y_i - \hat{\mu}_i) = \begin{cases} 1 & \text{if } y_i > \hat{\mu}_i \\ -1 & \text{w przeciwnym razie}. \end{cases}
$$

Reszty dewiacyjne są bardziej odpowiednie w otoczeniu GLM i często są preferowaną formą reszt. W przypadku rozkładu normalnego reszty Pearsona i dewiacyjne są identyczne. Należy jednak zauważyć, że analityk nie szuka normalności w resztach dewiacyjnych, ponieważ ich rozkład próbkowy pozostaje nieznany. Chodzi o brak dopasowania i szukanie wzorców widocznych na resztach: jeśli aktuariusz wykryje jakiś wzorzec na resztach wykreślonych względem

- dopasowanych wartości,
- cech zawartych w scoringu lub
- cech nieujętych w modelu

wtedy coś jest nie tak i należy podjąć jakieś działania. Na przykład, jeśli aktuariusz wykryje pewną strukturę w resztach wykreślonych względem nieujętej cechy, wówczas cecha ta powinna zostać zintegrowana ze scoringiem. Jeśli istnieją wzorce w resztach względem każdej cechy zawartej w scoringu, wówczas odpowiednia cecha nie jest prawidłowo zintegrowana ze scoringiem (powinna zostać przekształcona, lub aktuariusz powinien porzucić GLM na rzecz modelu addytywnego). Czasami interesujące jest również wykreślenie reszt względem współrzędnych przestrzennych lub czasowych, dla których mogą istnieć wzorce. Wzorce w rozrzucie (wykryte przez wykreślenie reszt względem dopasowanych wartości) mogą wskazywać na naddyspersję lub, bardziej ogólnie, na użycie niewłaściwej relacji średnia-wariancja.

Reszty standaryzowane uwzględniają różne dźwignie. Rozważmy macierz kapeluszową (4.12) pochodzącą z ostatniej iteracji algorytmu IRLS. Wartości dźwigni $h_{ii}$ mierzą dźwignię, czyli potencjał obserwacji do wpływania na dopasowane wartości. Duża wartość $h_{ii}$ wskazuje, że $i$-ta obserwacja jest wrażliwa na odpowiedź $y_i$. Duże dźwignie zazwyczaj oznaczają, że odpowiednie cechy są w jakiś sposób nietypowe. Standaryzowane reszty, zwane również resztami odpowiadającymi, są wtedy dane przez

$$
t_i^P = \frac{r_i^P}{\sqrt{\phi(1 - h_{ii})}} \quad \text{i} \quad t_i^D = \frac{r_i^D}{\sqrt{\phi(1 - h_{ii})}}.
$$

W przeciwieństwie do normalnego liniowego modelu regresji, gdzie standaryzowane reszty mają rozkład normalny, rozkład reszt w GLM jest ogólnie nieznany. Dlatego zakres reszt jest trudny do oceny, ale można je badać pod kątem struktury i dyspersji, wykreślając je względem dopasowanych wartości lub niektórych cech. Dźwignia mierzy tylko potencjał wpływu na dopasowanie, podczas gdy miary wpływu bardziej bezpośrednio mierzą efekt każdej obserwacji na dopasowanie. Odbywa się to zazwyczaj poprzez badanie zmiany w dopasowaniu po pominięciu tej obserwacji. W GLM można to zrobić, patrząc na zmiany w oszacowaniach współczynników regresji. Statystyka Cooka mierzy odległość między $\boldsymbol{\hat{\beta}}$ a $\boldsymbol{\hat{\beta}}_{(-i)}$ uzyskaną przez pominięcie $i$-tego punktu danych $(y_i, \boldsymbol{x}_i)$ ze zbioru uczącego.

### 4.9.3 Reszty Indywidualne lub Grupowe

Obliczenie wkładu każdej obserwacji do dewiacji może dostosować się do kształtu, ale nie do dyskrecji obserwacji. Często reszty indywidualne dla odpowiedzi dyskretnych są trudne do zinterpretowania, ponieważ dziedziczą strukturę wartości odpowiedzi. Na przykład, dla odpowiedzi binarnych reszty często skupiają się wokół dwóch krzywych, jednej dla obserwacji "0" i drugiej dla obserwacji "1". Dla zliczeń, reszty indywidualne koncentrują się wokół obserwacji "0", obserwacji "1", obserwacji "2" itd. 

Poprzez agregację reszt indywidualnych na duże grupy podobnych ryzyk, aktuariusz unika pozornej struktury wytworzonej przez całkowitoliczbową naturę odpowiedzi. Dzieje się tak, ponieważ wiemy z Rozdz. 2, że odpowiedzi podążające za rozkładem Poissona z dużą średnią lub rozkładem dwumianowym z dużą liczebnością są w przybliżeniu normalne. Reszty dewiacyjne obliczone na danych zagregowanych dają znacznie lepsze wskazania co do adekwatności modelu dla odpowiedzi.

Przypomnijmy również, że GLM służą do obliczania czystych premii. Stąd analityk jest zainteresowany estymacją wartości oczekiwanych, a nie przewidywaniem konkretnego wyniku. Dlatego dane można grupować przed oceną wyników modelu.

## 4.10 Klasyfikacja Ryzyka w Ubezpieczeniach Komunikacyjnych

Aby podsumować wszystkie dotychczas przedstawione pojęcia, przeanalizujmy teraz większy zbiór danych związany z ubezpieczeniami komunikacyjnymi. Odpowiedzią jest tutaj liczba szkód zgłoszonych przez ubezpieczonego kierowcę w ramach ubezpieczenia od odpowiedzialności cywilnej.

### 4.10.1 Opis Zbioru Danych

#### 4.10.1.1 Dostępne Cechy i Odpowiedzi

Przedstawmy pokrótce dane użyte do zilustrowania technik opisanych w tym rozdziale. Zbiór danych odnosi się do portfela belgijskich ubezpieczeń komunikacyjnych OC obserwowanego pod koniec lat 90. Obejmuje on 14 504 umowy z łączną ekspozycją wynoszącą 11 183.68 poliso-lat. Są to pojazdo-lata, ponieważ każda polisa obejmuje pojedynczy pojazd, ale niektóre umowy obowiązują tylko przez część roku.

Zmienne zawarte w pliku są następujące:

*   **AgePh**: Wiek ubezpieczającego w ostatnie urodziny (na 1 stycznia), w 5 grupach od G1 do G5 o rosnącym starszeństwie.
*   **Gender**: Płeć ubezpieczającego, mężczyzna lub kobieta.
*   **PowerCat**: Moc samochodu, w czterech kategoriach od C1 do C4 o rosnącej mocy.
*   **Expor**: Liczba dni obowiązywania polisy w okresie obserwacji.
*   **CitySize**: Wielkość miasta, w którym mieszka ubezpieczający, z trzema kategoriami: duże, średnie lub małe.

Oprócz tych cech, zarejestrowano liczbę szkód **Nclaim** zgłoszonych przez każdego ubezpieczającego w okresie obserwacji. Jest to odpowiedź badana w tej sekcji.

#### 4.10.1.2 Skład Portfela w Odniesieniu do Cech

Górny lewy panel na Rys. 4.11 przedstawia rozkład ekspozycji na ryzyko w portfelu. Około 60% polis było obserwowanych przez cały rok. Biorąc pod uwagę rozkład ekspozycji na ryzyko, widzimy, że wznowienia i wygaśnięcia polis są rozłożone w ciągu roku. Należy zauważyć, że firmy ubezpieczeniowe często tworzą nowy rekord za każdym razem, gdy następuje zmiana cech (na przykład ubezpieczający kupuje nowy samochód lub przeprowadza się w inne miejsce). To wyjaśnia stosunkowo dużą liczbę umów z ekspozycją znacznie mniejszą niż jeden. Struktura wiekowa portfela jest również opisana na Rys. 4.11 (tamże górny prawy panel), podobnie jak skład zbioru danych pod względem płci, wielkości miasta, w którym mieszka ubezpieczający, oraz mocy samochodu. Widzimy, że większość ubezpieczonych kierowców należy do grupy wiekowej G2, około dwie trzecie umów obejmuje kierowcę płci męskiej, rozkład mocy wykazuje kształt litery U, a umowy są równomiernie rozłożone na trzy typy miast.

Dolny prawy panel na Rys. 4.11 przedstawia histogram zaobserwowanych liczby szkód na polisę. Widzimy, że większość polis (dokładnie 12 458) nie wygenerowała żadnej szkody. Około 10% portfela wygenerowało jedną szkodę (1 820 umów). Następnie, 206 umów wygenerowało 2 szkody, 17 umów wygenerowało 3 szkody, 2 umowy 4 szkody i była jedna polisa z 5 szkodami. Daje to globalną częstość szkód równą 20,53% na poziomie portfela (co jest dość wysokie w porównaniu z dzisiejszymi częstościami szkód na poziomie 6-7% na rynku belgijskim).

#### 4.10.1.3 Związek Między Cechami

Użyjmy współczynnika V Craméra do wykrycia cech, które są ze sobą powiązane. Rysunek 4.12 przedstawia wartości V Craméra między cechami, a także między odpowiedzią a cechami. Widzimy, że powiązania są raczej słabe, z wyjątkiem związku między Mocą a Płcią. Ale pozostaje on stosunkowo umiarkowany w każdym przypadku. Ponadto, odpowiedź wydaje się być słabo powiązana z poszczególnymi cechami. Jest to typowe dla danych dotyczących liczby szkód w ubezpieczeniach ze względu na heterogeniczność obecną w danych i dyskretność odpowiedzi.

### 4.10.2 Marginalny Wpływ Cech na Liczbę Szkód

Badania ubezpieczeniowe generalnie zaczynają się od analiz jednoczynnikowych, czyli marginalnych, podsumowujących dane odpowiedzi dla każdej wartości cechy, ale bez uwzględniania wpływu innych cech. Efekty marginalne czterech ciągłych cech na roczne częstości szkód są przedstawione na Rys. 4.13. Widzimy, że oszacowana częstość szkód dla każdego poziomu cech kategorycznych. Najlepsze oszacowania (odpowiadające okręgom) są otoczone przedziałami ufności. Wyniki te można uzyskać za pomocą analizy GLM Poissona obejmującej pojedynczą cechę, dzięki czemu przedziały ufności są łatwo wyprowadzane z rozkładu próby oszacowanych współczynników regresji.

Pomimo stosunkowo niskich wartości V Craméra, widzimy, że częstości szkód wydają się zależeć od cech w zwykły sposób, malejąc wraz z wiekiem ubezpieczającego, większe dla kierowców płci męskiej, rosnące wraz z wielkością miasta, podczas gdy wpływ mocy jest na tym etapie trudniejszy do zinterpretowania. Oczywiście, są to tylko efekty marginalne, które mogą być zniekształcone przez korelację istniejącą między cechami, dlatego potrzebujemy analizy GLM uwzględniającej wszystkie cechy, aby prawidłowo ocenić wpływ każdej cechy na oczekiwaną liczbę szkód.

### 4.10.3 Analiza GLM Poissona dla Liczby Szkód

Rozkład Poissona jest naturalnym kandydatem do modelowania liczby szkód zgłaszanych przez ubezpieczających. Typowym założeniem w tych okolicznościach jest to, że warunkowa średnia częstość szkód może być zapisana jako funkcja wykładnicza wyniku liniowego ze współczynnikami do oszacowania na podstawie danych.

Zaczynamy od modelu uwzględniającego wszystkie cechy. Poziomy odniesienia dla cech binarnych to te najliczniej występujące, zgodnie z wyjaśnionymi powyżej wytycznymi. Używamy

$$
\text{offset}_i = \ln(\text{ExpoR}_i / 365).
$$

Wyniki uzyskane za pomocą funkcji `glm` oprogramowania R są wyświetlone w Tabeli 4.10.

**Tabela 4.10** Wynik funkcji `glm` w R dla dopasowania modelu do danych TPL. Liczba iteracji scoringu Fishera: 6

| Współczynnik    | Oszacowanie | Błąd stand. | wartość z | Pr(>\|z\|)           |   |
|:----------------|:------------|:------------|:----------|:---------------------|:--|
| Intercept       | -1.46605    | 0.05625     | -26.063   | < 2 x 10<sup>-16</sup> | *** |
| Female          | -0.12532    | 0.05567     | -2.209    | 0.02714              | * |
| Age G2          | 0.25232     | 0.08865     | 2.846     | 0.00442              | ** |
| Age G3          | -0.30633    | 0.05999     | -5.106    | 3.29 x 10<sup>-7</sup> | *** |
| Age G4          | -0.32670    | 0.05854     | -5.581    | 2.40 x 10<sup>-8</sup> | *** |
| Age G5          | -0.46584    | 0.06215     | -7.495    | 6.62 x 10<sup>-14</sup>| *** |
| Power C2        | 0.08483     | 0.05430     | 1.562     | 0.11819              |   |
| Power C3        | 0.07052     | 0.06211     | 1.135     | 0.25622              |   |
| Power C4        | 0.07930     | 0.06082     | 1.304     | 0.19230              |   |
| CitySize Large  | 0.24548     | 0.04955     | 4.954     | 7.25 x 10<sup>-7</sup> | *** |
| CitySize Small  | -0.09414    | 0.05340     | -1.763    | 0.07790              |  |

Widzimy, że wpływ wieku, płci i wielkości miasta jest zgodny z wstępną analizą marginalną. Moc nie wydaje się istotnie wpływać na oczekiwaną częstość szkód. Jednak wyniki przedstawione w Tabeli 4.10 nie są wystarczające, aby stwierdzić, że moc nie jest potrzebna w GLM. Powodem jest to, że ta cecha kategoryczna została zakodowana za pomocą trzech zmiennych zero-jedynkowych, więc w Tabeli 4.10 przeprowadzane są trzy testy, po jednym dla każdego poziomu różnego od kategorii odniesienia C1. Ponieważ nie kontrolujemy całkowitego błędu związanego z wykorzystaniem wniosków z tych trzech testów jednocześnie, wartości p-wartości wyświetlane w Tabeli 4.10 nie dają aktuariuszowi odpowiedniego narzędzia do podjęcia decyzji, czy moc można usunąć ze scoringu.

Dewiacja zerowa wynosi 9 508.6 na 14 503 stopniach swobody, a dewiacja resztkowa 9 363.5 na 14 493 stopniach swobody. To pokazuje, że model regresji Poissona wyjaśnia tylko niewielką część dewiacji. AIC wynosi 13 625. Jak omówiono wcześniej, Tabela 4.10 sugeruje, że moc nie wydaje się być czynnikiem ryzyka. Aby formalnie sprawdzić, czy moc można wykluczyć z modelu GLM, porównujemy modele z i bez tej cechy za pomocą tabeli analizy dewiacji. Różnica w dewiacjach modeli z i bez mocy wynosi 3.0654 z 3 stopniami swobody. Odpowiadająca p-wartość wynosi 38.17%, co potwierdza, że moc nie jest istotna dla wyjaśnienia liczby szkód.

Co więcej, wydaje się, że dwa ostatnie poziomy CitySize można połączyć. Sprawdzamy, czy to uproszczenie modelu znacząco pogarsza dopasowanie za pomocą tabeli analizy dewiacji. Różnica w dewiacjach między początkowym modelem uwzględniającym wszystkie cechy a modelem bez Mocy i z dwoma ostatnimi poziomami CitySize połączonymi wynosi 6.2137 z 4 stopniami swobody. Odpowiadająca p-wartość 18.37% wskazuje, że grupowanie nie pogarsza znacząco dopasowania. Po ponownym dopasowaniu modelu odkrywamy, że oszacowane współczynniki regresji dla kategorii wiekowych G3 i G4 są odpowiednio równe -0.30054 i -0.31933, przy błędach standardowych odpowiednio równych 0.05984 i 0.05801. To sugeruje, że poziomy G3 i G4 Wieku można połączyć. Potwierdzono to za pomocą tabeli analizy dewiacji, ponieważ różnica w dewiacjach wynosi 6.2867 z 5 stopniami swobody dla modelu początkowego w porównaniu z uproszczonym.

**Tabela 4.11** Wynik funkcji `glm` w R dla ostatecznego modelu regresji Poissona dla danych TPL. Liczba iteracji scoringu Fishera: 6

| Współczynnik   | Oszacowanie | Błąd stand. | wartość z | Pr(>\|z\|)            |   |
|:---------------|:------------|:------------|:----------|:----------------------|:--|
| Intercept      | -1.76608    | 0.04183     | -42.216   | < 2 x 10<sup>-16</sup>  | *** |
| Female         | -0.11779    | 0.04435     | -2.656    | 0.00791               | ** |
| Age G1         | 0.54798     | 0.08933     | 6.134     | 8.55 x 10<sup>-10</sup> | *** |
| Age G2         | 0.31040     | 0.04756     | 6.527     | 6.71 x 10<sup>-11</sup> | *** |
| Age G5         | -0.14506    | 0.06276     | -2.311    | 0.02081               | * |
| CitySize Large | 0.29026     | 0.04300     | 6.751     | 1.47 x 10<sup>-11</sup> | *** |

P-wartość 27.93% pokazuje, że dalsze grupowanie jest zgodne z danymi. Ponieważ ekspozycja jest teraz w połączonej grupie wiekowej G3-G4, ta ostatnia staje się nowym poziomem odniesienia.

Po tych krokach mamy teraz ostateczny model podsumowany w Tabeli 4.11. Dewiacja zerowa 9 508.6 na 14 503 stopniach swobody jest teraz porównywana z dewiacją resztkową 9 369.8 na 14 498 stopniach swobody. Ta ostatnia jest wyższa niż odpowiadająca wartość dla początkowego modelu obejmującego wszystkie cechy. Uproszczony model mimo to oferuje lepsze dopasowanie, co wskazuje wartość AIC, która zmniejszyła się z 13 625 do 13 621.

Widzimy, że reszty oparte na obserwacjach indywidualnych dziedziczą strukturę odpowiedzi, z dolną krzywą odpowiadającą polisom bez szkód, następną dla polis, które zgłosiły 1 szkodę, 2 szkody itd. Reszty obliczone na podstawie danych zgrupowanych pozwalają uniknąć tej pułapki. Dokładniej, dane indywidualne są grupowane zgodnie z cechami wchodzącymi do ostatecznego modelu (płeć, wiek z 4 poziomami i wielkość miasta) i dodawane są ekspozycje. To tworzy podsumowany zbiór danych z 16 grupami. Oszacowane współczynniki regresji pozostają niezmienione przez grupowanie, ale reszty można teraz ponownie obliczyć (zauważ, że reszty dewiacyjne nie są addytywne). Wynikowe zgrupowane reszty są wyświetlane na dolnym panelu Rys. 4.14. Po wykreśleniu względem wartości kapeluszowych i uzupełnieniu odległościami Cooka na Rys. 4.15, uzyskane reszty wydają się wspierać dopasowanie modelu, ponieważ żadna klasa ryzyka nie wykazuje jednocześnie dużych odległości Cooka i dźwigni. Widzimy na Rys. 4.16, że prognozy modelu są w dużej mierze zgodne z doświadczeniem portfela: klasy ryzyka plasują się bardzo blisko linii 45 stopni, zwłaszcza te najliczniej występujące.

## 4.11 Quasi-Wiarygodność, M-Estymacja i Pseudo-Wiarygodność

### 4.11.1 Estymacja Quasi-Wiarygodności

Wiemy, że wybór rozkładu do przeprowadzenia analizy GLM jest równoznaczny z wyborem funkcji wariancji, która wiąże wariancję odpowiedzi ze średnią. Algorytm IRLS (iteracyjnie ważonych najmniejszych kwadratów) uwzględnia jedynie warunkową średnią i wariancję odpowiedzi $Y_i$, biorąc pod uwagę cechy $x_i$. Dokładniej, o ile możemy wyrazić transformowaną średnią odpowiedzi $Y_i$ jako funkcję liniową cech $x_i$ i zapisać funkcję wariancji dla $Y_i$ (wyrażając warunkową wariancję $Y_i$ jako funkcję jej średniej $\mu_i$ i parametru dyspersji $\phi$), możemy zastosować algorytm IRLS i uzyskać oszacowane współczynniki regresji. Możemy to zrobić nawet bez pełnego specyfikowania rozkładu dla $Y_i$.

Podejście to jest określane jako estymacja quasi-wiarygodności i zachowuje wiele dobrych właściwości estymacji największej wiarygodności, pod warunkiem, że próba jest wystarczająco duża (co jest generalnie prawdą w zastosowaniach ubezpieczeniowych). Chociaż estymatory quasi-wiarygodności mogą nie być asymptotycznie nieobciążone, są one zgodne (consistent) i asymptotycznie normalne, z macierzą wariancji-kowariancji, którą można oszacować na podstawie danych. Quasi-wiarygodność jest analogiczna do estymacji metodą najmniejszych kwadratów z potencjalnie nienormalnymi odpowiedziami: o ile zależność między odpowiedzią a zmiennymi objaśniającymi jest liniowa, wariancja jest stała, a obserwacje są wzajemnie niezależne, estymacja metodą najmniejszych kwadratów ma zastosowanie. Wnioskowanie jest dość odporne na brak normalności, o ile próba jest duża. Biorąc pod uwagę ramy GLM, ważną częścią specyfikacji modelu jest funkcja łącząca i funkcje wariancji, podczas gdy wnioski analizy regresji są mniej wrażliwe na faktyczny rozkład odpowiedzi w dużych próbach.

Istnieje zaleta stosowania podejścia quasi-wiarygodności dla modeli z funkcjami wariancji odpowiadającymi rozkładom dwumianowemu i Poissona. Zwykłe GLM dwumianowe i Poissona zakładają $\phi = 1$, podczas gdy odpowiadające im GLM quasi-dwumianowe i quasi-Poissona pozwalają, aby dyspersja $\phi$ była wolnym parametrem. Jest to szczególnie przydatne w modelowaniu naddyspersji (overdispersion), która jest powszechnie obecna w danych dotyczących liczby szkód ubezpieczeniowych, a także niedodyspersji (underdispersion) wynikającej z sumowania wskaźników Bernoulliego o różnych prawdopodobieństwach sukcesu.

Naddyspersja odnosi się do sytuacji, w której warunkowa wariancja odpowiedzi jest większa niż wariancja implikowana przez rozkład ED (Exponential Dispersion) użyty do dopasowania modelu. Oznacza to, że $\text{Var}[Y|\boldsymbol{X}=\boldsymbol{x}] > \text{E}[Y|\boldsymbol{X}=\boldsymbol{x}]$ w przypadku Poissona. Istnieje kilka powodów, dla których w danych pojawia się naddyspersja, w tym:

-   odpowiedzi $Y_{i1}$ i $Y_{i2}$ nie mają tego samego rozkładu, mimo że dzielą te same cechy $\boldsymbol{x}_{i1} = \boldsymbol{x}_{i2}$. Dzieje się tak, ponieważ brakuje pewnych informacji i istnieje pewna resztkowa, niemodelowana heterogeniczność.
-   obserwacje mogą być skorelowane lub sklastrowane, podczas gdy określona struktura kowariancji błędnie zakłada dane niezależne.

Model quasi-Poissona jest przykładem podejścia quasi-wiarygodności, estymującego średnią odpowiedź z wariancją równą $\phi\mu$ (rozszerzając model Poissona, dla którego $\phi=1$). Naddyspersyjny Poisson, czyli ODP (overdispersed Poisson), odnosi się do konkretnego przypadku, w którym $\phi>1$. W rezerwach szkodowych ODP jest często używany.

## 4.12 Podsumowanie na temat GLM

Podejście GLM do modelowania danych posiada wiele dogodnych właściwości, które czynią je szczególnie atrakcyjnym do prowadzenia badań ubezpieczeniowych. Wymieńmy kilka z tych właściwości, które zostały omówione w tym rozdziale:

*   dyskretna lub skośna natura odpowiedzi jest uwzględniana bez konieczności wcześniejszej transformacji danych szkodowych;
*   liniowy scoring jest łatwo interpretowalny i komunikowalny, co czyni go idealnym dla komercyjnych taryf składek (przejrzystość, ograniczenia marketingowe itp.);
*   GLM są łatwo dopasowywane do danych ubezpieczeniowych za pomocą metody największej wiarygodności, z pomocą algorytmu IRLS, który wydaje się być ogólnie skuteczny z numerycznego punktu widzenia;
*   GLM rozszerzają metodę MMT (metodę sum brzegowych) zapoczątkowaną przez amerykańskich aktuariuszy w latach 60., co ułatwiło jej przyjęcie przez społeczność aktuarialną w latach 90., dostarczając analitykom odpowiednich interpretacji dla równań wiarygodności przy kanonicznej funkcji łączącej.

Oprócz tych zalet, GLM mają również kilka poważnych ograniczeń:

*   liniowa struktura scoringu jest zbyt prymitywna dla technicznej taryfy składek, gdzie nieliniowe, przestrzenne lub wielopoziomowe czynniki ryzyka muszą być odpowiednio zintegrowane w skali scoringu;
*   GLM nie uwzględniają automatycznie interakcji;
*   współliniowość jest problemem;
*   jak również przypadki „dużego $p$”, co sprawia, że ręczna procedura selekcji zmiennych zilustrowana w tym rozdziale jest nieosiągalna.

Należy również pamiętać, że cała historia dotyczy korelacji i że nie można wyciągać stanowczych wniosków na temat możliwego związku przyczynowo-skutkowego między cechami a odpowiedzią.