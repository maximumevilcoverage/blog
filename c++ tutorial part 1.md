First written: 28 April 2018  
Last full-check: 28 April 2018  
Last updated: 4 May 2018  

Estimated reading time: 10 minutes  
<!--more-->
- [Disclaimer](#disclaimer)
- [About](#about)
- [Compiler flags](#compiler-flags)
- [The C++ compilation model](#the-c-compilation-model)
  - [1. Preprocessing](#1-preprocessing)
    - [Introduction](#introduction)
    - [The cpp preprocessor](#the-cpp-preprocessor)
    - [Phases 1-3](#phases-1-3)
    - [Object-like macros](#object-like-macros)
    - [Macro expansion](#macro-expansion)
    - [Naming conventions](#naming-conventions)
    - [Function-like macros](#function-like-macros)
      - [Pitfall 1: Operator precedence](#pitfall-1-operator-precedence)
      - [Pitfall 2: Side effect duplication](#pitfall-2-side-effect-duplication)
    - [Macro use cases](#macro-use-cases)
      - [Debug mode and message logging](#debug-mode-and-message-logging)
      - [Conditional compilation](#conditional-compilation)
      - [Convenience](#convenience)
      - [Stringizing](#stringizing)
      - [Concatenation](#concatenation)
    - [File inclusion.](#file-inclusion)
  - [2. Compilation, linking, and loading](#2-compilation-linking-and-loading)
    - [Types of ELF files](#types-of-elf-files)
    - [Static vs dynamic linker](#static-vs-dynamic-linker)
    - [Static vs dynamic linking](#static-vs-dynamic-linking)
      - [Relative RPATH vs $ORIGIN](#relative-rpath-vs-origin)
    - [Position Independent Code](#position-independent-code)
    - [dlopen](#dlopen)
    - [ODR violation](#odr-violation)
    - [The inline keyword](#the-inline-keyword)
    - [Subtle ODR violations with inline functions](#subtle-odr-violations-with-inline-functions)
    - [Other ODR exemptions](#other-odr-exemptions)
- [Basic concepts](#basic-concepts)
  - [Declarations and Definitions](#declarations-and-definitions)
  - [Scope, Linkage, Lifetime, and Storage Duration](#scope-linkage-lifetime-and-storage-duration)
    - [Linkage](#linkage)
    - [Scope](#scope)
      - [Name hiding](#name-hiding)
      - [Block scope](#block-scope)
      - [Namespace scope](#namespace-scope)
        - [Namespaces](#namespaces)
      - [Class scope](#class-scope)
      - [Function scope](#function-scope)
      - [Other scopes](#other-scopes)
      - [Some funny stuff](#some-funny-stuff)
    - [Storage duration](#storage-duration)
    - [Storage class specifiers](#storage-class-specifiers)
    - [auto](#auto)
- [Basic knowledge](#basic-knowledge)
  - [Extraction operators](#extraction-operators)
  - [The main function](#the-main-function)
  - [Naming](#naming)
  - [Narrowing conversion](#narrowing-conversion)
  - [Overflow](#overflow)
  - [Fundamental Integral Types](#fundamental-integral-types)
  - [Integer promotion rules](#integer-promotion-rules)
  - [Pre and post increment](#pre-and-post-increment)
  - [Order of evaluation](#order-of-evaluation)
    - [Some useful things to know](#some-useful-things-to-know)
    - [Proposal P0145 - "but I have heard it works even if you don't believe in it"](#proposal-p0145---but-i-have-heard-it-works-even-if-you-dont-believe-in-it)
  - [Operator precedence](#operator-precedence)
  - [Literals](#literals)
    - [String literals](#string-literals)
  - [standard IO](#standard-io)
    - [IOstream manipulators](#iostream-manipulators)
  - [Literals](#literals)
  - [Arrays vs pointers](#arrays-vs-pointers)
    - [Pointer to array vs pointer to first element of array](#pointer-to-array-vs-pointer-to-first-element-of-array)
    - [Commutativity of the indexing operator](#commutativity-of-the-indexing-operator)
    - [Multidimensional arrays](#multidimensional-arrays)
    - [Arrays of pointers](#arrays-of-pointers)
    - [Aggregate initialization](#aggregate-initialization)
  - [Temporaries](#temporaries)
    - [Taking a reference to a temporary](#taking-a-reference-to-a-temporary)
  - [Floating points](#floating-points)
  - [Alternative operator representations](#alternative-operator-representations)
  - [Checking if a bit is set](#checking-if-a-bit-is-set)
  - [Bitwise operations on signed integers](#bitwise-operations-on-signed-integers)
  - [Switch case](#switch-case)
  - [Structs vs classes](#structs-vs-classes)
  - [Storing a class member of the same type as the class itself](#storing-a-class-member-of-the-same-type-as-the-class-itself)
  - [More on pointers](#more-on-pointers)
    - [Examining memory directly](#examining-memory-directly)
    - [Manually setting pointers](#manually-setting-pointers)
    - [Valid operations on pointers](#valid-operations-on-pointers)
      - [Random access iterators vs forward iterators](#random-access-iterators-vs-forward-iterators)
  - [Default parameters](#default-parameters)
  - [Function overloading](#function-overloading)
    - [Overload resolution ambiguity](#overload-resolution-ambiguity)
  - [The new operator](#the-new-operator)
  - [Dynamic arrays](#dynamic-arrays)
    - [Memory allocation overhead](#memory-allocation-overhead)
    - [Vector reallocation and FBVector](#vector-reallocation-and-fbvector)
  - [std::find and std::map::find](#stdfind-and-stdmapfind)
  - [Type conversion](#type-conversion)
  - [String operations](#string-operations)
    - [Substring](#substring)
  - [Static initialization](#static-initialization)
  - [Pass and return by value](#pass-and-return-by-value)
    - [lvalues and rvalues](#lvalues-and-rvalues)
    - [Move semantics](#move-semantics)
    - [Pass by value vs pass by reference](#pass-by-value-vs-pass-by-reference)
    - [Return by value vs pass by reference](#return-by-value-vs-pass-by-reference)
  - [Enums](#enums)
    - [Enum classes](#enum-classes)
  - [I/O](#io)
  - [Const](#const)
    - [Reading const declarations](#reading-const-declarations)
    - [Const infection](#const-infection)
- [Inheritance](#inheritance)
  - [`final` and `override`](#final-and-override)
  - [Member functions](#member-functions)
    - [Virtual functions, object slicing, and polymorphism](#virtual-functions-object-slicing-and-polymorphism)
      - [Calling a virtual function from a constructor or destructor](#calling-a-virtual-function-from-a-constructor-or-destructor)
  - [Member access](#member-access)
  - [Constructors](#constructors)
    - [Most vexing parse](#most-vexing-parse)
    - [Derived default constructor](#derived-default-constructor)
  - [Accessibility of members](#accessibility-of-members)
  - [C++ object model](#c-object-model)
  - [this](#this)
  - [Copy constructor and copy elision](#copy-constructor-and-copy-elision)
    - [Derived class copy constructor](#derived-class-copy-constructor)
    - [Why does the copy constructor take a reference](#why-does-the-copy-constructor-take-a-reference)
  - [Destructors](#destructors)
    - [Why you should always have a virtual destructor in all your classes](#why-you-should-always-have-a-virtual-destructor-in-all-your-classes)
  - [Copy initialization vs direct initialization](#copy-initialization-vs-direct-initialization)
  - [Friends](#friends)
- [Operator overloading](#operator-overloading)
  - [Overloadable operators](#overloadable-operators)
  - [Design of overloaded operators](#design-of-overloaded-operators)
  - [How to invoke overloaded operators recursively](#how-to-invoke-overloaded-operators-recursively)
- [Exceptions](#exceptions)

# Disclaimer

I don't endorse this tutorial because at the time of writing this tutorial, my C++ knowledge was at the pre-beginner level, so it was mainly a way to help me learn C++ (on the other hand, that means this tutorial is accessible to pre-beginners, so perhaps it is good for that audience). I would instead recommend that you read a Tour of C++ and C++ Primer 5th Edition (or PPP2 instead of Tour of C++ if you have literally zero programming experience). I have striven to ensure that everything written here is correct, so if you find anything incorrect **please** notify me of it. I will probably not add new material to this, but if you think there is something important I missed out please tell me about it, and I may add it in Part 2. Also, please understand that this is a **legality** guide only. If you want a **morality** guide, read Effective C++ (and similar books/articles by Meyers and Sutter, as well as articles on the ISOCPP FAQ, Dr.Dobbs, InformIT, various talks and blogs etc). Also, please do not use this as a reference, only the C++ Standard is authoritative. Nothing else - not even The C++ Programming Language - can be considered authoritative. And definitely don't take "it compiles" to mean "it's valid C++". As [Steve Summit](https://stackoverflow.com/questions/949433/why-are-these-constructs-using-undefined-behavior-in-c/2897275#comment58584601_2897275) puts it:

>For the record, if for whatever reason you're wondering what a given construct does -- and especially if there's any suspicion that it might be undefined behavior -- the age-old advice of "just try it with your compiler and see" is potentially quite perilous. You will learn, at best, what it does under this version of your compiler, under these circumstances, today. You will not learn much if anything about what it's guaranteed to do. In general, "just try it with your compiler" leads to nonportable programs that work only with your compiler.

Nothing can substitute for full understanding of the C++ Standard. Unfortunately the C++ Standard is not designed to be read by "normal people" (lowly non-compiler-writer peons like myself), so, depending on your purposes, an incomplete understanding from reading proposals and books like TC++PL may be "good enough". 

Another warning: Reading bits of cppreference and the C++ Standard may cause you to think you know more than you really do about C++. But knowing the rules doesn't mean you know anything about how to write good code. You need to know how to use the standard library effectively, you need to know C++ idioms (not to mention design patterns) and best practices (Meyers and Sutter are good but far from enough). You need to read a lot of code and see different ways to do things and be able to know which methods are better in what situations, you need to read code written by really good programmers and know why they did it that way and not some other way. Principles are more important than specific knowledge, but you need to have specific knowledge to know how to apply the principles in specific situations. That doesn't even cover knowledge of "hardware"/architecture (e.g [cache friendliness](https://stackoverflow.com/questions/16699247/what-is-a-cache-friendly-code), [memory](https://people.freebsd.org/~lstewart/articles/cpumemory.pdf)), design patterns, object-oriented design, data structures and algorithms (and algorithmic analysis, proofs etc), or system design, or any number of other things that a good programmer needs to know to write good code in general (not just C++). There are lots of things I've probably missed out here because I'm still at the pre-beginner level and I don't know what I don't know. 

Reading this tutorial will not make you a good C++ programmer, not even a beginner. After reading through this tutorial, you will have (by an optimistic estimate) learned 5% of the knowledge required to be called a beginner at C++ (I have missed out so many important things like STL, templates, exceptions etc), because of how inefficiently I have packed in the information in this tutorial it seems like it contains more content than it really does. You should instead read C++ Primer 5th Edition (and do all the exercises), and Effective C++, Effective Modern C++, and Effective STL. Once you have finished those then you will be at beginner level. 

# About

This "tutorial" is really just the typing up of a bunch of old C++ notes I made when I was still learning C++. Took me about 2 weeks to type this up and I know it's really incomplete but I just felt it would be a waste to not share it with other people. 

# Compiler flags

You should use clang++ with the `-Weverything` and `-std=c++17` flags, then remove the flags that you don't need such as with `-Wno-c++98-compat`. If you are feeling like a good boy then you should also enable `-Werror`, it will probably save you some pain and misery down the line. GCC AFAIK does not have an equivalent (`-Wall` is not equivalent to `-Weverything`) so you have to manually enable all the warnings, which is frankly just a PITA. It's probably a good idea to make sure your C++ code compiles under both compilers (and exhibits the same behavior) at least to have more confidence that it's portable. 

# The C++ compilation model

The C++ compilation phase consists of (depending on how detailed you want to be) 3 broad phases: preprocessing, compilation, and linking. 

## 1. Preprocessing

### Introduction

Each C/C++ compiler seems to come with its own preprocessor. GCC internally uses the `cc1` compiler for C and `cc1plus` for C++, both for preprocessing and compilation. I believe that `cpp` is a front-end for `cc1` or `cc1plus`'s preprocessor. If you ever wanted to know, here's how I went about finding out (keep in mind I'm a complete noob at Linux. If you want more Linux knowledge I have seen Advanced Linux Programming recommended):

`strace -fe execve -o /tmp/strace.log cpp main.cpp`

Here we are invoking `strace` with `-e trace=execve`, which tells it to trace only the execve() call. AFAIK all calls in the exec* family use the `sys_execve` system call, which is represented as `execve` by strace (so if your program calls execv or execl, in the strace output you will see execve and not execv or execl). Example:

Compile the following C code to a.out
```c
  #include <sys/types.h>
  #include <unistd.h>
  #include <stdio.h>

  main()
  {
     pid_t pid;
     char *const parmList[] = {"/bin/ls", "-l", "/u/userid/dirname", NULL};

     if ((pid = fork()) == -1)
        perror("fork error");
     else if (pid == 0) {
        execv("/bin/ls", parmList);
        printf("Return not expected. Must be an execv error.n");
     }
  }
```
Now run strace on it:
```
strace -f ./a.out 2>&1 | grep "exec"
```
Output:
```
execve("./a.out", ["./a.out"], [/* 72 vars */]) = 0
[pid #number] execve("/bin/ls", ["/bin/ls", "-l", "/u/userid/dirname"], [/* 72 vars */] <unfinished ...>
<... execve resumed> )                  = 0
```

Okay so back to the original command:
```
strace -fe trace=fork,vfork,clone,execve cpp main.cpp > /dev/null
```
Output:
```
execve("/usr/bin/cpp", ["cpp", "main.cpp"], [/* 72 vars */]) = 0
vfork([stuff]
execve("[stuff]/cc1plus"
```

Here we see that the first execve call is the call to cpp itself, and then we see that the vfork() system call is used instead of fork() or clone(). We need the -f flag to tell strace to trace child processes created by the current process as well (since it is the child that calls exec and not the parent). 

Now we can show that cpp will call cc1 or cc1plus depending on whether if the input file ends in `.c` or `.cpp`, as follows:

```
cp main.cpp main.c
strace -fe trace=fork,vfork,clone,execve cpp main.cpp > /dev/null
```
Output:
```
execve("/usr/bin/cpp", ["cpp", "main.c"], [/* 72 vars */]) = 0
vfork([stuff]
execve("[stuff]/cc1"
```

We can see that cpp calls cc1 or cc1plus depending the type of the file. I have seen it said somewhere that cpp is in fact just a front end for cc1plus (or just cc1 if it's a C rather than C++ program) which is where all the preprocessing and compilation happens in gcc/g++, which suggests to me that the output of cpp should always be the same as the output of gcc/g++'s preprocessor. So you could probably use `cpp` interchangeably with `g++ -E` (-E is the flag that causes the compiler to spit out preprocessor output, at least in both GCC and Clang) for seeing the preprocessor output. Different compilers may produce different preprocessor outputs (different linemarkers and such).

### The cpp preprocessor

There are indeed differences (albeit small) between the C and C++ preprocessor "languages". One way to distinguish between C and C++ preprocessor is to use the following code which is valid C++ but not C:

```cpp
#if 1 and 2
#endif
```

The reason this works is because in C++, "and" is an operator, and this is recognized by the C++ preprocessor, whereas it is not recognized by the C preprocessor because "and" is not a keyword in C. Just save that in a file, let's call it `program.cpp`. Copy it to `program.c` for good measure. 

Now, if we want to get the preprocessor output for these files, we can run any of these commands:

```bash
cpp program.cpp
g++ -E program.cpp
clang++ -E program.cpp
```

You may have noticed that g++ and clang++ seem to have produced slightly different output, which consist of lines that look like this:

```cpp
 1 "program.cpp"
```

This is called a linemarker and is only used for the compiler to generate error messages, it doesn't actually affect the code at all. The leftmost number is the line number, the string after it is the filename. You can get rid of these lines by passing the -P flag to cpp, clang++ -E or g++ -E. 

Now, we wanted to be able to distinguish between C and C++ preprocessors with our file. So we can do this:

```bash
cpp program.cpp
cpp program.c
cpp -x c++ program.c
```

You will notice that the 1st and 3rd commands run fine whilst the 2nd produces an error. This is due to cpp preprocessing program.c as a C file rather than as a C++ file because of its .c extension (just like how it preprocessed program.cpp as a C++ file because of its .cpp extension). On the 3rd line we explicitly told cpp to preprocess program.c as a C++ file using the -x flag, so there was no error message produced. If you do not pass it the -x flag then it uses the file extension to determine whether to preprocess it as C or C++ file. [Read more about cpp here](https://linux.die.net/man/1/cpp). 

### Phases 1-3

In fact, [according to cppreference](http://en.cppreference.com/w/cpp/language/translation_phases) there are 3 code transformation stages which are run **before** the preprocessor is run. These are pretty simple though, so I'll just briefly describe them here for the sake of completeness. 

1. The individual bytes of the source code are mapped to characters. Pretty simple. In previous versions of C++, trigraphs would be translated at this point, but they were removed in C++17 😞. One less obscure language feature to troll people with. What a darn shame. 

2.  Backslash-newline sequences are deleted, combining multiple lines each separated by a backslash-newline sequence into a single line. We can test it out here:

    ```cpp 
    in\
    t m\
    ain()
    {
        ch\
    ar* \
        a = "hah\
    aha this i\
    s the bomb l\
        ol\
        ";
    }
    ```
  
    Running cpp on this produces:

    ```cpp
    int
      main()
    
    {
        char*
    
        a = "hahaha this is the bomb l    ol    ";
    
    
    
    
    }
    ```
    Notice how "is" was joined back into a word but "lol" was not? Because only the backslash-newline sequence is deleted, the other spaces in the program source code remain, separating the "l" from "ol". This means if you want a word broken over several lines to stay a word, you have to make sure the parts of the word are separated **only** by the backslash-newline and nothing else - no whitespace for indentation. I'm not sure why cpp put int and main on separate lines whilst the clang preprocessor put both on the same line. It doesn't matter though because whitespace (which includes newlines) is ignored by the compiler (see phase 7 in C++17 standard: `White-space characters separating tokens are no longer significant`). Most important thing is the string literal is combined into one line since C++ does not allow multiline string literals (which is why we have to resort to using hacks like the backslash-newline in the first place).

    If we use cpp with the -P option then the following output is produced instead:
    ```cpp
    int main()
    {
        char* a = "hahaha this is the bomb l    ol    ";
    }
    ```
    Now the extraneous newlines and linemarkers are gone. This is semantically equivalent to the code from before, just looks cleaner. You may see a different result from clang. 

    Note that the use of the backslash-newline may lead to surprising line numbers in error messages.

3. Next, comments are replaced by a single whitespace character and the file is tokenized. Here's the C++17 standard, section 5.2 phase 3:

    <blockquote>The source file is <strong>decomposed into preprocessing tokens (5.4) and sequences of white-space characters (including comments)</strong>. A source file shall not end in a partial preprocessing token or in a partial comment.13 <strong>Each comment is replaced by one space character.</strong> New-line characters are retained. Whether each nonempty sequence of white-space characters other than new-line is retained or replaced by one space character is unspecified. The process of dividing a source file’s characters into preprocessing tokens is context-dependent. [ Example: see the handling of &lt; within a #include preprocessing directive. — end example ]</blockquote>

    Note that phase 3 does **not** strip newlines. One reason is so that the line numbers in the linemarkers are still valid. I would guess that another reason is that it would fuck up the preprocessor directives since each directive has to be newline-terminated.
    
### Object-like macros

After these 3 phases, we have phase 4, which is when preprocessor directives are actually executed. It is important to remember that the preprocessor works sequentially through the code on a line-by-line basis (remember that newlines are significant for the preprocessor, but not for the C/C++ compiler). It seems that the phases are designed so that preprocessing can be done in a single pass through the source code. In other words, the following code compiles:
```cpp
int main(){
#define foo 1
int bar = foo;
}
```
But the follow code does not:
```cpp
int main(){
int bar = foo;
#define foo 1
}
```
We can imagine that the preprocessor does a single pass through the code, line by line, and when it sees a #define statement, it stores the definition for that symbol in a symbol table somewhere, and when it sees that symbol later on in the text then it replaces the symbol with its definition. This is a 100% textual substitution (well, almost), it is completely unaware of C++ language rules. But, it is not the same as a find-replace in a text editor. So for example:
```cpp
#include <iostream>
int main(){
  #define s std
  #define c cout
  #define e 1
  #define hello 1.1 +
  #define world 2
  s::c << "e e e";
  s::c << hello world;
}
```
Running cpp -P on this gives us:
```cpp
...omitted a bunch of stuff from <iostream>...
int main(){
  std::cout << "e e e";
  std::cout << 1.1 + 2;
}
```
This prints:
```
e e e3.1
```
Notice how the "e" characters inside the string literal are not replaced. This illustrates that the C/C++ preprocessor is in fact not a straightforward find/replace, but in fact operates on the token level. Remember earlier I mentioned that the text is `decomposed into preprocessing tokens`? There are different types of preprocessing tokens, such as literals, identifiers, keywords, and operators. The full list of preprocessing tokens may be found in section 5.4 of the C++17 standard. The full list of preprocessing directives may be found in section 19 of the C++17 standard. There is actually a bunch of different `#define`s. Here is one of them:
```
 define identifier replacement-list new-line
```
This explains what we saw earlier, which is that the e inside string literals and identifiers are not replaced by 1, but that "whole words" i.e "hello" and "world" were replaced - because "hello" and "world" are identifiers but the "e" inside the string literals and inside "hello" are not identifiers. Notice that "s::c" was decomposed into 3 tokens, "s" and "c" are both identifiers whilst "::" is what the standard calls a "preprocessing-op-or-punc".  

In case you're wondering, a `replacement-list` is anything that decomposes into valid preprocessing tokens. It does not have to be valid C++ code. In particular, parentheses may be unbalanced, which can be convenient in many situations, such as competitive programming, code golf, and International Obfuscated C Code Contests (very important). 

It is easy to write illegal code using #defines that compiles and runs. For example, the following code compiles in gcc 8 and clang 7:
```cpp
#include <iostream>
#include <cstdio>
using namespace std;
int main(){
  #define constexpr unsigned
  #define for while
  #define cout printf
  constexpr short x = -1;
  cout("%d",x);
  for(x!=10) {
      x+=1;
      cout(" %d",x);
  }
}
```
And outputs:
```
65535 0 1 2 3 4 5 6 7 8 9 10
``` 
In C++17 standard section 20.5.4.3.2 Macro names it says:

>1 A translation unit that includes a standard library header shall not #define or #undef names declared in any standard library header.
>
>2 A translation unit shall not #define or #undef names lexically identical to keywords, to the identifiers listed in Table 4, or to the attribute-tokens described in 10.6.

What the hell is a "translation unit"? Here is the relevant paragraph from The C++ Programming Language 4th Edition:

>A user presents a source file to the compiler. The file is then preprocessed; that is, macro processing (§12.6) is done and #include directives bring in headers (§2.4.1, §15.2.2). **The result of preprocessing is called a translation unit. This unit is what the compiler proper works on and what the C++ language rules describe.**

So although the code above compiles in gcc 8 and clang 7 and runs as expected, it is actually (doubly) illegal because it violates both of the 2 rules: #define cout printf defines a name declared in a standard library header, whilst the #define for while defines a keyword. This just goes to show how easy it is to write illegal C++ code that compiles and runs normally. This is a theme we will often revisit in this tutorial. 

### Macro expansion

The rules for macro expansion are quite involved and I won't go into depth in this tutorial. Instead I'll just use some simple examples to illustrate the basics:
```cpp
#define TWO THREE
#define ONE TWO
ONE
TWO
THREE
```
Running cpp -P on this produces this:
```
THREE
THREE
THREE
```
So the first thing to remember about macro expansion is that macro expansion is recursive. After reading the #define directives, the preprocessor sees the identifier ONE, and notices that it can be expanded into TWO. But it doesn't stop there, it then checks to see if TWO can be expanded, and indeed it can be, into THREE. And THREE can't be expanded, so it is left as is. Then it does the same for the next 2 tokens. 

Now, what happens when we have circular define macro dependencies? 
```cpp
#define THREE ONE
#define TWO THREE
#define ONE TWO
ONE
TWO
THREE
```
This expands to:
```
ONE
TWO
THREE
```
Amazingly, the preprocessor does not go into infinite recursion and in fact just leaves the macros as they are. What about this?
```cpp
#define THREE TWO
#define TWO THREE
#define ONE TWO
ONE
TWO
THREE
```
Expands to:
```
TWO
TWO
THREE
```
It would appear that when an identifier is expanded back into itself, then it stops being expanded. So ONE is expanded into TWO, then TWO is expanded into THREE, then THREE is expanded back into TWO, and since we were already expanding TWO, we stop. The [GCC reference](https://gcc.gnu.org/onlinedocs/cpp/Object-like-Macros.html) had this to say:
>If the expansion of a macro contains its own name, either directly or via intermediate macros, it is not expanded again when the expansion is examined for more macros. This prevents infinite recursion.

### Naming conventions

By convention, macro names are written in uppercase, whereas other names are not. **You should definitely follow this rule**. Stroustrup has some quite sensible suggestions on C++ naming conventions in [his FAQ here](http://www.stroustrup.com/bs_faq2.html#Hungarian). In C/C++ because of how macros work you can easily accidentally override library functions with your own (e.g. `#define printf`), that's why functions are never fully capitalized whilst macros are always fully capitalized. Nowadays IDEs with proper code highlighting can actually understand C++ (to some extent) and thus highlight macros and functions differently, so this is probably not as important. Similarly some people use the code convention where they name member variables differently from non-member variables. With IDE code highlighting this is again perhaps less important. Also, m_ is a human-enforced convention, just because a variable's name starts with m_ does not necessarily mean it is a member variable, whereas if your IDE highlights a variable as a member variable then you have more assurance that it is a member variable, and the same applies to macro and function names. Whilst m_ is not compiler-enforced, this-> is. You may look to the [C++ Core Guidelines](https://github.com/isocpp/CppCoreGuidelines) for some ideas for how to name things. 

### Function-like macros

There are in fact two types of `#define` directives - the [object-like macros](https://gcc.gnu.org/onlinedocs/cpp/Object-like-Macros.html) we discussed earlier, and the [function-like macros](https://gcc.gnu.org/onlinedocs/cpp/Function-like-Macros.html) which are much more interesting. So, first thing to note with function-like macros is that you have to have parentheses immediately following the macro name, if there is a space between the macro name and the parenthesis then it's treated as an object-like macro and not a function-like macro. Again the GCC reference has a very good intro page on this. Here's my abridged version:


>Function-like macros can take arguments, just like real functions. To define a macro that uses arguments, you insert parameters between the pair of parentheses in the macro definition that make the macro function-like. The number of arguments you pass to a function-like macro invocation must match the number of parameters in the macro definition. When the macro is expanded, each use of a parameter in its body is replaced by the tokens of the corresponding argument. (You need not use all of the parameters in the macro body.)
>
>As an example, here is a macro that computes the minimum of two numeric values, as it is defined in many C programs, and some uses.
>
>```cpp
>#define min(X, Y)  ((X) < (Y) ? (X) : (Y))
>  x = min(a, b);          →  x = ((a) < (b) ? (a) : (b));
>  y = min(1, 2);          →  y = ((1) < (2) ? (1) : (2));
>  z = min(a + 28, *p);    →  z = ((a + 28) < (*p) ? (a + 28) : (*p));
>```
>Leading and trailing whitespace in each argument is dropped, and all whitespace between the tokens of an argument is reduced to a single space. Parentheses within each argument must balance; a comma within such parentheses does not end the argument. However, there is no requirement for square brackets or braces to balance, and they do not prevent a comma from separating arguments. Thus,
>
>```cpp
>macro (array[x = y, x + 1])
>```
>
>passes two arguments to macro: `array[x = y` and `x + 1]`. If you want to supply `array[x = y, x + 1]` as an argument, you can write it as `array[(x = y, x + 1)]`, which is equivalent C code.

A word of warning: you cannot have an object-like macro with the same name as a function-like macro. Example:

```cpp
#define A(x) x
#define A 1
A(aha)
```
Running cpp on this produces:
```
1(aha)
```

You may see a warning message `warning: "A" redefined`. 

#### Pitfall 1: Operator precedence
A common pitfall with macros is operator precedence:

```cpp
#define ADD(a, b) a + b
int x = 2 * ADD(2, 3);
```
preprocessor output:
```cpp
int x = 2 * 2 + 3;
```
So x is set to 7 instead of 10 as we would expect. Remember that function-like macros are not actually functions. One rule for making macros safer is to always ensure the parameters in the macro body are enclosed in parentheses, in order to avoid the situation above, like this: 

```cpp
#include <iostream>
#define ADD(a, b) (a + b)
int main(){
  int x = 2 * ADD(2 << 1, 3);
  std::cout << x << " ";
  x = 2 * ADD(4, 3);
  std::cout << x;
}
```
preprocessor output:
```cpp
...bunch of stuff...
int main(){
  int x = 2 * (2 << 1 + 3);
  std::cout << x << " ";
  x = 2 * (4 + 3);
  std::cout << x;
}
```
Output from running the program:
```
64 14
```
Huh? <code>2 << 1</code> is 4, so ADD(2 <code><<</code> 1, 3) should be the same as ADD(4, 3), yet the output shows that it is not. What happened here is that, because the <code><<</code> (bitwise left shift) operator has lower precedence than the addition operator, 2 <code><<</code> 1 + 3 actually means 2 <code><<</code> 4, which is 32. In order to make ADD behave as we would expect of a normal C++ function, we have to do this:

```cpp
#define ADD(a, b) ((a) + (b))
```

We need to put parentheses around every parameter in the macro body as well as the macro body itself. You can check for yourself that it solves the problem. As far as I know, this method avoids all the operator precedence problems. That does not mean it is always safe to use, because there are other pitfalls. 

#### Pitfall 2: Side effect duplication

This one is pretty straightforward. I will quote the [GCC docs here](https://gcc.gnu.org/onlinedocs/cpp/Duplication-of-Side-Effects.html) again:

>Many C programs define a macro min, for “minimum”, like this:
>
>`#define min(X, Y)  ((X) < (Y) ? (X) : (Y))`
>
>When you use this macro with an argument containing a side effect, as shown here,
>
>`next = min (x + y, foo (z));`
>
>it expands as follows:
>
>`next = ((x + y) < (foo (z)) ? (x + y) : (foo (z)));`
>
>where x + y has been substituted for X and foo (z) for Y.
>
>The function foo is used only once in the statement as it appears in the program, but the expression foo (z) has been substituted twice into the macro expansion. As a result, foo might be called two times when the statement is executed. If it has side effects or if it takes a long time to compute, the results might not be what you intended.

I think a reasonable conclusion to draw from this might be that you should never pass a side-effectful expression into a macro call. The GCC page suggests using GNU extensions but I would not recommend that, certainly it is not the way to go if you want to write portable, standards-compliant code like a good boy (similarly I would not recommend using clang extensions or MSVC extensions etc; you should always compile your code with the disable-extensions flag). 

There are other macro pitfalls which you can find [here](https://gcc.gnu.org/onlinedocs/cpp/Macro-Pitfalls.html), and I'm sure that's not an exhaustive list. 

### Macro use cases

Macros are really dangerous and tricky to use correctly, so what the hell are they actually useful for? 

Firstly, include guards are necessary because we have to use #includes (<span style="color:red">**at least until modules are introduced**</span>) because of the way the C++ compilation model works. This will be explained in the section on compilation (titled `2. Compilation, linking, and loading`). 

Other use cases are for logging debug messages, conditional compilation, "convenience" macros such as foreach loops as well as very repetitive code (e.g. exception handling code) that can't be placed in functions. There's a good [stackoverflow link here](https://stackoverflow.com/questions/96196/when-are-c-macros-beneficial). 

#### Debug mode and message logging

The following macro is useful for logging debug messages:

```cpp
#ifdef ( DEBUG )
#define LOG( msg )  std::cout << __FILE__ << ":" << __LINE__ << ": " << msg
#else
#define LOG( msg )
#endif
``` 

So, how does it work? `__FILE__` and `__LINE__` are predefined macros defined by the C++ language standard, meaning you can rely on them to be available across all good (standards-compliant) compilers. The `__FILE__` macro expands into a string literal containing the **`path by which the preprocessor opened the file, not the short name specified in ‘#include’ or as the input file name argument`**. It would be easiest to explain with some examples. If you have a file named `name.cpp` with the contents:
```cpp
#include <iostream>
#define LOG( msg )  std::cout << __FILE__ << ":" << __LINE__ << ": " << msg
int main(){
  LOG("");
  LOG("");
}
```
`cpp name.cpp` output:
```cpp
...
int main(){
  std::cout << "name.cpp" << ":" << 4 << ": " << "";
  std::cout << "name.cpp" << ":" << 5 << ": " << "";
}
```
`cpp test/name.cpp` output:
```cpp
...
int main(){
  std::cout << "test/name.cpp" << ":" << 4 << ": " << "";
  std::cout << "test/name.cpp" << ":" << 5 << ": " << "";
}
```
So that's something to be aware of. 

The `__LINE__` macro works a bit differently than other macros, it expands to the **current** line number, which means its "definition" changes every line. Also note that it starts from 1 rather than 0. 

There is also `__func__` which was introduced in C++11. This is not a macro, it is a special function-local predefined variable of type equivalent to `static const char` that contains the name of the function it is in. This is used by assert (`<cassert>`). 

Other predefined macros in the C++17 standard:

```cpp
__DATE__ //The date of translation of the source file: a character string literal of the form "Mmm dd yyyy"
__TIME__ //The time of translation of the source file: a character string literal of the form "hh:mm:ss"
```

#### Conditional compilation

This was used in the earlier example in debug message logging, but is a more general idea:

```cpp
#ifdef DEBUG 
#define D(x) x
#define R(x) 
#else 
#define D(x)
#define R(x) x
#endif
```

This macro allows you to simply add a flag to your build system for debug (D) and release (R) builds rather than having to go through and change all your code. Extremely convenient. The alternative is this:

```cpp
#ifdef DEBUG 
[debug mode code here]
#else
[release mode code here]
#endif
```

Blegh. Imagine having to write that for every location in your code where you want different code to run in debug vs release mode. Using D() and R() is much more concise and convenient. Probably you shouldn't have too much release-only code though, since bugs in release-only code won't show up in debug...thus removing the whole point of having debug mode. [What you don't want is a problem that goes away when you debug it](https://blogs.msdn.microsoft.com/oldnewthing/20060815-00/?p=30113). If you are working with timing-sensitive code (e.g. embedded systems, anything multithreaded) then be aware that debug print messages can change your program behavior (e.g. hide some race conditions or timing errors). Obviously you should test the release version. There are some who say you should use full optimizations for your debug build too. If you can handle stepping through optimized code then there's no reason not to use full optimization for all your builds. In The Practice of Programming (Kernighan & Pike 1999), the authors write: 

>we find stepping through a program less productive than thinking harder and adding output statements and self-checking code at critical places. Clicking over statements takes longer than scanning the output of judiciously-placed displays.

So at least some (well known) people believe that logging is better than debugging. If you agree, then there is no reason to not use full optimization for all your builds, since then you'll be able to see bugs only visible at high optimization levels in your debug builds. 

Another example of where conditional compilation is useful is for cross-platform code:

``` 
#ifdef _WIN32
[win32 specific code here]
#elif linux
[linux specific code here]
#else
[other stuff]
#endif
```

Note that in the GCC on my system, linux and unix are pre-defined. This could be quite dangerous:

```cpp
#include <iostream>
int main(){
        int Linux = 99;
        std::cout << linux;
}
```
Output when compiled with `g++`:
```
1
```
If you had tried to define the variable `linux` then it would have given you an error, since `linux` is predefined. So a typo can fuck up your program just like that. Here's what the GCC manual has to say about it:

>When the -ansi option, or any -std option that requests strict conformance, is given to the compiler, all the system-specific predefined macros outside the reserved namespace are suppressed. The parallel macros, inside the reserved namespace, remain defined.
>
>We are slowly phasing out all predefined macros which are outside the reserved namespace. You should never use them in new programs, and we encourage you to correct older code to use the parallel macros whenever you find it. We don’t recommend you use the system-specific macros that are in the reserved namespace, either. It is better in the long run to check specifically for features you need, using a tool such as autoconf.

And that's why you should always compile with `std=c++17`. 

You can get a list of pre-defined macros using this command:

```
gcc -dM -E - < /dev/null
```

It's worth giving this list a quick glance-over in case you find anything surprising (the only surprising ones on my system were "linux" and "unix"). Remember that macros (like everything else in C/C++ AFAIK) are case sensitive. 

#### Convenience

In competitive programming you need to write code fast. After coding for some time you begin to notice repetitive patterns in code, for example for loops. So for example you might use this to save yourself some keystrokes:

```
#define REP(i, a, b) for (int i = int(a); i <= int(b); i++)
``` 

Now instead of typing:

```
for (int i = 1; i <= 10; i++) {
```

You can just type:

```
REP(i, 1, 10) {
```

Much more concise. Typedefs are also useful for the same reasons (e.g ii, vii and so on). Obviously you should not use these shortcuts in production code, but even in production code people use macros to save themselves writing a lot of repetitive code, such as handling exceptions. For example, macros are used to avoid code repetition in [boost](http://www.boost.org/doc/libs/1_47_0/libs/preprocessor/doc/topics/motivation.html). 

#### [Stringizing](https://gcc.gnu.org/onlinedocs/cpp/Stringizing.html)

Stringizing turns a macro argument into a string constant. I don't believe there's a way to accomplish this without using macros. 

```cpp
#define WARN_IF(EXP) \
do { if (EXP) std::cout << "Warning: " #EXP "\n"; } while (false)
```

This macro expands into a do-while loop that prints a warning if EXP evaluates true, otherwise does nothing. Putting string literals next to each other concatenates them automatically, so you don't need '+' symbols between the string literals (in fact that would make the program invalid). Example usage:

```cpp
#include <iostream>
#define WARN_IF(EXP) \
do { if (EXP) std::cout << "Warning: " #EXP "\n"; } while (false)
int main(){
        WARN_IF(1+1==2);
        WARN_IF(2*2==3);
        WARN_IF(8*8==64);
}
```
cpp output:
```cpp
int main(){
 do { if (1+1==2) std::cout << "Warning: " "1+1==2" "\n"; } while (false);
 do { if (2*2==3) std::cout << "Warning: " "2*2==3" "\n"; } while (false);
 do { if (8*8==64) std::cout << "Warning: " "8*8==64" "\n"; } while (false);
}
```
Execution output:
```
Warning: 1+1==2
Warning: 8*8==64
```
Works pretty well. This should not be used to replace the assert macro which is defined in the C++ standard (in the `<cassert>` header), which terminates your program when the condition evaluates false, whereas this macro does not. Do note that, as explained earlier, whatever text you pass into a macro will have its comments stripped out and its whitespace "compressed". 

#### Concatenation

[Added on 4 May 2018]

The ## operator is the preprocessor directive for concatenation. It is used for directly modifying source code. An example is this:

```cpp
#define CONCAT(a,b) a##b
int CONCAT(ma,in){}
```
cpp output:
```
int main{}
```

Useful for creating new names out of names supplied to the macro. Example:

```cpp
#include <iostream>
#define ADDU(funcname) funcname##_
void f_(){ std::cout << "hello world";}
int main(){ ADDU(f)(); }
```
cpp output:
```
void f_(){ std::cout << "hello world";}
int main(){ f_(); }
```
output when run:
```
hello world
```

### File inclusion.

The C++ preprocessor is used to "include" files into your program (this is obviously horrible which is why the C++ standards committee is working on modules...). 

What happens is that if you have an #include directive on line x, then that line will be replaced with the contents of the specified file (not necessarily the entirety of the contents of the file). Remember how #define was recursive? Well, #include is recursive too! In fact, here's what the C++17 standard has to say about it:

>A #include preprocessing directive causes the named header or source file to be processed from phase 1 through phase 4, recursively. All preprocessing directives are then deleted.

Before we get started, let's talk about the syntax a bit more. There are 2 kinds of #includes, the angled brackets version and the quotes version:
```cpp
#include <file>
#include "file"
```
According to the C++17 standard (section 19.2 Source file inclusion) these directives both cause the directive to be replaced by a file searched for in an implementation-defined manner. The GCC manual has a [helpful section on this](https://gcc.gnu.org/onlinedocs/cpp/Search-Path.html):

>By default, the preprocessor looks for header files included by the quote form of the directive #include "file" first relative to the directory of the current file, and then in a preconfigured list of standard system directories. For example, if /usr/include/sys/stat.h contains #include "types.h", GCC looks for types.h first in /usr/include/sys, then in its usual search path.
>
>For the angle-bracket form #include <file>, the preprocessor’s default behavior is to look only in the standard system directories. The exact search directory list depends on the target system, how GCC is configured, and where it is installed. You can find the default search directory list for your version of CPP by invoking it with the -v option. For example,
>`cpp -v /dev/null -o /dev/null`

For getting C++ system headers you can use this command:

`cpp -x c++ -v /dev/null -o /dev/null`

You can find the standard library headers (e.g iostream) from there. 

So, generally you use the angled brackets version for including system headers (such as the standard C++ library headers) whereas you use the double quotes version for including your own headers. At least this is the case for GCC (not sure about other compilers, it's implementation-defined after all). You should also be aware that there's no real difference between a header file and a source file, as far as the compiler is concerned. You can put definitions in a header file and put declarations in a source file, no problems. You can even compile a .h file by itself e.g:

```cpp
g++ -x c++ main.h
```

Note the need for the `-x c++` before the filename. If you do not pass this flag then g++ (and clang++) will treat main.h as a header file and compile it into a .gch (precompiled header) file rather than into an executable. What's a precompiled header you ask? Well, GCC has a good [explanation here](http://gcc.gnu.org/onlinedocs/gcc/Precompiled-Headers.html):

>Often large projects have many header files that are included in every source file. The time the compiler takes to process these header files over and over again can account for nearly all of the time required to build the project. To make builds faster, GCC allows you to precompile a header file.
>
>To create a precompiled header file, simply compile it as you would any other file, if necessary using the -x option to make the driver treat it as a C or C++ header file. You may want to use a tool like make to keep the precompiled header up-to-date when the headers it contains change.
>
>A precompiled header file is searched for when #include is seen in the compilation. As it searches for the included file (see Search Path in The C Preprocessor) the compiler looks for a precompiled header in each directory just before it looks for the include file in that directory. The name searched for is the name specified in the #include with ‘.gch’ appended. If the precompiled header file cannot be used, it is ignored.
>
>For instance, if you have #include "all.h", and you have all.h.gch in the same directory as all.h, then the precompiled header file is used if possible, and the original header is used otherwise.
>
>A precompiled header file can be used only when these conditions apply:
>
>Only one precompiled header can be used in a particular compilation.  
>A precompiled header cannot be used once the first C token is seen. You can have preprocessor directives before a precompiled header; you cannot include a precompiled header from inside another header.  
>The precompiled header file must have been produced by the same compiler binary as the current compilation is using.

A simple implementation of a precompiled header (PCH) is just a dump of the compiler's internal state, and when a PCH is loaded, it simply replaces the internal state of the compiler with the state in the PCH. I don't know if this is how GCC or other compilers do it, but from the conditions listed it would seem this is probably not that far off. As mentioned, precompiled headers rely on the exact compiler binary, build settings, and the environment, so it's not suitable for distribution. 

Enough exposition, let's play with an example:

a.cpp:
```cpp
#include "b.cpp"
world
```
b.cpp:
```cpp
#include "a.cpp"
hello
```
What happens when we call cpp on a.cpp?
```
error: #include nested too deeply
```
You get an error, probably followed by many lines of hello and world. Here it **actually goes into infinite recursion**. This is one of the reasons why we have include guards - we add an include guard to a file in order to make the inclusion of that file idempotent (an idempotent operation is one where doing it n times is the same as doing it once). So we should change our files to this:
a.cpp:
```cpp
#ifndef A
#define A
#include "b.cpp"
world
#endif
```
b.cpp:
```cpp
#ifndef B
#define B
#include "a.cpp"
hello
#endif
```
Now we get:
```console
$ cpp -P a.cpp
hello
world
$ cpp -P b.cpp
world
hello
```
Remember I mentioned earlier that macro expansion is recursive? That applies to all directives, not just #define but also #include and everything else. The #include directive, contrary to what some people say, is **not** just a simple copy-paste. As mentioned in the standard, when you #include a file, the included file goes through phases 1-4 (so all the preprocessor directives in the included file get executed), **and then** the output replaces the #include directive. You saw this earlier with the include guards - the file isn't copy-pasted verbatim, it's first preprocessed, and then the output gets copy-pasted in. 

So, how does the include guard actually work? There is quite a good explanation [here](https://gcc.gnu.org/onlinedocs/cpp/Ifdef.html): 


>The simplest sort of conditional is
>```cpp
>#ifdef MACRO
>controlled text
>#endif /* MACRO */
>```
>This block is called a conditional group. controlled text will be included in the output of the preprocessor if and only if MACRO is defined. We say that the conditional succeeds if MACRO is defined, fails if it is not.
>
>The controlled text inside of a conditional can include preprocessing directives. They are executed only if the conditional succeeds. You can nest conditional groups inside other conditional groups, but they must be completely nested. In other words, ‘#endif’ always matches the nearest ‘#ifdef’ (or ‘#ifndef’, or ‘#if’). Also, you cannot start a conditional group in one file and end it in another.

So, when the preprocessor processes the header file for the first time, MACRO is not defined so it continues execution and executes the `#define MACRO` line, so the next time it sees `#ifndef MACRO` it knows to skip to the `#endif` line, thus skipping the part of the file that is "guarded" by the include guard. Obviously, if a part of the file is not protected by the include guard then it is not idempotent:

a.cpp:
```cpp
#include "b.cpp"
#include "b.cpp"
```
b.cpp:
```cpp
#ifndef B
#define B
hello
#endif
world
```
Output of cpp -P on a.cpp:
```
hello
world
world
```

Pretty simple. You could use `#pragma once` which is not in the C++ standard but is widely supported by many compilers (even including GCC! Which removed support for it at some point but then added it back in later on lul), although to be absolutely standards-compliant, you should just use include guards. And yes, every header file should have an include guard, it's good practice. Typically we use the file name as the identifier in an include guard to avoid name clashes. 

One useful thing to note is that you can put whitespace before or after the  symbol, so this is fine:

```cpp
#if 1
  #if 0
    int main(){}
  #endif
  #if 1
    int main(){}
  #endif
#endif 
```
Some (pre-Ansi C, so **absolutely ancient**) compilers forced the `#` to be the first symbol on the line but all modern compilers are fine with indented directives. I think it looks more readable this way.  
 

## 2. Compilation, linking, and loading 

Before we get started, I want to tell you about this cool command line tool called `file`. You can just type `file a.out` to find out what type of file `a.out` is (e.g whether if it's a statically or dynamically linked ELF). 

The answer to the question "Why do we need `#include <iostream>`?" is quite simple: Because C++ requires that you declare functions (and variables) before you use them. In contrast, ANSI C89 (which gcc still supports) allowed you to use functions before declaring them because the compiler automatically generated implicit declarations (the implicitly declared function returns an int and can take an unspecified parameters<sup>*</sup>). This feature was removed in ANSI C99 and C++. As for why, there is a short explanation in [Rationale for International Standard Programming Languages C](http://www.open-std.org/jtc1/sc22/wg14/www/C99RationaleV5.10.pdf) §6.5.2.2 Function calls:

>A new feature of C99: The rule for implicit declaration of functions has been removed in C99. The effect is to guarantee the production of a diagnostic that will catch an additional category of programming errors. After issuing the diagnostic, an implementation may choose to assume an implicit declaration and continue translation in order to support existing programs that exploited this feature.

See also [The New C Standard](http://www.coding-guidelines.com/cbook/cbook1_0b.pdf) (6.2.5 page 506):

>Similarly, for function return values, prior to C99 it was explicitly specified that if no function declaration was visible the translator provided one. These implicit declarations defaulted to a return type of int. If the actual function happened to return the type unsigned int, such a default declaration might have returned an unexpected result. A lot of developers had a casual attitude toward function declarations. The rest of us have to live with the consequences of the Committee not wanting to break all the source code they wrote. The interchangeability of function return values is now a moot point, because C99 requires that a function declaration be visible at the point of call (a default declaration is no longer provided). 

<sup>*</sup>calling f() without declaring it first is equivalent to calling the function `int f()`. In C (as opposed to C++), a function declared without any parameters can take an unspecified (**not variable**) number of arguments. Calling the function with the wrong number (more or less than in the function definition) of arguments results in undefined behavior. If you want a function to take a variable number of arguments declare a variadic function instead. Note that in C++ `int f()` means f takes zero arguments, if you want the same in C you have to use `int f(void)`. You can also use that in C++ but why bother.

So, you need to declare functions before using them. In practice this is often done by including header files which contain the declarations, but of course you could just put the declarations directly in the source file if you wanted to, makes no difference as far as the compiler is concerned. A declaration is essentially a promise that there will be a definition later. You still have to have the definition somewhere, when you call the function. Normally when the definition for that function is in a shared library then you have to link the file with a shared library (`.so` file) that contains that definition. If you don't have a `.so` file to link against you could make a dummy file or just use `dlopen`. 

The important thing you need to remember is that you can compile your program with one shared library at compile time and load it with a different shared library at load time (with completely different definitions for the functions that you use) and as long as they are compatible it will just work. 

You might be wondering: since `#include <iostream>` only puts the declarations into the translation unit, where are the actual definitions? Well these are in the shared library files that are linked into the object file produced from compiling your code when you run g++. In the case of `iostream` the definitions are in the standard C++ library which is by default dynamically linked into your object file when you run g++ (if you run `ldd` on a normally compiled file then you should see `libstdc++.so` in the output). 

What does that mean? Okay, let's go over the later phases of translations quickly. In phase 5, characters in the source character set are converted to characters in the execution character set<sup>*</sup>. In phase 6, adjacent string literals are concatenated. **Phase 7 is what we normally think of as compilation**. Phase 8 is when templates are instantiated. **Phase 9 is what we normally think of as linking**, it's when translation units and instantiation units are combined and external symbol references are resolved. 

This is probably a good place to define the "physical structure" vs "logical structure" of a program. Simply put, "physical structure" is how a program is organized into source code files (directory structure etc), whereas "logical structure" is how a program is divided into functions, classes, namespaces etc. We divide a project into many files for a variety of reasons, one of them being that multiple programmers can work on different files at the same time, the other being that multiple files means that a change to one file does not mean everything has to be recompiled. This is called separate compilation, and it allows one change to one file to not cause the entire project to be recompiled. The basic idea is that source files are compiled into object files, which are then linked together. If we change a source file, we only have to recompile the object files that depend on that source file, and then relink those object files with the object files/executables that depend on them and so on up the dependency tree, rather than having recompile/relink the entire project. You can specify this in a makefile if using make. 

BTW, we compile C++ code using g++, not gcc. But if you want to use gcc rather than g++ to link your code then you will need to pass along the standard C++ library to link against with `-lstdc++`. 

Anyway. Let's talk about compilation. In this stage, the preprocessed C++ source code (now devoid of preprocessing directives) is fed into the compiler. The compiler then compiles it into assembly code, and the assembler translates assembly code into object code i.e relocatable machine code, which is not executable. Some compilers directly compile into object code, but some e.g gcc -s will let you see the assembly output.  

Okay so, first of all, the output of the compilation phase is object files (.o files). These are then linked in the linking phase to produce executables (the file named `a.out`). You can tell gcc and clang to emit these object files using the -c option. So, as mentioned earlier, object code is relocatable machine code, it is not executable because the symbol references have not been resolved at this point. 

Questions: wtf is relocatable code? wtf is a symbol reference? 

In order to answer these questions you should know a little bit about the ELF (executable and linkable format). 


### Types of ELF files

There are 3 types of ELF files: 

1. Relocatable files (aka object files). These are produced by compilers (and assemblers) and need to be linked with other files before they can be executed. 
2. Executable files. These files specify how exec creates the program's process image. They are already linked and have all their symbols resolved except possibly for shared library symbols that are resolved at load time. 
3. Shared object files. These files can be linked with dynamically-linked executable files during load time automatically or during runtime using `dlopen` and contain executable code. Note that libraries on Linux, both static (`.a`) and dynamic (`.so`) are expected to begin with a `lib` in their name. So if you have `libmath.so` you link it with `gcc -lmath`. There is a [way around this](https://stackoverflow.com/questions/10234208/how-do-i-link-a-library-file-in-gcc-that-does-not-start-with-lib) e.g you can do `gcc main.o -L/path/to/foo -l:foo.a`. Note that some `.so` files can be directly executed. [Here](http://tldp.org/HOWTO/Program-Library-HOWTO/shared-libraries.html) is some info on shared libraries if you need to use or make shared libraries. 

Question: Can you dynamically link against an object file? AFAIK you cannot, although I'm not sure why. I believe it is because object files generally don't have a program header, which is required by the dynamic linker. From Linkers and Loaders chapter 10:

>Once it's found the file containing the library, the dynamic linker opens the file, and reads the ELF header to find the program header which in turn points to the file's segments including the dynamic segment. The linker allocates space for the library's text and data segments and maps them in, along with zeroed pages for the bss. From the library's dynamic segment, it adds the library's symbol table to the chain of symbol tables, and if the library requires further libraries not already loaded, adds any new libraries to the list to be loaded.

And chapter 3:

>Relocatable files have section tables, executable files have program header tables, and shared objects have both. The sections are intended for further processing by a linker, while the segments are intended to be mapped into memory.

Question: Can you statically link against a shared object? **I am not aware of any ways to statically link to a shared object file. If you try, ld will complain with `ld: attempted static link of dynamic object`**. A plausible answer I think is that this is because dynamic libraries are already linked, so the relocation information has been thrown out, which means they cannot be statically linked against. I'm not sure on this. 

Note that shared object files are "more similar" to executables than they are to object files (in that you can actually execute shared object files but not object files). From the ELF reference:

>Executable and shared object files statically represent programs. To execute such programs, the system uses the files to create dynamic program representations, or process images. A process image has segments that hold its text, data, stack, and so on. 

These segments are required in order to be able to dynamically link in a shared object during load time or during runtime (with `dlopen`). 

An ELF file begins with an ELF header which describes the structure of the file. Only the ELF header has a fixed position in the file, there is no specified order nor size for sections and segments. For more info, see [the Sky reference](http://www.skyfree.org/linux/references/ELF_Format.pdf). An ELF file can contain one or two tables. One is called the **section header table**, which sits at the end of the file and describes the structure of the file in terms of **sections**. Relocatable files must have this table because they are needed by the linker, but an executable file does not need to have one. Compilers, linkers and assemblers sees the file in terms of sections as described by the section header table. You may use `readelf -S` to see a list of sections. 

The other table is called the **program header table**. This sits at the top of the file and describes the file in terms of **segments**. The program header table tells the OS how to create a process image, without it a file cannot be executed. A segment may contain 0 or more sections. This is what the system loader uses to execute the file, so an executable file must have it, but a relocatable file does not need to have one. The system loader sees the file in terms of segments as described by the program header table. You may use `readelf -l` to see a list of segments. 

Relevant quote from Linkers and Loaders:

>ELF files come in three slightly different flavors: relocatable, executable,
and shared object. Relocatable files are created by compilers and assemblers
but need to be processed by the linker before running. Executable
files have all relocation done and all symbols resolved except perhaps
shared library symbols to be resolved at runtime. Shared objects are
shared libraries, containing both symbol information for the linker and directly
runnable code for runtime.

Relevant quote from the Oracle/Sun article/book:

>A program header table, if present, tells the system how to create a process image. Files used to generate a process image, executables and shared objects, must have a program header table; relocatable objects do not need such a table. 
>
>A section header table contains information describing the file's sections. Every section has an entry in the table. Each entry gives information such as the section name, the section size, and so forth. Files used in link-editing must have a section header table; other object files might or might not have one.

Where link-editing means linking. In short: **If a file is linkable, then it must have a section header table. If a file is executable, then it must have a program header table.** 

As mentioned earlier, you can use `ldd` to get information on what dynamic libraries an executable needs. This is recursive, so it will list not what the executable directly depends on but also what those dependencies depend on and so on. If ldd can't find a library it will say so. `readelf` is more limited in that it can only list the direct dependencies of an executable - it's not recursive like ldd. `readelf` is generally safer than `ldd` on untrusted binaries though (you should never run `ldd` on untrusted binaries since it may actually execute them ;). 


Okay, so let's distinguish between "linkers" and "loaders" in the classic sense. For this let's look at the definitions given in Linkers and Loaders [chapter 1](http://www.iecc.com/linker/linker01.html):


>Linking vs. loading
>
>Linkers and loaders perform several related but conceptually separate actions.
>
>Program loading: Copy a program from secondary storage (which since about 1968 invariably means a disk) into main memory so it's ready to run. In some cases loading just involves copying the data from disk to memory, in others it involves allocating storage, setting protection bits, or arranging for virtual memory to map virtual addresses to disk pages.
>
>Relocation: Compilers and assemblers generally create each file of object code with the program addresses starting at zero, but few computers let you load your program at location zero. If a program is created from multiple subprograms, all the subprograms have to be loaded at non-overlapping addresses. Relocation is the process of assigning load addresses to the various parts of the program, adjusting the code and data in the program to reflect the assigned addresses. In many systems, relocation happens more than once. It's quite common for a linker to create a program from multiple subprograms, and create one linked output program that starts at zero, with the various subprograms relocated to locations within the big program. Then when the program is loaded, the system picks the actual load address and the linked program is relocated as a whole to the load address.
>
>Symbol resolution: When a program is built from multiple subprograms, the references from one subprogram to another are made using symbols; a main program might use a square root routine called sqrt, and the math library defines sqrt. A linker resolves the symbol by noting the location assigned to sqrt in the library, and patching the caller's object code so the call instruction refers to that location.
>
>Although there's considerable overlap between linking and loading, it's reasonable to define a program that does program loading as a loader, and one that does symbol resolution as a linker. Either can do relocation, and there have been all-in-one linking loaders that do all three functions.
>
>One important feature that linkers and loaders share is that they both patch object code, the only widely used programs to do so other than perhaps debuggers. This is a uniquely powerful feature, albeit one that is extremely machine specific in the details, and can lead to baffling bugs if done wrong.


### Static vs dynamic linker

In order to answer the question of what exactly the difference is between a "dynamic linker" and a "normal" linker, let's look at `ld` and `ld.so`. 

[ld](http://man7.org/linux/man-pages/man1/ld.1.html) is the GNU linker and is used in the linking step after compilation. When you run gcc to produce an executable, gcc calls ld for the linking stage. `ld` does not know anything about C++ (nor C++ about `ld`, C++ is supposed to be platform independent after all) and can be used to link together any linkable ELF files. The flags you supply to `-Wl` are passed on to `ld`. You could use `ld` directly, but since the options that are passed to `ld` by `gcc` seems to change with every OS release, this seems ill advised. 

[ld.so](http://man7.org/linux/man-pages/man8/ld.so.8.html) (actually `ld-linux.so*` since we are dealing with `ELF` files) is called the "dynamic linker/loader" (note that `ld.so` is not a "loader" in the classic sense - it's really the kernel that does the loading). It is run **every time you run a dynamically linked program or shared object**:

>The programs ld.so and ld-linux.so* find and load the shared objects (shared libraries) needed by a program, prepare the program to run, and then run it.
>
>**Linux binaries require dynamic linking (linking at run time) unless the -static option was given to ld(1) during compilation.**
>
>The program ld.so handles a.out binaries, a format used long ago; ld-linux.so* (/lib/ld-linux.so.1 for libc5, /lib/ld-linux.so.2 for glibc2) handles ELF, which everybody has been using for years now. Otherwise, both have the same behavior, and use the same support files and programs as ldd(1), ldconfig(8), and /etc/ld.so.conf.

As the man page says, `ld.so` is run when you run a dynamically-linked program, and you can choose whether or not a program is dynamically or statically linked by controlling the options you pass to `ld`. If a program is statically linked then it won't call `ld.so`, but if it is dynamically linked then it will. You may think of `ld.so` as a continuation of the linking process: in static linking `ld` does all of the linking itself, whereas in dynamic linking `ld` leaves a part of the linking to be done by `ld.so` when the program is loaded.

So, now we know about `ld`, a "normal" linker, which does linking just after compilation, and `ld.so`, a "dynamic" linker, which does linking at program load time. It is also possible to link in a shared library after the program has been loaded, at runtime, and this is possible with [the `dlopen` function](https://linux.die.net/man/3/dlopen).

I have seen people refer to `ld` as a "static linker" and `ld.so` as a "dynamic linker". **Please** do not confuse that with "static linking" and "dynamic linking". 

### Static vs dynamic linking

Static linking is the act of copying all **used** library routines into the linked executable image (NOT the object file, which hasn't been linked yet), this results in an increased executable file size. Dynamic linking refers to the use of an external shared library (a `.so` shared object file is the linux equivalent of windows dll files) instead of storing the code in the executable file itself, this approach allows for multiple programs to use the same library, possibly resulting in reduced memory usage which is considered an advantage of dynamic linking. One advantage of static linking is that you don't have to distribute the shared object separately (otherwise making sure your client has the right version of the right library in the right place can be a pain, see DLL/dependency hell). Another advantage of static linking is that it does not have the load-time (and run-time in the case of PIC) overhead incurred by dynamic linking (but in practice on modern architectures this performance penalty is not necessarily noticeable). One often cited advantage of dynamic libraries is that if it's a library that a lot of applications rely on, then if a security flaw is discovered in it and it needs to be patched, then it is fairly trivial to patch that one shared object file, whereas if the library was statically linked into many applications on your system then it's much more effort to patch all of them. E.g if libc was statically linked into every executable then an update to libc would require rebuilding (or redownloading) every executable on your system. 

For the examples below we will be using 2 files: test.cpp and main.cpp. The test.cpp file will be compiled into a library and the main.cpp file will make use of that library:

Create test.cpp:
```cpp
#include <iostream>
void hello_world(){
  std::cout << "hello world!" << std::endl;
}
void waste_lots_of_space(){
  std::cout << "this is a long string to waste a lot of space!" << std::endl;
  std::cout << "another long string to waste a lot of space!" << std::endl;
}
``` 
Now create main.cpp:
```cpp
void hello_world();
int main(){
  hello_world();
}
```

Let's start with static linking, although it's less commonly used nowadays. In Linux, there are 2 types of libraries: static and dynamic. As the name suggests, static libraries (`.a` files) are only used for static linking whilst dynamic libraries (`.so` files) are only used for dynamic linking. A `.a` file is normally an archive of object (`.o`) files created by `ar`. Although `ar` is actually a general purpose archiver that can create archives holding all types of files, it's almost exclusively used for creating static libraries nowadays. When creating a `.a` file normally the `rcs` options are used. The `r` option means "insert with replacement" (without some insert option such as `q` or `r` you are not inserting anything into the archive), the `c` option specifies to create the archive (rather than to update an existing archive, you need this option to create new archives), and the `s` option "writes an object file index into the archive" (I'm not sure if/when this option is actually required). 

To link our library into our executable statically, we can do this:

```
g++ -c test.cpp 
ar cq libtest.a test.o  
g++ -static main.cpp libtest.a
```

The first line compiles test.cpp into test.o, the second line creates an archive called libtest.a from test.o, whilst the third line links the library together with the object file compiled from main.cpp into the executable `a.out`. Running `file a.out` reveals that it is statically linked, running `ldd a.out` gives the message `not a dynamic executable`, and running `readelf -d a.out` gives the message `There is no dynamic section in this file.` If you omit the "-static" flag you will notice that only libtest is statically linked into the resulting executable, the executable (9kb) is still dynamically linked to libc++ and other libraries. Whereas "-static" forces even libc++ and other libraries to be statically linked into the resulting executable as well, causing the executable size to blow up to 2mb (on my system). 

Note that you do not have to use `.a` files for static linking (you can just use the `.o` files directly) however the advantage of using the `.a` file is reduced file size. When statically linking with a `.a` file only the symbols (variables, functions, etc) used by your program is included in the final executable, whereas if you link a `.o` file then the whole of that file is included. If you try:

```
g++ -static main.cpp test.o
g++ -static main.cpp -L. -ltest
```

You will see that the second command produces a slightly smaller `a.out` file. However if you remove the `waste_lots_of_space()` function from test.cpp then you will see that the file sizes become the same. This suggests to me that the extra space used by linking with the object file directly compared to linking with the archive is taken up by the definition of the unused function. Also, a `.a` file may contain multiple object files so it might be more convenient to just archive them into a single library and link against that. 

Okay, let's talk about dynamic linking now. As mentioned earlier, these `.so` files can be linked into a program at load time (when program is loaded) or at run time (after the program has been loaded) with a function like `dlopen`. Continuing with our example from earlier, let's now link our library dynamically instead of statically. 

First, run this command to create the shared object file:
```
g++ -fPIC -shared -o libtest.so test.cpp
``` 
According to the gcc documentation, the "-shared" option tells gcc to:

>Produce a shared object which can then be linked with other objects to form an executable. Not all systems support this option. For predictable results, you must also specify the same set of options used for compilation (-fpic, -fPIC, or model suboptions) when you specify this linker option.

I will explain what the "-fPIC" option does in a bit.

Now to compile main.cpp with the library we just built:
```
g++ main.cpp ./libtest.so
```
Note that `libtest.so` is dynamically linked into `a.out` and not statically linked. Which means that if you delete `libtest.so` then `a.out` will no longer run. We use `./libtest.so` instead of `libtest.so` because if we don't...Okay, let's show you what happens if you use either of these commands (they are equivalent as far as I can tell):

```
g++ main.cpp libtest.so
g++ main.cpp -L./ -ltest
```

**Very important**: the -L option specifies the directory to use at compile time, **NOT** the directory that the loader should look in at load time! The "-L" flag isn't actually needed at all, it's just for convenience. 

If we run `readelf -d" on the output file from either of these commands we get this:
```
Dynamic section at offset 0xe18 contains 25 entries:
  Tag        Type                         Name/Value
 0x0000000000000001 (NEEDED)             Shared library: [libtest.so]
 0x0000000000000001 (NEEDED)             Shared library: [libc.so.6]
...
```
In fact, if you run ldd on the output file you will see this:
```
libtest.so => not found
```
On the other hand if we had used:
```
g++ main.cpp ./libtest.so
```
Then we would get:
```
Dynamic section at offset 0xe18 contains 25 entries:
  Tag        Type                         Name/Value
 0x0000000000000001 (NEEDED)             Shared library: [./libtest.so]
 0x0000000000000001 (NEEDED)             Shared library: [libc.so.6]
...
```
And a quick ldd shows us that libtest has been found. 

So, as you can see, the name of the shared library in the dynamic section has changed from `libtest.so` to `./libtest.so`. Why does this fix the problem? To answer this question, let's look at [the man page on ld.so](http://man7.org/linux/man-pages/man8/ld.so.8.html) again:

>**When resolving shared object dependencies, the dynamic linker first inspects each dependency string to see if it contains a slash (this can occur if a shared object pathname containing slashes was specified at link time). If a slash is found, then the dependency string is interpreted as a (relative or absolute) pathname, and the shared object is loaded using that pathname.**
>
>If a shared object dependency does not contain a slash, then it is searched for in the following order:
>
>- Using the directories specified in the DT_RPATH dynamic section attribute of the binary if present and DT_RUNPATH attribute does not exist. **Use of DT_RPATH is deprecated**.
>- Using the environment variable LD_LIBRARY_PATH (unless the executable is being run in secure-execution mode; see below). in which case it is ignored.
>- Using the directories specified in the DT_RUNPATH dynamic section attribute of the binary if present. Such directories are searched only to find those objects required by DT_NEEDED (direct dependencies) entries and do not apply to those objects' children, which must themselves have their own DT_RUNPATH entries. This is unlike DT_RPATH, which is applied to searches for all children in the dependency tree.
>- From the cache file /etc/ld.so.cache, which contains a compiled list of candidate shared objects previously found in the augmented library path. If, however, the binary was linked with the -z nodeflib linker option, shared objects in the default paths are skipped. Shared objects installed in hardware capability directories (see below) are preferred to other shared objects.
>- In the default path /lib, and then /usr/lib. (On some 64-bit architectures, the default paths for 64-bit shared objects are /lib64, and then /usr/lib64.) If the binary was linked with the -z nodeflib linker option, this step is skipped. 

Long story short, because we had added a slash in the string, that made the dynamic linker (loader) interpret the string as a pathname (relative in this case since it starts with `./`). Without that slash, the dynamic linker would have searched in the locations specified in the 5 bullet points (DT_RPATH, LD_LIBRARY_PATH, DT_RUNPATH, /etc/ld.so.cache, and /lib and /usr/lib). 

Now that we know that the dynamic linker searches in the LD_LIBRARY_PATH environment variable, we can pass that to the executable and see if that fixes it:

```
g++ main.cpp -L./ -ltest
LD_LIBRARY_PATH=./ ./a.out
```
output:
```
hello world!
```

So, when you don't have control over the binary, you can still pass LD_LIBRARY_PATH to it (if you want to simply append a path to the variable you can use `LD_LIBRARY_PATH=.:$LD_LIBRARY_PATH ./a.out` instead). Unfortunately if the library path is hardcoded into the ELF as an RPATH, then the linker will stop looking after it finds the .so file in the location specified by the RPATH. Allow me to illustrate this problem by creating a new directory in our current directory called fakelib:

fakelib/faketest.cpp:

```cpp
#include <iostream>
void hello_world(){
  std::cout << "haha! you got tricked!" << std::endl;
}
```
Then we compile it to a library with the same name:
```
g++ -fPIC -shared -o libtest.so faketest.cpp
```

Next, we go back to the original directory and compile main.cpp with RPATH:
```
g++ main.cpp -L./ -ltest -Wl,-rpath=./fakelib
```
The -Wl option passes a comma-separated list of arguments to the linker. `-rpath` is a linker flag so we have to use -Wl to specify the RPATH to write into the executable. 

Running readelf -d on the newly linked executable gives this:
```
Dynamic section at offset 0xe08 contains 26 entries:
  Tag        Type                         Name/Value
 0x0000000000000001 (NEEDED)             Shared library: [libtest.so]
 0x0000000000000001 (NEEDED)             Shared library: [libc.so.6]
 0x000000000000000f (RPATH)              Library rpath: [./fakelib]
...
```
So now when we run this:
```
LD_LIBRARY_PATH=./ ./a.out
```
We get this:
```
haha! you got tricked!
```
As expected, the RPATH overrides the LD_LIBRARY_PATH. If you deleted (or renamed) faketest/libtest.so then you would find that `LD_LIBRARY_PATH=./ ./a.out` prints `hello world` instead. But if we can't or don't want to delete or rename the file specified by the RPATH, what can do we instead? Well, there is a cool tool called `patchelf` which allows us to add, edit, or remove RPATHs from ELF files. So we can do this:
```
patchelf --remove-rpath a.out
```
Running readelf -d on it again:
```
Dynamic section at offset 0xe08 contains 25 entries:
  Tag        Type                         Name/Value
 0x0000000000000001 (NEEDED)             Shared library: [libtest.so]
 0x0000000000000001 (NEEDED)             Shared library: [libc.so.6]
```

Also you may see some people using "-fpic" instead of "-fPIC". When in doubt, use "-fPIC" because it always works, whereas "-fpic" may have limitations like the number of globally visible symbols or size of the code. The -fPIC tells g++ to create position independent code. 

#### Relative RPATH vs $ORIGIN

[This section was added on 3 May 2018]

From the [man page for ld.so](http://man7.org/linux/man-pages/man8/ld.so.8.html):

>   Rpath token expansion
>       The dynamic linker understands certain token strings in an rpath
>       specification (DT_RPATH or DT_RUNPATH).  Those strings are
>       substituted as follows:
>
>       $ORIGIN (or equivalently ${ORIGIN})
>              This expands to the directory containing the program or shared
>              object.  Thus, an application located in somedir/app could be
>              compiled with
>
>                  gcc -Wl,-rpath,'$ORIGIN/../lib'
>
>              so that it finds an associated shared object in somedir/lib no
>              matter where somedir is located in the directory hierarchy.
>              This facilitates the creation of "turn-key" applications that
>              do not need to be installed into special directories, but can
>              instead be unpacked into any directory and still find their
>              own shared objects.
>
>       $LIB (or equivalently ${LIB})
>              This expands to lib or lib64 depending on the architecture
>              (e.g., on x86-64, it expands to lib64 and on x86-32, it
>              expands to lib).
>
>       $PLATFORM (or equivalently ${PLATFORM})
>              This expands to a string corresponding to the processor type
>              of the host system (e.g., "x86_64").  On some architectures,
>              the Linux kernel doesn't provide a platform string to the
>              dynamic linker.  The value of this string is taken from the
>              AT_PLATFORM value in the auxiliary vector (see getauxval(3)).

As we saw in the previous section, it is possible to use relative paths but these relate to where you called the executable, rather than where the executable actually is. If you run this command:


```
g++ main.cpp -L./ -ltest -Wl,-rpath=./fakelib
cd fakelib
../a.out
```
You will see an error message:
```
../a.out: error while loading shared libraries: libtest.so: cannot open shared object file: No such file or directory
```

In order to gain the ability to run the executable from anywhere (and not just inside the directory of the executable itself), we need a way to specify where the libraries are relative to the location of the executable, and not the shell's current working directory. 

On systems that support it, the $ORIGIN variable gives us this ability. This special variable resolves to the directory in which the executable resides. It is used like this:

```
g++ main.cpp -L./ -ltest -Wl,-rpath='$ORIGIN/fakelib'
```

So if you build a.out with that, then you can run it from anywhere and it will still work. And just like with relative paths you can move the directory (that contains both the executable and the libraries that the executable depends on) around as you like and everything will still work. 

### Position Independent Code

This section may be outdated since I haven't verified that the information contained here is true. 

What is position independent code? As you know, programs (on modern OSes) run in their own address space (this is called **virtual addressing** NOT to be confused with **virtual memory**, they are complete different and orthogonal concepts - you can have neither, both, or either. Virtual addressing is presenting the illusion to each program that it has the entire address space to itself - not sharing it with other programs. Virtual memory is presenting the illusion that the system has more memory available than it actually does. Confusingly, both virtual addressing and virtual memory are often implemented using paging, which implements virtual memory by swapping pages in and out of memory to the disk, and implements virtual addressing via a page table which stores the translation between virtual and physical addresses - many computers have a hardware module called a TLB which caches address translations to speed it up), which is a great safety feature as it means that they cannot access other programs, and also makes the linker writer's job easier. This means that in order for a program to call a library function, that library must be loaded into that program's address space. However, since a program can load any number of libraries in any order, we cannot ensure that a library is loaded into a particular address on a program, which means that we need to relocate the library's code<sup>*</sup> (the text section must be writable or copy-on-write (COW) for this to happen) after loading it into the program's address space. This is called load-time relocation. [There's an article about it here](https://eli.thegreenplace.net/2011/08/25/load-time-relocation-of-shared-libraries/). 
This has the disadvantage of load-time overhead and means that applications generally cannot share the same page in memory for the shared library code, since it has to be relocated potentially differently for each application. The alternative, which is now the most commonly used method on Linux, is PIC. Basically, PIC is a technique that moves all the relocation to the writable data section so that the text section can be made readonly (i.e doesn't have to be relocated for every process that uses it), which means we can save memory by having all applications that use the shared library's text and readonly data to use the same pages in memory. The writable data section is usually COW (copy-on-write). 

<sup>*</sup>I'm simplifying here. There were in fact systems which allowed libraries to require themselves to be loaded at a hard-coded address. And then if a program tries to load 2 libraries that require the same address, the program can't launch. In such a system, a system manager would have to maintain a list of library memory addresses, so if for example a library expands to beyond its allotted limit, this could change the addresses of other libraries, which would then require executables that rely on these libraries to be relinked. This method generally isn't used anymore, at least not on general purpose OSes. 

Relevant quote from [Linkers and Loaders chapter 10](http://www.iecc.com/linker/linker10.html):

>ELF shared libraries can be loaded at any address, so they invariably use position independent code (PIC) so that the text pages of the file need not be relocated and can be shared among multiple processes. As described in Chapter 8, ELF linkers support PIC code with a Global Offset Table (GOT) in each shared library that contains pointers to all of the static data referenced in the program, Figure 1. The dynamic linker resolves and relocates all of the pointers in the GOT. This can be a performance issue but in practice the GOT is small except in very large libraries; a commonly used version of the standard C library has only 180 entries in the GOT for over 350K of code.

You can find out more about how exactly PIC works from that link. Basically, access to data (variables) is done by referring to the GOT (Global Offset Table) in the writable data section, whereas access to functions is done via the PLT (Procedure Linkage Table) which resides in the readonly text section, which also refers to the GOT:

>Programs that use shared libraries generally contain calls to a lot of functions. In a single run of the program many of the functions are never called, in error routines or other parts of the program that aren't used. Furthermore, each shared library also contains calls to functions in other libraries, even fewer of which will be executed in a given program run since many of them are in routines that the program never calls either directly or indirectly.
>
>To speed program startup, dynamically linked ELF programs use lazy binding of procedure addresses. That is, the address of a procedure isn't bound until the first time the procedure is called.
>
>ELF supports lazy binding via the Procedure Linkage Table, or PLT. Each dynamically bound program and shared library has a PLT, with the PLT containing an entry for each non-local routine called from the program or library, Figure 3. Note that the PLT in PIC code is itself PIC, so it can be part of the read-only text segment.
>
>All calls within the program or library to a particular routine are adjusted when the program or library is built to be calls to the routine's entry in the PLT. The first time the program or library calls a routine, the PLT entry calls the runtime linker to resolve the actual address of the routine. After that, the PLT entry jumps directly to the actual address, so after the first call, the cost of using the PLT is a single extra indirect jump at a procedure call, and nothing at a return.

I found the explanation of how the lazy binding works in the book a bit confusing. I think I prefer Eli Bendersky's simpler, dumbed down explanation:

>A GOT is simply a table of addresses, residing in the data section. Suppose some instruction in the code section wants to refer to a variable. Instead of referring to it directly by absolute address (which would require a relocation), it refers to an entry in the GOT. Since the GOT is in a known place in the data section, this reference is relative and known to the linker. The GOT entry, in turn, will contain the absolute address of the variable...
>
>What happens when func is called for the first time is this:
>
>1. PLT[n] is called and jumps to the address pointed to in GOT[n].
>
>2. This address points into PLT[n] itself, to the preparation of arguments for the resolver.
>
>3. The resolver is then called.
>
>4. The resolver performs resolution of the actual address of func, places its actual address into GOT[n] and calls func. Note that GOT[n] now points to the actual func instead of back into the PLT. 
>
>So, when func is called again:
>
>1. PLT[n] is called and jumps to the address pointed to in GOT[n].
>
>2. GOT[n] points to func, so this just transfers control to func.
>
>In other words, now func is being actually called, without going through the resolver, at the cost of one additional jump. That's all there is to it, really.

If I'm understanding Eli Bendersky correctly, the costs are as follows:

Normal: Call function. Total cost: 1 jump.  
PIC: Call PLT[n]. Jump to GOT[n]. Total cost: 2 jumps. 

The advantage of load-time relocation over PIC is primarily run-time performance (because PIC has that one additional jump cost to call any function). If you are interested in writing shared libraries, you might want to read [this paper](https://www.akkadia.org/drepper/dsohowto.pdf).

Recall that earlier we mentioned that a library may be linked at 3 points: link time, load time, or run time. In fact dynamic linking at run time is not too different from dynamic linking at load time. From Linkers and Loaders:

>Although the ELF dynamic linker is usually called implicitly at program load time and from PLT entries, programs can also call it explicitly using dlopen() to load a shared library and dlsym() to find the address of a symbol, usually a procedure to call. Those two routines are actually simple wrappers that call back into the dynamic linker. When the dynamic linker loads a library via dlopen(), it does the same relocation and symbol resolution it does on any other library, so the dynamically loaded program can without any special arrangements call back to routines already loaded and refer to global data in the running program.
>
>This permits users to add extra functionality to programs without access to the source code of the programs and without even having to stop and restart the programs (useful when the program is something like a database or a web server.) Mainframe operating systems have provided access to "exit routines" like this since at least the early 1960s, albeit without such a convenient interface, and it's long been a way to add great flexibility to packaged applications. It also provides a way for programs to extend themselves; **there's no reason a program couldn't write a routine in C or C++, run the compiler and linker to create a shared library, then dynamically load and run the new code**. (Mainframe sort programs have linked and loaded custom inner loop code for each sort job for decades.)

Further information: [20 part guide linkers and loaders](https://lwn.net/Articles/276782/)

### dlopen 

So, dlopen is useful if you want to load library functions (that you might not know about at compile time) at runtime. How do we use dlopen? There is a handy guide [here](https://www.tldp.org/HOWTO/html_single/C++-dlopen/), which the following code is based on. 

We'll use the same test.cpp as before, but we'll use the main.cpp from the guide (with our file and function names):
```cpp
#include <iostream>
#include <dlfcn.h>

int main() {
    using std::cout;
    using std::cerr;

    cout << "C++ dlopen demo\n\n";

    // open the library
    cout << "Opening hello.so...\n";
    void* handle = dlopen("./libtest.so", RTLD_LAZY); 

    if (!handle) {
        cerr << "Cannot open library: " << dlerror() << '\n';
        return 1;
    }

    // load the symbol
    cout << "Loading symbol hello...\n";
    typedef void (*hello_t)();

    // reset errors
    dlerror();
    hello_t hello = (hello_t) dlsym(handle, "hello_world");
    const char *dlsym_error = dlerror();
    if (dlsym_error) {
        cerr << "Cannot load symbol 'hello': " << dlsym_error <<
            '\n';
        dlclose(handle);
        return 1;
    }

    // use it to do the calculation
    cout << "Calling hello...\n";
    hello();

    // close the library
    cout << "Closing library...\n";
    dlclose(handle);
}

```
In case you were wondering, `dlopen("libtest.so", RTLD_LAZY)` does not work. This is because dlopen searches for `.so` files in the same way that `ld.so` searches for them. 

Now it is much simpler to compile and run main.cpp:
```
g++ main.cpp -ldl
./a.out
```
Oops, it says `undefined symbol: hello_world`. Let's actually go read the tutorial:

>2.1. Name Mangling
>
>In every C++ program (or library, or object file), all non-static functions are represented in the binary file as symbols. These symbols are special text strings that uniquely identify a function in the program, library, or object file.
>
>In C, the symbol name is the same as the function name: the symbol of strcpy will be strcpy, and so on. This is possible because in C no two non-static functions can have the same name.
>
>Because C++ allows overloading (different functions with the same name but different arguments) and has many features C does not — like classes, member functions, exception specifications — it is not possible to simply use the function name as the symbol name. To solve that, C++ uses so-called name mangling, which transforms the function name and all the necessary information (like the number and size of the arguments) into some weird-looking string which only the compiler knows about. The mangled name of foo might look like foo@4%6^, for example. Or it might not even contain the word "foo".
>
>One of the problems with name mangling is that the C++ standard (currently [ISO14882]) does not define how names have to be mangled; thus every compiler mangles names in its own way. Some compilers even change their name mangling algorithm between different versions (notably g++ 2.x and 3.x). Even if you worked out how your particular compiler mangles names (and would thus be able to load functions via dlsym), this would most probably work with your compiler only, and might already be broken with the next version.

Okay, so it turns out the names are actually mangled by the C++ compiler! How do we see names? We can use `nm -g` or `readelf -Ws`:

```
readelf -Ws libtest.so
```
Output:
```
...
... FUNC    GLOBAL DEFAULT   11 _Z11hello_worldv
...
```
You might see something like that. Again, the names are mangled in an implementation-dependent manner so you might not even see something with "hello_world" in the name. In fact, [GCC mangles the names differently to other compilers deliberately in order to avoid interop problems](https://gcc.gnu.org/onlinedocs/gcc-7.3.0/gcc/Interoperation.html#Interoperation), see:

>On many platforms, GCC supports a different ABI for C++ than do other compilers, so the object files compiled by GCC cannot be used with object files generated by another C++ compiler.
>
>An area where the difference is most apparent is name mangling. The use of different name mangling is intentional, to protect you from more subtle problems. Compilers differ as to many internal details of C++ implementation, including: how class instances are laid out, how multiple inheritance is implemented, and how virtual function calls are handled. If the name encoding were made the same, your programs would link against libraries provided from other compilers—but the programs would then crash when run. Incompatible libraries are then detected at link time, rather than at run time.

The root cause of the problem is that whilst most OSes define a C ABI, which then all compilers on that OS use, this is not the case for C++ (I don't know why). That's why people often resort to using extern "C" for interop. See [this Dr.Dobb's article](http://www.drdobbs.com/interoperability-c-compilers/184401769):

>Application Binary Interfaces (ABI) describe the contents of the object files and executable images emitted by compilers. Platform descriptions have included common C ABI documents for years. As a result, interoperable C compilers are a common occurrence. However, C++ compilers have not been able to achieve this level of interoperability due to the lack of a common C++ ABI.

Well, we can try it anyways. So, let's edit our main.cpp to change the name to the mangled name: 

```
...
    hello_t hello = (hello_t) dlsym(handle, "_Z11hello_worldv");
...
```

Compiling again with g++, now it works! So it turns out that we need to call the mangled name instead of the name we declared in the source code. This is a little inconvenient, since we would have to inspect the binary every time, and with different versions of the library, compiled by different compilers, there is no guarantee that the name would always get mangled the same way (and even if they were, it would probably break in other, worse, ways). We want a way to control the symbol name from the source code so we don't have to deal with name mangling.

The tutorial has a solution to this: 


>C++ has a special keyword to declare a function with C bindings: extern "C". A function declared as extern "C" uses the function name as symbol name, just as a C function. For that reason, only non-member functions can be declared as extern "C", and they cannot be overloaded.
>
>Although there are severe limitations, extern "C" functions are very useful because they can be dynamically loaded using dlopen just like a C function.
>
>This does not mean that functions qualified as extern "C" cannot contain C++ code. Such a function is a full-featured C++ function which can use C++ features and take any type of argument.

So if we change the function definition to be `extern "C"` then the resulting `.so` file should have the unmangled symbol name:

test.cpp:
```cpp
#include <iostream>
extern "C" void hello_world(){
  std::cout << "hello world!" << std::endl;
}
```

If we run `readelf -Ws libtest.so` now we get:

```
...
... FUNC    GLOBAL DEFAULT   12 hello_world
...
```

The symbol name is now unmangled! And now the original main.cpp file with the unmangled function name works. There's more complicated things you have to do in order to load classes but we won't go into that here. 

### ODR violation

One question you may have is "what if I define something twice?". In C++ there is something called the "One Definition Rule" (ODR), see C++17 Standard Section 6.2 One-definition rule:

>No translation unit shall contain more than one definition of any variable, function, class type, enumeration type, or template.
>
>**Every program shall contain exactly one definition of every non-inline function or variable that is odr-used in that program outside of a discarded statement; no diagnostic required.** The definition can appear explicitly in the program, it can be found in the standard or a user-defined library, or (when appropriate) it is implicitly defined (see [class.ctor], [class.dtor] and [class.copy]). An inline function or variable shall be defined in every translation unit in which it is odr-used outside of a discarded statement.
>
>Exactly one definition of a class is required in a translation unit if the class is used in a way that requires the class type to be complete.
>
>There can be more than one definition of a class type, enumeration type, inline function with external linkage ([dcl.inline]), inline variable with external linkage ([dcl.inline]), class template, non-static function template, concept ([temp.concept]), static data member of a class template, member function of a class template, or template specialization for which some template parameters are not specified ([temp.spec], [temp.class.spec]) in a program provided that each definition appears in a different translation unit, and provided the definitions satisfy the following requirements. Given such an entity named D defined in more than one translation unit, then
>
>- each definition of D shall consist of the same sequence of tokens; and
>
>- in each definition of D, corresponding names, looked up according to [basic.lookup], shall refer to an entity defined within the definition of D, or shall refer to the same entity, after overload resolution and after matching of partial template specialization ([temp.over]), except that a name can refer to...

The worst thing about ODR violations is that the compiler cannot detect it (ODR violations can occur when multiple libraries, each of which may contain only one definition of the same thing, are linked together, thus only the linker and not the compiler can detect the ODR violation), but the standard does not require the linker to report ODR violations either (I'm not sure if it is possible for them to, googling didn't turn up anything conclusive, but I don't see why in principle they can't). So basically you just have to be really careful (e.g use anonymous namespaces) lul. See [DCL60-CPP. Obey the one-definition rule](https://wiki.sei.cmu.edu/confluence/display/cplusplus/DCL60-CPP.+Obey+the+one-definition+rule):

Noncompliant code example:

```cpp
// a.cpp
struct S {
  int a;
};
  
// b.cpp
class S {
public:
  int a;
};
```

Compliant code example:

```cpp
// a.cpp
namespace {
struct S {
  int a;
};
}
  
// b.cpp
namespace {
class S {
public:
  int a;
};
}
```
Same applies to anything else with external linkage (variables, functions etc). Multiple definitions, or multiple conflicting definitions if inline, will violate the ODR. 



`int a;` is a definition, so this is illegal:

```cpp
int a;
int a; //not okay, redefinition
```

THIS ALSO APPLIES ACROSS TRANSLATION UNITS. Non-static functions declared at namespace scope, and non-static non-const variables declared at namespace scope have EXTERNAL LINKAGE. That means the ODR applies - there can be only one definition of that variable IN THE ENTIRE PROGRAM. Which means that this violates the ODR:

a.cpp
```cpp
int a; //NOT implicitly inline
void f(){} //NOT implicitly inline
```
b.cpp
```cpp
int a; //NOT implicitly inline
void f(){} //NOT implicitly inline
int main(){}
```

If you compile a.cpp and b.cpp together into one program then you have a ODR violation (two violations really) because a and f are both non-inline and you cannot have multiple definitions of non-inline variables or functions in the program (across all translation units). 


There is no "one declaration rule": whilst you cannot define something twice (unless that thing is `inline` or otherwise exempt), you can declare something any number of times, provided that those declarations agree in type:
```cpp
extern int a;
extern int a; //okay, redeclaration with same type
extern float a; //not okay, redeclaration with different type
```

### The inline keyword

The inline keyword is basically a way to get around the ODR. "A function defined within a class definition is an inline function." - C++17 Standard Section 10 Declarations. It basically means "multiple definitions of this variable/function are allowed as long as each definition is in a separate translation unit". 

So we can do this:

a.cpp
```cpp
inline int a;
inline void f(){}
```
b.cpp
```cpp
inline int a;
inline void f(){}
int main(){}
```

This is legal in C++17 and does not violate the ODR, because a and f are both inline here. 

The main use is to allow you to put a function or variable definition in a header file which gets `#include`d into multiple source files. It had another meaning (suggestion to the compiler to inline) once upon a time, which is pretty much irrelevant nowadays. 

Declaring a function or a variable inline means that in each translation unit that odr-uses that function/variable, there must be one definition for that function/variable, and all of the definitions for that function/variable, across all translation units, must be **token-for-token and entity-for-entity identical**, meaning all the entities in every occurrence of the inline definition must refer to the exact same objects, in memory. This can be done by `#include`ing a header file containing the definition for that function/variable in different source files - and you should take care that all source files use the same header file for each definition so that all the relevant files get rebuilt when you change the definition. You will still violate the ODR if you have local macros that rewrite tokens or type aliases that change meanings of tokens in the inline definitions. If the inline function/variable has external linkage then it must be declared `extern` in every translation unit. 

Also, with the inline variables introduced in C++17 you can now initialize static members inside a class:

```cpp
struct S
{
    inline static int i = 42;
};
int main(){}
```

So you can define static members inside class definitions and put those in header files that are included in many source files. 

If you didn't have the inline keyword you'd have to initialize static members out of class. As Stroustrup [explains](http://www.stroustrup.com/bs_faq2.html#in-class):

>So why do these inconvenient restrictions exist? A class is typically declared in a header file and a header file is typically included into many translation units. However, to avoid complicated linker rules, C++ requires that every object has a unique definition. That rule would be broken if C++ allowed in-class definition of entities that needed to be stored in memory as objects. See D&E for an explanation of C++'s design trade-offs.

So, the inline keyword allows us to get around the ODR, in this case for static member variables. 

### Subtle ODR violations with inline functions

C++ Templates: The Complete Guide is a great book that has a great [summary of the ODR in its appendix](https://flylib.com/books/en/3.401.1.177/1/). It would be better to just go read that than read this section. It goes into far more detail and is much deeper than the treatment here. 

I have seen it said that the linker does something like randomly pick one of the definitions (since they're all guaranteed to be the same anyway). Certainly every inline function and variable with external linkage all have the same address in every single translation unit, according to C++17 Standard Section 10.1.6 The inline specifier:

>An inline function or variable shall be defined in every translation unit in which it is odr-used and shall have exactly the same definition in every case ([basic.def.odr]). [ Note: A call to the inline function or a use of the inline variable may be encountered before its definition appears in the translation unit. — end note ] If the definition of a function or variable appears in a translation unit before its first declaration as inline, the program is ill-formed. If a function or variable with external linkage is declared inline in one translation unit, it shall be declared inline in all translation units in which it appears; no diagnostic is required. **An inline function or variable with external linkage shall have the same address in all translation units**. [ Note: A static local variable in an inline function with external linkage always refers to the same object. A type defined within the body of an inline function with external linkage is the same type in every translation unit. — end note ]

This gives some intuition to the requirement for all your inline definitions to be completely identical in both tokens and meaning. So, differing tokens is relatively easy to spot but differing meanings can be more subtle. You can have the same token sequence but still not have the same meaning. 

The canonical example of such an ODR violation is using a static variable inside an inline function:

```cpp
static int count;
inline void f(){
    count++;
}
int main(){}
```

Here the variable count is static and declared in global namespace therefore it has internal linkage. The inline function f(), which has external linkage, therefore refers to a different entity in every translation unit in which it is defined. This is a classic violation of ODR. 

[This is another great example](https://stackoverflow.com/questions/31643595/does-the-following-actually-violate-the-odr):

>```cpp
>struct piecewise_construct_t {};
>constexpr piecewise_construct_t piecewise_construct = {};
>
>const int magic_number = 42;
>
>inline std::tuple<int> make_magic() {
>return std::tuple<int>( piecewise_construct, magic_number );
>}
>```
>This function violates the ODR ([basic.def.odr] §3.2/6 ) twice because neither of the constructor 2 arguments receives an lvalue-to-rvalue conversion. They are therefore passed by address, but the address depends on the TU because const (and constexpr) implies internal linkage.

Again, violation of ODR by using entities with internal linkage. Here the inline function make_magic uses the names piecewise_construct and magic_number. These are non-volatile, non-inline, const-qualified variables not declared extern, declared in namespace scope. Refer to cppreference:

>Any of the following names declared at namespace scope have internal linkage:  
>- non-volatile non-inline (since C++17) const-qualified variables (including constexpr) that aren't declared extern and aren't previously declared to have external linkage.

So these two variables have internal linkage, which means each definition of the make_magic function in different translation units refer to different entities. Bam, instant ODR violation. The fun never stops. 

### Other ODR exemptions

Besides `inline` functions and variables, other things that are exempt from the ODR ( can have multiple definitions across translation units) include class types, enumeration types, concepts, and "most" templates. In other words the following code:

a.cpp:
```cpp
class C{};
```
b.cpp
```cpp
class C{};
```
Where a.cpp and b.cpp are compiled into one program, is legal, because the same class definition appears in each source file. There are also a [bunch of requirements](http://en.cppreference.com/w/cpp/language/definition) that you have to satisfy in order for there to not be undefined behavior:

>There can be more than one definition in a program, as long as each definition appears in a different translation unit, of each of the following: class type, enumeration type, inline function with external linkage inline variable with external linkage (since C++17), class template, non-static function template, static data member of a class template, member function of a class template, partial template specialization, concept, (since C++20) as long as all of the following is true:
>
>- each definition consists of the same sequence of tokens (typically, appears in the same header file)  
>- name lookup from within each definition finds the same entities (after overload-resolution), except that constants with internal or no linkage may refer to different objects as long as they are not ODR-used and have the same values in every definition.  
>- overloaded operators, including conversion, allocation, and deallocation functions refer to the same function from each definition (unless referring to one defined within the definition)  
>- the language linkage is the same (e.g. the include file isn't inside an extern "C" block)  
>- the three rules above apply to every default argument used in each definition  
>- if the definition is for a class with an implicitly-declared constructor, every translation unit where it is odr-used must call the same constructor for the base and members  
>- if the definition is for a template, then all these requirements apply to both names at the point of definition and dependent names at the point of instantiation  
>- If all these requirements are satisfied, the program behaves as if there is only one definition in the entire program. Otherwise, the behavior is undefined.  

This is why in header files you often see extern variables (and inline variables), function declarations (and inline function definitions), and class definitions, because these are legal to have in multiple translation units. 

# Basic concepts

## Declarations and Definitions

I like to think of declarations as "what the compiler needs" and definitions as "what the linker needs". 

What is a declaration? In C++ a declaration is a statement which introduces a new named entity of a specified type (where the name is valid is determined by its scope and linkage), such as type, function, or variable. As explained before, the compiler needs you to declare an identifier (a name) before using it, so that it knows what "kind of thing" the identifier refers to. We shall call those declarations that are not also definitions "pure declarations". 

What is a definition? In C++ a definition is a declaration which provides enough information to the "program" in order for it to know how to use the entity referred to by a name. Definitions do not have to be provided at compile time, but the symbol resolver needs it, because it's needed when you want to actually use the thing (e.g you need to find the definition for a function before you can actually call it). If the entity is an object, then the definition of that object allocates memory to it. 

Another way to think about it is that a definition provides both the interface and the implementation, whereas a pure declaration only provides the interface. 

I will quote the C++17 standard (N4659) here for completeness (I added some stuff from cppreference):
```
6.1 Declarations and definitions [basic.def]
1 A declaration (Clause 10) may introduce one or more names into a translation unit or redeclare
names introduced by previous declarations. If so, the declaration specifies the interpretation and
attributes of these names. A declaration may also have effects including:
(1.1) — a static assertion (Clause 10),
(1.2) — controlling template instantiation (17.7.2),
(1.3) — guiding template argument deduction for constructors (17.9),
(1.4) — use of attributes (Clause 10), and
(1.5) — nothing (in the case of an empty-declaration).

2 A declaration is a definition unless 
(2.1) — it declares a function without specifying the function’s body (11.4),
(2.2) — it contains the extern specifier (10.1.1) or a linkage-specification (10.5) and neither
an initializer nor a function-body,
(2.3) — it declares a non-inline static data member in a class definition (12.2, 12.2.3),
(2.4) — it declares a static data member outside a class definition and the variable was defined
within the class with the constexpr specifier (this usage is deprecated; see D.1),
(2.5) — it is a class name declaration (12.1),
(2.6) — it is an opaque-enum-declaration (10.2),
(2.7) — it is a template-parameter (17.1),
(2.8) — it is a parameter-declaration (11.3.5) in a function declarator that is not the declarator
of a function definition,
(2.9) — it is a typedef declaration (10.1.3),
(2.10) — it is an alias-declaration (10.1.3),
(2.11) — it is a using-declaration (10.3.3),
(2.12) — it is a deduction-guide (17.9),
(2.13) — it is a static_assert-declaration (Clause 10),
(2.14) — it is an attribute-declaration (Clause 10),
(2.15) — it is an empty-declaration (Clause 10),
(2.16) — it is a using-directive (10.3.4),
(2.17) — it is an explicit instantiation declaration (17.7.2), or
(2.18) — it is an explicit specialization (17.7.3) whose declaration is not a definition.

[Example: 
All but one of the following are definitions:
int a; // defines a
extern const int c = 1; // defines c
int f(int x) { return x+a; } // defines f and defines x
struct S { int a; int b; }; // defines S, S::a, and S::b
struct X { // defines X
    int x; // defines non-static data member x
    static int y; // declares but doesn't define static data member y
    X(): x(0) { } // defines a constructor of X
};
int X::y = 1; // defines X::y
enum { up, down }; // defines up and down
namespace N { int d; } // defines N and N::d
namespace N1 = N; // defines N1
X anX; // defines anX

whereas these are just declarations:
int f(int); // declares f
struct S; // declares S
extern int a; // declares a
extern const int c; // declares c
typedef int Int; // declares Int
typedef S S2; // declares S2 (S may be incomplete)
using S2 = S; // declares S2 (S may be incomplete)
using N::d; // declares d
— end example ]
```


## Scope, Linkage, Lifetime, and Storage Duration

This section is based off of the [cppreference page](http://en.cppreference.com/w/cpp/language/storage_duration). 

C++ has unfortunately inherited the confusion that are the "storage class specifiers" from C. Although storage duration and linkage are really unrelated concepts, they are unfortunately linked together by the storage class specifiers `static` and `extern`. 

Scope and linkage are properties of names. Lifetime and storage duration are properties of objects. These are very distinct concepts - just because a name is not visible in another translation unit doesn't mean that the functions in that translation unit cannot access the object that the name refers to. 

Lifetime is the period of time between when an object is created and destroyed. Storage duration is a set of rules governing the lifetime of the storage of an object, determining when the storage for an object will be allocated and deallocated. The lifetime of an object is nested within the lifetime of its storage.  

Scope and linkage both determine the visibility of names. The difference is: "Scope determines what you can see within a translation unit. Linkage determines what you can see across translation units." - Dan Saks. Another way to look at it is to say that scope is for compilers whereas linkage is for linkers. 

### Linkage

From the C++17 standard section 6.5:

>A name is said to have linkage when it might denote the same object, reference, function, type, template, namespace or value as a name introduced by a declaration in another scope:
>
>When a name has external linkage, the entity it denotes can be referred to by names from scopes of other translation units or from other scopes of the same translation unit.
>
>When a name has internal linkage, the entity it denotes can be referred to by names from other scopes in the same translation unit.
>
>When a name has no linkage, the entity it denotes cannot be referred to by names from other scopes.

From cppreference:

>If a name has linkage, it refers to the same entity as the same name introduced by a declaration in another scope. If a variable, function, or another entity with the same name is declared in several scopes, but does not have sufficient linkage, then several instances of the entity are generated.

The difference between no/internal linkage and external linkage is fairly straightforward and easy to understand - external linkage means a name is visible in other translation units, internal/no linkage means the name is not visible in other translation units. That means that if you declare a name with external linkage in multiple translation units, they all refer to the same entity. But if you declare a name with internal linkage in multiple translation unit then each declaration refers to a different entity. The difference between no linkage and internal linkage seems to be pretty subtle however and I do not understand it myself. But they are clearly different and distinct, because for example non-type template parameters must have linkage (whether internal or external). This section aims to give a brief overview of the concepts and does not go in depth on the rules, which you can find on cppreference. 

A name with no linkage can only be referred to from the scope that it's declared in. All local variables that are not declared extern have no linkage:

```cpp
void f(){
    static int i = 5;
} int main(){}
```

You can't access the variable `i` from outside the scope of the function `f()`. 

A name with internal linkage can be referred to from all scopes in its translation unit, but not from other translation units. Static variables and functions declared in namespace scope, as well as everything inside unnamed namespaces, have internal linkage:

```cpp
static int i;
void f(){
    i = 5;
}
int main(){
    i = 6;
}
```
Here, the variable `i` can be accessed from anywhere within the translation unit. However, it cannot be accessed from other translation units. 

A name with external linkage can be referred to from other translation units. Basically everything else declared in namespace scope, and local variables declared extern have external linkage:

a.cpp
```cpp
struct S{
    static int i;
};
int S::i = 8; //This is not susceptible to the static initialization order fiasco because 8 is a literal
void f() { S::i++; }
```
b.cpp
```cpp
#include <iostream>
struct S{
    static int i;
};
void f();
int main(){
    std::cout << S::i << '\n';
    S::i = 99;
    std::cout << S::i << '\n';
    f();
    std::cout << S::i;
}
```
Command: `clang++ b.cpp a.cpp -Weverything -std=c++17`  
Output:
```
8
99
100
```

### Scope

Scope is determined by where a name is declared. C++ has static (or lexical) scoping meaning the "visibility" of a variable throughout all parts of the program is always known at compile time. Keeping in mind what we know about the C++ compilation model, it is clear that since the C++ compiler only processes one translation unit at a time (separate compilation), it cannot possibly know about any other translation units when it is compiling a translation unit, then the "visibility" or scope of a variable can only be throughout a translation unit. In other words you cannot declare a variable in one translation unit and expect to be able to use that variable without declaring it again in another translation unit. This is something like a "declare before use" rule. 

There are 7 scopes in C++: block scope, namespace scope, class scope, function parameter scope, function scope, enumeration scope, and template parameter scope. I think of these 3 are most important for beginners: block, namespace, and class. The rules are very simple: If it's declared in a block, then it has block scope. If it's declared in a class, then it has class scope. If it's declared in a namespace (outside of blocks and classes etc), then it has namespace scope. 

There is a very readable explanation of what scope is in the C++17 standard:

>Every name is introduced in some portion of program text called a declarative region, which is the largest part of the program in which that name is valid, that is, in which that name may be used as an unqualified name to refer to the same entity. **In general, each particular name is valid only within some possibly discontiguous portion of program text called its scope.** To determine the scope of a declaration, it is sometimes convenient to refer to the potential scope of a declaration. **The scope of a declaration is the same as its potential scope unless the potential scope contains another declaration of the same name. In that case, the potential scope of the declaration in the inner (contained) declarative region is excluded from the scope of the declaration in the outer (containing) declarative region.**

#### Name hiding

Normally, a name declared in a scope is also visible in the scopes "inside" that scope. However, it is possible to declare a name inside a scope, and the name in the "inner" scope hides the name from the "outer" scope. 

This is the so-called "variable shadowing" or "name hiding" (as it's called in the C++17 standard), it occurs as a result of how name lookup works in C++. You can and should get the compiler to warn you about variable shadowing. 

>In all the cases listed in [basic.lookup.unqual], the scopes are searched for a declaration in the order listed in each of the respective categories; name lookup ends as soon as a declaration is found for the name.

Name lookup generally proceeds from most local to least local scope. Normally it looks something like this: block -> class -> global. 

Here is an example from C++17 standard:

>[ Example:
>```cpp
>class B { };
>namespace M {
>  namespace N {
>    class X : public B {
>      void f();
>    };
>  }
>}
>void M::N::X::f() {
>  i = 16;
>}
>
>// The following scopes are searched for a declaration of i:
>// 1) outermost block scope of M​::​N​::​X​::​f, before the use of i
>// 2) scope of class M​::​N​::​X
>// 3) scope of M​::​N​::​X's base class B
>// 4) scope of namespace M​::​N
>// 5) scope of namespace M
>// 6) global scope, before the definition of M​::​N​::​X​::​f
>```
>— end example ] 

If one variable hides another you can often access the hidden variable using the scope resolution operator `::`:

```cpp
#include <iostream>
struct C {
  int value = 5;
  int f(int value){ //the parameter shadows the member
    return C::value;
  }
};
int main(){
  C c;
  std::cout << c.f(99);
}
```

#### Block scope

C++17 Standard Section 6.3.3 Block scope:

>A name declared in a block is local to that block; it has block scope. Its potential scope begins at its point of declaration and ends at the end of its block. A variable declared at block scope is a local variable.
>
>Names declared in the init-statement, the for-range-declaration, and in the condition of if, while, for, and switch statements are local to the if, while, for, or switch statement (including the controlled statement), and shall not be redeclared in a subsequent condition of that statement nor in the outermost block (or, for the if statement, any of the outermost blocks) of the controlled statement; see [stmt.select].

Note that local variables in a function definition are of block scope (NOT function scope). For example:

```cpp
void do_something() { 
    int a; //a is block scope NOT function scope!
}
```

#### Namespace scope

C++17 Standard Section 6.3.6 Namespace scope:

>Entities declared in a namespace-body are said to be members of the namespace, and names introduced by these declarations into the declarative region of the namespace are said to be member names of the namespace. A namespace member name has namespace scope. Its potential scope includes its namespace from the name's point of declaration onwards; and for each using-directive that nominates the member's namespace, the member's potential scope includes that portion of the potential scope of the using-directive that follows the member's point of declaration. [ Example:
>```cpp
>namespace N {
>  int i;
>  int g(int a) { return a; }
>  void q();
>}
>namespace { int l=1; }
>// the potential scope of l is from its point of declaration to the end of the translation unit
>
>namespace N {
>  int g(char a) {   // overloads N​::​g(int)
>    return l+a;     // l is from unnamed namespace
>  }
>  int i;            // error: duplicate definition
>  int q();          // error: different return type
>}
>```
>— end example ]

Technically there is no "global scope" in C++. So-called "global" names are names declared in the global namespace, and have namespace scope. Namespace scope extends to the end of the namespace, names with namespace scope may be accessible from other translation units (depending on linkage). 

If you write `using namespace std;` (considered bad practice BTW because it pollutes your namespace, although TBH your namespace will likely get polluted anyway by the C library (cstdlib) functions such as div) it does [this](http://en.cppreference.com/w/cpp/language/namespace):

> using-directive: From the point of view of unqualified name lookup of any name after a using-directive and until the end of the scope in which it appears, every name from ns_name is visible as if it were declared in the nearest enclosing namespace which contains both the using-directive and ns_name.

Example:

```cpp
namespace { char x = 'a'; }
namespace A { float x = 1.0;}
namespace B { int x = 5;}
int main(){
  long x = 9;
  using A::x;
  using namespace B;
  //A::x and long x both shadow the x from the unnamed namespace and the x from B at this point. 
}
```
You'll see a compilation error on this because the name introduced by `long x = 9;` and `using A::x;` clash with each other (it's considered a redeclaration), but if you remove either then it compiles fine because both shadow the other two names. If you remove both and then try to use `x` then you'll get an ambiguous reference error. 

##### Namespaces

Namespaces are a way to avoid name collisions. Useful when using multiple libraries. Using long names with prefixes can be awkward so you can use aliases like this: `namespace s = std;`. Namespaces are discontiguous, so you can do this:

```cpp
namespace std{
  void f();
}
```

Though putting stuff in namespace std is obviously not recommended. 

#### Class scope

>The potential scope of a name declared in a class consists not only of the declarative region following the name's point of declaration, but also of all function bodies, default arguments, noexcept-specifiers, and default member initializers ([class.mem]) in that class (including such things in nested classes).

The scope of a class member extends from the start of a class declaration to the end the class declaration. The class scope includes not only the "class declaration" itself but also all the member function bodies etc as well. 

#### Function scope

The **ONLY** thing in C++17 that has function scope are labels. cppreference has a good explanation:

>Only labels have function scope. They are available within an entire function, even before they are declared and regardless of if they're declared in a sub-block.
>```cpp
>void label_example() {
>    //SomeLabel is available here
>    if (...) {
>        for (...) {
>            SomeLabel:
>            /* Code here */
>        }
>    }
>    //SomeLabel is available here
>    if (...) {
>        //SomeLabel is available here
>    }
>}
>```
>Note that even though SomeLabel is declared in a deeply nested block, it's available throughout the entire function.

Remember that local variables declared in the body of a function are block scope, not function scope. 

#### Other scopes

There is also function parameter scope, enumeration scope and template parameter scope. These aren't listed in TC++PL 4E so I guess they're not that important to know about...

#### Some funny stuff

I guess this is mainly good for bamboozling your friends. 

>The point of declaration for a name is immediately after its complete declarator and before its initializer (if any), except as noted below. 

```
[ Example:
unsigned char x = 12;
{ unsigned char x = x; }
Here the second x is initialized with its own (indeterminate) value.
— end example ]
```
>Note: A name from an outer scope remains visible up to the point of declaration of the name that hides it. 
```
[ Example:
const int  i = 2;
{ int  i[i]; }
declares a block-scope array of two integers. 
— end example ]
```

### Storage duration

There are 4 storage durations but a beginner really needs to know these 3: static, dynamic, and automatic. Static storage means it lives from start to end of the program, dynamic storage means it lives until deleted, and automatic storage means it lives until end of block - allocated at start of enclosing block and deallocated at the end of the block. 

Storage duration rules are also very simple. It works like this: Anything declared `thread_local` has thread storage. Anything allocated with `new` has dynamic storage. Anything with block scope that's not `static`, `extern`, or `thread_local` has automatic storage, as well as all function parameters. Everything else has static storage (i.e anything with namespace scope or marked `static` or `extern` unless also marked `thread_local`).

From [cppreference](http://en.cppreference.com/w/cpp/language/storage_duration):

>All objects in a program have one of the following 4 storage durations:
>
>**automatic** storage duration. The object is allocated at the beginning of the enclosing code block and deallocated at the end. All local objects have this storage duration, except those declared static, extern or thread_local.
>
>**static** storage duration. The storage for the object is allocated when the program begins and deallocated when the program ends. **Only one instance of the object exists**. All objects declared at namespace scope (including global namespace) have this storage duration, plus those declared with static or extern.
>
>**dynamic** storage duration. The object is allocated and deallocated per request by using dynamic memory allocation functions.
>
>**thread** storage duration. The object is allocated when the thread begins and deallocated when the thread ends. Each thread has its own instance of the object. Only objects declared thread_local have this storage duration. thread_local can appear together with static or extern to adjust linkage.

Example: If `char c;` is a statement in the global namespace, then `c` is a variable declared in the global namespace, therefore it has static storage duration. All variables of static storage duration are automatically zero-initialized if not explicitly initialized, therefore `c` is zero-initialized. This has no runtime overhead because this initialization can happen before the program starts. This is in contrast to automatic variables, which cannot be initialized before the program starts, which is why they are not initialized - because that would introduce runtime overhead (refer back to C++'s philosophy of "you don't pay for what you don't use").

### Storage class specifiers

As of C++17, there are only 3 storage class specifiers:

The `static` storage class specifier indicates static storage duration with internal linkage. 

The `extern` storage class specifier indicates static storage duration with external linkage. 

The `thread_local` storage class specifier indicates thread storage duration. When `thread_local` is combined with `static` or `extern`, the object still has thread storage duration, the `static` or `extern` merely determines the linkage of the object (except for static data members which always have external linkage).

### auto

Until C++11, `auto` was a keyword that indicated automatic (as opposed to static) storage duration. It was pretty much never used since its use was always either redundant or illegal (you could only use it with variables that were already auto by default). Similarly in C++17 the `register` storage class specifier was deprecated since it was completely pointless (just like `auto`, but unlike `auto` it has not been repurposed and as of C++17 is "unused and reserved"). Since C++11 the `auto` keyword now means something completely different (automatic type deduction). So if you ever see a C++ book/tutorial/course mentioning the `auto` keyword in the pre-C++11 sense but not in the C++11 sense, then you know it's outdated. 

[GOTW #94](https://herbsutter.com/2013/08/12/gotw-94-solution-aaa-style-almost-always-auto/) advocates the use of auto for every variable declaration (AAA - Almost Always Auto), e.g using:

```cpp
auto x = init; //when you don’t need to commit to a specific type, and
auto x = type{ init }; //when you do want to commit to a specific type by naming a type
```

# Basic knowledge

## Extraction operators 
So as you might have seen earlier we use the <code>>></code> and <code><<</code> operators which are the "insertion" and "extraction" operators for streams. This is one example of overloaded operators - they mean something different when applied to stream objects as opposed to int objects. People argue all day about when you should/should not use operator overloading. One advice is that if it makes the code easier to read then use it. 

## The main function
The main function should return a status code (you may omit the return statement since 0 is returned if the return statement is omitted). Usually 0 indicates successful termination but sometimes you want to return some other int to indicate an error has occurred. <code>&lt;cstdlib&gt;</code> provides EXIT_SUCCESS and EXIT_FAILURE for this purpose - it is not required that EXIT_SUCCESS equals zero. 

There are only a few functions which do not require a return statement:
- The main function
- void functions (void is not an object type)
- Constructors
- Destructors

When a return statement is omitted, control automatically returns to the caller at the end of the function (disregarding try-catch and exceptions), as if there was a `return;` statement there. 

There are only 2 legal signatures for the main function:

- `int main()`
- `int main(int argc, char **argv)`

Where argv is a pointer to an array of char*s (C strings) which are the arguments supplied to the program (often passed via the command line, this is specified by the POSIX standard) and argc is the number of strings in argv. The standard specifies that argv[argc] is a null pointer. Note that since on Linux a program can choose to call `exec*` with whatever arguments it wants to, there's no guarantee that argv[0] is the name of the program (although it usually is used by shells by convention). 

According to the C++17 standard section 6.8.3:

>The function main shall not be used within a program. The linkage ([basic.link]) of main is implementation-defined. A program that defines main as deleted or that declares main to be inline, static, or constexpr is ill-formed. 

Hence, the standard says the main function can be only entered once. Although many compilers will allow you to call main, this is not allowed by the standard and you probably shouldn't do it. 

## Naming

Variables must begin with a letter (the underscore character is considered a letter). Variable names consist of letters and digits but cannot start with a digit. It is also not recommended to start a variable name with an underscore since these names are reserved. 

## Narrowing conversion

The use of the `=` allows narrowing conversions whereas the uniform initialization syntax `{}` does not which makes it safer, so when in doubt use the uniform initialization syntax (this is Stroustrup's advice). For example:

```cpp
int main(){
  int a = 5.5; //this is fine
  int b {5.5}; //this will throw an error
}
```

## Overflow

Arithmetic overflow is defined for unsigned but not for signed integral types. With unsigned overflow the value simply "wraps around". So for example if an unsigned char with value 255 is incremented by 1 then its value becomes 0. Example:

```cpp
#include <iostream>
#include <climits>
int main(){
        unsigned int c = UINT_MAX, d = 1;
        unsigned long long x = c + d;
        std::cout << UINT_MAX << std::endl;
        std::cout << ULLONG_MAX << std::endl;
        std::cout << x;
}
```

If your architecture is the same as mine, then you might see this:

```
4294967295
18446744073709551615
0
```

As you can see, when we add 2 unsigned ints, the result of that expression is also an unsigned int (and the same for multiplication and division). Since we added 1 to UINT_MAX, unsigned overflow has caused the resulting value to be 0, which is then assigned to x even though x is a long long that can represent UINT_MAX + 1. See integer promotion rules below for further information. 

Don't forget that signed overflow and division by zero are undefined behavior: they do not throw exceptions. 

## Fundamental Integral Types

In C++17 standard section 6.9.1 it says:

>Specializations of the standard library template std::numeric_limits (21.3) shall specify the maximum and minimum values of each arithmetic type for an implementation.

>There are five standard signed integer types : “signed char”, “short int”, “int”, “long int”, and “long long int”. In this list, each type provides at least as much storage as those preceding it in the list. 

>For each of the standard signed integer types, there exists a corresponding (but different) standard unsigned integer type: “unsigned char”, “unsigned short int”, “unsigned int”, “unsigned long int”, and “unsigned long long int”

>The signed and unsigned integer types shall satisfy the constraints given in the C standard, section 5.2.4.2.1.

So the C++17 standard refers to the C standard. Let's look at the C standard section 5.2.4.2.1. 

>**Their implementation-defined values shall be equal or greater in magnitude (absolute value) to those shown, with the same sign.**

This means that the list of limits at the bottom of this section is a list of minimum magnitude requirements. A char MUST be able to represent the range of values 0-127 but it can represent more if the implementation chooses to. But you can only rely on it providing that much storage, so you definitely should not try to store negative values or values greater than 127 into a char. 

Almost forgot to mention: The number of bits in a byte in C++ is defined by the CHAR_BIT macro, the standard says CHAR_BIT >= 8. It could be 8 bits or 9 bits or 16 or 32 or anything. But the size of a char is always 1 byte. And everything in C++ is measured in bytes (using the `sizeof` keyword), so the size of all the other types are a multiple of 1 byte. More specifically, we have:

```
1 == sizeof(char) <= sizeof(short) <= sizeof(int) <= sizeof (long)
```

So you could have one byte being 32 bits and all the types char, short, int and long all consisting of one byte. That would actually be consistent with the standard. 

It has been claimed that `sizeof` is a compile-time operator in C++, unlike in C (which has variable length arrays). Do note that whatever you pass to sizeof will not be evaluated:

```cpp
sizeof(x++); //This will not increment x. 
```

Another thing to note: The signedness of char is implementation-defined. For portability therefore it's probably a good idea to use signed vs unsigned char rather than char, as the [SEI CERT C Coding Standard recommends](https://wiki.sei.cmu.edu/confluence/display/c/INT07-C.+Use+only+explicitly+signed+or+unsigned+char+type+for+numeric+values). From the C++17 standard:

>In any particular implementation, a plain char object can take on either the same values as a signed char or an unsigned char; which one is implementation-defined.

Char, signed char, and unsigned char are treated as distinct types, although they must occupy the same amount of storage. 

```
CHAR_BIT 8
SCHAR_MIN -127 //signed char
SCHAR_MAX +127 
UCHAR_MAX 255 //unsigned char, 2^8-1
SHRT_MIN -32767 //short
SHRT_MAX +32767 
USHRT_MAX 65535 //unsigned short, 2^16-1
INT_MIN -32767 //int
INT_MAX +32767
UINT_MAX 65535 //unsigned int, 2^16-1
LONG_MIN -2147483647 //long
LONG_MAX +2147483647 
ULONG_MAX 4294967295 //unsigned long, 2^32-1
LLONG_MIN -9223372036854775807 //long long
LLONG_MAX +9223372036854775807
ULLONG_MAX 18446744073709551615 //unsigned long long, 2^64-1
```

All unsigned types all have 0 as their minimum possible value, as far as I know. 

## Integer promotion rules

[There is a good page on integer promotion rules from the SEI CERT C Coding standard](https://wiki.sei.cmu.edu/confluence/display/c/INT02-C.+Understand+integer+conversion+rules). 

>Integer types smaller than int are promoted when an operation is performed on them. If all values of the original type can be represented as an int, the value of the smaller type is converted to an int; otherwise, it is converted to an unsigned int. Integer promotions are applied as part of the usual arithmetic conversions to certain argument expressions; operands of the unary +, -, and ~ operators; and operands of the shift operators. The following code fragment shows the application of integer promotions:
>```cpp
>char c1, c2;
>c1 = c1 + c2;
>```
>Integer promotions require the promotion of each variable (c1 and c2) to int size. The two int values are added, and the sum is truncated to fit into the char type. 

[From cppreference](http://en.cppreference.com/w/cpp/language/implicit_conversion):

>In particular, **arithmetic operators do not accept types smaller than int as arguments, and integral promotions are automatically applied after lvalue-to-rvalue conversion, if applicable**. This conversion always preserves the value.
>
>- the type bool can be converted to int with the value false becoming ​0​ and true becoming 1.
>
>- signed char or signed short can be converted to int;
>
>- unsigned char or unsigned short can be converted to int if it can hold its entire value range, and unsigned int otherwise;
>
>- an unscoped enumeration type whose underlying type is not fixed can be converted to the first type from the following list able to hold their entire value range: int, unsigned int, long, unsigned long, long long, or unsigned long long
>
>- a bit field type can be converted to int if it can represent entire value range of the bit field, otherwise to unsigned int if it can represent entire value range of the bit field, otherwise no integral promotions apply;
>
>- A prvalue of type float can be converted to a prvalue of type double. The value does not change.

Let's revisit the "usual arithmetic conversions" (which often happen when a binary operator expecting numeric types is called) again with some more rules now:


>- If either operand is of type long double, the other shall be converted to long double.
>
>- Otherwise, if either operand is double, the other shall be converted to double.
>
>- Otherwise, if either operand is float, the other shall be converted to float.
>
>- Otherwise, the integral promotions (7.6) shall be performed on both operands. Then the following rules shall be applied to the promoted operands:
>
>    - If both operands have the same type, no further conversion is needed.
>    
>    - Otherwise, if both operands have signed integer types or both have unsigned integer types, the operand with the type of lesser integer conversion rank shall be converted to the type of the operand with greater rank.
>    
>    - Otherwise, if the operand that has unsigned integer type has rank greater than or equal to the rank of the type of the other operand, the operand with signed integer type shall be converted to the type of the operand with unsigned integer type.
>    
>    - Otherwise, if the type of the operand with signed integer type can represent all of the values of the type of the operand with unsigned integer type, the operand with unsigned integer type shall be converted to the type of the operand with signed integer type.

## Pre and post increment

++i first increments i, and then returns a reference to i (equivalent to i+=1), whereas i++ first makes a copy of i, returns it, and then increments i. Example:

```cpp
int a = 0, b = 0;
int x = a++; //x = 0
int y = ++b; //y = 1
a = a++; //undefined behavior
```
As the Google C++ Style Guide says:

>When the return value is ignored, the "pre" form (++i) is never less efficient than the "post" form (i++), and is often more efficient. This is because post-increment (or decrement) requires a copy of i to be made, which is the value of the expression. If i is an iterator or other non-scalar type, copying i could be expensive. 

Thus, if you don't need the return value, always pre-increment. 

## Order of evaluation

If you don't remember anything else about order of evaluation, just remember this: in almost all cases, order of evaluation of subexpressions in a C++ expression is unspecified. 

The way to go about things in C++ is to assume every expression exhibits unspecified or undefined behavior unless the standard specifically says that it doesn't. This is counterintuitive, but if you try to proceed from the other way, i.e assume all expressions are well defined and then try to prove individual expressions undefined, then you are going to have a bad time. 

For example, it is stated everywhere that the following statement:

```cpp
int i = 0;
i = i++;
```

Exhibits undefined behavior. [Here](https://stackoverflow.com/questions/17400137/order-of-evaluation-and-undefined-behaviour) is a StackOverflow answer that attempts to answer the question of exactly why it's undefined behavior:

>The definitions of "side effects" and "evaluation" is found in paragraph 1.9/12:
>
>Accessing an object designated by a volatile glvalue (3.10), modifying an object, calling a library I/O function, or calling a function that does any of those operations are all side effects, which are changes in the state of the execution environment. Evaluation of an expression (or a sub-expression) in general includes both value computations (including determining the identity of an object for glvalue evaluation and fetching a value previously assigned to an object for prvalue evaluation) and initiation of side effects.
>
>The next relevant part is paragraph 1.9/15:
>
>Except where noted, evaluations of operands of individual operators and of subexpressions of individual expressions are unsequenced. [...] The value computations of the operands of an operator are sequenced before the value computation of the result of the operator. If a side effect on a scalar object is unsequenced relative to either another side effect on the same scalar object or a value computation using the value of the same scalar object, the behavior is undefined.
>
>Now let's see, how to apply this to the two examples.
>
>`i = i++;`
>
>This is the postfix form of increment and you find its definition in paragraph 5.2.6. The most relevant sentence reads:
>
>The value computation of the ++ expression is sequenced before the modification of the operand object.
>
>For the assignment expression see paragraph 5.17. The relevant part states:
>
>In all cases, the assignment is sequenced after the value computation of the right and left operands, and before the value computation of the assignment expression.
>
>Using all the information from above, the evaluation of the whole expression is (this order is not guaranteed by the standard!):
>
>- value computation of i++ (right hand side)
>- value computation of i (left hand side)
>- modification of i (side effect of ++)
>- modification of i (side effect of =)
>
>All the standard guarantees is that the value computations of the two operands is sequenced before the value computation of the assignment expression. But the value computation of the right hand side is only "reading the value of i" and not modifying i, the two modifications (side effects) are not sequenced with respect to each other and we get undefined behavior.

Now imagine doing that in every single situation where you're not sure if an expression exhibits unspecified behavior. Hmm...

Some other relevant quotes from the standard:

>An expression X is said to be sequenced before an expression Y if every value computation and every side effect associated with the expression X is sequenced before every value computation and every side effect associated with the expression Y. 
>
>Every value computation and side effect associated with a full-expression is sequenced before every value computation and side effect associated with the next full-expression to be evaluated.
>
>Except where noted, evaluations of operands of individual operators and of subexpressions of individual expressions are unsequenced.
>
>The value computations of the operands of an operator are sequenced before the value computation of the result of the operator. 
>
>If a side effect on a memory location ([intro.memory]) is unsequenced relative to either another side effect on the same memory location or a value computation using the value of any object in the same memory location, and they are not potentially concurrent ([intro.multithread]), the behavior is undefined.

Example from C++17 standard:

```cpp
void g(int i) {
  i = 7, i++, i++;              // i becomes 9
  i = i++ + 1;                  // the value of i is incremented
  i = i++ + i;                  // the behavior is undefined
  i = i + 1;                    // the value of i is incremented
}
```

[There is also a useful cppreference page on this](http://en.cppreference.com/w/cpp/language/eval_order): 

>Order of evaluation of the operands of almost all C++ operators (**including the order of evaluation of function arguments in a function-call expression and the order of evaluation of the subexpressions within any expression**) is unspecified. The compiler can evaluate operands in any order, and may choose another order when the same expression is evaluated again.
>
>There are exceptions to this rule which are noted below.
>
>Except where noted below, there is no concept of left-to-right or right-to-left evaluation in C++. This is not to be confused with left-to-right and right-to-left associativity of operators: **the expression f1() + f2() + f3() is parsed as (f1() + f2()) + f3() due to left-to-right associativity of operator+, but the function call to f3 may be evaluated first, last, or between f1() or f2() at run time**.

The general advice from TC++PL 4E is to just not write expressions that reads or modifies an object more than once, unless it uses only a single operator or uses operators to express ordering such as comma, <code>&&</code>, and `||` (since these have left-to-right evaluation with short-circuit behavior). 

For those coming from a Python background unfamiliar with the comma operator, it is useful to distinguish between these:

```cpp
f(a,b); //Passes a and b as arguments to f
f((a,b)); //Passes b as an argument to f
```

### [Some useful things to know](https://en.cppreference.com/w/cpp/language/eval_order)

1. Function arguments are sequenced before the function itself. 
2. The value computation in the post-increment/decrement is sequenced before the side effect. 
3. The side effect in the pre-increment/decrement is sequenced before the value computation (implicit rule). 
4. The left argument of a <code>&&</code> or `||` is sequenced before the right argument. 
5. The first expression in the `?:` (the only ternary) operator is sequenced before the second or third expression. 
6. The modification of the left argument of an assignment operator is sequenced before returning the reference to the modified object. 
7. Every overloaded operator obeys the sequencing rules of the built-in operator it overloads when called using operator notation.

### Proposal P0145 - "but I have heard it works even if you don't believe in it"

If you ever wanted to know what changed between C++14 and C++17, here is [a list directly from ISOCPP](https://isocpp.org/files/papers/p0636r0.html).

On that list is [P0145R3](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2016/p0145r3.pdf), which is primarily a fix for some tricky order-of-evaluation situations that most developers (including Stroustrup) himself thought to be well-defined but in fact turned out not to be. This was the code in question:

```cpp
void f()
{
 std::string s = “but I have heard it works even if you don’t believe in it”;
 s.replace(0, 4, “”).replace(s.find(“even”), 4, “only”).replace(s.find(“ don’t”), 6, “”);
 assert(s == “I have heard it works only if you believe in it”);
}
```

This is supposed to be valid C++ code (and indeed is valid C++, in C++17), but its order of evaluation was unspecified prior to C++17. In particular, `s.find("even")` could be evaluated before or after the `.replace(0, 4, "")` call, when intuitively you'd expect that it should be evaluated after. 

The proposal itself is only 8 pages and well worth reading but here are a few key points:

>Postfix expressions are evaluated from left to right. This includes functions calls and member selection expressions.
>
>Assignment expressions are evaluated from right to left. This includes compound assignments.
>
>Operands to shift operators are evaluated from left to right. 
>
>In summary, the following expressions are evaluated in the order a, then b, then c, then d:
>```
>a.b
>a->b
>a->*b
>a(b1, b2, b3)
>b @= a
>a[b]
>a << b
>a >> b
>```

Where the `@=` symbol stands for any kind of assignment (e.g `/=`, `+=`). So, in a function call, the expression naming the function is first evaluated before the arguments are, same for member reference and so on. 

Note that arguments can still be evaluated in any order, b1 can be evaluated before or after b2 for example. 

## Operator precedence

Operator precedence in C++ is completely fucked. You have to put parentheses around everything otherwise you have to constantly look up the [operator precedence table](http://en.cppreference.com/w/cpp/language/operator_precedence) (might be a good idea to memorize it so you know e.g `==` has precedence 10 and &lt;&lt; has precedence 7). It is hard to provide a useful summary, because the table is already very concise, but it is also a bit too big to memorize easily, but if you don't know it then surprising things can happen because it is not intuitive. For example, the bitwise operations &amp;, `^`, and `|` extremely low precedence, lower than the comparison operator `==` for example. Here are some examples:

```cpp
//These pairs are equivalent:
std::cout << x == y;
(std::cout << x) == y;

std::cout << a & b;
(std::cout << a) & b;

*p++;
*(p++);

*p.b
*(p.b)

++p.b
++(p.b)

x & 1 == 0;
x & (1 == 0);

if (flags & enable_foo != 0)
if (flags & (enable_foo != 0))

a >> b >> c;
(a >> b) >> c;

a == b == c;
(a == b) == c;

a = b = c;
a = (b = c);

a.b.c;
(a.b).c;
```

It is reassuring to know that the comma operator `,` has the lowest precedence, at least. 

Here's one attempt at summarizing it: postfix > prefix > unary > binary arithmetic > comparisons > bitwise > logical > assignment > comma. 

## Literals

In C and C++, 0 means false and anything else is true, including negative numbers. The floating point number 0.0f means false. Probably not a great idea to use floats as bools though. 

A number literal prefixed by a 0 is an octal, by 0x or 0X is a hex, and 0b or 0B is binary. As of C++17 there are hex floating point literals but not octal or binary floating point literals. Floating point literals beginning with a 0 can omit it (e.g 0.4 can be written as .4). 

Append L or l for a long literal, LL or ll for long long, U or u for unsigned. There is no suffix for short. 

2.0f is a float, 2.0 is a double, 2.0L is a long double. There are no unsigned floating point types. 

'c' is a char, "c" is a string literal. If you run sizeof on these you will find that 'c' occupies 1 byte of storage whilst "c" occupies two bytes. 

You can have literals that are too big, see C++17 Standard 5.13.2 Integer literals:

>A program is ill-formed if one of its translation units contains an integer literal that cannot be represented by any of the allowed types.

### String literals
 
C++ string literals are the same as C string literals - they are `\0` terminated which adds an extra char to the string, the type is const char array - that means they are read-only and if you try to modify them (e.g via const_cast) then you get undefined behavior. Remember that char array is **NOT** pointer to char. For example `"a"` is an array of 2 chars - `a` and `\0`. If you do this:

```cpp
const char a[] = "a";
sizeof("a");
sizeof(a);
```
Then you'll see that both of the `sizeof`s return 2. `strlen(a)` will return 1 however, because it doesn't count the `\0`. Similarly if you convert a to a std::string and run the `size()` function you will also get `1`. Note that unlike C strings, a std string can contain `\0` characters in the middle, so strlen and size can return different values if there's a `\0` in the middle of the string.

Note that there are only octal and hexadecimal escape sequences - no decimal. See [here](http://en.cppreference.com/w/cpp/language/escape). 

From C++17 standard section 24.3.2:

>A basic_string is a contiguous container (26.2.1).

So a std::string is guaranteed to be contiguous in memory (just as vectors are - super important that you know this btw). Also, `stdstring[stdstring.size()]` is guaranteed to be `\0` (this was introduced in C++11). 

From C++17 standard section 5.13.5:

>Ordinary string literals and UTF-8 string literals are also referred to as narrow string literals. A narrow string literal has type “array of n const char”, where n is the size of the string as defined below, and has **static storage duration** (6.7).

Ever since C++14 you can use std::string literals. From the literals library there is the [`std::literals::string_literals::operator""s`](http://en.cppreference.com/w/cpp/string/basic_string/operator%22%22s) which directly converts a string literal into a std::string like this:

```cpp
#include <string>
#include <iostream>
 
int main()
{
    using namespace std::string_literals;
 
    std::string s1 = "abc\0\0def";
    std::string s2 = "abc\0\0def"s;
    std::cout << "s1: " << s1.size() << " \"" << s1 << "\"\n";
    std::cout << "s2: " << s2.size() << " \"" << s2 << "\"\n";
}
```
Output:
```
s1: 3 "abc"
s2: 8 "abcdef"
```

As you can see, s1 stopped reading the string literal after 3 letters because it saw the null terminator. If you want to construct s2 with a std::string() constructor call you'd have to pass in the length of the string literal. 

Other types of string literals can be found [here](http://en.cppreference.com/w/cpp/language/string_literal). 

Remember earlier the C++ standard said that string literals have static storage duration. Does that mean that you can take the address of a string literal and it will always be valid? Yes:

```cpp
const char* f(){
  const char a[] = "hah!"; //char array with automatic storage
  const char* s = "haha!"; //pointer to static storage 
  return s;
}
```

This is perfectly valid C++ code and the pointer returned from f will always refer to the same string literal and it will always be valid. Remember that it is illegal to return `a` here because `a` is an array with automatic storage, when the function returns it is no longer guaranteed to be alive, so you will end up with a dangling pointer. Similarly, returning a reference to a temporary (and returning a reference to a variable with automatic storage) is a mistake and is undefined behavior, you'd end up with a dangling reference (even if the reference is to a const). 

If you wanted to return a string from a function you should just return a std::string (and likewise a std::vector if you want to return an array). Since then it would be returning by value rather than by pointer, so it would first (ignoring optimizations) create the string in the function, then copy it to the caller before destroying it. Much safer because you don't have to call delete[] on it. 

You may convert something to string using ostringstream or std::to_string (introduced in C++11). If you care about performance, profile your code. 

## standard IO

cin stops reading when it sees a whitespace (and doesn't read in that whitespace). If you want to read in an entire line including whitespace (but not including the newline) you should use `std::getline(std::cin, line);` instead.


### IOstream manipulators

Output formatting in C++17 is a clusterfuck and no sane person uses the stream manipulators. Mostly they either use printf or an external library like Boost.Format. In fact there was [an ACCU post on this in April 2018](https://accu.org/index.php/journals/2486):

>Actually, there are several such libraries (Boost format, FastFormat, tinyformat, {fmt}, and FollyFormat – and probably something else which I have forgotten to mention). I have to note that, personally, I don’t really care too much which one of the competing new-generation format libraries makes it into the standard (except, probably, for Boost format, which is way too resource-intensive when compared to the alternatives). In general, I (alongside with a very significant portion of the C++ community) just want some standard and better-than-iostream way of formatting human-readable data.
>
>Out of such newer formatting libraries I happen to know {fmt} by Victor Zverovich the best, and it certainly looks very good, satisfying all the points from C++ FAQ, and avoiding all the iostream problems listed above. As {fmt} is also the only new-generation library with an active WG21 proposal [P0645R1], it is the one I’m currently keeping my fingers crossed for. (NB: in the past, there was another proposal, [N3506], but it looks pretty much abandoned).

If you are a masochist and want to use the stream manipulators such as `std::dec`, `std::fixed`, and `std::scientific`, they are found in the <code>&lt;iomanip&gt;</code> header. 

**There is nothing wrong with using printf in C++ code**. Some people think printf is bad because it's C, but printf actually has a number of huge advantages over iostream. Performance is one reason (**printf is often way faster than cout; it is never slower** - do profile it yourself to check though), conciseness and flexibility in formatting output is another reason. [Example](https://stackoverflow.com/questions/2872543/printf-vs-cout-in-c):

```cpp
printf("0x%04x\n", 0x424);
std::cout << "0x" << std::hex << std::setfill('0') << std::setw(4) << 0x424 << std::endl;
```
As you can see, it is a lot more effort to format printing output using cout than it is using printf. Yes, I know you can create a class and then overload <code>operator<<</code> for iostream with that class but you'd still have to write the horrible cout-using code into it. No reason not to use printf instead. Luckily C++ is a superset of C, so this is not an argument against C++ at all. cout has its advantages (e.g it's type-safe), but [formatting options is not one of them](https://stackoverflow.com/questions/2485963/c-alignment-when-printing-cout/2486085) - the printf format string can be easily modified to suit your purposes, whereas fiddling around with cout is just a pain. 

All the manipulators (except for one) are "sticky" in the sense that they permanently change the `cout` object itself. Since most programs use one cout object, this means iostream manipulators alter a global mutable state. Nice. 

Keep in mind that `\n` does not flush whereas `std::endl` does. So for performance reasons you should always use `\n` unless you need to flush the buffer, in which case you can just use `std::flush` (if you really care about performance though you'd use printf instead of cout). 

Also, printf is super unsafe to use if you are using user-controlled strings (see format string vulns), so that's another argument in favor of cout. The old adage "never trust user input" applies as always. 

## Literals

All literals are prvalues except for string literals which are lvalues. You can take the address of a lvalue, so <code>&"hello world"</code> is valid, but <code>&25</code> is not, because you cannot take the address of a prvalue. 

## Arrays vs pointers

A pointer is completely different from an array. Unfortunately, C makes it easy to confuse the 2 by having a built-in array-to-pointer conversion (aka array-to-pointer decay), wherein an array is implicitly converted to a pointer to the first element of the array. 

What is an array? It is an object that consists of contiguously allocated objects (not references, not functions, not abstract class types) of the same type. You cannot modify arrays as a whole, so you can't do this:

```cpp
int a[3], b[3];
a = b;
```

How does array-to-pointer decay work? Well, unfortunately **you cannot pass arrays** by value (The C language designers didn't allow this because they thought it'd be "too inefficient", but they allow you to wrap arrays inside structs and pass **those** by value `¯\_(ツ)_/¯`). You can only pass arrays by pointer or reference. If you try to pass an array to anywhere where a pointer is expected, it gets automatically converted into a pointer. That's why you can pass arrays into functions that expect pointers. [Here](https://stackoverflow.com/questions/16144535/difference-between-passing-array-fixed-sized-array-and-base-address-of-array-as) is an example:

```cpp
// These functions are all exactly the same
void func1(const char* str){}
void func2(const char str[]){}
void func3(const char str[10]){}
int main(){
  func3("test"); // Works.
  func3("let's try something longer"); // Not a single fuck given.
}
```

Although the function declaration suggests that the function `func3` takes an argument that is an array of length 10, **this is in fact a very dangerous illusion** because the compiler turns it into a pointer without telling you. Since you only have a pointer, **you have no way to tell** how long the array is. This is why lots of C functions (such as fgets) take a pointer to an array as well as the size of the array: they need you to tell them how long the array is, otherwise they have no idea and might just write past the end of it. 

Ridiculous right? The fun never stops! So if you want to specify that a function should take a parameter that is an array of fixed size, you have to do something like this:

```cpp
void f(const char (&str)[10]){}
```

But this is a reference type parameter, not a pointer type parameter. Actually str is a "reference to an array of 10 chars" (read from right-to-left, reverse direction on reaching parentheses). 

```cpp
#include <iostream>
int main(){
int a[] = {1,2,3};
int* p = a; 
std::cout << sizeof(a) << " " << sizeof(p);
}
```
Output on my machine:
```
12 8
```

Running this code shows you that an array and a pointer are completely different things. The size of an array is how many elements are inside it multiplied by how big each element is. The size of a pointer is just how much space an address takes up on your machine architecture (32 or 64 bits usually). They are completely different. 

### Pointer to array vs pointer to first element of array

[More info here.](https://stackoverflow.com/questions/4810664/how-do-i-use-arrays-in-c)

It turns out that a pointer to an array is a completely different type from a pointer to the first element of an array. When you have array-to-pointer decay, the array decays to a pointer to its first element. But that's totally different from a pointer to the array itself, which you can obtain by using the &amp; operator on the array. 

```cpp
static_assert(!std::is_same<int*, int(*)[8]>::value, "distinct element type");
```

>The same situation arises in classes and is maybe more obvious. A pointer to an object and a pointer to its first data member have the same value (the same address), yet they are completely distinct types.
>
>If you are unfamiliar with the C declarator syntax, the parenthesis in the type int(*)[8] are essential:
>
>int(*)[8] is a pointer to an array of 8 integers.
>
>int*[8] is an array of 8 pointers, each element of type int*.

A pointer to an array has the size of the array encoded in its type (which means you can't take a pointer to an array of 5 elements and then point it to an array of 10 elements); this is not the case for a pointer to the first element of an array. 

If you dereference a pointer to an array then you get back the array itself. For example:

```cpp
void f(int (*p)[7])
{ 
	sizeof(*p); //This returns the size of the array itself. 
}
```

More examples to clarify:

```cpp
int a[10];
int* p1 = a; 
int* p2 = &a[0];
int(*)[10] p3 = &a;
```

Here, p1 and p2 are completely the same, but p3 is totally different. p1 and p2 have the same type, and can point to an int of any array, but p3 can only point to an array of 10 ints. There is a **big difference** between <code>&a</code> and <code>&a[0]</code> - the former is a pointer to an array whilst the latter is a pointer to the first element of an array. This means a pointer to an array is more type-safe than a pointer to the first element of the array. 

### Commutativity of the indexing operator

In C/C++, it is true that `a[5] == 5[a]`. Supposedly this is because, when one of its operands is a pointer (or implicitly convertible to a pointer) and the other is an integer, the indexing operator is defined as follows:

```cpp
a[b] == *(a + b)
```

Since addition is commutative, this means it doesn't matter whether which way round the array and the number are, since they'll be added together and then dereferenced. Very useful for writing obfuscated C/C++ code and bamboozling your friends. 

**THIS DOES NOT MEAN THAT THE INDEXING OPERATOR IS COMMUTATIVE**. The indexing operator itself is not commutative, a fact you will learn when you try to overload the indexing operator for user-defined classes. In particular, it is not the case that T1Obj[T2Obj] is the same as T2Obj[T1obj]. You can have custom_object[5] but you can't have 5[custom_object], because operator[] must be overloaded as a member function and I'm pretty sure you can't overload "member functions" in built-in types. 

### Multidimensional arrays

Ready for more funny stuff? It turns out that array-to-pointer decay only works for one-dimensional arrays. A 2-dimensional array decays to a 1 dimensional array of pointers. 

Actually there are no 2D arrays in C, only arrays of arrays. That explains why a 2D array of int (array of arrays of int) decays into a pointer to an array of int. That's why you can't pass a 2D array of T into a function that expects a `T**`. 

C++ uses [row-major-order](https://en.wikipedia.org/wiki/Row-_and_column-major_order). That means a 2D array, say a[2][3] is laid out like this:

```cpp
{ a[0][0], a[0][1], a[0][2], a[1][0], a[1][1], a[1][2] }
```

Laid out in the more familiar matrix format:
```cpp
//1st col, 2nd col, 3rd col
{ a[0][0], a[0][1], a[0][2],    //first row
  a[1][0], a[1][1], a[1][2] }   //second row
```
You access elements in the matrix like this: `a[row][column]`

So the first 3 elements are those in the first row, then the second 3 elements are those in the 2nd row and so on. 

Example:

```cpp
void f(int (*a)[4]){} //This works
void h(int a[][4]){} //This is equivalent to f
void f2(int a[2][4]){} //This is equivalent to f
void g(int* a[4]){} //This is equivalent to int**, and doesn't work

int main(){
  int a[2][4];
  int* p1 = a[0]; 
  int (*p2)[4] = a;
  int** p = &p1;
  f(a);
  h(a);
  g(a);
  f(p);
  g(p);
  f2(p);
}
```

If you try to compile the code above, you will get something like this:

```
error: cannot convert ‘int (*)[4]’ to ‘int**’ for argument ‘1’ to ‘void g(int**)’
error: cannot convert ‘int**’ to ‘int (*)[4]’ for argument ‘1’ to ‘void f(int (*)[4])’
error: cannot convert ‘int**’ to ‘int (*)[4]’ for argument ‘1’ to ‘void f2(int (*)[4])’
```

As you can see, `int[2][4]` is regarded to be the same argument type as `int(*)[4]`. Again, this shows you cannot pass arrays by value. If you try to do so, you will end up passing the array by pointer instead. 

Notice how the compiler tells you that the function `g` takes an argument of type `int**` and not `int(*)[4]`. 

In the declaration `void g(int* a[10])`, `a` is a pointer to a pointer, it is **NOT** an array of 10 pointers or a pointer to an array of 10 elements. `int(*) a[10]` is a pointer to an array of 10 elements, and is equivalent to `int a[][10]`. Although `int a[][10]` makes it look like it's a 2D array, in fact it is a pointer to an array of 10 elements. 

You might also notice that when you want to pass a pointer to a 2D array, the number of columns (and not the number of rows) is encoded into the type. That means that it's not possible to pass a generic 2D array by pointer, as far as I know. If you want to pass a generic 2D array, you can pass it by reference (using templates) or you can use dynamic arrays. 

An array of `T[2][4]` does not implicitly convert to `T**` (although `T*[5]`, an array of 5 pointers, does convert to `T**`). In fact it decays to `(T*)[4]`, which is a "pointer to an array of 4 elements of type T". If you increment this you are traversing the 2D array by row, so you can do this:

```cpp
#include <iostream>
int main(){
  int a[2][4] = {{1,2,3,4}, {5,6,7,8}};
  int* p1 = a[0]; //pointer to int
  int (*p2)[4] = a; //pointer to array of 4 ints
  for (int i=0; i < 8; ++i) //Iterates through the array of arrays as if it's a 2D matrix
    std::cout << p2[i/4][i%4]; //i/4 is the row, i%4 is the column
  std::cout << "\n";
  for (int i=0; i < 2; ++i) //Iterates through each row
    std::cout << *(p2[i]); //Print first element of the ith row
  std::cout << "\n";
  for (int i=0; i < 8; ++i) //Iterates through array of arrays as if it's a 1D array
    std::cout << *(p1+i);  //This works because an array is guaranteed to have a contiguous chunk of memory
}
```
Output:
```
12345678
15
12345678
```

The first for loop is fairly straightforward, since p2 is a pointer to an array we can use the 2D indexing notation on it. The second for loop shows that you get the next row (array) by adding 1 to the first subscript, and if you dereference that (the array decays to a pointer) then you get the first element of the row. In the third for loop we are simply incrementing p1 which is just a pointer to an int, initially pointing to the first element of the first row. We can iterate through the entire 2D matrix by simply incrementing p1 - incrementing it by the number of elements in each row gets us to the next row. 

To reiterate, if a is an array of arrays of ints (a 2D array) then:

- <code>&a</code> is a pointer to an array of arrays of ints
- <code>&a[0]</code> is a pointer to an array of ints
- <code>&a[0][0]</code> is a pointer to an int

What about higher dimensional arrays? Again same rule applies: only the "first level" is decayed to a pointer, the other levels stay as arrays. Example of 3D array:

```cpp
void f1(int a[2][3][4])
void f2(int a[][3][4])
void f3(int (*a)[3][4])
```

These are all completely the same function. However, you should just use the third one, because the other ones are misleading, especially the first one, since the `[2]` isn't actually in the type information at all - `a` is a pointer to a 2D array (actually an array of arrays), not a 3D array (array of arrays of arrays).  

### Arrays of pointers

Remember earlier I said that a 2D array is one contiguous block of memory, it is NOT an array of pointers. An array of pointers is not an array of arrays, these pointers could point to anywhere in memory, not necessarily to regions that are laid out contiguously with respect to one another, which is the case for an array of arrays. 

### Aggregate initialization

If you initialize an array like this:

```cpp
int a[5] = {1};
int b[5] = {};
```

The array is [aggregate-initialized](http://en.cppreference.com/w/cpp/language/aggregate_initialization). If the number of initializer clauses is less than the number of members in the aggregate then the remaining members are initialized using default initializers or by empty lists (value-initialized for non-class types, e.g integrals are zero-initialized). So all the members of `b` are initialized to 0, same for all the members of `a` except for the first member. 

You can initialize a struct or class using the aggregate initializer as well:

```cpp
struct Date d = {2018, 4, 1}
```

## Temporaries

What the hell is a temporary? As Meyers says in More Effective C++:

>True temporary objects in C++ are invisible — they don’t appear in your source code. They arise whenever a non-heap object is created but not named. Such unnamed objects usually arise in one of two situations: when implicit type conversions are applied to make function calls succeed and when functions return objects. It’s important to understand how and why these temporary objects are created and destroyed, because the attendant costs of their construction and destruction can have a noticeable impact on the performance of your programs.
>
>Consider first the case in which temporary objects are created to make function calls succeed. This happens when the type of object passed to a function is not the same as the type of the parameter to which it is being bound. 


cppreference says this:

>Temporary objects are created when a prvalue is materialized so that it can be used as a glvalue, which occurs in the following situations:
>
>- when initializing an object of type std::initializer_list<T> from a braced-init-list
>- when performing member access on a class prvalue
>- when performing an array-to-pointer conversion or subscripting on an array prvalue
>- for unevaluated operands in sizeof and typeid
>- when a prvalue appears as a discarded-value expression
>- if supported by the implementation, when passing or returning an object of trivially-copyable type in a function call expression (this models passing structs in CPU registers)
>- The materialization of a temporary object is generally delayed as long as possible in order to avoid creating unnecessary temporary object: see copy elision
>
>All temporary objects are destroyed as the last step in evaluating the full-expression that (lexically) contains the point where they were created, and if multiple temporary objects were created, they are destroyed in the order opposite to the order of creation. This is true even if that evaluation ends in throwing an exception.
>
>There are two exceptions from that:
>
>- The lifetime of a temporary object may be extended by binding to a const lvalue reference or to an rvalue reference (since C++11), see reference initialization for details.
>- The lifetime of a temporary object created when evaluating the default arguments of a default constructor used to initialize an element of an array ends before the next element of the array begins initialization.

See also C++17 Standard 15.2 Temporary objects:

>Temporary objects are destroyed as the last step in evaluating the full-expression that (lexically) contains the point where they were created. This is true even if that evaluation ends in throwing an exception. The value computations and side effects of destroying a temporary object are associated only with the full-expression, not with any specific subexpression.


### Taking a reference to a temporary

Temporary objects are truly temporary (destroyed at the end of the expression where they are created), unless they are bound to a reference, in which case this applies:

>Whenever a reference is bound to a temporary or to a subobject thereof, the lifetime of the temporary is extended to match the lifetime of the reference, with the following exceptions:
>
>- a temporary bound to a return value of a function in a return statement is not extended: it is destroyed immediately at the end of the return expression. Such function always returns a dangling reference.
>- a temporary bound to a reference member in a constructor initializer list persists only until the constructor exits, not as long as the object exists. (note: such initialization is ill-formed as of DR 1696)
>- a temporary bound to a reference parameter in a function call exists until the end of the full expression containing that function call: if the function returns a reference, which outlives the full expression, it becomes a dangling reference.
>- a temporary bound to a reference in the initializer used in a new-expression exists until the end of the full expression containing that new-expression, not as long as the initialized object. If the initialized object outlives the full expression, its reference member becomes a dangling reference.
>
>In general, the lifetime of a temporary cannot be further extended by "passing it on": a second reference, initialized from the reference to which the temporary was bound, does not affect its lifetime.

Observe:

```cpp
struct S{
  S():v(2){}
  const int& v;
};
int main(){
  S o;
}
```

This code compiles in g++-7 without any warnings unless you use `-Wextra` (doesn't even warn with `-Wall`). Clang on the other hand gives warnings:

```
warning: a temporary bound to ‘S::v’ only persists until the constructor exits [-Wextra]
```

I believe the C++ standard requires the compiler to emit a diagnostic when you try to bind a member reference to a temporary (defect report 1696): 

>A temporary expression bound to a reference member in a mem-initializer is ill-formed. [Example:
>```cpp
>  struct A {
>    A() : v(42) { }  // error
>    const int& v;
>  };
>```
>—end example]

Unfortunately the code still compiles (and on g++ without any warnings unless you use `-Wextra`). This would result in the reference member becoming a dangling reference as soon as the constructor exits. Another great way to blow your leg off. 

Note that you don't have to bind to const reference member via a constructor initializer, you can also use a default member initializer:

```cpp
#include <iostream>
struct S{
const char* const & a = "hello world";
};
int main(){
 S o;
 std::cout << o.a;
}
```

This also results in undefined behavior as the temporary dies as soon as the object's constructor exits. See, whilst the string literal itself is of static storage duration, when you assign it to a reference to a pointer to a const char, it gets converted into a temporary pointer to a const char (implicit conversion via array-to-pointer decay), which dies as soon as the statement finishes executing. Taking a reference to something that would immediately die seems pretty pointless for normal usage, but makes for a great way to blow your leg off by accident. 

In fact if you compile and run that it may even appear to work. To get it to output junk more reliably, try this:

```cpp
#include <iostream>
struct S{
const char* const & a = "hello world";
const char* const & b = "hahahahahah";
};
struct S2{
const char* const & a = "dkjhgfkjdhk";
};
int main(){
 S o;
 S2 o1;
 std::cout << o.a;
}
```

This outputs something other than "hello world" on most compilers I've tried but obviously since it's undefined behavior you can never know what will happen (nasal demons and all). 

Relevant quote from cppreference:
```cpp
struct A
{
    A() = default;          // OK
    A(int v) : v(v) { }     // OK
    const int& v = 42;      // causes undefined behavior (UB)
};
A a1;    // error: ill-formed binding of temporary to reference, compiles without error and causes UB
A a2(1); // OK (default member initializer ignored because v appears in a constructor)
         // however a2.v is a dangling reference and causes UB
```

Pretty fucking dangerous if I may say so myself. In general it is really easy to create dangling references by binding to temporaries, and it's all too easy to blow your leg off. One way to make it safer is to disable passing a temporary as an argument to the constructor by deleting the constructor that takes a rvalue:

```cpp
struct A
{
    A() = default;          
    A(int& v) : v(v) { }     
    A(int&& v) = delete; //now you can't pass a rvalue to the constructor
    const int& v = 42; //causes UB
};
int main(){
    A a1;    // unfortunately still compiles without error, causing UB
    A a2(1); // compile error: call to deleted constructor of A
}
```

See [GOTW #88](https://herbsutter.com/2008/01/01/gotw-88-a-candidate-for-the-most-important-const/) and [More ammo to blow your leg away](https://askldjd.com/2010/05/08/const-reference-to-temporary-is-useless/). 

Imagine you create a reference to a temporary by accident. Then you pass it around thinking it's still valid, not knowing that the temporary died as soon as the expression ended. Example from stack overflow:

```cpp
void inc(double& x){
  x++;
}
int i = 0;
inc(i); //i remains unchanged
```

The above code is invalid C++ for a good reason. However it becomes valid if you use a rvalue reference:

```cpp
#include <iostream>
void inc(double&& x){
  x++;
}
int main(){
  int i = 0;
  inc(i); //i remains unchanged
  std::cout << i;
}
```
Output:
```
0
```
Compiles with no warnings or errors. Foot, say hello to Mr.Bullet. 

Don't let this dissuade you from using const reference to temporary, however. It is considered idiomatic C++ to pass a temporary object by const reference (if you're not going to modify it), for example:

```cpp
#include <iostream>
void f(const std::string& s){ std::cout << s;}
int main(){
        f("hello world");
}
```

This is perfectly legal and idiomatic C++ code. The function `f` is probably as efficient as could be: it uses the temporary std::string created from the string literal directly. 

In general, I think a way to avoid dangling references is to always carefully consider the lifetime of the object that your reference refers to to ensure that the storage duration of your reference can never exceed the lifetime of the object that the reference references. 

## Floating points

C++ does not require floating point arithmetic to associative or even commutative. Specifically, C++ does not guarantee IEEE 754, which guarantees that floating point addition and multiplication are commutative (excluding NaN values), which means that if your implementation is IEEE 754 compliant (and most implementations are) then floating point addition and multiplication would be commutative. 

It is trivial to demonstrate non-associativity:

```cpp
#include <iostream>
int main(){
  float a = 1e8, b = -a, c = 1;
  std::cout << (a + b) + c << "\n";
  std::cout << a + (b + c);
}
```
Output:
```
1
0
```

How it works is very simple: 1e8 is so huge (in floating point terms) that adding 1 to it doesn't change it. Observe:

```cpp
#include <iostream>
#include <iomanip>
int main(){
  float x = 1e8;
  float y = 1e8 + 1;
  std::cout << std::boolalpha << (x == y);
}
```
Output:
```
true
```

Huh. Would you look at that. 1e8 is equal to 1e8 + 1. The joys of floating point arithmetic. 

Also: bitwise operators do not work on floating points. They only work on integers. 

## [Alternative operator representations](http://en.cppreference.com/w/cpp/language/operator_alternative)

## Checking if a bit is set

If you want to check if a bit is set in an unsigned int x you can do:

```cpp
unsigned int mask = 1 << n; //vacated bits are zero-filled
if ( x & mask ) { //do stuff
```

## Bitwise operations on signed integers

1. Right shifting a signed integer is implementation defined. 
2. Left shifting a negative integer is undefined. 
3. Left shifting a signed integer into negative is undefined. 

The bitwise representation of signed integers is not specified in the standard, so it could be two's complement or one's complement or something else see C++17 Standard Section 6.9.1 Fundamental Types:

>The representations of integral types shall define values by use of a pure binary numeration system.52 [Example: This International Standard permits two’s complement, ones’ complement and signed magnitude representations for integral types. — end example ]

This means the result of bitwise operations on signed integers is implementation defined. You do not get the same guarantees that you get with unsigned integers. 

That's why the [SEI CERT C Coding Standard recommends to only use bitwise operators on unsigned operands](https://wiki.sei.cmu.edu/confluence/display/c/INT13-C.+Use+bitwise+operators+only+on+unsigned+operands). I believe there is no reason why this advice should not apply to C++, because the signed integers are implementation defined in C++ therefore the results of bitwise operations are still going to be on these implementation-defined representations, whereas for unsigned integers there is only one representation allowed by the C++ standard so you know exactly what bitwise operations do. 

Do note that the bitwise left shift is "undefined if the right operand is greater than or equal to the length in bits of the promoted left operand".  

## Switch case

In a switch case, you usually want to terminate each case with a break, since otherwise execution will continue down the lines. Sometimes that is desirable. 

## Structs vs classes

A struct is almost exactly the same as a class except a struct's members are public by default whereas a class's members are private by default. A class can inherit from a struct and vice versa.  

## Storing a class member of the same type as the class itself

Unfortunately you cannot store a member inside a class that is of the same type as the class itself. Consider this:

```cpp
class A{
  A(){}
  A a;
}
```

When you try to create the first A object, it will try to create another A first, which will then try to create another A and so on ad infinitum. Infinite recursion. This is unfortunately not allowed in C++. No idea why. Maybe they just don't like fun things. 

You can do this though:

```cpp
struct S{
  S& s;
  S(S& s):s(s){}
}
int main(){
  S s(s);
}
```

Clang gives a warning:

```
warning: variable 's' is uninitialized when used within its own initialization [-Wuninitialized]
```

But as usual GCC does not (not even with `-Wall` and `-Wextra`). But clearly we have not initialized s. We are initializing it with itself...does that count as initialized? 

According to [this stack overflow answer here](https://stackoverflow.com/questions/32608458/is-passing-a-c-object-into-its-own-constructor-legal) this is perfectly valid C++ code because we are not actually accessing uninitialized memory. 

## More on pointers

### Examining memory directly

If you want to examine memory directly you can reinterpret an object as an array of chars like this:

```cpp
unsigned const char * bytes = reinterpret_cast<unsigned char const*>(object);
```

### Manually setting pointers

You can manually set pointer values like this:

```cpp
int* p = (int*)2; //useful when working on bare metal
p = 0; //prefer nullptr instead, 0 is the same as NULL
int i = 2;
p = &i; //p points to i
int& r = *p; //reference to the object pointed to by p
*p = 3; //change the value of i to 3
r = 1; //change the value i to 1
```

### Valid operations on pointers

Pointers are not the same as integers. In fact, pointers have iterator semantics (iterators are a generalization of pointers): pointers only have defined iterator operations in the context of arrays. Pointer arithmetic is undefined outside of the context of arrays. Doing any arithmetic that makes a pointer point to outside its original array (besides one-past-the-end) is undefined behavior (even if you do not dereference the pointer). See C++17 Standard Section 8.7 Additive operators:

>When an expression that has integral type is added to or subtracted from a pointer, the result has the type of the pointer operand. If the expression P points to element x[i] of an array object x with n elements,86 the expressions P + J and J + P (where J has the value j) point to the (possibly-hypothetical) element x[i + j] if 0 ≤ i + j ≤ n; otherwise, the behavior is undefined. Likewise, the expression P - J points to the (possibly-hypothetical) element x[i − j] if 0 ≤ i − j ≤ n; otherwise, the behavior is undefined.

From The C++ Programming Language 4E:

>Subtraction of pointers is defined only when both pointers point to elements of the same array (although the language has no fast way of ensuring that is the case). When subtracting a pointer p from another pointer q, q−p, the result is the number of array elements in the sequence [p:q) (an integer). One can add an integer to a pointer or subtract an integer from a pointer; in both cases, the result is a pointer value. If that value does not point to an element of the same array as the original pointer or one beyond, the result of using that value is undefined.

Only the following operations are valid on pointers (where p is a pointer and i is an integer):

```
p++, p--
p + i, p - i
p - p
Comparing 2 pointers  
Comparing a pointer with 0  
```
That's it. 

Please note that when you increment a pointer by 1, the pointer is not necessarily actually incremented by 1. Example:

```cpp
#include <iostream> //ok to have comments in preprocessor directives, they get stripped out before directives are evaluated
#include <cstdint> //this is where uintptr_t is defined
std::uintptr_t ptoi(void* p){ //should probably use templates instead of void*
  return reinterpret_cast<std::uintptr_t>(p); //uintptr_t is the correct integer type to use to store pointers in C++
}
int main(){
  char a1[10];
  long long a2[10];
  char* p1 = a1;
  long long* p2 = a2;
  std::cout << (ptoi(p1+1) - ptoi(p1)) << std::endl;
  std::cout << (ptoi(p2+1) - ptoi(p2)) << std::endl;
}
```
Output on my machine:
```
1
8
```

So as you can see, incrementing p1 by 1 actually incremented it by 1, whereas incrementing p2 by 1 actually incremented it by 8. This is because p2 points to an array of long longs, which are 8 bytes on my machine. This goes back to iterator semantics - incrementing an iterator by 1 always takes you to the next element. But it can be confusing if you thought that incrementing a pointer literally increments its value (the address it points to) by that amount. 

People sometimes talk about "pass by value" vs "pass by pointer". There isn't really a pass-by-pointer. Pointers are also objects, and they can be either passed by value or by reference. 

Also a note on the `void*` I used. These are used to pass objects of unknown type (prior to templates). Dereferencing a `void*` is not allowed, you must first convert it to a pointer-to-object. The reason is obvious: we don't know what it points to, hence it has no semantics - we neither know its size nor what operations are defined on it. 

#### Random access iterators vs forward iterators

With random-access iterators, moving by N (it += N) is a constant time operation, whereas with forward iterators it is a linear time operation (since std::advance has to call it++ N times). 

## Default parameters

Default parameters in C++ must come last, after any non-default parameters. So that when you pass arguments to a function, they are matched against first the normal parameters and then the default parameters. You cannot "skip" some default parameters in C++. There is a way to get around this, called the "named parameter idiom". 

## Function overloading

Function overloading in C++ is done by number and type of parameters. Functions can be distinguished by return type but not overloaded on return type. So this:

```cpp
int f();
string f();
```

Is invalid C++. 

### Overload resolution ambiguity

```cpp
void f(int x){}
void f(float x){}
f(1.0);
```

This is ambiguous because 1.0 is a double literal so a conversion to either float or int involves a demotion, so the compiler doesn't know which is a better fit. 

## The new operator

The new operator allocates memory (enough memory to hold the object or array of objects) and then calls the constructor(s) on the object(s). Then a pointer to the newly constructed object(s) is returned. The difference between malloc/free and new/delete is that new allocates memory before calling the constructor, and delete calls the destructor before deallocating memory. If the constructor throws an exception, the memory is automatically released, so new is safe. 

```cpp
int* p = new int;
int* a = new int[10];
delete p; //calls destructor on *p and frees memory it occupied
delete[] a; //destroys each element in a and frees memory it occupied
```

Here's how you might use new and delete with a 2D dynamic array:

```cpp
int main(){
  int r = 5, c = 6;
  int** m = new int*[r];
  for (int i = 0; i < r; ++i){
    m[i] = new int[c];
  }
  for (int i = 0; i << r; ++i){
    delete[] m[i];
  }
  delete[] m;
}
``` 

You must call delete on every object allocated with new and delete[] on every array allocated with new[]. You must not call delete on elements in an array allocated with new[]. You must not delete something twice. You must not mix malloc, free, new, and delete. And in the example above, you must delete the elements of m before deleting m itself. If you fail to follow any of these rules, then all life comes to a catastrophic end. See the isocpp FAQ. 

And yes, delete really means delete_the_thing_pointed_to_by. 

Prefer make_unique or make_shared over new/malloc (smart pointers over raw pointers).

When new fails to allocate memory it will throw a std::bad_alloc. 

## Dynamic arrays

You can create a dynamic array like this:

```cpp
#include <iostream>
int main(){
  int a[3] = {1,2,3};
  int* b = new int[3]{4,5,6}; //This is perfectly valid C++11 code
  for (auto& x : a){
    std::cout << x;
  }
  for (auto& x : b){
    std::cout << x;
  }
}
```

Unfortunately this doesn't compile because you can't use a range-for on a dynamic array (at least not without wrapping it in something). You can however use it with a vector. 

### Memory allocation overhead

In fact the C++ standard does not say anything about stack or heap, but since most implementations put automatic objects on the stack and dynamic objects on the heap, these have become more or less synonymous in many contexts.

Making system calls to allocate memory is slow. You should keep these things in mind:

1. It is faster to allocate a large block of memory than many small blocks. If you have to allocate and deallocate memory very frequently consider using a memory pool.  
2. Dynamic allocation is slower than automatic allocation because in dynamic allocation an OS call is involved whereas in automatic allocation the memory has already been allocated. This also means an automatic array is faster than an automatic std::vector, since a vector always has an underlying dynamically allocated array even when the vector itself has automatic storage duration. Do note that most systems have quite small stack frames so you cannot allocate large arrays on the stack. 

### Vector reallocation and FBVector

[This section was added on 3 May 2018]

Facebook came up with their own version of std::vector called [folly:FBVector](https://github.com/facebook/folly/blob/master/folly/docs/FBVector.md). Several optimizations were made including changing the growth factor from 2 to 1.5, and allowing objects to be "relocatable". Here is what Facebook meant by that term:

>One particularly sensitive topic about handling C++ values is that they are all conservatively considered non- relocatable. In contrast, a relocatable value would preserve its invariant even if its bits were moved arbitrarily in memory. For example, an int32 is relocatable because moving its 4 bytes would preserve its actual value, so the address of that value does not "matter" to its integrity.
>
>C++'s assumption of non-relocatable values hurts everybody for the benefit of a few questionable designs. The issue is that moving a C++ object "by the book" entails (a) creating a new copy from the existing value; (b) destroying the old value. This is quite vexing and violates common sense; consider this hypothetical conversation between Captain Picard and an incredulous alien:
>
>Incredulous Alien: "So, this teleporter, how does it work?"  
>Picard: "It beams people and arbitrary matter from one place to another."  
>Incredulous Alien: "Hmmm... is it safe?"  
>Picard: "Yes, but earlier models were a hassle. They'd clone the person to another location. Then the teleporting chief would have to shoot the original. Ask O'Brien, he was an intern during those times. A bloody mess, that's what it was."  

Let's use an example to illustrate the behavior of std::vector:

```cpp
#include <iostream>
#include <vector>
using std::cout;
struct S{
  int i;
  S(int i):i(i){ cout<<"S"<<i<<" constructed\n"; }
  S(const S& s):i(s.i){ cout<<"S"<<i<<" copy constructed\n"; }
  S(S&& s):i(s.i){ cout<<"S"<<i<<" move constructed\n"; }
  ~S(){cout << "S"<<i<<" Destructed\n"; }
};
int main(){
  std::vector<S> v;
  v.emplace_back(1);
  v.emplace_back(2);
  v.emplace_back(3);
  v.emplace_back(4);
  v.emplace_back(5);
}
```
Output:
```
S1 constructed
S2 constructed
S1 copy constructed
S1 Destructed
S3 constructed
S1 copy constructed
S2 copy constructed
S1 Destructed
S2 Destructed
S4 constructed
S5 constructed
S1 copy constructed
S2 copy constructed
S3 copy constructed
S4 copy constructed
S1 Destructed
S2 Destructed
S3 Destructed
S4 Destructed
S1 Destructed
S2 Destructed
S3 Destructed
S4 Destructed
S5 Destructed
```
As you can see, the vector starts with a size of 1, then when a new element is added, it grabs a bigger piece of memory (factor 2 by default) and constructs the new object in the destination, then it copies the old elements over to the new piece of memory, invoking their copy constructors, then destroys the old elements, invoking their destructors. 

As stated in the Facebook readme, copy-constructing objects isn't very smart if they're relocatable, since memmove/memcpy is much much faster. However, for trivially copyable objects, compilers will probably use memcpy/memmove anyway. See following code:

```cpp
#include <vector>
struct WackyObject {
    //WackyObject(){}
    //WackyObject(const WackyObject&){}
    int x, y;
    double p, q;
    char s[232];
};

void pushanother(std::vector<WackyObject>& v) 
{
    v.emplace_back();
}
```
The above code, when compiled with GCC 8.1, emits a memmove call, and the same when compiled with Clang 6.0. If you uncomment the comments then you will see that they won't emit the memmove calls anymore (you can use Godbolt to compile into assembly), precisely because they are no longer trivially copyable. See [cppreference](http://en.cppreference.com/w/cpp/concept/TriviallyCopyable):

>Requirements
>- Every copy constructor is trivial or deleted  
>- Every move constructor is trivial or deleted  
>- Every copy assignment operator is trivial or deleted  
>- Every move assignment operator is trivial or deleted  
>- at least one copy constructor, move constructor, copy assignment operator, or move assignment operator is non-deleted  
>- Trivial non-deleted destructor  

Where "trivial" means "not user defined". 

On the other hand, FBVector's notion of relocatable is "can be moved by memmove/memcpy". It is distinctly different from the C++ notion of trivially copyable. An object may not be trivially copyable yet still be relocatable. 

## std::find and std::map::find

std::find takes linear time regardless of the container, since it only iterators, which know nothing about the container. On the other hand std::map::find takes log time, so you should never use std::find on a std::map. 

## Type conversion

Types can be converted using the C style cast T(e) or (T)e - these are equivalent and causes the compiler to call the following in order:

- const_cast<T>(e)
- static_cast<T>(e)
- reinterpret_cast<T>(e)

The C-style cast is considered unsafe because it doesn't warn you when it uses const_cast or reinterpret_cast which are both considered unsafe. Much better to use explicit casts to make it clear which cast is being used. 

You may also use Boost cast. 

## String operations

String comparison works lexicographically: two strings are compared element by element, the first mismatching element determines which is greater. If one string is a prefix of the other, the shorter is the lesser. Two empty strings are equal. 

Comparisons use character encoding rather than alphabetical order. In other words the ordering is implementation defined (except for the numerical characters). Many systems use ASCII but the C++ standard does not guarantee this. strcoll is portable but it requires the appropriate locale setting. 

The C++ standard does guarantee that the characters 0-9 are sequential, and that the null character's value is 0. From C++17 standard section 5.3 Character sets:

>The basic execution character set and the basic execution wide-character set shall each contain all the members of the basic source character set, plus control characters representing alert, backspace, and carriage return, plus a null character (respectively, null wide character), whose value is 0. For each basic execution character set, the values of the members shall be non-negative and distinct from one another. In both the source and execution basic character sets, the value of each character after 0 in the above list of decimal digits shall be one greater than the value of the previous. The execution character set and the execution wide-character set are implementation-defined supersets of the basic execution character set and the basic execution wide-character set, respectively. The values of the members of the execution character sets and the sets of additional members are locale-specific.

In other words it is guaranteed that:

```cpp
'6' - '0' == 6;
'0' + 1 == '1';
```

But it is NOT guaranteed that `'a' + 1 == 'b'`. In some performance critical applications you may assume ASCII whilst knowingly breaking portability (that is the cost you pay for performance) but in general you should avoid making implementation-defined assumptions whenever possible. 

The s.compare(t) will return 0 if strings are equal, <code><0</code> if <code>s<t</code>, <code>>0</code> if <code>s>t</code>. Return type is an int and it is not specified to be a specific value. 

The s.find(t) will return the index of the first occurrence of t in s or std::npos if not found. It returns a size_type which is an unsigned integer. 

s.reserve expands capacity but does not necessarily shrink-to-fit. 

s.clear empties the string. 

s.empty checks if the string is empty. Same as =="" or size()==0. 

s.resize appends or truncates characters to/from the string to specified size (it does not reduce the amount of memory a string takes up). By default (without a 2nd argument) it appends `\0`. 

s.append appends a string and returns a reference:

```cpp
s.append(s).append(s); //This quadruples s
```

Performance wise append may differ from push_back. 

s.swap(t) swaps s with t. Same as using std::swap. 

s.insert(i,n,c) inserts n copies of the character c at index i. 

s.assign(n,c) clears the string and replaces it with n copies of char c. 

s.erase(i,n) erases from i to i+n. 

etc. 


### Substring

The string.substr(i,n) or substr(i) function returns a substring of the string given, the first argument is the index and the 2nd argument is the length. So:

```cpp
s.substr(1,1); //gives 2nd character of s
s.substr(1); //gives string from 2nd char to end of s inclusive
s.substr(n); //gives string from n+1th character to end of s inclusive
s.substr(s.size()-1); //gives last character of string
s.substr(s.size()-n); //gives last n chars of string
```

str.size and str.length are synonyms. 

In C++ it is guaranteed that where s is a std::string, s[s.size()] returns `\0`. However, modifying s[s.size()] is undefined behavior. See C++17 standard section 24.3.2.5 basic_string element access:

>Returns: *(begin() + pos) if pos < size(). Otherwise, returns a reference to an object of type charT with value charT(), where modifying the object to any value other than charT() leads to undefined behavior.

LWG Issue 2475 (Allow overwriting of std::basic_string terminator with charT() to allow cleaner interoperation with legacy APIs) suggests the reason is that some legacy code liked to write one past the end of a string to `\0`, as this code from the issue does:

```cpp
void legacy_function(char *out, size_t count) {
  for (size_t i = 0; i < count; ++i) {
    *out++ = '0' + (i % 10);
  }
  *out = '\0'; // if size() == count, this results in undefined behavior
}

int main() {
  std::string s(10, '\0');
  legacy_function(&s[0], s.size()); // undefined behavior
}
```

This is now perfectly valid C++ code (since C++14 I believe, since the C++11 standard said it should not be modified). 

Here the code is taking a pointer to the first element of the string and accessing the string as if it was a char array. This is perfectly valid code, to simplify:

```cpp
std::string s("haha");
char* pc = &s[0]; //This is valid C++11 code
```

Since C++11 std::string is guaranteed to be contiguous so you can access and modify the underlying char array of a std::string like that, and that's perfectly valid C++. You can also write one past the last character of the string but you can only write it to be `\0`. 

## Static initialization

If you have a static local variable, its declaration is only executed once, on all subsequent executions it's skipped over:

```cpp
#include <iostream>
int f(int j){
  static int i = j;
  return ++i;
}
int main(){
  std::cout << f(5) << f(5) << f(5);
}
```
Output:
```
678
```

You may think of it as being internally implemented as something like an if statement like this:

```cpp
int f(int j){
  if(!i_initialized){
    i = j;
    i_initialized = true;
  }
  return ++i;
}
```

There is also something called the static initialization order fiasco, which I won't go into in this tutorial, but the basic idea is that you need to be very careful about initializing static variables with non-literals. Note that in C you cannot initialize static variables with non-literals, so this is not a problem in C, at least as far as I know. The fun never stops. 


## Pass and return by value

General advice: If you want efficiency, pass small primitives such as ints by value, otherwise always pass by reference. The only time you'd want to pass big objects by value is if you needed to make a local copy of an argument to operate on just for the duration of the function. 

Some C++ Terminology:

Formal parameter = parameter (things in function declaration)  
Actual parameter = argument (things passed in the function call)

When you pass by value you are creating a copy of the object.  
When you return by value you are creating a copy of the object.

When you create a copy of the object, the object's copy constructor is called. See example below:

```cpp
#include <iostream>
struct S{
  S() = default;
  S(const S&){
    std::cout << "copy ctor called" << std::endl;
  }
};
S f(S s){ return s; }
int main(){
  S s;
  f(s);
}
```
Output:
```
copy ctor called
copy ctor called
```

As you can see, the copy constructor is called twice here. The first time when s is passed into f, since it's passed by value, a copy is made. The second is when the parameter is returned by value from f. We can get rid of the second by returning by reference instead of by value (though you should never return a parameter by reference, because it dies as soon as the function returns):

```cpp
S& f(S s){ return s; }
```

When I compiled the version above, I get a warning from g++ saying `warning: reference to local variable ‘s’ returned [-Wreturn-local-addr]`. This warning is correct, you shouldn't return a reference to a local variable. Trying to use whatever is returned will lead to UB. 

Output:
```
copy ctor called
```
And likewise we can make the parameter pass by reference as well:
```cpp
S& f(S& s){ return s; }
```
Output: Nothing. 

So as you can see, passing by value and returning value both cause a copy of the object to be made, whereas this does not happen when you pass by reference. That's why Effective C++ recommends to **always pass large objects by reference and not by value**. With copy elision (which includes return value optimization) sometimes compilers can elide away the copy constructor call...which means that any side effects in the copy constructor will also not appear. So you have to be really careful with this. Outside of a handful of specific situations (in C++17), you are not guaranteed copy elision, so you should not rely on it for efficiency. If you want to avoid costly copies, pass by reference or pointer, don't pass large objects by value. 

In C++17, in the following cases you are guaranteed copy elision:

```cpp
struct T { 
  public: 
    T() = default;
    T(T&) = delete;  
};
T f() { return T{}; }
int main(){
  T a;
  T b{T{}};
  T c = T{};
  T d = f(); //The T returned by f() is not a temporary, it is a prvalue that directly initializes d
  T* p = new T{f()}; 
}
```
C++17 has redefined prvalue and temporaries such that in these situations, a temporary is not produced in the first place, so there is nothing to copy from. In C++14, prvalues are temporaries. In C++17, a prvalue can materialize a temporary, but isn't a temporary itself. In the case above, the function returns a prvalue, which is then used to initialize an object. Thus, guaranteed copy elision. 

The important thing to notice here is that there is no copy constructor - indeed, it has been deleted. In previous versions of C++, even if the temporary that would have been theoretically created during copy initialization was copy-elided by the compiler, a copy constructor would still have been required. 

However, in C++17 the copy constructor is not required because of guaranteed copy elision - T() and f() are prvalues not temporaries, and can be used to directly initialize the T objects, thus avoiding any need for the copy constructor call. Thus, this code compiles in C++17 but not previous versions of C++. 

Notice how it says nothing about copy elision for parameter passing by value. C++17 does not guarantee copy elision for parameters passed by value. You can only rely on the copy elision guaranteed by the standard, so the age-old advice of "pass big objects by const reference" still holds in C++17. 

### lvalues and rvalues

The concept of expressions is a fundamental C++ idea that every beginner must know. In order to understand what expressions are, you should read the C++17 Standard Section 8 Expressions. All the unsourced quotes in this section are from there. 

>An expression is a sequence of operators and operands that specifies a computation. An expression can result in a value and can cause side effects.
>
>Every expression belongs to exactly one of the fundamental classifications in this taxonomy: lvalue, xvalue, or prvalue. This property of an expression is called its value category.
>
>Expressions are categorized according to the taxonomy:

```
Expression: glvalue, rvalue
glvalue:    xvalue, lvalue
rvalue:     xvalue, prvalue
```

>A glvalue is an expression whose evaluation determines the identity of an object, bit-field, or function.
>
>An xvalue is a glvalue that denotes an object or bit-field whose resources can be reused (usually because it is near the end of its lifetime). 
>
>A prvalue is an expression whose evaluation initializes an object or a bit-field, or computes the value of the operand of an operator, as specified by the context in which it appears.

The take-away message here is that evaluating a lvalue **determines the identity of** an object, whilst evaluating a prvalue **initializes** an object. lvalue expressions refer to real objects that exist and have an address where multiple occurrences of that expression refer to the same thing. Eli Bendersky has a good [blog post on this](https://eli.thegreenplace.net/2011/12/15/understanding-lvalues-and-rvalues-in-c-and-c/):

>the unary '*' (dereference) operator takes an rvalue argument but produces an lvalue as a result.  
>Conversely, the unary address-of operator '&' takes an lvalue argument and produces an rvalue  

>Historically, lvalues and rvalues were so-called because they could appear on the left- and right-hand side of an assignment (although this is no longer generally true); glvalues are “generalized” lvalues, prvalues are “pure” rvalues, and xvalues are “eXpiring” lvalues. Despite their names, these terms classify expressions, not values.

Historically, a lvalue expression is anything that can appear on the left hand side of an assignment, and a rvalue is anything that can't appear on the left hand side of an assignment (however this is not strictly true nowadays). It is important to remember that lvalue and rvalue are kinds of expressions. They do not describe objects or variables. The name of a variable is a lvalue expression - even if the type of that variable is a rvalue reference. 

>In general, the effect of this rule is that named rvalue references are treated as lvalues and unnamed rvalue references to objects are treated as xvalues; rvalue references to functions are treated as lvalues whether named or not. 

`std::move()` produces a xvalue expression from a lvalue expression. Functions that accept a rvalue reference parameter will accept a xvalue expression as its argument but not a lvalue. E.g:

```cpp
#include <utility>
struct S{};
void f(S&& s){}
int main(){
    S a;
    f(a); //error: cannot bind rvalue reference of type ‘S&&’ to lvalue of type ‘S’
    f(std::move(a)); //OK
}
```

As you can see above, the name of a rvalue reference parameter is a lvalue expression and therefore must be converted to a xvalue expression using `std::move` before it can be fed into a move constructor. Thus [the following idiom](http://en.cppreference.com/w/cpp/utility/move):

```cpp
// Simple move constructor
A(A&& arg) : member(std::move(arg.member)) // the expression "arg.member" is lvalue, the expression "std::move(arg.member)" is xvalue
{} 
// Simple move assignment operator
A& operator=(A&& other) {
     member = std::move(other.member);
     return *this;
}
```

### Move semantics

I think a good way to think about move semantics is in terms of moving pointers. E.g:

```cpp
#include <utility> //where std::move is declared
#include <iostream>
#include <string>
#include <cassert>
struct BigObj{
    std::string s{"hahaha"};
};
struct Movable{
    BigObj* b;
    Movable() { b = new BigObj; }
    Movable(Movable&& m) { //copy the target's pointer, then set the target's pointer to null
        std::cout << "move ctor called";
        this->b = m.b;
        m.b = nullptr;
    }
};
int main(){
    Movable m1;
    Movable m2{std::move(m1)};
    assert(m1.b == nullptr);
    std::cout << "\n----\n";
    Movable m3{Movable{}};
    std::cout << m3.b->s;
}
```
Output:
```
move ctor called
----
hahaha
```
Note that `std::move` has nothing to do with moving, it marks an object as "can be moved from". That's all it does. After an object has been moved from, it is in an unspecified state, and operations that would require it to be in a specific state would no longer be valid, but operations that do not require it to be in a specified state, such as destruction, are still valid. 

The canonical example to illustrate difference between copy and move semantics is this:

```cpp
#include <string>
#include <cassert>
int main(){
std::string s1 = "hi!";
std::string s2{s1};
assert(s2==s1);
std::string s3{std::move(s1)};
assert(s3==s1);
}
```
Output:
```
int main(): Assertion `s3==s1' failed.
```

Here, s2 is copy-constructed from s1, whereas s3 is move-constructed from s1. Right after s2 is copy-constructed from s1, we see that s2 and s1 have the same content, whereas when s3 is move-constructed from s1, s3 and s1 do not have the same content. Thinking back to our earlier example, we can think of it as copying the pointers of s1 to s3 and then nulling the pointers in s1. 

If we think of resources as the actual allocated heap memory that an object "holds" (in this context, meaning the memory that the object holds pointers to), moving the object does not actually move that memory, but rather, moves the pointers pointing to that memory. When people explain moving as stealing the resources of an object, they really mean stealing the pointers to the resources of that object rather than stealing the resources themselves. That's a big difference. You can't "move" an object's stack-allocated memory to the heap. You have to copy that. The only thing you can "move" (really, copy and delete) is the pointers to heap storage (and other "movable" resources like file descriptors, sockets, streams etc). 

### Pass by value vs pass by reference 

So, pass by value usually creates an extra copy, whereas passing by reference does not. If you don't want a copy, then it's a no brainer - just always use const reference. It works for both lvalue and rvalue arguments. But let's say we want to store the argument in dynamic storage. Let's write some code to highlight the possible situations:

```cpp
#include <iostream>
#include <vector>
using std::cout;
struct S{ //imagine that S contains some heap-allocated content
    S() { cout << "S default-ctor called\n"; }
    S(const S&) { cout << "S copy-ctor called\n"; }
    S(S&&) { cout << "S move-ctor called\n"; }
};
struct VS{ //vector of S objects
    std::vector<S*> v;
    void add_by_value(S s){ //the pass-by-value-and-then-move-construct idiom
        v.push_back(new S{std::move(s)});
    }
    void add_by_ref(const S& s){ //the pass-by-const-reference-and-then-copy-construct idiom
        v.push_back(new S{s});
    }
    void add_by_ref(S&& s){ //the pass-by-rvalue-reference-and-then-move-construct idiom
        v.push_back(new S{std::move(s)}); 
    }
};
S f(){
    return S{};
}
int main(){
    VS vs;
    S s;
    cout << "--calling add_by_value with lvalue--\n";
    vs.add_by_value(s);
    cout << "--calling add_by_ref with lvalue--\n";
    vs.add_by_ref(s);
    cout << "--calling add_by_value with rvalue--\n";
    vs.add_by_value(f());
    cout << "--calling add_by_ref with rvalue--\n";
    vs.add_by_ref(f());
}
```
Output:
```
S default-ctor called
--calling add_by_value with lvalue--
S copy-ctor called
S move-ctor called
--calling add_by_ref with lvalue--
S copy-ctor called
--calling add_by_value with rvalue--
S default-ctor called
S move-ctor called
--calling add_by_ref with rvalue--
S default-ctor called
S move-ctor called
```

First, a word of explanation. When we are passing a lvalue, we usually don't want to modify the passed object (since it's a lvalue, and we probably want to continue using it afterwards). Pass by value cannot modify the passed argument because it copy-constructs anyways, and in pass-by-reference we use a const reference to ensure that the passed argument is not modified. When we are passing by rvalue, we don't care if we modify the temporary (or local variable) that is generated since it will expire at the end of the function call anyway. So we must compare efficiency in these 2 contexts separately. It makes no sense to compare the efficiency of pass-by-value with lvalue and pass-by-reference with rvalue for example. 

In the case of passing a lvalue, pass-by-value creates an extra move-constructor call when compared to pass-by-const-reference. A pass-by-value function in this case will copy-construct the passed object, and since we don't need the newly created object we can move-construct it into heap storage. On the other hand, a pass-by-const-reference function can directly copy-construct it into heap storage, without having to copy-construct in stack storage and then move-construct it into heap storage afterwards. So in this case pass-by-const-reference is more efficient. This especially matters if your object is expensive to move. Here the best case is one copy constructor call. 

In the case of passing a rvalue, it doesn't seem to matter whether you pass by value or by reference. In both cases, the rvalue will materialize a temporary, which will then be move-constructed into heap storage. Here the best case is one default constructor call to create the temporary and one move constructor call to steal from the temporary. Fun fact: In C++11 without copy elision pass by value would have one more move constructor call than pass by rvalue reference, because the rvalue itself is a temporary that would have to get moved into the value argument. In C++17 however, since the rvalue is no longer a temporary, it is directly used to construct the value parameter and therefore there is no extra move needed. That's why if you compile with copy elision disabled (with `-fno-elide-constructors` on gcc) with `-std=c++11` then you see one more move constructor call for pass by value but with `-std=c++17` you see only one move constructor call for both types of argument passing. If you want to have just one constructor call without the corresponding move-constructor call then you should use something like the emplace_back from std::vector. You have to avoid passing the rvalue into a function call because then a local temporary is created no matter what. 

See code:

```cpp
#include <iostream>
struct S{
  S() = default;
  S(int) { std::cout << "int ctor called" << std::endl;  }
  S(const S&){
    std::cout << "copy ctor called" << std::endl;
  }
  S(S&&){
    std::cout << "move ctor called" << std::endl;
  }
};

void pv(S s) { S b = std::move(s); }
void pr(S&& s) { S b = std::move(s); }
S gets(){return S();}
int main(){
S s;
std::cout << "pv:";
pv(gets());
std::cout << "pr:";
pr(gets());
}
```

Compile that in both C++11 and C++17 modes with copy elision disabled and see what happens. You should see that in C++11 the pass by value makes one more move constructor call than pass by rvalue reference but in C++17 they both make only one move constructor call. 


### Return by value vs pass by reference

Sometimes you want to create a large object inside a function and then return it. If you create it as a local object and then return it by value, the best you can do is to move from it (without optional copy elision). Example:

```cpp
#include <iostream>
struct S{
    int i = 9;
    S(){ std::cout << "default ctor called\n"; }
    S(const S&){ std::cout << "copy ctor called\n"; }
    S(S&&){ std::cout << "move ctor called\n"; }
    void m(){
        i = 10;
    }
};
S f(){
    S s;
    s.m();
    return s;
}
int main(){
    S* s = new S{f()};
}
```
Output when compiled with `-fno-elide-constructors`:
```
default ctor called
move ctor called
```

Now, on my compiler, when compiled with copy elision enabled, only the default constructor is called, but you can't rely on that. This optional copy elision is called Named Return Value Optimization (NRVO), and is distinct from the RVO guaranteed by the C++17 standard which is only for prvalues such as `T()`. 

In the case of an object like std::vector where most of the memory is allocated on the heap, this isn't that bad, but still it would be more efficient to create a std::vector on the caller's side and then pass that into the function by reference. 

If you return by value, you have to (conceptually at least) move-construct the object from the object created in the function to the object on the caller's side. This is not very efficient. So if we want to avoid that, depending on your requirements, you could create the object on the caller's side and then pass it into the function by reference. Or you could dynamically allocate the object inside the function and return a pointer. Whilst this would be just as efficient as passing by reference, this way the caller would have to manually delete the object later. 

## Enums

Enums are implicitly converted to ints in many contexts, and that along with their global namespacing (which means you cannot distinguish between a enum of one "type" from another) makes them unsafe. Example:

```cpp
#include <iostream>
enum Mood { happy, blue };
enum Color { red, green };
int main(){
  Color c = green;
  std::cout << std::boolalpha << (c == blue);
}
```
Output:
```
true
```
Some more examples:

int = enum is ok, enum is implicitly converted to int
enum = int is not allowed. Same with enum = enum + 1
enum = static_cast<enum>(int) is the way to do it
enum = (enum)int uses C style casting
cout <code><<</code> enum will print an int
enum {a=-1, b=2, c} //a = -1, b = -2, c = -1. This is fine. 
enum {a=1, b=1, a = 1} //invalid, you redefined a
enum {dog, cat}; enum {cat, ls} //not allowed, enum names are all global

You should prefer enum classes to enums. 

### Enum classes

C++11 introduced strongly typed enums - enum classes. Now you can have:

enum class Color {RED, BLUE};
enum class Mood {SAD, BLUE}; 

To access them you'd write Color::BLUE. 

## I/O

According to [ISOCPP](https://isocpp.org/wiki/faq/input-output) the idiomatic way to read in input in C++ is this:

```cpp
#include <iostream>
int main()
{
  int i = 0;
  while (std::cin >> i) {    // GOOD FORM
    if (i == -1) break;
    std::cout << "You entered " << i << '\n';
  }
}
```
>This will cause the while loop to exit either when you hit end-of-file, or when you enter a bad integer, or when you enter -1.

In Linux you can enter the EOF file character using Ctrl-D, in Windows you use Ctrl-Z. The <code>cin >> i</code> statement returns a reference back to the cin object, which is then implicit converted into a bool, and the returned value is false if the cin object is in a "bad state", which can be caused by hitting EOF. 

## Const

First, we must distinguish between const objects and references or pointers to const. 

An object declared const is not supposed to modified, ever. You can get around this by using const_cast, but if you actually modify an object that was declared const (the only exception is mutable members of const objects), then you get undefined behavior. 

From C++17 Standard Section 10.1.7.1 The cv-qualifiers:

>Except that any class member declared mutable can be modified, any attempt to modify a const object during its lifetime results in undefined behavior. 
>```cpp
>const int ci = 3;                       // cv-qualified (initialized as required)
>ci = 4;                                 // ill-formed: attempt to modify const
>
>int i = 2;                              // not cv-qualified
>const int* cip;                         // pointer to const int
>cip = &i;                               // OK: cv-qualified access path to unqualified
>*cip = 4;                               // ill-formed: attempt to modify through ptr to const
>
>int* ip;
>ip = const_cast<int*>(cip);             // cast needed to convert const int* to int*
>*ip = 4;                                // defined: *ip points to i, a non-const object
>
>const int* ciq = new const int (3);     // initialized as required
>int* iq = const_cast<int*>(ciq);        // cast required
>*iq = 4;                                // undefined: modifies a const object
>```

In the code above, we have 2 objects, ci, which is declared a const int, and i, which is declared as a non-const int. We then have cip, which is a pointer to const int, and we point cip at i. 

Then we const_cast away the constness of cip, i.e we turn cip from a pointer to const int to a pointer to int, and we can modify i through cip now and that's fine because i is a non-const int. 

This shows that you can point to the same object via both pointers-to-nonconst and pointers-to-const. The type of the pointer determine whether or not you can modify the object via that pointer, not the actual constness of the object itself. 

The `ciq` example shows creating a const int, pointing to it via a pointer to const int, then casting away the constness of the pointer using const_cast, and then modifying the const int via the resulting pointer to int. This results in undefined behavior: if you attempt to modify an object that was originally declared const, like this:

```cpp
#include <iostream>
int main(){
  const int i = 1;
  int* p = const_cast<int*>(&i);
  (*p) = 5; //UB
  std::cout << p << '\t' << (*p) << std::endl;
  std::cout << &i << '\t' << i;
}
```
Output:
```
[some address]  5
[same address]  1
```

What the output appears to indicate is that the very same variable at the same memory location gives 2 different values when accessed directly vs when accessed through a pointer.  Whilst the code at the time of writing compiles without any warnings in clang7 and runs, it is undefined behavior. You should never modify an originally-const object. 

Here's another example:

```cpp
int main(){
  const char* s = "lol";
  char* p = const_cast<char*>(s);
  p[0] = 'n';
}
```
This one, fortunately, produces a seg fault on my machine. 

### Reading const declarations

Well, let's start with a review of how to read pointer and reference declarations first:

`T*` <- This is a "pointer to T"
<code>T&</code> <- This is a "reference to T"
`T**` <- This is a "pointer to pointer to T"
<code>T&&</code> <- This is a "rvalue reference to T"

So, the rule is that you **read from right to left** (except for the rvalue reference...you can't have a reference to reference). So you see a `*` and you say "pointer to", and then you see `*` again and say "pointer to" again, same with <code>&</code> and "reference to". There's a nice document [here](https://gist.github.com/c00kiemon5ter/3023944) that explains it in more detail. 

A note here:

<code>T* x, y</code> Declares x as a pointer to T, but y is a T, NOT a pointer to T. That's why some people prefer the C style pointer declaration: `T *x, y`, because it better captures the fact that x and y do not have the same type. 

Recall that pointers are actual objects whilst references are not really objects. So you can have:

<code>T*&</code> <- This is a reference to a pointer

But you cannot have <code>T&*</code>, i.e you cannot have a pointer to a reference, since you cannot take the address of a reference - it's not an object, and does not have an address. 

Okay, so let's start reading const declarations:

```cpp
const T* p; //pointer to const T
T const* p; //same as above
const T& r; //reference to const T
T const& r; //same as above
```

If you like consistency, then you would prefer the const-on-the-right style, because the const-on-the-left style only works in certain situations, whereas the const-on-the-right style always works. So for the rest of this section we will only use the const-on-the-right style (also called "consistent const" see [ISOCPP](https://isocpp.org/wiki/faq/const-correctness)).

```cpp
T const * p; //pointer to const T
T const & p; //reference to const T
T * const p; //const pointer to T
T & const p; //const reference - ILLEGAL
T const * const p; //const pointer to const T
T const & const p; //const reference - ILLEGAL
T ** const p; //const pointer to pointer to T
T * const * p; //pointer to const pointer to T
T * const & p; //reference to const pointer to T
T const ** p; //pointer to pointer to const T
T const * const p; //const pointer to const T
```

So, it is illegal to declare a const reference - because references are always const. You can never change the object which a reference is an alias for (though the object could expire before its reference does, leaving a dangling reference). 

### Const infection

We can also declare a member function const. What does that actually mean? It means that that function cannot directly modify **any** of the member variables of the class (whether const or non-const) that it's in **unless** that member variable is declared mutable:

```cpp
struct S{
  S() = default;
  int a;
  mutable int b;
  //const mutable int c; Error: can't mix const and mutable
  void f () const {
    a = 1; //Error: a is not mutable
    b = 1;
  }
};
int main(){
  S o;
  o.f();
}
```

This does NOT stop the const member function from changing the members variables through a reference passed in or through global reference however. As the [ISOCPP FAQ says](https://isocpp.org/wiki/faq/const-correctness#const-member-fns):

>The trailing const on inspect() member function should be used to mean the method won’t change the object’s abstract (client-visible) state. That is slightly different from saying the method won’t change the “raw bits” of the object’s struct. C++ compilers aren’t allowed to take the “bitwise” interpretation unless they can solve the aliasing problem, which normally can’t be solved (i.e., a non-const alias could exist which could modify the state of the object). Another (important) insight from this aliasing issue: pointing at an object with a pointer-to-const doesn’t guarantee that the object won’t change; it merely promises that the object won’t change via that pointer.

Another way to think about this is discussed [here](https://stackoverflow.com/questions/3141087/what-is-meant-with-const-at-end-of-function-declaration):

>Another way of thinking about such "const function" is by viewing an class function as a normal function taking an implicit this pointer. So a method int Foo::Bar(int random_arg) (without the const at the end) results in a function like int Foo_Bar(Foo* this, int random_arg), and a call such as Foo f; f.Bar(4) will internally correspond to something like Foo f; Foo_Bar(&f, 4). Now adding the const at the end (int Foo::Bar(int random_arg) const) can then be understood as a declaration with a const this pointer: int Foo_Bar(const Foo* this, int random_arg). Since the type of this in such case is const, no modifications of member variables are possible.

Note that if you pass in an object as a const reference or pointer, **then you can ONLY call const functions on that object**. This is one of the things that makes const infectious - if you pass in an object via a const pointer or reference, and you want to call some functions on that object, then you find that you can't call the functions you want because they aren't const, so you go and make them const, and then you have to change more things and so on. That's why ISOCPP suggests getting const-correctness right [at the very beginning](https://isocpp.org/wiki/faq/const-correctness). 

Example:

```cpp
#include <iostream>
struct S{ 
  int i = 42;
  void f () const { 
    auto t = const_cast<S*>(this); //extremely evil
    t->i = 66;
  }
  void g () {}
};
void f(S const& o){
  std::cout << o.i;
  o.f(); //const function modifies the object
  std::cout << o.i;
  //o.g(); //Error: g is not marked const
}
int main(){
  const S o;
  //o.f(); //Undefined behavior
  //o.g(); //Error: g is not marked const
  S o2;
  f(o2); //This is fine. 
}
```
Output:
```
4266
```

So. Const methods may in fact mutate the state of the object, it is up to the programmer to not go out of their way to do so. They are a useful convention and gets you some help from the compiler, that's all. 

You should also note that [const semantics don't kick in until the object is fully constructed](https://stackoverflow.com/questions/2271046/if-changing-a-const-object-is-undefined-behavior-then-how-do-constructors-and-des). See C++17 Standard Section 15.1 Constructors:

>A constructor can be invoked for a const, volatile or const volatile object. const and volatile semantics ([dcl.type.cv]) are not applied on an object under construction. They come into effect when the constructor for the most derived object ends.



# Inheritance

"The child contains its parents". I just made that up but I think it gives some intuition of what inheritance really is. The "is a" relation that inheritance (well, public inheritance at least) models really should be called "is a kind of" or "is a specialization of". 

There are 3 modes of inheritance in C++: public, protected, and private. I will not delve into exactly when you should use each type of inheritance because that's quite a complex topic (see [this](http://www.gotw.ca/publications/mill06.htm) and [this](http://en.cppreference.com/w/cpp/language/derived_class) to have some idea of what is involved). 


The default type of inheritance depends on the type of the child and not the type of the parent. For example:

```cpp
struct S{ int i; };
class C{ public: int i; };
struct ScS : S {};
struct ScC : C {};
class CcS : S {};
class CcC : C {};
int main(){
  ScS scs; ScC scc; CcS ccs; CcC ccc;
  scs.i; 
  scc.i;
  ccs.i;
  ccc.i;
}
```
Output:
```
error: ‘int S::i’ is inaccessible within this context
   ccs.i;
       ^
error: ‘int C::i’ is inaccessible within this context
   ccc.i;
       ^
```

As you can see, ccs.i and ccc.i are inaccessible, whilst scs and scc are fine. This shows that if the child is a class, then the default inheritance type is private, whereas if the child is a struct, then the default inheritance type is public: It appears that to determine the default type of inheritance **the type of the parent does not matter, only the child** (in fact, both structs and classes are considered classes in C++). 

A derived class will "override" parent class member functions. More on this in a bit. 

## `final` and `override`

In c++11 two new specifiers were introduced: `final` and `override`. These are totally optional and do not change the semantics of your program, but judicious use of these specifiers will make it harder for you to make mistakes. 

The more straightforward use of `final` is to mark a class as non-inheritable, i.e it cannot be inherited from. Example:

```cpp
class Base final {};
class Derived : public Base {};
int main(){}
```

This fails to compile because Base is marked final, and Derived tries to inherit from it. 

The `override` specifier is used to mark a virtual function as overriding a parent virtual function with the same signature. It ensures that the function is indeed overriding a parent virtual function and not a new function that's not overriding anything. [From cppreference](http://en.cppreference.com/w/cpp/language/override):

```cpp
struct A
{
    virtual void foo();
    void bar();
};
 
struct B : A
{
    void foo() const override; // Error: B::foo does not override A::foo
                               // (signature mismatch)
    void foo() override; // OK: B::foo overrides A::foo
    void bar() override; // Error: A::bar is not virtual
};
```

This is useful because it's checked at compile time, so helps to detect errors with inheritance. Whenever you override a virtual member function you should **always** mark the overriding function with the `override` keyword to make your intent clear and to prevent subtle bugs. I cannot think of any situation where it can be used but shouldn't. 

`final` is different from `override` in that it can be used in 2 cases: on a base virtual member function, as well as on an inherited virtual member function, whereas `override` can only be used on inherited (hence the name, "override") virtual member function. 

When `final` is used on a base virtual member function, that member function has to be declared virtual as well (you cannot have a non-virtual final member function). Example:

```cpp
class S{
  void f () final{} //Error: f() is not virtual
  virtual void g () final{} //Correct.
};
int main(){}
```

In the second case the virtual specifier is redundant, [example](http://en.cppreference.com/w/cpp/language/final):

```cpp
struct Base
{
    virtual void foo();
};
 
struct A : Base
{
    virtual void goo() final; //final base virtual function
    void foo() final override; // A::foo is overridden and it is the final override
};
 
struct B final : A // struct B is final
{
    void foo() override; // Error: foo cannot be overridden as it's final in A
    void goo() override; // Error: goo cannot be overridden as it's final in A
};
int main(){}
```

In all cases (base vs inherited virtual member function) the effect is that the function marked `final` is made unoverridable. 

## Member functions

You may declare a function inside a class and define it elsewhere. You cannot define a function outside of a class without first declaring it inside the class. Example:

```cpp
struct S{
  void f();
};
void S::f(){}
int main(){}
```

### Virtual functions, object slicing, and polymorphism

Normally the compiler decides at compile time which function to call. But sometimes we want the decision to depend on the type of the actual object itself so the decision must take place at runtime, example:

```cpp
void f(Base b){
  b.m();
}
```

Where we want the member function m() to differ depending on b's actual type rather than the base class's implementation. This is achieved through the use of virtual functions (which is typically implemented using vtables which allow runtime function lookup, which incurs a performance overhead of 5-15% depending on situation). In this example we make the m member function virtual:

```cpp
struct Base {
  public:
    virtual void m() {}
};
```

Here's what it means: normally, if you cast a subclass object to its base class, then the behavior changes to its base class. However, for virtual functions the behavior stays that of the subclass whilst the type changes to that of the base class. This means that, given an object of type Base, if you don't know all of the subclasses of class Base, then you don't know what Base's virtual functions do for that object. 

If you want to call the original m on a derived object then you'd call <code>derived_ptr->Base::m()</code>.

Example:

```cpp
#include <iostream>
class X{
  public:
    X(){}
    X(const X&) {std::cout << "X copy ctor called!"; }
    virtual void f() { std::cout << "X"; }
};
class Y: public X{
  public: 
    Y(){}
    Y(const Y&) {std::cout << "Y copy ctor called!"; }
    void f() { std::cout << "Y"; }
};
class Z: public Y{
  public:
    Z(){}
    Z(const Z&) {std::cout << "Z copy ctor called!"; }
    void f() { std::cout << "Z"; }
};
int main(){
  Z *z = new Z();
  static_cast<Y*>(z)->f();
  Z z2;
  static_cast<Y>(z2).f();
}
```
Output:
```
ZY copy ctor called!Y
```
I used static_cast here because I know the cast is valid, but you can also use dynamic_cast to cast an object up or down (or even sideways along) the inheritance hierarchy. The difference is that static_cast is compile-time only (does not do runtime check) whereas dynamic_cast does a runtime check (meaning worse performance but more safe), returning NULL if a pointer conversion is impossible:

Code to [test if an object is of a type](https://stackoverflow.com/questions/9704893/c-dynamic-cast-vs-typeid-for-class-comparison)
```cpp
if(A* test = dynamic_cast<A*>(&obj)) {
    // obj is of type A or a type publicly derived from A
}
if(typeid(someClassCVar) == typeid(A)) {
   // obj is of type A, not a derived type
}
```

or throws std::bad_cast if a reference conversion is impossible, which you might want in situations where you can't know if the cast would be valid at compile time. 

So, what does this example show us? 

First of all, it shows that the "virtualness" of a member function is **determined by where it is first declared**, because "virtualness" is inherited, meaning the member function f() in class Y and Z are in fact both virtual, even though they were not declared virtual. **It is therefore redundant to declare a member function virtual** if it was already declared virtual in a base class. This can be seen in that if you create a Z, and then static cast a pointer to that object into a pointer to a Y, then calling functions from that pointer will still invoke Z functions rather than Y functions. This is fairly easy to grasp. 

Secondly, notice how the result from static casting a pointer is different from static casting an object. Normally, when we want to make use of dynamic dispatch we upcast or downcast (the difference is that children ARE parents, but parents absolutely are NOT children, which means upcasting makes logical sense whereas downcasting is rarely ever a good idea) pointers or references to objects, not the objects themselves. When you cast to an object type instead of a pointer type, it calls the copy constructor on the base object with the derived object as the argument. 

From The C++17 standard section 8.5.1.9 Static cast:

>The result of the expression `static_­cast<T>(v)` is the result of converting the expression v to type T. If T is an lvalue reference type or an rvalue reference to function type, the result is an lvalue; if T is an rvalue reference to object type, the result is an xvalue; otherwise, the result is a prvalue. 

So, if T is not a reference type, then the result is a prvalue, which is used to initialize an object. In this case a temporary is materialized in order to call the f() function. In other words the code:

```cpp
static_cast<Y>(z2).f();
```
Is equivalent to this:
```cpp
Y{z2}.f();
```

Now, we saw that the copy constructor for Y was called. However, we also know that this copy constructor takes a reference to a base type object as its parameter, **not** a reference to a derived type object, so what's going on here? 

As it turns out, in C++, the base class copy constructor can take an object of derived type, and treat it as if it was a base class object, by "slicing" off the parts that don't exist in the base type. This is called "slicing" (see TC++PL) and is a common source of unpleasant surprises. Of course, you have also noticed that the virtual inheritance failed in the second case (Y is printed). This is, of course, because slicing gets rid of the virtual inheritance as well. Since a temporary Y object is constructed, the f() function called is from that temporary, **not** the f() function from the `z2` object. And that's why you always upcast with pointers or references. To avoid slicing. 

Of course, the problem of slicing is not only limited to casting, it is in fact a much more pervasive problem. Here I will only illustrate one more example but there are so many situations where it occurs - you really have to be careful to avoid blowing your leg off with this stuff:

```cpp
#include <iostream>
using std::cout;
struct Base {
  virtual void f() { cout << "Base called"; }
};
struct Derived : public Base {
  void f() { cout << "Derived called"; }
};
void f(Base b){
  b.f();
}
void g(Base& b){
  b.f();
}
int main(){
  Derived d;
  f(d);
  cout << "\n-----\n";
  g(d);
}
```
Output:
```
Base called
-----
Derived called
```
So, slicing also happens whenever you pass a derived object to a function that takes a base object by value. But it doesn't happen if the function takes a reference. Thus, slicing provides another reason to use pointers and references instead of passing by value. The fun never stops!

#### Calling a virtual function from a constructor or destructor

The C++17 Standard Section 15.7 Construction and destruction says this:

>Member functions, including virtual functions, can be called during construction or destruction ([class.base.init]). When a virtual function is called directly or indirectly from a constructor or from a destructor, including during the construction or destruction of the class's non-static data members, and the object to which the call applies is the object (call it x) under construction or destruction, **the function called is the final overrider in the constructor's or destructor's class and not one overriding it in a more-derived class**. If the virtual function call uses an explicit class member access and the object expression refers to the complete object of x or one of that object's base class subobjects but not x or one of its base class subobjects, the behavior is undefined.

As explained in [the ISOCPP FAQ](https://isocpp.org/wiki/faq/strange-inheritance#calling-virtuals-from-ctors), it would be dangerous for a base class to call a virtual function if the derived class object hasn't even been constructed yet. Therefore it is not allowed for a constructor to call a derived virtual function, i.e when you call a virtual function in the constructor of a class, the version of the virtual function that gets called is that class's version and not the version in its derived classes. Example:

```cpp
#include <iostream>
struct Base{
  Base(){ f(); }
  virtual void f(){ std::cout << "Base f\n";}
};
struct Derived : public Base{
  void f(){ std::cout << "Derived\n";}
};
int main(){
  Derived d;
}
```
Output:
```
Base f
```

The example above shows that the virtual call mechanism is **disabled** in the constructor. Likewise the same happens in the destructor because what are you doing calling a function of a derived class which may be using resources that have already been released? See the [SERT guideline](https://wiki.sei.cmu.edu/confluence/display/cplusplus/OOP50-CPP.+Do+not+invoke+virtual+functions+from+constructors+or+destructors): 
```cpp
struct A {
  A() {
    // f();   // WRONG!
    A::f();   // Okay
  }
  virtual void f();
};
```
It's a good idea to make it clear that you are calling the current class's function in the constructor using the scope resolution operator (unless you have marked the function or class final). 

## Member access

<code>p->b</code> is the same as `(*p).b` (parentheses needed because of C++'s retarded operator precedence) however <code>-></code> can be overloaded whereas `.` and `::` cannot. C::m can be used to access static members of class C, as well as its functions. You can also use `.` to access static members from an object as well. 

For obvious reasons, a static member function cannot call a non-static member function, and you cannot use the "this" pointer in a static member function (you can get around that by having the static member function take a reference parameter to an object of its class). 

## Constructors

The first thing to know about constructors is that they aren't real functions. They don't have any return type (not even void) and cannot be called on an object. They do not even have names:

C++17 standard Section 15.1:

>A constructor is used to initialize objects of its class type. **Because constructors do not have names, they are never found during name lookup**; however an explicit type conversion using the functional notation (8.2.3) will cause a constructor to be called to initialize an object. 

So you can't do this:

```cpp
C obj;
obj.C(); //illegal
C::C(); //illegal
```

If you don't declare any constructors, the compiler will generate an implicit default constructor. If you declare any constructors then the default constructor is not generated. You may force it to be generated using the `= default;`. Note that this is not the case for other constructors - if you write your own default constructor but not a copy constructor the compiler will still generate an implicit copy constructor for you. Note that a constructor where all parameters have default values can be treated as a default constructor. 

[Here is a summary from Microsoft](https://msdn.microsoft.com/en-us/library/dn457344.aspx):

>If any constructor is explicitly declared, then no default constructor is automatically generated.
>
>If a virtual destructor is explicitly declared, then no default destructor is automatically generated.
>
>If a move constructor or move-assignment operator is explicitly declared, then:
>
>- No copy constructor is automatically generated.
>
>- No copy-assignment operator is automatically generated.
>
>If a copy constructor, copy-assignment operator, move constructor, move-assignment operator, or destructor is explicitly declared, then:
>
>- No move constructor is automatically generated.
>
>- No move-assignment operator is automatically generated.

The second thing to remember about constructors is that the order of construction (i.e the order in which the constructors are called) of an object is as follows:

1. The base class object, from left to right. Virtual inheritance has higher precedence. 
2. Member fields in order of declaration. 
3. The class itself. 

From C++17 Standard Section 15.6.2.13:

>In a non-delegating constructor, initialization proceeds in the following order:
>
>- First, and only for the constructor of the most derived class, virtual base classes are initialized in the order they appear on a depth-first left-to-right traversal of the directed acyclic graph of base classes, where “left-to-right” is the order of appearance of the base classes in the derived class base-specifier-list.
>
>- Then, direct base classes are initialized in declaration order as they appear in the base-specifier-list (regardless of the order of the mem-initializers).
>
>- Then, non-static data members are initialized in the order they were declared in the class definition (again regardless of the order of the mem-initializers).
>
>- Finally, the compound-statement of the constructor body is executed.
>
>[ Note: The declaration order is mandated to ensure that base and member subobjects are destroyed in the reverse order of initialization. — end note ]

Example:

```cpp
#include <iostream>
using std::cout;
struct A{ A() { cout << "A constructed\n"; } ~A() { cout << "A destructed\n"; }};
struct B{ B() { cout << "B constructed\n"; } ~B() { cout << "B destructed\n"; }};
struct C{ C() { cout << "C constructed\n"; } ~C() { cout << "C destructed\n"; }};
struct M1{ M1() { cout << "M1 constructed\n"; } ~M1() { cout << "M1 destructed\n"; }};
struct M2{ M2() { cout << "M2 constructed\n"; } ~M2() { cout << "M2 destructed\n"; }};
struct M3{ M3(int) { cout << "M3 constructed\n"; } ~M3() { cout << "M3 destructed\n"; }};
struct M4{ M4(int) { cout << "M4 constructed\n"; } ~M4() { cout << "M4 destructed\n"; }};
struct E : public B, public C, public A {
  E() : m4(0), m3(0) { cout << "E constructed\n"; }
  ~E() { cout << "E destructed\n"; }
  M3 m3;
  M2 m2;
  M4 m4; 
  M1 m1; 
};
int main(){
  E e;
}
```
Output:
```
B constructed
C constructed
A constructed
M3 constructed
M2 constructed
M4 constructed
M1 constructed
E constructed
E destructed
M1 destructed
M4 destructed
M2 destructed
M3 destructed
A destructed
C destructed
B destructed
```
As you may notice here, the order of initialization of members is in the order they are declared NOT the order they are initialized in the member initializer list (which btw can only exist in the definition not the declaration of a constructor). So here since m3 is declared first, that means m3 gets initialized first even though it's initialized second in the initializer list. GCC and clang helpfully warns us when we get the order wrong in the initializer list. 

By the way you probably shouldn't use structs in the same way you use classes (like I did here) - it's a C++ convention that structs are only used for POD (Plain Old Data) types and classes are used instead for actual object-oriented programming. Always a good idea to stick to convention unless you have a special reason not to. 

### [Most vexing parse](https://wiki.sei.cmu.edu/confluence/display/cplusplus/DCL53-CPP.+Do+not+write+syntactically+ambiguous+declarations)

Pop quiz, what is the type of `obj` in the following code?

```cpp
#include <iostream>
struct S{
  int v=42;
};
int main(){
  S obj();
  std::cout << obj.v;
}
```

In fact, obj is a function. Makes sense right? If you write `T f();` then f is a function that returns a T. Helpfully, clang++ tells us about the vexing parse. Here's the error message:

```
error: base of member reference is a function; 
```

**As a general rule, always remember that whenever a statement is ambiguous, the C++ compiler will always interpret it in the way that you don't expect it to**. In this case the declaration of `obj` could be interpreted either as an object of type S or as a declaration for a function that takes no parameters and returns a S, and the C++ compiler chooses the latter meaning. So what do you do if you want to declare an object instead of a function? In "modern C++" you do that with `S obj;` rather than `S obj();`. In fact in C++11 you can also do `S obj{};`. 

If you try to declare and initialize a variable and pass it a function call as its argument then that statement will be interpreted as a function declaration rather than a variable declaration. This can be avoided using the uniform initialization syntax. Example:

```cpp
#include <iostream>
struct S{
  int v=9;
};
int main(){
  S a(S()); //declares a function
//  std::cout << a.v;
  S b((S())); //declares a variable
  std::cout << b.v;
  S c{S()}; //also declares a variable
  std::cout << c.v;
}
```

If you uncomment the line in the code above then you see an error:

```
error: member reference base type 'S (S (*)())' is not a structure or union
```

As [GOTW #1](https://herbsutter.com/2013/05/09/gotw-1-solution/) states:
>Scott Meyers long ago named this “C++’s most vexing parse,” because the standard resolves the parsing ambiguity by saying: “if it can be a function declaration, it is.”

It is not only function declarations, but variable declarations also, as the CERT C++ Coding Standard points out:

```cpp
#include <mutex>
 
static std::mutex m;
static int shared_resource;
 
void increment_by_42() {
  std::unique_lock<std::mutex>(m);
  shared_resource += 42;
}
```
Instead of creating an anonymous unique_lock object with the mutex m as the argument, this code actually declares an unique_lock object called m, which shadows the static global variable m. clang++ will give you this warning:

```
warning: declaration shadows a variable in the global namespace [-Wshadow]
```
Which should alert you to the fact that something is wrong. 

As it turns out, this:

```cpp
  std::unique_lock<std::mutex>(m);
```
Is equivalent to this:

```cpp
  std::unique_lock<std::mutex> m;
```

In much the same way that `int(i);` does not declare an anonymous int passing i as the argument but rather is equivalent to `int (i);` which is the same as `int i;`. The fun never stops. 


### Derived default constructor

**If you don't define a derived default constructor, or if you don't specify which base constructor to call, then it calls the base default constructor**:

```cpp
#include <iostream>
class B{ public: B(){std::cout<< "B constructed";}};
class D: public B { public: D(){} };
class E: public B { };
int main(){ D d; std::cout << "\n"; E e; }
```
Output:
```
B constructed
B constructed
```

In other words:

```cpp
D(){}
D():B(){}
```
Are equivalent. 

To disable calling the default base class constructor you'd need to explicitly call a different base constructor:

```cpp
#include <iostream>
class B{
  public: 
    B() { std::cout << "B constructed"; }
    B(int) {std::cout << "B constructed with int\n"; }
};
class D:public B{
  public:
    D():B(0) {std::cout << "D constructed"; }
};
int main(){ D d; }
```
Output:
```
B constructed with int
D constructed
```
There's no way (AFAIK) to avoid calling a base class constructor. You have to construct the base class object somehow. And the base object gets constructed first, before the derived object's constructor is called. Which means that the derived class constructor can make full use of the base object. 

## Accessibility of members

You can only access a private member from within that class or from a friend of that class. You cannot access a private member from a derived class (unless it's a friend). 

To access an overridden base class member, use the scope resolution operator:

```cpp
#include <iostream>
class B{public:int i=42;};
class D:public B{public: int i = 1; void f(){B::i=5;}};
int main(){ 
  D d; 
  std::cout << d.i << d.B::i << std::endl; 
  d.f(); 
  std::cout << d.i << d.B::i << std::endl;
}
```
Output:
```
142
15
```
You may think of the derived object as containing an entire base class object inside it (a parent as a subobject of a child). You may access it almost as if it was a member object. You can access its member variables and functions etc. 

## C++ object model

Inside the C++ Object Model by Stanley Lippman is a good book for this. 

You may have wondered, how is it possible that we can assign a pointer-to-child to a pointer-to-base? Because a child IS-A base...but more precisely, how does that work? [Let's look at the memory layout](https://stackoverflow.com/questions/8672218/memory-layout-of-inherited-class):

```cpp
#include <iostream>
#include <cstdint> //uintptr_t is an integer type big enough to hold a void*
struct Parentbase {
  int a, b;
};
struct Childerived : Parentbase {
  int c, d;
};
int main(){
  Parentbase pb;
  Childerived cd;
  std::cout << uintptr_t(&pb.a) - uintptr_t(&pb) << std::endl;
  std::cout << uintptr_t(&pb.b) - uintptr_t(&pb) << std::endl;
  std::cout << uintptr_t(&cd.a) - uintptr_t(&cd) << std::endl;
  std::cout << uintptr_t(&cd.b) - uintptr_t(&cd) << std::endl;
  std::cout << uintptr_t(&cd.c) - uintptr_t(&cd) << std::endl;
  std::cout << uintptr_t(&cd.d) - uintptr_t(&cd) << std::endl;
}
```
Output:
```
0
4
0
4
8
12
```
The output shows that in both the Parentbase and Childerived objects, the a and b variables are stored at byte offsets 0 and 4 respectively (makes sense because a and b are ints, and in this case an int takes up 4 bytes). Then in the case of the Childerived object the c and d variables are stored afterwards. 

We may draw a rudimentary representation of the memory layout of a Childerived object:

```
----------------Childerived object----------------
--------- Start of Parentbase subobject ----------
a - Offset 0
b - Offset 4 
---------- End of Parentbase subobject -----------
------ Start of rest of Childerived object -------
c - Offset 8 
d - Offset 12
--------------------------------------------------
```

This gives some intuition behind the idea that you can simply convert a pointer to a Childerived object into a pointer to a Parentbase object - because a Childerived object in fact consists of the Parentbase object followed by its own member variables. Thus it is useful to think of the Parentbase object as a subobject of the Childerived object. 

Note that **OBVIOUSLY** this behavior (I'm referring to the memory layout) is not mandated by the C++ language standard so you should definitely not rely on it. It's just something to help form an intuitive understanding of how the pointer upcast works. 

From [this Dr.Dobbs article](http://www.drdobbs.com/cpp/multiple-inheritance-considered-useful/184402074):
>Because I am using public inheritance, the C++ specification lets you implicitly convert a pointer to an object into a pointer to its public base. **If you use the layout in Figure 1, then this implicit conversion does not actually change the value of the pointer, just how the compiler treats its type. In other words, there is no extra overhead associated with this assignment.**

## this

As mentioned before, every class has a "this" pointer which points to the object itself. It is a prvalue expression which means you cannot modify it, meaning you can't do this:

```cpp
this = new T(); //this is illegal
```

## Copy constructor and copy elision

The copy constructor is implicitly defined if the user does not supply a copy constructor (and if there is no user-defined move constructor or move assignment operator). This implicitly defined copy constructor does memberwise copy, in order of initialization (i.e order of declaration, see above), using direct initialization. Member objects are copied using their copy constructors, and arrays are copied element by element as you'd expect, but pointers are copied without also copying the memory regions that they refer to, which can lead to subtle bugs. Example:

```cpp
#include <iostream>
using std::cout;
struct S {
  int* i;
  S(){
    i = new int{0};
  }
};
int main(){
  S a, b=a;
  cout << std::boolalpha << (a.i == b.i);
  cout << ++*a.i;
  cout << ++*b.i;
}
```
Output:
```
true12
```
As you can see, a.i and b.i are pointers to the same memory region, so when you modify the memory region through a.i, the memory pointed to by b.i is also modified. 

Of course you can also delete it with <code>C(const C&) = delete;</code>. This will make the class non-copy-constructible. 

Your copy and move constructors may be elided by the compiler. The C++17 standard specifically allows compilers to elide copy and move constructor calls even if they are side-effectful in certain situations, meaning the copy elision would change the semantics of the program. That is why you should not have side effects in your copy and move constructors unless you know exactly what you're doing. In general, copy elision won't happen if the thing being copied from needs to continue to exist after the copy, e.g:

```cpp
T a;
T b=a; //no copy elision because a needs to exist afterwards, so the copy constructor is definitely called
```

From the C++17 standard section 15.8.3 Copy/move elision:

>When certain criteria are met, an implementation is allowed to omit the copy/move construction of a class object, even if the constructor selected for the copy/move operation and/or the destructor for the object have side effects. In such cases, the implementation treats the source and target of the omitted copy/move operation as simply two different ways of referring to the same object. If the first parameter of the selected constructor is an rvalue reference to the object’s type, the destruction of that object occurs when the target would have been destroyed; otherwise, the destruction occurs at the later of the times when the two objects would have been destroyed without the optimization.

### Derived class copy constructor

If you write a copy constructor for a child class then you have to manually call its components and parents' copy constructors as well:

```cpp
#include <iostream>
struct S{
  S() { std::cout << "default ctor called"; }
  S(const S&) { std::cout << "copy ctor called"; }
};
struct Child : public S {
  S s1, s2;
  Child(){}
  Child(const Child& c) : S(c), s1{c.s1}, s2{c.s2} {}
};
int main(){
  Child a;
  Child b{a};
}
```

Amazingly, you can pass the child class object directly to the parent copy constructor. It just implicitly upcasts it to a const reference to an object of the parent class. 

Now, let's look at something interesting:

```cpp
#include <iostream>
struct S{
  S() { std::cout << "default ctor called"; }
  S(const S&) { std::cout << "copy ctor called"; }
};
struct Child : public S {
  Child(){}
  Child(const Child& c) {}
};
struct D : public S {};
int main(){
  Child a; 
  std::cout << "\n";
  Child b{a};
  std::cout << "\n";
  D c;
  std::cout << "\n";
  D d{c};
}
```
Output:
```                    
default ctor called
default ctor called
default ctor called
copy ctor called
```

Interestingly, in the case of the Child class only the default constructor for the base class constructor is called. This is because, if you write your own copy constructor, but don't specify a base class constructor, **then the base class default constructor is called rather than the base class copy constructor. On the other hand, the implicitly generated copy constructor calls the base class copy constructor instead of the base class default constructor**. It's very important to remember this: **When you define your own copy constructor, you need to explicitly invoke the base class copy constructor in its member initializer list.** The fun never stops. 


### Why does the copy constructor take a reference

Because pass by value (and return by value) invokes the copy constructor. 

The copy constructor is pass-by-reference because if it was pass-by-value then it would call itself. Observe:

```cpp
struct C{
  C(){}
  C(C){} 
};
int main(){
  C c;
  C d(c);
}
```
Here, to construct d, we would need to call the copy constructor. However, since the copy constructor takes a value, then it means a copy of c has to be made. This means calling itself with the same argument - infinite recursion. 


## Destructors

A destructor is supposed to clean up any resources. When an exception is thrown, the "stack is unwinded", which means destructors may be called to destroy objects. Since C++11, destructors are implicitly noexcept [unless a base or member is potentially throwing](http://en.cppreference.com/w/cpp/language/destructor), so you have to explicitly declare them `noexcept(false)` to throw. Throwing from a destructor is considered bad practice (see More Effective C++ item 11). 

### Why you should always have a virtual destructor in all your classes

Because `delete`ing a derived object from a base pointer is undefined behavior if the base class does not have a virtual destructor. [Example](https://wiki.sei.cmu.edu/confluence/display/cplusplus/OOP52-CPP.+Do+not+delete+a+polymorphic+object+without+a+virtual+destructor):

```cpp
#include <memory>
struct Base {
  virtual void f();
};
struct Derived : Base {};
void f() {
  std::unique_ptr<Base> b = std::make_unique<Derived()>();
}
```

Thus the GOTW guideline: [A base class destructor should be either public and virtual, or protected and nonvirtual.](http://www.gotw.ca/publications/mill18.htm).


## Copy initialization vs direct initialization

It is important to note the difference between direct and copy initialization. 

```cpp
string a;
string b(a); //direct initialization
string b=a;  //copy initialization
```

These are different. In this case they both call the copy constructor but there are other cases where they have different behavior. For example explicit constructors are not considered for copy initialization, only for direct initialization. You use the `explicit` specifier on a constructor if you don't want it to be used in implicit conversions e.g:

```cpp
struct S{
  S(int){}
};
struct T{
  explicit T(int){}
};
void f1(S){}
void f2(T){}
int main(){
  f1(1);
  f2(1);
}
```
Output:
```
error: no matching function for call to 'f2'
```

The `explicit` specifier prevents a constructor from being used in implicit conversions. According to the [C++ Core Guidelines](https://github.com/isocpp/CppCoreGuidelines/blob/master/CppCoreGuidelines.md) you should always mark your one-argument constructors (that are not move or copy constructors) explicit unless they are specifically for implicit conversions (e.g `const char*` to std::string). 

Implicit conversions are defined in terms of copy-initialization: if T can be copy-initialized with expression E then E is implicitly convertible to T. Copy initialization happens whenever you pass an object by value or return an object by value. 

Assignment operator overloads have no effect on copy initialization, e.g:

```cpp
T a=b;
```

This always calls the copy constructor and not the assignment operator. 

## Friends

A class may have friends. A friend can be a class or a function. 

The only friends of a class are those that it declares are its friends. Neither its parents nor children's friends are its friends. 

Only friends can touch a class's privates, neither its parents nor children can. 


# Operator overloading

Every shitty noob C++ tutorial has to have a chapter on operator overloading so this one must have it too. It only makes sense. 

## Overloadable operators

You cannot define new operators and you cannot change the precedence or associativity of existing operators (so overloading `^` as the exponentiation operator is probably a bad idea given that it has lower precedence than all the arithmetic operators). Furthermore, only some existing operators can be overloaded, and of those only some can be overloaded as non-member functions. And you can only overload operators for user-defined types (not for built-ins). 

Note that all the operators are independent so overloading + doesn't affect = nor does it affect += and so on. 

If you define the same operator as both member and nonmember functions and it can match either then you get error: ambiguous overload. 

Only these operators cannot be overloaded:

1. ::
2. .
3. .*
4. ?:
5. sizeof
6. typeid

Only these operators must be member functions:

1. =
2. ()
3. []
4. -> 

I personally don't see the value in memorizing these facts (what might be considered trivia)...

Whilst the assignment operator `=` must be overloaded as a member function, all the other assignment operators (`+=`, `/=` etc) can be overloaded as nonmember functions. The fun never stops. 

The -> operator is special because it's the only operator that has restrictions on what its return type can be, and also because it implements "drill-down" behavior. If your overloaded -> function doesn't return a pointer, then it takes the object returned by your function and calls its -> function, and so on until a pointer to an object is returned, then that object is used to perform member lookup:

```cpp
#include <iostream>
struct A{
 int i = 42;
};
struct B{
 A a;
 A* operator->(){
   return &a;
 }
};
struct C{
 B b;
 B operator->(){
   return b;
 }
};
int main(){
 C c;
 std::cout << c->i;
}
```
Output:
```
42
```

So in the example above, when `b` returns a pointer to `a`, `a` is used to perform member lookup, so in this case we look for the member `i` in `a`. 

All other operators can be overloaded both as member and as non-member functions. Where @ is an operator:

```
+--------+-----------------+---------------------+
|  Type  | Member function | Non-member function |
+--------+-----------------+---------------------+
| Prefix | operator@()     | operator@(T)        |
| Postix | operator@(int)  | operator@(T,int)    |
| Infix  | operator@(T)    | operator@(T,T)      |
+--------+-----------------+---------------------+
```

The exception is the () operator, which is the only operator that can take an arbitrary number of parameters. When you overload the () operator of a function, it becomes a `FunctionObject`. These are also called "functors", or function objects. 

You might be thinking, "why do you have to have an int parameter when overloading a postfix operator even though the int is usually not used", the answer is that this is how C++ distinguishes postfix from prefix operators. 

When you overload the && and || operators they unfortunately lose short-circuit evaluation. C++17 guarantees the order of evaluation is left-to-right but the shortcircuit evaluation is lost. You have to use bools with these operators to get the shortcircuit behavior. 

Conversion operators such as bool(), as well as `new` and `delete`, as well as the address-of and dereference operators can also be overloaded for hilarious effects. You should definitely overload these operators for maximum trolling effects. 

## Design of overloaded operators

GOTW #4 tells us to not use pass by value when we don't need a copy. Example from Sutter:

```cpp
void operator+ ( complex other ) {
    real = real + other.real;
    imag = imag + other.imag;
}
```
Sutter tells us that it is better to use a const&, like this:
```cpp
void operator+ ( const complex& other ) {
    real = real + other.real;
    imag = imag + other.imag;
}
``` 

## How to invoke overloaded operators recursively

It is in fact possible to invoke overloaded operators recursively:

```cpp
class T {};
T operator+(T x, int y){
  return T(x+y); 
}
int main(){
  T a; a+1;
}
```

Compiling with clang with `-Weverything` gives this warning:

```
warning: all paths through this function will call itself [-Winfinite-recursion]
```

Otherwise it compiles fine. When you run it you get a seg fault. 

# Exceptions

C++ does not have compile-time exception checking. This means that if you throw an exception in a function declared noexcept(true), the compiler will not tell you about it, the program will just call std::terminate at runtime when the exception is thrown. In other words, there is no compile-time guarantee that a noexcept(true) function won't throw, only a run-time guarantee to "beat your program senseless" (in the words of Sutter) if a noexcept(true) function throws. 

Which is not very useful. Thus, exception specifications were removed in c++17. The fun never stops. 

See also [Sutter's article](http://www.gotw.ca/publications/mill22.htm):

>Besides the overhead for generating the try/catch blocks shown above, which might be minor on efficient compilers, there are at least two other ways that exception specifications can commonly cost you in runtime performance. First, some compilers will automatically refuse to inline a function having an exception specification, just as they can apply other heuristics such as refusing to inline functions that have more than a certain number of nested statements or that contain any kind of loop construct. Second, some compilers don’t optimize exception-related knowledge well at all, and will add the above-shown try/catch blocks even when the function body provably can’t throw.
>
>Moving beyond runtime performance, exception specifications can cost you programmer time because they increase coupling. For example, removing a type from the base class virtual function’s exception specification is a quick and easy way to break lots of derived classes in one swell foop (if you’re looking for a way). Try it on a Friday afternoon checkin, and start a pool to guess the number of angry emails that will be waiting for you in your inbox on Monday morning.
>
>So here’s what seems to be the best advice we as a community have learned as of today:
>
>Moral #1: Never write an exception specification.
>
>Moral #2: Except possibly an empty one, but if I were you I’d avoid even that.
>
>Boost’s experience is that a throws-nothing specification on a non-inline function is the only place where an exception specification “may have some benefit with some compilers” [emphasis mine]. That’s a rather underwhelming statement in its own right, but a useful consideration if you have to write portable code that will be used on more than one compiler platform.

To be continued in Part 2.