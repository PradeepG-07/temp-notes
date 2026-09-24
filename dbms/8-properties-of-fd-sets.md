## Properties of Functional Dependency (FD) Set
### 1. Membership
Let F be an FD set on relation R, if X->Y using given FDs in F then X->Y is a member of F.

#### Example-1
Let R(A,B,C,D) be a relation with 4 attributes and the following FD set {AB->C, BC->D}. Is A->C a member of FD set ?

$$A^+ = \{A\}$$

We can see the attribute closure doesnot have C and hence C cannot be determined by A. Hence A->C is not a member of FD set.

#### Example-2
Let R(A,B,C,D,E) be a relation with 5 attributes and the following FD set {AB->C, A->D, CD->E}. Is AC->E a member of FD set ?

$$(AC)^+ = \{A, C, D, E\}$$

We can see the attribute closure have E and hence E can be determined by A. Hence AC->E is a member of FD set.

### 2. Closure of an FD Set
Set of all functional dependencies that can be determined using FDs in F.

#### Example-1
Let R(A,B,C) be a relation with 3 attributes and the following FD set {A->B, B->C}. How many FDs can be determined or simply size of the closure of FD set ?

To get the count of FDs that can be determined using given FD set.

Let's enumerate and count them.

**0-attributes on LHS**
$$\emptyset -> \emptyset => Subsets = 1$$

**1-attributes on LHS**
$$
\begin{aligned}
A^+ = \{A, B, C\} => Subsets = 2^3 = 8 \\
B^+ = \{B, C\} => Subsets = 2^2 = 4 \\
C^+ = \{C\} => Subsets = 2^1 = 2 \\
\end{aligned}
$$

**2-attributes on LHS**
$$
\begin{aligned}
(AB)^+ = \{A, B, C\} => Subsets = 2^3 = 8 \\
(BC)^+ = \{B, C\} => Subsets = 2^2 = 4 \\
(AC)^+ = \{C, A, B\} => Subsets = 2^3 = 8 \\
\end{aligned}
$$

**3-attributes on LHS**
$$
\begin{aligned}
(ABC)^+ = \{A, B, C\} => Subsets = 2^3 = 8 \\
\end{aligned}
$$

Total count = 1+8+4+2+8+4+8+8 = 43

**Here we determined 41 more FDs using 2 given FDs and these 41 FDs are redundant FDs.**

### 3. Equality of FD sets
Two FD sets F and G are equal if and only if
1. Closure of FD sets F and G are equal. (But this is very tedious task to identify F closure and G closure as we have seen above.)
2. Or F covers G and G covers F

**Note:** A covers B meaning, all given FDs in B can be determined using given FDs of A.

#### Example - 1
Let's consider a relation R(A,B,C,D) and FDs F = {AB->CD, B->C, C->D}, G = {AB->C, AB->D, C->D}. Find whether F = G.

**F Covers G**

Given F = {AB->CD, B->C, C->D}. Let's try to derive FDs of G.

$$(AB)^+ = \{ A, B, C, D\}$$
$$C^+ = \{D\}$$
So we can say AB->C, AB->D and C->D are FDs in G which can be derived from F. Hence we can say that **F covers G**.

**G Covers F**
Given G = {AB->C, AB->D, C->D}. Let's try to derive FDs of F.

$$(AB)^+ = \{ A, B, C, D\}$$
$$B^+ = \{B\}$$
$$C^+ = \{D\}$$
From above we can see only AB->CD and C->D are the only FDs that can be determined. Hence, **G doesn't cover F**.

Therefore, F is not equal to G.

### 4. Minimal/Canonical Covers
Given a set of FDs F, can I eliminate the redundant FDs in F.

Redundant FDs are those FDs whose removal does not impact closure of FD set F.

#### Why minimize the FDs ?
In relational database like SQL these FDs are implemented using CHECKS, or Primary Keys. As the FDs reduce there will be fewer checks to perform and hence the DB runs faster.

#### Extraneous Attributes
Removal some attributes from an FD which does not change the expressive power of the FD set.

Let's consider a relation R(A,B,C) with functional dependencies F = {A->C, AB->C}

Here A can determine C alone with A->C. But there exist another FD AB->C, where B is extraneous. If remove it we will get F = {A->C} which is a **Minimal Cover**.

So removal of such all extraneous attributes is a way to obtain a minimal cover of a functional dependency set.

#### Steps to find minimal cover
1. Find FDs in FD set which have same LHS and union the RHS
2. Find FDs that have extraneous attributes in LHS or RHS, resulted from step-1.
3. Remove any other extraneous attributes in the individual FDs from step-2 result. 
4. Remove the redundant FDs which can be derived using inference rules.
5. Repeat steps-1,2,3 until the minimal cover appears.
6. Verify if the resultant FD set is equal to the initial FD set.