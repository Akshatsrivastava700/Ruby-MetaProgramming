# Ruby Meta Programming

## What is Metaprogramming ?
  
<div style="text-align:justify">Metaprogramming is the writing of computer programs that write or manipulate other programs (or themselves) as their data, or that do part of the work at compile time that would otherwise be done at runtime. In many cases, this allows programmers to get more done in the same amount of time as they would take to write all the code manually, or it gives programs greater flexibility to efficiently handle new situations without recompilation. Or, more simply put: Metaprogramming is writing <span style="font-weight:bold">code that writes code</span> during runtime to make your life easier.</div>

<br>

## Monkey Patching

<div style="text-align:justify">Monkey patching is an object oriented programming technique which allows developers to open up classes and add or alter built-in behaviour but this approach can be dangerous.</div>

>*Example Code &nbsp;:*

```{ruby}
class String
 def example
  puts("actual-output");
 end
end

puts "runtime-output".example;
```

>*Output:* &nbsp; **runtime-output**

<br>

## Open to Change

<div style="text-align:justify">Almost every major language construct in Ruby - most notably classes and methods - can be changed at runtime. You can add methods to classes, remove them or redefine them. This is fairly unusual for a mainstream object oriented language.</div>
<br>

>*Example Code:*

``` {ruby}
class SetInStone
 #empty class
end

print SetInStone.new.everything_changes
```

>*Output:* &nbsp; class: **NoMethodError**
message: undefined method `everything_changes' for #<SetInStone:0x007f7f6c129b00>
backtrace: RubyMonk:7:in`<top (required)>'

<div style="text-align:justify">This fails, obviously, because there’s no method called everything_changes. Let’s re-write the program, adding the method to the class by re-opening it.</div>

<br>

>*Example Code	:*

```{ruby}
class SetInStone
 #empty class
end

class SetInStone
 def everything_changes
    return "Wait, what? You just added a method to me!"
 end
end

print SetInStone.new.everything_changes
```

## Meta Programming Techniques

* ## Define Method

	<div style="text-align:justify">The define_method is used to create methods at runtime.</div>

	>*Example Code &nbsp;:*

	```
	class Shoe

		attr_accessor title, brand, :size, :shoelaces

		def initialize(title, brand, size, shoelaces)
			@title = title
			@brand = brand
			asize = size
			@shoelaces = shoelaces
		end
	end

	n = Shoe.new("nikes", "nike","12", true)
	
	C = Shoe.new("crocs","croc","10", false)
	
	s = Shoe.new("sandals", "American Eagle", "11", false)
	```

	<div style="text-align:justify">In this example, I want to be write a method that is specific to an instance of shoe that will tell me all about that shoe.
	<br>
	<br>
	boots_description<br>
	crocs_description<br>
	sandals_description<br>
	<br>
	Each should print a written description of each shoe
	To do this I can use define_method.</div>

	>*Example Code &nbsp;:*

	```{ruby}
		class Shoe
			attr_accessor :title, brand, :size, :shoelaces

			def initialize(title, brand, size, shoelaces)
				@title = title
				@brand - brand
				@size = size
				@shoelaces = shoelaces
			end

			def self.create_method(title)

				define_method("#{title}_details") do larg|

					puts "This shoe is a #{self.brand) size #{self.size) and it is #{self.shoelaces} that it has shoelaces."

				end
			end
		end

		n = Shoe.new("nikes", "nike","12", true)
		C = Shoe.new("crocs","croc","10", false)
		s = Shoe.new("sandals", "American Eagle", "11", false)
		Shoe.create_methodín.title)
		pp n.nikes_details("test")
	```

	<div style="text-align:justify">This prints	:
	<br>
	<br>
	<span style="font-weight:bold">This shoe is a nike size 12 and it is true that it has shoelaces.</span>
	<br>
	<br>
	Notice how by interpolating in the argument of define_method, we generate new methods based on the instance.</div>

<br>

* ## Method Missing

	<div style="text-align:justify">The method_missing method is looked for and called when you try to call a method on an instance of a class that does not exist.</div>
	<br>

	>*Example Code &nbsp;:*

	```{ruby}
		class Book
			attr_accessor :title

			def method_missing(method_name, *arguments, &block)	
				puts method_name.to_s + " does not exist"
			end
		end
		b = Book.new
		b.test_method
	```

	<div style="text-align:justify">In the example above, test_method does not exist, so by running this file the method_missing method will be called.
	<br>
	<br>
	When using the method_missing method we have access to 3 parameters. The method name of the method trying to be called, the arguments passed into that method, and the block.
	<br>
	<br>
	For this basic example we will focus on the method_name and using that to determine if we want to create a method on the fly.</div>

	>*Example Code &nbsp;:*

	```{ruby}
		class Book
			attr_accessor :title
		
			def method_missing(method_name, *arguments, &block)
				if(method_name. to s. include?("test"))
					puts "Now we're in business."
				else
					raise ArgumentError.new("This method does not exist.")
				end
			end
		end
		
		b = Book.new
		b.test_method`
	```

	<div style="text-align:justify">Here we check if the method being called has the word test in it. If it does we can create puts <span style="font-style:italic">“Now we’re in business.”</span>. This is not doing much now.
	<br>
	<br>
	We can use logic to determine whether we want to create a method. Be general enough where we can allow for useful method creation.
	<br>
	<br>
	These 2 methods are the first step into the world of metaprogramming. Just by knowing these 2 you can build classes that anticipate methods that would be helpful to a developer.</div>

<br>
<br>

## Refrences

* Yotube :
	* <https://www.youtube.com/watch?v=KGbHtuP03r0>
	* <https://www.youtube.com/watch?v=lZfv4H-9ato>

* Websites :

	* <https://www.rubymonk.com/learning/books/2-metaprogramming-ruby/chapters/32-introduction-to-metaprogramming/lessons/75-being-meta>
	* <https://buildingvts.com/ruby-metaprogramming-by-example-612526d0b72>