# GATE DA · Day 1 of 20
## Probability I: counting, axioms, conditional probability, Bayes

*About a 30-minute read, in 3 parts. Part 1 is the theory, Part 2 the worked examples and cheat card, Part 3 the practice questions. Answers are at the very bottom of Part 3.*

---

## 1. What this is and how GATE asks it

Probability & Statistics is the biggest maths block in DA. Your tracker puts it at about **18 marks a paper**. Today's material is the base for every other probability question you'll meet, and it also shows up in General Aptitude.

The usual question shapes:
- **NAT:** "How many ways…", "find P(A | B) to two decimals", Bayes with a factory or test-and-disease story.
- **MSQ:** "Which statements are always true?" These are mixes of independent, mutually exclusive and conditional claims, and one wrong tick costs you the whole question.
- **MCQ:** a short conditional-probability story with four close options.

The skill being tested is **turning words into the right formula**. The arithmetic is usually small.

---

## 2. The ideas from zero

### 2.1 Counting: how many ways can something happen?

**Two base rules**
- **Multiplication rule:** a task done in step 1 (m ways) and then step 2 (n ways) can be done in **m × n** ways. A 4-digit PIN with digits 0–9 gives 10 × 10 × 10 × 10 = 10⁴ = 10,000.
- **Addition rule:** if a task is done *either* one way (m) *or* another way (n), with no overlap, you get **m + n** ways.

The word "and" between steps means multiply. The word "or" between cases that don't overlap means add.

**Ask two questions first**

| | Order matters | Order doesn't matter |
|---|---|---|
| **No repetition** | Permutation ⁿPᵣ = n!/(n−r)! | Combination ⁿCᵣ = n!/(r!(n−r)!) |
| **Repetition allowed** | nʳ | C(n+r−1, r) (stars and bars) |

A PIN, a ranking, a seating or a password is ordered. A team, a hand of cards or a committee is unordered.

**Permutations**
- All n distinct items in a row: n!
- Choose r of n and arrange them: ⁿPᵣ = n!/(n−r)!. For example, 4-digit codes with no repeated digit: ¹⁰P₄ = 10·9·8·7 = 5,040.
- **Identical items:** arrange n items where n₁ are alike, n₂ are alike, and so on: **n!/(n₁! n₂! …)**. You divide because swapping identical items gives the same arrangement.

**Circular arrangements**
- n people around a round table: **(n−1)!**. Rotations are the same seating, so fix one person and arrange the rest.
- A necklace or garland, where flipping it over gives the same thing: **(n−1)!/2** for n ≥ 3.
- "Two particular people must sit together": glue them into one block. That gives (n−1) units around the table, so (n−2)! arrangements, times 2! for the order inside the block.

**Combinations**
- ⁿCᵣ = n!/(r!(n−r)!). Useful facts:
  - ⁿCᵣ = ⁿCₙ₋ᵣ (choosing who's in is the same as choosing who's out)
  - Pascal: ⁿCᵣ = ⁿ⁻¹Cᵣ₋₁ + ⁿ⁻¹Cᵣ (either a given item is in, or it isn't)
  - Σᵣ ⁿCᵣ = 2ⁿ (every subset)
  - ⁿPᵣ = ⁿCᵣ × r!

**Stars and bars (identical items into distinct boxes)**
- Put n identical items into k distinct boxes, empty boxes allowed. This is the same as counting non-negative solutions of x₁+…+xₖ = n. The count is **C(n+k−1, k−1)**.
  - Picture n stars and k−1 bars in a line. Choosing where the bars go fixes the split.
- **Each box gets at least 1:** count positive solutions, **C(n−1, k−1)**. Give each box one item first, then share out the remaining n−k freely.
- With **distinct** items into k distinct boxes, each item picks a box: **kⁿ**.

**Handy tricks**
- **"At least one"** → use the complement: total − none.
- **"Not together"** → total − together.
- **"No two of these adjacent"** (gap method): arrange the m others first. That leaves m+1 gaps. Choose k gaps, C(m+1, k), then arrange the k items in them.
- **Derangements** (nobody gets their own item back): D(n) = n!·[1 − 1/1! + 1/2! − … + (−1)ⁿ/n!] ≈ n!/e.
  - D(1)=0, D(2)=1, D(3)=2, D(4)=9, D(5)=44. Learn these by heart.
- **Inclusion–exclusion for counting:** |A∪B∪C| = |A|+|B|+|C| − |A∩B| − |A∩C| − |B∩C| + |A∩B∩C|.

---

### 2.2 Sample space and events

- An **experiment** is anything with an uncertain outcome (toss, roll, draw).
- The **sample space S** is the set of all outcomes. Two coin tosses: S = {HH, HT, TH, TT}.
- An **event** is any subset of S. "At least one head" = {HH, HT, TH}.
- The set operations have plain meanings:
  - A ∪ B = "A or B (or both)"
  - A ∩ B = "both"
  - Aᶜ = "not A"
  - A ∩ B = ∅ means they **can't happen together** (mutually exclusive)
- **De Morgan:** (A∪B)ᶜ = Aᶜ ∩ Bᶜ and (A∩B)ᶜ = Aᶜ ∪ Bᶜ. "Not (A or B)" means neither happens.
- If all outcomes are **equally likely**: **P(A) = |A| / |S|**. Counting is why section 2.1 came first.

---

### 2.3 Probability axioms and the rules that follow

Every probability rule comes from three axioms (Kolmogorov):
1. **P(A) ≥ 0** for every event A.
2. **P(S) = 1.**
3. If A₁, A₂, … are pairwise disjoint, **P(A₁ ∪ A₂ ∪ …) = P(A₁) + P(A₂) + …**

Consequences you'll use constantly:
- P(∅) = 0, and 0 ≤ P(A) ≤ 1.
- **Complement:** P(Aᶜ) = 1 − P(A).
- **Monotonicity:** if A ⊆ B then P(A) ≤ P(B).
- **Union of two (addition rule):** P(A ∪ B) = P(A) + P(B) − P(A ∩ B). You subtract because the overlap was counted twice.
- **Union of three:** P(A∪B∪C) = ΣP(single) − ΣP(pairs) + P(all three).
- **"A but not B":** P(A ∩ Bᶜ) = P(A) − P(A ∩ B).
- **Exactly one of A, B:** P(A) + P(B) − 2P(A∩B).
- **Bounds on the overlap:** max(0, P(A)+P(B)−1) ≤ P(A∩B) ≤ min(P(A), P(B)). GATE likes "what is the least or greatest possible P(A∩B)?"
- **Union bound (Boole):** P(A∪B) ≤ P(A) + P(B).

A quick sanity check: if a question gives P(A)=0.7 and P(B)=0.6, the overlap must be at least 0.3. If they claim A and B are mutually exclusive, that's impossible.

---

### 2.4 Joint, marginal and conditional probability

Think of a 2×2 table of 100 students, by gender and whether they passed:

| | Pass | Fail | Total |
|---|---|---|---|
| Male | 30 | 20 | 50 |
| Female | 35 | 15 | 50 |
| Total | 65 | 35 | 100 |

- **Joint:** P(Male ∩ Pass) = 30/100 = 0.30. These are the cells.
- **Marginal:** P(Pass) = 65/100 = 0.65. These are the row and column totals: add up the joint over the other variable.
- **Conditional:** P(Pass | Male) = 30/50 = 0.60. **Zoom into the "Male" row and treat it as the new universe.**

Formally:

**P(A | B) = P(A ∩ B) / P(B), when P(B) > 0.**

- Rearranged, this is the **multiplication rule:** P(A ∩ B) = P(A | B)·P(B) = P(B | A)·P(A).
- **Chain rule** for several events: P(A∩B∩C) = P(A)·P(B|A)·P(C|A∩B). Use this for drawing cards **without replacement** one after another.
- P(· | B) is itself a full probability. So P(Aᶜ | B) = 1 − P(A | B), and the union rule works inside the condition.
- **Trap:** P(A | B) ≠ P(B | A) in general. P(fever | flu) is high, but P(flu | fever) may not be. Flipping that bar is exactly what Bayes does properly.

---

### 2.5 Independence

**A and B are independent if P(A ∩ B) = P(A)·P(B).**

When P(B) > 0, this is the same as **P(A | B) = P(A)**: knowing B happened tells you nothing new about A.

- If A, B are independent, then so are (A, Bᶜ), (Aᶜ, B) and (Aᶜ, Bᶜ).
  - Use this for "probability neither happens" = (1−P(A))(1−P(B)).
- An event with probability 0 or 1 is independent of every event.
- Independence is about **probabilities**, not about whether the events are physically linked. Always check with the formula.

**Mutually exclusive vs independent (the most-tested confusion)**
- **Mutually exclusive:** P(A ∩ B) = 0. If one happens, the other **can't**.
- **Independent:** P(A ∩ B) = P(A)P(B). One happening **doesn't change** the other's chances.
- If P(A) > 0 and P(B) > 0, then disjoint events are **always dependent**: knowing A happened forces P(B | A) = 0 ≠ P(B).
- So "mutually exclusive and independent" is only possible when at least one event has probability 0.

**Three or more events**
- **Pairwise independent:** every pair satisfies the product rule.
- **Mutually (fully) independent:** every pair **and** the triple: P(A∩B∩C) = P(A)P(B)P(C). For n events you need 2ⁿ − n − 1 equations, which is 4 for n = 3.
- **Pairwise does NOT imply mutual.** The classic example is in Part 2.

---

### 2.6 Law of total probability and Bayes' theorem

**Partition:** events B₁, …, Bₙ are a partition if they are disjoint and together cover all of S. Every outcome lands in exactly one Bᵢ. Examples: which machine made a part, which urn was picked, diseased or healthy.

**Law of total probability.** Get P(A) by going through every route:

**P(A) = Σᵢ P(A | Bᵢ)·P(Bᵢ)**

On a tree diagram, multiply along each branch and add the branches that end in A.

**Bayes' theorem.** Reverse the condition once you've seen A:

**P(Bⱼ | A) = P(A | Bⱼ)·P(Bⱼ) / Σᵢ P(A | Bᵢ)·P(Bᵢ)**

- P(Bⱼ) is the **prior** (what you believed before the evidence).
- P(A | Bⱼ) is the **likelihood** (how well Bⱼ explains the evidence).
- P(Bⱼ | A) is the **posterior** (your updated belief).
- The denominator is just P(A) from the total probability rule.

**In words:** posterior = (that branch's joint probability) ÷ (sum of all branches' joint probabilities that end in the evidence).

**Odds form** (fast for two hypotheses): posterior odds = prior odds × likelihood ratio.

**Base-rate neglect:** when the prior is tiny, even a very accurate test gives a small posterior. Example 3 in Part 2 shows this. GATE (and real life) loves it.

*Part 2 has a tree diagram, 7 worked examples, traps and the cheat card.*

## Part 2: worked examples, traps, cheat card

The tree above is **Example 3**. Multiply along each path to get the joint probabilities at the leaves. Bayes then keeps only the leaves that match the evidence (the coral "+" leaves) and asks what share of them came from the disease branch.

---

## 3. Worked GATE-style examples

**Example 1: identical objects (NAT)**
*How many distinct arrangements of the letters of MISSISSIPPI are there?*

- 11 letters: M×1, I×4, S×4, P×2.
- Arrangements = 11! / (1!·4!·4!·2!) = 39,916,800 / (24·24·2) = **34,650**.
- Follow-up GATE twist, "all four S's together": glue SSSS into one block. You now have 8 units (M, I×4, P×2, [SSSS]): 8!/(4!·2!) = 840.

**Example 2: stars and bars (NAT)**
*How many non-negative integer solutions does x₁ + x₂ + x₃ = 10 have? How many if every xᵢ ≥ 1?*

- Non-negative: n = 10 stars, k = 3 boxes, so C(10+3−1, 3−1) = C(12, 2) = **66**.
- Each ≥ 1: give each variable 1 first, which leaves 7 to share freely: C(7+2, 2) = C(9, 2) = **36**. Same as the formula C(n−1, k−1) = C(9, 2).
- The same maths answers "distribute 10 identical chocolates among 3 kids".

**Example 3: Bayes, disease test (the tree above)**
*A disease affects 1% of people. The test catches 99% of sick people (sensitivity). It also flags 5% of healthy people (false-positive rate). A random person tests positive. What is P(disease | +)?*

- Joint, disease and +: 0.01 × 0.99 = 0.0099.
- Joint, healthy and +: 0.99 × 0.05 = 0.0495.
- P(+) by total probability = 0.0099 + 0.0495 = 0.0594.
- P(D | +) = 0.0099 / 0.0594 = **1/6 ≈ 0.167**.
- Lesson: the test is "99% accurate", yet a positive result means only about a 17% chance of disease. Healthy people outnumber sick ones 99 to 1, so their 5% false alarms swamp the true positives. That is base-rate neglect.
- Odds check: prior odds 1:99, likelihood ratio 0.99/0.05 = 19.8, posterior odds 19.8:99 = 1:5. So the probability is 1/6. ✓

**Example 4: urn Bayes (MCQ)**
*Urn A has 3 red and 2 black balls. Urn B has 1 red and 4 black. You toss a fair coin to pick an urn, then draw one ball. It is red. P(it came from urn A)?*

- P(R | A) = 3/5 = 0.6 and P(R | B) = 1/5 = 0.2. Each prior is 0.5.
- P(A | R) = (0.5 × 0.6) / (0.5 × 0.6 + 0.5 × 0.2) = 0.3 / 0.4 = **0.75**.
- Shortcut: with equal priors, the posterior is just the share of the likelihoods, 0.6 / (0.6 + 0.2).

**Example 5: inclusion–exclusion (NAT)**
*How many integers from 1 to 1000 are divisible by 2, 3 or 5?*

- Singles: ⌊1000/2⌋ = 500, ⌊1000/3⌋ = 333, ⌊1000/5⌋ = 200.
- Pairs: ⌊1000/6⌋ = 166, ⌊1000/10⌋ = 100, ⌊1000/15⌋ = 66.
- Triple: ⌊1000/30⌋ = 33.
- Total: 500 + 333 + 200 − 166 − 100 − 66 + 33 = **734**.
- As a probability, a random number from 1–1000 is divisible by 2, 3 or 5 with probability 0.734. The "not divisible by any" count is 1000 − 734 = 266.

**Example 6: pairwise but not mutually independent (MSQ favourite)**
*Toss two fair coins. A = first is H, B = second is H, C = both tosses show the same face.*

- S = {HH, HT, TH, TT}, each with probability ¼.
- P(A) = P(B) = P(C) = ½.
- A∩B = {HH}, A∩C = {HH}, B∩C = {HH}, each ¼ = ½·½. So **every pair is independent.** ✓
- A∩B∩C = {HH}, so P = ¼. But P(A)P(B)P(C) = ⅛. **Not mutually independent.** ✗
- Intuition: once you know A and B, C is certain. Any two of them say nothing about the third alone, but two together decide it.

**Example 7: complement, "at least one" (MCQ)**
*Roll a fair die 4 times. P(at least one six)?*

- P(no six in 4 rolls) = (5/6)⁴ = 625/1296.
- P(at least one six) = 1 − 625/1296 ≈ **0.518**.
- Don't add 1/6 four times to get 4/6. That double-counts the overlaps, and with 7 rolls it would exceed 1.

---

## 4. Common traps

1. **Disjoint vs independent.** Disjoint events with positive probabilities are always dependent. If an MSQ option says "A, B mutually exclusive ⇒ independent", it's false.
2. **P(A|B) vs P(B|A).** Read which event is *given* (after the bar). "Probability a defective item came from machine 3" is P(M3 | defective), so it's Bayes.
3. **With vs without replacement.** Without replacement, use the chain rule with shrinking denominators (4/52 then 3/51). With replacement the draws are independent.
4. **P(2nd card is an ace) with no other information is still 4/52 = 1/13.** By symmetry, every position is equally likely to hold any card. People wrongly try to "condition on the first card" when nothing was revealed.
5. **Circular counting:** (n−1)! for a round table, but (n−1)!/2 only when mirror images count as the same (necklaces, garlands). People seated at a table are *not* divided by 2.
6. **Identical vs distinct objects.** Distinct objects into boxes is kⁿ. Identical objects is stars and bars. Read the noun carefully: "balls", "identical balls" and "different coloured balls" mean different things.
7. **"At least one"** is almost always 1 − P(none). Direct casework is slow and error-prone.
8. **Pairwise ≠ mutual independence.** For "mutually independent" you must check the triple product too.
9. **Conditional probabilities across different conditions don't add.** P(A|B) + P(A|Bᶜ) need not be 1. What is always 1 is P(A|B) + P(Aᶜ|B).
10. **NAT rounding.** Keep fractions exact until the last step. 1/6 = 0.1667, so enter 0.17 if they ask for 2 decimals. GATE gives a range, but intermediate rounding can push you out of it.

---

## 5. Cheat card (screenshot this)

**Counting**
- Ordered, no repeats: ⁿPᵣ = n!/(n−r)!. Ordered, repeats allowed: nʳ.
- Unordered: ⁿCᵣ = n!/(r!(n−r)!). Unordered with repeats (stars and bars): C(n+r−1, r).
- Identical items: n!/(n₁!n₂!…).
- Round table: (n−1)!. Necklace: (n−1)!/2.
- n identical items into k boxes: C(n+k−1, k−1). With each box ≥ 1: C(n−1, k−1).
- n distinct items into k boxes: kⁿ.
- Derangements: D(n) ≈ n!/e. D(3)=2, D(4)=9, D(5)=44.
- No two adjacent (gap method): arrange the m others, then choose C(m+1, k) gaps and arrange the k items.

**Rules**
- P(Aᶜ) = 1 − P(A).
- P(A∪B) = P(A) + P(B) − P(A∩B).
- P(A∪B∪C) = Σ singles − Σ pairs + triple.
- P(A∩Bᶜ) = P(A) − P(A∩B).
- max(0, P(A)+P(B)−1) ≤ P(A∩B) ≤ min(P(A), P(B)).

**Conditional**
- P(A|B) = P(A∩B)/P(B).
- P(A∩B) = P(A|B)P(B).
- Chain rule: P(A∩B∩C) = P(A)P(B|A)P(C|A∩B).

**Independence**
- P(A∩B) = P(A)P(B) ⇔ P(A|B) = P(A).
- This carries over to complements.
- For 3 events, mutual independence needs all 3 pairs plus the triple.

**Total probability**
- P(A) = Σ P(A|Bᵢ)P(Bᵢ) over a partition.

**Bayes**
- P(Bⱼ|A) = P(A|Bⱼ)P(Bⱼ) / Σ P(A|Bᵢ)P(Bᵢ).
- Odds form: posterior odds = prior odds × P(A|B₁)/P(A|B₂).

*Part 3: practice questions.*

## Part 3: practice (try them before scrolling to the answers)

Give yourself about 12 minutes for all six, and enter your score out of 6 on the Daily Notes tab.

**Q1 (NAT).** 6 people sit around a round table. In how many ways can they sit if two particular people must sit next to each other?

**Q2 (MCQ).** P(A) = 0.5, P(B) = 0.4 and P(A ∪ B) = 0.7. Which is true?
(a) A and B are mutually exclusive
(b) A and B are independent
(c) P(A | B) = 0.4
(d) P(A ∩ B) = 0.1

**Q3 (MSQ).** A and B are mutually exclusive, with P(A) > 0 and P(B) > 0. Which of these are ALWAYS true?
(a) A and B are independent
(b) P(A | B) = 0
(c) P(A ∪ B) = P(A) + P(B)
(d) P(A ∩ Bᶜ) = P(A)

**Q4 (NAT, 2 decimals).** Machines M1, M2 and M3 make 50%, 30% and 20% of a factory's output. Their defect rates are 2%, 3% and 4%. A randomly picked item is defective. What is the probability it came from M3?

**Q5 (NAT).** How many 5-card hands from a standard 52-card deck contain exactly 2 aces?

**Q6 (MCQ).** Two cards are drawn one after another, without replacement, from a well-shuffled deck. Nobody looks at the first card. What is the probability that the second card is an ace?
(a) 3/51  (b) 4/51  (c) 1/13  (d) 1/17

---

**Tomorrow, Day 2:** random variables, PMF, PDF and CDF, expectation and variance, and the discrete distributions: Bernoulli, binomial and Poisson. That's where today's counting turns into formulas you'll use everywhere.

---
---
---

## Answers

**Q1: 48.** Glue the pair into one block, giving 5 units around the table: (5−1)! = 24. Multiply by 2 for the pair's internal order: 24 × 2 = 48.

**Q2: (b).** P(A∩B) = 0.5 + 0.4 − 0.7 = 0.2, and P(A)P(B) = 0.5 × 0.4 = 0.2, so they are independent.
- (a) is false since the overlap is 0.2, not 0.
- (c) is false since P(A|B) = 0.2/0.4 = 0.5.
- (d) is false since the overlap is 0.2.

**Q3: (b), (c), (d).**
- (a) is false: disjoint events with positive probabilities are dependent, because P(A∩B) = 0 ≠ P(A)P(B).
- (b) is true: P(A|B) = 0/P(B) = 0.
- (c) is the addition rule with a zero overlap.
- (d) is true: P(A∩Bᶜ) = P(A) − P(A∩B) = P(A) − 0.

**Q4: 0.30** (exactly 8/27 ≈ 0.296).
- P(defective) = 0.5×0.02 + 0.3×0.03 + 0.2×0.04 = 0.010 + 0.009 + 0.008 = 0.027.
- P(M3 | defective) = 0.008 / 0.027 ≈ 0.296.

**Q5: 103,776.**
- Choose 2 of the 4 aces: C(4,2) = 6.
- Choose 3 of the 48 non-aces: C(48,3) = 17,296.
- 6 × 17,296 = 103,776.

**Q6: (c) 1/13.**
- Total probability: P(2nd ace) = (4/52)(3/51) + (48/52)(4/51) = (12 + 192)/2652 = 204/2652 = 1/13.
- Or use symmetry: nothing was revealed about the first card, so the second card is just as likely as the first to be an ace.
- (a) 3/51 is the trap: that's P(2nd is an ace | 1st was an ace).

Day 1 of your GATE notes went out in three parts: probability, counting and Bayes, with a tree diagram, seven worked examples, a cheat card, and six practice questions with answers at the bottom. I ran every number through Python first. Once you've done the six questions, tick Read and put your score in the Daily Notes tab. Day 2 (random variables and discrete distributions) arrives tomorrow at 8:40 AM.
