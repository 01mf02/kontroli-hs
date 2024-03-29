# Kontroli

Kontroli (Esperanto for *verify*) is
an alternative implementation of the logical framework
[Dedukti](https://deducteam.github.io/).

The use case of Kontroli is to
verify machine-generated output of automated reasoning tools,
such as proof assistants and automated theorem provers.
Kontroli strives to provide a
small, well-readable implementation
using established techniques
emphasising performance.
It shall serve to verify the output of Dedukti.

Currently, Kontroli only implements a parser.
On files generated from Isabelle (HOL.Inductive),
Dedukti takes 10.1 seconds to parse a 74MB DK file and
Kontroli takes 9.6 seconds to parse a 75MB KO file.

## Syntax

Kontroli implements a small subset of Dedukti's syntax,
modulo the addition of a prefix binder in front of
lambda abstractions and dependent products.
That is, a lambda abstraction
`x : A => y : B => x` in Dedukti becomes
`\ x : A => \ y : B => x` in Kontroli.
This eliminates [left-recursion](https://en.wikipedia.org/wiki/Left_recursion)
from the grammar, thus allowing for a greatly simplified parser in Kontroli.

An example of the syntax can be seen in [examples/pure.ko](examples/pure.ko).

To obtain a Dedukti from a Kontroli file, use:

    sed -e 's/\\ //g' -e 's/! //g' file.ko > file.dk

## Installation

    cabal update
    cabal sandbox init
    cabal install --only-dependencies
    cabal configure
    cabal install

## Usage

To run Kontroli on output generated from Isabelle/Pure:

    cabal run -- examples/pure.ko
