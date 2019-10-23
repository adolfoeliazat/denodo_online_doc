=======================================
Functions for Handling Compressed Files
=======================================

-  ``UNZIPFILE``: This function can be used to unzip ZIP or GZ files. It
   receives two arguments:

   -  As first argument it receives a ``string``-type value that can
      represent a path to a file or a path to a directory. If it represents
      a path to a file, then this file will be uncompressed. If it
      represents a directory, then all the files inside that directory will
      be uncompressed.
   -  As second argument it receives a ``string``-type value with the path
      to the directory where the input file (or input files) must be
      uncompressed. If this second parameter is left empty: when the first
      parameter is a file, then its parent directory is used as the output
      directory, and when the first parameter is a directory, then it is
      also used as the output directory.

   The result is a ``string``-type value with the path to the output
   directory or an error message if any problem is found while
   uncompressing the input file. When the first input parameter is a path
   to a directory, the function result only reflects the result of
   uncompressing the last file of the directory.
