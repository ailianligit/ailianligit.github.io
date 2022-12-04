# Uncertain Knowledge & Reasoning

## Conditional Independence

- Property: $Pr(B\cap C|A)=Pr(B|A)\cdot Pr(C|A)$
- Variable: $Pr(X=x\and Y=y|Z=z)=Pr(X=x|Z=z)\cdot Pr(Y=y|Z=z)$



## Bayesian Networks

- A BN is a graphical representation of the **direct dependencies** over a set of variables, together with a set of **conditional probability** tables (CPTs) quantifying the strength of those influences
- A BN over variables ${X_1, X_2, \dots , X_n}$ consists of a DAG (directed acyclic graph) whose nodes are the variables & a set of CPTs (conditional probability tables) $Pr(X_i|Par(X_i))$ for each $X_i$