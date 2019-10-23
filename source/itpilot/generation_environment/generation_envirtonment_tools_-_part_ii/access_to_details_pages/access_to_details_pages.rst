=======================
Access to Details Pages
=======================


.. toctree::
   :hidden:

   access_to_the_details_page_using_extractor_sequence.rst
   individual_test_of_extractor_sequence_next_interval_iterator_and_form_iterator_components.rst
   specifying_the_structure_of_the_detail_page_information.rst
   generating_the_access_specification_to_the_details_page.rst
   iteration_on_the_details_page_structures_and_creation_of_the_output_record.rst

In this section we explain how to create and execute navigation
sequences that are contextual to the records extracted by an Extractor
component. This type of sequences is called “Extractor Sequence”. The
most usual type of extractor sequence is accessing ‘details’ pages that
provide further information about each of the records in a list of
results.



To illustrate the process, we will further extend the webmail wrapper
example. Until now, we have developed a process that allows obtaining
the list of messages that appear on a page of a Web e-mail application.
However, on this page only a sub-group of data for each message appears.
Elements such as the message body, the complete date, carbon-copied
recipients or attached data appear only in the detail page of each of
the messages.



In this section we explain the modifications that need to be made to the
example in order to obtain all these data.



The structure now required is the following:

-  MESSAGEDATE: date the e-mail was received
-  SENDER: who sent the e-mail
-  SUBJECT: message title
-  SIZE: size of the message
-  MESSAGE: content of the e-mail



As can be seen, the elements MESSAGEDATE, SENDER, SUBJECT and SIZE are
maintained. However, a new field is added, called MESSAGE (that is
obtained from the detail pages).



We must, therefore, modify the process in order to add those components
that allow the browsing to each one of the detail pages; besides, we
will have to modify some of the already existing ones.


