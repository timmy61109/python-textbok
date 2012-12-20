*******************
Variables and scope
*******************

Variables
=========

Recall that a variable is a label for a location in memory.  It can be used to hold a value.  In statically typed languages, variables have predetermined types, and a variable can only be used to hold values of that type.  In Python, you may reuse the same variable to store values of any type.

A variable is similar to the memory functionality found in most calculators, in that it holds one value which can be retrieved many times, and that storing a new value erases the old. A variable differs from a calculator's memory in that one can have many variables storing different values, and that each variable is referred to by name.

Defining variables
------------------

To define a new variable in Python, you simply assign a value to a label.  For example, this is how you create a variable called ``count``, which contains an integer value of zero::

    count = 0

This is exactly the same syntax as assigning a new value to an existing variable called ``count``.  Later in this chapter we will discuss under what circumstances this statement will cause a new variable to be created.

If you try to access the value of a variable which hasn't been defined anywhere yet, the interpreter will exit with a name error.

We can define several variables in one line, but this is usually considered bad style::

    # Define three variables at once:
    count, result, total = 0, 0, 0

    # This is equivalent to:
    count = 0
    result = 0
    total = 0

In keeping with good programming style, you should make use of meaningful names for variables.

Variable scope and lifetime
---------------------------

Not all variables are accessible from all parts of your program, and not all variables exist for the same amount of time.  Where a variable is accessible and how long it exists depend on how it is defined.  We call the part of a program where a variable is accessible its *scope*, and the duration for which the variable exists its *lifetime*.

A variable which is defined in the main body of a file is called a *global* variable.  It will be visible throughout the file, and also inside any file which imports that file.  Global variables can have unintended consequences because of their wide-ranging effects -- that is why you should almost never use them.  Only objects which are intended to be used globally, like functions and classes, should be put in the global namespace.

A variable which is defined inside a function is *local* to that function.  It is accessible from the point at which it is defined until the end of the function, and exists for as long as the function is executing.  The parameter names in the function definition behave like local variables, but they contain the values that you pass into the function when you call it.  When you use the assignment operator (``=``) inside a function, its default behaviour is to create a new local variable -- unless a variable with the same name is already defined in the local scope.

Here is an example of variables in different scopes::

    # This is a global variable
    a = 0

    if a == 0:
        # This is still a global variable
        b = 1

    def my_function(c):
        # this is a local variable
        d = 3
        print(c)
        print(d)

    # Now we call the function, passing the value 7 as the first and only parameter
    my_function(7)

    # a and b still exist
    print(a)
    print(b)

    # c and d don't exist anymore -- these statements will give you name errors!
    print(c)
    print(d)


The assignment operator
-----------------------

As you saw in the previous sections, the assignment operator in Python is a single equals sign (``=``).  This operator assigns the value on the right hand side to the variable on the left hand side, sometimes creating the variable first.  If the right hand side is an expression (such as an arithmetic expression), it will be evaluated before the assignment occurs.  Here are a few examples::

    a_number = 5              # a_number becomes 5
    a_number = total          # a_number becomes the value of total
    a_number = total + 5      # a_number becomes the value of total + 5
    a_number = a_number + 1   # a_number becomes the value of a_number + 1

The last statement might look a bit strange if you were to interpret ``=`` as a mathematical equals sign -- clearly a number cannot be equal to the same number plus one!  Remember that ``=`` is an assignment operator -- this statement is assigning a new value to the variable ``a_number`` which is equal to the old value of ``a_number`` plus one.

Assigning an initial value to variable is called *initialising* the variable.  In some languages defining a variable can be done in a separate step before the first value assignment.  It is thus possible in those languages for a variable to be defined but not have a value -- which could lead to errors or unexpected behaviour if you try to use the value before it has been assigned.  In Python a variable is defined and assigned a value in a single step, so you will almost never encounter situations like this.

The left hand side of the assignment statement must be a valid target::

    # this is fine:
    a = 3

    # these are all illegal:
    3 = 4
    3 = a
    a + b = 3

An assignment statement may have multiple targets separated by equals signs.  The expression on the right hand side of the last equals sign will be assigned to all the targets.  All the targets must be valid::

    # both a and b will be set to zero:
    a = b = 0

    # this is illegal, because we can't set 0 to b:
    a = 0 = b


Compound assignment operators
-----------------------------

You have already seen that you can assign the result of an arithmetic expression to a variable::

    total = a + b + c + 50

Counting is something that is done often in a program. For example, you might want to keep count of how many times a certain event occurs by using a variable called ``count``.   You would initialise this variable to zero and add one to it every time the event occurs.  You would perform the addition with this statement::

    count = count + 1

This is in fact a very common operation.  Python has a shorthand operator, ``+=`` which lets you express it more cleanly, without having to write the name of the variable twice::

    # These statements mean exactly the same thing:
    count = count + 1
    count += 1

    # You can increment a variable by any number you like.
    count += 2
    count += 7
    count += a + b

There is a similar operator, ``-=``, which lets you decrement numbers::

    # These statements mean exactly the same thing:
    count = count - 3
    count -= 3

Other common compound assignment operators are given in the table below:

========  ==========  =============
Operator  Example     Equivalent to
========  ==========  =============
``+=``    ``a += 5``  ``a = a + 5``
``-=``    ``a -= 5``  ``a = a - 5``
``*=``    ``a *= 5``  ``a = a * 5``
``/=``    ``a /= 5``  ``a = a / 5``
``%=``    ``a %= 5``  ``a = a % 5``
========  ==========  =============


More about scope: crossing boundaries
-------------------------------------

What if you want to access a global variable from inside a function?  It is possible, but doing so comes with a few caveats::

    a = 0

    def my_function():
        print(a)

    my_function()

The print statement will output ``0``, the value of the global variable ``a``, as you probably expected.  But what about this program? ::

    a = 0

    def my_function():
        a = 3
        print(a)

    my_function()

    print(a)

When you call the function, the print statement inside outputs ``3`` -- but why does the print statement at the end of the program output ``0``?

By default, the assignment statement creates variables in the local scope.  So the assignment inside the function does not modify the global variable ``a`` -- it creates a new local variable called ``a``, and assigns the value ``3`` to that variable.  The first print statement outputs the value of the new local variable -- because if a local variable has the same name as a global variable the local variable will always take precedence.  The last print statement prints out the global variable, which has remained unchanged.

What if you really want to modify a global variable from inside a function?  You can use the ``global`` keyword::

    a = 0

    def my_function():
        global a
        a = 3
        print(a)

    my_function()

    print(a)

You may not refer to both a global variable and a local variable by the same name inside the same function.  This program will give you an error::

    a = 0

    def my_function():
        print(a)
        a = 3
        print(a)

    my_function()

Because you haven't declared ``a`` to be global, the assignment in the second line of the function will create a local variable ``a``.  This means that you can't refer to the global variable ``a`` elsewhere in the function, even before this line!  The first print statement now refers to the local variable ``a`` -- but this variable doesn't have a value in the first line, because you haven't assigned it yet!

Note that it is usually very bad practice to access global variables from inside functions, and even worse practice to modify them.  This makes it difficult to arrange your program into logically encapsulated parts which do not affect each other in unexpected ways.  If a function needs to access some external value, you should pass the value into the function as a parameter.  If the function is a method of an object, it is sometimes appropriate to make the value an attribute of the same object -- we will discuss this in the chapter about object orientation.

.. Note:: There is also a ``nonlocal`` keyword in Python -- when you nest a function inside another function, it allows you to modify a variable in the outer function from inside the inner function (or, if the function is nested multiple times, a variable in one of the outer functions).  If you use the ``global`` keyword, the assignment statement will create the variable in the global scope if it does not exist already.  If you use the ``nonlocal`` keyword, however, the variable must be defined, because it is impossible for Python to determine in which scope it should be created.

Modifying values
================

Constants
---------

In some languages, it is possible to define special variables which can be assigned a value only once -- once their values have been set, they cannot be changed.  We call these kinds of variables *constants*.  Python does not allow you to set such a restriction on variables, but there is a widely used convention for marking certain variables to indicate that their values are not meant to change: we write their names in all caps, with underscores separating words::

    # These variables are "constants" by convention:
    NUMBER_OF_DAYS_IN_A_WEEK = 7
    NUMBER_OF_MONTHS_IN_A_YEAR = 12

    # Nothing is actually stopping us from redefining them...
    NUMBER_OF_DAYS_IN_A_WEEK = 8

    # ...but it's probably not a good idea.

Why do we bother defining variables that we don't intend to change?  Consider this example::

    MAXIMUM_MARK = 80

    tom_mark = 58
    print(("Tom's mark is %.2f%%" % (tom_mark / MAXIMUM_MARK * 100)))
    # %% is how we escape a literal % inside a string

There are several good reasons to define ``MAXIMUM_MARK`` instead of just writing ``80`` inside the print statement.  First, this gives the number a descriptive label which explains what it is -- this makes the code more understandable.  Second, you may eventually need to refer to this number in your program more than once.  If you ever need to update your code with a new value for the maximum mark, you will only have to change it in one place, instead of finding every place where it is used -- such replacements are often error-prone.

Literal numbers scattered throughout a program are known as "magic numbers" -- using them is considered poor coding style.  This does not apply to small numbers which are considered self-explanatory -- it's easy to understand why a total is initialised to zero or incremented by one.

Sometimes we want to use a variable to distinguish between several discrete options.  It is useful to refer to the option values using constants instead of using them directly if the values themselves have no intrinsic meaning::

    # We define some options
    LOWER, UPPER, CAPITAL = 1, 2, 3

    name = "jane"
    # We use our constants when assigning these values...
    print_style = UPPER

    # ...and when checking them:
    if print_style == LOWER:
        print(name.lower())
    elif print_style == UPPER:
        print(name.upper())
    elif print_style == CAPITAL:
        print(name.capitalize())
    else:
        # Nothing prevents us from accidentally setting print_style to 4, 90 or
        # "spoon", so we put in this fallback just in case:
        print("Unknown style option!")

In the above example, the values ``1``, ``2`` and ``3`` are not important -- they are completely meaningless.  You could equally well use ``4``, ``5`` and ``6`` or the strings ``'lower'``, ``'upper'`` and ``'capital'``.  The only important thing is that the three values must be different.  If we used the numbers directly instead of the constants the program would be much more confusing to read.  Using meaningful strings would make the code more readable, but you could accidentally make a spelling mistake while setting one of the values and not notice -- if you mistype the name of one of the constants you are more likely to get an error straight away.

Some Python libraries define common constants for your convenience, for example::

    # you need to import these libraries before you use them
    import string
    import math
    import re

    # All the lowercase ASCII letters: 'abcdefghijklmnopqrstuvwxyz'
    print(string.ascii_lowercase)

    # The mathematical constants pi and e, both floating point numbers
    print(math.pi) # ratio of circumference of a circle to its diameter
    print(math.e) # natural base of logarithms

    # This integer is an option which you can pass to functions in the re
    # (regular expression) library.
    print(re.IGNORECASE)

Note that many built-in constants don't follow the all-caps naming convention.

Mutable and immutable types
---------------------------

Some *values* in python can be modified, and some cannot.  This does not ever mean that you can't change the value of a variable -- but if a variable contains a value of an *immutable type*, you can only assign it a *new value*.  You cannot *alter the existing value* in any way.

Integers, floating-point numbers and strings are all immutable types -- in all the previous examples, when we changed the values of existing variables we used the assignment operator to assign them new values::

    a = 3
    a = 2

    b = "jane"
    b = "bob"

Even this operator doesn't modify the value of ``total`` in-place -- it also assigns a new value::

    total += 4

We haven't encountered any mutable types yet, but we will use them extensively in later chapters.  Lists and dictionaries are mutable, and so are most objects that you are likely to write yourself::

    # this is a list of numbers
    my_list = [1, 2, 3]
    my_list[0] = 5 # we can change just the first element of the list
    print(my_list)

    class MyClass(object):
        pass # this is a very silly class

    # Now we make a very simple object using our class as a type
    my_object = MyClass()

    # We can change the values of attributes on the object
    my_object.some_property = 42

.. Todo:: Exercise 7

More about input
----------------

In the earlier sections of this unit you learned how to make a program display a message using the ``print`` function or read a string value from the user using the ``input`` function.  What if you want the user to input numbers or other types of variables?  You still use the ``input`` function, but you must convert the string values returned by ``input`` to the types that you want.  Here is a simple example::

    height = int(input("Enter height of rectangle: "))
    width = int(input("Enter width of rectangle: "))

    print("The area of the rectangle is %d" % (width * height))

``int`` is a function which converts values of various types to ints.  We will discuss type conversion in greater detail in the next section, but for now it is important to know that ``int`` will not be able to convert a string to an integer if it contains anything except digits.  The program above will exit with an error if the user enters ``"aaa"``, ``"zzz10"`` or even ``"7.5"``.  When we write a program which relies on user input, which can be incorrect, we need to add some safeguards so that we can recover if the user makes a mistake.  For example, we can detect if the user entered bad input and exit with a nicer error message::

    try:
        height = int(input("Enter height of rectangle: "))
        width = int(input("Enter width of rectangle: "))
    except ValueError as e: # if a value error occurs, we will skip to this point
        print("Error reading height and width: %s" % e)

This program will still only attempt to read in the input once, and exit if it is incorrect.  If we want to keep asking the user for input until it is correct, we can do something like this::

    correct_input = False # this is a boolean value -- it can be either true or false.

    while not correct_input: # this is a while loop
        try:
            height = int(input("Enter height of rectangle: "))
            width = int(input("Enter width of rectangle: "))
        except ValueError:
            print("Please enter valid integers for the height and width.")
        else: # this will be executed if there is no value error
            correct_input = True

We will learn more about boolean values, loops and exceptions later.

Example: calculating petrol consumption of a car
------------------------------------------------

In this example, we will write a simple program which asks the user for the distance travelled by a car, and the monetary value of the petrol that was used to cover that distance. From this information, together with the price per litre of petrol, the program will calculate the efficiency of the car, both in litres per 100 kilometres and and kilometres per litre.

First we will define the petrol price as a constant at the top. This will make it easy for us to update the price when it changes on the first Wednesday of every month::

    PETROL_PRICE_PER_LITRE = 4.50

When the program starts,we want to print out a welcome message::

    print("*** Welcome to the fuel efficiency calculator! ***\n")
    # we add an extra blank line after the message with \n

Ask the user for his or her name::

    name = input("Enter your name: ")

Ask the user for the distance travelled::

    # float is a function which converts values to floating-point numbers.
    distance_travelled = float(input("Enter distance travelled in km: "))

Then ask the user for the amount paid::

    amount_paid = float(input("Enter monetary value of fuel bought for the trip: R"))

Now we will do the calculations::

    fuel_consumed = amount_paid / PETROL_PRICE_PER_LITRE

    efficiency_l_per_100_km = fuel_consumed / distance_travelled * 100
    efficiency_km_per_l = distance_travelled / fuel_consumed

Finally, we output the results::

    print("Hi, %s!" % name)
    print("Your car's efficiency is %.2f litres per 100 km." % efficiency_l_per_100_km)
    print("This means that you can travel %.2f km on a litre of petrol." % efficiency_km_per_l)

    # we add an extra blank line before the message with \n
    print("\nThanks for using the program.")

.. Todo: Exercise 8

Type conversion
===============

As you write more programs, you will often find that you need to convert data from one type to another, for example for a string to an integer or from an integer to a floating-point number.  There are two kinds of type conversions in Python: implicit and explicit conversions.

Implicit conversion
-------------------

Recall from the section about floating-point operators that you can arbitrarily combine integers and floating-point numbers in an arithmetic expression -- and that the result of any such expression will always be a floating-point number.  This is because Python will convert the integers to floating-point numbers before evaluating the expression.  This is an *implicit conversion* -- you don't have to convert anything yourself.  There is usually no loss of precision when an integer is converted to a floating-point number.

For example, the integer ``2`` will automatically be converted to a floating-point number in the following example::

    result = 8.5 * 2

``8.5`` is a ``float`` while ``2`` is an ``int``.  Python will automatically convert operands so that they are of the same type.  In this case this is achieved if the integer ``2`` is converted to the floating-point equivalent ``2.0``.  Then the two floating-point numbers can be multiplied.

Let's have a look at a more complex example::

    result = 8.5 + 7 // 3 - 2.5

Python performs operations according to the order of precedence, and decides whether a conversion is needed on a per-operation basis. In our example ``//`` has the highest precedence, so it will be processed first.  ``7`` and ``3`` are both integers and ``//`` is the integer division operator -- the result of this operation is the integer ``2``. Now we are left with ``8.5 + 2 - 2.5``.  The addition and subtraction are at the same level of precedence, so they are evaluated left-to-right, starting with addition.  First ``2`` is converted to the floating-point number ``2.0``, and the two floating-point numbers are added, which leaves us with ``10.5 - 2.5``.  The result of this floating-point subtraction is ``2.0``, which is assigned to ``result``.

Explicit conversion
-------------------

Converting numbers from ``float`` to ``int`` will result in a loss of precision. For example, try to convert ``5.834`` to an ``int`` -- it is not possible to do this without losing precision. In order for this to happen, you must explicitly tell Python that you are aware that precision will be lost. For example, you need to tell the compiler to convert a ``float`` to an ``int`` like this::

    i = int(5.834)

The ``int`` function converts a ``float`` to an ``int`` by discarding the fractional part -- it will always round down!  If you want more control over the way in which the number is rounded, you will need to use a different function::

    # the floor and ceil functions are in the math module
    import math

    # ceil returns the closest integer greater than or equal to the number
    # (so it always rounds up)
    i = math.ceil(5.834)

    # floor returns the closest integer less than or equal to the number
    # (so it always rounds down)
    i = math.floor(5.834)

    # round returns the closest integer to the number
    # (so it rounds up or down)
    # Note that this is a built-in function -- you don't need to import math to use it.
    i = round(5.834)

Explicit conversion is sometimes also called *casting* -- you may read about a ``float`` being *cast* to ``int`` or vice-versa.

Converting to and from strings
------------------------------

As you saw in the earlier sections, Python seldom performs implicit conversions to and from ``str`` -- you usually have to convert values explicitly.  If you pass a single number (or any other value) to the ``print`` function, it will be converted to a string automatically -- but if you try to add a number and a string, you will get an error::

    # This is OK
    print(5)
    print(6.7)

    # This is not OK
    print("3" + 4)

    # Do you mean this...
    print("3%d" % 4) # concatenate "3" and "4" to get "34"

    # Or this?
    print(int("3") + 4) # add 3 and 4 to get 7

To convert numbers to strings, you can use string formatting -- this is usually the cleanest and most readable way to insert multiple values into a message.  If you want to convert a single number to a string, you can also use the ``str`` function explicitly::

    # These lines will do the same thing
    print("3%d" % 4)
    print("3" + str(4))

More about conversions
----------------------

In Python, functions like ``str``, ``int`` and ``float`` will try to convert *anything* to their respective types -- for example, you can use the ``int`` function to convert strings to integers or to convert floating-point numbers to integers.  Note that although ``int`` can convert a float to an integer it can't convert a string containing a float to an integer directly! ::

    # This is OK
    int("3")

    # This is OK
    int(3.7)

    # This is not OK
    int("3.7") # This is a string representation of a float, not an integer!

    # You have to convert the string to a float first
    int(float("3.7"))

Values of type ``bool`` can contain the value ``True`` or ``False``.  These values are used extensively in conditional statements, which execute or do not execute parts of your program depending on some binary condition::

    my_flag = True

    if my_flag:
        print("Hello!")

The condition is often an expression which evaluates to a boolean value::

    if 3 > 4:
        print("This will not be printed.")

However, almost any value can implicitly be converted to a boolean if it is used in a statement like this::

    my_number = 3

    if my_number:
        print("My number is non-zero!")

This usually behaves in the way that you would expect: non-zero numbers are ``True`` values and zero is ``False``.  However, you should be careful when using strings -- the empty string is treated as ``False``, but any other string is ``True`` -- even ``"0"`` and ``"False"``! ::

    # bool is a function which converts values to booleans
    bool(34) # True
    bool(0) # False
    bool(1) # True

    bool("") # False
    bool("Jane") # True
    bool("0") # True!
    bool("False") # Also True!