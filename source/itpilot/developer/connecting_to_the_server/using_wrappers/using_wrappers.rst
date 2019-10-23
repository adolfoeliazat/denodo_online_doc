==============
Using Wrappers
==============

Once a reference to a wrapper has been obtained (instance of the class
``HTMLWrapperProxy``) various operations can be carried out on it,
through the methods of said class.



To execute a query to a wrapper we will use the method
``HTMLWrapperResultIterator query(Map parameters)``.



The query to be executed is represented as a map of pairs *name of
attribute/value*. The attribute names must match the names of the input
parameters specified during the creation of the wrapper.



The values must be specified as character strings, even when the input
parameters expected by the wrapper belong to other type. For example, if
a wrapper is expecting a ``float``-type parameter, and we want to assign
the value ``3.25`` when invoking it, we must pass the ``3.25``
string. In the case of ``float``, ``double`` and ``date`` data types, it
is important to make sure that the values are provided according to the
internationalization configuration specified in the wrapper Init
component or, in case of date data types, the date pattern if it was
set.



It is important to take into account that for the query to execute
correctly, a value must be specified for all the mandatory attributes.
See :doc:`/itpilot/generation_environment/index` for more information on the process of generating
wrappers in ITPilot.



Although most of the applications will not require this, a wrapper
schema can be obtained using the method
``HTMLWrapperMetaRegisterRawVO getSchema()``.



This method returns the schema of the results returned by the wrapper
and the characteristics of the atomic fields that form part of said
schema. The schema was defined during the generation of the wrapper (see
:doc:`/itpilot/generation_environment/index`).



The results returned by a wrapper follow a hierarchical structure. Each
output tuple contains a value for every attribute contained in the
wrapper response. Each attribute may be either atomic or compound. The
value of atomic attributes can be of any of the basic data types
available in ITPilot: ``int``, ``long``, ``float``, ``double``,
``text``, ``date``, ``Boolean`` or ``blob``. The value of a compound
attribute can be a register or an array of registers. In the same form,
each register will be composed of several fields and, again, these
fields may be either atomic or compound.



Page type in ITPilot is represented as a register that contains the URL,
the method, the parameters and the cookies of the page.



For example, a wrapper that returns data on movies may have a schema in
which each result is comprised of the fields TITLE, DIRECTOR and
EDITIONS. TITLE and DIRECTOR are atomic fields and EDITIONS is a
compound field containing data on various editions available of the
movie (DVD, VHS, director’s cut, etc.). The value of EDITIONS is an
array of registers, where each register contains the fields FORMAT,
PRICE and DESCRIPTION, all of which are atomic.



The invocation to ``getSchema()`` returns an instance of the class
``HTMLWrapperMetaRegisterRawVO``, which represents the schema of a
“hierarchical” register of the type described above. See the Javadoc
documentation for a detailed description of the methods provided by
``HTMLWrapperMetaRegisterRawVO``.



It is also possible to access the characteristics of the various atomic
fields that comprise the schema. Information about these atomic fields
is represented as instances of the class ``HTMLWrapperMetaSimpleRawVO``.
Specifically, the following information can be obtained from an atomic
field: its type, by using the method ``java.lang.Class getType()``,
whether the value is obtained from the source or not (that is, to know
if it is a searchable field that cannot be found in the output schema,
using the method ``boolean isSearchStatus()``) and, in that case,
whether it is mandatory or not (method ``boolean isMandatoryStatus()``).
Furthermore, if they have been defined during the generation process, it
is also possible to obtain the regular expression (method
``java.lang.String getRegexp()``) and the aliases defined for each field
(method ``java.util.List getTextValues()``).



Finally, the method ``void setVerification(boolean value)`` allows
setting via API whether a wrapper should be automatically verified or
not by ITPilot automatic verification server.