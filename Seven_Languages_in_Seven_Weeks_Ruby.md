# Seven Languages in Seven Weeks
# Week 1 - Ruby
## Day 1: Finding a Nanny
- Very little unneeded punctuation, but plenty of syntactic sugar in Ruby, meant to make things as productive as possible for the programmer rather than the computer.
- Two types of string literals: single-quoted and double-quoted. Single-quoted are just taken at face value, whereas double-quoted are more like Python f-strings. The latter can have variables substituted into them, and will also correctly interpret things like `\n` as special characters:
```
irb(main):001:0> puts 'Hello, world!'
Hello, world!
=> nil
irb(main):002:0> language = 'Ruby'
irb(main):003:0> puts "hello, #{language}"
hello, Ruby
=> nil
```
- Everything is an object. Even numeric literals
```
irb(main):004:0> 4
=> 4
irb(main):005:0> 4 * 2
=> 8
irb(main):006:0> 4.class
=> Integer
irb(main):007:0> 4.methods
=> [:bit_length, :digits, :|, :lcm, :gcd, :-@, :**, :<=>, :<<, :>>, :<=, :>=, :==, :===, :rationalize, :next, :[], :gcdlcm, :denominator, :numerator, :upto, :chr, :%, :&, :*, :+, :inspect, :-, :/, :size, :succ, :<, :>, :ord, :to_int, :to_s, :to_i, :to_f, :to_r, :div, :divmod, :fdiv, :coerce, :^, :modulo, :remainder, :abs, :magnitude, :integer?, :floor, :ceil, :round, :truncate, :odd?, :even?, :allbits?, :anybits?, :nobits?, :downto, :times, :pred, :pow, :~, :dup, :+@, :rectangular, :rect, :arg, :real, :imaginary, :imag, :abs2, :angle, :phase, :conjugate, :conj, :to_c, :polar, :eql?, :singleton_method_added, :quo, :i, :real?, :zero?, :nonzero?, :finite?, :infinite?, :step, :positive?, :negative?, :clone, :clamp, :between?, :itself, :yield_self, :then, :taint, :tainted?, :untaint, :untrust, :untrusted?, :trust, :frozen?, :methods, :singleton_methods, :protected_methods, :private_methods, :public_methods, :instance_variables, :instance_variable_get, :instance_variable_set, :instance_variable_defined?, :remove_instance_variable, :instance_of?, :kind_of?, :is_a?, :tap, :class, :singleton_class, :display, :hash, :public_send, :method, :public_method, :singleton_method, :define_singleton_method, :extend, :to_enum, :enum_for, :=~, :!~, :nil?, :respond_to?, :freeze, :object_id, :send, :__send__, :!, :!=, :equal?, :__id__, :instance_eval, :instance_exec]
```
- There are some conventions familiar to Python programmers
```
irb(main):001:0> x,y = "test 123".split(" ")
irb(main):002:0> x
=> "test"
irb(main):003:0> y
=> "123"
```
- True and false in Ruby are very symbolic, not like in C-based languages. Empty strings and 0 are *not* false in Ruby, they're true. `nil` isn't equal to false, but it will count as false in a conditional.
```
irb(main):006:0> 0 == true
=> false
irb(main):007:0> true == true
=> true
irb(main):008:0> true == false
=> false
irb(main):009:0> 1 == false
=> false
irb(main):010:0> nil == false
=> false
irb(main):011:0> nil == true
=> false
irb(main):012:1* if nil
irb(main):013:1*   puts "yes"
irb(main):014:1* else
irb(main):015:1*   puts "no"
irb(main):016:0> end
no
=> nil
```
- You can use control blocks at the beginning of a line like in most programming languages, or at the end of one
```
irb(main):001:0> x = 100
irb(main):002:0> puts "#{x} is a big number" unless x < 50
100 is a big number
=> nil
```
- While and until are pretty straight forward
```
irb(main):003:0> x = 3
irb(main):004:0> x += 3 while x % 4 != 0
=> nil
irb(main):005:0> x
=> 12
irb(main):006:0> x = 1
irb(main):007:0> x += 1 until x % 13 == 0
=> nil
irb(main):008:0> x
=> 12
irb(main):009:1* while x > 0
irb(main):010:1*   puts "Countdown: #{x}"
irb(main):011:1*   x -= 1
irb(main):012:0> end
Countdown: 10
Countdown: 9
Countdown: 8
Countdown: 7
Countdown: 6
Countdown: 5
Countdown: 4
Countdown: 3
Countdown: 2
Countdown: 1
=> nil
```
- Logical operators have two forms: one that stops evaluating once it's either true or false (`and`/`&&` and `or`/`||`), and one that is fully evaluated regardless (`&` and `|`).
- For example, here we'll assign y to x as part of the conditional, but this code will only execute when we use `|` instead of `or`/`||`.
```
irb(main):001:0> x = true
irb(main):002:0> y = false
irb(main):003:0> true and (true or x = y)
=> true
irb(main):004:0> x
=> true
irb(main):005:0> true and (true | x = y)
=> true
irb(main):006:0> x
=> false
```
- Ruby is duck-typed, but still strongly-typed at the same time. Expect errors if you don't properly cast objects to the types you need.
```
irb(main):001:0> "id " + 4
Traceback (most recent call last):
        5: from /usr/bin/irb:23:in `<main>'
        4: from /usr/bin/irb:23:in `load'
        3: from /usr/lib/ruby/gems/2.7.0/gems/irb-1.2.1/exe/irb:11:in `<top (required)>'
        2: from (irb):1
        1: from (irb):1:in `+'
TypeError (no implicit conversion of Integer into String)
irb(main):002:0> "id " + 4.to_s
=> "id 4"
irb(main):003:0> "4" + 3
Traceback (most recent call last):
        5: from /usr/bin/irb:23:in `<main>'
        4: from /usr/bin/irb:23:in `load'
        3: from /usr/lib/ruby/gems/2.7.0/gems/irb-1.2.1/exe/irb:11:in `<top (required)>'
        2: from (irb):3
        1: from (irb):3:in `+'
TypeError (no implicit conversion of Integer into String)
irb(main):004:0> "4".to_i + 3
=> 7
```

### Find
- The Ruby API - https://rubyapi.org/
- The free, online version of *Programming Ruby: The Pragmatic Programmer's Guide* - https://ruby-doc.com/docs/ProgrammingRuby/
- A method that substitutes part of a string - Sub or gsub - https://ruby-doc.org/core-2.4.2/String.html#method-i-sub
- Information about Ruby's regular expressions:
  - https://www.rubyguides.com/2015/06/ruby-regex/
  - https://ruby-doc.org/core-2.5.1/Regexp.html
  - https://rubular.com/
- Information about Ruby's ranges - https://ruby-doc.org/core-2.5.1/Range.html

### Do
- Question: Print the string "Hello, World"
Answer:
```
irb(main):011:0> puts 'Hello, World!'
Hello, World!
=> nil
```
- Question: For the string "Hello, Ruby", find the index of the word "Ruby".
Answer: `scan` will match regexes, but `index` is what returns their locations
```
irb(main):013:0> "Hello, Ruby".scan(/Ruby/)
=> ["Ruby"]
irb(main):014:0> "Hello, Ruby".index("Ruby")
=> 7
```
- Question: Print your name ten times.
Answer:
```
irb(main):015:0> x = 0
irb(main):016:1* while x < 10
irb(main):017:1*   puts "Benjamin"
irb(main):018:1*   x = x + 1
irb(main):019:0> end
Benjamin
Benjamin
Benjamin
Benjamin
Benjamin
Benjamin
Benjamin
Benjamin
Benjamin
Benjamin
=> nil
```
- Question: Print the string "This is sentence number 1", where the number 1 changes from 1 to 10.
Answer:
```
irb(main):021:0> x = 1
irb(main):022:1* while x < 11
irb(main):023:1*   puts "This is sentence number #{x}"
irb(main):024:1*   x = x + 1
irb(main):025:0> end
This is sentence number 1
This is sentence number 2
This is sentence number 3
This is sentence number 4
This is sentence number 5
This is sentence number 6
This is sentence number 7
This is sentence number 8
This is sentence number 9
This is sentence number 10
=> nil
```
- Question: Run a ruby program from a file.
Answer:
```
benjamin@testVM:~$ echo "puts 'This is a ruby script.'" > test.rb
benjamin@testVM:~$ ruby ./test.rb
This is a ruby script.
```
- Question: Bonus problem: If you're feeling the need for a little more, write a program that picks a random number. Let a player guess the number, telling the player whether the guess is too low or too high.
Answer:
Program source, `guess.rb`
```
puts 'Guessing game v.1.0'
n = rand(10)
puts 'Guess a number between 1 and 10, inclusive'
guess = gets
guess = guess.to_i
puts 'Too high' if guess > n
puts 'Too low' if guess < n
puts 'Suspiciously accurate' if guess == n
```
Executing it
```
benjamin@testVM:~$ ruby guess.rb
Guessing game v.1.0
Guess a number between 1 and 10, inclusive
5
Too low
```

## Day 2: Floating Down from the Sky
### Arrays
- Ruby arrays are like lists or vectors in most languages, not static like C/C++ arrays.
- A single array may have values of different types, e.g., `x = [1, "dog", ["another_list"]]`
- One of the best things about them is the syntactic sugar that provides many different ways of accessing them:
```
irb(main):001:0> animals = ['lions', 'tigers', 'bears']
irb(main):002:0> puts animals
lions
tigers
bears
=> nil
irb(main):003:0> animals[0]
=> "lions"
irb(main):004:0> animals[2]
=> "bears"
irb(main):005:0> animals[5]
=> nil
irb(main):006:0> animals[-1]
=> "bears"
irb(main):007:0> animals[-2]
=> "tigers"
irb(main):008:0> animals[0..2]
=> ["lions", "tigers", "bears"]
irb(main):009:0> animals.include?('lions')
=> true
```
- Note that arrays return `nil` rather than throwing an error if you try to access an element that doesn't exist.

### Hashes
- The Ruby equivalent of Python's `dict`s or Clojure's `map`s. These are essentially associative arrays that map keys to values. You can't have duplicate keys, but you can have duplicate values.
```
irb(main):001:0> x = {"id" => 1, "size" => 4, "inventory" => ['pencil', 'pen', 'ink', 'eraser']}
=> {"id"=>1, "size"=>4, "inventory"=>["pencil", "pen", "ink", "eraser"]}
irb(main):002:0> x['id']
=> 1
irb(main):003:0> x['inventory']
=> ["pencil", "pen", "ink", "eraser"]
irb(main):004:0> x.keys
=> ["id", "size", "inventory"]
irb(main):005:0> x.values
=> [1, 4, ["pencil", "pen", "ink", "eraser"]]
```
- This subject brings up a data type we haven't encountered yet: symbols, which operate the same you would expect in any Lisp:
```
irb(main):003:0> x = {:first_name => "bob", :last_name => :common_last_name}
=> {:first_name=>"bob", :last_name=>:common_last_name}
irb(main):004:0> x[:last_name]
=> :common_last_name
```
- Just like everything else in Ruby, symbols are objects with methods and properties:
```
irb(main):005:0> :x.methods
=> [:start_with?, :end_with?, :<=>, :to_sym, :to_proc, :==, :to_s, :===, :=~, :next, :[], :casecmp, :casecmp?, :match, :match?, :empty?, :slice, :encoding, :upcase, :inspect, :intern, :capitalize, :downcase, :swapcase, :length, :id2name, :size, :succ, :clamp, :between?, :<=, :>=, :>, :<, :dup, :itself, :yield_self, :then, :taint, :tainted?, :untaint, :untrust, :untrusted?, :trust, :frozen?, :methods, :singleton_methods, :protected_methods, :private_methods, :public_methods, :instance_variables, :instance_variable_get, :instance_variable_set, :instance_variable_defined?, :remove_instance_variable, :instance_of?, :kind_of?, :is_a?, :tap, :class, :display, :hash, :singleton_class, :clone, :public_send, :method, :public_method, :singleton_method, :define_singleton_method, :extend, :to_enum, :enum_for, :!~, :nil?, :eql?, :respond_to?, :freeze, :object_id, :send, :__send__, :!, :!=, :equal?, :__id__, :instance_eval, :instance_exec]
```
- The main utility of symbols is that they are Singletons, so they're always equal to identical symbols.
```
irb(main):007:0> "x".object_id
=> 260
irb(main):008:0> "x".object_id
=> 280
irb(main):009:0> :x.object_id
=> 772188
irb(main):010:0> :x.object_id
=> 772188
```
- Hashes and symbols are a great combination for flexible, non-OOP data structures like we often use in Clojure.
- They can also be used to emulate using named parameters, which Ruby doesn't have built-in.
```
def process_payment(options=[])
  if options[:type] == "credit" and options[:brand] == "VISA"
    puts "Sorry, we don't take that one."
  end
end
```

### Code Blocks and Yield
- Code blocks are anonymous (lambda) functions that provide many of Ruby's unique advantages. These can be passed to other functions.
```
irb(main):151:0> 3.times {puts 'This is repetitive'}
This is repetitive
This is repetitive
This is repetitive
=> 3
```
- The multi-line alternative is to use `do` and `end`:
```
irb(main):152:1* 3.times do
irb(main):153:1*   puts 'some stuff'
irb(main):154:0> end
some stuff
some stuff
some stuff
=> 3
```
- You can define methods in a way that calls code blocks passed to them.
```
irb(main):001:0> "4".to_i + 3
=> 7
irb(main):002:1* def foo(x)
irb(main):003:2*   if x > 0
irb(main):004:2*     yield
irb(main):005:1*   end
irb(main):006:0> end
=> :foo
irb(main):007:0> foo(5) {puts "it worked!"}
it worked!
=> nil
```

### Defining Classes
- Class names have to start with a capital letter, and are usually CamelCase.
- Object variables are private by default, but you can make them public with the `attr_accessor` keyword:
```
class Foo
  attr_accessor :public_info

  def initialize(public_info, private_info)
    @public_info = public_info
    @private_info = private_info
  end
end
```
Now, the object variable `public_info` is public and `private_info` is, well, private.
```
irb(main):134:0> x = Foo.new("everyone know this", "don't tell anyone my passsword is password")
=> #<Foo:0x000055e480eff028 @public_info="everyone know this", @private_info="don't tell anyone my passsword is password">
irb(main):136:0> x.public_info
=> "everyone know this"
irb(main):137:0> x.private_info
Traceback (most recent call last):
        4: from /usr/bin/irb:23:in `<main>'
        3: from /usr/bin/irb:23:in `load'
        2: from /usr/lib/ruby/gems/2.7.0/gems/irb-1.2.6/exe/irb:11:in `<top (required)>'
        1: from (irb):137
NoMethodError (undefined method `private_info' for #<Foo:0x000055e480eff028>)
Did you mean?  private_methods
```
- You can use `@` to specify an instance variable (unique values for each instance of that class) and `@@` to specify a class variable (shared by all instances of that class.)
```
class Person
  @@population = 0

  def initialize(name)
    @@population += 1
    @name = name
  end

  def get_name
    puts "My name is #{@name}."
  end

  def get_population
    puts "Global population is #{@@population}."
  end
end

irb(main):018:0> alice = Person.new("Alice")
irb(main):019:0> alice.get_population()
Global population is 1.
=> nil
irb(main):020:0> bob = Person.new("Bob")
irb(main):021:0> alice.get_population()
Global population is 2.
=> nil
irb(main):022:0> chuck = Person.new("Chuck")
irb(main):023:0> alice.get_population()
Global population is 3.
=> nil
```

### Writing a mixin
- Mix-ins are Ruby's solution to not allowing Classes to have more than one parent class. They are similar to Java's solution of defining Interfaces.
```
module ToFile
  def filename
    "object_#{self.object_id}.txt"
  end

  def to_f
    File.open(filename, 'w') {|f| f.write(to_s)}
  end
end

class Person
  include ToFile
  attr_accessor :name

  def initialize(name)
    @name = name
  end

  def to_s
    name
  end
end
Person.new('matz').to_f
```
which results in
```
irb(main):034:0> Person.new('matz').to_f
=> 4
```
and creates a new file named `object_180.txt` with the contents `matz`.

### Modules, Enumerable, and Sets
- Modules are like namespaces or libraries in other languages. They have the same format as a Class, except using the `module` keyword instead of `class`.
- Just like Classes, Module names have to start with a capital letter.
- You can use the `include` keyword in a class to give it access to a module's methods, variables, and data.
```
module Money_Transferer
  exchange_types = ['credit', 'debit']

  def send_money(recipient, amount)
    recipient.add_balance(amount)
    self.reduce_balance(amount)
  end
end

class Person
  include Money_Transferer

  def initialize(balance)
    @balance = balance
  end

  def add_balance(amount)
    @balance += amount
  end

  def reduce_balance(amount)
    @balance -= amount
  end
end

alice = Person.new(100)
bob = Person.new(0)
alice.send_money(bob, 5)
print alice.balance
print bob.balance
```
- Two of the most important mixins in Ruby are `enumerable` and `comparable`.
- Implementing `enumerable` requires that the class implements `each`.
- Implementing `comparable` requires that the class implements the spaceship operator, `<=>`.
- `a <=> b` evaluates to
  - 1 if `a > b`
  - -1 if `a < b`
  - 0 otherwise
- An example of enumerable and comparable,
```
class Person
  include Comparable
  attr_accessor :name, :id

  def initialize(name, id)
    @name = name
    @id = id
  end

  def <=>(other)
    if self.id > other.id
      return 1
    elsif self.id < other.id
      return -1
    else
      return 0
    end
  end

  def to_s
    name + ", ID # " + id
  end
end

irb(main):025:0> alice = Person.new("Alice", 3)
irb(main):026:0> bob = Person.new("Bob", 1)
irb(main):027:0> chuck = Person.new("Chuck", 2)
irb(main):029:0> x.sort
=> [#<Person:0x000055ab9a9a2ab8 @name="Bob", @id=1>, #<Person:0x000055ab9a9385c8 @name="Chuck", @id=2>, #<Person:0x000055ab9a992258 @name="Alice", @id=3>]
```

### Find
- Question: Find out how to access files with and without code blocks. What is the benefit of the code block?
Answer: The pattern without code blocks is something like this (see https://ruby-doc.com/docs/ProgrammingRuby/html/tut_io.html ),
```
irb(main):212:1* File.open("test.txt", "r") do |my_file|
irb(main):213:2*   while not my_file.eof?
irb(main):214:2*     puts my_file.readline()
irb(main):215:1*   end
irb(main):216:0> end
Ruby world 1.0
this is a place
or is it a world?
no one knows
world world world world
=> nil
```
The approach with code blocks passes an anonymous function to `each_line` (or `each_byte`).
```
irb(main):218:0> File.open("test.txt", "r").each_line {|line| puts line }
Ruby world 1.0
this is a place
or is it a world?
no one knows
world world world world
=> #<File:test.txt>
```
- Question: How would you translate a hash to an array? Can you translate arrays to hashes?
Answer: This is fairly easy if you just want to drop the values into an array.
```
irb(main):021:0> z = []
=> []
irb(main):022:0> x = {'key1' => 'value1', 'key2' => 'value2'}
=> {"key1"=>"value1", "key2"=>"value2"}
irb(main):023:0> x.each_value{|y| z.push(y)}
=> {"key1"=>"value1", "key2"=>"value2"}
irb(main):024:0> z
=> ["value1", "value2", "value1", "value2", "value1", "value2"]
```
A simple way to put translate arrays to hashes is to use the indices of the array as the keys pointing to their values.
```
irb(main):028:0> x = ["cow", "dog", "horse"]
=> ["cow", "dog", "horse"]
irb(main):029:0> y = {}
=> {}
irb(main):030:0> x.each_index {|i| y[i] = x[i]}
=> ["cow", "dog", "horse"]
irb(main):031:0> y
=> {0=>"cow", 1=>"dog", 2=>"horse"}
```
- Question: Can you iterate through a hash?
Answer: Similar to the example, above we can iterate through the hash keys to show the key -> value relationships.
```
irb(main):034:0> y.each_key { |x| puts "y[#{x}] => #{y[x]}"}
y[0] => cow
y[1] => dog
y[2] => horse
=> {0=>"cow", 1=>"dog", 2=>"horse"}
```
- Question: You can use Ruby arrays as stacks. What other common data structures do arrays support?
Answer: You can use an array as,
  - Stack (LIFO) via the `push` and `pop` methods.
  ```
  irb(main):035:0> my_stack = []
  => []
  irb(main):036:0> my_stack.push("first")
  => ["first"]
  irb(main):037:0> my_stack.push("second")
  => ["first", "second"]
  irb(main):038:0> my_stack.push("third")
  => ["first", "second", "third"]
  irb(main):039:0> my_stack.pop()
  => "third"
  irb(main):040:0> my_stack.pop()
  => "second"
  irb(main):041:0> my_stack.pop()
  => "first"
  ```
  - Queue (FIFO), similarly, except taking from the front of the array.
  ```
  irb(main):042:0> my_stack.push("first")
  => ["first"]
  irb(main):043:0> my_stack.push("second")
  => ["first", "second"]
  irb(main):044:0> my_stack.push("third")
  => ["first", "second", "third"]
  irb(main):045:0> my_stack.each { |x| puts x}
  first
  second
  third
  => ["first", "second", "third"]
  ```

### Do
- Question: Print the contents of an array of sixteen numbers, four numbers at a time, using just `each`. Now, do the same with `each_slice` in `Enumerable`.
Answer: Using `each`, we're going to have to build up some kind of buffer, as well as keep track of the number of items accessed (we can't just go by string length do issues like multi-digit numbers and newlines.) Also, note that `output += '\n'` instead of `output += "\n"` would've inserted the character `\` followed by `n` instead of the newline character, resulting in a single line of output.
```
irb(main):001:0> output = ''
=> ""
irb(main):002:0> accessed = 0
=> 0
irb(main):003:1* my_array.each do |each_element|
irb(main):004:1*   output += each_element.to_s + " "
irb(main):005:1*   accessed += 1
irb(main):006:1*   output += "\n" if accessed % 4 == 0
irb(main):007:0> end
=> [0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15]
irb(main):008:0> puts output
0 1 2 3
4 5 6 7
8 9 10 11
12 13 14 15
=> nil
```
And now, using `each_slice` (note: the key here is using `p` instead of `puts`. This is a method that writes the value of `object.inspect` to the console, see: https://ruby-doc.com/core/Kernel.html#method-i-p )
```
irb(main):054:0> x = (0..15).to_a
=> [0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15]
irb(main):055:0> x.each_slice(4) { |y| p y}
[0, 1, 2, 3]
[4, 5, 6, 7]
[8, 9, 10, 11]
[12, 13, 14, 15]
=> nil
```
- Question: The Tree class was interesting, but it did not allow you to specify a new tree with a clean user interface. Let the initiator accept a nested structure of hashes. You should be able to specify a tree like this: `{'grandpa' => {'dad' => {'child1' => {}, 'child2' => {} }, 'uncle' => {'child3' => {}, 'child4' => {} } } }`.
Answer:
Modifying the `Tree.rb` example (see https://pragprog.com/titles/btlang/seven-languages-in-seven-weeks/ ) so the initializer takes a nested hash instead of a string and a list.
```
class Tree
  attr_accessor :children, :node_name

  def initialize(args={})
    @node_name = args.keys[0]
    @children = []
    # if there are any children specified, initialize them recursively
    temp_children = args[args.keys[0]]
    if temp_children.keys.length() > 0
      # each child will only be passed their sub-tree
      temp_children.each_key {|each_child| @children.push(Tree.new({each_child => temp_children[each_child]}))}
    end
  end

  def visit_all(&block)
    visit &block
    children.each {|c| c.visit_all &block}
  end

  def visit(&block)
    block.call self
  end
end

ruby_tree = Tree.new( {'grandpa' =>
  {'dad' => {'child1' => {}, 'child2' => {} },
  'uncle' => {'child3' => {}, 'child4' => {} } } })

puts "Visiting a node"
ruby_tree.visit {|node| puts node.node_name}
puts

puts "visiting entire tree"
ruby_tree.visit_all {|node| puts node.node_name}
```
- Question: Write a simple `grep` implementation that will print the lines of a file having any occurrences of a phrase anywhere in that line. You will need to do a simple regular expression match and read lines from a file. (This is surprisingly simple in Ruby.) If you want, include line numbers.
Answer:
This is fairly simple.
```
def match_line(line, line_number, regex)
  if line.match?(regex)
    puts "#{line_number}: #{line}"
  end
  line_number + 1
end

def grep(filename, regex)
  my_file = File.open(filename, "r")
  line_number = 0
  # this is kind of a kludge, using a
  my_file.each_line do |line|
     match_line(line, line_number, regex)
     line_number += 1
   end
end
```
For example, let's check what's in `test.txt` and how well our grep matches it.
```
irb(main):200:0> File.open('test.txt', 'r').each_line {|x| puts x}
Ruby world 1.0
this is a place
or is it a world?
no one knows
world world world world
=> #<File:test.txt>
irb(main):201:0> grep('test.txt', 'world')
0: Ruby world 1.0
2: or is it a world?
4: world world world world
=> #<File:test.txt>
```

## Day 3: Serious change
### Metaprogramming
- Programs that write programs. This is one of the key strengths of Ruby.
- Example in Ruby on Rails, uses the `has_many` and `has_one` methods to create the setters, getters, and storage variables needed to establish these relationships without the programmer wasting time spelling out how this should be done:
```
class Department < ActiveRecord::Base
  has_many:employees
  has_one:manager
end
```
### Open Classes
- Every Class in Ruby is redefinable at any time. This makes it very flexible, and also breakable.
- For example, we can add a `blank?` method to `nil` and to String objects like so:
```
class NilClass
  def blank?
    true
  end
end

class String
  def blank?
    self.size == 0
  end
end

["", "person", nil].each do |element|
  puts element unless element.blank?
end
```
- The example above lets us treat a `nil` and an empty string the same way.
- This can back-fire if you redefine something like `Class.new`.
- Another cool use is adding methods to the `Numeric` class to make unit conversion easy. See the `ruby\units.rb` example for this.
### Via method_missing
- `method_missing` is a debugging function Ruby calls when you try to invoke an method that doesn't exist.
- Many Ruby programmers overload this function to allow for all kinds of syntactical gymnastics.
- For example, you can implement Roman numerals like so (from `ruby\roman.rb`):
```
class Roman
  def self.method_missing name, *args
    roman = name.to_s
    roman.gsub!("IV", "IIII")
    roman.gsub!("IX", "VIIII")
    roman.gsub!("XL", "XXXX")
    roman.gsub!("XC", "LXXXX")

    (roman.count("I") +  
     roman.count("V") * 5 +
     roman.count("X") * 10 +
     roman.count("L") * 50 +
     roman.count("C") * 100)
  end
end

irb(main):016:0> puts Roman.X
10
=> nil
irb(main):017:0> puts Roman.XC
90
=> nil
irb(main):018:0> puts Roman.XII
12
=> nil
irb(main):019:0> puts Roman.X
10
=> nil
```
### Modules
- Modules are the primary way Ruby programmers use metaprogramming.
- The file `ruby\acts_as_csv_module.rb` provides a pretty weird example of this. The class `RubyCsv` has a single method called `acts_as_csv`.
- When `acts_as_csv` is called, the module it's defined in actually adds new methods to the `RubyCsv` class at runtime.
- This is more than simple inheritance, then. Instead, we're looking at classes that can morph and evolve in real-time, depending on what the situation requires.

### Day 3 Self-Study
#### Do
- Question: Modify the CSV application to support an `each` method to return a CsvRow object. Use `method_missing` on that `CsvRow` to return the value for the column for a given heading.
```
class CsvRow
  def method_missing name, *args
    column = name.to_s
    column = @headers.index(column)
    @row_contents[column]
  end

  def initialize(headers, row_contents)
    @headers = headers
    @row_contents = row_contents
  end
end

module ActsAsCsv
  def self.included(base)
    base.extend ClassMethods
  end

  module ClassMethods
    def acts_as_csv
      include InstanceMethods
    end
  end

  module InstanceMethods
    def read
      @csv_contents = []
      filename = self.class.to_s.downcase + '.txt'
      file = File.new(filename)
      @headers = file.gets.chomp.split(', ')

      file.each do |row|
        @csv_contents << CsvRow.new(@headers, row.chomp.split(', '))
      end
    end

    def each(&blk)
      @csv_contents.each(&blk)
    end

    attr_accessor :headers, :csv_contents
    def initialize
      read
    end
  end
end

class RubyCsv  # no inheritance! You can mix it in
  include ActsAsCsv
  acts_as_csv
end

irb(main):053:0> csv = RubyCsv.new
=> #<RubyCsv:0x0000564de1598ed8 @csv_contents=[#<CsvRow:0x0000564de1598a78 @headers=["one", "two"], @row_contents=[]>, #<CsvRow:0x0000564de1...
irb(main):054:0> csv.each {|row| puts row.one}

lions

=> [#<CsvRow:0x0000564de1598a78 @headers=["one", "two"], @row_contents=[]>, #<CsvRow:0x0000564de1598910 @headers=["one", "two"], @row_contents=["lions", "tigers"]>, #<CsvRow:0x0000564de15987d0 @headers=["one", "two"], @row_contents=[]>]
```

## 2.5 Wrapping Up Ruby
### Core strengths
- Pure object orientation
- Duck typing
- Modules and open classes
- Extremely productive

### Scripting
- Again, being so productive and expressive, Ruby is well-suited to one-offs and glue code.

### Web Development
- Ruby on Rails is one of the most popular web development frameworks ever.
- Rails is expressive enough that you can configure a production web app with a few lines of code.
- It handles all the details of database schemes and moving between them for you.


### Time to Market
- Similar to Python, Ruby has libraries for everything. This is due to its ubiquity in the startup world, and its common use as "glue code."
- Used in commercial sites all the time. Twitter ran on Ruby for years (although it eventually moved to another language in this book, Scala.)

### Weaknesses
- Performance is Ruby's biggest weakness. This hasn't stopped it from being adopted in high-performance environments, but it comes with the turf of an interpreted language with absolute freedom.
- Ruby's use of OOP complicates how it deals with concurrency, just as it does with any OOP language.
- Ruby's Duck Typing is one of its most useful features, but it inevitably reduces type safety and makes code analysis more difficult.


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
- a
### Reflection
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
- Question: 3. Write a program to add up all of the numbers in a two-dimensional array.
Answer:
- Question: 4. Add a slot called myAverage to a list that computes the average of all the numbers in a list. What happens if there are no numbers in a list? (Bonus: Raise an Io exception if any item in the list is not a number.)
Answer:
- Question: 5. Write a prototype for a two-dimensional list. The dim(x, y) method should allocate a list of y lists that are x elements long. set(x, y, value) should set a value, and get(x, y) should return that value.
Answer:
- Question: 6. Bonus: Write a transpose method so that (new_matrix get(y, x)) == matrix get(x, y) on the original list.
Answer:
- Question: 7. Write the matrix to a file, and read a matrix from a file.
Answer:
- Question: 8. Write a program that gives you ten tries to guess a random number from 1--100. If you would like, give a hint of “hotter” or “colder” after the first guess.
Answer:

## Day 3: The Parade and Other Strange Places
