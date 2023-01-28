### Problem Statement
Given an array of **N** integers **arr[]** where each element represents the **maximum** length of the jump that can be made forward from that element. This means if arr[i] = x, then we can jump any distance y such that y ≤ x.  
Find the minimum number of jumps to reach the end of the array (starting from the first element). If an element is **0**, then you cannot move through that element.
**Problem Link** - https://practice.geeksforgeeks.org/problems/minimum-number-of-jumps-1587115620/1?page=1&category[]=Greedy&sortBy=submissions


### Solution

#### Algorithm
First, we will make arr[i] = min(arr[i] + i,N)
Now, the arr[i] represents the maximum index we can jump to from index i.

Our algorithm will be as follows:
If we are on index $i$, we will jump to index $j$ such that $arr[j] = max(arr[i+1], arr[i+2], ..., arr[arr[i]])$. If there are more than one maximum value, we will choose the one with maximum index. This will give us minimum number of jumps. 

#### Proof

We can prove this algorithm with greedy stays ahead technique.

Let's say $A$ is solution from our algorithm & $O$ is some other optimal solution.

Let $P_x$ is the index after $x$ jumps in $A$ & $F(w)$ denotes the value of $arr[w]$. We will prove $2$ properties for $P_x$.

1. $F(P_x) \leq F(P_{x+1})$
2. $F(P_x) \geq F(y)$ where $y \leq P_x$

First we make a observation that $F(w) \geq w$ since the minimum value in array before transformation was $0$ & we just added index to that element. 

$1st$ property:
We know that in the worst case (all array values are minimum possible) we can go to index $F(P_x)$ from $P_x$, so $P_{x+1} = F(P_x)$. From $F(w) \geq w$, $F(P_{x+1}) \geq F(P_x)$.

$2nd$ property:
From the $P_0 = 0$ index, we will go to $P_1 \leq F(P_0)$. Now, we already know that $F(P_0) \leq F(P_1)$ from $1st$ property. And we can also say that $F(P_1) \geq F(y)$ where $P_0 < y < P_1$ because it was not true for any such $y$, we would have picked that $y$ as $P_1$ according to our algorithm. Similary, for $P_2$, we can say that $F(P_1) \leq F(P_2)$ and $F(P_2) \geq F(y)$ where $P_1 < y < P_2$. And since $F(P_1) \geq F(y)$ where $y > P_0$, $F(P_2) \geq F(y)$ where $y > P_0$. We can go on and prove this property for all $P_x$ with induction.

**Now let's come to the algorithm proof**

$A$ is solution from our algorithm & $O$ is some other optimal solution.

Let $Q_x$ is the index after $x$ jumps in $O$. Let's assume that $F(P_x) \geq F(Q_x)   \forall x \in (0, Ans_O)$ where $Ans_O$ is number of jumps produced in $O$, we will prove it later but let's prove that $A$ is optimal solution using this assumption.

We know that $F(P_{Ans_O - 1}) \geq F(Q_{Ans_O - 1})$. So, if we can go to last index from $Q_{Ans_O - 1}$, we can definitely go to last index from $P_{Ans_O-1}$ since $F(w)$ represents what is last index we can go to in one jump from index $w$.

Hence, we have proved that if $F(P_x) \geq F(Q_x)$ at every step, we can achieve the same answer produced by any other optimal solution.

Now, we just need to prove that $F(P_x) \geq F(Q_x)$ after any $x$ jumps before reaching to last index. We can prove it by induction.

**Base Case, x=0**
We know that $P_0 =0$ and $Q_0 = 0$, hence $F(P_0) = F(Q_0)$.

**Assumption, it is true for x=k**
Let's assume that $F(P_k) \geq F(Q_k)$

**Induction step, x=k+1**

Case 1: $P_k \leq Q_k$
In this case we know that $F(P_k) \geq F(Q_k)$, so the full range is covered by $F(P_k)$ whatever is covered by $F(Q_k)$. So whatever index $O$ chooses, we can choose that & in fact we will choose the one with maximum $F$. So, $F(P_{k+1}) \geq F(Q_{k+1})$.


Case 2: $P_k > Q_k$
Now, for this case, if from $Q_k$, we jump to some index less than $P_k$, we can use the property 1.
If we jump to some index greater than $P_k$, range is covered by $F(P_k)$.

