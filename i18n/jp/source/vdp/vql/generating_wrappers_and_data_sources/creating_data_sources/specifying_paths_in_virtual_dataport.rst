====================================
Specifying Paths in Virtual DataPort
====================================

To create a DF, Excel, JSON or XML data source you have to specify a path to
the data. The syntax to specify this path is common between these data
source types: DF, Excel, JSON, XML and Custom.

.. code-block:: bnf
   :caption: Syntax to set the path in a DF, JSON or XML data source
   :name: Syntax to set the path in a DF, JSON or XML data source

   <route> ::=
       LOCAL { 'LocalConnection' | 'VariableConnection' } <path:literal>
           [ FILENAMEPATTERN = <literal> ]
           [ CHARSET = <encoding:literal> ]
     | HTTP 
         { 
             'http.CommonsHttpClientConnection [ <option> {, <option> }* } ]' 
           | 'http.DenodoBrowserPoolConnection [ <option> {, <option> }* } ]' 
         }
         { GET | POST } <uri:literal> 
         [ POSTBODY <body:literal> [ MIME <mimetype:literal> ] ]
         [ HEADERS (
             <header name:literal> = <header value:literal>
             [, <header name:literal> = <header value:literal> ]*
         ) ]
         [ <pagination settings>  ]
         [ CHECKCERTIFICATES ] 
         [ <authentication> ]
         [ <proxy> ]
         [ CHARSET = <encoding:literal> ]
      | FTP 'ftp.FtpClientConnectionAdapter' <uri:literal> <login:literal>
        <password:literal> [ ENCRYPTED ] 
        [ FILENAMEPATTERN = <literal> ]
        [ CHARSET = <encoding:literal> ]
      }

   <pagination settings> ::=
       PAGINATION_SETTINGS (
         PAGE_SIZE_PARAMETER = <literal>
         PAGE_SIZE = <integer>
         PAGE_NUMBER_PARAMETER = <literal>
         FIRST_PAGE_INDEX = <integer>
         OFFSET_FOR_NEXT_REQUESTS = <integer>
         MAX_NUMBER_OF_REQUESTS = <integer>
       )
     | PAGINATION_SETTINGS (
         PAGE_SIZE_PARAMETER = <literal>
         PAGE_SIZE = <integer>
         NEXT_TOKEN_PARAMETER = <literal>
         NEXT_TOKEN_PATH = <literal>
         [ MAX_NUMBER_OF_REQUESTS = <integer> ]
       )
     | PAGINATION_SETTINGS (
         PAGE_SIZE_PARAMETER = <literal>
         PAGE_SIZE = <integer>
         NEXT_TOKEN_PATH = <literal>
         [ MAX_NUMBER_OF_REQUESTS = <integer> ]
       )
   
   <authentication> ::= 
     AUTHENTICATION {
         OFF 
       | BASIC ( <credentials> )
       | BASIC ( WITH PASS-THROUGH SESSION CREDENTIALS ( <credentials> ) )
       | DIGEST ( <credentials> ) 
       | DIGEST ( WITH PASS-THROUGH SESSION CREDENTIALS ( <credentials> ) ) 
       | NTLM ( <ntlm_credentials> ) 
       | NTLM ( WITH PASS-THROUGH SESSION CREDENTIALS ( <ntlm_credentials> ) )
       | OAUTH10A (
             CLIENTIDENTIFIER = <literal>
             CLIENTSHAREDSECRET = <literal> [ ENCRYPTED ]
             [
                 ACCESSTOKEN = <literal> [ ENCRYPTED ]
                 ACCESSTOKENSECRET = <literal> [ ENCRYPTED ]
             ]
             SIGNATUREMETHOD = { HMAC_SHA1 | PLAINTEXT }
             [
                 TEMPORARYCREDENTIALREQUESTURL = <literal> { GET | POST }
                 RESOURCEOWNERAUTHORIZATIONURL = <literal>
                 TOKENREQUESTURL = <literal> { GET | POST }
                 REDIRECTURL = { OOB | DEFAULT | <literal> }
             ]
         )
       | OAUTH20 (
             ACCESSTOKEN = { 
                   <token:literal> [ ENCRYPTED ] 
                 | VARIABLE <name of the variable:literal>
             }
             REQUESTSIGNINGMETHOD = { 
                   HEADER
                 | FORM_ENCODED
                 | URL [ <query parameter name:literal> ] 
             }
             {
                AUTHENTICATION_GRANT = PASSWORD_GRANT (
                    USER_IDENTIFIER = <literal>
                    USER_PASSWORD = <literal> [ ENCRYPTED ]
                )
                [ TOKENENDPOINTURL = <literal> ]
                [ EXTRA_PARAMETERS_OF_REFRESH_TOKEN_REQUEST ( 
                    <parameter name:literal> = <parameter value:literal> 
                    [ , <parameter name:literal> = <parameter value:literal> ]+
                  )
                ]
                CLIENTIDENTIFIER = <literal>
                CLIENTSECRET = <literal> [ ENCRYPTED ] 
                [ AUTHENTICATION_METHOD_OF_AUTHORIZATION_SERVERS = 
                    { HTTP_BASIC | REQUEST_BODY } ]
                [ ACCESSTOKENEXPIRESIN = 
                     <access token expires in # seconds:long> ]
                ]
             |
                AUTHENTICATION_GRANT = CLIENT_CREDENTIALS_GRANT
                [ TOKENENDPOINTURL = <literal> ]
                [ EXTRA_PARAMETERS_OF_REFRESH_TOKEN_REQUEST ( 
                    <parameter name:literal> = <parameter value:literal> 
                    [ , <parameter name:literal> = <parameter value:literal> ]+
                  )
                ]
                CLIENTIDENTIFIER = <literal>
                CLIENTSECRET = <literal> [ ENCRYPTED ] 
                [ AUTHENTICATION_METHOD_OF_AUTHORIZATION_SERVERS = 
                    { HTTP_BASIC | REQUEST_BODY } ]
                [ ACCESSTOKENEXPIRESIN = 
                    <access token expires in # seconds:long> ]
                ]
             |
                AUTHENTICATION_GRANT = CODE_GRANT
                [ TOKENENDPOINTURL = <literal> ]
                [ EXTRA_PARAMETERS_OF_REFRESH_TOKEN_REQUEST ( 
                    <parameter name:literal> = <parameter value:literal> 
                    [ , <parameter name:literal> = <parameter value:literal> ]+
                  )
                ]
                CLIENTIDENTIFIER = <literal>
                CLIENTSECRET = <literal> [ ENCRYPTED ] 
                [ AUTHENTICATION_METHOD_OF_AUTHORIZATION_SERVERS = 
                    { HTTP_BASIC | REQUEST_BODY } ]
                [ REFRESHTOKEN = { 
                      <token:literal> [ ENCRYPTED ] 
                    | VARIABLE <name of the variable:literal> 
                  }
                ]
                [ ACCESSTOKENEXPIRESIN = 
                    <access token expires in # seconds:long> ]
                ]
                [
                    AUTHORIZATIONSERVERURL = <literal>
                    [ REDIRECTIONENDPOINTURL { DEFAULT | <literal> } ]
                    [ SCOPES = <scope 1:literal> [, <scope n:literal> ]* ]
                    SETSTATEPARAMETER = { TRUE | FALSE }
    			]
            }
       )
       | TWO_WAY_SSL (
           CERTIFICATE = <literal> [ ENCRYPTED ] 
           [ CERTIFICATE_PASSWORD = <literal> [ ENCRYPTED ] ]
       )
   }
   
   <proxy>::= PROXY
       OFF
     | DEFAULT
     | ON ( HOST <literal> PORT <integer> [ <credentials> ] ) 
     | AUTOMATIC ( PACURI <literal> )
   
   <credentials> ::= USER <literal> PASSWORD <literal> [ ENCRYPTED ]
   
   <ntlm_credentials> ::= <credentials> [ DOMAIN <literal> ]


There are five types of paths in Virtual DataPort:

1. **Local** (``LOCAL 'LocalConnection'``): path to a single file or to a
   directory. It can be in the local file system of the host where the
   Virtual DataPort server runs, or in a Windows shared drive.

   When the path is a directory and ``FILENAMEPATTERN`` is present, the
   data source will only process the files whose name matches the regular
   expression ``FILENAMEPATTERN.`` This clause is only valid for custom
   wrappers and delimited file data sources.
   
   The parameter ``CHARSET`` is available for DF and JSON data sources.

2. **From Variable** (``LOCAL 'VariableConnection'``): use this path if the
   data is not be obtained from any source but it is provided by clients at
   runtime, in the ``WHERE`` clause of the queries that involve the base
   views of the data source.


3. **HTTP Client** (``HTTP 'http.CommonsHttpClientConnection'``): path to
   retrieve a file by sending an HTTP request. The parameters of this path
   are the following:

   -  Timeout: beside ``http.CommonsHttpClientConnection``, you can
      specify the timeout of the request.
      For example, ``http.CommonsHttpClientConnection,120000`` indicates
      that the timeout of the requests will be 2 minutes.

   -  HTTP method (``GET`` or ``PUT`` or ``POST`` or ``PATCH`` or ``DELETE``) of the request. This parameter only
      has to be set with ``http.CommonsHttpClientConnection`` and not with
      ``http.DenodoBrowserPoolConnection``

   -  ``URI`` of the file of the data source. This Uri can have interpolation
      variables whose value will be provided at runtime. See section :ref:`Execution Context of a Query and Interpolation Strings` for
      information about interpolation variables.

   -  ``POSTBODY`` and ``MIME`` (optional): Use the parameter ``POSTBODY`` if
      the HTTP method is ``POST`` and you want to set the body of the request.
      MIME represents the Mime type of the body of this request. E.g.
      ``application/json`` or ``application/xml``

   -  ``HEADERS`` (optional): use this parameter to set the headers of the
      HTTP request.

   -  ``CHECKCERTIFICATES`` (optional): Adding this clause is equivalent to
      selecting the check box *Check certificates* of the *Configuration* tab,
      in the *HTTP client* configuration of a path to a file. The section
      :ref:`HTTP Path` (subsection of :ref:`Path Types in Virtual DataPort`) of the
      Administration Guide explains when you should enable this option.

   -  ``PAGINATION SETTINGS`` (optional): If you choose to or are required to use pagination you should enable this option. This clause is equivalent to filling in the ``Pagination Tab`` of the ``Create Data Source Dialog``. The section :ref:`vdp_admin_guide_path_types_pagination` of the Administration Guide explains when you should enable this option.

   -  ``AUTHENTICATION`` (optional): If the HTTP server requires
      authentication, add this parameter to indicate the credentials of the
      server. The supported authentication methods are BASIC, DIGEST, Mutual (two-way SSL), NTLM,
      `OAuth 1.0a <https://tools.ietf.org/html/rfc5849>`_ and `OAuth 2.0 <https://tools.ietf.org/html/rfc6749>`_.

      In the authentication methods ``BASIC``, ``DIGEST`` and ``NTLM``, if you
      add the clause ``WITH PASS-THROUGH SESSION CREDENTIALS``, when a user
      queries a view that uses this data source, Virtual DataPort uses the
      credentials of this user to authenticate against the HTTP server. In
      this case, the values of the parameters ``USERNAME`` and ``PASSWORD``
      are used only by the Administration Tool to connect to the database and
      show the schemas of the database and their tables/views. But not for
      querying tables or views of the database.
      
      The section :ref:`Mutual Authentication <vdp_admin_guide_path_mutual_authentication>` of the Administration Guide explains how 
      to use this authentication mechanism.

      For the values where you can add the ``ENCRYPTED`` modified next to
      them, you can provide those values in clear or encrypted. For the values
      you provide encrypted, add the ``ENCRYPTED`` modifier next to the value.

      To encrypt a value, execute the statement ``ENCRYPT_PASSWORD`` followed
      by the password. For example, ``ENCRYPT_PASSWORD 'password';``.
      
      The statement ``ENCRYPT_PASSWORD`` can be used to encrypt the Mutual Authentication certificate as well
      using the Base64 representation of the file as parameter.

      .. warning:: Users should be careful when enabling the cache for views
         that involve data sources with pass-through credentials enabled. The
         section :ref:`Considerations When Configuring Data Sources with Pass-Through
         Credentials` explains the issues that may arise.


      The section :ref:`OAuth Authentication <vdp_admin_guide_path_types_oauth_authentication>` of the Administration Guide provides
      more details about these authentication methods.

   -  ``PROXY`` (optional): If the HTTP request is sent through a proxy, you
      have three options:

      -  ``DEFAULT``: the data source will use the default HTTP proxy
         configuration of the Server. See the section `Default Configuration
         of HTTP Proxy` of the Administration Guide to learn how to
         configure these default values.
      -  ``ON``: the Server will connect to the proxy indicated in the
         parameters ``HOST`` and ``PORT``. If the proxy requires
         authentication, you also have to provide the credentials of the
         proxy.
      -  ``AUTOMATIC``: provide the URL of a ``proxy.pac`` file that contains
         the configuration parameters of the proxy.

   -  The parameter ``CHARSET`` is available for DF and JSON data sources.

4. **Denodo Browser** (``HTTP 'http.DenodoBrowserPoolConnection'``): the
   file is obtained using the Denodo Browser, which can execute complex
   navigation sequences written in NSEQL (Denodo ITPilot Navigation SEQuence Language).

   The NSEQL sequence is indicated in the ``uri`` parameter.

   The browser can be obtained from the internal browser pool or from a
   remote one. I.e. ``HTTP 'http.DenodoBrowserPoolConnection, 3, 1'``
   will create a HTTP route using a browser obtained from the internal
   port. Change the second parameter to 2, to obtain the browser from a
   remote pool.
 
   See the Denodo ITPilot User Guide and the NSEQL Manual for more
   information about the Denodo Browser and the NSEQL sequences.


5. **FTP / FTPS / SFTP** (``FTP``): Path that accesses a file via FTP. The
   parameters of this path are the following:


   -  The URL of the FTP server with the following format:
      ``host:port/path/file``
   -  Login of the user to connect to the FTP server.
   -  Password of the user to connect to the FTP server.
   -  The clause ``FILENAMEPATTERN`` is only valid for custom wrappers and
      delimited-file data sources.
   -  The parameter ``CHARSET`` is available for DF and JSON data sources.
   
For Local or FTP/FTPS/SFTP routes, if ``URI`` points to a directory
instead of a single file, when you query a base view created over this
data source, the Server will retrieve the data from all the files in the
directory and not just one file. In this case, the Server assumes that
all the files of the directory have the same format as the first one.