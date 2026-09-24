### Functional Dependency
Functional dependency is a constraint between two sets of attributes in a relation.
i.e. $$X->Y$$
> **Meaning**: For any two tuples in the relation, if they have the same values for all attributes in XXX, then they must also have the same values for all attributes in YYY.

In other words, XXX **functionally determines** YYY.

Let's consider a relation $$R(A1,A2,....,An)$$ and the dependency $${A1,A2}->{A3,A4}$$
**Set Theory**
![[functional-dependency.png]]

**Logical Statement**
Consider a tuple T and attributes X and Y, then X functionally determines Y if and only if for any tuple t1 and t2 if t1.x == t2.x then must and should be t1.y == t2.y
$$X->Y, \quad iff \quad (t1.x = t2.x => (t1.y = t2.y)) $$
#### Types of Functional Dependency
#### 1. Trivial Functional Dependency
In a relation `R` if X functionally determines Y and X is super set of Y then it is called trivial functional dependency.
$$R: \quad X->Y, \quad X \supseteq Y$$
##### 2. Non Trivial Functional Dependency
In a relation `R` if X functionally determines Y and X is not a super set of Y then it is called non trivial functional dependency.
$$R: \quad X->Y, \quad X \not\supseteq Y$$