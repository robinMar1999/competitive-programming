
### Problem Statement
[Problem Link](https://cses.fi/problemset/task/1632)
In a movie festival, $n$ movies will be shown. Syrjälä's movie club consists of $k$ members, who will be all attending the festival.  
  
You know the starting and ending time of each movie. What is the maximum total number of movies the club members can watch entirely if they act optimally?


### Solution

#### Algorithm
We can sort the movies in ascending order of ending time. Now we will iterate over the movies and for each movie we will try to find the club member who has largest ending time of previous movie but still lesser than or equal to current movie starting time.

#### Proof

We will prove this with exchange arguments technique.

Let's say we have some optimal solution $O*$ and our solution $O$. We put the movies in optimal solution in some sequence (optimal sequence) $S_1', S_2', ..., S_x'$. So there are $x$ movies that we can watch if we go with optimal solution. Let (algorithm sequence) $S_1, S_2, ..., S_y$ be sequence with our algorithm where we can watch $y$ movies with our algorithm. Here $S_i$ means that we assign some movie to some member, we can assume that we assign movies in watching order for each member in optimal solution since it doesn't matter, so if $S_i$ & $S_j$ are both watched by some member than ending time of $ith$ movie is less than or equal to $jth$ movie if $i \leq j$.

Now, let's say that optimal sequence & algorithm sequence differ at some lowest index $i$. We will prove that if we instead put a movie according to our sequence in optimal sequence we will get a better or equal answer.

There are 2 cases that the sequences differ at some lowest index $i$.

**Case 1**
We do not assign the current movie to the member with largest previous ending time who is still available. Instead we assign this movie to some different member (maybe at later point in time, it doesn't matter).  Now we can exchange the movie sequence from this movie forward for this member with the member with largest previous ending time. Essentially, if there are two members $Member_x$ & $Member_y$, with sequence as follows: 
   $Member_x = M_{x,1}, M_{x,2}, ..., M_{x,a}, M_{x,a+1}, ..., M_{x,m}$ $Member_y = M_{y,1}, M_{y,2}, ..., M_{y,b}, M_{y,b+1}, ..., M_{y,n}$ 
   
   Here, let's say $M_{x,a}$ is the movie that was assigned wrongly & it should have come in place of $M_{y,b}$ according to our algorithm. So we just exchange the sequences $M_{x,a}, ..., M_{x,m}$ & $M_{y,b}, ..., M_{y,n}$ with each other since we are sure that $M_{x,a}$ starting time is greater than ending time of $M_{y,b-1}$ (assumed according to our algorithm) so we can be sure that we can put $M_{y,b}$ after $M_{x,a-1}$ because we are sure that ending time of $M_{y,b-1}$ is greater that ending time of $M_{x,a-1}$ and starting time $M_{y,b}$ must be greater than $M_{x,a-1}$.
   After, this exchange of sequences we can see that the number of movies are same as was before in optimal solution. In fact, now there is a possibility that we can add more movies before $M_{y,b}$ in $Member_x$ since there is more or equal gap now between $M_{x,a-1}$ & $M_{y,b}$.
   Also note that after doing the exchange we can still treat the solution as produced by optimal solution since we do not care about which member holds which sequence and optimal solution can assign movies from this point by assuming that $Member_x$ & $Member_y$ are exchanged. Nothing is changed in the eyes of optimal solution as we can assume that it just sees the last movies & not care what member it assigned it to.
   
**Case 2**
We do not assign the current movie to any member. Let's say the current movie should have been assigned to $Member_x$ according to our algorithm. Instead, we assigned a movie $M_{x,a}$ in place of that. Now, we can easily replace the current movie with $M_{x,a}$ as we can be sure that current movie must have a ending time less than or equal to $M_{x,a}$, so the sequence $M_{x,a+1},...$ won't be disturbed and we already assumed that we can put this movie after $M_{x,a-1}$ according to our algorithm. So, we have achieved the same number of movies after this exchange. In fact, we have created a higher gap between current movie & $M_{x,a+1}$ so there are chances that we can insert some more movies there.

In conclusion, in both cases, we are able to achieve equal or better answer if we do a exchange at lowest index $i$ where the sequences differ. We can do this repetitively until we make both the sequences equal without reducing the answer. Hence, our algorithm must produce one of the optimal solutions.



