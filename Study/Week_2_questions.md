## Week 2 Review Questions

1) What is Bayes rule?  What does it relate?

**answer**

  - ![](http://www.eecs.qmul.ac.uk/~norman/BBNs/image/BBNs0050.gif)
  - Bayes Rule uses 4 pieces of information to predict probability. It is common to think of Bayes rule in terms of updating our belief about a hypothesis A in the light of new evidence B. Specifically, our posterior belief P(A|B) is calculated by multiplying our prior belief P(A) by the likelihood P(B|A) that B will occur if A is true.
    - The denominator P(B) in the equation is a normalising constant which can be computed, for example, by marginalisation whereby ![](http://www.eecs.qmul.ac.uk/~norman/BBNs/image/BBNs0051_wmf.gif)

      Hence we can state Bayes rule in another way as:
    ![](http://www.eecs.qmul.ac.uk/~norman/BBNs/image/BBNs0052_wmf.gif)


**Example**

  `Suppose that we have two bags each containing black and white balls. One bag contains three times as many white balls as blacks. The other bag contains three times as many black balls as white. Suppose we choose one of these bags at random. For this bag we select five balls at random, replacing each ball after it has been selected. The result is that we find 4 white balls and one black. What is the probability that we were using the bag with mainly white balls?`

  >Solution. Let A be the random variable "bag chosen" then A={a1,a2} where a1 represents "bag with mostly white balls" and a2 represents "bag with mostly black balls" . We know that P(a1)=P(a2)=1/2 since we choose the bag at random.
Let B be the event "4 white balls and one black ball chosen from 5 selections".
Then we have to calculate P(a1|B).
![](http://www.eecs.qmul.ac.uk/~norman/BBNs/image/BBNs0053_wmf.gif)

  > Now, for the bag with mostly white balls the probability of a ball being white is ¾ and the probability of a ball being black is ¼. Thus, we can use the Binomial Theorem, to compute P(B|a1) as:

  > ![](http://www.eecs.qmul.ac.uk/~norman/BBNs/image/BBNs0054_wmf.gif)
  >
  > Similarly

  > ![](http://www.eecs.qmul.ac.uk/~norman/BBNs/image/BBNs0055_wmf.gif)
  >
  hence

  > ![](http://www.eecs.qmul.ac.uk/~norman/BBNs/image/BBNs0056_wmf.gif)
</details>
---

2) How many ways can I arrange n unique objects?  Formula?
  - n! ways

**Example**

`How many different ways can the letters P, Q, R, S be arranged?`

> The answer is 4! = 24.

---

3) How many ways can I arrange k of n unique objects, where k < n?  What is this called? Formula?

  - **\(\binom{n}{k}=\frac{n!}{(n-k)!}\)**
  - This is called a **permutation**. When order matters each individual ordering is counted

**Example**

`How many different poker hands (5 cards) can you have? A deck holds 52 cards. `

> The answer is: \(\frac{52!}{(47!)}\) = 311,875,200 different ways


---

4) If order doesn't matter, how many ways can I pick of n objects? What is this called? Formula?

  - $$\binom{n}{k}=\frac{n!}{k!(n-k)!}
  - This is called a **combination**. When order does **not** matter we divide by `k!(n-k)!` instead of just `(n-k)!` like in permutations because we're dividing out all of the redudant orders that have the same number of each selection.

**Example**

`How many different poker hands (5 cards) can you have? A deck holds 52 cards. `

> The answer is: \(\frac{52!}{(5! * 47!)}\) = 2,598,960 different ways
>
> This is smaller than the permutation value calculated in question 3 and this makes intuitive sense because if you don't care about order. For example if you wanted picked the following five cards:
 >- Queen of Hearts
 >- Queen of Diamonds
 >- Ace of Spades
 >- 7 of Clubs
 >- 2 of Spades
 >
 >It doesn't matter which order you selected in, that is just 1 combination out of 2,598,960 total combinations of cards. Whereas, if you were curious how many different **permutations** you could achieve, you'd care about what order you selected cards in so there would be a total of 5! different ways to order those particular cards and you need to compensate for this by using the permutations equation.

---

5) What distribution would you use in the following cases:

![](/Users/damien/Documents/DSI/Study/Distributions.png)

  - What is the probability that the mean volume of 50 bottles is less than 500 ml?
    - Gaussian
  * Deciding to go for a run or not.
    - Bernoulli
  - Determining how many days pass before you finally decide to go for a run.
    - Geometric
  * Determining how likely it is that you go for 10 runs in a month.
    - Binomial
  * Calculating how much water falls into an open swimming pool during a rain storm.
    - Poisson or Uniform (?)
  * Assuming you run at a 9 minute mile pace avg pace, determining how likely it is that you pass the 3 mile mark in a race in 25 minutes?
    - Exponential

6) What is the central limit theorem?
- The **Central Limit Theorem** (CTL) is a statistical theory that states that with a sufficiently large sample size, that is *representative* of the population, with a *finite level of variance*, the mean of all of the sample subsets will approximate the true population mean on a normal distribution.
- Basically this is saying if you take a representative sample of a population, the sample means when plotted follow a normal distribution and the population mean, depending on your significance level should fall somewhere within this distribution.

---

7) How would you go about implementing a bootstrap, and why would you do it?
  > - You would implement a bootstrap when the theoretical distribution of the statistic (parameter) is complicated or unknown. (E.g. Median or Correlation)
  >- When the sample size is too small for traditional methods.
  >    - However, for smaller samples, boostrap percentile intervals tend to be narrower and *ultimately less accurate* than common t intervals, though *more accurate* for large samples. This is because the more accurate test for the substitution data would be with a *pivotal statistic* and t statistics are close to pivotal[¹](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC4784504/)[²](https://www.quora.com/What-is-the-difference-between-a-pivotal-quantity-a-sufficient-statistic-and-just-a-general-estimator)
  >
  >- Favor accuracy over computational cost.
  >
  > **How to Bootstrap:**
  >    1. Start with your dataset of size n
  >    2. Sample from your dataset with replacement to create 1 bootstrap >sample of size n
  >    3. Repeat B times (200-2000, as mentioned in the reading)
  >    4. Each bootstrap sample can then be used as a separate dataset for
estimation and model fitting
>
  >```python
>
>    def bootstrap_ci(sample, stat_function=np.mean, iterations=1000, ci=95):
>    fig, ax = plt.subplots(1,1,figsize=(6,6))
>    means = []
>    for i in range(iterations):
>        a = [choices(array) for j in range(len(array))]
>        means.append(stat_function(a))
>    ax.hist(means,bins=50)
>    meanMeans = np.mean(means)
>    stdMeans = np.sqrt(np.var(means))
>    [CI_low, CI_high] = np.percentile(means,[(100-ci)/2,100-(100-ci)/2])
>    print('bootstrap {} times'.format(iterations))
>    print('mean of means = {0:0.4}'.format(meanMeans))
>    print('stdev of means = {0:0.4}'.format(stdMeans))
>    print('CI = {} to {}'.format(CI_low.round(3),CI_high.round(3)))
>
>  ```

---

8) Conceptual - Studies of American health habits often show that urban dwellers have lower average body mass index (BMI)s and are less likely to be obese than people who live in other types of locations. A naive scientist would conclude that living in cities causes people to be thinner.
Discuss at least two confounding variables and how you might change your sampling parameters to mitigate these effects.
> 1. Urban dwellers might use different means of transportation than people who live in non-urban areas. The confounding variable would be the fact that an urban dweller may walk to more places, so in fact the urban dweller's mode of transportation is in fact the variable that ultimately has influence over their lower BMI.
> 2. Urban dwellers may have higher rent/mortgage costs and as a result are less capable of purchasing surplus food compared to people who live elsewhere. The confounding variable would be that urban dwellers eat less food and therefore have a lower BMI.
>
> `You could change your sampling parameters to investigate if perhaps people who do not live in urban areas are less active, or eat more than urban dwellers.`
>
>
> **Confounding Variable:** an outside influce that changes the effect of a dependent and independent variable.
> - Example:
>      - There is the correlation between murder rate and the sale of ice-cream. As the murder rate raises so does the sale of ice-cream. One suggestion for this could be that murderers cause people to buy ice-cream. This is highly unlikely. A second suggestion is that purchasing ice-cream causes people to commit murder, also highly unlikely. Then there is a third variable which includes a confounding variable. It is distinctly possible that the weather causes the correlation. While the weather is icy cold, fewer people are out interacting with others and less likely to purchase ice-cream. Conversely, when it is hot outside, there is more social interaction and more ice-cream being purchased. In this example, the weather is the variable that confounds the relationship between ice-cream sales and murder.[³](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC4784504/)
---
9) I surveyed 200 people and looked at their average expenditure on coffee/tea by season. I found that the mean coffee expenditure in winter was greater than that in the summer, with a p-value of .02. Is this significant? Why or why not? What significance level would convince you?
> - This may be significant but it depends on what statistic was used and what was tested. For example:
>     - if a t-test was performed, what was the significant level (\(\alpha\))
>     - What was the null hypothesis and alternative hypothesis?
>     - This p value would be significant if the hypothesis was defined as:
>        - H0: There is no change in the average expenditure on coffee/tea with respect to changing seasons.
>        - H1: There is an increase in the expenditure on coffee/tea during the winter vs summer season.


---
10) What are the four factors that influence statistical power?
> 1. Sample Size (N)
> 2. significance Level (\(\alpha\))
> 3. Statistical Power Variable (1-\(\beta\))[⁴](https://effectsizefaq.com/category/statistical-power/)
>     - Basically, the statistical power variable is the likelihood that a study will detect an effect if there is a statistically significant effect to be detected. It demonstrates how likely a type II (false negative) error is. As the statistical power variable increases, beta decreases, and fewer type II errors are expected.
> 4. Effect Size
>     - The actual measurement of the magnitude of the change some variable has on the data you are studying. The measurement of the effect.


---
11) Draw the distributions associated with a hypothesis test between two means.  Label the critical value, the Type I error, and Type II error.  Indicate on the diagram how you'd calculate the statistical power.
> ![](/Users/damien/Documents/DSI/Study/Errors.png)
> - The statistical power, (1-\(\beta\)), is the 1 minus the red area in the image.

---

# Additional Resources
> If there are any additional notes you'd like to keep, make them in the box below:
> ~~~~

## Helpful Images

#### Selecting a statistical test based on continuous or discrete data:
![](/Users/damien/Documents/DSI/Study/flowchart.png)

####

## Distribution Codes

#### Gamma & Overlay (Individial_Day_7)
![](/Users/damien/Documents/DSI/Week2/Day_7_Samples_Estimation/overlay.png)
```python
plt.subplots(1,1,figsize=(12, 5))
plt.hist(x=rainfall['Jan'], normed=True)
xt = plt.xticks()[0]
xmax = max(xt)
lnspc = list(np.linspace(0, 16, len(rainfall['Jan'])))
plt.plot(lnspc, stats.gamma.pdf(lnspc, u), lw=2, c='green', label='Gamma')
plt.plot(x, stats.poisson.pmf(x, mu), lw=2, c='red', label='Poisson')
plt.legend()
plt.savefig('overlay.png')
```

#### Poisson (Esimation_MOM_Example_2)
![](/Users/damien/Documents/DSI/Week2/Day_7_Samples_Estimation/estimation_students/poisson.png)
```python
fig, ax = plt.subplots(1, 1, figsize=(12, 5))
ax.plot(x, stats.binom.pmf(x, n, p), 'bo', ms=8, label='poisson pmf')
ax.vlines(x, 0, stats.binom.pmf(x, n, p), colors='b', lw=5, alpha=0.5)
ax.set_title("Binomial, $n={0}, p = {1:0.2f}$".format(n, p));
```

## Other Codes

#### Frequency Codes (AB_testing_afternoon)

```python
# fair dice distribution
fair_rolls = np.random.randint(1, 7, size = (120, ))

# weighted_rolls
unfair_rolls = np.random.choice(np.arange(1, 7), p = [0.1, 0.05, 0.3, 0.4, 0.05, 0.1], size = (120,))

# get frequencies for fair counts
fair_counts = Counter(fair_rolls)
fair_freqs = [fair_counts[i] for i in range(1, 7)]

# frequencies for unfair counts
unfair_counts = Counter(unfair_rolls)
unfair_freqs = [unfair_counts[i] for i in range(1, 7)]

#fair die
scs.chisquare(fair_freqs)

#unfair die
scs.chisquare(unfair_freqs)



## np.random.choices(sample, size=len(sample), replace=True)

## np.percentile(range, p=95)
