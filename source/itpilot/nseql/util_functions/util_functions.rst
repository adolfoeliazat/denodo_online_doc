==============
Util Functions
==============

ITPilot provides a set of functions that can be used in combination with
the discussed NSEQL commands, as parameters of said commands. These
functions are specified by the character ´^´ followed by the name of the
function.

.. contents::
   :depth: 1
   :local:
   :backlinks: none

EncodeSEQ
=========

This function accepts a character string as input, and returns that same
string but with the appropriate encoding for its adequate behavior in
NSEQL (mainly, character escaping)



For instance, a variable ``@VARIABLE`` is going to be used with value
“\john@acme.com”. Since the “@” symbol is used by NSEQL as a reserved
word, writing the value of ``@VARIABLE`` can provoke an error. In
situations like this, the following should be used:
``^EncodeSeq(@VARIABLE)``.


