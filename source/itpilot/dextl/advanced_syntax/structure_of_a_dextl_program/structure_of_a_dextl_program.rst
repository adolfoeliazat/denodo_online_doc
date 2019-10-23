============================
Structure of a DEXTL Program
============================

DEXTL programs are divided into two sections that are not physically
separated by any element, but that should appear in the order indicated
below:

-  Declarations section
-  Specification section

Declarations Section
====================

The following elements are included in this section:

#. Name of the Lexical Analyzer (or scanner) to be used. This is
   mandatory and should appear at the beginning of the DEXTL program.
   The file will be included with the desired scanner (see section :ref:`Scanners` for a description of the available scanners and to know
   how to create new scanners). This is indicated with:
   
   #include “scanners/name_scanner”

   The same scanner can be used for all the specifications that share the same tagsets.
   
   
#. Name of the skeleton used to generate the scanner corresponding to
   the specification. The specifications generator tool adds to the
   DEXTL program:
   
   LEXERTYPE="skeleton"
   
#. Name of the default tagset for the patterns, where none is specified.

   DEFAULTSET= "Name set"

#. Files included. Likewise, the specification generation tool will
   indicate in the DEXTL program what other external programs are
   included. The effect will be the same as if the content of this file
   were in the current file, where the include is found. The syntax is:
   
   #include "Name file"
   
#. Attributes with default value. These have a fixed value for all the
   items/results found with the specification, and we will call them
   attributes with fixed value:
   
   NAME[CONTEXT]="VALUE"

   Where the context is optional. This context must not be mistaken with the navigation context, but with data associated with this field that is communicated together with the value associated with it.

   It is possible to define an attribute with fixed value that can be used later in the program, but where its value is not to be communicated to the application. In this case, the format is

   ::NAME[CONTEXT]="VALUE"

   It is possible to assign several values (multivalued value) to an attribute with fixed value:

   NAME [CONTEXT] = {"VALUE1",”VALUE2”,...}
      
#. Tagsets to be used. Format:

   SET NAME
   
     TAG := EXPRESSION
   
     TAG := EXPRESSION
   
     …
   
     TAG := EXPRESSION
   
   ENDSET



-  As mentioned in the preceding section, it is possible to define
   parameters (or identifiers) associated with a format tag to which
   certain values can be assigned and which are passed on to the user
   application for all the tags of this type found.



Specification Section
=====================

This section includes the extraction specifications per se.
