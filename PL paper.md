## Description of Function Annotations
### as descriped in PEP 3107

While there significant dissagreementa about the usefulness of Funciton Annotations as described in PEP 3107 whith some people shurgging it off as insignificant
>I still think function annotations are a “meh” feature of Python 3. Without a clear definition in the PEP of how to use them
>-Andrew Montalenti, Aug 16,2014 [7] 

While others think this ability greatly improves the ones ability to write effective code.
>Coming from an academic background, I can tell you that annotations have proved themselves invaluable for enabling smart static analyzers for languages like Java.
>-Username: Uri Jun 14,2010 [8]

The opponents to the feature seem complain that they don't know how to use it, however it seems the expressed pupose of this change was to create a vehicle for community creativity in how they use this standardized annotation system[1]. However the syntax chosen does leave it rather open ended

The annotations are created by placing a colon after a parameter followed by a valid expression. The parameter – expression pairs are stored in a map called func_annotations (now just ''''__annotations__'''' since python 3.2), with the parameter being the key.

````python
def function(a:annotaion,b:annotation)->returnval:
	return a+b
````

to access the annotation simply reference the map method 

````python
functionName.func_annotations['paramName'] #now functionName.__annotations__['paramName']
````

There is no restriction on the annotation, as long as it is a valid expression. A given annotation can be of any data type but must be a literal.  It will accept a variable but that variable will be evaluated and that value will be used not the variable.  In addition to being of a data type the annotation can itself also be a type itself.  That is the annotation can, for example, have the values “int”,”str”, or “bool” as shown below

The parameters keep the same restrictions as before with one exception.  The string “return” is a protected value for parameter names as it is the map key for the return annotation. If there are no annotations the func_annotations map is empty, the same thing goes for lambda functions which do not utilize the “def” keyword.

While this opens up the opportunity for many things such as query mapping, adapter patterns, IDE readability and analysis, foreign language communication, and predicate logic, the most prevelant use is for type checking. Which can be done using a simple ```isinstance() ``` call 
	
````python
def function(a:int,b:str,c:bool)->returnval:
	for key val in function.func_annotations:
		if(isinstance(key,val)):
			#do something
		else:
			return TypeError
````
Consequently type checking and automation of the process opens the door for function overloading by means of type, which allows you to limit the type of parameters more effectively.

### grammar
python 2 grammar
````python
decorated      ::=  decorators (classdef | funcdef)						
decorators     ::=  decorator+	
decorator      ::=  "@" dotted_name ["(" [argument_list [","]] ")"] NEWLINE	
funcdef        ::=  "def" funcname "(" [parameter_list] ")" ":" suite
dotted_name    ::=  identifier ("." identifier)*								
parameter_list ::=  (defparameter ",")*
                    (  "*" identifier ["," "**" identifier]
                    | "**" identifier
                    | defparameter [","] )
defparameter   ::=  parameter ["=" expression]
sublist        ::=  parameter ("," parameter)* [","]
parameter      ::=  identifier | "(" sublist ")"
funcname       ::=  identifier
````
python 3 grammar
````python
decorators	   ::=  '@' dotted_name [ '(' [arglist] ')' ] NEWLINE
decorator	   ::=  decorator+
funcdef		   ::=  [decorators] 'def' NAME parameters ['->' test] ':' suite
parameters	   ::= '(' [typedargslist] ')'
typedargslist  ::= ((tfpdef ['=' test] ',')*
                ('*' [tname] (',' tname ['=' test])* [',' '**' tname]
                 | '**' tname)
                | tfpdef ['=' test] (',' tfpdef ['=' test])* [','])
tname		   ::= NAME [':' test]
tfpdef		   ::= tname | '(' tfplist ')'
tfplist		   ::= tfpdef (',' tfpdef)* [',']
````

The new grammar maintains the decorator functionality same as before which will become essential for type checking and function overloading as it allows for automation. However the funcdef has changed and the paramater_list has been replaced with typedargslist which is defined as a paramerater with the option of a default values (defined in arglist by the ['=' test]) and the optional annotation (defined in tname)

[8] Uri. "What Are Good Python Uses for Python3's Function Annotations?" Stack Overflow. 14 Jun. 2010. Web. h<ttp://stackoverflow.com/questions/3038033/what-are-good-uses-for-python3s-function-annotations>