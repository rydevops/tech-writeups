# Ruby Basics

*  Everything is an object
*  Ruby is case-sensitive
*  Ruby is not whitespace sensitive meaning we can use whitespace (spaces or tabs) to help make code easier to read
   *  A single line can contain multiple actions if they are seperated by `;`
*  When calling methods with more than one argument you must us parentheses.
   *  A good rule of thumb is to use parentheses unless it's perfectly clear when reading the code 
*  The `!` operator can be used to make changes to a variable and assign the result to that same variable in a single statement
   ```ruby
   name = 'jim'
   name.capitalize!  # Equivalent to 'name = name.capitalize'
   ```
*  The `?` operator can be applied to a method to generally indicate that the method returns `true` or `false`
   ```ruby
   my_data = "Some data to search through"

   if my_data.include? "data"
     puts "'data' included in '#{my_data}'"
   else
     puts "'data' not included in '#{my_data}'"
   end
   ```
*  The `=` tag is provided on a function name as a convention to incidate it is setting something
   ```ruby   
   class Car
     def name=(value)
       @name = value
     end
    end
    ```   
*  **Naming conventions**:
   *  **Variables** should use **snake case** labelling (e.g. `my_name = 17`)
   *  **Class Names** use camel casing (e.g. MyCar)
*  **Comments**:
   *  `#` - Single line comment (can be used after a executing statement)
      ```ruby
      # This is a comment
      puts "Hello!"  # This is another commment
      ```
   *   `=begin` & `=end` - Multi-line commenting
       ```ruby
       # To begin a multiline comment start with the =begin and 
       # end it with =end. Your comments go in between the two statements

       =begin
       Hello I am mulitline comment
       and I am showing how to use this 
       statement
       =end
       ```
*  **Custom Methods**:
   *  Syntax:
      ```ruby
      # Without any function arguments
      def method_name
        # method contents
      end

      # With basic arguments
      def method_name(parameter1, paramter2)
        # contents
      end

      # with splat agruments (i.e. a variable list of arguments)
      def greet(*friends)

      end 
      ```
   *  Methods can be define with and without arguments
   *  Methods can accept a variable number of items using the splat operator (*) as shown below. These parameters are an **Array**
      ```ruby
      # Accept a list of friends
      def greet(*friends)
        friends.each do |friend|
          puts "HEllo #{friend}"
        end  
      end

      greet('Jim', 'John', 'Tina')
      ```
   *  Methods can return data using the `return` keyword. Methods can also return values if the last statement in the method to execute
      returns a value (even if it's stored in a varaible) (this is how blocks work without using return keywords)
      ```ruby
      def sum(*numbers)
        sum = 0
        numbers.each { |number| sum += number }
        return sum
      end
      ```
   *  Method arguments can also contain default by assigning a value to them as shown below
      ```ruby
      def sort(array, reverse=false)
        # ...
      end 
      ```
*  **Blocks**
   *  Blocks are defined using `do` & `end` sections or `{}` sections. These create anonymous functions
   *  Blocks can be passed to methods as a parameter (e.g. the `each` methods of an `Array` receives a block to execute)
   *  To execute a block passed into a function use the `yeild` keyword within the function. 
      ```ruby
      #  creating a method that yields (calls) the block passed to it
      def execute_block
        yield # executes the block
      end

      execute_block { puts "Hello from the block!" }

      # Passing data to the block both from the function and before the function call
      def greet(name)
        yield("Jordan") # Passes an argument to the block
        yield(name) # Passes the argument to the block
      end

      greet("Peter") {|name| puts "Hello #{name}"} # Notice how the name is the parameter being passed to the block
      ```
   *  A **block** can be given optionally within a function by checking if a block as provided using `block_given?`
      ```ruby
      def greet(name)
        if block_given?
          yield(name)
        else
          puts "Hello #{name}"
        end    
      end

      greet("Jim") # Without block
      greet("Jim") { |name| "Bonjour #{name}" } # With block
      ```
*  **Proc** - Provides a way to name a block and store it for later use (similar to a function definition)
   *  Syntax
      ```ruby
      # Create a new Proc and assign a block to it
      double = Proc.new do |number|
        number * 2
      end

      # Passing the block to a function that yields
      [1, 2, 3].collect!(&double)

      # Another example
      def greeter(name)
        yield name
      end

      phrase = Proc.new do |name|
        puts "Hello #{name}!"
      end

      greeter("James", &phrase) 
      ```
      *  The `&` converts the proc to a block before passing it to the method
      *  Procs can be called directly using the `.call` method
         ```ruby
         phrase = Proc.new do |name|
           puts "Hello #{name}!"
         end

         phrase.call("Bob")
         ```
      * **procs** do not check the number of parameters passed to it 
      * **procs** that have a return keyword define in them will immediately terminate the calling function
        that yields the proc.        
        ```ruby
        def talker
            speaker = Proc.new { return "I am a speaker" }
            speaker.call # Do to above return we never get past this line
            returns "I am a talker"
        end

        puts talker # prints "I am a speaker"
        ```
      
*  **Lambdas** - Are similar to **proc**
   *  Syntax:
      ```ruby
      lambda { |param| block }
      ```
      *  Can be stored as a variable
         ```ruby
         double = lambda { |number| number * 2 }
         ```
      * **lambdas** check the number of parameters passed to it. `nil` is assigned to missing variables and extra vars are ignored
      * Unlike **procs** **labmdas** return control back to the yielding function even when they have a return statement within them
      * **lambdas** can be passed by converting `&` the **lambda** to a **block*
        ```ruby
        numbers = [1, 9, 2, 8, 3, 7, 4, 6, 5]

        sorter = lambda {|number_a, number_b| number_a <=> number_b}
        reverser = lambda {|number_a, number_b| number_b <=> number_a}

        sorted = numbers.sort(&reverser)

        puts sorted
        ```
*  **Classes** - Define a group of related attributes and methods to represent a real-time object (often)
   *  Syntax:
      ```ruby
      class Car
        $creator = "Jim Brown"  # Global variable acccessible by all
        @@count = 0  # Class-level variable
 
        # initialization method called automatically when instance created
        def initialize(make, model, year)
          # Create instance attributes
          @make = make
          @model = model
          @year = year
          @powered_on = false
          @count += 1
        end
         
        def toggle_power
            @powered_on = !@powered_on
            
            state = @powered_on ? "on" : "off"
            puts "Engine has been powered #{state}"
        end

        def Car.count
            puts @@count
        end

        def display_details
            puts "Make: #{@make}"
            puts "Model: #{@model}"
            puts "Year: #{@year}"
        end    
      end   
 
      car = Car.new("Toyota", "RAV4", 2019)
      car.toggle_power
      car.display_details
      puts "#{$creator}"
      ```
      *  Class methods are named after the class with a method name as shown below:
         ```ruby
         class Car
           def Car.do_something # Class method
           end
         end
         ```
      *  Methods can be marked `public` or `private`. If no modifier is specified the default will be `public`. Ruby
         marks sections as `public` and `private` instead of having to mark each method as shown below
         ```ruby
         class Car
           public
           # all public below this point
           def start
               puts "Starting car"
           end
       
           def stop
               puts "Stopping car"
           end
       
           private
           # Everything below is private
           def honk_horn
               puts "Honk honk!"
           end
       
           def lock_doors
               puts "Doors locked!"
           end
       
           public 
           # Again all public below this point
           def unlock_doors
               puts "Doors unlocked!"
           end
         end
         ```
      *  Instance variables have a short hand option for making both accessor and setter methods without having to
         define a new function for each by using `attr_reader` (accessor) and `attr_writer` (setter). These take symbols
         that match the name of your instance variable as shown below. If both an accessor and setter are required a shortcut
         using `attr_accessor` is also available. 
         ```ruby
         class Car
           attr_reader :make  # Accessor for 'make' instance variable
           attr_writer :make  # Settor for 'make' instance variable
           attr_accessor :make # Same as the two lines above
           
           # Equivalent to the following two methods
           def name
             return name
           end
           def name=(value)
             @name = value
           end
         end

         car = Car.new
         car.name = "Toyota"
         puts "Model is #{car.name}"  
         ```             
   *  Class variables:
      *  `@var_name`  - Instance variable
      *  `@@var_name` - Class variable
      *  `$var_name`  - Global variable (defined within the class itself)
   *  Inheritance:
      *  Syntax:
         ```ruby
         class ParentClass
           # parent definitions
         end

         class ChildClass < ParentClass # Inherit from the ParentClass
           #  child definitions 
         end
         ```
         *  Methods can be overridden (replaced) by redefining the definition of the method in the derived class
         *  You can call the super (parent) method by using the `super` keyword within the defined method. This will
            automatically find the same method name within the parent can execute it with any parameters passed
            ```ruby
            class ChildClass < ParentClass
              def do_something(name)
                super(name) # Look in the parent class for 'do_something' and call it with the name provided
              end
            ```
         *  Ruby only supports single-class inheritance but does support **mixins**
*  **Data Types**:
   *  **Number**:
      ```ruby
      age = 34
      salary = 99.95
      ```
   *  **Boolean** (true or false):
      ```ruby
      is_valid = false
      is_number = true
      ```
   *  **String**:
      ```ruby
      name = "Jim"
      type = 'Person'
      ```
      * **Methods/Attributes**
        *  **length** - Returns the number of characters in the string
        *  **reverse** - Reverse the order of the string
        *  **upcase** - Converts string to all uppercase
        *  **downcase** - Converts string to all lowercase
        *  **capitalize** - Ensures the first character is capitalized
        *  **include? <substring>** - Checks if the substring is included in the string
        *  **gsub!(<regexp>, <replace_value>)** - Performs global substituation replacing the regexp (if matched) with the replacement value
           ```ruby
           # Replace all 'a' characters with '@' characters
           data = "Some random data"           
           data.gsub!(/a/, "@")
           ```
      *  **String Interpolation** - Ruby provides a method to insert variables into strings without using concatenation. 
         To do this simply surround your variable name with a `#{}` operator. Methods can also be passed into the brackets. 
         ```ruby
         name = "Jim"
         age = 24

         puts "Hello #{name} I hear you are #{age} year(s) old!"
         puts "You are #{Float(age)}!"
         ```
   *  **Integer**:
      *  **Methods/Attributes**:
         *  **to_s** - Converts this instance to a string
   *  **Float**:
      *  **Methods/Attributes**:
         *  **to_s** - Converts this instance to a string
   *  **Array**:
      *  To create a new implicit Array place the items in `[]` brackets
         ```ruby
         a = [1, 2, 3]
         b = ["Hello", "world"]
         c = [1, "hello", 2, "world", 3] # Mixed type array

         # Access elements by index
         puts a[0]
         puts b[1]
         puts c[3]

         # Printing multi-dimension arrays
         city_province_map = [
             ["Winnipeg", 'MB'],
             ["Toronto", 'ON'],
             ["Vancouver", 'BC']
         ]

         cit_provice_map.each do |city,province| puts "#{city}, #{province}" end
         ```
      *  **Methods/Attributes**:
         *  **each** - Returns an Enumerator which can be used in a loop
            ```ruby
            numbers = [1, 2, 3]

            numbers.each do |x| # Loop and assign each value to x on each loop
                puts x
            end

            # alternate loop
            numbers.each { |x|
                puts x
            }
            ```
         *  **reverse** - Reverses the current sort order
   *  **Hash**: Used to create a set of key/value pairs
      ```ruby
      # Create an empty hash
      employee = Hash.new
      employee = Hash.new(0) # Set the default value of an undefined hash element (zero in this case)
                             # Defaults to nil

      # Creation of a new hash literal
      employee = {
          "name" => "Jim Brown",
          "gender" => "Male",
          "age" => 40,
          "contact_information" => {
              "address" => {
                  "suite" => nil,
                  "street" => "75 Barber St",
                  "city" => "Barry"
              },
              "phone_numbers" => {
                "home" => "204-555-1212",
                "mobile" => "204-555-9999"
              }
          }
      }

      # Updating, inserting and removing key/value pairs from an existing hash
      employee["name"] = "Jason" # Updated
      employee["new_key"] = "new_value" # Added a new value
      employee.delete('new_value') # Remove the new value

      # Print out a select value from the hash. 
      # Pay special attention to the nested hashes within this example
      puts "Employee's name: #{employee['name']}"
      puts "Employee's home address: #{employee['contact_information']['address']['street']}"

      # Iterate over the hash
      employee.each do |key, value|
          puts "#{key}: #{value}"
      end
      ```
      *  The `=>` operator works like assignment and assigned the value (right-side) to the key (left-side)
      *  As of version 1.9 of Ruby the hash-rocket (`=>`) syntax has been simplified to use json syntax. Strings as keys are automatically converted
         to symbols in this mode during creation and you must access the keys using the symbol format. 
         ```ruby
         data = {
             "name": "Bob the builder", # Converts "name" to :name
             age: 90 # Key becomes :age
         }

         puts data["name".intern] # Intern is an alias for to_sym
         puts data[:age]
         ```
      *  When attempting to access a key that doesn't exist the default is to return `nil` unless the default is overriden
         ```ruby
         contact = Hash.new # returns nil by default
         contact = Hash.new(0) # return '0' by default
         ```
      *  Keys don't need to be just Strings. They can also be other types including **Symbols**. 
         ```ruby
         data = {
             :id = 132
         }
         ```
      *  **Methods/Arguments**
         *  **delete(key)** - Removes an key/value pair from the hash
         *  **sort_by <expression>** - Sorts a hash using a custom defined sorting method
            ```ruby
            tally = {"Females" => 5, "Males" => 3}

            # Sort alphabetically by key name
            sorted = tally.sort_by do |gender, count| 
                gender # loop iterator passed into sort_by.
                # This method sort using -1, 0, 1 matching based on characters or type specific operators
            end
            ```
   *  **Symbol** - Is an identifier that always refers to the same location in memory when referenced. Use `:symbol_name` to create a new symbol. 
      ```ruby
      # Create a new symbol by referencing it at any point for the first time
      a_symbol = :my_new_symbol

      # Print out the memory ID for this symbol   
      :my_new_symbol.object_id
      a_symbol.object_id

      ```      
      *  These are generally used to reference keys in a hash or to reference method names
      *  Are immutable
      *  Only one instance of a symbol will ever exist and as such they save memory
      *  Can be converted to/from strings as follows
         ```ruby
         # Returns 'one' as string
         :one.to_s

         # Returns :two as symbol
         "two".to_sym
         ```
*  **Operators**:
   *  `+` - Addition
   *  `-` - Subtraction
   *  `*` - Multiplication
   *  `/` - Division
      ```ruby
      # Integer division (both numbers are integers)
      result = 7 / 2 # Returns 3

      # Floating point division (at least one number is a floating point number)
      result = 7 / 2.0 # Returns 3.5
      ```
   *  `**` - Exponentiation
   *  `%` - Modulus
   *  `!=` - Not equal (relational/comparator operator)
   *  `==` - Equality (relational/comparator operator)
   *  `>`, `>=`, `<`, `<=` (relational/comparator operators)
   *  `&&`, `||`, `!` (boolean/logical operators)
   *  `condition ? true_result : false_result` (Ternary operator)
   *  `||=` (conditional variable settings only if variable is not nil or undefined)
      ```ruby
      name = nil
      name ||= 'Jim' # This gets set to 'Jim'
      name ||= 'Bob' # name is not nil so it remains 'Jim'
      ```
   * `<<` - Shortcut for string concatentation and array.push
     ```ruby
     items = []
     items << 5
     items << 6

     name = "Jim"
     name << "Bob" # Now string is "JimBob"
     ```
*  **Flow-control**:
   *  **if-elsif-else-end**:
      ```ruby
        number = 0
        # Start of an if-statement
        if number < 0
            puts "Number is negative"

        # Secondary if-statement
        elsif number > 0
            puts "Number is positive"

        # All other conditions
        else
            puts "Number is zero"

        # End the if statement (required)
        end
      ```
   *  **unless-else-end**: Alternative form of an if-statement allowing for reverse logic
      ```ruby
      data_provided = false

      unless data_provided
        # Only execute if  data was provided (i.e. data_provided == false)
        puts "No data provided"
      else
        puts "Data was provided"
      end
      ```
      The `unless` statement can also be used in regular statements to conditionally execute them:
      ```ruby
      is_valid = false

      puts "Data is invalid" unless is_valid
      ```
    *  **while loop**: Performs an action until the loop condition is false
       ```ruby
       counter = 0
       while counter < 5 do # note: do keyword is optional
           puts counter
           counter = counter + 1
       end
       ```
    *  **until loop**: Performs an action until the loop condition is true
       ```ruby
       counter = 6
       until counter == 10 do # note: do keyword is optional
           puts counter
           counter = counter + 1
       end
       ```
    *  **for x in <range_or_iterator>**: Performs a for loop
       ```ruby
       # Using a range
       # Note: do keyword is optional
       for x in 1...10 do # three dots creates a Exclusive Range (1 to 9, excluding 10) object
         puts x
       end

       for x in 1..10 # two dots creates an Inclusive Range (1 through 10 including 10) object

       # for each loop
       for element in my_array do
         puts element
       end
       ```
    *  **loop do <actions> end**: Performs an loop
       ```ruby
       # Syntax 1: including do/end keywords
       count = 0
       loop do
         count += 1
         puts "Count: #{count}"
         break if count > 5
       end
   
       # Syntax 2: do/end can be replaces with {} brackets
       count = 0
       loop {
         count += 1
         puts "Count: #{count}"
         break if count > 5
       }

       # Counter loop
       # Loops 30 times (0 through 29)
       30.times do |number|
         puts("#{number+1}. Hi!")
       end
       ```
    *  **case-when** - Provides a select case option for picking an option from a list of available options
       *  Syntax:
          ```ruby
          print "Enter a choice: "
          choice = gets.chomp

          case choice
            when 'option1'
              do_some_stuff
            when 'option2'
              do_some_other_stuff
            else # All other choices
              display_error
          end
          ```        
*  Built-in objects:
   *  **puts** - Prints the provided arguments to the screen followed by a newline (i.e. put string)
      ```ruby
      puts age
      puts salary
      puts is_valid
      puts name
      puts(type) # Alternate form
      ```
   * **print** - Prints the provided arguments and nothing more
     ```ruby
     print "Hello!" # No new line
     print "Hello!\n" # Prints a new line like puts

     print name + " " + String(age)  # String concatenation and object type conversation
     ```
   *  **gets** - Retrieves input from the user and returns the results after the user presses enter. 
      The result contains the string plus the new line character by default. 
      *   **Methods/attributes**:
          *  **chomp** - Removes the newline character from the input
             ```ruby
             print "What is your name: "
             name = gets.chomp
             ```
      
         
