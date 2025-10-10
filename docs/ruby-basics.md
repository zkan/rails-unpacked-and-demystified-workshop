# Ruby Basics (+ Minitest)

## Basics

### Variables

ทดสอบ

```bash
ruby hello.rb
```

```ruby
name = "John"

# Global variable
$redis_conn = Redis.new

# Class variable - ซึ่งจะไม่ค่อยได้ใช้ ไม่แนะนำให้ใช้จาก style guide ต่าง ๆ
@@count = 1

# Instance variable - ตัวแปรที่อยู่ใน instance ของ class
@name = "Kan"

# local, block
country = "Thailand"
```

```ruby
i = 5
f = 5.0

5 + 5.0
# => 10.0

(5 + 5.0).round

# Big Decimal
# BigDecimal("0.0001")

# Rational Number - จำนวนตรรกยะ เช่น 1/2
r = 5/8r

5/8r + 3/8r

n = 3
n = n + 5
n += 1
```

```ruby
name = 'John'
name2 = "John" # Prefer double quotes - ตอนนี้ performance ไม่ต่างกับ single quote แล้ว


# Concatenation
c = "a" + "b"
c2 = "name: #{name}" # Prefer this way
c3 = "name: " + name
c4 = format("name: %s", name)

first = name[0]
last = name[-1]
before_last = name[-2]

# Symbol - ใช้ในกรณีที่เป็น key ซึ่งจริง ๆ จะ interchangable กับ string ซึ่ง symbol จะใช้ memory น้อยกว่า
another_name = :Kan
category = [:fruit, :vegetable]

# strip
"Kan\n".strip

# .empty?
# .include?

# .match
name.match(/j/) # => ได้ object MatchData ถ้าอยากได้ string เราจะสั่ง .to_s อีกที

# .gsub
name.gsub(/j/, "")

# HEREDOC
content = <<-CONTENT
Lorm Ipsum asadd
Hello WOrld
Yo yo
CONTENT

content = <<-HHHH
Lorm Ipsum asadd
Hello WOrld
Yo yo
HHHH

sql = <<-SQL
SELECT * FROM `users` WHERE name = '#{name}'
SQL
```

### Array

```ruby
arr = []
arr2 = Array.new # ไม่ recommend แบบนี้

arr3 = Array.new(3, 0) # dimension, default value -- ในงานจริงไม่ค่อยได้ใช้เลย

arr4 = [1, 2, 3]
arr4.first
arr4.first(2) # first 2 ตัวแรก
arr4.last
arr4.last(2) # last 2 ตัวท้าย

[1, 2] + [3] # => [1, 2, 3]
[1, 2] - [2] # => [1]

[1, 2] | [3] # Union
[1, 2] & [2] # Intersection

arr.push(5) # append 5 to array -> mutated
[1, 2, 3, 4] << 5 # not mutated

arr.pop # pop last one out -> mutated

[1, 2, 3, 4].join
[1, 2, 3, 4].join(", ")
[1, 2, 3, 4].join(" ")

# Set
numbers = Set.new
numbers << 1
numbers << 1
numbers << 1
numbers << 2
# => #<Set: {1, 2}>
numbers.to_a # To array
# Check if item is included?
numbers.include?(1)
```

```ruby
# Enumerable
# การนับทีละ 1

arr = [1, 2, 3, 4]
h = {
  name: "John",
  age: 18
}

# .each

arr.each { |i| puts i }

arr.each do |i|
  puts i
end

h.each { |k, v| puts k; puts v }

h.each do |k, v|
  puts k
  puts v
end

# map
arr.map { |i| i * 2 }

arr.map do |k, v|
  { k => v}
end.reduce(&:merge)

# reduce
arr.map { |i| i * 2 }
   .reduce(0) { |sum, i| sum + i }

arr.map { |i| i * 2 }
   .reduce { |sum, i| sum + i }

# split
"John, Jack".split(", ").map { |name| { name: name } }

"John, Jack".split(", ").map(&:strip)

[1, 2, 3, nil].all? { |i| !i.nil? }
```

### Hash

```ruby
h = {}

h[:name] = "John"
h[:age] = 18

h.keys
h.values

h.key? :name
h.key? :age
```

### Struct

```ruby
Person = Struct.new :name, :age do
  def display_name
    "Mr. #{name}"
  end
end

john = Person.new("John", 18)
puts john.display_name

jack = Person.new(name: "Jack", age: 20)
puts jack.display_name
```

### Control Flow

```ruby
# If
n = 5

if n < 5
  puts n
elsif n > 5
  puts false
else
  puts "blank"
end

case n
when n < 5
  puts n
else
  puts "blank"
end


# Loop
(0..5).to_a

for i in 0..5
  puts i
end

while true
  break
  next
end

# Falsy
# nil, false
```

### Method

```ruby
def sum(a, b)
  a + b # Prefer to remove return
end

def sum2(a, b)
  return 0 if a.nil? || b.nil? # Use as guard

  a + b
end

def sum3(a = 0, b = 0)
  a + b
end

def sum4(a:, b: 0)
  a + b
end

# sum4 a: 1
# sum4 b: 1
# sum4 b: 1, a: 10

def sum5(*args)
  args.each do |arg|
    puts arg
  end
end

def sum6(**kwargs)
  kwargs.each do |k, v|
    puts k
  end
end

def sum7(h: {})
end

# Try Exception

begin
  a + b
rescue
  puts "error"
ensure
  puts 0
end

# Ruby community recommends this in method
def sum7(a, b)
  a + b
# rescue (all exceptions when use only rescue)
rescue NameError
  puts "error"
end
```

```ruby
def greet
  # ...
  #yield
  yield "Kan"
  #yield do
  #end
end

greet { puts "Hello" }
greet { |name| puts "Hello, #{name}" }
```

### Class

```ruby
class Person
  # Reserved method + overriable
  def initialize(name:)
    @name = name
  end

  # เป็น shorthand แทนที่เราจะประกาศ method ด้านล่าง
  #attr_reader :name

  #def name
  #  @name
  #end

  # เป็น shorthand แทนที่เราจะประกาศ method ด้านล่าง
  #attr_writer :name

  # ทำให้เรา assign ค่าให้ name ได้
  #def name=(value)
  #  @name = value
  #end

  # เป็นทั้ง reader และ writer
  attr_accessor :name

  # Method อะไรก็ตามอยู่ใต้ protected นี้ จะเป็น protected
  protected

  def display_name
  end

  # Method อะไรก็ตามอยู่ใต้ private นี้ จะเป็น private
  private

  def show
  end
end

class Employer < Person
end

class Employee < Person
end

john = Employer.new(name: "John")
puts john.name

john.name = "Kan"
puts john.name

john = nil
puts john&.lastname # & เป็น safe operand ดักไว้ก่อน
```

### Module

```ruby
# ใช้เป็น mixin
module Greetable
  attr_accessor :name

  def hello(other_name)
    puts "#{name} said: Hello, #{other_name}"
  end
end

module ComputerCompany
  class Person
    include Greetable
  end
end

# ใช้เป็น namespace
john = ComputerCompany::Person.new
john.name = "John"
john.hello "Kan"
```

```ruby
module Greetable
  attr_accessor :name

  def hello(other_name)
    puts "#{name} said: Hello, #{other_name}"
  end
end
```

```ruby
#require "greetable"
require_relative "./greetable"

module ComputerCompany
  class Person
    include Greetable
  end
end
```

## Testing with Minitest

```bash
gem install minitest
```

```ruby
require "minitest/autorun"
require_relative "../fizzbuzz"

class FizzBuzzTest < Minitest::Test
  def test_it_should_get_fizz_when_input_is_3
    result = fizzbuzz(3)
    assert_equal "Fizz", result
  end
end
```

```ruby
def fizzbuzz(number)
  "Fizz"
end
```

```ruby
require "minitest/autorun"
```

```ruby
require_relative "./test_helper"
require_relative "./greetable"

class MockPerson
  include Greetable
end

class TestGreetable < Minitest::Test
  def test_hello
    john = MockPerson.new
    john.name = "John"

    assert_equal john.name, "John"
  end
end
```

```ruby
# minitest - เป็น PORO - อยู่ใน core ของ Ruby เลย
# assert_equal @name, "John"
#
# rspec - มีคนใช้เยอะกว่า จะเป็น DSL ของตัวเอง
# expect(@name).to eq("Kan")

require_relative "./test_helper"
require_relative "./computer_company"

class TestComputerCompany < Minitest::Test
  def setup
    @john = ComputerCompany::Person.new
    @john.name = "John"
  end

  def test_name
    assert_equal @john.name, "John"
  end

  def teardown
  end
end
```