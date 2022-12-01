# Yuescript-P8

Yuescript-P8 is a Yuescript dialect that compiles to PICO-8 Lua. Yuescript is derived from [Moonscript language](https://github.com/leafo/moonscript) 0.5.0 and continuously adopting new features to be more up to date.

Moonscript is a language that compiles to Lua. Since original Moonscript has been used to write web framework [lapis](https://github.com/leafo/lapis) and run a few business web sites like [itch.io](https://itch.io) and [streak.club](https://streak.club) with some large code bases. The original language is getting too hard to adopt new features for those may break the stablility for existing applications.

So Yuescript is a new code base for pushing the language to go forward and being a playground to try introducing new language syntax or programing paradigms to make Moonscript language more expressive and productive.

Yue (月) is the name of moon in Chinese and it's pronounced as [jyɛ].

## Features

Yuescript-P8's API is similar to regular Yuescript, although it features several key differences.

* The `target` feature has been removed from the compiler, as it is tuned to PICO-8's Lua only.
* The `^^`, `>>>`, `<<>`, and `>><` operators are now supported.
  * Their update assignment counterparts are also supported (i.e. `^^=`).
* `0b` prefix is now supported for binary numbers.
* Decimal exponents (`e`/`E`) and hexadecimal exponents (`p`/`P`) are no longer supported because PICO-8's Lua does not support them.
* New `` ` ``, ``` `` ```, and ```` ``` ```` operators that compile to the memory operators `@`, `%`, and `$` respectively.
* `\` is no longer a chain operator, because it is used as an operator for floor division in PICO-8 Lua. Use `::` instead.
  * `\=` can now be used for update assignment.
  * The `//` operator is no longer supported, as it is replaced by `\` in PICO-8 Lua.
* Unassigned values in a multiple-assignment expression are now allowed, which helps save tokens.
* Implicit returns are no longer generated to help save tokens after compilation.
  * Returns must now be explicitly stated, like Lua.
  * The compiler option for implicit root returns has been removed as well.

Additionally, the following changes have been made to Lua code generation (transparent to the API):

* PICO-8 Lua supports update assignment operators, so they are carried over from Yuescript code instead of being expanded.
* Class generation code has been tweaked for compatibility.
* String expressions (`#{}`) are now generated with `tostr` instead of `tostring`.
* The `!=` operator is now carried over instead of being converted to `~=`, as it is supported in PICO-8 Lua.

### Yuescript Features

* No other dependencies needed except modified **parserlib** library from Achilleas Margaritis with some performance enhancement. **lpeg** library is no longer needed.
* Written in C++17.
* Support most of the features from Moonscript language. Generate Lua codes in the same way like the original compiler.
* Reserve line numbers from source file in the compiled Lua codes to help debugging.
* More features like macro, existential operator, pipe operator, Javascript-like export syntax and etc.
* See other details in the [changelog](./CHANGELOG.md). Find document [here](http://yuescript.org).



## Installation & Usage

* **Lua Module**

&emsp;&emsp;Build `yue-p8.so` file with

```sh
> make shared LUAI=/usr/local/include/lua LUAL=/usr/local/lib/lua
```

&emsp;&emsp;Then get the binary file from path `bin/shared/yue-p8.so`.

&emsp;&emsp;Then require the Yuescript module in Lua:



* **Binary Tool**

&emsp;&emsp;Clone this repo, then build and install executable with:
```sh
> make install
```

&emsp;&emsp;Build Yuescript tool without macro feature:
```sh
> make install NO_MACRO=true
```

&emsp;&emsp;Build Yuescript tool without built-in Lua binary:
```sh
> make install NO_LUA=true
```

&emsp;&emsp;Use Yuescript tool with:

```sh
> yue-p8 -h
Usage: yue-p8 [options|files|directories] ...

   -h       Print this message
   -e str   Execute a file or raw codes
   -m       Generate minified codes
   -t path  Specify where to place compiled files
   -o file  Write output to file
   -s       Use spaces in generated codes instead of tabs
   -p       Write output to standard out
   -b       Dump compile time (doesn't write output)
   -g       Dump global variables used in NAME LINE COLUMN
   -l       Write line numbers from source codes
   -w path  Watch changes and compile every file under directory
   -v       Print version
   --       Read from standard in, print to standard out
            (Must be first and only argument)

   --target=version  Specify the Lua version that codes will be generated to
                     (version can only be 5.1, 5.2, 5.3 or 5.4)
   --path=path_str   Append an extra Lua search path string to package.path

   Execute without options to enter REPL, type symbol '$'
   in a single line to start/stop multi-line mode
```
&emsp;&emsp;Use cases:
&emsp;&emsp;Recursively compile every Yuescript file with extension `.yue` under current path:  `yue-p8 .`
&emsp;&emsp;Compile and save results to a target path:  `yue-p8 -t /target/path/ .`
&emsp;&emsp;Compile and reserve debug info:  `yue-p8 -l .`
&emsp;&emsp;Compile and generate minified codes:  `yue-p8 -m .`



## Editor Support

Yuescript-P8 does not currently have any dedicated extensions for editors, but since the API is almost entirely the same as the original Yuescript, you can use its extensions.

* [Vim](https://github.com/pigpigyyy/Yuescript-vim)
* [ZeroBraneStudio](https://github.com/pkulchenko/ZeroBraneStudio/issues/1134) (Syntax highlighting)
* [Visual Studio Code](https://github.com/pigpigyyy/yuescript-vscode)

## License

MIT
