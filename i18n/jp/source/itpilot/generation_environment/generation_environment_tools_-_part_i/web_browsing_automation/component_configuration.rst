=======================
Component Configuration
=======================

The sequence component can now be configured. To do so, once again
select the component so that its input configuration area is shown. In
this area you can specify the inputs for the component:

-  *Input Values*: input values to be used in Web browsing. In this
   case, it is necessary to select the record created by the init
   component in section :ref:`Process initialization`, by clicking on the |image0| icon 
   of the “Input Values” element and selecting the register “MAILPARAMS”. 
-  *Input Page*: Optionally, the Sequence component can also receive a
   page type variable as input. This page must come from another
   component with a Web page as its output value, such as another
   Sequence component or a Next Interval Iterator component. When this
   parameter is provided, the input page will be loaded into the browser
   before starting the sequence execution. This is not necessary in this
   example.



Double-click on the Sequence component to access the Sequence Editor
wizard. This is where the navigation sequence must be loaded. A
navigation sequence is a set of NSEQL commands that describes different
events on a Web browser. The navigation sequence can be created in two
different ways:

-  Using the navigation sequence generation tool included in ITPilot.
   This tool (described in detail in section :ref:`Generating Navigation
   Sequences` of this manual), integrated as a tool bar in the
   Microsoft Internet Explorer browser, allows automatically generating
   the NSEQL program by recording user operations in the browser. This
   is the recommended way to generate navigation sequences.
-  Typing the NSEQL sequence manually. NSEQL is a relatively simple language to use and
   advanced users may prefer this option. In this case, the Sequence
   Editor helps the advanced user in the process through syntax
   highlight and auto-complete features.



In this example, the sequence generation tool is used. Open a Microsoft
Internet Explorer browser from the Denodo generation tool (either
directly via the Browser >New Browser option in the main menu or by
pressing the combination of keys Ctrl-B). A Microsoft Internet Explorer
browser window will appear with the navigation sequence recording
toolbar (see `Denodo Toolbar`_). Notice that the Internet Explorer
instances launched from the Wrapper Generation Tool can be easily
identified because the Internet Explorer icon appears in a red color
instead of the usual blue. If the toolbar does not appear, check that
the menu option “View -> Tool Bars -> Sequence Generator” is marked.



Although this section does not intend to give full details as to
how this toolbar works, we will explain the steps required for our
example. For further information, read the section :ref:`Generating Navigation
Sequences`,
which explains the way in which the sequence generation tool works, and the manual
:doc:`ITPilot NSEQL Guide <../../../nseql/index>`.



.. figure:: DenodoITPilot.GenerationEnvironment-21.png
   :align: center
   :alt: Denodo Toolbar
   :name: Denodo Toolbar




Start recording by clicking on the |image1| button and entering the
navigation sequence initial URL (`Initial URL`_). In our example, it
is the login page of the Web mail system:

.. figure:: DenodoITPilot.GenerationEnvironment-23.png
   :align: center
   :alt: Initial URL
   :name: Initial URL

   Initial URL



The “Converter” selection list indicates whether the resource to be accessed
via the URL is a Microsoft Word, Microsoft Excel or PDF document (it is
left blank by default, which means that an HTML resource is expected).
ITPilot is capable of processing Microsoft Word, Microsoft Excel and PDF
documents through automatic HTML conversion.



After clicking on OK, the browser will display the home page (`Home
Page`_):



.. figure:: DenodoITPilot.GenerationEnvironment-24.png
   :align: center
   :alt: Home Page
   :name: Home Page

   Home Page





Now,
we place the cursor on the Username field, then right-click and select
the “SetText” option; a dialog box (see `Text Selection Dialog`_)
will appear, allowing us to write the text we would like to add to the
field (using the “Set Custom Text” option). In this case, we will type
the username ‘demos’. We can also use the “Variable name” field in the
dialog to specify that at execution time, an input parameter of the
sequence should be used instead of the typed example value. In our case,
we should type ‘LOGIN’ as variable name since that was the name we
assigned to the parameter in the Initialization component (recall
section :ref:`Process
initialization`).
Clicking ok, the username field is filled in with ‘demos’.



Now, repeat the process with the password field. The password is
‘DeMo.04’ and the variable name should be ‘PASSWORD’. The result can be
seen in the `Result of the Text Selection operation`_.



.. figure:: DenodoITPilot.GenerationEnvironment-25.png
   :align: center
   :alt: Text Selection Dialog
   :name: Text Selection Dialog

   Text Selection Dialog





.. figure:: DenodoITPilot.GenerationEnvironment-26.png
   :align: center
   :alt: Result of the Text Selection operation
   :name: Result of the Text Selection operation

   Result of the Text Selection operation

To proceed with navigation, we need to click “Log in”. This is done by
positioning the cursor over the button, right-clicking, and selecting
the “Click” action. The browser will go to the results page.



Navigation status can be checked at any time by clicking on “ToolBar
State Info”. This will show the series of NSEQL commands that the
generation tool has created to date.

After checking that the semaphore icon is green, signaling that the
recording of the last navigation step has finished, you can stop the
recording process by clicking on the |image6| button.



The sequence can be imported directly in the Sequence component by
clicking on the “Import from Browser” button in its wizard. `Sequence
editor with loaded sequence`_ shows the result of the action.



You can also save the sequence if it is going to be used in other
processes. This is achieved by clicking on the |image7| button of the
toolbar. You can use it later in other ‘Sequence’ components by using
the “Load from File” button in the Sequence wizard.



Once loaded, the sequence can be modified where necessary, which also
implies that the navigation sequence can also be handwritten, although
in this case we recommend that you first read the :doc:`ITPilot
NSEQL Guide <../../../nseql/index>`.


.. _figure-21-label:

.. figure:: DenodoITPilot.GenerationEnvironment-29.png
   :align: center
   :alt: Sequence editor with loaded sequence
   :name: Sequence editor with loaded sequence

   Sequence editor with loaded sequence

Once we have imported the sequence, we can test it by clicking on the
Test button. The dialog shown in `Edit variable values dialog`_ will
appear to request values for LOGIN and PASSWORD. The “Hidden” check boxes
will allow hiding/showing a variable value during the test execution.



.. figure:: DenodoITPilot.GenerationEnvironment-30.png
   :align: center
   :alt: Edit variable values dialog
   :name: Edit variable values dialog

   Edit variable values dialog

Regarding our example, we are finished with the Sequence component, but
the component wizard includes many other options. Therefore, at this
point you can either click ‘ok’ and skip to section :ref:`Error Processing on the Web Browsing Automation`
to continue with the example, or keep reading to know more about
advanced configuration options of the ‘Sequence ‘ component.



`Sequence editor with loaded sequence`_ also shows some configuration
parameters in the Sequence editor. More specifically:


-  *Sequence Type*: ITPilot provides access to Web resources via different
   communication protocols described below:

   -  browser pool: This is the default option. In this case, the sequence
      will be run using a browser contained in the *browser pool*. The browser
      pool is configured in the Wrapper Server in which the wrapper is run.
      The browser pool uses browsers to run NSEQL sequences, using Microsoft
      Internet Explorer or the Denodo browser. The default setting will select
      the browser specified in the overall preferences page of the generation
      tool, as described in the section :ref:`Generation Tool Global Preferences`.
      In section :ref:`Comparison between MSIE and Denodo Browser`, we compare the
      different browser options.

   -  Denodo Browser: This option uses the Denodo browser. The difference with
      respect to selecting the ‘browser pool’ option and the ‘Denodo Browser’
      browser type is that in this case the browser pool server will not be
      needed to execute the wrapper; instead the browser will be created in
      runtime by the Wrapper Server.

   -  FTP: This provides access to the resource via ftp, ftps or sftp
      protocols. The format in which the access path to the resource must be
      entered in the write area is as follows:
      protocol://login:password@domain:port, where:

      -  protocol: ftp, ftps or sftp
      -  login: user name
      -  password: password to access the server
      -  domain: specific address of the server
      -  port: port where the server is run (by default this is port 21 for
         ftp, port 22 for sftp and port 990 for ftps)

   -  local: Likewise, ITPilot provides access to resources in the local file
      system. The format to use is: ``file://address`` [#f1]_, where:

      -  address: access path and resource name

-  *Reuse Connection*: When a page value is provided as input parameter to
   the component, this option indicates whether the browser used to obtain
   the input page is reused to execute the sequence or whether a new
   browser is launched, and the input page loaded in that browser. This
   option is usually selected because in the majority of cases makes sense
   to reuse the same browser, although in some cases (such as when the
   Iterator component is used, as explained in section :ref:`Processing the
   Retrieved Results`) it may not be useful. In our example this option is
   disabled because the component has no Page-type input parameter.

-  *Maximum retries*: as indicated in the section :ref:`Comparison between MSIE
   and Denodo
   Browser`,
   where the processing of errors of some type for this component is
   configured to “retry and fail if error persists” or “retry and ignore if
   error persists”, this parameter determines the number of retries to be
   made.

-  *Time between retries*: this indicates the time between one retry and
   the next in the event of the first failing. The time is defined in
   milliseconds.

--------------

.. rubric:: Footnotes

.. [#f1] Please note that the path can start with a “/” symbol. For example, 
   Windows paths start by “/”, so in order to access a specific directory,
   enter ``file:///c:/directory``.

.. |image0| image:: DenodoITPilot.GenerationEnvironment-20.png
.. |image1| image:: DenodoITPilot.GenerationEnvironment-22.png
.. |image6| image:: DenodoITPilot.GenerationEnvironment-27.png
.. |image7| image:: DenodoITPilot.GenerationEnvironment-28.png
.. |image10| image:: DenodoITPilot.GenerationEnvironment-22.png
