
https://practice.geeksforgeeks.org/problems/majority-element-1587115620/1?page=1&category[]=Greedy&sortBy=submissions
# The Boyer-Moore algorithm

And now comes the magic. We will compute the majority element, if such an element exists, by simply using _2_ memory slots, or two lines on a single piece of paper!

We store two entries:

-   **_m_**, which represents the candidate “majority” element, and
-   **_count,_** which keeps track of whether we have an excess of appearances of some element. The variable _count_ will never drop below _0_.

We now make things more concrete. Suppose that we have a sequence

_x₁, x₂, …, xₙ_

of _n_ elements.

We start by setting _m := x₁_ and _count := 1_. We continue with the second element _x₂_. If _x₂ = m_, then we increase _count_ by _1_. If not, then we decrease _count_ by _1_, which means that it becomes _0_. In general, when processing the _i_-th element _xᵢ_, for _i > 1_, we do the following:

1.  If _count = 0_, then we set _m := xᵢ_ and _count :=1._
2.  If _count > 0_, we further do the following check. If _xᵢ = m_, then we increase _count_ by _1_, otherwise we decrease _count_ by _1_.

We are essentially done! In case we are guaranteed to have a majority element in the sequence, we simply return _m_. If there is no such guarantee, then _m_ is the only candidate majority element, and we have to do a second pass in order to check whether _m_ is indeed a majority element (in fact, this second pass is only needed if the final value of _count_ is strictly positive, as will become clear in the next section). This can be done very easily, by simply counting the occurrences of _m_.

The whole algorithm runs in _O(n)_ time and uses _O(1)_ extra space, or more precisely, _O(1)_ extra memory slots.

# Proof of correctness

The main idea behind the algorithm is the following. At a high level, we “pair” elements that are not equal, from left to right, and we cross them out in pairs. After an iteration, we have a value _m_ and a number _count_; this means that we have _count_ many appearances of the element _m_ that have not been crossed out yet. Then, if at the next iteration we encounter some element different than _m_, we pair it with the first (leftmost) appearance of the element _m_ that has not yet been crossed out, and we cross both elements out. This is the reason we decrease the variable _count_ by _1_ in this case. At the end, we can be certain that if there is a majority element, then since it appears more than _n/2_ times, there are not enough elements to cross out all of its occurrences, and thus it must be the element stored at variable _m_.

We now make things more formal. We use this notion of pairs that we used in the previous paragraph, and we view the algorithm as an algorithm that creates pairs of elements that are not equal. We will show, by using mathematical induction, that for every _i_,

> After iteration _i_, the only element that has not been paired yet is the one stored at variable “_m”_, and moreover, we have exactly “_count”_ many unpaired copies of that element.

We are ready to do the induction (in case you need to refresh some things about mathematical induction, you can have a quick look at a previous article of mine [here](https://medium.com/cantors-paradise/mathematical-induction-the-domino-effect-in-natural-numbers-61e6754b40f3)).

-   _i = 1_ (base case). After processing the first number, we have that _m = x₁_ and _count = 1._ It is clear that we have exactly _1_ unpaired copy of the element stored at variable _m._
-   from _i-1_ to _i_ (induction step). Let _i ≥ 2_, and suppose that the induction hypothesis holds for all iterations up to _i-1._ This means that after iteration _i-1_, the only element that has not been paired yet is the one stored at variable _m_, and moreover, we have exactly _count_ many unpaired copies of that element. The algorithm now processes element _xᵢ._ We do some case analysis. If _count = 0,_ then this means that all elements before _xᵢ_ have been paired, and thus the only unpaired element after iteration _i_ is _xᵢ._ Since the algorithm sets _m = xᵢ_ and _count = 1_ in this case, then the statement indeed holds after iteration _i._ So, suppose that _count > 0_ at the beginning of iteration _i._ In this case, if _m_ is equal to _xᵢ,_ then this means that we have one extra copy of the element stored in _m_ that is unpaired. Since the variable _count_ is increased by _1_ in such a case, then the statement holds after iteration _i_. Finally, if if _m_ is not equal to _xᵢ,_ then we can pair one copy of the element stored in _m_ with _xᵢ_, and so the total number of unpaired copies of the element stored in _m_ decreases by _1_. Since the variable _count_ is decreased by _1_ in such a case, then the statement again holds after iteration _i_. We are done!

The above proof shows that at the end of the algorithm, we have managed to pair distinct elements, and the only element that we have not been able to pair (if any) is the one stored at variable _m,_ if and only if _count > 0._

To conclude the proof (and see why this pairing of elements was, conceptually, very convenient), we observe that if an element is a majority element in a sequence, then it is impossible to pair all of its occurrences with distinct elements, exactly because it is a majority element (which means that there are not enough other elements to create these pairs). Thus, at the end of the algorithm, the variable _m_ will necessarily contain the majority element!