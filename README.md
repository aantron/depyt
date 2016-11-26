### Depyt — yet-an-other type combinator library

Depyt is yet an-other type combinator library. It tries to expose a nice API
to define records and variants. It also exposes higher-level combinators
to easily generate equality functions, comparators, pretty-printer, parser
and unparsers.

#### Variants

For instance, to define variants:

```ocaml
# open Depyt;;
# type t = Foo | Bar of string option;;
# let t =
    let foo, mkfoo = case0 "Foo" Foo in
    let bar, mkbar = case1 "Bar" (option string) (fun x -> Bar x) in
    variant "v" [foo; bar] @@ function Foo -> mkfoo | Bar x -> mkbar x;;
val t : t Depyt.t = <abstr>

# Fmt.pr "t = %a\n" (pp t) Foo;;
t = Foo
- : unit = ()

# compare t Foo (Bar (Some "a"));;
- : int = -1

# compare t Foo (Bar (Some "a"));;
- : int = -1
```

#### Records

To define records:

```ocaml
# open Depyt;;
# type t = { foo: int; bar: string list };;
# let t =
    record "r" (fun foo bar -> { foo; bar })
    |+ field "foo" int (fun t -> t.foo)
    |+ field "bar" (list string) (fun t -> t.bar)
    |> seal
va t : t Depyt.t = <abstr>

# Fmt.pr "t = %a\n" (pp t) { foor = 3; bar = ["foo"] };;
{ foo = 3; bar = ["foo"]; }
- : unit = ()
```

Depyt is distributed under the ISC license.

Homepage: https://github.com/samoht/depyt

## Installation

Depyt can be installed with `opam`:

    opam install depyt

If you don't use `opam` consult the [`opam`](opam) file for build
instructions.

## Documentation

The documentation and API reference is automatically generated by from
the source interfaces. It can be consulted [online][doc] or via
`odig doc depyt`.

[doc]: https://samoht.github.io/depyt/doc
