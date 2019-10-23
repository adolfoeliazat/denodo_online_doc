============================================================================
Advanced Configuration of the Sequence and Next Interval Iterator Components
============================================================================

The ‘Advanced’ tab of the Sequence component configuration wizard allows
providing an alternative navigation sequence in order to load the
optional page value received as input by the component while the ‘Denodo
Browser’ tab allows configuring advanced options if Denodo Browser has
been chosen to execute the sequence. The following subsections describe
each of these aspects. These options are also available in the rest of
components that execute navigation sequences: ExtractorSequence (see
section :ref:`Extractor Sequence`), Next Interval Iterator (see section :ref:`Next Interval Iterator`) and Form Iterator (see section :ref:`Form
Iterator`).


Loading a Page Received as Input Parameter
=================================================================================

The Sequence component has a page-type element as optional input
parameter. In these cases, the page will be loaded before executing the
configured navigation sequence. This is useful when the sequence must
continue from a previously browsed page.



Even though it is not required in the example provided in this guide
(nor in most applications), it is possible to configure how ITPilot
performs the loading of this input page by using the ‘Advanced’ tab. The
following graphical elements are used:

#. Check box “Use Custom Sequence for Restoring Input Page”. It will be
   checked when the user wants to load the input page by him/herself. In
   the default option ITPilot generates a GET/POST http request to load
   the page.
#. If the previous check box has been checked, the user may load a
   specific navigation sequence by using the buttons “Load from File” or
   “Import from Browser”, as it was previously explained for the
   Sequence component (see section :ref:`Web browsing automation`). This can
   be useful if the input page load process performed by default by
   ITPilot does not worked adequately or can be optimized by means of an
   ad hoc sequence.

The sequence can be left blank if you know that a restore sequence is
not needed (in that case, leaving it blank improves performance).



Preferences of Denodo Browser
=================================================================================

In order to increase the efficiency of the Denodo Browser, components
that execute sequences using this browser can be configured to optimize
the sequence execution in two exclusive ways:

-  Automatic Optimization: A test execution of the sequence is performed
   to detect what elements of the page are needed to execute the
   sequence. During that execution some information is collected to
   identify those elements, and in subsequent executions only the
   necessary elements are loaded.
-  Manual Optimization: some optimization parameters can be manually
   set.



To enable the automatic optimization, click on the “Automatic
Optimization” check box. Then, press the “Collect Optimization
Information Now” button. This action will perform a test execution of
the sequence to detect what elements of the accessed pages are needed
and Denodo Browser will collect information to identify them. At
execution time, Denodo Browser will use that information to load only
the relevant elements of the pages and discard those that are not needed
for the correct execution of the sequence. The button “Clear
Optimization Information” allows removing the information collected by
the automatic optimization process. Note that, the information
collection process needs to be manually rerun every time the sequence is
changed.



Once the wrapper is deployed on the server, the Denodo Browser automatic
optimization (i.e. the use of the optimization information collected
during the generation of the wrapper) can be enabled or disabled from
the ITPilot web administration tool.



To enable the manual optimization, click on the “Manual Optimization”
check box. When it is checked, then the following parameters can be
configured:



-  Never execute JavaScript: specifies that the browsers will not try to
   execute JavaScript code. This value can be changed dynamically
   through the NSEQL command :ref:`SetAmbientProperties <nseql_guide_set_ambient_properties>`. If JavaScript processing is not required in the sequence,
   selecting this option is advisable, since it considerably improves
   performance.
-  Use simplified DOM: if this option is checked, the browsers will not
   build the complete DOM tree of the page, but a simplified one that
   considers only the link-type HTML elements and the form ones. If the
   required navigation in that source can be performed with only those
   types of elements, the use of this option can be advisable in order
   to generate a more efficient execution. The HTML tags considered are
   the following: ``A``, ``AREA``, ``BASE``, ``BODY``, ``BUTTON``,
   ``FORM``, ``FRAME``, ``HTML``, ``IFRAME``, ``IMAGE``, ``INPUT``,
   ``MAP``, ``OPTION``, ``SELECT``, ``TEXTAREA``, ``SCRIPT``. Therefore,
   NSEQL commands will only work with these tags. :ref:`Appendix D:
   Constraints of the Simplified DOM` describes further constraints
   with respect to these tags.
-  Disable page loading: if selected, the browser will not build the DOM
   tree nor interpret the JavaScript code. It can be used in navigation
   sequences that *only* have the commands *Navigate* and *PostData*.
-  Disable Iframes: if selected, the browsers will not download iframes
   content. If this content is not needed, this will improve
   performance.
-  Exclude domains: list of domains from which no content is going to be
   downloaded. For instance, this is useful to avoid the downloading of
   ads or other content types that can slow down the navigation, and are
   not necessary.


