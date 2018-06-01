### Riddler Classic 6/1/2018
From David Seal, a classic construction problem perfect for Infrastructure Week:

Consider four towns arranged to form the corners of a square, where each side is 10 miles long. You own a road-building company. The state has offered you $28 million to construct a road system linking all four towns in some way, and it costs you $1 million to build one mile of road. Can you turn a profit if you take the job?

_Extra credit_: How does your business calculus change if there were five towns arranged as a pentagon? Six as a hexagon? Etc.?

### Answer:

Ollie clarified on Twitter that we should think of the towns as points at the corners of a square:

![Points](/2018-06-01/points5.PNG)

_NOTE: The gridlines represent miles; each side has length 10:_

![Points](/2018-06-01/points4.PNG)

By a kind of heuristic / guessing / iterative reduction method, I found that the shortest graph I could come up with that connects those four points looked like this:

![Points](/2018-06-01/points2.PNG)



Assuming the four outlying segments have the same length, that length is a function of the length of the center segment.

I tried to come up with `y = f(x)`, where `f(x)` describes the length of all 5 segments.

- `y = f(x)` => total length of all segments
- `x` => length of center segment
- `n` => length of outlying segment

![Points](/2018-06-01/points3.PNG)

From first principles, the length of all sides is 4 * the length of an outlying segment + the length of the center segment:

=> `y = 4n + x`

To determine `n`, I tried some example values:

`x` | `n`
--- | ---
`0` | `sqrt(5^2 + 5^2)`
`1` | `sqrt(5^2 + (4.5^2)`
`2` | `sqrt(5^2 + (4^2)`

From this it became clear that 

![Equation](/2018-06-01/equation3.PNG)

Plugging that into the original equation for `y`, we have

![Equation](/2018-06-01/equation1.PNG)

Graphing that, courtesy of https://www.desmos.com/calculator:

![Graph](/2018-06-01/graph1.png)

The way to interpret this is that there is a minimum in cost at around `x ~ 4`. To find that mininum, we calculate the derivative of `y`, `y'`:

![Equation](/2018-06-01/equation2.PNG)

Setting that equal to `0` and solving (and graphing) shows that the minimum occurs at `x = 4.226`:

![Graph](/2018-06-01/graph2.png)

Plugging `x = 4.226` into the original equation for `y` shows that the minimum cost is `y ~ 27.321`:

![Graph](/2018-06-01/graph3.png)

Therefore, since the budget is $28m, we can construct the road at a minimum cost of about $27.321m, which is profitable.
