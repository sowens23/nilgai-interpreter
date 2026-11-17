# Nilgai Interpreter

An interpreter for **Nilgai**, a small imperative programming language, written in Lua. It includes a hand-written lexer, a recursive-descent parser, and a tree-walking interpreter.

Built for CS 331 (Programming Languages) at the University of Alaska Fairbanks, Spring 2024.

## How It Works

```
source code  ->  Lexer  ->  lexemes  ->  Parser  ->  AST  ->  Interpreter  ->  output
```

- **Lexer (`lexit.lua`):** a state-machine lexer that sorts input into keywords, identifiers, numeric and string literals, operators, and punctuation, and flags malformed lexemes such as unterminated strings.
- **Parser (`parseit.lua`):** a recursive-descent parser with one function per grammar rule. It validates programs against the Nilgai grammar, handles operator precedence and left associativity, and builds an abstract syntax tree.
- **Interpreter (`interpit.lua`):** walks the AST and executes it, supporting variables, arrays, arithmetic, comparison, and logical operators, `if`/`elseif`/`else`, `while` loops, user-defined functions, input, and random numbers.
- **Shell (`nilgai.lua`):** runs a Nilgai source file, or starts an interactive REPL when no file is given.

## Example

```
$ lua nilgai.lua examples/primes.ngai
Primes up to 100:
2 3 5 7 11 13 17 19 23 29 31 37 41 43 47 53 59 61 67 71 73 79 83 89 97
Total: 25 primes
```

From [`examples/primes.ngai`](examples/primes.ngai), a Sieve of Eratosthenes:

```
def runSieve()
{
    p = 2;
    while (p * p <= limit)
    {
        if (isPrime[p])
        {
            m = p * p;
            while (m <= limit)
            {
                isPrime[m] = false;
                m = m + p;
            }
        }
        p = p + 1;
    }
}
```

## Running It

Requires [Lua](https://www.lua.org/) 5.3 or later.

```
git clone https://github.com/sowens23/nilgai-interpreter.git
cd nilgai-interpreter

lua nilgai.lua examples/fizzbuzz.ngai   # run a program
lua nilgai.lua                          # start the interactive REPL
```

## Project Structure

```
nilgai-interpreter/
├── lexit.lua          # Lexer
├── parseit.lua        # Recursive-descent parser
├── interpit.lua       # AST interpreter
├── nilgai.lua         # File runner and REPL
├── examples/
│   ├── fizzbuzz.ngai  # FizzBuzz: functions, loops, conditionals
│   └── primes.ngai    # Sieve of Eratosthenes: arrays, nested loops, return values
└── docs/
    └── grammar.md     # Lexical rules, grammar, precedence, and semantics
```

## Language Reference

See [docs/grammar.md](docs/grammar.md) for the full grammar, operator precedence, and semantics.

## Credits

The Nilgai language, the shell (`nilgai.lua`), and starter skeletons for the parser and interpreter were provided by Professor Glenn G. Chappell. I implemented the lexer extensions (keywords, string literals, and operators), the recursive-descent parser, the AST interpreter, and the example programs.
