=======
Example
=======

Example of a function with annotations, that returns an array: ``SPLIT``
which splits strings around matches of a given regular expression and
returns the array of these substrings.


.. code-block:: java
   :caption: ITPilot Custom Function Sample
   :name: ITPilot Custom Function Sample

   import com.denodo.common.custom.annotations.*;
   import com.denodo.common.custom.elements.*;
   import java.util.*;
   
   @CustomElement(type = CustomElementType.ITPFUNCTION, name = "SPLIT_SAMPLE")
   public class Split {
   
       private static final String STRING_FIELD = "string";
   
       @CustomExecutor()
       public CustomArrayValue split_sample(@CustomParam(name = "regexp") String regex,
               @CustomParam(name = "valuer") String value) {
           
           if (value == null || regex == null) {
               
               return null;
           }
           String[] result = value.split(regex);
           LinkedHashMap<String, Object> results = new LinkedHashMap<String, Object>(1);
           List<CustomRecordValue> arrayValues = new ArrayList<CustomRecordValue>(result.length);
           for (String string : result) {
               
               results.put(STRING_FIELD, string);
               CustomRecordValue recordValue = CustomElementsUtil.createCustomRecordValue(results);
               arrayValues.add(recordValue);
           }
   
           return CustomElementsUtil.createCustomArrayValue(arrayValues);
       }
   
       @CustomExecutorReturnType
       public CustomArrayType split_sampleReturnType(String regex, String value) {
           
           LinkedHashMap<String, Object> props = new LinkedHashMap<String, Object>();
           props.put(STRING_FIELD, String.class);
           CustomRecordType record = CustomElementsUtil.createCustomRecordType(props);
           CustomArrayType array = CustomElementsUtil.createCustomArrayType(record);
           return array;
       }
   }
