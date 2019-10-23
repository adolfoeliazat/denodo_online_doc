===============================
Regular Expressions for Filters
===============================

A regular expression is a text model formed of ordinary characters (for
example, letters from *a* to *z*) and special characters known as
*metacharacters*. The model describes one or several strings that
coincide when searching a text body. The regular expression is used as a
template to collate a model of characters with the string being
searched.

 

The table below includes a complete list of metacharacters and their
behavior in the context of the regular expressions:

 
.. table:: Metacharacters
   :name: Metacharacters

   +--------------------------------------+--------------------------------------+
   |   Expression                         |   Matches                            |
   +======================================+======================================+
   | X                                    | The character x                      |
   +--------------------------------------+--------------------------------------+
   | \\\                                  | The character \\                     |
   +--------------------------------------+--------------------------------------+
   | [abc]                                | a, b, or c                           |
   +--------------------------------------+--------------------------------------+
   | [^abc]                               | Any character except a, b, or c      |
   |                                      | (negation)                           |
   +--------------------------------------+--------------------------------------+
   | [A-Z]                                | From A to Z inclusive                |
   +--------------------------------------+--------------------------------------+
   | .                                    | Any character                        |
   +--------------------------------------+--------------------------------------+
   | ^                                    | Line start                           |
   +--------------------------------------+--------------------------------------+
   | $                                    | Line end                             |
   +--------------------------------------+--------------------------------------+
   | X?                                   | X, once or never                     |
   +--------------------------------------+--------------------------------------+
   | X\*                                  | X, zero or more times                |
   +--------------------------------------+--------------------------------------+
   | X+                                   | X, once or more times                |
   +--------------------------------------+--------------------------------------+
   | XY                                   | X followed by Y                      |
   +--------------------------------------+--------------------------------------+
   | X\|Y                                 | X or Y                               |
   +--------------------------------------+--------------------------------------+
   | \(X\)                                | X, as a group                        |
   +--------------------------------------+--------------------------------------+


Groups are enumerated by counting the brackets open from left to right.
The zero group refers to the complete expression.

 

The ``\`` character can be used to escape the metacharacters used in
the expressions. For instance, the ``\\`` expression represents a
single ``\,`` and ``\(`` represents a parenthesis.
