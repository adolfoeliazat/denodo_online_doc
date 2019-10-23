===================================================================
Customizing Events and Public Render Parameters of JSR-286 Portlets
===================================================================

.. note:: Publishing views as widgets is a deprecated feature and it may be removed in future
   major versions of the Denodo Platform. 
   
   The section :ref:`Features Deprecated in Virtual DataPort 7.0` lists all the features that are deprecated.

This appendix explains how to change the name of the events and public
render parameters (from now on, PRP) that a Denodo portlet can publish
and process. This only applies to JSR-286 portlets.

A Denodo portlet processes and publishes an event for each field of the
view that publishes. It also reads the value of a PRP and sets the value
of another PRP, for each field of the view.

For example, if a Denodo portlet publishes a view that has a field
called ``account_name``, the portlet processes the event
``Input_account_name`` and publishes the event
``Selected_account_name``. It also reads the value of the PRP
``Input_account_name`` and set the value of the PRP
``Selected_account_name``.

When the portlet receives the event ``Input_account_name``, it adds a
condition to the ``WHERE`` clause of the query executed by the portlet
to retrieve the data. I.e. "``SELECT ... FROM ... WHERE account_name =``
<value received from the event>".

If the user clicks on the link of the portlet to select a row, the
portlet publishes one event for each field of the view. Each event has
the value of the field in the selected cell. The name of these events is
``Selected_<field name>``.

The name of the events and parameters can be customized so their name
matches the name of an event/PRP of another portlet. This is necessary
if you want to deploy a Denodo portlet in a portal that does not support
*event wiring*.

We will explain how to customize these names with an example:

Let us say that we want to deploy two widgets:

-  A widget that publishes the view internet\_inc, which has a field
   called iinc\_id.
-  And a widget that publishes the view phone\_inc, which has a field
   called pinc\_id.

We will explain how to map the event “Selected\_iinc\_id” published by
the portlet “internet\_inc” with the event “Input\_PINC\_ID” that the
portlet “phone\_inc” can process.

Follow these steps:


#. Publish the views “internet\_inc” and “phone\_inc” as widgets. You will
   find the generated war files in the URL :file:`https://{<host name of the Denodo server>}:9090/export/`.

#. Edit the ``portlet.xml`` file of ``phone_inc.war`` (located in the
   directory ``/WEB-INF/`` of the war file).
   
   Note that “phone\_inc” is the portlet that will receive the events
   sent by “internet\_inc”.
   
   Do the following changes:

   a. Search for the ``<preference>`` with
      ``<name>IPC_NAME_PINC_ID</name>`` and replace ``Input_PINC_ID`` with
      ``Selected_iinc_id``, which is the name of the event sent by the
      portlet ``internet_inc`` and that we want ``phone_inc`` to process
      it.
      That is, you have to replace this:

      .. code-block:: xml
       
            <preference>
                <name>IPC_NAME_PINC_ID</name>
                <value>Input_PINC_ID</value>
                <value>Selected_PINC_ID</value>
               <read-only>true</read-only>
            </preference>
       
      with this:
           
      .. code-block:: xml
      
            <preference>
                <name>IPC_NAME_PINC_ID</name>
                <value>Selected_iinc_id</value>
                <value>Selected_PINC_ID</value>
               <read-only>true</read-only>
            </preference>


      The first ``<value>`` of the preferences that start with ``IPC_NAME``
      maps the name of a processing event and a PRP, with the name of a
      field. The default value is ``Input_PINC_ID``, which means that when
      the portlet receives an event called ``Input_PINC_ID``, it will add a
      condition to the query executed by the portlet (i.e.
      "``PINC_ID=`` <value sent by the event>").

      The second ``<value>`` of these preferences is the name of the event
      and PRP that will be published, which will include the value of the
      field ``PINC_ID`` in the selected row.

   b. Search the element ``<supported-processing-event>`` with the
      ``<qname> Input_PINC_ID``.
      Replace ``Input_PINC_ID`` with ``Selected_iinc_id``.
  
   c. Search the element
      ``<supported-public-render-parameter>Input_PINC_ID</supported-public-render-parameter>`` 
      
      Replace ``Input_PINC_ID`` with ``Selected_iinc_id``.

   d. Search the element ``<event-definition>`` with the ``<qname> Input_PINC_ID``.
      Replace ``Input_PINC_ID`` with ``Selected_iinc_id``.

   e. Search the element ``<public-render-parameter>`` with the
      ``<identifier> Input_PINC_ID``.
      In the ``<qname>`` and the ``<identifier>`` elements, replace
      ``Input_PINC_ID`` with ``Selected_iinc_id``.


3. Replace the ``portlet.xml`` of ``phone_inc.war`` with the new one.


4. Deploy the war files of the two portlets as usual.


 
