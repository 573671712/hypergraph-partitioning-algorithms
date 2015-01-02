hypergraph-partitioning-algorithms
==================================

This package contains the multi-way hypergraph partitioning
algorithms: FMS (Fiduccia-Mattheyses-Sanchis), PLM (Partitioning by
Locked Moves), PFM (Partitioning by Free Moves), SA (Simulated
Annealing - 2 versions), and RSA (Simulated Annealing with ratio cut
model), as detailed in [DaAy97].

TODO: RSA will be added soon.

## THE HYPERGRAPH PARTITIONING PROBLEM

The hypergraph partitioning problem is defined as follows: Given an
input hypergraph, partition it into a given number of almost
equal-sized parts in such a way that the cutsize, i.e., the sum of the
net weights whose end cells are in different parts, is minimized. This
problem has many variations as well as has important applications in
many areas. Unfortunately, the problem is NP-hard so the algorithms in
this package are heuristics (but they work very very well).

If you do not know what a hypergraph is, remember this: In a graph,
you have vertices and edges, where every edge connects two vertices;
in a hypergraph, you have vertices and hyperedges, where every
hyperedge connects two or more vertices. Since hypergraphs can model
electronic circuits well, a hypergraph is often said to have cells and
nets instead of vertices and hyperedges. In a circuit cell where a net
is connected is called a pin. You may see the cell, net, and pin
terminology in my code.

## A SHORT HISTORY ON THESE ALGORITHMS

The basis for these algorithms go back to the Kernighan-Lin (KL)
algorithm for hypergraph partitioning. The KL algorithm produces very
good partitions but it is slow. The Fiduccia-Mattheyses (FM) algorithm
is not only a faster version of the KL algorithm but it also
generalizes the KL algorithm to run on hypergraphs. Sanchis
generalized the FM algorithm from 2-way partitioning to multi-way
partitioning.

All these KL-based algorithms work in multiple passes over the number
of vertices; in each pass, these algorithms find the best destination
part for a cell and lock this cell from moving again in the rest of
the current pass. Realizing the limitations of this "locking"
mechanism, I devised ways to relax this mechanism, resulting in the
Partitioning by Locked Moves (PLM) algorithm and the Partitioning by
Free Moves (PFM) algorithm. The PLM algorithm still uses locking but
it goes through multiple phases of locking and unlocking within a pass
over the input hypergraph. The PFM algorithm does not use locking at
all; it uses a way to penalize the moves to march towards a local
minimum. For more details, please refer to [DaAy97].

## MORE INTRODUCTION

I originally developed this package in C during my MSc study (around
1991-1993). Before putting this package on github, I converted my code
to ANSI C (c99) and cleaned it a bit. For very large inputs, it is a
good idea to convert all variable-length arrays into dynamically
allocated arrays.

In my code, you may notice some biologically inspired names such as
'population', 'chromosome', 'allele', etc. These names actually come
from genetic algorithms. During my MSc years, my eventual goal was to
implement graph and hypergraph partitioning using genetic algorithms
on a hypercube connected parallel computer (from Intel). Once I
discovered the limitations of the locking mechanism, I changed my
research direction towards what would become PLM and PFM. By the way,
I did still work on genetic algorithms but in a different setting
(unofficially doing another MSc thesis in the process).

This package is available on an "as is" basis. I do not say or imply
that it will be useful for whatever you want to do with it. It may
also contain bugs, and I assume no responsibility for any potential
problems associated with its use. You can use this package free of
charge in academic research and teaching. For any commercial use,
contact Ali Dasdan at ali_dasdan@yahoo.com. See the COPYRIGHT section
below.

## HOW TO COMPILE AND BUILD

Type 'make' (or 'gmake') to build all executables. The executables are
all have .x extension: ad_fms.x, ad_plm.x, ad_pfm.x.

With no targets following the make command, the following executables
will be generated:
- 'ad_fms.x'
- 'ad_plm.x'
- 'ad_pfm.x;

# HOW TO RUN

Type the name of one of the executables in your command line to get
the usage information. At minimum, each executable requires the input
hypergraph and the number of parts to partition the hypergraph. PLM
and PFM require additional parameters to create different versions of
them, which trade off runtime for partition quality.

For example, a run of FMS (in 'ad_fms.x') to partition the input
hypergraph 'p9' into two parts will produce this output:

```
> ad_fms.x input/hp9 2 123456
SEED = 123456 fname = input/p9
pass_no = 7 Final cutsize = 85 Check cutsize = 85
```

This output shows that FMS took 7 passes over the cells of the input
hypergraph 'hp9' when started with a seed of 123456 (which is needed
to make the results repeatable). FMS found a cutsize of 85, which is
correct as FMS and the other programs will check every cutsize they
report for correctness. That is, each of the executables are self
validating.

## HOW TO TEST

Type 'make test' to test each executable on the input hypergraphs under the
'input' directory. The result will be a 'pass' or a 'fail'.

## INPUT FILE FORMAT

The input file format is explained below using a very simple hypergraph.

```
> cat input/hp1
6
7
14
1 2 0 1
1 2 1 2
1 2 2 0
1 2 3 4
1 2 4 5
1 2 5 3
1 2 0 3
1
1
1
1
1
1
```

The first three lines give the number of cells, the number of nets,
and the number of pins (or endpoints), respectively. Thus, 'p1' has 6
vertices, 7 edges, and 14 pins.

The following 7 lines describe the nets, one net per line. The first
number is the net weight; the second number is the number of pins on
this net (always equal to 2 for graphs)); the third number is the
source cell; and the fourth number is the target cell. For example,
the first net from cell 0 to cell 1 has a weight of 1.

The last 6 lines describe the cell weights.

## REFERENCE

Please cite this reference if you use my programs in your research
work.

```
@article{DaAy97,
 author = {Ali Dasdan and C. Aykanat},
 title = {Two Novel Circuit Partitioning Algorithms Using Relaxed Locking},
 journal = {IEEE Transactions on Computer-Aided Design of Integrated Circuits and Systems (TCAD)},
 volume = {16},
 number = {2},
 year = {1997},
 pages = {169-178},
 }
```

## COPYRIGHT

COPYRIGHT C 1991 - Ali Dasdan

Permission to use for non-commercial purposes is granted provided that
proper acknowledgments are given. For a commercial licence, contact
Ali Dasdan at ali_dasdan@yahoo.com.

This software is provided on an "as is" basis, without warranties or
conditions of any kind, either express or implied including, without
limitation, any warranties or conditions of title, non-infringement,
merchantability or fitness for a particular purpose.

## END OF FILE

