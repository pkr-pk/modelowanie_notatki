# Rozdział 9: Symulacje

## 9.2 Techniki redukcji wariancji

### 9.2.2 Próbkowanie z wagami (Importance Sampling)

Jednym z problemów surowej estymacji MC jest to, że wiele wygenerowanych punktów próbki w rzeczywistości nie znajdzie się w regionie istotnym dla szacowanej wielkości. Na przykład w Przykładzie 9.1 dotyczącym estymacji kwantyla, dla bardzo małych lub bardzo dużych wartości $\alpha$, nie będzie wystarczającej liczby punktów próbki w pobliżu kwantyla (nieefektywne jest dokładne określanie wartości replikacji, jeśli i tak jest ona daleka od kwantyla; istotny jest tylko fakt, że znajduje się po danej stronie kwantyla), co zwiększa wariancję estymatora.

Załóżmy ponownie, że $F_X$ dopuszcza gęstość $f_X$. Ideą próbkowania z wagami jest teraz przejście z $f_X$ na inną gęstość $f_{\tilde{X}}$, która koncentruje się bardziej na interesującym nas regionie (w Przykładzie 9.1 oznaczałoby to, że $f_{\tilde{X}}$ ma dużą masę prawdopodobieństwa wokół przypuszczalnej wartości $Q(\alpha)$). Taka nowa gęstość $f_{\tilde{X}}$ może być uzyskana z $f_X$ przez przesunięcie, przeskalowanie, przekształcenie itp. Wielkość $f_X(x)/f_{\tilde{X}}(x)$ nazywana jest funkcją ilorazu wiarygodności. Mamy wtedy

$$ E(X) = \int x f_X(x)dx = \int \left(x \frac{f_X(x)}{f_{\tilde{X}}(x)}\right) f_{\tilde{X}}(x)dx = E\left(\tilde{X} \frac{f_X(\tilde{X})}{f_{\tilde{X}}(\tilde{X})}\right). $$

Zamiast tego symulujemy $n$ niezależnych replikacji $\tilde{X}_1, \dots, \tilde{X}_n$ z nowej zmiennej losowej $\tilde{X}$ (o gęstości $f_{\tilde{X}}$) i używamy estymatora próbkowania z wagami

$$ \hat{\mu}_{n}^I = \frac{1}{n} \sum_{i=1}^{n} \tilde{X}_i \frac{f_X(\tilde{X}_i)}{f_{\tilde{X}}(\tilde{X}_i)}. \quad (9.2.4) $$

Reprezentuje to ważony estymator MC dla $E(X)$ z wagami zgodnymi z funkcją ilorazu wiarygodności, aby „skorygować” użycie nowej gęstości $f_{\tilde{X}}$. Wariancja tego estymatora wynosi

$$ \text{Var}(\hat{\mu}_{n}^I) = \frac{1}{n} \left( \int x^2 \frac{f_X(x)}{f_{\tilde{X}}(x)} f_X(x)dx - E^2(X) \right), $$

więc osiągniemy redukcję wariancji, gdy $E[X^2 f_X(X)/f_{\tilde{X}}(X)] < E(X^2)$. W związku z tym próbkowanie z wagami będzie najbardziej efektywne, jeśli heurystycznie:
* $f_{\tilde{X}}(x)$ jest duże, gdy $x^2 f_X(x)$ jest duże,
* $f_{\tilde{X}}(x)$ jest małe, gdy $x^2 f_X(x)$ jest małe.

Ponadto, $f_{\tilde{X}}(x)$ powinno być łatwe do obliczenia, a $\tilde{X}$ powinno być łatwe do symulacji. Poniższy przykład ilustruje wzrost wydajności.

---
**Ilustracja: VaR zmiennej losowej o rozkładzie gamma.**

Rozważmy symulację $\text{VaR}_{0.995}(X)$ dla przypadku, w którym $X$ ma gęstość gamma(3.3,0.9). Jeśli mamy wstępne przypuszczenie co do tej wartości, możemy użyć próbkowania z wagami, aby skoncentrować masę prawdopodobieństwa w tym regionie. Na przykład, argument oparty na dużych odchyleniach może sugerować, że ostateczna wartość wynosi 14.2. Jeśli zdecydujemy się użyć przekształcenia wykładniczego (exponential twisting)

$$ f_{\tilde{X}}(x) = e^{\theta x} f_X(x) / E(e^{\theta X}) $$

(co jest w istocie transformatą Esschera zmiennej X, por. Rozdział 7), możemy zidentyfikować wartość $\theta$, dla której $E(\tilde{X}) = 14.2$, co okazuje się być $\theta = 0.88$. Rysunek 9.2 ilustruje wzrost wydajności użycia estymatora próbkowania z wagami (9.2.4) w porównaniu z surową metodą MC w funkcji liczby symulacji. Zauważmy, że chociaż pierwsze przypuszczenie 14.2 co do prawdziwej wartości było w rzeczywistości dość niedokładne, wynikowe przekształcenie X jest wciąż znacznie lepsze niż użycie oryginalnego rozkładu.

---
Przejdźmy teraz do szacowania prawdopodobieństwa ogona $z(u) = P(S(t) > u)$ sumy zagregowanej $S(t) = \sum_{i=1}^{N(t)} X_i$ dla dużego $u$ i szkód $X_i$ o lekkim ogonie. Możemy wtedy zastosować ideę przekształcenia wykładniczego opisaną w powyższej ilustracji do całej zmiennej losowej $S(t)$ (co modyfikuje rozkład zarówno $N(t)$, jak i $X_i$), aby uzyskać estymator próbkowania z wagami

$$ Z^*(u) = \exp \left\{ -\theta(u) \sum_{i=1}^{N(t)} X_i \right\} e^{\kappa_S(\theta(u))} \mathbf{1}_{\left\{\sum_{i=1}^{N(t)} X_i > u\right\}}, $$

gdzie $\kappa_S(\theta(u)) = \log E(e^{\theta(u)S(t)})$ jest funkcją generującą kumulanty dla $S(t)$. Możemy teraz wybrać $\theta(u)$ (tj. zdefiniować stopień przekształcenia) w taki sposób, aby przy nowej mierze oczekiwać, że $S(t)$ będzie równe wartości progowej $u$, czyli

$$ \tilde{E}_X(S(t)) = \kappa'_S(\theta(u)) \stackrel{!}{=} u. $$

---
**Ilustracja: VaR złożonej sumy Poissona dla szkód o rozkładzie gamma.** 

Jeśli $N(t)$ jest jednorodnym procesem Poissona ze wskaźnikiem $\lambda$, wówczas $\kappa_S(\theta(u)) = \lambda t(M_{X_i}(\theta(u)) - 1)$, gdzie $M_{X_i}(\theta(u)) = E(e^{\theta(u)X_i})$ oznacza funkcję generującą momenty dla poszczególnych szkód $X_i$. Ustalmy teraz $t = 1$. W związku z tym możemy wybrać $\theta(u)$ jako rozwiązanie równania $\lambda M'_{X_i}(\theta(u)) = u$. Ze względu na $E(e^{(r+\theta(u))S(1)})/E(e^{\theta(u)S(1)}) = e^{\lambda M_{X_i}(\theta(u))(M_{X_i}(r+\theta(u))/M_{X_i}(\theta(u)) - 1)}$, symulujemy więc złożony proces Poissona ze wskaźnikiem $\lambda \cdot M_{X_i}(\theta(u))$ i poszczególnymi szkodami o gęstości $e^{\theta(u)x} f_{X_i}(x) / E(e^{\theta(u)X_i})$ i obliczamy $Z^*(u)$. Rysunek 9.3 ilustruje znaczną poprawę wydajności dla konkretnej złożonej sumy Poissona zmiennych losowych o rozkładzie gamma.

---
Dla szkód o ciężkich ogonach przekształcenie wykładnicze nie ma zastosowania, ponieważ funkcja generująca momenty $M_{X_i}(r)$ nie istnieje dla żadnego $r > 0$. W takich przypadkach popularne jest przekształcanie funkcji hazardu $f_X(x)/F_X(x)$ (dalsze udoskonalenia to tzw. asymetryczne przekształcanie funkcji hazardu i opóźnione przekształcanie funkcji hazardu, zob. Juneja i Shahabuddin).
