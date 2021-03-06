# Week 2 - Io
## Introducing Io
- Best movie metaphor for Io is Ferris Bueller from Ferris Bueller's Day Off. Very smart, easy to understand, but unpredictable.
- Invented by Steven Dekorte in 2002.
- Active development stopped in 2008, but the binaries and build system are still actively maintained as of October 2021 (when I'm reading the book.)
- Io was created as a toy language, meant to help its author understand the mechanics of interpreters. It's still a very small community, although there are some production uses of it.
- Current Io community focus is embeddable system, as it's a very small language but it has modern features like actor-based concurrency.
## Day 1: Skipping School, Hanging Out
- Io syntax is dead simple. Every *thing* is both an object and a message, and Io code consists of messages being passed between objects.
- There are no classes in Io, just objects.
- Io is a **prototype language**, so it has inheritance, but between objects instead of classes.
- Every object is a clone of another object, going back to the root `Object`.
### Breaking the ice
- Io has a REPL, although it doesn't provide auto-completion or command history.
- Syntax is very simple:
```
Io> "This is a message and an object" print  
This is a message and an object==> This is a message and an object
```
- In this example, we implicitly create a string object and then pass it the method `print`, which tells the string to print itself.
- You create new objects in Io with the `clone` method of an existing object:
```
Io> Vehicle := Object clone
==>  Vehicle_0x55b3beafaa10:
  type             = "Vehicle"
```
- Objects have `slots`, which are like the key-value pairs of a hash/map.
```
Io> Vehicle description := "Something to take you places."
==> Something to take you places.
Io> Vehicle slotNames
==> list(description, type)
Io> Object slotNames
==> list(break, slotSummary, writeln, println, isError, setSlotWithType, hasLocalSlot, super, ifNilEval, thisMessage, loop, ownsSlots, appendProto, lexicalDo, shallowCopy, >=, markClean, uniqueId, newSlot, ifNonNil, for, coroDoLater, !=, slotValues, return, .., method, slotNames, evalArgAndReturnSelf, setSlot, cloneWithoutInit, block, isIdenticalTo, print, relativeDoFile, handleActorException, hasSlot, removeProto, asString, argIsCall, contextWithSlot, justSerialized, yield, thisLocalContext, isNil, launchFile, hasDirtySlot, getSlot, resend, prependProto, try, stopStatus, coroFor, @, inlineMethod, ancestorWithSlot, coroDo, lazySlot, raiseIfError, evalArgAndReturnNil, coroWith, actorRun, thisContext, updateSlot, -, serialized, isKindOf, actorProcessQueue, ifError, hasProto, isActivatable, setProtos, in, returnIfError, wait, ifNonNilEval, , asyncSend, proto, argIsActivationRecord, serializedSlots, removeAllSlots, <=, if, message, init, list, type, performWithArgList, deprecatedWarning, compare, ifNil, isTrue, isLaunchScript, ?, >, pause, doString, asSimpleString, futureSend, not, continue, doFile, doMessage, slotDescriptionMap, evalArg, setIsActivatable, currentCoro, <, switch, @@, while, serializedSlotsWithNames, perform, returnIfNonNil, ancestors, or, ==, protos, do, removeSlot, uniqueHexId, clone, apropos, removeAllProtos, doRelativeFile, asBoolean, addTrait, memorySize, foreachSlot, and, write, getLocalSlot, become, setProto)
```
- Io has multiple assignment operators (see https://iolanguage.org/guide/guide.html#Syntax-Assignment ):
  - `::=` - Creates a slot, creates setter method, and assigns a value.
  - `:=` - Creates a slot and assigns a value.
  - `=` - Assigns a value to an existing slot, or raises an exception.
```
Io> Vehicle doesntExistYet = "A value that won't get assigned"

  Exception: Slot doesntExistYet not found. Must define slot using := operator before updating.
  ---------
  message 'updateSlot' in 'Command Line' on line 1
```
- You get the value of a slot by sending the slot's name to the object:
```
Io> Vehicle description
==> Something to take you places.
```
- Every object has a `type` slot.
### Objects, Prototypes, and Inheritance
- In an OOP language, you could create a class `Vehicle`, a child class `Car`, and an instance of `Car` called `ferrari`.
- In Io, you would do something similar with cloning objects:
```
Io> Car := Vehicle clone
==>  Car_0x55b3beac89d0:
  type             = "Car"
Io> Car type
==> Car
Io> ferrari := Car clone
==>  Car_0x55b3bed43260:
Io> ferrari type
==> Car
```
- As shown above, Io is case-sensitive in how it handles types. Lowercase named objects share the type of their parent, Uppercase objects are their own types.
- Nonetheless, types are purely conventions for readability. Methods and slots will inherit the same regardless of whether an object has its own type or the type of its parents.

### Methods
- Methods are easy to define in Io. They're objects themselves, can be assigned to slots, and can be invoked by passing the slot name to the object that holds it:
```
Io> method() type
==> Block
Io> method("This is a method" print)
==> method(
    "This is a method" print
)
Io> Vehicle move := method("Go there!" print)
==> method(
    "Go there!" print
)
Io> Vehicle move
Go there!==> Go there!
```
- That's all there is to Io's object model. Everything else is just other built-ins and libraries.
- Some useful built-in methods:
  - `slotNames`
  - `getSlot`
  - `proto`
- `Lobby` is the namespace that holds all named objects, user-created or part of the language base.
```
Io> Lobby
==>  Object_0x55b3be9d2ad0:
  Car              = Car_0x55b3beac89d0
  Lobby            = Object_0x55b3be9d2ad0
  Protos           = Object_0x55b3be9d2980
  Vehicle          = Vehicle_0x55b3beafaa10
  _                = Object_0x55b3be9d2ad0
  exit             = method(...)
  ferrari          = Car_0x55b3bed43260
  forward          = method(...)
  set_             = method(...)
```
- `slotSummary` gives usage information on a slot (if available):
```
Io> slotNames
==> list(set_, exit, _, Protos, Lobby, forward)
Io> exit slotSummary

usage: io [-h | -e expr | -i file.io, file.io, ...| file.io arg, arg, ... | --version]

options:
  --version   print the version of the interpreter and exit
  -h          print this help message and exit
  -e          eval a given expression and exit
  -i          run the interpreter, after processing the files passed
```
- `Protos` is a useful method for inspecting the structure of Io:
```
Io> Protos
==>  Object_0x563633fc0980:
  Addons           = Object_0x563633fc0e10
  Core             = Object_0x563633fc0a40

Io> Protos Addons
==>  Object_0x563633fc0e10:

Io> Protos Core
==>  Object_0x563633fc0a40:
  Addon            = Addon_0x56363419d570
  AddonLoader      = AddonLoader_0x56363407cd30
  Block            = method(...)
  Break            = Break_0x563633fe60a0
  CFunction        = Object_<unnamed>()
  CLI              = CLI_0x56363434dcd0
  Call             = Call_0x563633fe46e0
  Collector        = Collector_0x563633fc1730
  Compiler         = Compiler_0x563633fc0fc0
  Continue         = Continue_0x563633fe6250
  Coroutine        = Coroutine_0x563633f9aea0
  Date             = Date_0x563633fe5a00
  Debugger         = Debugger_0x563633fe7e10
  Directory        = Directory_0x563633ff42a0
  DirectoryCollector = DirectoryCollector_0x563634277d60
  DummyLine        = DummyLine_0x56363405b7c0
  Duration         = Duration_0x563633fc7160
  DynLib           = DynLib_0x563633fc9e10
  Eol              = Eol_0x563633fe6640
  Error            = Error_0x563633fedc90
  Exception        = Exception_0x563633fc4320
  File             = File_0x563633fedeb0
  FileCollector    = FileCollector_0x563634279970
  Future           = Future_0x563634077890
  FutureProxy      = FutureProxy_0x563634077b90
  ImmutableSequence = ""
  Importer         = Importer_0x563633fb4440
  List             = list()
  Locals           = nil
  Map              = Map_0x563633feca90
  Message          = [unnamed]
  Normal           = Normal_0x563633fe5ef0
  Notifier         = Notifier_0x5636340619a0
  Number           = 0
  Object           = Object_0x563633f9acb0
  OperatorTable    = OperatorTable_0x56363404dc40
  Path             = Path_0x5636342f9fd0
  Profiler         = Profiler_0x563633fcab20
  Return           = Return_0x563633fe6400
  RunnerMixIn      = RunnerMixIn_0x5636342acc30
  Sandbox          = Sandbox_0x563633fc92c0
  Scheduler        = Scheduler_0x563634142880
  Sequence         = ""
  SerializationStream = SerializationStream_0x5636342c0990
  String           = ""
  System           = System_0x563633fd82f0
  TestRunner       = TestRunner_0x5636342816e0
  TestSuite        = DirectoryCollector_0x563634277d60
  UnitTest         = UnitTest_0x5636342acf00
  Vector           = ""
  WeakLink         = WeakLink_0x563633fc8cf0
  false            = false
  nil              = nil
  tildeExpandsTo   = method(...)
  true             = true
  vector           = method(...)
```
- Lastly, the `getSlot` method will give you a decompiled version of a method (see https://iolanguage.org/guide/guide.html#Introduction-Interactive-Mode ):
```
Io> List getSlot("asJson")
==> # /home/kali/io/libs/iovm/io/A3_List.io:288
method(
    "[" .. self map(asJson) join(",") .. "]"
)
```

### List and Maps
- Lists are ordered collections, cloned from the prototype object `List`, and there is a method `list` to initialize a list with values.
- Maps are collections of key-value pairs (like Ruby hashes), with `Map` as the prototype object.
- Lists are easy to work with,
```
Io> myList := list("This is my list", 5, "what else?")
==> list(This is my list, 5, what else?)
Io> myList append("Other stuff")
==> list(This is my list, 5, what else?, Other stuff)
```
- Io has other convenience functions for lists, including `sum`, `average`, `append`, `prepend`, `isEmpty`, and others:
```
Io> list(1, 2, 3, 4) sum    
==> 10
Io> list(1, 2, 3, 4) average
==> 2.5
Io> list(1, 2, 3, 4) append(5)
==> list(1, 2, 3, 4, 5)
Io> list(1, 2, 3, 4) prepend(0)
==> list(0, 1, 2, 3, 4)
Io> list(1, 2, 3, 4) isEmpty    
==> false
Io> List slotNames
==> list(asMessage, exSlice, sum, reduce, itemCopy, indexOf, remove, insertAt, isNotEmpty, removeSeq, detect, asSimpleString, at, removeAll, intersect, push, uniqueCount, capacity, isEmpty, map, reverseInPlace, append, copy, ListCursor, pop, sortInPlace, join, removeFirst, groupBy, min, justSerialized, atPut, union, max, fromEncodedList, size, empty, third, swapIndices, selectInPlace, atInsert, unique, sortBy, sort, flatten, containsAny, asString, mapFromKey, last, preallocateToSize, setSize, mapInPlace, sortKey, removeLast, first, reverseForeach, asMap, select, insertAfter, second, sortByKey, difference, cursor, appendIfAbsent, contains, with, prepend, reverse, insertBefore, reverseReduce, removeAt, sortInPlaceBy, sliceInPlace, foreach, slice, containsAll, asJson, rest, average, containsIdenticalTo, appendSeq, asEncodedList)
```
- Maps are the other major collection in Io. Io doesn't have the syntactic sugar of Ruby, but at least it's lightweight and easy to remember:
```
Io> myMap := Map clone
==>  Map_0x5636345a3f60:
Io> myMap atPut("id", 0)
==>  Map_0x5636345a3f60:
Io> myMap atPut("name", "root")
==>  Map_0x5636345a3f60:
Io> myMap at("id")
==> 0
Io> myMap at("name")
==> root
```
- Maps have much fewer slots than Lists,
```
Io> Map slotNames
==> list(asList, mergeInPlace, hasValue, values, isNotEmpty, detect, keys, foreach, asObject, isEmpty, at, with, merge, removeAt, hasKey, select, addKeysAndValues, justSerialized, atPut, atIfAbsentPut, reverseMap, size, empty, asJson, map)
Io> myMap asList
==> list(list(id, 0), list(name, root))
Io> myMap asJson
==> {"id":0,"name":"root"}
Io> myMap keys
==> list(id, name)
Io> myMap values
==> list(0, root)
```

### true, false, nil, and singletons
- Just like in Ruby, 0 is `true`, NOT `false` like it would be in C/C++.
- `true`, `false`, and `nil` are the first singletons we encounter in Io:
```
Io> x := nil clone
==> nil
```
- We can create more singletons by overloading the `clone` method of an object:
```
Io> Highlander := Object clone
==>  Highlander_0x56363404be50:
  type             = "Highlander"

Io> Highlander clone := Highlander
==>  Highlander_0x56363404be50:
  clone            = Highlander_0x56363404be50
  type             = "Highlander"

Io> x := Highlander clone
==>  Highlander_0x56363404be50:
  clone            = Highlander_0x56363404be50
  type             = "Highlander"
```
- Singletons are equal, whereas most clones of Object or other types are not.
- Overloading `clone` to make a singleton is a great example of how much power Io gives you, and how much you can implement with a little bit of code.
### Day 1 Self-Study
#### Find
- Question: Some example Io problems.
Answer: https://gist.github.com/jezen/7972975
- Question: An Io community that will answer questions.
Answer: This doesn't seem to exist any more, but there are archived posts from various communities on https://iolanguage.org/links.html
- Question: A style guide with Io idioms.
Answer: https://en.wikibooks.org/wiki/Io_Programming/Io_Style_Guide

#### Answer
- Question: Evaluate 1 + 1 and then 1 + "one". Is Io strongly typed or weakly typed? Support your answer with code.
Answer: Io is a strongly typed language that does not try to coerce values from one type to another:
```
Io> 1 + 1
==> 2
Io> 1 + "one"

  Exception: argument 0 to method '+' must be a Number, not a 'Sequence'
  ---------
  message '+' in 'Command Line' on line 1
```
- Question: Is 0 true or false? What about the empty string? Is nil true or false? Support your answer with code.
Answer: 0 is true, an empty string is true, and nil is false.
```
Io> 0 asBoolean
==> true
Io> "" asBoolean
==> true
Io> nil asBoolean
==> nil
Io> if (nil) then("nil is true" print) else("nil is false" print)
nil is false==> nil
```
- Question: How can you tell what slots a prototype supports?
Answer: Just call `proto` on the object to see the slots of its prototype
```
Io> Car proto
==>  Vehicle_0x5636346a3560:
  description      = "Something to take you places."
  type             = "Vehicle"
```
- Question: What is the difference between = (equals), := (colon equals), and ::= (colon colon equals)? When would you use each one?
Answer: I mentioned this above in my own notes even though it wasn't covered in the book. The Io guide covers this (see https://iolanguage.org/guide/guide.html#Syntax-Assignment ):
  - `::=` - Creates a slot, creates setter method, and assigns a value.
  - `:=` - Creates a slot and assigns a value.
  - `=` - Assigns a value to an existing slot, or raises an exception.

#### Do
- Question: Run an Io program from a file.
Answer: Easy,
```
benjamin@testVM:~$ cat test.io
"This is a program in a file" print
benjamin@testVM:~$ io test.io
This is a program in a file
```
- Question: Execute the code in a slot given its name.
Answer: This is a pretty confusing question, and we already know how to execute a method assigned to a slot! Nick Knowlson suggested the alternative interpretation of this question: try storing code as a string in a slot then executing it, and a third alternative that seems less likely to me (see http://www.nickknowlson.com/blog/2011/12/18/seven-languages-week-2-day-1/ ):
```
# If the code in a slot is stored as a string then you should use something like
# doString:

Zerg macroHarderSteps := ("\"Spreading creep now!\" println")
Zerg macroHarder := method(doString(Zerg macroHarderSteps))
Zerg macroHarder

# Update: Re-reading this now the intent of the question seems obvious! Write a
# method that, given a method name, will try to execute that method.
"\nLet's try that again" println
Zerg specifyMacro := method(name, perform(name))
Zerg specifyMacro("macroItUp")
Zerg specifyMacro("macroHarder")
"Done!" println
```

## Day 2: The Sausage King
### Conditionals and Loops
- Io has an unconditional `loop`, but this is only useful for concurrency
```
Io> loop("I'm getting dizzy" println)
I'm getting dizzy
I'm getting dizzy
I'm getting dizzy
I'm getting dizzy
...
I'm getting dizzy
^C
IOVM:
        Received signal. Setting interrupt flag.

  current coroutine
  ---------
  Coroutine callStack                  A4_Exception.io 244
  Coroutine backTraceString            A4_Exception.io 274
  Coroutine showStack                  System.io 69
  System userInterruptHandler          [unlabeled] 0
  Sequence println                     Command Line 1
```
- The conditional loops `while` and `for` are much more common, as you'd expect in any language
```
Io> i := 0
==> 0
Io> while(i<=11, i println; i = i + 1); "This one goes up to 11" println
0
1
2
3
4
5
6
7
8
9
10
11
This one goes up to 11
==> This one goes up to 11
```
- For loops are pretty standard, taking the name of the counter variable, a starting value, an ending value, an *optional* increment, and the message with sender to execute at each stage of the loop:
```
Io> for(i, 1, 11, 2, i println); "This one goes up to 11" println
1
3
5
7
9
11
This one goes up to 11
==> This one goes up to 11
```
- `if` and `elseIf` are pretty normal:
```
Io> if(true) then(x := "someValue") else(x := "someOtherValue")
==> nil
Io> x
==> someValue

Io> if(false) then(x := "someValue") elseif(false) then(x := "someOtherValue") else(x := "aThirdValue")
==> nil
Io> x
==> aThirdValue
```

### Operators
- Io is definitely light on syntactic sugar, but it allows operator overloading and custom operators like any good OOP should.
- Io's operators are defined in the `OperatorTable`
```
Io> OperatorTable
==> OperatorTable_0x5590e92e3c40:
Operators
  0   ? @ @@
  1   **
  2   % * /
  3   + -
  4   << >>
  5   < <= > >=
  6   != ==
  7   &
  8   ^
  9   |
  10  && and
  11  or ||
  12  ..
  13  %= &= *= += -= /= <<= >>= ^= |=
  14  return

Assign Operators
  ::= newSlot
  :=  setSlot
  =   updateSlot

To add a new operator: OperatorTable addOperator("+", 4) and implement the + message.
To add a new assign operator: OperatorTable addAssignOperator("=", "updateSlot") and implement the updateSlot message.
```
- This table specifies what operators exist, as well as their precedence.
- We can add a `xor` operator like this
```
Io> OperatorTable addOperator("xor", 11)
...
Io> true xor := method(bool, if(bool, false, true))
==> method(bool,
    if(bool, false, true)
)
Io> false xor := method(bool, if(bool, true, false))
==> method(bool,
    if(bool, true, false)
)
Io> true xor false
==> true
Io> true xor true
==> false
Io> false xor true
==> true
Io> false xor false
==> false
```
### Messages
- One of the trickiest concepts in Io is that everything is a message. Everything except comment markers and comments are messages.
- Every message has three components:
  - The Sender - sends the message (and its arguments.)
  - The Target - executes the message.
  - The Arguments - included with the message.
- The `call` method gives you access to this information via the arguments `sender`, `message`, `activated`, `slotContext`, or `target` (see https://iolanguage.org/guide/guide.html#Syntax-Messages ):
```
informative := method(x,
  "sender:" println;
  call sender println;
  "message:" println;
  call message println;
  "activated:" println;
  call activated println;
  "slotContext:" println;
  call slotContext println;
  "target:" println;
  call target println
  )

newObject := Object clone
newObject getInfo := method(informative(1))
newObject getInfo()
```
produces
```
sender:
 Object_0x55b98ff0bc90:
  getInfo          = method(...)

message:
informative(1)
activated:

# test.io:2
method(x,
    "sender:" println
    call sender println
    "message:" println
    call message println
    "activated:" println
    call activated println
    "slotContext:" println
    call slotContext println
    "target:" println
    call target println
)
slotContext:
 Object_0x55b98fb2ead0:
  Lobby            = Object_0x55b98fb2ead0
  Protos           = Object_0x55b98fb2e980
  _                = nil
  exit             = method(...)
  forward          = method(...)
  informative      = method(x, ...)
  newObject        = Object_0x55b98ff0bc90
  set_             = method(...)

target:
 Object_0x55b98ff0bc90:
  getInfo          = method(...)
```
- There are more arguments specifically for `call message`:
```
y := method(a, b, c,
  "message name: " print;
  call message name println;
  "message name: " print;
  call message arguments println;
  "message argAt(0): " print;
  call message argAt(0) println;
  "message argAt(1): " print;
  call message argAt(1) println
  "message argAt(2): " print;
  call message argAt(2) println
)

y("first", "second", "third")
message name: y
message name: list("first", "second", "third")
message argAt(0): "first"
message argAt(1): "second"
message argAt(2): "third"
```
- You can even implement new control structures due to the flexibility in how messages are evaluated. The book gives this example of an implementation of `unless` (see `\io\unless.io`):
```
unless := method(
    (call sender doMessage(call message argAt(0))) ifFalse(
    call sender doMessage(call message argAt(1))) ifTrue(
    call sender doMessage(call message argAt(2)))
)

unless(1 == 2, write("One is not two\n"), write("one is two\n"))
```

### Reflection
- Covered most of this in my notes above. There are many methods for getting information on objects and slots. `self proto` is one of the key ones.

### Day 2 Self-Study
#### Do
- Question: 1. A Fibonacci sequence starts with two 1s. Each subsequent number is the sum of the two numbers that came before: 1, 1, 2, 3, 5, 8, 13, 21, and so on. Write a program to find the nth Fibonacci number. fib(1) is 1, and fib(4) is 3. As a bonus, solve the problem with recursion and with loops.
Answer:
```
# with recursion
fib := method(i,
  if (i == 1 or i == 2) then (return 1) else (return fib(i - 1) + fib(i - 2)))
for (i, 1, 14, fib(i) println)

# with loops
fib := method(i,
  counter := 3;
  sum := 1;
  previous := 1;
  while (counter < i,
    temp := sum;
    sum := sum + previous;
    previous := temp;
    counter := counter + 1)
  return sum)
for (i, 3, 14, fib(i) println)
```
- Question: 2. How would you change / to return 0 if the denominator is zero?
Answer:
```
# we have to copy the current division slot to another place so we
# can still use it when the denominator isn't 0
Number setSlot("regularDivision", Number getSlot("/"))
# now we override the denominator method
Number/ = method(denominator,
  if (denominator == 0) then (return 0) else (return self regularDivision(denominator)))
  )
```
- Question: 3. Write a program to add up all of the numbers in a two-dimensional array.
Answer:
```
x := list(list(0, 1, 2, 3), list(4, 5, 6, 7), list(8, 9, 10, 11), list(12, 13, 14, 15))
"x: " println
x println
"x at(0) at(2): " println
x at(0) at(2) println
"x flatten: " println
x flatten println
"x flatten sum: " println
x flatten sum println
```
produces:
```
x:
list(list(0, 1, 2, 3), list(4, 5, 6, 7), list(8, 9, 10, 11), list(12, 13, 14, 15))
x at(0) at(2):
2
x flatten:
list(0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15)
x flatten sum:
120
```
- Question: 4. Add a slot called myAverage to a list that computes the average of all the numbers in a list. What happens if there are no numbers in a list? (Bonus: Raise an Io exception if any item in the list is not a number.)
Answer:
```
List myAverage := method(sum := 0;
  call target foreach(index, value, sum := sum + value);
  return (sum / (call target size)))
x := list(1, 2, 3, 4)
x myAverage(1) println

2.5
```
- Question: 5. Write a prototype for a two-dimensional list. The dim(x, y) method should allocate a list of y lists that are x elements long. set(x, y, value) should set a value, and get(x, y) should return that value.
Answer:
```
Matrix := Object clone
Matrix dim := method(x, y,
  self contents := List clone;
  for(i, 0, y - 1,
    self contents append(List clone);
    x repeat(self contents at(i) append(0));
    )
  )
Matrix set := method(x, y, value,
  self contents at(y) atPut(x, value)
  )
Matrix get := method(x, y,
  self contents at(y) at(x)
  )
x := Matrix clone
x dim(3, 3)
x set(0, 0, 1)
x set(1, 2, 5)
x contents at(0) println
x contents at(1) println
x contents at(2) println
"x get(0, 0):  " print
x get(0, 0) println
"x get(1, 1):  " print
x get(1, 2) println
```
- Question: 6. Bonus: Write a transpose method so that (new_matrix get(y, x)) == matrix get(x, y) on the original list.
Answer:
```
Matrix transpose := method(
  newMatrix := Matrix clone;
  newMatrix dim(self contents size, self contents at(0) size);
  for(y, 0, (self contents size) - 1,
    for(x, 0, (self contents at(0) size) - 1,
      newMatrix set(y, x, self get(x, y))
      )
    );
  return newMatrix
  )

newMatrix := x transpose()
for (y, 0, (newMatrix contents size) - 1,
  newMatrix contents at(y) println
  )
```
- Question: 7. Write the matrix to a file, and read a matrix from a file.
Answer:
```
Matrix toFile := method(fileName,
  fileHandle := File with(fileName);
  # erase the old file if necessary
  fileHandle remove;
  fileHandle openForUpdating;
  # the first line of the file will just be the x and y dimensions
  fileHandle write(self contents at(0) size asString);
  fileHandle write(" ")
  fileHandle write(self contents size asString);
  fileHandle write("\n")
  # then each line of the file is a row of the matrix.
  for (y, 0, (self contents size) - 1,
      for (x, 0, (self contents at(y) size) - 1,
        fileHandle write(self contents at(y) at(x) asString);
        fileHandle write(" ");
        )
      fileHandle write("\n");
    )
  fileHandle close
  )

Matrix fromFile := method(fileName,
  fileHandle := File with(fileName);
  fileHandle openForReading;
  topLine := fileHandle readLine;
  topLine := topLine split(" ");
  x_max := topLine first asNumber;
  y_max := topLine second asNumber;
  newMatrix := Matrix clone;
  newMatrix dim(x_max, y_max);
  for (y, 0, y_max - 1,
    nextLine := fileHandle readLine;
    nextLine split(" ") foreach(x, value, newMatrix set(x, y, value asNumber))
    );
  return newMatrix
  )

x toFile("test_x.txt")

reloadedMatrix := Matrix fromFile("test_x.txt")
for (y, 0, (reloadedMatrix contents size) - 1,
  reloadedMatrix contents at(y) println )
```
- Question: 8. Write a program that gives you ten tries to guess a random number from 1--100. If you would like, give a hint of ???hotter??? or ???colder??? after the first guess.
Answer:
```
guess := method(
  # pick a random number between 1 and 100, inclusive
  random := ((Random value() * 100) + 1) integerMin
  guesses := 0;
  previous_diff := 0;
  win := false;
  while (guesses < 10,
    "Guess the number between 1 and 100 (inclusive): " print
    guess := File standardInput readLine;
    guess := guess asNumber;
    if (guess == random) then (win = true; break) else (
      if (guesses > 0) then (
        if (((random - guess) abs) < previous_diff) then ("Hotter" print) else ("Colder" print)
        )
      previous_diff := (random - guess) abs;
      guesses := guesses + 1;
    )
  )
  if (win) then ("You win!" println) else (
    "You lost :(" println;
    "The correct answer was " print;
    random asString println;
    )
  )

guess()
```

## Day 3: The Parade and Other Strange Places
### Domain Specific Languages (DSLs)
- One of the best uses of Io is for DSLs, as it is so compact and easily transformed into one.
- Example of parsing this format:
```
{
  "Adam Smith": "555-555-1234",
  "Random Person": "555-555-4321"
}
```
- The example file `io\phonebook.io` shows how to make it so that Io can parse this format natively:
```
# START:range
OperatorTable addAssignOperator(":", "atPutNumber")
# END:range
# START:curlyBrackets
curlyBrackets := method(
  r := Map clone
  call message arguments foreach(arg,
       r doMessage(arg)
       )
  r
)
# END:curlyBrackets
# START:atPutNumber
Map atPutNumber := method(
  self atPut(
       call evalArgAt(0) asMutable removePrefix("\"") removeSuffix("\""),
       call evalArgAt(1))
)
# END:atPutNumber
# START:use
s := File with("phonebook.txt") openForReading contents
phoneNumbers := doString(s)
phoneNumbers keys   println
phoneNumbers values println
# END:use
```
- This example adds `:` as a new operator, and adds some methods that will parse curly brackets as a call to create a new `Map`, assigning the proper key-value pairs.

### Io's missing_method
- In the previous chapter, we discussed how `missing_method` can dramatically alter the syntax for a given class (or all classes.)
- The equivalent in Io is the `forward` method, which transforms Io's syntax even more completely than `missing_method` changes Ruby's.
- The `io\builder.io` example allows you to build XML by parsing something like this:
```
body(
  p("This is some test paragraph content.")
  )
```
- The example source `io\builder.io` is:
```
Builder := Object clone
Builder forward := method(
  writeln("<", call message name, ">")
  call message arguments foreach(
	arg,
	content := self doMessage(arg);
	if(content type == "Sequence", writeln(content)))
  writeln("</", call message name, ">"))
Builder  ul(
	li("Io"),
	li("Lua"),
	li("JavaScript"))
```
- And this gives us the expected output:
```
<ul>
  <li>
    Io
  </li>
  <li>
    Lua
  </li>
  <li>
    JavaScript
  </li>
</ul>
```
- We'll expand this example more in the upcoming exercises.

### Concurrency
- Io's creator was very forward-thinking when it comes to concurrency.

#### Coroutines
- A **coroutine** is one of the building blocks of concurrency. A coroutine is a function with multiple entry and exit blocks, allowing it to voluntarily give up control or resume execution smoothly.
- This is in contradistinction to methods of scheduling that may stop execution of a thread at any given instruction (i.e., **pre-emptive multitasking**.) Pre-emptive multitasking requires a lot of work on the programmer's part to ensure that multitasking doesn't leave files or other resources in invalid states.
- The `yield` keyword voluntarily suspends the process.
- A message is sent synchronously by prepending the message with `@` or `@@`.
- `@` returns a future.
- `@@` returns nil and starts the message in its own thread.
- The book provides the `io\coroutine.io` example:
```
vizzini := Object clone
vizzini talk := method(
            "Fezzik, are there rocks ahead?" println
            yield
            "No more rhymes now, I mean it." println
             yield)

fezzik := Object clone

fezzik rhyme := method(
			yield
            "If there are, we'll all be dead." println
            yield
            "Anybody want a peanut?" println)

vizzini @@talk; fezzik @@rhyme

Coroutine currentCoroutine pause
```
- This example sets up the `vizzini` and `fezzik` objects with lines from *The Princess Bride*, triggers their messages `talk` and `rhyme` in their own threads, and uses `pause` to ensure that the program runs until all asynchronous messages complete.
- The output from this is:
```
$ io coroutine.io
Fezzik, are there rocks ahead?
If there are, we'll all be dead.
No more rhymes now, I mean it.
Anybody want a peanut?
Scheduler: nothing left to resume so we are exiting

  Exception: Scheduler: nothing left to resume so we are exiting
  ---------
  Coroutine callStack                  A4_Exception.io 244
  Coroutine backTraceString            A4_Exception.io 274
  Coroutine showStack                  A4_Exception.io 177
  Coroutine pause                      Actor.io 150
  Object actorProcessQueue             Actor.io 115
```

#### Actors
- **Actors** are the abstractions that utilize coroutines for more complex behavior. Actors send, receive, and process messages, as well as create other actors.
- Io handles these messages concurrently by placing messages into queues that are then processed by coroutines.
- There's no real complexity to creating an actor in Io. In fact, all you need to do is to send an asynchronous message to an object and it immediately becomes an Actor.
- For example, without Actors, these messages execute sequentially,
```
slower := Object clone
slower start := method(wait(2); writeln("slowly"))
faster := Object clone
faster start := method(wait(1); writeln("quickly"))
slower start; faster start;
Io> slower start; faster start;
slowly
quickly
==> nil
```
- But we can make `faster` and `slower` into Actors with `@@`,
```
Io> slower @@start; faster @@start; wait(3)
quickly
slowly
quickly
==> nil
```

### Futures
- A **future** is an object returned immediately from an asynchronous message call. It remains null until the actual result of that call is available.
- If you check the value of a future, the process will block until the value is available (if it isn't already.)
- Futures in Io provide automatic deadlock detection, which is useful as you could get circular dependencies otherwise.

### Day 3 Self-Study
#### Do:
- Question: Enhance the XML program to add spaces to show the indentation structure.
Answer:
- Question: Create a list syntax that uses brackets.
Answer:
- Question: Enhance the XML program to handle attributes: if the first argument is a map (use the curly brackets syntax), add attributes to the XML tag. For example `book({"author": "Tate"}...)` would print `<book author="Tate">`
Answer:

## 3.5 Wrapping Up Io
- Io is a great example of a prototype-based language (e.g., JavaScript and Lua.)

### Strengths
#### Footprint
- Io is small enough to run comfortably on embedded systems, something it has support for on many platforms already.
- Io runs in its own small VM that is easily ported instead of requiring a bulky compiler that would need to handle the details of various platforms.

#### Simplicity
- You can learn Io's syntax in 15 minutes. The Io website gives the following informal BNF description that covers almost all of it (see https://iolanguage.org/guide/guide.html ):
```
exp        ::= { message | terminator }
message    ::= symbol [arguments]
arguments  ::= "(" [exp [ { "," exp } ]] ")"
symbol     ::= identifier | number | string
terminator ::= "\n" | ";"
```
- Because there are no reserved words, you can very quickly inspect and master the whole language.
- This simplicity is also what makes it so easy to extend the syntax or transform it to meet your needs via metaprogramming.

#### Flexibility
- Duck-typing is always nice.
- You can literally change any slot in any object. Thus, one line of Io can fundamentally change the entire language.
- You can overload the `forward` slot in any object (or all objects) in order to alter or inspect the behavior of the language.

#### Concurrency
- Despite development starting in 2002, Io's concurrency is much fresher than older but more popular languages like Java and Ruby.
- As a result, it's nearly effortless to use coroutines and Actors in Io.
- Considerations of mutable vs immutable data were baked into Io's design from the beginning, rather than being bolted on after the fact.
- The concepts introduced in Io's concurrency model are used in three other languages in this book: Scala, Erlang, and Haskell.

### Weaknesses
#### Syntax
- Io has zero syntactic sugar. This makes Io very easy to parse, but difficult to analyze and troubleshoot.
- A good analogy here is Assembly language. It's quite easy to understand each line of a series of Assembly instructions, but understanding the context or goal of them is much more difficult than in an higher-level, programmer-friendly language.

#### Community
- Io's community was very small at the time this book was published (2010), and has shrunk much more since then.
- I couldn't find any active communities answering questions on it. Even the subreddit's most recent post was three years old at the time of writing this (November 2021.)

#### Performance
- The author mentions that Io has some features that slow down its single-threaded execution speed? I assume this has something to do with the flexibility of the language and how it runs in the VM.
- This is probably the least important weakness, in my opinion.

#### My own complaints
- I love Io, but it looks like I discovered it too late. Active development of the language stopped a full two years before this book was written, 13 years before I read it.
- Some of the retrospectives written online argued that Io's creator Steven Dekorte made a mistake in focusing on embedded systems over general ones. I can't comment on that, but I do think he went too far on the side of simplicity over programmer convenience.
- My biggest complaint is how clumsy the interpreter is. I could tolerate the lack of autocompletion, but the lack of command history or even being able to paste multiple lines in safely is a deal-breaker for me.
- These drawbacks essentially negate even having an interpreter, forcing you to develop with the kind of workflow typical of a compiled language.
- I could not get any of the Io libraries working, including built-ins like URL and Random. I knew going into Io that I wouldn't have the third-party libraries of a language like Ruby or Python, but there's not much you can do without these core libraries.
