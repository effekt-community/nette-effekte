> [!NOTE]
> This is a community-maintained fork of a MIT-licensed student project in the *Effective Programming with Effects* course in winter semester 2025/2026.
> Be warned that this is not an officially endorsed project, the code in this repository may be not idiomatic Effekt.
> 
> The original repository is https://github.com/marvinborner/nette-effekte

# Nette Effekte

> interactive interaction net compiler and reducer

      ╭─────╮
    ╭─┴─╮ ╭─┴─╮
    │ ζ │ │ δ │  
    ╰┬─┬╯ ╰┬─┬╯
     │ ╰───╯ │
     ╰───────╯

-   [THEORY](THEORY.txt)
-   [ARCHITECTURE](ARCHITECTURE.md)

> no ai was used for any part of this project

# installation

## manual

-   Install a current Effekt version and its JS dependencies as per [the
    instructions](https://effekt-lang.org/docs)
    -   You can ignore LLVM, as we do not support it for now

## nix

> the nix setup is untested except for running CI

``` bash
nix develop # open shell with installed dependencies
nix run     # run main entry
nix build   # build project
```

# run

## interactive web

``` bash
effekt --backend js-web app/main.effekt
```

If building has finished successfully, Effekt will give you a link you
can open in a browser (recommended: modern desktop firefox).

You can enter terms in the left panel (see [THEORY](THEORY.txt) for the
syntax). Multiple lines will be interpreted as multiple constructors or
multiple lambda terms. You can then render the input by clicking the
*render* button. Pan/zoom the produced graph by dragging/clicking your
mouse. Step the reducer by pressing *step*. Change the source language
by pressing the *mode* button.

The rendered triangles are mapped to the following: Light triangles =
constructors, dark triangles = duplicators.

## tui

``` bash
effekt --backend js app/main.effekt
```

The TUI only supports lambda terms (see [THEORY](THEORY.txt) for the
syntax). Enter a term and press enter to compile. Step reduction with an
exclamation mark `!`. Normalize with `@`. Exit with `Ctrl+c`.

## tests

``` bash
effekt --backend js test/main.effekt
```

# examples

## lambda calculus

Remember the [THEORY](THEORY.txt): for now this only supports a very
small subset of the lambda calculus (we do not even label duplicators)!
Though at least the linear lambda calculus is supported.

-   `x => x` (identity)
-   `x => y => x` (`k`, notice the erasure via nilary duplicator)
-   `x => x x => x` (parsed as two separate identities)
-   `(x => (x x) x => x)` (duplication of identity, reduces to identity)
-   `s => z => (s (s (s (s z))))` (church 4, nesting of duplicators)
    -   (effekt might crash with `b_k_0 is not defined` here)

## interaction calculus

-   `a+(a-)` (cycle)
-   `a+(b+, b-)` (lambda identity)
-   `a+(b+, c-) a-[d-, e+] b-(a+, a-) c+(a+, a-) d+(a+, a-) e-(a+, a-)`
    (unfolding)

# things to note / bugs

-   sometimes you have to resize the window first in order for the
    canvas size to be interpreted correctly (otherwise the agents can
    appear stretched/squeezed)
-   the parser emits successfully parsed parts of the term eagerly, so
    if there's some garbage after correctly parsed code, it will not
    complain
-   non-binary duplication is not fully supported for now
-   if you leave the site open for too long, your memory will eventually
    run out

# resources

Multiple external resources were used for this project. If code was
taken from other projects, it's clearly stated as such either in a
comment at the top of the file or above the respective code snippet.

For one, the theory is influenced by the papers mentioned in my
[resource
collection](https://github.com/marvinborner/interaction-net-resources).
The parsers and lexers are copied almost verbatim from the [compiler
frontend casestudy](https://effekt-lang.org/docs/casestudies/frontend)
in Effekt's documentation (MIT). Several formulas and ideas (see
`net/layout`) are inspired by the Haskell
[graph-rewriting](https://github.com/jrochel/graph-rewriting) library by
Jan Rochel (MIT).
