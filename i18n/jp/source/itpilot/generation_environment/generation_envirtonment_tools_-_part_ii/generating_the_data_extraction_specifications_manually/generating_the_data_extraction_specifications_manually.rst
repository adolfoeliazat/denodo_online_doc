======================================================
Generating the Data Extraction Specifications Manually
======================================================


.. toctree::
   :hidden:

   creating_a_structure_using_the_structure_pane.rst
   obtaining_the_tokens_of_the_data_to_extract.rst
   creating_a_dextl_specification_from_the_tags.rst

The process of generating a DEXTL program for data extraction can also
be performed manually, instead of relying in the automated generation
from examples assigned in the browser. This way needs a complete
understanding of the DEXTL language (see :doc:`/itpilot/dextl/index`) and it is only recommended
for advanced users.



The outline of the process is the following:

#. Once decided what information is going to be extracted, create a
   structure for that data in the Structure pane.
#. Highlight some data to be extracted in the browser and check what
   tags are present in that chunk, using the Token Viewer pane.
#. From the tags of the web fragment, use the Generation pane to type
   the DEXTL program that extracts the desired data.
#. Test the specification in the Specification Test pane, and repeat
   steps 3 and 4 until all the data is extracted correctly.

