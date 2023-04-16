## Module 1 - Workflow Automation and Productivity
Written by: Jason Wong

One of the most prominent use cases of bash scripting is the ability to wrap several commands into one script. **After all, command-line bash is the same thing as scripting bash. Virtually anything you can do via the bash terminal can be done in a bash script.** For example, if you are working in Java on the command line, the compile and run commands can be added to a script so the program can be compiled and run in a single command. This module will explain some of `bash`’s basic syntax and cover how to convert a command-line workflow into a script.


```danger
```
Before diving into automation, we need to explore some of the basics of bash. Let’s go back to the Java example for a moment. To compile and run a program called `myProgram`, the following commands could be used:

```bash
javac myProgram.java
java myProgram
```

```note
__Bash scripts are essentially a collection of bash commands tied together with various control flow structures, variables, and specialized functions.__ In other words, the simplest bash script is simply a shebang (which will be covered later in the module) followed by a series of bash commands–the same exact ones that you run on the command line. So, a script to compile and run the previous Java program would look like this:
```

```bash
#!/bin/bash

javac myProgram.java
java myProgram
```

Of course, this script may not save that much time since the two commands are very simple. But, imagine if you had a series of five or even 10 commands to run, each with its own set of options. Then, it becomes easier to appreciate how bash scripting could save lots of time and effort in the long run. A complex example that would benefit from bash scripting is included at the end of this module.


### The Shebang
You may have noticed the strange comment on the first line in the Java script. This line, `#!/bin/bash`, is referred to as the shebang. It tells the interpreter to run that file as a bash script (as opposed to other interpreters like sh), making it an important component in every script you write. Shebangs can be used with virtually any interpreter–just specify the location of the interpreter in place of the `/bin/bash` portion of the shebang.

### Executing the Script
After writing and saving the script, the next step is to test it out. Bash scripts kind of act like bash commands–all you need to do is type the name of the script into a bash terminal and voilà, it runs… kind of. 

```tip
When the filename is typed, by default, bash searches a specific set of file paths for that filename, which usually does not include the current directory. Therefore, the name of the script needs to be prefixed by a `./` to force bash to look only in the current directory for the script. Running the script using the `./` prefix should result in a permissions error. To solve this, execute the command `chmod +x myScript.sh`, replacing `myScript.sh` with the name of your script. This tells the operating system to allow users to execute `myScript.sh`.
```

For curious users, that set of file paths where bash by default assumes executables to be stored is defined in the `$PATH` variable. Adding your script to `$PATH` allows you to execute it from any location on your computer. More information about `$PATH` and adding file paths can be found [here](https://linuxize.com/post/how-to-add-directory-to-path-in-linux/).

### Variables
Like any good programming or scripting language, bash supports the use of variables, which greatly expands the possibilities of bash scripting. However, for users familiar with Python, Java, or C, the syntax for bash variables looks slightly different. Variables are created and assigned using the equals operator (`=`) like in the following.

```bash
# define a variable called val and give it with the value of 1000
val=1000
```

However, the way that the value is retrieved may look a bit strange, as the variable name is preceded by a `$` sign to indicate to the interpreter that we are reading the variable. For example, `$val `retrieves the value stored in the variable `val`. Variables can be used in commands, like `echo "$val"` (we will explore this more in Module 3).

There are also some special variables that can be used in a bash script. The most useful ones are as follows:

* `$0` → Stores the name of the script (the 0th argument)
* `$1`, `$2`, `$3`, etc. → Each variable stores a different command-line arguments
* `$?` → Stores the exit status of the last run command within the script
* `$HOME` → Stores the path of the user’s home directory (see Concatenating Strings example)

```danger
```
Here is an example of how each variable might be used. Notice that each variable deals with obtaining external information, rather than information from within the script.

```bash
echo “Running $0 with arguments $1 and $2...”
cd “$HOME/Downloads”
echo “cd resulted in $? (0 for success, 1 for failure)”
```

This script simply prints out the script’s name and arguments before switching directories to the user’s downloads directory. Then, it reports if it was successful in switching directories. For now, don’t worry about the specifics of how we combine the variables with other text as this will be explored later in Module 3 - Concatenating Strings.

For curious learners, a longer list of operations that can be performed on variables can be found on a tutorial site [here](https://ryanstutorials.net/bash-scripting-tutorial/bash-variables.php).

### Loops and Logic

#### `if` Statements
Let’s augment the previous example, which reported if a particular `cd` command was successful. In that script, we relied on the user to know what the exit codes mean. But, what if we could simply print out `success` or `failure` to make it easier for the user? To accomplish this, we need some sort of logic structure. Fortunately, bash provides `if` statements, `for` loops, and `while` loops. In our example, the `if` statement makes the most sense. If the exit code is `0`, then print `success`.

The basic syntax of an if statement is:
```bash
# Basic if
if [ <condition> ]
then 
	<commands>
fi

# Basic if-elif
if [ <condition> ]
then 
	<commands>
elif [ <other condition> ]
then
	<other commands>
fi

# if-else
if [ <condition> ]
then 
	<commands>
else
	<other commands>
fi
```
An easy way to understand if statements is that they take the form of “if <condition> is true, do these commands.” Else-if statements mean “if <condition> is true, do these statements, otherwise, check if <other condition> is true and do those statements.” Else statements mean that “if <condition> is true, do these commands, but if it is not true, do these other commands.”

An easy way to understand `if` statements is that they take the form of “if <condition> is true, do these commands.” `Else-if` statements mean “if <condition> is true, do these statements, otherwise, check if <other condition> is true and do those statements.” Else statements mean that “if <condition> is true, do these commands, but if it is not true, do these other commands.”

```danger
```
In our example, we would write the following to print a user-friendly status message:
```bash
if [ $? = 0 ]
then
	echo "success"
else
	echo "failure"
fi
```

In this case, we are using the `=` operator to check for equality. However, there are many other operators that can be used, and many of them are unlike anything found in popular languages like Java or Python.

```bash
!<expression> # Negates the expression

<string1> = <string2> # Checks for string equality
<string1> != <string2> # Checks for string inequality

<integer1> -eq <integer2> # Checks for numerical equality
<integer1> -ne <integer2> # Checks for numerical inequality

<integer1> -gt <integer2> # Similar to the ">" operator
<integer1> -lt <integer2> # Similar to the "<" operator
<integer1> -ge <integer2> # Similar to the ">=" operator
<integer1> -le <integer2> # Similar to the "<=" operator
```

#### `for` Loops
Another common programming construct is the `for` loop. These, like in other languages, are used to repeat a set of statements for a particular number of times. The basic syntax to iterate for a set number of times is as follows:

```danger
```

```bash
# Basic syntax
for <counterVar> in {1..<numberOfLoops>};
do
<command>;
done

# Concrete example (prints "hello world" 10 times)
for i in {1..10};
do
	echo "hello world";
done
```

We can also iterate over items in a list. In the following example, the usage of the variable `varName` becomes more clear.

```bash
# Basic syntax
for <varName> in <item1> <item2> <item3>;
do
	<command>
done

# Concrete example (prints each item)
for item in a b c;
do
	echo "Item: $item"
done
```

For loops can be used to apply an operation to every file in a directory, attempt a command a particular number of times, or anything else that needs to be repeated multiple times.

#### `while` Loops

The last important looping structure is the `while` loop. `while` loops are used to execute a set of statements until a given condition is true. This differs from `for` loops, which are used to execute a set of statements a predetermined number of times, or for each element in a set. `while` loops are often used to retry commands until they succeed or to keep prompting a user for valid input until they enter the correct input. Like `if` statements, they use bracket notation to represent the conditional. Additionally, the same operators can be used.

```danger
```

```bash
# Basic syntax
while [ <condition> ]
do
	<commands>
done

# Concrete example
ssh hostname@remote # Remote machine to connect via ssh into
while [ $? -ne 0 ]
do
	echo "Failed to connect. Retrying..."
ssh hostname@remote
done
```

Additional examples of while loops can be found on The Linux Documentation Project’s [website](https://tldp.org/LDP/Bash-Beginners-Guide/html/sect_09_02.html). This resource shows several examples of where `while` loops could be used. Although the examples are more complex than what was covered in this module, they are great examples of real-world use cases of `while` loops.

### Workflow Automation
Workflow Automation
Now that we’re familiar with the basic syntax of `bash`, let’s explore how it can be used to automate various tasks.

#### Code Editor Launcher
Let’s say that you’re working on a Python project stored in `$HOME/projects/python/code`. It’s a hassle to have to navigate to that directory and source the virtual environment, so you choose to write a script to accomplish that. Here’s what it might look like.

```danger
```

```bash
#!/bin/bash

cd $HOME/projects/python/code # In the Variables section, we learned what the $HOME environment variable is

# We learned how to use if statements with conditionals earlier in this module
if [ $? -ne 0 ]
then
	echo "Directory $HOME/projects/python/code does not exist"
else
	echo "Successfully navigated to project!"
fi

# Remember, we can execute any bash command simply by writing it in the script. Here, we are running the source command to source the virtual environment
source env/bin/activate

# Let's spawn another terminal instance to use to run our code in without closing our editor. Replace "konsole" with the name of your preferred terminal application.
konsole . &

# Finally, let's open the code in an editor
nano main.py
```

Now, your code is open in an editor, and you have a separate terminal window to run it in!

#### Automatic GitHub Committer
Here’s another example. Adding, committing, and pushing files to GitHub can be annoying and repetitive. Let’s make a script that takes the commit message as a command-line argument, creates a commit containing all changed files, and pushes it.

```bash
#!/bin/bash

# We learned how to use the $1 variable to access the first command-line argument
echo "Commit message: $1"
# Use the $1 variable as the commit message
git commit -m $1

git push
while [ $? -ne 0 ] # We learned how to repeatedly try a command until successful
do
	sleep 5 # Wait 5 seconds for the user to resolve the issue
	git push
done
```

We encourage you to try and augment this example to ask for user confirmation before pushing. Additionally, you could use arguments to pass in the files that you want to commit to experiment more with command-line arguments in `bash`.

### Ideas for Automation
Possible ideas for workflows to automate include (in order of estimated complexity):

* A script to launch your favorite applications
    * Great if you have a particular set of applications that you always have open, like Spotify, Firefox, and VSCode.
    * You can even play around with having the [script execute on login](https://stackabuse.com/how-to-run-a-bash-script-on-login/) to automatically launch your applications.
* A manual backup script using a tool like Back In Time or BorgBackup
    * [Back In Time](https://backintime.readthedocs.io/en/latest/)
    * [BorgBackup](https://borgbackup.readthedocs.io/en/stable/)
    * You may consider using `if` statements to check previous commands’ output to detect any failures.
* A script to download and configure a development environment, like VSCode
    * An example can be found in this StackOverflow [post](https://codereview.stackexchange.com/questions/249244/bash-script-to-automate-dev-environment-setup).
    * This can be as basic as automatically installing your favorite packages, like Chromium, VSCode, and more.

### Diving Deeper

The topics that you saw in this module are just an introduction to the syntax of bash. There are many other resources that cover the same topics in more detail. We encourage you to browse through these outside resources, especially if you feel comfortable with the topics presented in this module.

```warning
[C-style `for` loops (cyberciti.biz)](https://www.cyberciti.biz/faq/linux-unix-applesox-bsd-bash-cstyle-for-loop/)
* If you are familiar with the C programming language or its relatives, you may be more comfortable working with a C-style `for` loop. Luckily, bash has you covered.
```
```warning
[Case statements](https://linuxize.com/post/bash-case-statement/)
* Case statements are similar to a series of if statements. They check for equality against a number of cases. Then, the code in the matching case is run.
```warning
[What are Environment Variables (opensource.com)](https://opensource.com/article/19/8/what-are-environment-variables)
* Environment variables are variables that persist throughout the lifetime of a particular shell instance.
* You can think of them as global variables that live outside of any script.
* Normally environment variables are used to store important persistent information, like installation directories, information about the system, and more.
* You can also define custom environment variables to suit your needs.
```
```warning
[Additional Conditional Expressions (Bash Reference Manual)](https://www.gnu.org/software/bash/manual/html_node/Bash-Conditional-Expressions.html)
* There are many different conditional expressions other than the simple equals or greater than that could be used in logic structures. In particular, there are a lot of expressions that pertain to file operations, which can be used to automate a lot of different tasks.
```
```warning
[Command Substitution (Bash Reference Manual)](https://www.gnu.org/savannah-checkouts/gnu/bash/manual/bash.html#Command-Substitution)
* Command substitution allows you to use the output of commands in your scripts. It works by running the command and then replacing the substitution with the output of that command.
```