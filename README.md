# padics: A package for computing with p-adic numbers in Maxima CAS #

## Overview ##

This is just a first attempt to create a Maxima package for
working with p-adic numbers. It is extremely ugly and slow, lacking 
anything remotely resembling elegance or optimization, but at least
it gives correct answers (for all those examples that I have been
able to find in the literature).
The current version is suitable for basic courses on the subject of p-adic
analysis, and maybe for studying examples illustrating simple theoretical
constructions.
Topics covered are:
* p-adic norm and distance
* Finite-segment representations of **Q**<sub>p</sub> (Hensel codes)
* p-adic arithmetic
* Conversion from Hensel codes to rational functions (through Farey fractions)
* Newton's method and square roots in **Q**<sub>p</sub>
* p-adic systems of linear equations

**Remark**: Some functions in the package make use of the commands
`firstn` and `lastn`, which were introduced in Maxima 5.39.
If `firstn` or `lastn` are not detected as built-in functions,
equivalent functions are defined here.


## Installation ##

To install the package, simply copy `padics.mac` to your working
directory, or make a global installation by copying the file to
`/usr/share/maxima/5.42.1/share/contrib` (or its Windows/MacOS equivalent).

The package can be loaded inside a Maxima session by typing
```
load("padics.mac");
```

The file `padics-doc.pdf` describes a Maxima session using the commands in the package.
After loading the file `padics-index.lisp`, the Maxima commands ? and ?? will find items
in the index and display them, e.g. 
```
? padic_sum;
```
Loading via a specific path e.g. 
```
load("path/to/padics/padics-index.lisp")
```
and loading via appending the padics package directory e.g.
```
push("path/to/padics/###.lisp");
load("padics-index.lisp");
```
both load the index and append the `padics` documentation items to the online help system.

## Examples ##

```
(%i1)	load("padics.mac")$
```
An example giving the p-adic norm of the rational 140/297 for several values of p
(taken from http://mathworld.wolfram.com/p-adicNorm.html)
```
(%i2)	makelist(padic_norm(140/297,k),k,[2,3,5,7,11]);
(%o2)	[1/4,27,1/5,1/7,11]
```
The next example (3-adic distance between 82 and 1) comes from 
[](https://www.sangakoo.com/en/unit/p-adic-distance)
```
(%i3)	padic_distance(82,1,3);
(%o3)	1/81
```
Hensel codes are displayed as lists, as in this example (giving the Hensel code
of length 4 of the rational 2/7 in **Q**<sub>5</sub>):
```
(%i4)	hensel(2/7,5,4);
(%o4)	[[0],1,2,1,4]
```
To display the result in the form commonly found in textbooks and expository works,
we have the command `nicehensel`:
```
(%i5)	nicehensel(2/7,5,4);
(%o5)	.1214
```
Arithmetic operations are carried using the format used by `hensel`. This example
is taken from C. Limongelli and R. Pirastu (p-adic Arithmetic and Parallel Symbolic Computation: 
An Implementation for Solving Linear Systems. Computers and Artificial Intelligence **14** 1 (1996) 35–62):
```
(%i6)	padic_sum(padic_divide([[0],4,3,3,3],padic_sum([[0],3,2,2,2],[[0],2,3,1,3],5),5),[[-2],1,0,0,0],5);
(%o6)	[[-2],1,4,2,2]
```
A quick check that the answer is correct, because 1/4/(1/2+1/3)+1/25=17/50
```
(%i7)	hensel(17/50,5,4);
(%o7)	[[-2],1,4,2,2]
```
We can convert Hensel codes back to rationals using Farey sequences:
```
(%i8)	hensel_to_farey([[1],2,4,1,4],5);
(%o8)	5/8
```
We can compute p-adic roots using Newton's method, and the command `padic_sqrt` 
--whose syntax is `padic_sqrt(number,p)`-- admits an optional argument fixing the 
number of iterations to be done, as in `padic_sqrt(number,p,iterations)`:
```
(%i9)	padic_sqrt(2,7);
(%o9)	[215912063945802350977/152672884556058511392,2267891697076964737/1603641597827614272]
(%i10)	padic_sqrt(7,3,3);
(%o10)	[977/368,108497/41008]
```
To solve the problem Ax=b, we have the commands `padic_gauss` and
`padic_backsub`. The first one triangularizes the system, and its syntax
is `padic_gauss(B,p)`, where B=A|b is the augmented matrix of the system
(that is, the coefficient matrix A augmented with the column b of non
homogeneous terms). The resulting triangular matrix is processed
by `padic_backsub` to obtain the Hensel codes of the solution:
```
(%i11)	D:matrix([3,1,3,16],[1,3,1,8],[1,1,3,12]);
(D)	matrix(
		[3,	1,	3,	16],
		[1,	3,	1,	8],
		[1,	1,	3,	12]
	)
(%i12)	padic_gauss(D,11);
(%o12)	matrix(
		[[[0],3,0,0,0],	[[0],1,0,0,0],	[[0],3,0,0,0],	[[0],5,1,0,0]],
		[[[0],0,0,0,0],	[[0],10,3,7,3],	[[0],0,0,0,0],	[[0],10,3,7,3]],
		[[[0],0,0,0,0],	[[0],0,0,0,0],	[[0],2,0,0,0],	[[0],6,0,0,0]]
	)
(%i13)	padic_backsub(%,11);
(%o13)	[[[0],2,0,0,0],[[0],1,0,0,0],[[0],3,0,0,0]]
```
Converting to Farey fractions we get the rational form of the solutions:
```
(%i14)	map(lambda([x],hensel_to_farey(x,11)),%);
(%o14)	[2,1,3]
```
