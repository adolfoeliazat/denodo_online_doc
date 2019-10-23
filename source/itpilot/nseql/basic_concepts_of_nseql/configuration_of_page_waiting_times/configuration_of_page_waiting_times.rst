===================================
Configuration of Page Waiting Times
===================================

The *WaitPages* and *ExtendedWaitPages* commands and certain others
(through configuration of the input argument (int) pages) allow for the
number of expected pages to be loaded before running the browser
sequence to be defined. The configuration parameters for the different
times involved are stored in the
:file:`{<DENODO_HOME>}/conf/iebrowser/IEBrowserConfiguration.properties` file:

-  ``IEBrowser.MAX_DOWNLOAD_TIME``. This parameter indicates the maximum
   waiting time until all the expected pages are loaded. 60000 ms are
   waited by default (60 seconds).
-  ``IEBrowser.INTERVAL_CHECKING``. This parameter determines the
   frequency with which loading of the new page is checked. This occurs
   every 300 ms by default. ITPilot checks whether any new page is being
   loaded.
-  ``IEBrowser.BEGIN_NAVIGATION_CHECKS``. Used in the
   ``ExtendedWaitPages`` command, this determines the maximum number of
   times that it is checked whether there is a new browsing. The
   parameter has a default value equal to 5.
