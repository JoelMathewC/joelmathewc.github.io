---
title: "APL Compiler Part 1: Parser"
description: "Leveraging Flex/Bison to parse "
date: 2025-12-15
weight: 2
series: ["LLVM"]
series_order: 2
---

APL is a programming language introduced in Kenneth Iverson's turing award winning paper titled "Notation as a tool of thought". I came across this paper in a compiler class I took during graduate school. As a part of the class we had to build a APL2C compiler which generated C kernels for various APL expressions. While working on the project I started to align myself with the corner of the programming commuity that rallied behind APL as a reasonable programming lanaguage :). As such it must be noted that there are a ton of open-source APL compilers out there, however the most widely used APL compiler by Dyalog is not open-source but free. 

If you're interested in building an APL compiler, I would recommend [this resource](https://xpqz.github.io/learnapl/intro.html) since it discusses the details of the language in great detail. However, if you're just here for the LLVM bits some of my not so complete explanation should get you by. Throughout this tutorial we'll build the APL compiler to match the functionality of APL as indicated in the above resource. We'll build the compiler in C++.

## Parsing

Parsing is the process of converting a string of characters that form a program into a some logical representation we can analyse. The logical representation in this case is an abstract syntax tree but we'll get to that in the next section. When it comes to parsing we have a bunch of options. I wrote my first compiler using Lex/YACC parser generator but Flex/Bison are a little more popular over those variants since they are free and open-source. I will note that whether or not you use a parser generator is upto you, many folks prefer to write their own parser for the languages they build, but for this tutorial I will use Flex/Biso since it works better for me. You may find this introduction to bison useful - [ref](https://web.stanford.edu/class/archive/cs/cs143/cs143.1128/handouts/120%20Introducing%20bison.pdf) and [ref](https://web.stanford.edu/class/archive/cs/cs143/cs143.1128/handouts/050%20Flex%20In%20A%20Nutshell.pdf)

If you do decide to work with Flex/Bison, use the package manager on your machine to install the two tools. I use an Intel Mac and homebrew as my package manager, so I installed it from there.

### Flex
