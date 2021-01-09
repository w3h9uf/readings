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








