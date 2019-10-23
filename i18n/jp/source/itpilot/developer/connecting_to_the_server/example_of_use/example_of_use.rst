==============
Example of Use
==============

This section shows a simple example of how to use the API.



The application starts connecting to an execution server installed in
the ‘acme’ machine in port 9999. Next, a reference to the wrapper called
“Movies” is obtained whose schema is the same used as an example in the
preceding section:



``{TITLE, DIRECTOR, EDITIONS {FORMAT, PRICE, DESCRIPTION}}``,



where TITLE and DIRECTOR are optional search fields.



Then, a query is issued to the wrapper using the input parameter
DIRECTOR with the value “Woody Allen”, and the results are processed and
shown in the standard output.



To process the results, the hierarchical structure of ``ValueVO``
elements is navigated. First, the objects ``SimpleVO`` are obtained that
represent the atomic fields TITLE and DIRECTOR. Then the compound field
EDITIONS, which is represented by an object ``ArrayVO`` that contains an
object ``RegisterVO`` for each edition of the film. Each of these
registers contains the atomic fields FORMAT, PRICE and DESCRIPTION. All
atomic fields are of the type ``text`` except the field PRICE, which is
a ``double``.



Finally, any possible errors produced during execution are checked.


.. code-block:: java
   :name: Example of query execution to a wrapper
   :caption: Example of query execution to a wrapper

   package com.denodo.itpilot.client;
   
   import java.util.List;
   import java.util.HashMap;
   import java.util.Map;
   import java.util.Iterator;
   import com.denodo.vdb.vdbinterface.common.clientResult.vo.sentences.ValueVO;
   import com.denodo.vdb.vdbinterface.common.clientResult.vo.sentences.SimpleVO;
   import com.denodo.vdb.vdbinterface.common.clientResult.vo.sentences.ArrayVO;
   import com.denodo.vdb.vdbinterface.common.clientResult.vo.sentences.RegisterVO;
   import com.denodo.vdb.vdbinterface.client.printer.standard.StandardRowVO;
   
   public class ITPilotExample {
       
       public static void main(String args[]) {
   
           try {
   
               // Connect to server
               HTMLWrapperServerProxy server = new HTMLWrapperServerProxy("acme", 9999);
   
               // Get Wrapper
               HTMLWrapperProxy wrapper = server.getHTMLWrapper("Movies");
   
               // Prepare query params
               Map queryParams = new HashMap();
               queryParams.put("DIRECTOR", "Woody Allen");
   
               // Execute query
               HTMLWrapperResultIterator results = wrapper.query(queryParams);
   
               // Iterate results
               int numOfTuples = 0;
               while (results.hasNext()) {
                   numOfTuples++;
                   StandardRowVO tuple = (StandardRowVO) results.next();
   
                   /* Process each tuple */
                   System.out.print(numOfTuples + ". ");
   
                   // Get and print atomic fields: TITLE, DIRECTOR
                   SimpleVO titleVO = (SimpleVO) tuple.getValue("TITLE");
                   String title = (String) titleVO.getValue();
                   System.out.println("TITLE:" + title);
   
                   SimpleVO directorVO = (SimpleVO) tuple.getValue("DIRECTOR");
                   String director = (String) directorVO.getValue();
                   System.out.println("DIRECTOR:" + director);
   
                   // Get EDITIONS array
                   ArrayVO editionsVO = (ArrayVO) tuple.getValue("EDITIONS");
   
                   // Iterate over EDITION registers
                   int numEditions = 0;
                   Iterator editions = editionsVO.getValues().iterator();
                   while (editions.hasNext()) {
                       numEditions++;
                       System.out.println("EDITION: " + numEditions);
                       RegisterVO editionVO = (RegisterVO) editions.next();
                       Map edition = editionVO.getValues();
                       SimpleVO formatVO = (SimpleVO) editionVO.get("FORMAT");
                       String format = (String) formatVO.getValue();
                       System.out.println("\t FORMAT:" + format);
   
                       DoubleVO priceVO = (DoubleVO) editionVO.getValue("PRICE");
                       Double price = priceVO.getDouble();
                       System.out.println("\t PRICE:" + price);
   
                       SimpleVO descriptionVO = (SimpleVO) editionVO.getValue("DESCRIPTION");
                       String description = (String) descriptionVO.getValue();
                       System.out.println("\tDESCRIPTION:" + description);
                   }
   
                   System.out.println("");
               }
               // Check errors
               if (results.checkErrors())
                   System.out.println("Error: " + results.getErrorDescription());
   
           } catch (Exception e) {
               System.err.println("Error trying to access server ... ");
           } finally {
               // ...
           }
       }
   }

   