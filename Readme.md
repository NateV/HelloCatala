# Hello Catala

Getting started with the Catala system for programming statutes

## Motivation

At the risk of trivializing a challenge that's fundamental to human society...law is _confusing_. Perhaps we invented law to offer some predictability and transparency to individuals asking themselves, "What will happen to me if I do X?" and "Is that fair?" We have worked on the problem for quite a few years, but I think its fair to say that humanity hasn't completely solved the problem. In the United States, "the law" is an ever-shifting mass of procedures, statutes, precedents, traditions, and more. Even experts (or perhaps especially experts) often disagree about the meanings of legal rules and how to apply legal rules to factual situations.

The field of _Computational Law_ proposes that at least some parts of the legal system might be encoded in software. A computer might be a useful tool for determining how certain rules apply to certain facts. Where a human might get confused, or just bored, and make mistakes when following complicated rules of logic to a conclusion, a computer might excel. Software-defined legal rules could also make it possible to thoroughly test legal rules. We could even formally, mathematically prove that they have the expected results under all the factual situations that might arise. Wouldn't that be neat?

There are quite a few challenges to defining legal rules in software. Some of these challenges are fundamental. Will code ever be able to judge what a person's "intent" was at some point in time, or whether a particular word in a rule should mean one thing or another? Some kinds of legal decisionmaking may be, by definition, impossible for anything other than a human being to do. But suppose we accept that there is some set of legal rules that are sufficiently well-defined that applying the rules can be an excercise of mechanical logic (if this, then that, otherwise something else). Even then, there's another enormous challenge: there are not very many legal experts who write or read computer code. The people who can understand what a statute means are not able to write software describing it, and the people who can write software lack the expertise to fully grasp the nuances of statutury texts.

I recently came across an interesting new approach to encoding law in software called [Catala](https://catala-lang.org/). Catala tries to address the second problem I mention above, relating to the respective limitations of lawyers and programmers. It employs three powerful techniques to bridge the gap between lawyers and programmers.

First, Catala uses "literate programming". This is a style of programming in which machine-readable code is written alongside human-readable text. A literate program is not just well-commented code. Literate programming takes the perspective that the program is "primarily a document directed at humans", and the code for computers nestles itself inside the human-focused text. [literateprogramming.com](http://www.literateprogramming.com/) In Catala's literate programming style, the "program" is the actual text of a statute, with blocks of code inserted as close as possible to the lines of statutory text that the code reflects.

Catala's second "killer feature" is its model of statutory logic. Classical logic requires all the relevant facts to be known before a conclusion can be reached. Various authors have argued that legal reasoning is often better modeled with "defeasible logic", logic in which you can reach a conclusion, then accept new information, and modify the conclusion you've reached. Catala's code employs "default logic", a form of defeasible logic in which there is a default conclusion that can be altered by subsequent information. Catala's creators argue that this form of logic matches the most common form of statutory structure in which a statute presents a default rule ("income tax is 15% of income") followed by more detailed adjustments of the rule ("if income is greater than $1 million, then tax is 25%"). This form of logic allows Catala programs to declare a default rule in one code block alongside the statute's general rule, and then to alter the default rule elsewhere, following statutory text that adds exceptions and adjustments.

With literate programming and default logic, Catala enables us to write programs in which the actual statutory text is very tightly coupled with code that reflects each little bit of statutory text.

The final feature of Catala that I wish to highlight is the flexibility of its compiler. In a Catala program, we can write code that tests statutory rules under different conditions (what happens if income is 0? what happens if the number of children is greater than the person's income?). The program is also a human-readable text. The compiler allows us to take advantage of both of these qualities of the program. We can compile the program to a display format such as HTML or PDF (at least, to Latex, which can become a PDF). We can also compile just the code to other programming languages (currently only ocaml). And we can run tests we've written to evaluate whether the code accurately reflects what the statute says and to evaluate if the statue has unintended effects or gaps.

Catala is a young project, and still a work in progress, but it implements several very powerful ideas. **Lets try it out!**

## Set up the Environment

Installing the software you need to run Catala is fairly straightforward. There are either some rough edges, or its assumed that users are more familiar with the Ocaml programming environment than I am.

Catala requires opam, the package manager for the language Ocaml to be installed. So head over to [ocaml.org](https://ocaml.org) to get opam installed.

Next, install Catala, following the instructions on [github.com/CatalaLang/catala](https://github.com/CatalaLang/catala).

I have never used Ocaml or Opam, so I struggled with this a little. I wasn't able to install Catala directly with Opam (my installation of opam could not find a Catala package). I installed Opam from Debian's official repositories, and then ended up building Catala from source. My suspicion is that the Debian version of opam is older than the verison of opam you would need to install Catala.

## Create your Catala Program

Let's create a very simple Catala program called "hello". This is effectively a classic "Hello World!" program.

**You should think of the whole document as the "program".** Just like any other programming language, Catala enforces strict rules in the syntax of the "code" sections. Unlike other "literate programming" systems, it also enforces rules about the syntax of the non-code sections. For example, "/" and "'" characters are not allowed in the text sections. This means you can't include [hyperlinks](https://en.wikipedia.org/wiki/Hyperlink) or markdown-style `code highlighting`. Also, code blocks are only allowed under `### [Article n]` headings. The whole document, including the sections that are just text, is your program, and has to follow Catala's rules.

Go ahead and create a directory for our project in an empty text file. The text file can be called "hello.catala_en". The extension indicates this is a "catala"
file written in English.

```
$ mkdir hello_catala
$ touch hello_catala/hello.catala_en
```

## Basic Structure of a Catala Program

A Catala program can start with lawyer-friendly text, perhaps introducing the statute. The rest of the text is one or more "Articles". An "Article" is introduced
with hashes indicating a "header" followed by square brackets, as in: `### [Article 1]`

An Article can contain human-readable text as well as code blocks. Human-readable text is written similarly to Markdown, with headers written with `### Header`.

Code blocks begin with three backticks and the word "catala", and close with three backticks (just like Markdown). Comments in code blocks start with hash-marks.

````
And now for some code:

```catala
# This is a comment.
```

That was the code.
````

At this point, you should tell the compiler to "knit" the file into a display format such as HTML. This way the compiler will verify the syntax even though there's no "code" yet. (Remember, the whole text, including the parts just for humans to read, is the program, and has to follow Catala's syntax rules.)

To run the program through the compiler, run

```
catala --language=en Html hello.catala_en
```

In this command, the `language` flag tells the compiler what human language you're using (syntax differs for different supported languages). The `Html` value tells
the compiler to use the `HTML` backend and render the input file into HTML. Latex is another backend for generating a nicely laid out file from your program.

At this point, if there are no errors, you will have a new `hello.html` file in your directory that shows what you've written so far.

## Add an Article and some code

Let's write some code. Add an Article to your program by adding a heading and a square-bracket-enclosed title for the article, such as `[Article 1]`.

````

### [Article 1]

An article might start with regular text, like this. Our first code block will come next.

```catala
declaration scope Say100:
    context a_number content integer

scope Say100:
    definition a_number equals 100
```

````

The line "declaration scope Say100" creates a "scope" called "Say100". A scope is something like a function in other programming languages. Its an area bounding
data and rules that apply to the data in that scope.

"context a_number content integer" says that our Say100 scope includes a piece of data called "a_number". "a_number" will hold an integer, when we use this
scope later.

The lines "scope Say100" and "definition a_number equals 100" defines the data "a_number" inside the Say100 scope. These lines set the value of the Say100 scope's "a_number" to the integer 100.

Now we have a complete Catala program. You can compile to html again, with "catala --language=en Html hello.catala_en".

Or you can tell Catala to figure out the value of a scope the program defines. Run

```
$ catala -s Say100 --language=en Interpret hello.catala_en

```

You should see messages that "a_number = 100".

In this modified command, note that "Html" has been replaced with "Interpret". This means we want the compiler to actually run the code we've written.
And we need to tell the compiler which scope we want it to try to execute. In this simple example, there is only one scope.

## A More Complicated Example

Our `hello` program demonstrates just about the simplest Catala program there can be.

The `CarRentalTax.catala_en` example in this repository is one step of complexity higher. It demonstrates a few more pieces of the sytax of the Catala language. In that example, you can see that one area where Catala shines is the ability to define the parts of the thing you're trying to evaluate, like a tax, in different places in the text, unified by the declaration of single "scope". The compiler is able to unify parts of scope that are declared in different places, even if the scope is initially defined _after_ somewhere in the text it is used. This way, Catala lets us write the code right next to statutory language, even though statutory language is constantly backtracking or referring to provisions that come later.

## I like ...

There are some things that I like a lot about Catala.

The process of writing code to reflect the statute forces you to think really carefully about each piece of the statute. Granted, one can think carefully about a statute the old-fashioned way too, but help is still useful.

The literate style is very compelling. Based on my experience writing code evaluating criminal records for expungements, I understand how useful it is to have the authoritative statutory text in the same place as code describing it. I'm a little skeptical that many lawyers will be willing to understand the code themselves, but even for communications between non-programmer lawyers and programmers, having the text and law in the same place is very valuable.

## I Wish ...

There are also a few areas where I hope to see Catala evolve.

I'm still struggling with understanding how to use the syntax, as its very different from anything I've used before. Even after reading the paper introducing the default logic that inspired Catala, I still don't really understand the basis for some of the language constructs. Like what _is_ a scope, really? It's _like_ a function, but its not one, exactly, it seems. I think. This is an area where I may just not have put in the work yet, though, and as a new project, there aren't millions of blog posts and books about Catala's design.

One of the areas where encoding law seems to have a lot of potential is in testing. Already in Catala we define the parameters that a law acts on. (like a person's income and family size). I'd like to be able to automatically generate test cases that fully explore the possible domain of inputes. These concepts of "fuzzing" or "property-based" testing are pretty widely used in other programming languages, so it would be great to use them in Catala. (I suppose one could use Catala to compile to ocaml, and then use ocaml tools to generate test cases for the generated code, but that's pretty indirect. Could it be possible to use a scope to call a collection of other scopes?)

It would also be useful for Catala's interpreter to be better able to explain its answers. _Why_ was a person's income tax $400? _Why not_ $0, or _what would need to happen_ to get to $0? This requirement of 'explainability' is really difficult, but also really important. Somehow a person using this system needs access to not just a result, but a blob of information that records how the result was achieved and enables identifying paths-not-taken in the chain of decision-making. A lawyer's response to a question about the applicability of a rule will always include this kind of why- and what-if interaction. Law-as-code needs to be able to offer that transpency and some mechanism for interactivity as well.

## Next ...

It will be very interesting to try out writing a Catala program with a non-programmer lawyer, perhaps to evaluate some local ordinance. I'm eager to see what it's like to introduce literate programming and Catala's logic to others.
