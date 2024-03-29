# Calculation of Probability 1 
*taken from the books A Modern Introduction to Probability and Statistics by Dekken and co, and Introduction to Probability by Blitzstein and Hwang*
Note: when referencing to pages, I refer to the page on the pdf of each book.
## 1.2 Sample spaces and Pebble World (Blitzstein)
*this is complementary to my course in college*
4. Let D be the event that there were at least two consecutive Heads. As a set,
	$$ D = \bigcup_{j=1}^{9}(A_{j}\cap A_{j+1})$$
	So two consecutive *anything* is the intersection of: $$ i\cap i_{++}$$
*page 23:* In fact, the counting methods introduced later in this chapter show that there are $$2^{52} \approx 4.5 \times 10^{15} $$ events in this problem, even though there are only 52 pebbles.This is the algebra of the sample space? 
*Page 25:* A good strategy when trying to find the probability of an event is to start by thinking about whether it will be easier to find the probability of the event or the probability of its complement
**Multiplication rule:** Consider a compound experiment consisting of two sub-experiments, Experiment A and Experiment B. Suppose that Experiment A has a possible outcomes, and for each of those outcomes Experiment B has b possible outcomes. Then the compound experiment has ab possible outcome. For each outcome of A there's n outcomes that could happen on B. There's no guarantee that B happens after A. 
	Note: It doesn’t matter whether the same flavors are available on a cake cone as on a waffle cone. What matters is that there are exactly 3 flavor choices for each cone choice. **If for some strange reason it were forbidden to have chocolate ice cream on a waffle cone, with no substitute flavor available (aside from vanilla and strawberry), there would be 3 + 2 = 5 possibilities and the multiplication rule wouldn’t apply.** In larger examples, such complications could make counting the number of possibilities vastly more difficult.

**Why and when do we use n^k:** When sampling with replacement (e.g in a sack you take a ball out and then put it in again) 
**Why and when do we use n(n-1)...(n-(k-1))**: sampling without replacement.

(c) Show that for any events A and B (with probabilities not equal to 0 or 1), P(A|B) > P(A|B c ) is equivalent to P(B|A) > P(B|A c ).

	$$\text{Show that for any events }A \text{ and }B \text{ (with probabilities not equal to 0 or 1 ), } P(A|B)>P(A|B^{c}) \text{ is equivalent to }P(B|A)>P(B|A^{c})$$

$$ \sum_{i=0}^{\infty}p^{i}=\frac{1}{1-p}$$
![[Pasted image 20240221110502.png]]
$$\text{Independent trials that result in a success with probability }p \text{ are successively performed until a total of }r\text{ successes is obtained. Show that the probability that exactly }n\text{ trials are required is } \binom{n-1}{r-1}=p^{r}(1-p)^{n-r} 
\text{Hint: In order for it to take n trials to obtain r successes, how many successes must occur in the first n-1 trials?}
$$

6. An urn contains b black balls and r red balls. One of the balls is drawn at random, but when it is put back in the urn, c additional balls of the same color are put in with it. Now, suppose that we draw another ball. Show that the probability that the first ball was black, given that the second ball drawn was red, is b/(b + r + c).
$$$$


$$\text{Find the value of }P(B^{c}|A) \text{ given that the following is true: } P(A|B) = P(A^{c}|B^{c})=p \text{ and that p = 0.95}$$

$$ \frac{(0.05)P(B^{c})}{(0.90)P(B)+0.05} $$

Simplify $$ (0.95)P(B)+(0.05)P(B^{c}) $$ 
$$ \text{Given that the following is true: } P(A|B) = P(A^{c}|B^{c})=p \text{. How big should p be so that }P(B|A) = 0.9$$



# Examen de proba 

$$ 1) \text{ Sea } S \text{ el espacio muestral y } A \subset S, \text{ demuestre que } F=\{\emptyset,A,A^{c},S\} \text{ es una algebra.} $$ $$ 2) \text{ } 2a) \text{ Demuestre la identidad de Vandermonde} \binom{n+m}{k}=\sum_{j=0}^{k}\binom{m}{j}\binom{n}{k-j}.$$
$$ 2) \text{ } 2b) \text{ Demuestre que } \binom{2n}{n}=\sum_{k=0}^{n}\binom{n}{k}^2.$$
$$ 3) \text{ } 3a) \text{ De un ejemplo de eventos independientes } A \text{ y }B \text{ en un espacio muestral finito (ninguno de estos eventos es igual a } \emptyset \text{ o } S. \text{ e ilustre el ejemplo usando un diagrama del mundo de las piedritas (espacio muestral)}  $$

$$ 3) \text{ } 3b) \text{ Muestre que si } A \text{ y } B \text{ son independientes, entonces } Pr(A\cup B)=Pr(A)+Pr(B)-Pr(A)Pr(B)=1-Pr(A^{c})Pr(B^{c})$$
$$ 4) \text{De acuerdo con el CDC (Center of Disease Control and Prevention) los hombres que fuman tienen 23 veces una probabilidad más alta de desarrollar cáncer de pulmón en comparación con los hombres que no fuman. Además de acuerdo al CDC 21.6% de los hombres en USA fuman. ¿Cuál es la probabiidad de que un hombre en USA sea fumador dado que desarrolló cáncer de pulmón?} $$

$$ $$ 5) \text{ Enuncie y demuestre el teorema de la probabilidad total.}$$



# Cosas útiles 

## Pascal Identity
$$ \text{Show that for all positive integers } n \text{ and }k \text{ with } n \geq k, \binom{n}{k}+\binom{n}{k-1}=\binom{n+1}{k}$$
$$ \begin{align*} \binom{n}{k} + \binom{n}{k-1} &= \frac{n!}{k!(n-k)!} + \frac{n!}{(k-1)!(n-k+1)!} \ &= \frac{n!(n-k+1)}{k!(n-k+1)!} + \frac{n!k}{k!(n-k+1)!} \ &= \frac{ n!(n-k+1) + n!k} {k!(n-k+1)!} \ &= \frac{n!(n+1)}{k!(n-k+1)!} \ &= \frac{(n+1)!}{k!(n-k+1)!} \ &= \binom{n+1}{k} \end{align*} $$

