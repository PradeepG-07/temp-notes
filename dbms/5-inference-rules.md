Based on the system or properties of the system that we are designing we will get some base functional dependencies.

Rules to obtain new functional dependencies from existing ones are called **Inference rules**.

#### 1. Reflexivity
If X is super set of Y then X functionally determines Y. It is same as Trivial Functional Dependency.
$$if \quad X \supseteq Y \quad then \quad X->Y $$
#### 2. Associativity
If X functionally determines Y and Y functionally determines Z then X also functionally determines Z.
$$if \quad X->Y, \quad Y->Z \quad then \quad X->Z$$
#### 3. Augmentation
If X functionally determines Y then XZ also functionally determines YZ.
$$if \quad X->Y, \quad then \quad XZ->YZ$$
#### 4. Decomposition
If X functionally determines YZ then X functionally determines Y and X functionally determines Z.
$$if \quad X->YZ, \quad then \quad X->Y \quad and \quad X->Z$$
#### 5. Union
If X functionally determines Y and X functionally determines Z then X functionally determines Y and Z.
$$if \quad X->Y \quad and \quad X->Z, \quad then \quad X->YZ$$
#### 6. Composition
If X functionally determines Y and W functionally determines Z then XW functionally determines YZ.
$$if \quad X->Y \quad and \quad W->Z, \quad then \quad XW->YZ$$
#### 7. Pseudo Transitivity
If X functionally determines Y and WY functionally determines Z then WX functionally determines Z.
$$if \quad X->Y \quad and \quad WY->Z, \quad then \quad WX->Y$$
Explanation:
$$
\begin{matrix}
X->Y => eq-1 \\ 
WY->Z => eq-2 \\
\\
from \quad augumentation \quad eq-1 \quad becomes \\ 
WX -> WY => eq-3\\
\\
from \quad associativity \quad eq-3 \quad and \quad eq-2 \quad becomes \\
WX -> Z
\end{matrix}
$$
