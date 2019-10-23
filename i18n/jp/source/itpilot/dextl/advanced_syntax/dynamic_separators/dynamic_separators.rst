==================
Dynamic Separators
==================

A dynamic separator is a string-type separator, which value is
calculated at runtime.



**Example**: We want to use a web source that contains tuples for the
relation COUNTRY (NAME, NUMBER\_INHABITANTS, EXTENSION\_IN\_KMS) to
obtain data of a country from its name. The interface of this source
consists on a page with a list of links to details pages of every
country. To locate the link of a country, without knowing at generation
time the name of the country that we want to obtain the details of, we
should use a dynamic separator. In this case "@COUNTRY"



.. code-block:: none
   :caption: Example of use of a dynamic separator
   :name: Example of use of a dynamic separator
   
   ANCHOR (:URLPAGECOUNTRY=URL) "@COUNTRY" ENDANCHOR
   
The values of dynamic separators used in a DEXTL program of an Extractor 
component are obtained from the input of the
component ("Input values"). These values can be 

-  Simple values

See more about the Extractor component in the section :doc:`/itpilot/generation_environment/appendix_c_catalog_of_components/extractor/extractor` of the ITPilot Generation Environment Guide.
