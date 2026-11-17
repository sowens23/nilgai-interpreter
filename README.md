# Nilgai Interpreter

An interpreter for **Nilgai**, a small imperative programming language, written in Lua. Built for CS 331 (Programming Languages) at the University of Alaska Fairbanks, Spring 2024.

## How It Works

Source code → **Lexer** → tokens → **Parser** → abstract syntax tree → **Interpreter** → output

- **Lexer (`lexit.lua`):** a state-machine lexer that categorizes keywords, identifiers, numeric and string literals, operators, and punctuation, and flags malformed lexemes.
- **Parser (`parseit.lua`):** a recursive-descent parser that checks programs against the Nilgai grammar and builds an AST, handling operator precedence and associativity.
- **Interpreter (`interpit.lua`):** walks the AST and executes it, supporting variables, arrays, arithmetic and comparison operators, `if/elseif/else`, `while` loops, functions, input, and output.

## Example

    def fibo() { ... }
    output("F(10) = ", fibo(), eol);

## Running It

Requires Lua 5.3 or later.

    lua nilgai.lua examples/fizzbuzz.ngai   # run a program
    lua nilgai.lua                          # interactive REPL

## Language Grammar

See [docs/grammar.md](docs/grammar.md).

## Credits

The Nilgai language, the REPL (`nilgai.lua`), and starter skeletons for the parser and interpreter were provided by Professor Glenn G. Chappell. I implemented the lexer extensions, the recursive-descent parser, and the AST interpreter.
