============
Status Codes
============

Following the RESTful conventions, the HTTP status codes returned by the
RESTful Web service have a special meaning:


.. table:: HTTP status codes returned by the RESTful Web service
   :name: HTTP status codes returned by the RESTful Web service

   +------+-------------------+--------------------------------------------------+
   | Code | Message           | Meaning                                          |
   +======+===================+==================================================+
   | 200  |Success            |                                                  |
   +------+-------------------+--------------------------------------------------+
   | 201  |Created            | The POST request (INSERT operation) was          |
   |      |                   | processed successfully, which means that the row |
   |      |                   | was inserted correctly in the source.            |
   +------+-------------------+--------------------------------------------------+
   | 204  |No content         | For GET requests, it means that the view does    |
   |      |                   | not have any row.                                |
   |      |                   |                                                  |
   |      |                   | Only for the XML and                             |
   |      |                   | JSON representations of                          |
   |      |                   | view resources.                                  |
   |      |                   |                                                  |
   |      |                   | For IDU requests (see                            |
   |      |                   | section :ref:`IDU                                |
   |      |                   | Requests`), it means                             |
   |      |                   | that the IDU statement                           |
   |      |                   | was executed                                     |
   |      |                   | successfully.                                    |
   +------+-------------------+--------------------------------------------------+
   | 400  |Bad request        | It means one of the                              |
   |      |                   | following:                                       |
   |      |                   |                                                  |
   |      |                   | -  Incorrect parameters                          |
   |      |                   |    or malformed URL                              |
   |      |                   | -  Mandatory parameter                           |
   |      |                   |    missing                                       |
   |      |                   | -  Try to access an                              |
   |      |                   |    element URI in a                              |
   |      |                   |    view that does not                            |
   |      |                   |    have primary key                              |
   |      |                   | -  For ``POST`` or                               |
   |      |                   |    ``PUT`` requests                              |
   |      |                   |    (see section                                  |
   |      |                   |    :ref:`IDU Requests`),                         |
   |      |                   |    the XML                                       |
   |      |                   |    or JSON document is                           |
   |      |                   |    not well-formed                               |
   +------+-------------------+--------------------------------------------------+
   | 401  |Not authorized     | The credentials provided are incorrect           |
   +------+-------------------+--------------------------------------------------+
   | 403  |Forbidden          | The user does not have                           |
   |      |                   | permissions to perform                           |
   |      |                   | the action. That is, the credentials are correct |
   |      |                   | but the user account is forbidden to do this     |
   |      |                   | action. E.g. the user tries to insert a row on a |
   |      |                   | view and does not have privileges to do so.      |
   +------+-------------------+--------------------------------------------------+
   | 404  |Not found          | The resource does not                            |
   |      |                   | exist. E.g. the database does not exist, the     |
   |      |                   | view does not exist or the URL points to a row   |
   |      |                   | by its primary key and there is not row on that  |
   |      |                   | view with that key.                              |
   +------+-------------------+--------------------------------------------------+
   | 405  |Method not allowed | HTTP Method not allowed                          |
   |      |                   | on that resource                                 |
   +------+-------------------+--------------------------------------------------+
   | 406  |Not accepted       | The client requests a                            |
   |      |                   | representation format,                           |
   |      |                   | which is not supported                           |
   +------+-------------------+--------------------------------------------------+
   | 500  |Internal error     | Runtime error. If the                            |
   |      |                   | error occurred during                            |
   |      |                   | the execution of a                               |
   |      |                   | query, you will also                             |
   |      |                   | obtain the execution                             |
   |      |                   | trace, unless you are                            |
   |      |                   | accessing a published                            |
   |      |                   | REST Web service and                             |
   |      |                   | you have cleared the                             |
   |      |                   | option “Display verbose                          |
   |      |                   | error messages”.                                 |
   +------+-------------------+--------------------------------------------------+


