# Crotalinae

Metaprogramming for Ethereum Smart Contracts, expressed in Scala's Type System.

- what is scala: functional, strictly typed, JVM, huge, banking and big data developers
- code reuse is as simple as in javascript: everything is modular, but finally compiles to a single file
- you can optimize workload and reuse code snippets in secure way
- if you break something in a piece of code, you won't be able to compile it -- notice problems on early stage!

## How to use it

So you take Scala language and _Crotalinae_ DSL, built with Shapeless and Free Monads, and write the code.

If this code compiles (in Scala), then the Smart Contract generated with this code will also compile. Constraints are validated in the type system, so you can relay on IDE support and compile time errors.

Finally you have composable chunks of code that can form different contracts with no copy-paste.

Contracts are exported in [Vyper](https://github.com/ethereum/vyper) language. If you generate a lot of code, you still can check it visually.

## Example

This _crazy_ Scala code:

```scala

  val f = `@public` @:
    sumArgs.funcDef("sum", uint256) { args ⇒
    for {
      c ← 'c :=: `++`(args.ref('a), args.ref('b))
      d ← 'd :=: `++`(args.ref('b), c)
      sum ← `++`(args.ref('a), d).toReturn
    } yield sum
  }
  
  println(f.toVyper)

```

Compiles into this:

```python

@public
def sum(a: uint256, b: uint256) -> uint256:
  c = a + b
  d = b + c
  return a + d

```

This smart contract is deployed on [Rinkeby](some link to rinkeby scanner). Hooray!

## Future plans

Of course, building EVM code from Vyper, from Scala DSL, using Free Monads and Shapeless Records, is not enough.

So we're working on Macro Programming support as well. It will add one more layer of Scala code (parsed to Scala AST, used to generate Crotalinae DSL, and down on a chain).

So stay tuned!