# monkey-go 🐒

An interpreter **and** a bytecode compiler + virtual machine for the **Monkey** programming language, written in Go.

Monkey has C-like syntax, first-class and higher-order functions, closures, integers, booleans, strings, arrays, and hash maps. This project implements it twice, sharing the same lexer, parser, and AST:

1. **Tree-walking evaluator**: walks the AST and evaluates nodes directly.
2. **Compiler + VM**: compiles the AST into bytecode once, then runs it on a stack-based virtual machine. It's about **3x faster** than the evaluator (see [Benchmark](#benchmark)).

The REPL runs on the compiler + VM.

## Features

- **Lexer:** hand-written, no regex, tokenizes the full Monkey grammar
- **Parser:** recursive-descent with Pratt-style precedence parsing for expressions
- **Evaluator:** tree-walking evaluator with closures and an environment chain
- **Compiler:** AST → bytecode, with symbol tables for global, local, built-in, and free (captured) variables
- **Virtual machine:** stack-based VM with call frames and real closures
- Data types: integers, booleans, strings, arrays, hashes, functions
- Built-in functions: `len`, `first`, `last`, `rest`, `push`, `puts`
- A REPL for interactive exploration

## How it works

```
                                    ┌──> evaluator ──────────────> result
source ──> lexer ──> parser ──> AST ┤
                                    └──> compiler ──> bytecode ──> VM ──> result
```

## Project layout

```
.
├── token/       Token type definitions
├── lexer/       Source code → tokens
├── ast/         AST node definitions
├── parser/      Tokens → AST (Pratt parsing)
├── object/      Runtime object system (Integer, String, Closure, built-ins, ...)
├── evaluator/   Tree-walking evaluator: AST → result
├── code/        Bytecode instruction set: opcodes and encoding
├── compiler/    AST → bytecode, plus symbol tables for scope resolution
├── vm/          Stack-based virtual machine that executes bytecode
├── repl/        Interactive read-eval-print loop (runs on the VM)
├── benchmark/   Evaluator vs. VM performance comparison
└── main.go      Entry point, starts the REPL
```

## Examples

**Closures and higher-order functions:**

```monkey
let newAdder = fn(x) {
  fn(y) { x + y };
};

let addTwo = newAdder(2);
addTwo(3); // => 5
```

**Arrays, hashes, and built-ins:**

```monkey
let people = [{"name": "Anna", "age": 24}, {"name": "Bob", "age": 30}];
let getName = fn(person) { person["name"] };

getName(people[0]); // => Anna
len(people);        // => 2
```

## Getting started

Requires [Go](https://go.dev/) 1.25+.

```sh
git clone https://github.com/arafat4693/monkey-go.git
cd monkey-go
go run .
```

This drops you into the REPL:

```
Hello <you>! This is the Monkey programming language!
Feel free to type in commands
>> let x = 10;
10
>> x * 2;
20
```

Run the test suite:

```sh
go test ./...
```

## Benchmark

[benchmark/main.go](benchmark/main.go) computes `fibonacci(35)` recursively with either engine:

```sh
go build -o fibonacci ./benchmark
./fibonacci -engine=eval
./fibonacci -engine=vm
```

Results on an Apple M3 Pro:

| Engine    | Time  |
|-----------|-------|
| evaluator | 8.98s |
| VM        | 3.00s |

The VM is faster because the compiler walks the AST only once. The evaluator re-walks the same function-body nodes on every recursive call, while the VM replays already-compiled bytecode.

## Acknowledgements

This project follows the design and exercises from two books by Thorsten Ball:

- [*Writing An Interpreter In Go*](https://interpreterbook.com/): the lexer, parser, and tree-walking evaluator
- [*Writing A Compiler In Go*](https://compilerbook.com/): the bytecode compiler and virtual machine

Both are excellent, hands-on introductions. If you want to understand *how* any of this works, they're the best place to start.

## License

[MIT](LICENSE)
