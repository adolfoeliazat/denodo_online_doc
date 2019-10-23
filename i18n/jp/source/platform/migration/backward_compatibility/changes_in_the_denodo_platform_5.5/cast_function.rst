=============
CAST Function
=============

Starting with Denodo 5.5, the function ``CAST`` truncates the output
when converting a value to a text, when these two conditions are met:

#. You specify a length for the target data type
#. And, this length is lower than the length of the input value.

For example, ``CAST ("Denodo" AS VARCHAR(2))`` returns “De” because the
target type specifies a length lower than the length of the input value.

In Virtual DataPort 5.0, the behavior of the CAST function can be
altered to avoid truncating the value. In later versions, the behavior
of the CAST function cannot be altered and it always truncates strings
if necessary.
