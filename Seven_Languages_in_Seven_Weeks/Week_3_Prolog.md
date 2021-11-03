# Week 3 - Prolog
- Prolog is an unusual but very powerful language.
- Best movie analogy is Raymond, the autistic savant from the movie *Rain Man*. Prolog amazes you with raw deductive power, but you need to know how to efficiently communicate with it.
- Io and Ruby were both imperative languages, but Prolog is a declarative language.
- Instead of telling Prolog what to do, you tell it what you know and what you want, then you let it figure out the solution for you.
- This makes Prolog extremely powerful for problems that are well-defined, but are difficult to solve.
- (Personal Reflection)-In some ways, Prolog is both a top-down and bottom-up AI language simultaneously. Just like bottom-up AI systems, Prolog provides very little insight into how it arrives at its solutions for you. However, it still does so using classic logic rules and constraint solving, so it's not as unpredictable as, say, a neural network.
- Prolog was one of the first successful logic programming languages. This seems to be a pretty niche area of programming, as I don't recognize any of its successors, but they seem to revolve around constraint processing, theorem proving, etc.

## 4.1 About Prolog
- Invented in 1971 by Alain Colmerauer and Phillipe Roussel.
- It was originally developed for Natural-Language Processing (NLP), but is now also used for scheduling and expert systems.
- Many similarities to SQL, as both work on databases. Both also provide a syntax for expressing data, and another syntax for querying that data.
- Prolog building blocks:
  - **Facts** - A basic assertion (e.g., Socrates is a man.)
  - **Rules** - An inference about facts (e.g., All men are mortal.)
  - **Queries** - A question about the world (e.g., Is Socrates mortal?)
- One of the quirks about Prolog is that the facts and rules go into a **knowledge base** which has to be compiled before performing any queries. This threw me off the first time I tried to use the prolog interpreter to define facts and rules instead of loading and compiling them from a file.

## 4.2 Day 1: An Excellent Driver
- Goal for day 1 is to just learn the basics of facts, rules, and queries without necessarily understanding the big picture.
- The book uses GNU Prolog, version 1.3.1 for its examples. I used the latest version on Debian, version 1.4.5.
- I think the most important factor here is that we're using GNU Prolog, as Prolog has evolved into an entire family of programming languages at this point, with major differences in behavior and syntax between them.

### Basic Facts
- Capitalization is very important in Prolog.
  - **Atoms** - Begin with lowercase letters; fixed meaning similar to Ruby symbols.
  - **Variables** - Begin with uppercase letters; their value can change.
- Basic example from `prolog\friends.pl`,
```
likes(wallace, cheese).
likes(grommit, cheese).
likes(wendolene, sheep).
friends(X, Y) :- \+(X = Y), likes(X, Z), likes(Y, Z).
```
- This examples uses a number of atoms (`wallace`, `grommit`, `cheese`, etc) and two variables (`X` and `Y`). The facts are in the form `likes(wallace, cheese).`

### Basic Inferences and Variables
- The rule is a little more complicated, consisting of three subgoals:
  - `\+ (X = Y)` - `\+` is logical negation, so we're saying this rule only applies when X != Y. In other words, something can't be its own friend.
  - `likes(X, Z)` - There is some variable `Z` that `X` likes.
  - `likes(Y, Z)` - Which `Y` also likes.
- You can load this file by running `gprolog` from your OS command-line, and then doing the following:
```
$ gprolog           
GNU Prolog 1.4.5 (64 bits)
Compiled Feb 23 2020, 20:14:50 with gcc
By Daniel Diaz
Copyright (C) 1999-2020 Daniel Diaz
| ?- ['friends']
.
compiling /home/kali/Downloads/friends.pl for byte code...
/home/kali/Downloads/friends.pl compiled, 4 lines read - 939 bytes written, 7 ms

(1 ms) yes
| ?- likes(wallace, sheep).

no
| ?- likes(wallace, cheese).

yes
| ?- friends(wallace, grommit).

(1 ms) yes
| ?- friends(wallace, wendolene).

no
```
- a

### Filling in the Blanks
- a

#### Map Coloring
- a

#### Where's the Program?
- a

### Unification, Part 1
- a

### Prolog in Practice
- a

### Day 1 Self-Study
#### Find:
- Question:
Answer:
- Question:
Answer:
- Question:
Answer:

#### Do:
- Question:
Answer:
- Question:
Answer:
- Question:
Answer:
- Question:
Answer:

## Day 2: Fifteen minutes to Wapner
- a

### Recursion
