## Prime Attributes
Attributes which are part of at least one candidate key.
## Non Prime Attributes
Remaining attributes which are not prime attributes. 

**Example:**
Lets consider relation R(A,B,C,D,E) and functional dependencies {AB->C, C->D,B->E, E->B}

AB is a super key because attribute closure of AB is set of all attributes of relation R.
AB is a candidate key as well because the subsets A and B are not candidate keys themselves not even super keys themselves.

Now prime attributes of Relation R with this current given configuration are {A,B}. 
Non prime attributes are {C,D,E}

### Property of Prime Attribute
If X is a set of attributes which can determine a prime attribute then **we may have** more candidate keys.

From the previous example,
B is a prime attribute and E functionally determines B then we may have a possibility  that AE can also be a candidate key.

Attribute Closure of AE : {A,E,B,C,D} which is all attributes in R, hence AE is super key.

Subsets of AE are A and E.
Attribute closure of A = {A} which is not even a super key, so it is not a candidate key
Attribute closure of E = {E, B} which is not even a super key, so it is not a candidate key

As there is no subset of AE which is a candidate key, so AE  is also candidate key.