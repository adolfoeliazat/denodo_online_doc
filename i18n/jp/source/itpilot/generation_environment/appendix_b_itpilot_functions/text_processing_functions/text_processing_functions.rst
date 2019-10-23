.. _itp_gen_environment_guide_text_processing_functions:

=========================
Text Processing Functions
=========================

Text processing functions have the objective of executing a
transformation or calculation on a text-type attribute or constant.


-  ``CONCAT``: The concatenation function receives a variable number of
   arguments and allows a ``string``-type element to be obtained as a
   result of concatenating its parameters. The infix version of this
   function receives two arguments and is represented by the symbol ‘\|\|’.

-  ``CONTEXTUALSUMMARY``: This function obtains a contextual summary of a
   text based on a keyword search. A series of text fragments containing
   the word or sentence specified is obtained. The function has the
   following signature:

   ``CONTEXTUALSUMMARY(content:string, keyword:string, [beginDelim:string,
   endDelim:string, fragmentSeparator:string, fragmentLength:int [,maxFragmentsNumber:int]])``
   
   , where:
   
   -  ``content``: text to analyze and the one from which the most relevant
      fragments are to be extracted (mandatory)
   -  ``keyword``: the keyword used to extract the text fragments
      (mandatory). The content of this argument can be a single word, or a
      sentence.
   -  ``beginDelim``: text to add as prefix of the keyword whenever it
      appears in the text (optional, default value is “”).
   -  ``endDelim``: text to add as suffix of the keyword whenever it
      appears in the text (optional, default value is “”).
   -  ``fragmentSeparator``: text to use as separator of the different text
      fragments obtained as a result (optional, default value is “…”)
   -  ``fragmentLength``: approximate number of characters that will appear
      before and after the keyword occurrences inside of the text
      (optional, default value is 5).
   -  ``maxFragmentNumber``: maximum number of fragments to retrieve.
   -  ``analyzer``: analyzer to use when performing the keywords search. By
      default, the Standard Analyzer (``std``) is used: this analyzer does
      not consider lemmatization or stopwords. Analyzers for English
      (``en``) and Spanish (``es``) are also included.

-  ``GETBYTES``: This function receives 2 ``string``-type arguments and
   returns the result of transforming the text received in the first
   argument to a byte array, using the encoding specified in the second
   argument. If the text to transform (first argument) is null then the
   function returns null. If the specified encoding is null (second
   argument), then the JVM default encoding will be used.

-  ``INDEXOF``: The ``INDEXOF`` function receives two ``string``-type
   parameters and returns the index of the first appearance of the second
   string inside the first string, or -1 if the second string is not
   contained inside the first. The return type is integer.

-  ``LEN``: The LEN function receives as a parameter a ``string``-type
   argument and returns the number of characters that form it.
   Alternatively, it accepts as parameter a binary-type argument and
   returns its size in bytes.

-  ``LOWER``: This function receives a ``string``-type argument and returns
   it to the output with all of its characters changed to lower case.

-  ``REGEXP``: This function allows for transformations on character
   strings based on regular expressions. It is given three arguments: one
   ``string``-type element, one input *regular* *expression* and one output
   *regular expression*. The regular expressions must be expressed using
   `Java Regular Expressions <https://docs.oracle.com/javase/8/docs/api/index.html?java/util/regex/Pattern.html>`_.
   The function behaves in the following manner: The input regular
   expression is assessed against the text from the first argument and the
   output regular expression may include the “groups” defined in the input
   regular expression. The portions of text matching them will be replaced
   in the output expression. For example, the result of evaluating:

   -  ``REGEXP('Shakespeare, William','(\\w+), (\\w+)','$2 $1')``

   will be "William Shakespeare".

-  ``REMOVEACCENTS``: This function receives a ``string``-type argument and
   returns that same argument value but with no accents.

-  ``REMOVEWHITESPACES``: This function receives a ``string``-type argument
   and returns that same argument value but with no blanks.

-  ``REPLACE``: This function receives 3 ``string``-type arguments and
   returns the result of replacing the occurrences of the second one in the
   first one by those of the third one.

-  ``SIMILARITY``: This function receives two character strings and returns
   a value between 0 and 1, which is an estimated measurement of similarity
   between the strings. The third ``string``-type parameter (optional)
   specifies the algorithm to use to calculate the similarity measurement.
   ITPilot includes the following algorithms (if no algorithm is specified,
   ITPilot chooses the one to apply):


   1. Based on the editing distance between the text strings:
   ``ScaledLevenshtein``, ``JaroWinkler``, ``Jaro``, ``Level2Jaro``,
   ``MongeElkan``, ``Level2MongeElkan``.

   2. Based on the appearance of common terms in the texts: ``TFIDF``,
      ``Jaccard``, ``UnsmoothedJS``.

   3. Combinations of both: ``JaroWinklerTFIDF``.

-  ``SPLIT``: The split function takes two string-type arguments. It splits
   the second argument around matches of the regular expression given as
   the first argument, and returns an array containing the generated
   substrings.

-  ``SUBSTRING``: The substring function receives as parameters a
   ``string``-type argument and two integer numbers. It returns as output
   the part of the substring of the first argument that corresponds to the
   positions indicated by the second (beginning) and third (end) arguments.
   The result string contains all the characters from the beginning up to
   the previous character to the end index: the end index marks the first
   character that is not included in the output.

-  ``TRIM``: This function receives a ``string``-type argument and returns
   the same argument with all the spaces and carriage returns removed from
   the beginning and the end of the string.

-  ``UPPER``: This function receives a ``string``-type argument and returns
   it to the output with all of its characters changed to upper case.
