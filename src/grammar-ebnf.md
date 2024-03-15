# Extended BNF

It's not a secret that BNF specification are not always nice to write down.
There were quite a few hoops to jump through in order to design an unambiguous grammar for arithmetic expressions in the previous section.

So it's not surprising that there are a number of extensions to the BNF (meta-)syntax which make it more usable.
If you go digging around the Internet for Extended BNF (EBNF), you'll find a couple definitions, most of which are more complex than what we will choose to call EBNF here.
That said, the extensions we consider in this short section will a useful precursor for the following section on regular grammars and regular expressions.

## Optional

We use `[ SENT-FORM ]` to notate part of a production rule which is optional.
We may, for instance, want to write a language which has both if-then and if-then-else expressions.
Rather than using the BNF production rules

```
<if-expr> ::= if <expr> then <expr> | if <expr> then <expr> else <expr>
```

we can use the EBNF production rule

```
<if-expr> ::= if <expr> then <expr> [else <expr>]
```

These two rules express the same thing.
This also points to an important point: all BNF production rules are also EBNF production rules, and all EBNF production rules can be rewritten as a collection of BNF production rules.

> **Exercise.** Rewrite the following EBNF production rule as a collection of BNF production rules.
> ```
> <a> ::= a [ <b> ] [ a ]
> ```

> **Remark.** One issue with extending BNF is now we've made it harder to express grammars which include the symbols used for the EBNF extensions (e.g., `[` and `]`).
> In practice this is not a problem, we will try to be very explicit if something like `[` appears as part of a symbol of the grammar, and not a part of the specification of the grammar

## Alternative

We use `( SENT-FORM-1 | SENT-FORM-2 | ... | SENT-FORM-k )` to notate the choice of multiple sentential forms as part of a production rule.
In the previous section we defined a grammar for arithmetic expressions with multiple rules for the choice of operation.

```
<expr>          ::= <only-mul-div> | <expr> <add-sub> <only-mul-div>
<only-mul-div>  ::= <var-or-parens> | <only-mul-div> <mul-div> <var-or-parens>
<add-sub>       ::= + | -
<mul-div>       ::= * | /
<var-or-parens> ::= x | ( <expr> )
```

We can simplify this in EBNF by removing the production rules for operators.

```
<expr>          ::= <only-mul-div> | <expr> (+ | -)  <only-mul-div>
<only-mul-div>  ::= <var-or-parens> | <only-mul-div> (* | /) <var-or-parens>
<var-or-parens> ::= x | ( <expr> )
```

## Repetition

We use `{ SENT-FORM }` to notate a part of a production rule which can be repeated as many times as we want.
We can also combine this with the alternative notation to as

```
{SENT-FORM-1 | SENT-FORM-2 | ... | SENT-FORM-k}
```

to represent a collection of choices we may repeat as many times as we want.

In the same grammar as above, we can rewrite the recursive production rules in terms of repetition.

```
<expr>          ::= <only-mul-div> { (+ | -) <only-mul-div> }
<only-mul-div>  ::= <var-or-parens> { (* | /) <var-or-parens> }
<var-or-parens> ::= x | ( <expr> )
```

In general, we can think of the following pairs of rules as equivalent.

```
<a> ::= a { b }
<a> ::= a | <a> b

<a> ::= { b } a
<a> ::= a | b <a>
```

> **Exercise.** Write an EBNF specification for Boolean expressions in Python.
