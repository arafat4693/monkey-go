# monkey-go 🐒

A tree-walking interpreter for the **Monkey** programming language, written in Go.

Monkey has C-like syntax, first-class and higher-order functions, closures, integers, booleans, strings, arrays, and hash maps. This implementation includes a lexer, a Pratt (operator-precedence) parser, an AST, and a tree-walking evaluator, plus a REPL to try it all out interactively.

## Features

- **Lexer:** hand-written, no regex, tokenizes the full Monkey grammar
- **Parser:** recursive-descent with Pratt-style precedence parsing for expressions
- **Evaluator:** tree-walking evaluator with proper closures and an environment chain
- Data types: integers, booleans, strings, arrays, hashes, functions
- Built-in functions: `len`, `first`, `last`, `rest`, `push`, `puts`
- A REPL for interactive exploration

## Project layout

```
.
├── token/       Token type definitions
├── lexer/       Source code → tokens
├── ast/         AST node definitions
├── parser/      Tokens → AST (Pratt parsing)
├── object/      Runtime object system (Integer, String, Function, Environment, ...)
├── evaluator/   AST → evaluated result (tree-walking evaluator + built-ins)
├── repl/        Interactive read-eval-print loop
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
>> x * 2;
20
```

Run the test suite:

```sh
go test ./...
```

## Acknowledgements

This project follows the design and exercises from [*Writing An Interpreter In Go*](https://interpreterbook.com/) by Thorsten Ball, an excellent, hands-on introduction to building interpreters. If you want to understand *how* any of this works, that book is the best place to start.

## License

[MIT](LICENSE)
