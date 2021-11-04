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
- So, when we query `friends(wallace, grommit).`, Prolog checks the first sub-goal, making sure that `wallace` is not `grommit`, which it isn't. Then it tries to find a value of `Z` that will simultaneously solve both of the remaining sub-goals (i.e., something that both `wallace` and `grommit` like.)
- In Prolog, rules are referred to by the number of parameters they take, so we can refer to this as `friends/2`. This is particularly useful when the same rule has multiple arities, often with the forms with fewer parameters representing the base case.

### Filling in the Blanks
- We can query Prolog with more than just yes or no questions. For example, let's ask which variable value satisfies our requirements (example `prolog/food.pl`):
```
food_type(velveeta, cheese).
food_type(ritz, cracker).
food_type(spam, meat).
food_type(sausage, meat).
food_type(jolt, soda).
food_type(twinkie, dessert).

flavor(sweet, dessert).
flavor(savory, meat).
flavor(savory, cheese).
flavor(sweet, soda).

food_flavor(X, Y) :- food_type(X, Z), flavor(Y, Z).
```
- We can query which foods are `savory` with, `flavor(savory, What).`. Prolog recognizes `What` as a variable by its capitalization, and allows us to find one or all solutions:
```
| ?- flavor(savory, What).

What = meat ? a

What = cheese

yes
```

#### Map Coloring
- This is first example that shows the power of Prolog. Map coloring is a classic mathematical problem: how do you render a map with the minimum number of colors, without adjacent territories having the same color?
- Instead of coming up with an algorithm for this, we can simply tell Prolog that three colors are different from each other. Then we can tell it which states need to have different colors from each other (example `prolog/map.pl`):
```
different(red, green). different(red, blue).
different(green, red). different(green, blue).
different(blue, red). different(blue, green).

coloring(Alabama, Mississippi, Georgia, Tennessee, Florida) :-
  different(Mississippi, Tennessee),
  different(Mississippi, Alabama),
  different(Alabama, Tennessee),
  different(Alabama, Mississippi),
  different(Alabama, Georgia),
  different(Alabama, Florida),
  different(Georgia, Florida),
  different(Georgia, Tennessee).
```
- Within a few seconds, Prolog compiles this knowledge base and presents the six valid solutions out of 243 possible combinations:
```
| ?- ['map'].
compiling /home/kali/Downloads/code/prolog/map.pl for byte code...
/home/kali/Downloads/code/prolog/map.pl compiled, 22 lines read - 1731 bytes written, 7 ms

(1 ms) yes
| ?- coloring(Alabama, Mississippi, Georgia, Tennessee, Florida).

Alabama = blue
Florida = green
Georgia = red
Mississippi = red
Tennessee = green ? a

Alabama = green
Florida = blue
Georgia = red
Mississippi = red
Tennessee = blue

Alabama = blue
Florida = red
Georgia = green
Mississippi = green
Tennessee = red

Alabama = red
Florida = blue
Georgia = green
Mississippi = green
Tennessee = blue

Alabama = green
Florida = red
Georgia = blue
Mississippi = blue
Tennessee = red

Alabama = red
Florida = green
Georgia = blue
Mississippi = blue
Tennessee = green

(1 ms) no
```

#### Where's the Program?
- Unlike in a procedural language, we didn't tell Prolog an algorithm. We didn't define any loops or branching logic. We simply laid out a few, obvious constraints and let Prolog do the work for us.

### Unification, Part 1
- `=` in Prolog means **unification** rather than assignment. Unification is the process of making two structures identical.
- In other words, `=` is more like the equals sign in mathematics than in most programming languages. We're telling Prolog to solve for the variables that balance out both sides of the equals sign.

### Prolog in Practice
- Book here gives an interview with someone who used Prolog for scheduling, as well as modeling the knowledge base a dolphin was trained on.
- The example is pretty cool, as the dolphin developed a behavior that confused the trainers, but querying Prolog revealed the dolphin was exploiting a loophole based on a command the trainers had forgotten.

### Day 1 Self-Study
#### Find:
- Question: Some free Prolog tutorials.
Answer:
  - http://www.cs.nuim.ie/~jpower/Courses/Previous/PROLOG/
  - https://www.usna.edu/Users/cs/roche/courses/f18si413/proj/prolog.php
  - http://jmvanel.free.fr/ai/prolog-getting-started.html
- Question: A support forum (there are several.)
Answer: https://www.reddit.com/r/prolog/
- Question: One online reference for the Prolog version you're using.
Answer: http://www.gprolog.org/manual/html_node/index.html

#### Do:
- Question: Make a simple knowledge base. Represent some of your favorite books and authors.
Answer:
```
wrote(frank_herbert, dune).
wrote(jrr_tolkien, lotr).
wrote(jrr_tolkien, hobbit).
wrote(jrr_tolkien, silmarillion).
wrote(william_gibson, neuromancer).
```
- Question: Find all books in your knowledge base written by one author.
Answer:
```
| ?- ['books'].
compiling /home/kali/books.pl for byte code...
/home/kali/books.pl compiled, 5 lines read - 703 bytes written, 5 ms

(1 ms) yes
| ?- wrote(jrr_tolkien, What).

What = lotr ? a

What = hobbit

What = silmarillion

yes
```
- Question: Make a knowledge base representing musicians and instruments. Also represent musicians and their genre of music.
Answer:
```
played(taylor_swift, guitar).
played(taylor_swift, piano).
played(tom_morello, guitar).
played(yo-yo_ma, cello).
genre(taylor_swift, country).
genre(taylor_swift, pop).
genre(tom_morello, rock).
genre(tom_morello, rap_metal).
genre(tom_morello, alternative_metal).
genre(yo-yo_ma, classical).
genre(yo-yo_ma, pop).
```
- Question: Find all musicians who play the guitar.
Answer:
```
| ?- ['music'].
compiling /home/kali/music.pl for byte code...
/home/kali/music.pl compiled, 11 lines read - 1521 bytes written, 7 ms

(1 ms) yes
| ?- played(Who, guitar).

Who = taylor_swift ? a

Who = tom_morello

no
```

## Day 2: Fifteen minutes to Wapner
- a

### Recursion
