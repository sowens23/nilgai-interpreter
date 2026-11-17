# Nilgai Language Grammar

This document describes the syntax of Nilgai, the language implemented by this interpreter. The lexer (`lexit.lua`) turns source text into lexemes, the parser (`parseit.lua`) checks them against the grammar below and builds an abstract syntax tree (AST), and the interpreter (`interpit.lua`) executes the AST.

## Lexical Structure

| Category | Description | Examples |
| --- | --- | --- |
| Keyword | 16 reserved words | `and` `char` `def` `else` `elseif` `eol` `false` `if` `inputnum` `not` `or` `output` `rand` `return` `true` `while` |
| Identifier | Letter or underscore, followed by letters, digits, or underscores; not a keyword | `limit` `isPrime` `my_var2` |
| Numeric literal | Sequence of digits, with an optional exponent | `0` `123` `7e34` `34567E+1` |
| String literal | Text in matching single or double quotes, on one line | `"Hello"` `'Fizz'` |
| Operator | 14 operators | `==` `!=` `<` `<=` `>` `>=` `+` `-` `*` `/` `%` `[` `]` `=` |
| Punctuation | Any other single non-whitespace character | `;` `{` `}` `(` `)` `,` |
| Malformed | Invalid lexemes, such as an unterminated string | `"no closing quote` |

Comments begin with `#` and continue to the end of the line. Whitespace separates lexemes and is otherwise ignored.

## Grammar

The grammar is written in EBNF:

- `{ x }` means zero or more repetitions of `x`
- `[ x ]` means `x` is optional
- `( a | b )` means a choice between `a` and `b`
- Quoted text is a literal lexeme; `ID`, `NUMLIT`, and `STRLIT` are lexeme categories

```ebnf
program       ->  { statement }

statement     ->  ';'
               |  'output' '(' [ output_arg { ',' output_arg } ] ')' ';'
               |  'return' expr ';'
               |  ID ( '(' ')' | [ '[' expr ']' ] '=' expr ) ';'
               |  'def' ID '(' ')' '{' program '}'
               |  'if' '(' expr ')' '{' program '}'
                      { 'elseif' '(' expr ')' '{' program '}' }
                      [ 'else' '{' program '}' ]
               |  'while' '(' expr ')' '{' program '}'

output_arg    ->  STRLIT
               |  'eol'
               |  'char' '(' expr ')'
               |  expr

expr          ->  compare_expr { ( 'and' | 'or' ) compare_expr }
compare_expr  ->  arith_expr { ( '==' | '!=' | '<' | '<=' | '>' | '>=' ) arith_expr }
arith_expr    ->  term { ( '+' | '-' ) term }
term          ->  factor { ( '*' | '/' | '%' ) factor }

factor        ->  NUMLIT
               |  '(' expr ')'
               |  ( 'true' | 'false' )
               |  ( '+' | '-' | 'not' ) factor
               |  'inputnum' '(' ')'
               |  'rand' '(' expr ')'
               |  ID [ '(' ')' | '[' expr ']' ]
```

## Operator Precedence

From lowest to highest. All binary operators are left-associative.

| Level | Operators |
| --- | --- |
| 1 (lowest) | `and` `or` |
| 2 | `==` `!=` `<` `<=` `>` `>=` |
| 3 | `+` `-` (binary) |
| 4 | `*` `/` `%` |
| 5 (highest) | `+` `-` `not` (unary) |

## Semantics Notes

- **Booleans are integers:** `true` evaluates to `1` and `false` to `0`, and conditions treat any nonzero value as true.
- **Integer arithmetic:** results are converted to integers, so `7 / 2` evaluates to `3`.
- **Division by zero:** `x / 0` and `x % 0` evaluate to `0` instead of raising an error.
- **Functions take no parameters.** Values are passed in and out through variables, and `return` sets a function's return value.
- **`output`** prints its arguments in order with no separators. Use `eol` for a newline and `char(n)` to print the character with code `n`.
- **`inputnum()`** reads a number from standard input, and **`rand(n)`** returns a random integer from `0` to `n - 1`.

## AST Node Types

The parser produces these node types for the interpreter:

| Statements | Output | Expressions |
| --- | --- | --- |
| `PROGRAM` | `STRLIT_OUT` | `BIN_OP` |
| `EMPTY_STMT` | `EOL_OUT` | `UN_OP` |
| `OUTPUT_STMT` | `CHAR_CALL` | `NUMLIT_VAL` |
| `RETURN_STMT` | | `BOOLLIT_VAL` |
| `ASSN_STMT` | | `INPUT_CALL` |
| `FUNC_CALL` | | `RAND_CALL` |
| `FUNC_DEF` | | `SIMPLE_VAR` |
| `IF_STMT` | | `ARRAY_VAR` |
| `WHILE_LOOP` | | |

*The Nilgai language was designed by Professor Glenn G. Chappell for CS 331 at the University of Alaska Fairbanks.*
