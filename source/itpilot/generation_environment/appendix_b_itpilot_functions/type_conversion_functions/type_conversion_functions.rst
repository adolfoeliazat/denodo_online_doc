.. _itp_gen_environment_guide_type_conversion_functions:

=========================
Type Conversion Functions
=========================

These functions allow for different transformations among different
types of data.

-  ``CAST``: This function is given two arguments. The first one
   specifies the name of a data type and the second one specifies a
   value to be converted to the provided data type. The following table
   shows the possible type conversions:

   
.. table:: Type conversions permitted with the CAST function
   :name: Type conversions permitted with the CAST function
   
   +----------------+---------------------------------------------+
   | Target Type    | Source Type                                 |
   +================+=============================================+
   | boolean        |  String                                     |
   +----------------+---------------------------------------------+
   | double         |  String                                     |
   +----------------+---------------------------------------------+
   | float          |  String                                     | 
   +----------------+---------------------------------------------+
   | int            |  String                                     |
   +----------------+---------------------------------------------+
   | url            |  String                                     |
   +----------------+---------------------------------------------+
   | long           |  String                                     |
   +----------------+---------------------------------------------+
   | String         |  int, long, float, double, boolean, url,    |
   |                |  Page (retrieves the page’s source code     |  
   |                |  as a String)                               | 
   +----------------+---------------------------------------------+
   | Binary         |  Page (retrieves the page’s source code     |
   |                |  as Binary; to retrieve a binary page as    |
   |                |  a Binary data type, it must be             |
   |                |  downloaded by using the Denodo             |
   |                |  Browser)                                   |
   +----------------+---------------------------------------------+
   | Page           |  String (casts a string containing a page   |
   |                |  source code to a Page object)              |
   +----------------+---------------------------------------------+
   | Browser        |  String (casts a string containing a        |
   |                |  browser id to a browserId-type; that can   |
   |                |  be used in persistent browser management   |
   |                |  components)                                |     
   +----------------+---------------------------------------------+
