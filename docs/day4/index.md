Day 4: Syntax and Parsing
=========================

# Section 1: CFGs with NLTK

## Exercise 1: Basic CFG use

NLTK contains a method for loading a CFG from a string.
Here, for example, is the small CFG given in the lecture,
specified in the format NLTK can load.

````python
cfg_rules = """
S -> NP VP
NP -> Det N | PropN
Det -> PosPro | Art
VP -> Vt NP

Art -> 'the' | 'a'
PropN -> 'Alice'
N -> 'duck' | 'telescope' | 'park'
Vt -> 'saw'
PosPro -> 'my' | 'her'
"""
cfg = nltk.CFG.fromstring(cfg_rules)
````

This grammar is almost in Chomsky Normal Form.
The only respect in which it diverges is that it
contains 'unary' rules, like 'NP -> PropN'.
The version of CKY shown in the lecture permits
these and NLTK's `is_flexible_chomsky_normal_form()`
method does too.

````python
print(small_cfg.is_flexible_chomsky_normal_form())
````

![Example tree 1](example_tree1.png "Example tree 1")

![Example tree 2](example_tree1.png "Example tree 2")

 * Look at the two example trees above, taken from the Penn Treebank.
   Write a CFG using the above format that produces
   these tree analyses for these two sentences.
   (The grammar does not yet need to be in CNF.)

   **Submit your CFG rules in text form**


The NLTK CFG type has a method to check that
all the words of the input sentence are covered
by lexical rules in the grammar. Check now that
you've got lexical rules in your grammar for the
two example sentences.

````python
# Check that all the words of the input sentence are covered
sentences = [
    "the purchase price includes two ancillary companies .".split(),
    "the guild began a strike against the TV and movie industry in March 1988 .".split(),
]
for s in sentences:
    grammar.check_coverage(s)
````


## Exercise 2: Extending the grammar

Add some more rules to your grammar so that it can
also generate the sentence:

*"the guild bought one ancillary company ."*

Check that the new, extended grammar can be loaded
and that it covers the new sentence as well as the old
ones.

 * **Submit the additional rules you needed to add**


## Exercise 3: Converting to CNF

So far, we've only run sanity checks that the words
of sentences are covered by the grammar. We haven't
yet used the grammar to parse the sentences.
NLTK includes implementations of a number of different
parsing algorithms, including the bottom-up chart parsing
algorithm introduced in the lecture (*CKY*).

The example trees include nodes with more than two
children (e.g. the NP covering *"the TV and movie
industry"*). This causes problems for the parsing
algorithm, but any CFG can be converted to
**Chomsky normal form** (CNF) without
changing the sentences it generates.

For example, a rule
````
A -> B C D
````
can be replaced by
````
A -> B2 D
B2 -> B C
````
where `B2` is a new non-terminal.

Convert your grammar above into "flexible" CNF
(i.e. CNF, but allowing unary rules), load it and
verify that it's correct using
`cfg.is_flexible_chomsky_normal_form()`.



## Exercise 4: Parsing with the grammar

Load a bottom-up chart parser and initialize it with
your CNF grammar:

````python
from nltk.parse.chart import BottomUpChartParser
parser = BottomUpChartParser(cnf_grammar)
parses = list(parser.parse(sentence))
````

The results are tree structures.

If your grammar is correct, you should get at least
one full parse for each of the example sentences in
exercises 1 and 2.

Confirm that the trees produced by the parser match
the two example trees (with the exception of the
additional nodes added in normalization of the grammar).

(You can use the method `parse.draw()` to display the
parse result graphically.)

 * Do you get multiple full parses for any of your
   sentences?
 * Do any of these capture "real" ambiguity
   that distinguishes different interpretations of the
   sentence?

   **Submit your answers**






# Section 2: Treebank parser


## Exercise 5

NLTK provides easy access to a 10% sample of the Penn
Treebank. The full treebank is not available without a
license, but this sample is enough for us to build a
treebank grammar from.

Start by loading the treebank as follows and taking a
look at a couple of its parse trees:

````python
from nltk.corpus import treebank
print(treebank.parsed_sents()[0])
print(treebank.parsed_sents()[1])
````

Or, graphically:

````python
treebank.parsed_sents()[0].draw()
````

Build a treebank grammar from all the trees in this sample
of the corpus.

Use your grammar as you did in  to parse the following
sentences.

 - *Mr. Vinken is chairman .*
 - *Stocks rose .*
 - *Alan introduced a plan .*

 * How many parse trees does the parser find
   for each sentence?
 * What problems do you see with this parsing
   process?

   **Submit your answers to these questions**