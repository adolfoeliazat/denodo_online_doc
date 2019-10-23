===================
Denodo Browser Path
===================

The Denodo Browser is a GUI-less browser that embeds an HTTP client and
a JavaScript engine. It can perform web navigations over complex web
sites as any other browser. The benefits of the Denodo browser over
other browsers is that, as it does not have a graphical interface, is
much lighter. This improve the response time of the navigation sequences
and decreases the CPU load.

To retrieve data from a web site using the Denodo Browser you have to
provide a navigation sequence written in ITPilot NSEQL (Navigation
SEQuence Language).

You can easily generate a navigation sequence using the ITPilot toolbar
for Microsoft Internet Explorer. To do this, do the following:


1. Open Microsoft Internet Explorer. We assume that the ITPilot toolbar for
   Internet Explorer is installed.

2. Click **Rec**.

3. In the **URL** box, enter the URL where you want to start the
   navigation.
   
4. Record the navigation.

5. Once you finish, click **Stop** and then, **Toolbar State Info**.

6. Copy the content of the text box **NSEQL Sequence**.

7. Go back to the administration tool and paste this sequence in the
   **Sequence** box of the dialog “Edit Denodo Browser Connection”. Take
   the following into account:


   -  Paste the sequence *after* the text ``sequence://``. For example:
   
.. code-block:: none

   sequence://Navigate(www.denodo.com,0);
   ExtendedWaitPages(-1);

.. _path_types_in_virtual_dataport_separator_4:

   -  You cannot put comments in the sequence. I.e. there can be no lines
      that start with ``#`` or ``/*``.


In the **Browser pool** list, the available options are:

-  **Internal**: to execute the navigation sequence, Virtual DataPort
   will create an instance of the Denodo Browser to execute the
   navigation sequence.
-  **External**: to execute the navigation sequence, Virtual DataPort
   will send a request to the ITPilot browser pool of the current
   installation (you have to make sure it is started) to execute the
   navigation sequence.

The benefit of using the internal browser is that you do not have to
start the ITPilot Browser Pool. On the other hand, it puts more load on
the Virtual DataPort server than if the external browser pool executes
the navigation sequence.

.. note::
   Sometimes you have to do minor changes on the navigation
   sequences recorded from Internet Explorer so they work with the Denodo
   Browser, especially when dealing with complex websites. If this is the
   case, you can use the Sequence Debugger of the ITPilot Wrapper
   Generator Tool to help you debug where the problem is.

The :doc:`/itpilot/user_guide/index` and the :doc:`/itpilot/nseql/index` 
provide more information about the
Denodo Browser, NSEQL sequences and how to record sequences using the
Internet Explorer toolbar.
