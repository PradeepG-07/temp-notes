## Normalization
Normalization is a structured way to decompose a table and ensure the objectives minimize/remove redundancy, lossless decomposition, dependency preserving and reduce anomalies.

There are multiple normalizations called as 1NF, 2NF, 3NF, BCNF, 4NF.
One thing to note here is, every higher normal form is in lower normal form.

## 1NF
- A table is in First Normal Form (1NF) if every attribute contains only atomic (indivisible) values. In other words, a single column in a row should not store multiple values.
- If a column contains multiple values for a row, convert the table to 1NF by creating separate rows so that each row contains exactly one value for that attribute.

### Example - 1
Let's consider a relation R(cid, purchased) with the following rows:

| cid | purchased |
|-----|-----------|
| 1   | 2,3       |
| 2   | 1,3       |

If we observe this table `cid` is a candidate key and `purchased` is a multivalued attribute.

We can convert this table into 1NF as below by creating separate rows

**1NF**

| cid | purchased |
|-----|-----------|
| 1   | 2         |
| 1   | 3         |
| 2   | 1         |
| 2   | 3         |

> If we observe carefully, the `cid` is no longer a candidate key, and introduced more redundancy. And a new candidate key should be `cid+purchased`.

### Example - 2
Let's consider a relation R(A,B,C,D) where D is a multivalued attribute and FD set = {A->B, B->C}

D is not part of FD set, so it should be there in every candidate key.

Let's try to find out the candidate key
$$AD^+ = {A,D,B,C}$$
As `AD` closure contains all attributes of R, and no other subset of AD is a super key. Hence, AD is a candidate key.

In the new relation R1(A,B,C,D), I would create separate rows for each value of D and declare my candidate key as AD.

### Problems in 1NF
- Introduces more redundancy

<hr />

### Partial Dependencies
If there is an FD where a **proper/strict subset of a candidate key** determines a non prime or non-key attribute, then the FD is called Partial Dependency.

Example: Consider a set {A,B} then strict subsets are {}, {A} and {B} but not {A,B}. 

Consider a relation R(cid,cname,pid) and the rows of the table are

| cid | cname | pid |
|-----|-------|-----|
| c1  | cn1   | p1  |
| c1  | cn1   | p2  |
| c2  | cn2   | p2  |
| c2  | cn2   | p3  |

Here, FD set = { cid->cname } and the candidate keys = {{cid,pid}}.

From above, the prime attributes are {cid,pid}. If we observe the FD cid->cname, `cid` is a proper subset of candidate key {cid,pid} which is determining a non-prime attribute `cname`.

> A form of redundancy occurs if there are partial dependencies

## 2NF
A relation R is said to be in 2NF if and only if
1. R is in 1NF - All attributes are atomic
2. R has no partial dependencies.

### Example
Let's consider the same example used in partial dependencies.

If we see, the relation R(cid, cname, pid) is in 1NF but not in 2NF. So, we try to decompose it into R1(cid, cname) and R2(cid, pid).

R1(cid, cname), FD set (F1) = {cid->cname}, candidate keys (CK1) = {{cid}}

R2(cid, pid), FD set (F2) = {}, candidate keys (CK2) = {{cid,pid}}

**R1**

| cid | cname |
|-----|-------|
| c1  | cn1   |
| c2  | cn2   |

**R2**

| cid | pid |
|-----|-----|
| c1  | p1  |
| c1  | p2  |
| c2  | p2  |
| c2  | p3  |

Now let's verify the objectives of normalization and 2NF rules.

#### 1. 2NF rule verification
2NF rule for R1:

For FD cid->cname in F1, `cid` is a candidate key and determines the non-prime attribute `cname`. But, `cid` is not a strict subset of candidate key, hence this FD is not a partial dependency.

2NF rule for R2: 

There are no FDs in the FD set F2. Hence, there are no partial dependencies.

#### 2. LossLess Decomposition Verification
Natural Join R1 and R2 and check we get R.

$$ R3 = R1 \bowtie R2 $$

| cid | cname | pid |
|-----|-------|-----|
| c1  | cn1   | p1  |
| c1  | cn1   | p2  |
| c2  | cn2   | p2  |
| c2  | cn2   | p3  |

Therefore, R3 = R and hence the decomposition is Lossless decomposition.

#### 3. Dependency preserving Verification
As per the process of Dependency preserving Verification, we need to enumerate all the non-trivial FDs of each sub relation and check if all FDs of R can be determined using the FDs union of all sub relations.

But here, the only FD in R is already present in R1. Hence, there is no need to generate all non-trivial FDs as it already satisfies the rules. So the decomposition is dependency preserving.

Now, the relations R1 and R2 are in 2NF.

## 3NF
A relation R is in 3NF if and only if:
1. R is in 2NF and
2. There are multiple definitions for 3NF. They are: A
   1. All non-prime attributes must depend on a key.
   2. (or) For all non-trivial FDs X->Y, X should be a super key or Y should be a prime attribute. 
   3. (or) No non-prime attribute is transitively determined by a key 
   4. (or) No non-prime attribute depends on non-prime attribute.
3. **Mostly we prefer definition-2** which is easy to deal with when decomposing tables.

## BCNF
Boyce-codd normal form is stricter version of 3NF.
A relation R is in BCNF if and only if:
1. R is in 3NF
2. For all non-trivial functional dependency X->Y, **X must be a superkey**. 
>The above rule alone tells that a relation in BCNF will always be in 3NF. So real world examples of 3NF & not in BCNF are very rare.

### Acheivability of BCNF
It is not possible to get BCNF always. Below is an example from a research paper from Beeri & Bernstein, where they stated that **lossless join, BCNF, and dependency preserving** all three are not always possible.

**Example:** Let's consider a relation R(A,B,C) with FD set = {AB->C, C->B}.

Whatever the decomposition is made one of the **lossless join, BCNF, and dependency preserving** objectives will be violated. 

## Steps to perform normalization
1. We can try the normal approach by converting the relation into 2NF, then 3NF and then BCNF.
2. Instead, we can directly convert the relation into BCNF, and it always satisfies the below normal forms. We just need to check if `lossless decomposition` objectives.
> Always try to create sub relations from the FDs only, so that we need not verify the **dependency preserving**.

## Properties
1. If there is an FD X->Y, such that Y is a CK then X is also a CK. 
2. If every CK is a simple key then the relation R with the FD set F is always in 2NF. 
3. If every attribute is a prime attribute then the relation R is always in 3NF.
4. If R is in 3NF and all CKs are simple keys then relation R along with FD set F is in BCNF.
   1. But how? If R is in 3NF, then in each FD X->Y, X is either SK or Y is a prime attribute. 
   2. If X was a super key in an FD then no problem because BCNF also states that X should be an SK.
   3. If Y was a prime attribute in an FD then, Y must be a key, if Y is a key then, X must be a key as well (from property 1).
5. A relation with only two attributes then it is in BCNF.

## Multi Valued Dependencies
Before going to 4NF, let's see an example where a lecturer can teach multiple courses using multiple books.

R=(lecturer, courses, books)

| lecturer (l) | courses (c) | books (b) |
|--------------|-------------|-----------|
| l1           | c1/c2       | b1/b2     |
| l2           | c2          | b2        |

This table is not in 1NF. So we decompose it, by separating the rows.

R=(l, c, d)

| l  | c  | b  |
|----|----|----|
| l1 | c1 | b1 |
| l1 | c1 | b2 |
| l1 | c2 | b1 |
| l1 | c2 | b2 |
| l2 | c2 | b2 |

If we find the candidate key CK = {{lcb}} and there are no FDs. 
>Hence, the relation R is in BCNF. But there is a lot of redundancy here. 

To solve this, the concept of functional dependencies is not adequate so a new concept called **multivalued dependencies** are introduced.

It is represented as $$X \twoheadrightarrow Y$$ Read as X multi determines Y.

The definition of MVD is difficult to explain so we remember it with an example. Let us consider the following relation

R = (X,Y,Z)

| X  | Y  | Z  |
|----|----|----|
| X1 | Y1 | Z1 |
| X1 | Y1 | Z2 |
| X1 | Y2 | Z1 |
| X1 | Y2 | Z2 |

**X multi determines Y iff:**
1. Attributes of Z = (attributes of R - (attributes of X U attributes of Y))
2. t1.x = t2.x = t3.x = t4.x 
3. t1.y = t2.y and t3.y = t4.y 
4. t1.z = t3.z and t2.z = t4.z

where t1, t2, t3, t4 are tuples of the relation R.

### Properties of MVD
1. If X multi determines Y then X multi determines Z if attributes of Z = (attributes of R - (attributes of X U attributes of Y)).
2. **Trivial MVD:** 
   1. X is super set of Y then X multi determines Y.
   2. X U Y = R, then X multi determines Y.
3. **Non-Trivial MVD**:
   1. We need atleast 3 attributes to be there in relation R for a non-trivial MVD to exist.
4. **Augmentation:** If X multi determines Y and Z is super set of W then XZ multi determines YW.
5. **Transitivity:** If X multi determines Y and Y multi determines Z then X multi determines (Z-Y). Here it is set subtraction.
6. **Replication:** If X determines Y then, X multi determines Y.
7. MVDs does not obey composition and decomposition principles.

## 4NF
A relation R is in 4NF iff:
1. R and FD set is in BCNF.
2. For all non-trivial dependencies X multi determines Y, X should be a super key. And this super key should be determined using functional dependencies not MVDs.