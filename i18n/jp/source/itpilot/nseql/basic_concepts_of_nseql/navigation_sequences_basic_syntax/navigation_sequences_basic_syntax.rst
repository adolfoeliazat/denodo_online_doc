==================================
Navigation Sequences: Basic Syntax
==================================

The fundamental concept of NSEQL is *navigation sequence*. A navigation
sequence is comprised of a list of commands that are executed
sequentially on the interface of an Internet browser.



The basic syntax for specifying a *navigation sequence* is as follows:



*command-1;command-2;…;command-n;*

Each command has a name and can receive a list of parameters. The format
of each command is as follows:

*NAME( param1, param2, …, paramN)*

The alternative syntax is also allowed:

*NAME, param1, param2, …, paramN*



This alternative syntax is discouraged for general use because is less
clear than the parenthesis notation.



NSEQL syntax has the following special characters: ‘(’, ‘)’, ‘,’ and
‘;’.Ifa string parameter contains any of these characters,they have
to be escaped using a backslash (‘\\’). Thebackslash itself can also be
escaped, i.e.:“string with \\ symbol as part of content \\” should be
written as “string with \\\\ symbol as part of content \\\\”.

In addition, when an interpolation is performed, ‘{’, ‘}’, ‘(’, ‘)’,
‘,’, ‘^’ and ‘@’ have a special meaning. Ifthey are used without that
meaning (i.e. to use them as part of an attribute name, or so that ‘@’
does not refer to a variable), they must be escapedwithabackslash
(‘\\’).

Following, there are someexamples of how to escapethese special
characters:

-  @ => \\@
-  ^ => \\^
-  { => \\{
-  } => \\}
-  ( => \\\\(
-  ) => \\\\)
-  \\ => \\\\\\\\
-  , => \\\\,
-  ; => \\\\;

Some commands can receive optional parameters. These parameters can be
of two types: those that have to be present, but can take the “\_NULL\_”
in case of not wanting to specify a value for the parameter (e.g., the
parameter “name” in the commands SetInputValue, SetTextAreaValue,
SelectIndexByText, SelectIndexByPosition, SelectIndexByValue and
ClickOnElement), and those where the parameter can be omitted (for
example the “timeout” parameter of the FindElementByXXX commands).



The input parameters cannot be neither preceded nor followed by blanks.



Some commands accept text as input parameters that represent search
strings; these can be compared in equality or containment operations. In
those cases, NSEQL allows the use of not only literals, but also regular
expressions. Their use makes NSEQL look for page patterns that match the
`Java regular expressions <https://docs.oracle.com/javase/8/docs/api/index.html?java/util/regex/Pattern.html>`_.