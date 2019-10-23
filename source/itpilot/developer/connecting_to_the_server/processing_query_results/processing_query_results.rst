========================
Processing Query Results
========================


.. toctree::
   :hidden:

   canceling_queries.rst

The ``query`` method for executing queries to a wrapper returns as a
result an instance of the class
``com.denodo.itpilot.client.HTMLWrapperResultIterator``. This class
(which implements the interface ``java.util.Iterator``) provides
asynchronous access to the results of the query made.



Results being accessed in an asynchronous manner means that the server
will return results of the query as they are obtained from the source
(it is important to remember that the wrapper obtains the data from the
source in real time through the network).



The method ``hasNext()`` checks if there are still elements
to return. Due to the asynchronous behavior of this case, this method
must be used before accessing each element, to make sure that data
elements are available.



The method ``next()`` of ``HTMLWrapperResultIterator`` obtains the next
result. In this case, each result is an instance of the class



com.denodo.vdb.vdbinterface.client.printer.standard.StandardRowVO.



The value associated with each field will be obtained by invoking the
method:

com.denodo.vdb.vdbinterface.common.clientResult.vo.sentences.ValueVO
getValue (String fieldname)



, where ``fieldname`` is the name of the desired field.



The method ``next()`` will throw an exception of type
``NoSuchElementException`` if there are no available data at that
moment, even if the wrapper still has results to return. Thus the
necessity of using the method ``hasNext``\ ().



As mentioned in the preceding section, the value of a field can be
atomic or compound. If it is atomic, the instance of ``ValueVO`` belongs
to the subclass ``SimpleVO``. ``SimpleVO`` is an abstract class which
subclasses are related to the basic types available in ITPilot:
``TextVO``, ``IntVO``, ``LongVO``, ``FloatVO``, ``DoubleVO``,
``DateVO``, ``BooleanVO``, ``BlobVO``. The subclasses ``IntVO``,
``LongVO``, ``FloatVO``, ``DoubleVO`` and ``BooleanVO`` provide a method
``getXXX`` (where ``XXX`` represents the name of the data type) to
access their values. For example, ``IntVO`` provides the method:



``java.lang.Integer getInt()``


In the case of ``BlobVO``, the following method is provided:


``java.lang.Byte[] getBytes()``

In the case of ``DateVO``, this is the method:


``long getTime()``

In addition, the ``SimpleVO`` superclass provides a representation of
the value as a character string accessible through the ``getValue()``
method. See Javadoc documentation for detail.



If the value is compound, the instance of ``ValueVO`` could represent an
array of registers (subclass ``ArrayVO``) or a register (subclass
``RegisterVO)``. In the case of a list of registers the method
``ArrayVO.getValues()`` returns the list of the registers it contains
(instances of the subclass ``RegisterVO``).



Page type in ITPilot is represented as a register (subclass
``RegisterVO)`` that contains the following fiels:

-  ConnectionType (0: default, 1: MSIE, 3: Denodo Browser): Browser type
   used to retrieve the page.
-  URL: URL of the page.
-  Method: HTTP method used to retrieve the page (GET or POST).
-  Parameters: HTTP POST parameters used to retrieve the page.
-  Cookies: Cookies associated to the page.



See the Javadoc documentation to see more detailed information on the
methods and properties of the class ``ValueVO`` and its subclasses.



Another important aspect of processing queries is dealing with any
errors that may arise (e.g. error connecting to the data source). There
are two methods for this of the class ``HTMLWrapperResultIterator``:



-  ``Boolean checkErrors()``. Allows you to check if an error has
   occurred during query execution. Returns ``true`` if an error
   has occurred and ``false`` if not.
-  ``String getErrorDescription()``. Where errors have occurred, this
   allows you to obtain a textual description of it. Otherwise, it
   returns ``null``. In wrappers created with versions previous to the
   5.5, the custom error messages (no longer supported from version 5.5)
   are accessed through this method.



The method ``getTraceEvents()`` returns the list of records
(``RegisterVO``) added by wrapper components to the execution trace (see
:doc:`/itpilot/generation_environment/index`).
