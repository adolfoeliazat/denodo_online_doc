==========================
Escape Characters in DEXTL
==========================

Two different string types can be distinguished in DEXTL programs:



-  The ones that interpolate variable values with contained variables
   (@VAR) or functions (see section :ref:`Format Tags`). These strings are
   evaluated when analyzing the DEXTL program, and include the text
   delimiters appearing in the patterns, as well as the fixed-tag values
   (section :ref:`Attributes with FIXED value and default value`).
-  The ones that do not interpolate variable values. These are the rest
   of fixed strings appearing in the specification.



The string delimiter is the double quotation mark, ‘ " ‘. Therefore, if
it is necessary for this character to appear as element of any string,
it must be escaped. This is performed by means of the escape character
"\\".



The escape character must be also escaped if it has to be used as a
string character. Thus, "string with \\ symbol as part of content \\"
should be written as "string with \\\\ symbol as part of content \\\\".



When an interpolation is performed, characters ‘{‘ , ‘ } ‘ and ‘ @ ‘
have a special meaning. If their use is required without that meaning
(i.e. to use them as part of an attribute name, or so that ‘@’ does not
refer to a variable), they must be escaped. Again, the escape character
is "\\", which must also be escaped when its use is not that of an
escape character. For the sake of clarity, some examples are shown about
how to write the following interpolable strings:



-  @ => \\\\@
-  ^ => \\\\^
-  { => \\\\{
-  } => \\\\}
-  \\ => \\\\\\\\
-  " => \\"

