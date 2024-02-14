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
**Multplication rule:** Consider a compound experiment consisting of two sub-experiments, Experiment A and Experiment B. Suppose that Experiment A has a possible outcomes, and for each of those outcomes Experiment B has b possible outcomes. Then the compound experiment has ab possible outcome. For each outcome of A there's n outcomes that could happen on B. There's no guarantee that B happens after A. 
	Note: It doesn’t matter whether the same flavors are available on a cake cone as on a waffle cone. What matters is that there are exactly 3 flavor choices for each cone choice. **If for some strange reason it were forbidden to have chocolate ice cream on a waffle cone, with no substitute flavor available (aside from vanilla and strawberry), there would be 3 + 2 = 5 possibilities and the multiplication rule wouldn’t apply.** In larger examples, such complications could make counting the number of possibilities vastly more difficult.

**Why and when do we use n^k:** When sampling with replacement (e.g in a sack you take a ball out and then put it in again) 
**Why and when do we use n(n-1)...(n-(k-1))**: sampling without replacement.