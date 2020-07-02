# Floating-point arithmetic
A collection of comments on floating-point arithmetic (e.g. rounding in GCC) for future reference to myself that may also be useful for others.

## Floating-point rounding and GCC
Discrepancies in floating-point rounding between the IEEE and GCC have been known for over a decade [1], yet they continue to give rise to blogs and discussions [2,3]. All of them contribute to create awareness of an issue that even well-seasoned developers might ignore, but some seem to only insist in points that have already been addressed. This is the case of regarding the fact that GCC may not round to the nearest representable value as a bug [2]. Many explanations have been given for this discrepancy, but I have never seen any mention to the C standard. Turns out that the C standard specifically states that

> For decimal floating constants, and also for hexadecimal floating constants when FLT_RADIX is not a power of 2, the result is either the nearest representable value, or the larger or smaller representable value immediately adjacent to the nearest representable value, chosen in an implementation-defined manner

Contrary to [2], the C standard does not guarantee that 
```
0.50178230318 = 4519653187245114.50011795456 * 2 ** -53
```
will be represented as 
```
4519653187245115  * 2 ** -53
```
but allows it to be represented as one of the following values
```
4519653187245114  * 2 ** -53
4519653187245115  * 2 ** -53
4519653187245116  * 2 ** -53
```
in an implementation defined manner, thus letting GCC define how it does it. In addition, rounding to the nearest integer need not always be the best as [2] states but depends on the application. Finally, the problem is not limited to GCC [4]. 

Whether a bug or not and who the culprit is in anyone's view, this discrepancy an be exploited in very interesting ways, as discussed in [3]. 

## Fix-point arithmetic issues in GCC

Working on it based on [5].

## References
[1]: https://www.springer.com/gp/book/9780817647056
[2]: https://lemire.me/blog/2020/06/26/gcc-not-nearest/
[3]: https://news.ycombinator.com/item?id=23665159
[4]: https://wiki.tcl-lang.org/page/A+real+problem
[5]: https://arxiv.org/pdf/2001.01496.pdf
