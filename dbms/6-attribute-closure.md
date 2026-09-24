## Attribute Closure
Attribute closure is set of attributes that can be functionally determined using X.

Consider a relation `R` and `X` is a set of attributes.
Attribute closure of `X` is represented as $$X^+$$
**Example**
1. Consider a relation `R` with four attributes `A,B,C,D` and the functional dependencies `{A->B, B->C, C->D}`
$$
\begin{aligned}
A^+ = {A,B,C,D} \\
B^+ = {B,C,D} \\
C^+ = {C,D} \\
D^+ = {D}
\end{aligned}
$$

2. Consider a relation `R` with attributes `A,B,C,D,E,F,G` and the functional dependencies  $$
	\begin{matrix} 
	\{AB->CD, \quad AF->D, \quad DE->F, \quad C->G, \quad F->E, \quad G->A\}
	\end{matrix} 
   $$
$$
\begin{aligned}
CF^+ = {C,F,G,E,A,D,F} \\
BG^+ = {B,G,A,C,D} \\
AF^+ = {A,F,D,E} \\
\end{aligned}
$$
### Application
Biggest application of closure is to determine **super keys** and **candidate keys**
#### How ?
Lets consider a relation R of `n` attributes.
$$R: (a_1,a_2,..,a_n) $$
X is said as a super key if and only if attribute closure of X is relation R. 
As per definition of super key X is a set of attributes which can uniquely determine a tuple(i.e. all column values)
$$if \quad X^+: (a_1,a_2,..,a_n) \quad then \quad X = super key$$
Also X is said to be candidate key, if X is super key and there is no other subset of X which is a candidate key.