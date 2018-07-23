
## Probability Theory final lab

Probability theory is everywhere! In this big lab on probability theory, you'll practice all the concepts learned in the probability theory section.

As a field trip, the Flatiron School Data Science Immersive students are going to The Bronx Zoo. This is the setting of this lab.

## Exercise 1: birthday problem

Everyone is excited about the Zoo visit. The entire group consists of 20 students, 1 lead instructor and 2 TAs. One of the students is celebrating their birthday today. Now, one of the TA's is wondering what the probability is that at least 2 people who joined the trip have their birthdays on the same day. 

Hint: you know that just 1 person has their birthday today, so make sure to take this into account in your calculations! You can ignore the notion of a leap year.

## Solution

Use the complement rule to compute this: "at least 2 people" = 1 - "no one has the same birthday".

You know that 1 person has their birthday today. Now we want to check for the other 22 people that don't have their birthdays today. Then, "no one has the same birthday" can be computed as:

$(364*363*...*343)/364^{22}$



```python
import math
no_one = math.factorial(364)/(math.factorial(342)*364**22)
```


```python
at_least_two = 1-no_one
at_least_two
```




    0.4766435536772232



Now, the next thing that we'll ask you to do is wrap this in a function with 2 arguments: $n$ representing the number of days in this case, $n_{people}$ the number of people.


```python
def birthday(n, n_people):
    no_one = math.factorial(n)/(math.factorial(n-n_people)*(n**n_people))
    at_least_two = 1-no_one
    return at_least_two
```

Now, use this function to calculate the probability that two people will have birthdays in the same week. We're not sure if there are more birthdays are coming up this week, so make sure to take this into account again.


```python
two_each_week = birthday(52, 23)
two_each_week  # correct answer: 0.996895206
```




    0.9968952068726301



As you can see, the probability that two people have birthdays in the same week is almost 100%!

## Exercise 2

Jane, who is a student of the Flatiron School immersive program, is fascinated by koalas, panda bears and dolphins. Her classmate Albert did some research and estimated that the probability that the there will be koalas at the Bronx Zoo is equal to 70%, the probability that they have panda bears is equal to 40%, and the probability that they will have dolphins is equal to 80%. He also knew that the probability that they have all three of them is 22.4%.

- Are these events independent?
- What is the probability that they have koalas and dolphins, but no panda bears?
- What is the probability that they have koala bears given that they have panda bears?

**Are these events independent?**

This can be shown if $P(A \cap B \cap C) = P(A)P(B)P(C)$


```python
independent = 0.7*0.4*0.8 # you should get to 22.4%, meaning that events are independent
```

**What is the probability that they do have koalas and dolphins, but no panda bears?**


```python
P_no_panda= 0.6*0.7*0.8 # 0.336
P_no_panda = 0.336
```

**What is the probability that they have koala bears given that they have panda bears?**

$P(B \mid A) = \dfrac{P(B \cap A)}{P(A)} = \dfrac{P(B)P(A)}{P(A)}  $

As might have been expected since the probabilities are independent, $P(B \mid A) = P(B)$, so the probability is just equal to 70%!

## Exercise 3

The entire Flatiron crew is fascinated by the elephants. In the Bronx zoo, there keep male and female elephants seperately. It is known that female elephants lose their appetite during their early pregnancy. They'll skip 2 our of 10 meals during this period, whereas they'll eat 99% of their meals when they're not pregnant. On average, we know that 10% of the female elephants are pregnant in the Bronx zoo.

The Flatiron folks have been watching a particular elephant for a while, and noticed that while she skipped her breakfast, when they fed her again for lunch, she ate it all. 

What is the probability that this elephant is pregnant?

B = prob pregnant
O = prob outcome (not eat first, does eat second)

$$ P(B \mid O) = \dfrac{P(O\mid B)P(B)}{P(O\mid B')P(B')+P(O\mid B)P(B)}$$


```python
numer=(2/10)*(8/10)*0.1

denom=((1/100)*(99/100)*0.9 + (2/10)*(8/10)*0.1)

P_pregnant = numer/denom
P_pregnant #prob is 0.6423123
```




    0.6423123243677238



## Exercise 4: 

The Zoo is about to close, and there are 9 species that the group hasn't been able to see yet: 
4 reptiles (Alligators, lizards, snakes, turtles) and 5 mammals (chimpansees, zebras,  Bengalese tigers, polar bears and lions). Realistically, the group only has time to see five more species. The group decides that everyone can write down their top five. 

- How many top fives exist? ordering is not important.
- how many top fives exist if everyone has to choose 2 reptiles and 3 mammals?
- How many top fives exist everyone has to list their animals in decreasing order of importance (species they definitely still want to see first, then second most important second, etc)?
- how many top fives exist if everyone has to choose 2 reptiles and 3 mammals, and they are ordered?
- How many top fives exist if everyone has to choose 2 reptiles and 3 mammals and they are ordered, but everyone agrees they'll go watch the mammals first, and the reptiles last? 

**How many top fives exist?**


```python
import scipy.special as prob
import math
top_5 = prob.comb(9,5) 
top_5 #126
```




    126.0



**how many top fives exist if everyone has to choose 2 reptiles and 3 mammals?**


```python
top_5_3m= prob.comb(4,2)*prob.comb(5,3)
top_5_3m  # 60
```




    60.0



**how many top fives exist if they are ordered?**


```python
top_5_order = prob.perm(9,5)
top_5_order # 15120.0
```




    15120.0



**how many top fives exist if everyone has to choose 2 reptiles and 3 mammals, and then order is important?**


```python
top_5_3m= prob.comb(4,2) * prob.comb(5,3) * math.factorial(5)
top_5_3m # 7200
```




    7200.0



**How many top fives if everyone has to choose 2 reptiles and 3 mammals, order important and watch mammals first?**


```python
top_5_order= prob.perm(5,3)*prob.perm(4,2)
top_5_order # 720
```




    720.0



Among all those options, the group finally decided that everyone can write down an unordered top five (in whatever species they prefer). If there are 2 or more exact same top 5's, then they'll go watch the top fives with the most votes. If not, they'll randomly choose 5 animals to watch and ignore the top fives.

What is the probability that in the end, a random top 5 will be visited?



This is exactly the same problem as the birthday problem! But you have to do (1-...) again.


```python
1- birthday(126, 23) #11.77
```




    0.11772303234775938


