=======================
ITPilot Development API
=======================

Denodo ITPilot incorporates a Java API for developing applications using
the wrappers created with it.



Amongst other functions, this API facilitates connection to a Denodo
ITPilot execution server, obtaining a reference to a wrapper installed
in said server and querying it. It also allows a series of additional
tasks like obtaining the list of wrappers installed in the server or
activating automatic verification of a specific wrapper.



The first step in using the API is to connect to a Denodo ITPilot
execution server. This is done by constructing an instance of the class
``com.denodo.itpilot.client.HTMLWrapperServerProxy``. Amongst other
tasks, said instance will allow to obtain a list of the available
wrappers in the server, as well as a reference to a specific wrapper,
represented through an instance of the class ``HTMLWrapperProxy``.



Said instance may be used to carry out various tasks on the wrapper, the
most important of which is query execution. When a query is invoked on
the wrapper, the results are returned to the application in an
asynchronous manner (i.e. the first results of the query will be
accessible to the application as they are obtained from the source,
without having to wait for all the results to be received).



The following subsections deal in more detail with each of the stages
mentioned: connection to the server, obtaining references to wrappers,
executing actions on them and query processing. An exhaustive
description of the API on a programming level can be found in the
Javadoc documentation.


