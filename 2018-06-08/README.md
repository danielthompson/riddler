### Riddler Express 6/8/2018

Original post [here](https://fivethirtyeight.com/features/the-case-of-the-smudged-secret-message/) (fivethirtyeight.com).

From Max Rosmarin, a puzzle of villainous improv:

Imagine a group of five improvisers performing the following acting exercise: They all begin a scene together, establishing a character for themselves. Halfway through the scene, each improviser switches to a character someone else has been playing, such that the same five characters remain for the entire scene but no one plays the same character in both halves. As it happens, two of the characters in the scene are villains, and three are heroes.

Assuming that every valid reallocation of characters is equally likely, what is the probability that at least one of the actors plays a villain in both halves of the scene?

### Answer:

*20%.*

I started by laying out the people and role combinations in a table, like so:

a|1 - V|2 - V|3 - H|4 - H|5 - H
-|-|-|-|-
A|X| | | | 
B| |X| | | 
C| | |X| | 
D| | | |X| 
E| | | | |X

Each lettered person starts out with a numbered role (either V or H) denoted by *X*.

There are 5 * 4 * 3 * 2 * 1 = 120 permutations of people/role assignments. However, since no person can occupy their original role, only 44 of those 120 permutations are valid. 

> _Note: I didn't know what these "no-repeated-position" permutations were called, nor could I derive a formula to calculate them - I had to write a short script to calculate them directly. It turns out they are called *derangements* of the original sequence (see [Wikipedia](https://en.wikipedia.org/wiki/Derangement).

So, how many of those 44 include * A * * * or B * * * *?

|||
-|-|-|-
BADEC|BAECD|BCAED|BCDEA 
BCEAD|BDAEC|BDEAC|BDECA 
BEACD|BEDAC|BEDCA|CABED
CADEB|CAEBD|CDAEB|CDBEA
CDEAB|CDEBA|CEABD|CEBAD
CEDAB|CEDBA|DABEC|DAEBC
DAECB|DCAEB|DCBEA|DCEAB
DCEBA|DEABC|DEACB|DEBAC
DEBCA|EABCD|EADBC|EADCB
ECABD|ECBAD|ECDAB|ECDBA
EDABC|EDACB|EDBAC|EDBCA

Person A starts out in role 1, a villain. Since she must switch characters, she has 4 potential roles to switch to. She can only be a villain again if she switches to role 2. Therefore, the probability of that initial switch is 1/4. 

Similarly, person B starts out in role 2, a villain. She can only be a villain again if she switches to role 1. Since A has already switched, the probability of person B switching into role 1 depends on what A switched into:

P(B => 1) = (P(B => 1|A = 2) + P(B => 1|A = 3) + P(B => 1|A = 4) + P(B => 1|A = 5)) / 4

P(B => 1|A = 2) = 1/4

P(B => 1|A = 3) = 1/3

P(B => 1|A = 4) = 1/3

P(B => 1|A = 5) = 1/3

P(B => 1) = (1/4 + 1/3 + 1/3 + 1/3) / 4

P(B => 1) = (3/12 + 4/12 + 4/12 + 4/12) / 4

P(B => 1) = (15/12) * 1 / 4

P(B => 1) = (15/48)

Therefore, the probability of either of these things happening is 

=> 1/4 + 15/48

=> 12/48 + 15/48

=> 27/48

Furthermore, 


### Riddler Classic 6/8/2018

Original post [here](https://fivethirtyeight.com/features/the-case-of-the-smudged-secret-message/) (fivethirtyeight.com).

From Ben Gundry via Eric Emmet, find and replace with a twist:

Riddler Nation has been enlisted by the Pentagon to perform crucial (and arithmetical) intelligence gathering. Our mission: decode two equations. In each of them, every different letter stands for a different digit. But there is a minor problem in both equations.

In the first equation, letters accidentally were smudged on their clandestine journey to a safe room within Riddler Headquarters and are now unreadable. (These are represented with dashes below.) But we know that all 10 digits, 0 through 9, appear in the equation.

![Message 1](message1.png)

What digits belong to what letters, and what are the dashes?

In the second equation, our mathematical spies have said that one of the letters in the equation is wrong. But they can’t remember which one. Which is it?

![Message 2](message2.png)

### Answer:
