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





