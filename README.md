## How to run examples

Use `nix eval --expr` instead of `nix-instantiate --eval --expr`:

```bash
$ nix-instantiate --eval --expr '(x: y: { a = x + "-" + y; }) "a" "b"'
{ a = <CODE>; }
$ nix eval --expr '(x: y: { a = x + "-" + y; }) "a" "b"'
{ a = "a-b"; }
```

For node you must use `console.log` unless it is already in the example.

```bash
$ node -e 'console.log(((x, y) => ({ a: x + "-" + y }))("a", "b"))'
{ a: 'a-b' }
```

If you want to have `nix eval` available in nixos stable, you need to install unstable nix command as described in https://nixos.wiki/wiki/Flakes.


## Examples

| Nix | Javascript |
| --- | --- |
| `"Hello world!"` | `"Hello world!"` |
| `2/3` | `require("path").join("2", "3")` |
| `2/ 3` | `Math.ceil(2/3)` | |
| `2/ 3.` | `2/3` |
| `(a: { a = a; }) 2` | `(a => ({ a: a }))(2)` |
| `(a: b: { a = a; b = b; }) 2 3` | `((a, b) => ({ a: a, b: b }))(2, 3)` |
| `(a: { inherit a; }) 2` | `(a => ({ a }))(2)` |
| `(a: b: { inherit a b; }) 2 3` | `(a => b => ({ a, b }))(2)(3)` |
| `(a: b: { inherit a b; }) 2 3` | `((a, b) => ({ a, b }))(2, 3)` |
| `({ a }: { inherit a; }) { a = 2; }` | `(({ a }) => ({ a }))({ a: 2 })` |
| `({ a, b, ... }: { inherit a b; }) { a = 2; b = 3; c = 4; }` | `(({ a, b }) => ({ a, b }))({ a: 2, b: 3, c: 4 })` |
| `(a: { c = a.b.c; }) { b = { c = 2; }; }` | `(a => ({ c: a.b.c }))({ b: { c: 2 } })` |
| `(a: { inherit (a.b) c; }) { b = { c = 2; }; }` | `(a => ({ c: a.b.c }))({ b: { c: 2 } })` |
| `({ a ? 2 }: a) {}` | `(({ a = 2 }) => a)({})` |
| `let double = x: x*2; in double 3` | `const double = x => x*2; console.log(double(3));` |
| `let mul = a: (b: a*b); in (mul 2) 3` | `const mul = a => b =>  a*b; console.log(mul(2)(3));` |
| `let x = 3; mul = a: (b: a*b); in (mul 2) 3` | `const x = 3, mul = a => b =>  a*b; console.log(mul(x)(3));` |
| `if true then 3 else 2` | `true ? 3: 2` |

## Links

- https://learnxinyminutes.com/docs/nix/
- https://medium.com/@MrJamesFisher/nix-by-example-a0063a1a4c55
- https://nixery.dev/nix-1p.html
- https://nixos.org/manual/nix/unstable/command-ref/new-cli/nix3-eval.html
- https://nixos.wiki/wiki/Nix_command/eval
- https://nixos.org/guides/nix-pills/functions-and-imports.html

## Expression vs statement

You can read that Nix is Nix expression language.

>Expression: Something which evaluates to a value. Example: 1+2/x
>
>Statement: A line of code which does something. Example: GOTO 100

Or another explanation:

Expression -- from the New Oxford American Dictionary:

> expression: Mathematics a collection of symbols that jointly express a quantity : the expression for the circumference of a circle is 2πr.

Statement from [Wikipedia](http://en.wikipedia.org/wiki/Python_(programming_language)#Statements_and_control_flow):

>In computer programming a statement can be thought of as the smallest standalone element of an imperative programming language. A program is formed by a sequence of one or more statements. A statement will have internal components (e.g., expressions).

- https://stackoverflow.com/questions/19132/expression-versus-statement/19224#19224
- https://stackoverflow.com/questions/4728073/what-is-the-difference-between-an-expression-and-a-statement-in-python/4730559#4730559

### Example

Below `const a` is a statement which consist of expression. [Conditional (ternary) operator](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Conditional_Operator) is a statement.

```javascript
const a = true ? 1 : 0;
```

## JSON

Description from [Nickel](https://github.com/tweag/nickel), possible successor of Nix language:

>Its purpose is to automate the generation of static configuration files - think JSON, YAML, XML, or your favorite data representation language - that are then fed to another system. It is designed to have a simple, well-understood core: it is in essence JSON with functions.

