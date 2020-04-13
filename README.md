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
