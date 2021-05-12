# Hello Catala

Getting started with the Catala system for programming statutes

## Motivation

At the risk of trivializing a concept that's fundamental to modern society...law is confusing. In the United States, "the law" is an ever-shifting mass of
procedures, statutes, precedents, traditions, and more. Even experts (or perhaps especially experts) often disagree about the meanings of legal rules and how to
apply legal rules to factual situations.

The field of _Computational Law_ proposes that at least some parts of the legal system might be encoded in software. A computer might be a useful tool for determining how certain rules apply to certain facts. Where a human might get confused, or just bored, and make mistakes when following complicated rules of logic to a conclusion, a computer might excel. Software-defined legal rules could also make it possible to thoroughly test legal rules and even formally, mathematically prove that they have the expected results under all the factual situations that might arise.

There are quite a few challenges to defining legal rules in software. Some of these challenges are fundamental - will code ever be able to judge what a person's "intent" was at some point in time, or whether a particular word in a rule should mean one thing or another? But suppose we accept that there is some set of legal rules that are sufficiently well-defined that applying the rules can be an excercise of mechanical logic. Even then, there's another enormous challenge: there are not very many legal experts who write or read computer code. The people who can understand what a statute means are not able to write software describing it, and the people who can write software lack the expertise to fully grasp the nuances of statutury texts.

I recently came across a very interesting new approach to encoding law in software called [Catala](https://catala-lang.org/). Catala tries to address the second problem I mention above, relating to the respective limitations of lawyers and programmers. It employs two powerful techniques to bridge the gap between lawyers and programmers. First, Catala uses "literate programming". This is a style of programming in which

Catala exists to enable programmers and lawyers to work together on the same texts, to test and verify legal statutes.
It provides set of tools for annotating statutory text with code that describes the statute. Catala's compiler can then use your annotated text to

1. Test if the statute and code have the expected results.

2. Create nicely formatted versions of your annotated statute in formats such as html and latex.

3. Transpile your Catala code into code in another language. (Currently only ocaml)

This tutorial will help you get started with Catala by writing a very simple annotated text.

## Set up the Environment

Catala requires opam, the package manager for the language Ocaml to be installed. So head over to [ocaml.org](https://ocaml.org) to get opam installed.

Next, install Catala, following the instructions on [github.com/CatalaLang/catala](https://github.com/CatalaLang/catala).

## Create your Catala Program

We are going to create a very simple Catala program. This will be a single file.
This file will contain programmer-friendly "code" interspersed with lawyer-friendly statutory text.

**You should think of the whole document as the "program".** Just like any other programming language, Catala enforces strict rules in the syntax of the "code" sections. Unlike other "literate programming" systems, it also enforces rules about the syntax of the non-code sections. For example, "/" and "'" characters are not allowed in the text sections. No [hyperlinks](https://en.wikipedia.org/wiki/Hyperlink) or markdown-style `code highlighting`. Also, code blocks are only allowed under `### [Article n]` headings. The whole document, including the sections that are just text, is your program, and has to follow Catala's rules.

Go ahead and create a directory for our project an empty text file. The text file can be called "hello.catala_en". The extension indicates this is a "catala"
file written in English.

## Basic Structure of a Catala Program

A Catala program can start with lawyer-friendly text, perhaps introducing the statute. The rest of the text is one or more "Articles". An "Article" is introduced
with hashes indicating a "header" followed by square brackets, like [Article 1]

An Article can contain human-readable text as well as code blocks. All the text so far in this document would be valid in human-readable text sections.
Text may contain headings, which are lines with leading hash marks. More hash marks demote a heading, just like Markdown.

Code blocks begin with three backticks and the word "catala", and close with three backticks (just like Markdown).

Text blocks cannot contain certain characters like slashes or backticks. So even before there's any code in your program, you can periodically send it to the
compiler to make sure your code works. At this point, you should tell the compiler to "knit" the file into a display format such as HTML.
This way the compiler will verify the syntax even though there's no "code" yet.

To run the program through the compiler, run "catala --language=en Html hello.catala_en"

In this command, the "language" flag tells the compiler what human language you're using (syntax differs for different supported languages). The "Html" value tells
the compiler to use the "HTML" backend, and render the input file into HTML. Latex is another backend for generating a nicely laid out file from your program.

At this point, if there are no errors, you will have a new ".html" file in your directory that shows what you've written so far.

## Add an Article and some code

Let's write some code. Add an Article to your program by adding a heading and a square-bracket-enclosed title for the article, such as [Article 1].

```
### [Article 1]

An article might start with regular text, like this. Our first code block will come next.

\`\`\`catala
declaration scope Say100:
    context a_number content integer

scope Say100:
    definition a_number equals 100
\`\`\`


```
