# padics
## A package for computing with p-adic numbers in Maxima CAS

This is just a first attempt to create a Maxima package for
working with p-adic numbers. It is extremely ugly and slow, lacking 
anything remotely resembling elegance or optimization, but at least
it gives correct answers (for all those examples that I have been
able to find in the literature).
The current version is suitable for basic courses on the subject of p-adic
analysis, and maybe for constructing examples illustrating simple theoretical
constructions.
Topics covered are:
* p-adic norm and distance
* Finite-segment representations of Q<sub>p</sub> (Hensel codes)
* p-adic arithmetic
* Conversion from Hensel codes to rational functions
* Newton's method and square roots in Q<sub>p</sub>
* p-adic systems of linear equations

**Remark**: Some functions in the package make use of the commands
`firstn` and `lastn`, which require a Maxima version 5.41 or higher.