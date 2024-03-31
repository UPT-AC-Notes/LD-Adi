# Minimizing functions
## Minterms with 3 variables

- A ``minterm`` can be written as:
$m_{i}^{n}=\sum \left( 1,2,4,5\right) =m\left( 1,2,4,5\right) =m_{1}^{3}+m_{2}^{3}+m_{4}^{3}+m_{5}^{3}$

- In this example, ``n = the number of variables = 3``, and ``i`` is the decimal equivalent for each variable (``1,2,4,5``). We can now write the truth table for the above ``minterm`` notation. Because we have ``3`` variables, we'll use the letters ``A, B, C`` as inputs, and ``F`` for output:

| A   | B   | C   | F   |
| --- | --- | --- | --- |
| 0   | 0   | 0   | 0   |
| 0   | 0   | 1   | 1   |
| 0   | 1   | 0   | 1   |
| 0   | 1   | 1   | 0   |
| 1   | 0   | 0   | 1   |
| 1   | 0   | 1   | 1   |
| 1   | 1   | 0   | 0   |
| 1   | 1   | 1   | 0   |

- ``F`` is ``true`` only for the numbers ``1, 2, 4, 5``. Their binary equivalents are:
	- ``1 (B10)`` = ``1 (B2)``
	- ``2 (B10)`` = ``10 (B2)``
	- ``4 (B10)`` = ``100 (B2)``
	- ``5 (B10)`` = ``101 (B2)``

- For easy simplification of  ``boolean expressions``, we use **K-MAPS**  (Karnaugh Map)

![[Lab5Ex1a.png]]

- The places marked with ``1`` are the same places that the function is ``true`` at: ``(ABC) = { 001, 010, 100, 101 }``
- In Karnaugh Maps, we have to:
	- Group all ``ones (minterms)``
	- The number of terms in every group should be a power of two: ``2^n``
	- The shape of the groups should either be a square or a rectangle.
	- The 'header' values of the table should be adjacent in binary
	- For a good function optimization, groups should be as big as possible
- The K-MAP above generates the function ``F = AB' + B'C + A'BC' ``

- The same K-MAP can be drawn as:
![[Lab5Ex1b.png]]
- The K-MAP above generates the function ``F = A'BC' + AB' + CB'``
- !!! Notice that the K-MAP can wrap around its corners. This is because values there are adjacent.


## Minterms with 4 variables

![[Lab5Ex2.png]]

- To only select the needed groups we need to define the following:
	- **Implicant** = A ``minterm (a 1)`` which can be placed in the K-MAP
	- **Prime Implicant** = The biggest group that covers one or more ``minterms``
	- **Essential Prime Implicant** = Prime Implicant that doesn't appear (as a whole) in other implicants.
- To select the most optimized function, we only take **Essential Prime Implicants** into consideration, which in our case are: **RED, BLUE, PURPLE**
	- Yellow and Green are not essential

- That means the resulted function is ``F = A'B (blue) + BC (purple) + AC'D (red)``
	- We **minimized** the function as much as possible 

- The truth table for the function above is:

| A   | B   | C   | D   | F   |
| --- | --- | --- | --- | --- |
| 0   | 0   | 0   | 0   | 0   |
| 0   | 0   | 0   | 1   | 0   |
| 0   | 0   | 1   | 0   | 0   |
| 0   | 0   | 1   | 1   | 0   |
| 0   | 1   | 0   | 0   | 1   |
| 0   | 1   | 0   | 1   | 1   |
| 0   | 1   | 1   | 0   | 1   |
| 0   | 1   | 1   | 1   | 1   |
| 1   | 0   | 0   | 0   | 0   |
| 1   | 0   | 0   | 1   | 1   |
| 1   | 0   | 1   | 0   | 0   |
| 1   | 0   | 1   | 1   | 0   |
| 1   | 1   | 0   | 0   | 0   |
| 1   | 1   | 0   | 1   | 1   |
| 1   | 1   | 1   | 0   | 1   |
| 1   | 1   | 1   | 1   | 1   |

