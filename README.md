## blott

An implementation of normalization by evaluation (nbe) & semantic type checking for Martin-Löf Type
Theory with dependent products, dependent sums, natural numbers, id, box, and a cumulative hierarchy
of universes. This implementation supports eta-rules for box, pi, and sigma.

For examples, see the `test/` directory.

The type theory underlying `blott` as well as the implementation is described in
[Implementing a Modal Dependent Type Theory](https://doi.acm.org/10.1145/3341711).

## building

blott has been built with OCaml 4.06.1 and 4.07.1 with [opam 2.0](https://opam.ocaml.org/). Once
these dependencies are installed blott can be built with the following set of commands.

```
$ opam update
$ opam pin add -y blott .               # first time
$ opam upgrade                          # after packages change
```

After this, the executable `blott` should be available. The makefile can be used to rebuild the
package for small tests. Locally, blott is built with [dune](https://dune.build), running the above
commands will also install dune. Once dune is available the executable can be locally changed and
run with the following:

```
$ dune exec ./src/bin/main.exe          # from the `blott` top-level directory
```

## syntax

This experimental proof assistant supports the following top-level declarations:

 - A definition, written `let NAME : TYPE = TERM`
 - A command to normalize a definition `normalize def NAME`
 - A command to normalize an expression `normalize TERM at TYPE`

Unlike in the paper, names instead of indices are used for the surface syntax. The following are
the valid expressions in blott.

``` ocaml
     (* Let bindings *)
     let NAME = TERM in TERM
     let NAME : TYPE = TERM in TERM
     (* Natural numbers *)
     nat, 0, 1, 2...
     (* Recursion on natural numbers *)
     rec NUMBER at x -> MOTIVE with
     | zero -> TERM
     | suc n, ih -> TERM

     (* Functions *)
     (NAME : TP) -> TP
     fun NAME -> TERM
     TERM TERM

     (* Pairs *)
     (NAME : TP) * TP
     <TERM, TERM>
     fst TERM
     snd TERM

     (* The box modality *)
     [box TP]
     [lock TERM]
     [unlock TERM]

     (* Universes *)
     U<0>,U<1>,...

     (* Identity types *)
     Id TYPE TERM TERM
     refl TERM
     match PRF at x y prf -> MOTIVE with
     | refl x -> TERM
```

A small collection of example programs is contained in the `test/` directory. See `test/README.md`
for a brief description of each program's purpose.
