## Module 2 - Text Parsing and Manipulation
Written by: Caleb Rollins

One core use case of bash scripting deals with reading and writing to special types of variables called strings. Strings are simply a collection or arrangement of characters with some type of meaning to the programmer depending on the task at hand. Let's first look at how we create strings inside of the bash environment.

### Your First String

```danger
```
To declare a new string variable `str` with the value `helloworld` you can write to the terminal or a script file (remember there is usually no difference) `str="helloworld"` <i>almost</i> like we did earlier for a variable with an integer value. To verify that our new string variable was correctly set, we can check its value by printing it out with the bash command `echo`. When you issue the command `echo "$str"`, you should see `helloworld` printed to the terminal. This is great, we just made our first string.

You may have noticed when we used the string variable with `echo` above, that quotation marks were used. In bash, quotes indicate that the contents should be treated as a single item. Double quotes, in addition to grouping contents, allow for variable substitution. Single quotes, on the other hand, tell the interpreter to treat each character literally.

```bash
$ echo “$str” # all characters are one unit; variable substitution
helloworld
$ echo ‘$str’ # look at individual characters; treat as literal
$str
```

### Concatenating Strings
Now that we know how to create strings, what do we do if we want to join two strings together? Imagine you are creating a program to greet the user using a workplace computer. Although you could create a separate string variable for each user that could possibly login to the system, this would be inefficient and a real hassle for a large organization that has hundreds of users. 

Instead you could use concatenate (fancy word for join) two string variables together. Earlier in Module 1 concatenating was employed, but we did some magic hand waving. We could create a variable for the message called  `msg` with the value `Welcome and enjoy your time, `and another variable that would be appended to the message such as the username. In bash, many prefilled variables [already exist for your use](https://opensource.com/article/19/8/what-are-environment-variables) (see resource for more information) and we already saw the use of one of these special variables in Module 1 called `$HOME`.We will utilize another variable called `$USER`.

```note
For now, don’t worry about how this variable is set, but just know that it has the current user’s username and can be treated just like any string variable you have created earlier. 
```

```danger
```
To write out our complete login message we would once again `echo` but now with each variable directly after each other in the intended order.
```bash
$ echo "$msg$USER"
Welcome and enjoy your time, ccrollin
```
Wow, that is so nice that you can greet each user personally without having to create individual string variables for all the users. The modular nature of string concatenation illustrated makes it a cornerstone tool to generalize your bash scripts. Other more complex operations of concatenation do exist in bash and if you want to explore more [checkout this resource](https://linuxize.com/post/bash-concatenate-strings/#concatenating-strings-with-the--operator).


### The Length of a String
Oftentimes we want to get the length of a string variable in order to influence the logic of our program. In bash there are multiple ways to reference the value of a string variable. For earlier sections of this module, we have used the “`$strvar"` syntax, when we are referencing the value stored inside a string variable. While this syntax suffices for basic operations like reading and concatenating, for other operations like getting the string length we must use a new syntax called expansion syntax. 

Expansion syntax adds curly branches to the outside of the variable name such that the equivalent of `"$strvar"` in expansion syntax would be “`${strvar}"`. It is important to note that expansion syntax can do everything our basic syntax can but is just more fully featured. Now that we know what expansion syntax is, we can use its built-in `#` operator to get the string length such that the command `echo "${#strvar}"` would print a number to the terminal.

You might be asking, why would someone want to know the length of a string? Think about a program that makes users create a password with a minimum number of characters. Since the user can input varied text each time the program runs, the idea of checking the string length as we did above would be ideal to see if a user gave a password that was too short.


### Getting Parts of a String
In some instances, we want to only get portions of a string variable that are applicable to the programmers intentions at a certain time. Say we declared a string variable called `classname` with the formal name of this class. It is feasible that we might want to isolate certain parts of the `classname` into other variables such as `coursecode`, `section`, and `title` to do separate processing on. These smaller components that make up a complete string are called substrings. Our goal can be accomplished by choosing reference points within the string where each substring will be delineated and divided from.

```tip
Often in programming languages (including bash)  these reference points are referred to as indexes or positions. In bash, strings are indexed by incrementally counting starting from 1 as you read the string from left to right. See the example below.
```

```danger
```

```bash
$ classname="ENG 331, 301, Communication for Engineering and Technology"
# character that corresponds with index 1 = 'E'
# character that corresponds with index 2 = 'N'
# character that corresponds with index 3 = 'G'
# character that corresponds with index 4 = ' '
# character that corresponds with index 5 = '3'
# ... (this would continue till end of string reached)
```

Now that we have shown how indexing works for strings in bash, we are ready to get substrings of a string using the expansion syntax used previously with some additions. Here we supply our string variable, the starting index of the new substring, and the desired length of the new substring - “`${classname:index:length}"` where `index` and `length` are placeholders for numbers the programmer will decide on depending on the substring to extract. 

```warning
There are [other substring syntaxes](https://earthly.dev/blog/bash-string/#bash-substring) we did not mention that are more condensed that you can check out if interested. In this section, we focused on substring extraction that is derived from a base string, but there are also ways to [replace](https://earthly.dev/blog/bash-string/#bash-string-replace) and [delete](https://earthly.dev/blog/bash-string/#bash-last-character) from the base string to create a substring, which might be worth exploring if you want to do more work with substrings.
```

### Reading a String from the User

There are instances where we want to write a script or issue commands that respond in a very specific way depending on user input. This usually means that input from the terminal is stored inside a string variable and is accessed later for more processing. Lucky for us, there is a bash built-in command that we can utilize called `read` that sets a variable to an input value. To read into a variable `myvar` and print out the value of `myvar` you can use the following bash commands.

```bash
$ read myvar
$ echo "$myvar"
```

```danger
```
With our knowledge of `if` statements from Module 1, let's see the power of reading user input to generate a dynamic response. In this example, we will create a script that takes as input a message for a social media post and reports if the length of the message has reached the word limit. Use the comments alongside the code snippet to follow along with what this script is doing.

```bash
#!/bin/bash

read message
# message now is set to whatever the user typed in

len="${#message}"
# len now stores the length of the message string

# if the length of the message is greater than or equal to 500...
if [ $len -ge 500  ]
then
     # then let the user know they are over the word limit
    	echo "You reached the word limit of over 500"
else
     # else let the user know they are under the word limit
    	echo "You have not reached the word limit. Yay!"
fi
```

```warning
While most of the time you will probably want your bash script to treat user input as a string, there could be instances where you seek to interpret the input as an integer or another data type so you can conduct further checks. The conversion from string to integer for example is a topic outside the scope of this document, but if you want to learn more check out [this article](https://linuxhandbook.com/bash-convert-string-to-number/).
```

```warning
If you wanted to read in user input or even file input until a condition was met (like the end of the file was reached), you could utilize a special construction of a `while` loop (discussed in Module 1) with the condition being the result of `read`. These types of loops are often used in programming when reading in large amounts of data in batches and are aptly named read-while loops. To get more information about implementing read-while loops in the context of bash scripting check out [this article](https://linuxhint.com/while_read_line_bash/).
```

### Ideas for Practice

Practicing the concepts introduced in this module are just as important as the initial uptake of the information introduced. Below are a set of problem statements for your consideration if you want more hands-on exposure to the topics in this module. 

For ease of use, we recommend using _[myCompiler](https://www.mycompiler.io/new/bash)_, an online development website that lets you write bash, immediately run your code, and see output without having to worry about the details required to configure your own machine. You are by all means welcome to not use this tool, if you already have Linux and bash installed.
* Write a bash script that outputs a string once with and once without introducing a variable
* Write a bash script to read in from the terminal and print out the input twice using concatenation
* Write a bash script to check if the length of a string is even or odd
* Write a bash script to read in a line and print out each character of the input on a new line


### Diving Deeper

The topics you saw in this module are just the tip of the iceberg when it comes to the power of text manipulation and parsing inside of bash. While we primarily focused on built-in operations unique to bash, there are a multitude of external programs at your disposal that are more fully featured. Below are links to resources where you can learn about each.

* [`sed`](https://www.howtogeek.com/666395/how-to-use-the-sed-command-on-linux/) and [`awk`](https://tldp.org/LDP/abs/html/awk.html#AWKREF)
    * <code>sed</code> stands for stream (usually input from the terminal) editor. It uses a basic format syntax to specify what you want to do. <code>sed</code> is most often used when searching for a substring contained inside a base string and when performing substring substitution (find and replace).
    * <code>awk</code> is a fully featured text manipulation programmable interface that allows you to have conditionals, loops, arrays, and so much more. <code>awk</code> extends the functionality of <code>sed</code> while cleaning up the often messy syntax from <code>sed</code>
* [`grep`](https://www.geeksforgeeks.org/grep-command-in-unixlinux/)
    * grep can be used for more advanced substring and pattern matching for an input string or even an input file that contains lots of text
    * grep allows for the use of [regular expressions](https://www.computerhope.com/jargon/r/regex.htm) to find certain types of text that follows a general structure (like phone numbers, email addresses, or website addresses)
* [`expand` and `unexpand`](https://linuxjourney.com/lesson/expand-unexpand-command)
    * to increase readability, it is best to use spaces instead of tab characters in text files (especially for programs)
    * the <code>expand</code> command will take input and replace the tab characters with spaces
    * the <code>unexpand</code> command will undo tab expansion (ie. groups of spaces are turned into tab characters)
* [`sort`](https://linuxjourney.com/lesson/sort-command)
    * Have you ever had a list of names, but wanted to get them alphabetically?
    * <code>sort</code> will take input from either a file or the terminal and sort each line alphabetically as if each line was an entry in the dictionary
* [`translate`](https://linuxjourney.com/lesson/tr-translate-command)
    * <code>translate</code> will convert one range of values to another parallel range of values
    * In the example linked above, <code>translate</code> is used to change lowercase letters to uppercase letters. 
* [`uniq`](https://linuxjourney.com/lesson/uniq-unique-command)
    * Say you wanted to read all the names of students that were in a high school class, but you only want to see one instance of each name (ie. no duplicates)
    * <code>uniq</code> will read from a file stored on the computer or terminal input and output only the lines that are unique.
* [`wc` and `nl`](https://linuxjourney.com/lesson/nl-wc-command)
    * <code>wc</code> stands for word count and can give you just that for a file or terminal text input
    * <code>nl</code> stands new lines and will output the number of lines that are in a file or in terminal text input