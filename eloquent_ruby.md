# Chapter 1

> These kinds of “how to” comments should focus on exactly that: how to use the thing. Don’t explain why you wrote it, the algorithm that it uses, or how you got it to run faster than fast. 

> as the old bit of programming wisdom says, good code is like a good joke: It needs no explanation.

Camels for classes, snakes everywhere else. (`ALL_UPPERCASE` flavor of constant)

Paretheses are optional for function definition and ivocation. Do not add paretheses for methods with empty argument. Conditional statements **do not need paretheses**.

A good way to learn Ruby code is to read Ruby [Standard library source code](https://github.com/ruby/ruby/tree/master/lib).


# Chapter 2 Control Structure

`unless` <-> `if not`

`until` <-> `while !`

## Modifier form
 `@title = new_title unless @read_only`
 
 `doc.print_next_page while doc.pages_available?`
 
 `doc.print_next_page while doc.printed?`
 
 _Do this if that_
 
 
 ## Use each, not for
 ```
# for font in fonts
#   puts font
# end
 
 fonts.each do |font|
  puts font
 end
 ```
 
 > Mainly it is a question of eliminating one level of indirection. Ruby actually defines the for loop in terms of the each method: When you say `for font in fonts`, Ruby will actually conjure up a call to `fonts.each`. 
 
 
## `case`
```
case ticker
when 'AAPL'
  puts 'apple'
when 'AMZN'
  puts 'amazon'
else
  puts 'I don't know'
end
```

assign value computed from `case`
```
company = case ticker
          when 'AAPL' then 'apple'
          when 'AMZN' then 'amazon'
          else 'I don't know'
          end
```

`case` statements use `===` to do comparisons, which means you can use `case` to switch on the class of object, detect regular expressions and supply your own conditions

## Boolean logic
only `false` and `nil` are treated as false, everything else is treated as true. (`0` is also evaluated as `true`)


```
# init variable if it is nil
@first_name = '' unless @first_name

# it is the same as 
@first_name ||= ''
# this is more concise but with caveat, if @first_name is false, then it will assign '' to @false_name
```

# Chapter 3 Smart Collections

Creating arrays, hashes

```
names = ['Alan', 'James', 'Tracy']

# is same as 
names = %w{Alan James Tracy}
```

## function arguments 
You can specify how many arguments are needed with defaults
```
def print_name( first, last='Yang' }
  # do things
end

print_name ('Jason')
```

`*` means function can take arbitrary number of arguments
```
def print_labels( first, *args, last)
  message = first
  message += " #{args.join(' ')}"
  message += last
  puts message
end
```

## Iterate collections
```
words = %w{This is a sentence}

# do not use this verbose version
for i in 0..words.size
  puts words[i]
end

# use each
words.each { |word| puts word }

# iterate hash
m = {time: 2021, location: 'NJ', event: 'study'}
m.each {|w| pp w}

# or
m.each { |field, value| puts "#{field} => #{value}" }
```

## useful collection APIs
```
# find_index
def index_for (word)
  words.find_index { |this_word| word == this_word }
end

# map
pp words.map { |word| word.size }
# will give you [3, 5, 2]

# inject -- passes two arguments to the block
def total_word_length
  words.inject(0) { |result, word| word.size + result }
end
```

### `!` for collection mutation
```
a = [1, 2, 3]
# return a copy of reverse array
a.reverse

# mutate array, reverse in place
a.reverse!

# the same for sort v.s. sort!
```

> Don’t, however, get the idea that only methods with names ending in ! will change your collection. Remember, the Ruby convention is that an exclamation point at the end of a method name indicates that the method is the dangerous or surprising version of a pair of methods. Since making a modified copy of a collection seems very safe while changing a collection in place can be a bit dicey, we have sort and sort!. 

## Keys in hashes are ordered
Ordered in the way it is created, later added items will appear at the end. Changing values won't change the key order.


> There are two reasons for this preference for the bare collections over more specialized classes. First, the Ruby collection classes are so powerful that often there is no practical reason to create a custom-tailored collection simply to get some specialized feature. Frequently, a call to each or map is all you really need. And second, all things being equal, Ruby programmers actually prefer to work with generic collections. To the Ruby way of thinking, one less class is one less thing to go wrong. In addition, I know exactly how an array is going to react to a call to each or map, which isn’t necessarily true of the SpecializedCollectionOfStuff. When the problem is complexity, the cure might just be simplicity.

## caveats
take care when adding new elements well past the existing end of array
```
array = []
# 123457 new elements will be created with 123456 of them nil
array[123456] = 1
```

Also remember there is a `Set` class to use!


# Chpater 4 Smart Strings

Embed arbitrary expression in a __double quoted string__ by enclosing the expression between `#{` and `}`.
```
first_name = "Joe"
last_name = "Biden"
puts "The name is #{first_name} #{last_name}"
```

Escape
```
str = '"Stop", she said, "I can\'t live without \'s and "s."'

# %q to have same spirit of single quoted string
str = %q{"Stop", she said, "I can't live without 's and "s."}
str = %q("Stop", she said, "I can't live without 's and "s.")
str = %q<"Stop", she said, "I can't live without 's and "s.">

# %Q to have same spirit as double quoted string
str = %Q<The time in now #{Time.now}>

```

Span string multi-lines
```
str = "a long
string"

str = %q{a long
string}

puts str
# will show: a long 
#            string

str = %Q<a long \
string>
puts str
# will show: a long string

str = <<~P
 Here is how to 
 construct a string 
 that span across multiple 
 lines!
P
# can also use <<-P but that will leave the identation as it is.
```

Decision: it seems `%Q{}`  will be the best way to write complex string literals while single quoted string is the simplist.

## String API
```
# strip, lstrip, rstrip
' hello'.lstrip   # ->'hello'
' world '.strip    # ->'world'

# chomp to remove one line-terminating \n
# chop to remove trailing character
'hello\n\n\n'.chomp.   # 'hello\n\n'
'hello'.chop.   # 'hell'

# swapcase

# sub to substitute first occurance
'It was warm outside.'.sub('warm', 'cold')

# gsub to substitute all occurances
'yes yes'.gsub('yes', 'no')

'hello world'.split   # ["hello", "world"]
'1234:567:89'.split(':') # ["1234". "567", "89"]

# trailing ! after method will mutate the string instead of return a modified copy


"Here we go".index("go") # returns 2
```

## play with characters and bytes
```
str = 'hello'

str.each_char {|c| puts c}

str.each_byte {{b} puts b}

str2 = "hello
world"

str2.each_line { |l| puts l}
```

## variables are assigned by ref
```
str = "apple"
str2 = str
str.upcase!
puts str2  # APPLE
```


# RegEx
```
[0-9a-zA-Z]

\d matches any digits
\w matches any letterm number or underscore

A|B matches either A or B

\d:\d (AM|PM) matches time format 11:59 AM
```

RegEx in Ruby
```
# surround with forward slashes will make a RegEx
puts /\d:\d (AM|PM)/ =~ '10:23 AM'  # prints 0, means regex matches starting from index 0

puts /AM/ =~ '10:23 AM' # prints 6 means regex matches starting from index 6

puts /AM/ =~ '10:23' # prints nothing means regex does not match

# trailing i after regex will disable case sensitivity
puts /AM/i =~ 'am' # prints 0

# sub() and gsub can take a regex

# \A only matches start of a string
# \z only matches end of a string

# ^ matches start of a string or start of new line in a string
# $ matches end of a string or end of a line in a string

# trailing m will disable the fact that .* cannot match multiple lines
```

# Chapter 6 Symbols in Ruby

There can only ever be one instance of any given symbol

```
a = :all
b = a
c = :all

a == c # true
a === c # true
```

Symbols are immutable

In ruby, hash key are also immutable. There are sepcial defenses built in to guard against changing of hash keys.

```
# use to_s to turn a symbol into string
str = :all.to_s

# use to_sym to turn a string to symbol
the_symbol = 'all'.to_sym
```

_Use string for data, treat symbol as 'stand for', a const expression_



# Chapter 7 Everything is an object

class inheritance
```
class Dog < Animal
# ...
end
```

Everything is an object, including `nil`

```
puts nil.class # NilClass
puts nil.class.class # Class
```

All classes is inherited fron `Object`, `Object` bestows about fifty methods to its children.


`eval("some command")` to run the command as Ruby

Class methods are public by default, can expliciylt make them `private` and `protected`


`to_s` is a method of `Object` so if you `puts SomeClass`, `to_s` will get called to convert `SomeClass` to string and print it. You can override `to_s` in `SomeClass` to have your own printing format

> In a very real way, the Object class is the glue that binds Ruby together and lends the language its simple elegance.


# Chapter 8 Dynamic Typing

Ruby does not judge an object by its class hierarchy. If an object has the right methods, then it's the right kind of object. so called __Duck Typing__


> When you are coding, anything that reduces the number of revolving mental plates is a win. From this perspective, a typing system that you can sum up in a short phrase, “The method is either there or it is not,” has some definite appeal. If the problem is complexity, the solution might just be simplicity.



# Chapter 9 Write Specs!

## Test::Unit

```
require 'test/unit'

class DocumentTest < Test::Unit::TestCase
 # setup and teardown are called for each test case.
 def setup
  @text = 'a lot of words'
  doc = Document.new('test', 'author', @text)
 end
 
 def teardown
  # can close database connections, close socket, etc.
 end
 
 def test_doc_holds_onto_contents
  assert_equal @text, @doc.content, 'Check contents are still there'
 end
 
 def test_doc_can_return_words_in_array
  text = 'some more words'
  doc.Document.new('test', 'author', text)
  assert @doc.words.include?('a')
  assert @doc.words.include?('lot')
  assert @doc.words.include?('of')
  assert @doc.words.include?('words')
 end
 
 def test_word_count_is_correct
  asssert_equal 4, doc.word_count, 'Word count is correct'
 end
end
```

More assertions

```
assert_not_equal
assert_nil
assert_not_nil

assert_match /\d\d/, '23'

assert_instance_of String, 'hello'

# assert code will raise an exception
assert_raise ZeroDivisionError do
 x = 1/0
end

# assert code will not raise exception
assert_nothing_thrown do 
 x = 1/2
end


```

## Don't test it, spec it!

> RSpec tries to weave a sort of pseudo-English out of Ruby: The code above isn’t a test, it’s a description. 

Use `spec` to run your ruby spec files
```
spec document_spec.rb

# run all spec files in a whole directory tree (file will naming format <file>_spec.rb)
spec .
```

Example of spec file
```
require 'document'

describe Document do
 # similar to setup in Test::Unit, run before each test case
 before :each do 
  @text = 'a lot of wrds'
  @doc = Document.new('test', 'author', @text)
 end
 
 after :each do
  # run after each test case
 end
 
 # before :all and after :all to run before or after all test cases
 
 it 'should hold on the the contents' do
  @doc.content.should == @text
 end
 
 it 'should know which words it has' do
  @doc.words.should include('a')
  @doc.words.should include('lot')
  @doc.words.should include('of')
  @doc.words.should include('word')
 end
 
 it 'should know how many words it contains' do
  @doc.word_count.should == 4
 end
 
end
```

### Mocking in spec 

stub and mock
```
class PrintableDocument < Document
 def print( printer )
  return 'Printer unavailable' unless printer.available?
  printer.render( '#{title}\n' )
  printer.render( 'By #{author}\n' )
  printer.render( context )
  'Done'
 end
end

describe PrintableDocument do 
 before :each do 
  @text = 'a lot of wrds'
  @doc = Document.new('test', 'author', @text)
 end
 
 # stub
 it 'should_know how to print itself' do
  stub_printer = stub :available? => true, :render =>nil
  @doc.print( stub_printer ).should == 'Done'
 end
 
 it 'should return proper string if printer is not available' do
  stub_printer = stub :available? =>false, :render => nil
  @doc.print( stub_printer ).should == 'Printer unavailable'
 end
 
 # mock
 it 'should know how to print itself with mock' do
  mock_printer = mock('Printer')
  mock_printer.should_receive(:available?).and_return(true)
  mock_printer.should_receive(:render).exactly(3).times
  @doc.print(mock_printer).should == 'Done'
 end
 
end
```


```
# stub! can be used to stub out methods on any regular object
str = 'short string'
str.stub!(:length).and_return(10000)
```

### shoulda
```
require 'test/unit'
require 'shoulda'

class DocumentTest < Test::Unit::TestCase
 context 'a basic document class' do
  def setup
   @text = 'a lot of words'
   doc = Document.new('test', 'author', @text)
  end

  should 'hold on to the contents' do
   assert_equal @text, @doc.content, 'Check contents are still there'
  end
 end
end
```

MiniTest is also something to explore


> Unit tests should run quick with the setup that every developer has. They are your first line of defense, and in order to be any good they must be run often.





