=========
Extractor
=========

.. rubric:: Description

This component is responsible for extracting structured data from an
HTML page, thus generating a DEXTL program (:doc:`/itpilot/dextl/index`).

.. rubric:: Input Parameters

This component accepts the following inputs:

-  A Page-type element (e.g. like the output of a Sequence component),
   from which information is going to be extracted.
-  Zero or more records as variables that can be used in the DEXTL
   specification and the detail sequences.

.. rubric:: Output Values

The Extractor component returns a list of records with the results
extracted from the HTML page.

.. rubric:: Details of the Component

See section :ref:`Configuration of the Extractor Component` for an in-depth
explanation of the component.



