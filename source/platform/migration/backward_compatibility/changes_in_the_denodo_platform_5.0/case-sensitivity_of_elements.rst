============================
Case-Sensitivity of Elements
============================

Starting with Denodo 5.0, the following elements, parameters, functions,
etc. are now **case-sensitive**:

-   The connection URI to the Virtual DataPort server because the following
    elements are now case-sensitive:

    -  Name of the database
    -  Login
    -  Value of the parameter i18n

-   The literals in the ``CONTEXT`` clause of a query are now
    case-sensitive. I.e. literals representing view names or variables.

-   The literal parameters representing maps or views in functions; type
    names specified as literals in functions ``CAST``, ``CREATETYPEFROMXML``
    and ``GETVAR``; literals in functions ``TO_DATE``, ``FORMATDATE`` and
    ``MAP``; and literals representing fields in function
    ``IS_PROJECTED_FIELD``.


-   The names of the jars of the stored procedures and custom wrappers.
    Virtual DataPort 4.7 exports these elements in lowercase to avoid
    conflicts when importing them into Virtual DataPort |version|.

-   JMS listeners:

    -  XML output of JMS listeners. In version 4.7, XML tags are always in
       lower case but in |version|, they are in the same case as in the queried
       view.
    -  JSON output of JMS listeners. In version 4.7, the names of the
       top-level fields are in upper case and the names of the subfields are
       in lower case. In version |version| the name of the fields and subfields
       are in the same case as in the queried view.

-   Field names in the output of JSON Web services are always in uppercase
    unless they were renamed. In version 4.7, they were always in lower
    case.


-   Web services:

    -   The input parameters of REST Web services published by the Denodo
        Platform.


    -   The URLs of the SOAP Web services. For example, let us say that you
        publish the SOAP Web service “orders”.

        In Denodo 4.7 and earlier versions, the following URLs are valid:

        -   \http://acme:9090/server/pruebas/orders/services/orders
        -   \http://acme:9090/server/pruebas/orders/services/ORDERS

        Starting with Denodo 5.0, the entire URL is case sensitive. Therefore,
        the URL \http://acme:9090/server/pruebas/orders/services/ORDERS is no
        longer valid and it will return the HTTP error 404 (not found). You will
        have to use the name that matches the case of the service.
