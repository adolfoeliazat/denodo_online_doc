========================================
Error Codes Returned by Virtual DataPort
========================================

The following table lists the error codes returned by the JDBC API of
Virtual DataPort.

.. table:: Error codes returned by the Denodo JDBC API
   :name: Error codes returned by the Denodo JDBC API 

   +-------------------------+-------------------------+-------------------------+
   | Error Type              | Error Codes             | Meaning                 |
   +=========================+=========================+=========================+
   | Authentication          | 20                      | The Server cannot       |
   |                         |                         | authenticate the user.  |
   |                         | 21                      |                         |
   |                         |                         | Usually, it means that  |
   |                         | 22                      | the credentials of the  |
   |                         |                         | user are not valid or   |
   |                         | 23                      | that the user does not  |
   |                         |                         | have enough privileges  |
   |                         | Range from 600 to 700   | to connect to the       |
   |                         |                         | database.               |
   |                         |                         |                         |
   |                         |                         | If the user is trying   |
   |                         |                         | to connect to an        |
   |                         |                         | LDAP-authenticated      |
   |                         |                         | database, it could      |
   |                         |                         | there be an error       |
   |                         |                         | establishing a          |
   |                         |                         | connection to the LDAP  |
   |                         |                         | server.                 |
   +-------------------------+-------------------------+-------------------------+
   | Parsing                 | 1                       | There was an error      |
   |                         |                         | parsing the query. It   |
   |                         | 19                      | usually means that some |
   |                         |                         | clause of the query is  |
   |                         | Range from 1100 to 1199 | misspelled or some      |
   |                         |                         | parameter is missing.   |
   +-------------------------+-------------------------+-------------------------+
   | Connection              | Range from 10000 to     | The Server could not    |
   |                         | 19999                   | open a connection to a  |
   |                         |                         | data source.            |
   |                         |                         |                         |
   |                         |                         | This happens when the   |
   |                         |                         | source is down, in case |
   |                         |                         | of JDBC data sources,   |
   |                         |                         | the JDBC driver cannot  |
   |                         |                         | be loaded, there was an |
   |                         |                         | error creating an       |
   |                         |                         | instance of a Custom    |
   |                         |                         | wrapper, etc.           |
   +-------------------------+-------------------------+-------------------------+
   | Security                | 11                      | The user does not have  |
   |                         |                         | enough privileges to    |
   |                         | 12                      | execute the request.    |
   |                         |                         |                         |
   |                         | Range from 20000 to     | For example, when a     |
   |                         | 29999                   | user with READ          |
   |                         |                         | privileges tries to     |
   |                         |                         | execute an ``INSERT``   |
   |                         |                         | query.                  |
   +-------------------------+-------------------------+-------------------------+
   | Compute capabilities    | 2                       | There was an error      |
   |                         |                         | while creating a view   |
   |                         | 9                       | or preparing the        |
   |                         |                         | execution plan of a     |
   |                         | Range from 30000 to     | query because:          |
   |                         | 39999                   |                         |
   |                         |                         | -  The schema of the    |
   |                         |                         |    view or the query    |
   |                         |                         |    cannot be            |
   |                         |                         |    calculated. E.g.,    |
   |                         |                         |    the query tries to   |
   |                         |                         |    project a field that |
   |                         |                         |    does not exist.      |
   |                         |                         | -  Or, the restrictions |
   |                         |                         |    of one or more       |
   |                         |                         |    sources used in the  |
   |                         |                         |    query are not met.   |
   |                         |                         |    E.g., the query does |
   |                         |                         |    not provide a value  |
   |                         |                         |    for the mandatory    |
   |                         |                         |    fields.              |
   +-------------------------+-------------------------+-------------------------+
   | Metadata management     | 5                       | There was an error      |
   |                         |                         | loading/storing the     |
   |                         | 8                       | metadata of an object   |
   |                         |                         | (data source, wrapper,  |
   |                         | 10                      | view, etc.). This may   |
   |                         |                         | happen when there is    |
   |                         | 17                      | already an object with  |
   |                         |                         | the same name, the name |
   |                         | 18                      | of the new object is    |
   |                         |                         | not valid, etc.         |
   |                         | 24                      |                         |
   |                         |                         |                         |
   |                         | Range from 40000 to     |                         |
   |                         | 49999                   |                         |
   +-------------------------+-------------------------+-------------------------+
   | Execution               | 4                       | There was an error      |
   |                         |                         | during the execution of |
   |                         | 6                       | the query.              |
   |                         |                         |                         |
   |                         | 7                       |                         |
   |                         |                         |                         |
   |                         | 13                      |                         |
   |                         |                         |                         |
   |                         | 14                      |                         |
   |                         |                         |                         |
   |                         | 15                      |                         |
   |                         |                         |                         |
   |                         | 16                      |                         |
   |                         |                         |                         |
   |                         | 25                      |                         |
   |                         |                         |                         |
   |                         | Range from 50000 to     |                         |
   |                         | 59999                   |                         |
   +-------------------------+-------------------------+-------------------------+

