=============================================
Obtaining the Number of Rows of a Result Set
=============================================

To obtain the number of rows of a result set, add the segment $count to the URL.

**Examples**

-  Obtain the number of rows of the view customer:
   \http://acme:9090/denodo-restfulws/administration_guide/views/customer/$count
-  Obtain the number of rows of the view customer that meet the condition Country = 'Mexico'
   \http://acme:9090/denodo-restfulws/administration_guide/views/customer/$count?Country=Mexico
-  Obtain the number of rows of the view that meet the condition Country <> 'France':
   \http://acme:9090/denodo-restfulws/administration_guide/views/customer/$count?$filter=%22Country%22%3C%3E%27France%27
   
Depending on the representation requested (HTML, XML or JSON), the result is different:
   
-  If you open the URLs above from a browser, the service will return the HTML representation. That is, a table with one column “count” with the number of rows of the result set.
-  If you request the XML or JSON representations (by adding the parameter $format or with the HTTP header Accept), the service simply returns the number of rows. This number is in plain text; i.e. is not in an XML or a JSON document.
-  If you request the JSON representation, you can add the parameter $jsoncallback to obtain the number of rows inside a callback function.
   
   For example, the URL 
   \http://acme:9090/denodo-restfulws/administration_guide/views/customer/$count?$format=json&$jsoncallback=function1
   
   will return

   .. code-block:: javascript
   
      function1(39)
   
   being 39 the total number of rows in the view customer.

When you add the segment ``$count`` to the URL, the service does not support adding the parameters ``$start_index`` nor $count.


