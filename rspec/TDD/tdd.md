www.mashrurhossain.com

docu

# rspec
# spec is a test file
# run the spec and use the errors displayed to drive the development of our programm

- hello world

T D D

rspec.info

RSPEC to run our SPEC files!

install rspec

gem install rspec

rspec -v

```ruby
.
└── tests
    └── hello
        └── hello_spec.rb
```

```ruby
RSpec.describe __classname__ do

end
```

```ruby
RSpec.describe Hello do

end
```

running the file

rspec hello_spec.rb


```ruby
NameError:
  uninitialized constant Hello
```

```ruby
class Hello

end


RSpec.describe Hello do

end
```

now there are no errors...

but we dont do it like this

in hello.rb

```ruby
class Hello

end
```


```ruby
require_relative "hello"

RSpec.describe Hello do

end
```

# expectations

- we want a method, in a class, which will say Hello world
- expect running the method to yield an output of Hello World!


```ruby
require_relative "hello"

RSpec.describe Hello do
	it "it should return 'Hello World!' " do
		greeting = Hello.say_hello
		# expect the output from this line to match "Hello World!"
		expect(greeting).to eq("Hello World!")
	end
end
```

```ruby
Failures:

  1) Hello should return 'Hello World!'
     Failure/Error: greeting = Hello.say_hello

     NoMethodError:
       undefined method `say_hello' for Hello:Class
```


- step by step eliminate the error message
- do the minimum necessary at every step

```ruby
class Hello
	def self.say_hello
		
	end
end
```

```ruby
class Hello
	def self.say_hello
		"Hello World!"
	end
end
```

# Exercise

- Test a word is a Palindrom

- Spells the same forwards as backwards

- Example - Madam, Level, Deed etc.

- Not a palindrome - Food, Energy etc.

Step 1 - Devise how you would test this?

Two context here - 

1) The word is a Palindrome

2) The word is NOT a Palindrome

---------

Two context here - 

1) The word is a Palindrome
- it should read the same forwards as backwards

2) The word is NOT a Palindrome
- it should NOT read the same forwards as backwards


```ruby
require_relative 'word'

RSpec.describe Word do
	context "test word is a palindrome" do

	end

	context "test word is not a palindrome" do
		
	end
end
```

word_spec.rb
```ruby
require_relative 'word'

RSpec.describe Word do
	context "test word is a palindrome" do
		it "should read the same forwards as backwards" do
			test_word = "Madam"
			result = Word.palindrome? test_word
			expect(result).to be_truthy
		end
	end

	context "test word is not a palindrome" do
		it "should not read the same forwards as backwards" do

		end
	end
end
````

word.rb
```ruby
class Word
	def self.palindrome?(test_word)
		return true
	end
end
```

```ruby
	context "test word is not a palindrome" do
		it "should not read the same forwards as backwards" do
			test_word = "Food"
			result = Word.palindrome? test_word
			expect(result).to be_falsey
		end
	end
```

now the test fails




how to implement the method?

- change the case to lower case
- find the number of characters in the word
- determin the mid-point, depending on whether the length is an even or odd number
- split the word into two substrings, the head and the tail
- reverse the tail
- compare the two halves

```ruby
class Word
	def self.palindrome?(test_word)
		test_word = test_word.downcase
		word_length = test_word.length
		mid = word_length / 2
		mid_length = word_length % 2 == 0 ? mid : mid + 1

		first_half = test_word.slice(0, mid_length)

		second_half = ""

		if word_length % 2 == 0
			second_half = test_word.slice(mid_length, mid_length).reverse
		else
			second_half = test_word.slice(mid_length - 1, mid_length).reverse
		end
		first_half == second_half
	end
end
```

# example

We have so far been working with class level methods

We'll shift our focus now to work with instances, objects of classes and instance methods,
class-inherited variables associated with them

Let's talk about Student class

- students will have first name
- students will have last name

So any student object should thus respond to first name, last name and full name

Let's take a look at this through specs


student_spec.rb
```ruby
require_relative "student"

RSpec.describe Student do
	it "should responde to #first_name" do
		student = Student.new("John", "Doe")
		expect(student).to respond_to(:first_name)
	end

	it "should responde to #last_name" do
		student = Student.new("John", "Doe")
		expect(student).to respond_to(:last_name)
	end

	it "should responde to #student_fullname" do
		student = Student.new("John", "Doe")
		expect(student).to respond_to(:full_name)
	end
end
```

to create an object of a class, with need a constructor

with the @, instnace variable, they will be available to all the methods of this class

```ruby
class Student
	def initialize(first, last)
		@first_name = first
		@last_name = last
	end

	def first_name
		@first_name
	end

	def last_name
		@last_name
	end

	def student_fullname
		"#{first_name} #{last_name}"
	end
end
```

refactor with attr reader
you automatically say that you want access to the objects

```ruby
class Student
	attr_reader :first_name, :last_name

	def initialize(first, last)
		@first_name = first
		@last_name = last
	end

	def student_fullname
		"#{first_name} #{last_name}"
	end
end
```

refactor spec

```ruby
require_relative "student"

RSpec.describe Student do

	before do
		@student = Student.new("John", "Doe")
	end

	it "should responde to #first_name" do
		expect(@student).to respond_to(:first_name)
	end

	it "should responde to #last_name" do
		expect(@student).to respond_to(:last_name)
	end

	it "should responde to #student_fullname" do
		expect(@student).to respond_to(:student_fullname)
	end
end
```

without the instance variable it does not work.



# Introducing describe

in before blocs, we can introduce subject

subject - the object that's being described

```ruby
before do
	@student = Student.new("John", "Doe")
end
```

```ruby
subject { Student.new("John", "Doe") }
```

```ruby
it "should responde to #first_name" do
	expect(@student).to respond_to(:first_name)
end
```

```ruby
it { respond_to(:first_name) }
```

```ruby
require_relative "student"

RSpec.describe Student do
	describe "instance methods" do
		subject { Student.new("John", "Doe") }

		it { respond_to(:first_name) }

		it { respond_to(:last_name) }

		it { respond_to(:student_fullname) }
	end
end
```

# class variables

student_spec.rb
```ruby
require_relative "student"

RSpec.describe Student do
	[...]

	describe "total number of students created" do
		it "should have students in total" do
			Student.new("John", "Doe")
			Student.new("Jane", "Doe")

			expect(Student.total_count).to eq(2)
		end
	end
end
```

student.rb
```ruby
class Student
	attr_reader :first_name, :last_name
	@@total_count = 0

	def initialize(first, last)
		@first_name = first
		@last_name = last
		@@titak_count += 1
	end

	def student_fullname
		"#{first_name} #{last_name}"
	end

	def self.total_count
		@@total_count
	end
end
```

# Arithmetic in Ruby

- let's do some math in the console
- how would we design a spec for this?
- give it some contexts and tests based on expected results from methods
- get started building

calculator_spec.rb
```ruby
require_relative "calculator"

RSpec.describe Calculator do

	before do
		@first = 10
		@second = 50
	end

	context "adding two numbers together" do
		it "should return the sum of two numbers" do
			result = Calculator.add(@first, @second)
			expect(result).to eq 60
		end
	end

	context "substracting one number from the other" do
		it "should return the difference of the numberes" do
			result = Calculator.substract(@first, @second)
			expect(result).to eq (-40)
		end
	end

	context "multiplying two numbers" do
		it "should return the product of the numbers" do
			result = Calculator.multiply(@first, @second)
			expect(result).to eq 500
		end
	end

	context "dividing one number by another" do
		it "should return the quotient of the numbers" do
			result = Calculator.divide(@first, @second)
			expect(result).to eq 0.2
		end
	end

	context "raising one number to the power of the other" do
		it "should return the first number raised to the power of the other number" do
			result = Calculator.exp(2,3)
			expect(result).to eq 8
		end
	end

end
```

calculator.rb
```ruby
class Calculator
	def self.add(first, second)
		first + second
	end

	def self.substract(first, second)
		first - second
	end

	def self.multiply(first, second)
		first * second
	end

	def self.divide(first, second)
		first.to_f / second
	end

	def self.exp(first, second)
		first ** second
	end
end
```

# STRINGS

- widespread use in programming
- ability to work with and manipulate strings imperative
- usually surrounded by " - doubles quotes or ' - single quotes
- basic difference is " - double quotes allow string interpolation

there is a gemston called ruby in existence

Ruby is included in that stringd


```ruby
require_relative "strings"

RSpec.describe BasicString do
	
	context "case-sensitive" do
		it "should output interpolated text" do
			test_word = "Ruby"
			sentence = "There is a gemstone called ruby in existence"

			text = BasicString.new(sentence)
			result = text.contains_word? test_word

			expect(result).to be_falsey
		end
	end

	context "case-insensitive" do
		it "should output interpolated text" do
			test_word = "Ruby"
			sentence = "There is a gemstone called ruby in existence"

			text = BasicString.new(sentence)
			result = text.contains_word_ignorecase? test_word

			expect(result).to be_truthy
		end
	end
end
```


```ruby
class BasicString
	attr_reader :sentence

	def initialize(sentence)
		@sentence = sentence
	end

	def contains_word?(test_word)
		@sentence.include? test_word
	end

	def contains_word_ignorecase?(test_word)
		puts @sentence
		puts self.sentence
		@sentence.include? test_word.downcase
	end
end
```

refactor

```ruby
require_relative "strings"

RSpec.describe BasicString do
	before do
		@test_word = "Ruby"
		@sentence = "There is a gemstone called ruby in existence"
		@text = BasicString.new(@sentence)
	end
	
	context "case-sensitive" do
		it "should output interpolated text" do
			result = @text.contains_word? @test_word
			expect(result).to be_falsey
		end
	end

	context "case-insensitive" do
		it "should output interpolated text" do
			result = @text.contains_word_ignorecase? @test_word
			expect(result).to be_truthy
		end
	end
end
```

# Strings
- widespread use in programming
- ability to work with and manipulate strings imperative
- usually surrounded by " - double quotes or ' - single quotes
- basic difference is " - double quotes allow string interpolation

There is a gemstone called ruby in existence

What is %q or %Q

x = "The sum of 5 + 10 is #{5 + 10}"
=> "The sum of 5 + 10 is 15"
x = %Q{The sum of 5 + 10 is #{5 + 10}}
=> "The sum of 5 + 10 is 15"
x = %q{The sum of 5 + 10 is #{5 + 10}}
=> "The sum of 5 + 10 is \#{5 + 10}"

quoted_runner.rb

```ruby
require_relative "quoted"

placeholder = 5 + 10

sentence = %q{The sum of 5 + 10 is: #{placeholder}}
string = QuotedString.new sentence
puts string
# expectation, is not gonna interpolate, cuz the q

sentence = %Q{The sum of 5 + 10 is: #{placeholder}}
string = QuotedString.new sentence
puts string
# expectation, is gonna interpolate, cuz the Q
```


quoted_spec.rb

```ruby
require_relative "quoted"

RSpec.describe QuotedString do
	before do
		@placeholder = 5 + 10
	end

	context "quoted with %q" do
		it "should not output interpolated text" do
			# %q behaves like single quote
			sentence = %q{The sum of 5 + 10 is: #{@placeholder}}
			interpolated = "The sum of 5 + 10 is: 15"

			text = QuotedString.new sentence

			expect(text.start_with?('The sum')).to eq(true)
			expect(text).not_to eq(interpolated)
		end
	end
end
```

```ruby
NoMethodError:
 undefined method `start_with?' for #<QuotedString:0x00007ffe1a80abe0>
```

start_with? is for strings, not for objects

in our class

```ruby
class QuotedString
	[...]

	def to_s
		@sentence
	end
end
```


quoted_spec.rb
```ruby
expect(text.to_s.start_with?('The sum')).to eq(true)
```

we need another context

```ruby
require_relative "quoted"

RSpec.describe QuotedString do
	before do
		@placeholder = 5 + 10
	end

	context "quoted with %q" do
		it "should not output interpolated text" do
			# %q behaves like single quote
			sentence = %q{The sum of 5 + 10 is: #{@placeholder}}
			interpolated = "The sum of 5 + 10 is: 15"

			text = QuotedString.new sentence

			expect(text.to_s.start_with?('The sum')).to eq(true)
			expect(text).not_to eq(interpolated)
		end
	end

	context "quoted with %Q" do
		it "should output interpolated text" do
			sentence = %Q{The sum of 5 + 10 is: #{@placeholder}}
			interpolated = "The sum of 5 + 10 is: 15"

			text = QuotedString.new sentence

			expect(text.to_s).to eq(interpolated)
		end
	end
end
```


# Collections: Hash/Array etc...

Collections are data structures that allow us to have a number of objects grouped in them

Example - Products, you could have a collection of products as objects in a hash

Each product can have id, number, name or other attributes

We will work with a Product class now and Product will have the following attributes:

id, name, quantity, price

product_spec.rb

```ruby
require_relative "product"

RSpec.describe Product do

	before do
		@p1 = Product.new({id: 1, name: "Item1", quantity: 3, price: 25})
	end

	# tests to ensure getter methods are working
	it "responds to id" do
		expect(@p1).to respond_to(:id)
	end

	it "responds to name" do
		expect(@p1).to respond_to(:name)
	end

	it "responds to quantity" do
		expect(@p1).to respond_to(:quantity)
	end

	it "responds to price" do
		expect(@p1).to respond_to(:price)
	end
end
```

# This before block runs in every example, that means, we have multiple objecets!



```ruby
require_relative "product"

RSpec.describe Product do

	[...]

	it "returns correct attributes" do
		expect(@p1.id).to eq 1
		expect(@p1.name).to eq "Item1"
		expect(@p1.quantity).to eq 3
		expect(@p1.price).to eq 25
	end
end
```

```ruby
class Product
	attr_reader :id, :name, :quantity, :price
	def initialize(data = {})
		@id = data[:id] || 0
		@name = data[:name] || "Test Product"
		@quantity = data[:quantity] || 0
		@price = data[:price] || 0
	end
end
```

spec: we return an array of all the products that we are creating
- all
Product.all

Expectations

Array of all products

- array
- @@products
- add product objects to this @@products class array as they are built in the constructor


# This before block runs in every example, that means, we have multiple objecets!

product_spec.rb
```ruby
require_relative "product"

RSpec.describe Product do

	before do
		@p1 = Product.new({id: 1, name: "Item1", quantity: 3, price: 25})
	end

	[...]

	it "returns the list of all products" do
		expect(Product.all).to eq([@p1])
	end
end
```

product.rb
```ruby
require "pry"

class Product

	@@products = []

	attr_reader :id, :name, :quantity, :price
	def initialize(data = {})
		@id = data[:id] || 0
		@name = data[:name] || "Test Product"
		@quantity = data[:quantity] || 0
		@price = data[:price] || 0
		@@products << self
	end

	def self.all
		@@products
	end
end
```

to avoid creating an object for every example, 

```ruby
	before(:all) do
		@p1 = Product.new({id: 1, name: "Item1", quantity: 3, price: 25})
	end
```

specs
names of all the products from the list of products - Get this list


product_spec.rb
```ruby
	it "returns list of product names" do
		expect(Product.product_names).to eq([@p1.name])
	end
```

product.rb

```ruby
	def self.product_names
		# @@products.map { |product| product.name }
		@@products.map(&:name)
	end
```

Ability to filter our data
- we can replenish our stock when we run out of stock
- we get to know the items that have sold outr


product_spec.rb
```ruby
	it "returns the list of sold out products" do
		# create products, one with quantity 0 and another with quantity more than 0
		p2 = Product.new({id: 2, name: "Item2", quantity: 5, price: 15})
		p3 = Product.new({id: 3, name: "Item3", quantity: 0, price: 30})

		# since one product quantity is 0 it has to be restocked
		expect(Product.products_to_order).to eq [p3]
	end
```

product.rb
```ruby
	def self.products_to_order
		# select
		@@products.select { |product| product.quantity <= 0}
		# reject
		# @@products.reject { |product| product.quantity > 0 }
	end
```


Homework

Project: return the total inventory value

1 - 25
2 - 40

- spec:

- devse the method - class method
























































































