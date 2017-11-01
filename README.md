# Pyon
Pyon is an extended Python interpreter inspired by ARBLE (A Rather Brief Lua Extension) by @ATaco. It is very inefficient compared to ARBLE because of the differences between Python and Lua's semantics, but in principle it serves the same purpose, which is to make a production language better suited for golfing purposes.

# Features

### Partial Evaluation

Pyon uses partial evaluation to assign values to variables that were not defined.

`print(a)` would normally result in a `NameError`; however, this error is caught during interpreting and a line is added before the code which assigns the input / argument to `a`. It repeats this until `NameError`s are no longer thrown.

If command line arguments are passed without flags, they are read as inputs. They will be fed to the program in order until exhausted, after which the program will being taking input from STDIN. Should this result in an `EOFError`, remaining variables will be set to `0`.

The behaviour changes when the undefined variable name can be imported.

### Automatic Import

If a variable is used but was not defined and the name is a valid package, it will be automatically imported.

### Automatic Bracket Completion

Pyon will complete brackets and quotation marks at the end of the program if they are missing. If a bracket is opened and then another type of close-bracket is encountered before that bracket is closed, it will be automatically closed before the second one, so `print([1)` becomes `print([1])`. Quotation marks are not automatically closed when close-brackets are encountered because that would be dumb. If a close-bracket is encountered without a matching open-bracket, it is placed immediately after the most recent open-bracket whose close-bracket is after it, so `print([1]2])` would become `print([[1]2])` (which is still not syntactically sound, but is just for demonstration), whereas `print(1])` becomes `print([1])`.

### Automatic Attribute Name Completion

If the program references a non-existent attribute of an object, it will check the object's type and try to find an attribute that matches the given string as a prefix, with a shorter match being better (higher concentration of correct characters).

### Macros

Since `$` doesn't mean anything in Python otherwise, macros can be defined this way. Define a macro using `$<identifier> ?<code>` and incorporate it with `$<identifier>`. Note that `<code>` acts like a string and backslashes should be used to escape characters. It is evaluated by placing `"""..."""` around it. This also allowed code on multiple lines in a macro. Note that an identifier must consist only of `A-Za-z_`. Note that macros cannot be used at the beginning of lines because that is how they are defined. If there is a space between the name of a called macro and the next character, that space will be removed.
