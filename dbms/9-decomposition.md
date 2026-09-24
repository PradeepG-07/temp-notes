## Decomposition
Breaking down a relation R into sub relations R1, R2, ... ,Rn. Objectives of decomposition are
1. No redundancy - Don't store duplicate data.
2. Lossless decomposition - Don't miss any data while decomposing.
3. Dependency preserving - Preserve all the FDs even after decomposition.

## Anomalies
Consider a relation entire_relation = (cid, pd_id, cname, pd_name, time).
cid = customer_id, pd_id = product_id, cname = customer_name, pd_name = product_name, time = updated time of row

**Table: entire_relation**

| cid | pd_id | cname   | pd_name  | time |
|-----|-------|---------|----------|------|
| 1   | 1     | pradeep | laptop   | 2:00 |
| 1   | 2     | pradeep | keyboard | 3:00 |
| 2   | 1     | someone | laptop   | 2:00 |
| 2   | 1     | someone | keyboard | 3:00 |

Here there are three anomalies
1. **Update Anomaly:** When there is update to a pd_name in the relation there are multiple rows needs to be updated which is **time-consuming**.
2. **Insertion Anomaly:** When we want to insert new customer without any purchases, the columns (pd_id, pd_name) **will have null values which will waste space.**
3. **Deletion Anomaly:** When we want to delete a purchase information from the relation, we have to remove pd_id, pd_name and add null over there. This also will **waste space.**

> So there is a need to decompose the table to avoid anomalies.

## Lossless Decomposition
1. When a relation R is decomposed into sub relations R1, R2 and R3, this decomposition is lossless if R1 natural join R2 natural join R3 equals to R.
    $$(R1 \bowtie R2 \bowtie R3) = R$$
2. It is also called as Lossless Join Decomposition.

### Algorithm to determine for lossy/lossless decomposition
A relation R with FD set F decomposed into relations R1, R2, R3,....,Rn is lossless if:
1. Union of attributes of R1,R2,....,Rn = All attributes of R.
2. We can merge Ri and Rj relations only if
    1. Ri intersection Rj not equal to null set. Meaning, there should be at least one common attribute between Ri and Rj.
    2. The common attribute between Ri and Rj should be super key in either Ri or Rj.
3. If we keep merging the relations and finally end up at R then it is lossless decomposition.

### Example - 1
Let's consider a table R(A,B,C).

| A | B | C |
|---|---|---|
| 1 | 2 | 1 |
| 2 | 2 | 2 |
| 3 | 1 | 2 |

Let's try to decompose R into R1(A,B) and R2(B,C) then we have the tables R1 and R2 as follows

**R1(A,B)**

| A | B | 
|---|---|
| 1 | 2 | 
| 2 | 2 | 
| 3 | 1 | 

**R2(B,C)**

| B | C | 
|---|---|
| 2 | 1 | 
| 2 | 2 | 
| 1 | 2 | 

Now, we will check the natural join between to R1 and R2. Meaning join the tables using the common column. We will get the result as 
Result(A,B,C)

**Result(A,B,C)**

| A | B | C |
|---|---|---|
| 1 | 2 | 1 |
| 1 | 2 | 2 |
| 2 | 2 | 1 |
| 2 | 2 | 2 |
| 3 | 1 | 2 |

If we observe, the Result is not equal to original R. Hence this decomposition is lossy.

> The observation is when the common column is not a super key in either of the tables where join is being performed, then it will result in a lossy decomposition.

Now let's try to decompose R into R1(A,B) and R2(A,C)
**R1(A,B)**

| A | B | 
|---|---|
| 1 | 2 | 
| 2 | 2 | 
| 3 | 1 | 

**R2(B,C)**

| A | C | 
|---|---|
| 1 | 1 | 
| 2 | 2 | 
| 3 | 2 | 

Now, we will check the natural join between to R1 and R2. Meaning join the tables using the common column. We will get the result as
Result(A,B,C)

**Result(A,B,C)**

| A | B | C |
|---|---|---|
| 1 | 2 | 1 |
| 2 | 2 | 2 |
| 3 | 1 | 2 |

If we observe, the Result is equal to original R. Hence this decomposition is lossless.

Also, if we see A is super key in R1 and also in R2.

R1(A,B), F1 = {A->B}, A closure = {A,B} = R1, so A is super key in R1.
R2(A,C), F2 = {A->C}, A closure = {A,C} = R2, so A is super key in R2.

> **Note:** If the common attribute in decomposition is a super key of either of the resultant relations where join is being performed then it is lossless decomposition.

## Dependency Preserving Decomposition
Let's consider a relation R and FD set F. If we decompose this relation into sub relations R1, R2,...,Rn and FD set into sub FD sets F1, F2,...,Fn and if F1 U F2 U ... U Fn = F then the decomposition is called Dependency Preserving Decomposition. **Here the equality meaning the equality of Functional Dependencies.**

To determine if a decomposition is dependency preserving we can follow the below steps:
1. Enumerate all non-trivial FDs using F for each sub relation namely F1,F2,....,Fn.
2. Determine if F1 U F2 U...U Fn = F

### Example - 1
Let's consider relation R(A,B,C,D) and FD set F = {A->B, B->C, C->D, D->A}.

Let's try to decompose R into R1(A,B), R2(B,C), R3(C,D).

**Step-1** 

Identify all non-trivial FDs using F for each sub relation i.e.

1. R1(A,B) , F1 = {A->B, B->A} 
2. R2(B,C) , F2 = {B->C, C->B} 
3. R3(C,D) , F3 = {C->D, D->C}

**Step-2** 

Determine F1 U F2 U F3 = F

Let F1 U F2 U F3 = F123 = {A->B, B->A, B->C, C->B, C->D, D->C}

Now find F123 = F i.e. check if F123 covers F and F covers F123

1. F123 covers F
   1. A->B: Yes it is a direct FD in F123.
   2. B->C: Yes it is a direct FD in F123.
   3. C->D: Yes it is a direct FD in F123. 
   4. D->A: From transitivity (D->C, C->B, B->A) from F123.
   5. Therefore, all FDs of F are covered by F123. Hence, F123 covers F.
2. F covers F123
    1. A->B: Yes it is a direct FD in F.
    2. B->A: From transitivity (B->C, C->D, D->A) from F. 
    3. B->C: Yes it is a direct FD in F.
    4. C->B: From transitivity (C->D, D->A, A->B) from F.
    5. C->D: Yes it is a direct FD in F.
    6. D->C: From transitivity (D->A, A->B, B->C) from F.
    7. Therefore, all FDs of F123 are covered by F. Hence, F covers F123.

Therefore, F123 = F and hence the decomposition is dependency preserving.