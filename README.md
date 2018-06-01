### Riddler Classic
From David Seal, a classic construction problem perfect for Infrastructure Week:

Consider four towns arranged to form the corners of a square, where each side is 10 miles long. You own a road-building company. The state has offered you $28 million to construct a road system linking all four towns in some way, and it costs you $1 million to build one mile of road. Can you turn a profit if you take the job?

_Extra credit_: How does your business calculus change if there were five towns arranged as a pentagon? Six as a hexagon? Etc.?

## Answer:

Ollie clarified on Twitter that we should think of the towns as points at the corners.

By a kind of heuristic / guessing / iterative reduction method, I found that the shortest graph I could come up with that connects those four points looked like this:

![Points](/2018-06-01/points2.png)

Assuming the four outlying segments have the same length, that length is a function of the length of the middle segment.

I tried to come up with `y = f(x)`, where `f(x)` describes the length of all 5 segments.

- `y = f(x)` => total length of all segments
- `x` => length of center segment
- `n` => length of outlying segment

![Points](/2018-06-01/points3.png)

From first principles, the length of all sides is 4 * the length of an outlying segment + the length of the center segment:

=> `y = 4n + x`

To determine `n`, I tried some example values:

`x` | `n`
--- | ---
`0` | `sqrt(5^2 + 5^2)`
`1` | `sqrt(5^2 + (4.5^2)`
`2` | `sqrt(5^2 + (4^2)`

From this it became clear that 

=> `n = sqrt(5^2 + (5 - x/2)^2)`

Plugging that into the original equation for `y`, we have

![Points](/2018-06-01/equation1.png)

Graphing that, courtesy of https://www.desmos.com/calculator:

![Points](/2018-06-01/graph1.png)

The way to interpret this is that there is a minimum in cost at around `x ~ 4`. To find that, we calculate the derivative:

