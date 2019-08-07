# Learning Ruby Basics

*  Everything is an object
*  Ruby is case-sensitive
*  Ruby is not whitespace sensitive meaning we can use whitespace (spaces or tabs) to help make code easier to read
*  When calling methods with more than one argument you must us parentheses. 
*  The `!` operator can be used to make changes to a variable and assign the result to that same variable
   ```ruby
   name = 'jim'
   name.capitalize!  # Equivalent to 'name = name.capitalize'
   ```
*  The `?` operator can be applied to a method to indicate that the method generally returns `true` or `false`
   ```ruby
   my_data = "Some data to search through"

   if my_data.include? "data"
     puts "'data' included in '#{my_data}'"
   else
     puts "'data' not included in '#{my_data}'"
   end
   ```
*  **Naming conventions**:
   *  **Variables** should use **snake case** labelling (e.g. `my_name = 17`)
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
         To do this simply surround your variable name with a `#{}` operator. 
         ```ruby
         name = "Jim"
         age = 24

         puts "Hello #{name} I hear you are #{age} year(s) old!"
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
      
         