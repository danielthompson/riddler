### Riddler Express 8/14/2020

Original post [here](https://fivethirtyeight.com/features/can-you-cover-the-globe/) (fivethirtyeight.com).

From Dean Ballard comes a sneaky sorting of squares:

> Suppose you have a rope that goes all the way around the Earth’s equator, flat on the ground. (For the entirety of this puzzle, you should assume that the Earth is a perfect sphere with a radius of 6,378 kilometers.)
> 
> You want to lengthen the rope just the right amount so that it’s 1 meter off the ground all the way around the Earth. How much longer did you have to make the rope?
> 
> If you’ve never heard this puzzle before, the answer is surprisingly small — about 6.28 (i.e., 2𝜋) meters. Also, spoiler alert! (Darn, I was one sentence too late.)
> 
> Now, instead of tying the Earth up with rope, you’ve moved on to covering the globe with a giant sheet that lies flat on the ground. If you want the sheet to be 1 meter off the ground (just like the rope), by how much would you have to increase the area of your sheet?
> 
> _Extra credit:_ What city, country, land mass or body of water has an area that is very close to your answer?

### Answer

**51.028π** (approximately **160.309**) additional square kilometers. 

### Derivation

Setting up the problem, we define:

- `A1` = surface area of the original globe
- `A2` = surface area of the sheet that's 1m off the ground
- `x` = difference between A1 and A2

We relate them:

![2](eq2.svg)

We also define `r1` to be the radius of the original globe, and `r2` to be the radius of the sheet. From the definition of the problem, we know:

![3](eq3.svg)

Next, recall the formula for the surface area of a sphere:  

![1](eq1.svg)

Since both the original globe and the sheet are a sphere (in this problem, anyway), we have

![4](eq4.svg)

In the problem, we're told to assume that the radius `r1` of the globe is `6378` km. Therefore, `r2` = `6379` km. Substituting those into the previous equations, we have:

![5](eq5.svg)

Then, we simplify algebraically and solve for `x`:

![6](eq6.svg)

![7](eq7.svg)

![8](eq8.svg)

![9](eq9.svg)

![10](eq10.svg)

Note that there are 1,000,000 square meters (not 1,000) in 1 square kilometer! Therefore, we have to divide by 1,000 to get the actual answer. 

### Commentary

But wait, why is the additional surface area required for the sheet (`51028π`) so much larger than the additional length required for the rope (`2π`)?

The derivation for the sheet above shows that the additional surface area required is proportional to the difference of the **square** of the globe and sheet's radii. 

![11](eq11.svg)

However, the additional length required for the rope **doesn't depend on the radius at all**. To see why, recall the formula for the circumference of a circle (i.e. the length of the rope on the ground):

![12](eq12.svg)

To find the length of the rope 1m above the surface, we can use a similar setup as we did above for the sheet example:

![13](eq13.svg)

![14](eq14.svg)

Substituting and solving for x, we get:

![15](eq15.svg)

![16](eq16.svg)

![17](eq17.svg)

The idea of a quantity growing rapidly as dimensions are added is common across a number of mathematical disciplines. For more on this idea, see [on the curse of dimensionality](https://towardsdatascience.com/on-the-curse-of-dimensionality-b91a3a51268).
 
