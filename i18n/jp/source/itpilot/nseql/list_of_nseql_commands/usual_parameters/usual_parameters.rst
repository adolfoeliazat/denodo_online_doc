================
Usual Parameters
================

Certain parameters are common to a high number of NSEQL commands:

-  **‘int position’ parameters**. A parameter of this type represents an
   index (starting in 0) of the elements of a sorted group. Typically, the
   members of this group are the HTML elements of a specific type (e.g.
   links, forms, frames, etc.) that are present in the current document and
   that verify a specific condition imposed by the command to which the
   parameter belongs. For example, the parameter ``position`` of the
   command ``FINDFORMBYNAME`` indexes the group of forms present in the
   current document with a name that verifies the conditions specified by
   the remaining parameters of the command. The order of the various
   elements in the group is determined by the order in which they appear in
   the HTML code of the page.

-  **‘int pages’ parameters**. This type of parameter is received by the
   commands that generate a new navigation. It specifies the number of
   pages that should be waited to download after execution of the command,
   before continuing with execution of the sequence. The function of this
   parameter is to ensure that subsequent NSEQL commands do not begin
   executing until the navigation issued by the current command has
   completed (had the navigation not been completed, subsequent NSEQL
   commands could fail, as the HTML elements on which they act could not be
   available yet). In the most common case, this parameter receives the
   value ‘1’, as the induced navigation involves only one new page being
   loaded. However, there are certain cases where more pages need to be
   loaded:

   -  When a multi-frame document is loaded, it is necessary to wait until
      all the frames have loaded (each frame loads its own page).
   -  If the server carries out certain types of automatic redirecting, the
      system can move through one or various intermediate pages before
      arriving at the required page.
