=========================
Developing Custom Filters
=========================

To develop a custom filter, create a new class that extends the class
``CustomConnectionFilter`` (``com.denodo.parser.connection.filter``
package).

This class has to implement the method
``execute(InputStream is):InputStream``.

A custom connection filter may have input parameters. They can be useful
if you want the behavior of the custom filter to be easily customizable.
To retrieve the parameters entered by the user when assigning the filter
to a data source, invoke the method
``getParameters():Map<String, Object>`` from the ``execute(...)``
method.

The folder :file:`{<DENODO_HOME>}/samples/vdp/customConnectionFilter` contains
an example of a custom filter.

After developing the custom filter, generate its jar and import it into
Virtual DataPort (see section :ref:`Importing Extensions` of the
Administration Guide).
