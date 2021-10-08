# Seven Languages in Seven Weeks
# Week 1 - Ruby
## Day 1 self-study
- Very little unneeded puncuation, but plenty of syntactic sugar in Ruby, meant to make things as productive as possible for the programmer rather than the computer.
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

## Day 2 self-study
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
- They can also be used to emulate using named parameters, which Ruby doesn't have built-in
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
-

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
- a

### Writing a mixin
- Mix-ins are Ruby's solution to not allowing Classes to have more than one parent class. They are similar to Java's solution of defining Interfaces.
- You can have

### Modules, Enumerable, and Sets
- Modules are like namespaces or libraries in other langauges. They have the same format as a Class, except using the `module` keyword instead of `class`.
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
- Enumerable is cool

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
