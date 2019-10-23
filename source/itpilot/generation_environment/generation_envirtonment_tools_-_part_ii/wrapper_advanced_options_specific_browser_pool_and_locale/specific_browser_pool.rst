=====================
Specific Browser Pool
=====================


Motivation and Overview
=================================================================================



It often occurs that navigation sequences executed by a specific wrapper
share a series of initial common steps; for example, imagine that a
wrapper has been created to automate the search process in a specific
Web source. The source requires an authentication process that involves
the introduction of a user name and a password. In our example, let us
imagine that the wrapper uses the same login/password pair for all
source accesses.



Using Denodo ITPilot to create this wrapper, an initial navigation
sequence would be created that would execute the following steps:

#. Connect to the source home page.
#. Complete the authentication form with the login/password and press
   the “Submit” or “Enter” button to authenticate.
#. Once authenticated, click on the link that accesses the search page.
#. Complete the search form with the required query.
#. The server returns a page with the query results.

The first three steps are common to all queries made to the wrapper. The
difference between one query and the next only arises in step four, when
the search form is completed according to the specific query to be made
at each moment in time.



It would be nice to save time on these first three steps in each query:
ideally, when a new query is received, one browser is already
authenticated and situated in the search page of the source to which the
new request could be assigned. The browser searches immediately (step 4)
and returns the results (step 5), thus avoiding wasting time in steps
1-3.



Denodo ITPilot supports this “intelligent” reuse of browsers by enabling
a specific browser pool for a wrapper (see :ref:`Wrapper options dialog`). A
wrapper-specific browser pool consists on a set of browsers, of a
certain type, reserved to be used exclusively by a wrapper. These
wrapper-specific pools work as follows:

-  When the pool creates a new browser to be used by this wrapper, it
   will execute an “initial sequence” common to all the executions (e.g.
   steps 1-3 in the above example).
-  When the wrapper execution ends, it should situate the browser in the
   same page it found it (e.g. the search form page, in the above
   example). This way, the browser is ready to be used in the next
   wrapper execution without executing the initial common sequence.
-  Other wrappers should not use the browsers created to be used by this
   wrapper.



Basic Configuration
=================================================================================

To configure the current wrapper to use a wrapper-specific browser pool,
the option “Enable specific Browser Pool for this wrapper” must be
selected (see :ref:`Wrapper options dialog`).



The following basic parameters can be configured:

-  Browser Type. Type of the browsers to be used by the specific pool:
   MSIE or Denodo Browser.
-  Max pool size. Maximum number of browsers that can be used by this
   specific pool. When the limit is reached an error will be returned if
   a new browser needs to be created.
   
   .. note:: Browsers created inside specific browser pools count toward
      the global browser limit. That means that if the global browser pool
      has a limit of N browsers for a certain browser type (MSIE or Denodo Browser),
      and M browsers of that type are being used by specific browser pools, then
      the global browser pool could only contain other N-M browsers of that type.
      See section :ref:`Browser Lifecycle` for further detail.
   
   .. note:: When a wrapper is configured to use a specific pool, only the initial
      browser used by each wrapper execution is obtained from that specific browser pool.
      If more browsers are required during the execution (e.g. because the wrapper access
      several pages in parallel), they are requested to the global browser pool.
   
-  Min pool size. When the pool is started, the number of browsers
   indicated by this parameter are created (as it is explained later,
   the specific browser pool will be started the first time the wrapper
   is executed).
-  Initialization Sequence. It represents the constant common sequence
   needed in all wrapper executions. This sequence is executed each time
   a new browser of the pool is created, to situate it in the page that
   the wrapper expects (e.g. it can be used to submit a login/password
   form which is common to all wrapper executions).
   
   .. note:: The initialization sequence cannot contain variables
   
-  Back Sequence. It is executed each time a wrapper execution ends, and
   it should be used to situate the browser in the same page it was
   located before the wrapper execution (i.e. it should situate the
   browser in the same page as the Initialization Sequence, to prepare
   it for the next wrapper execution).
-  Browser TTL. Maximum Time To Live of a browser of this pool. Some
   versions of MSIE have memory leaks and their performance degrades
   after they have been running for a certain time. To prevent this
   situation we can use this parameter. When the pool returns a browser
   to the wrapper using it, it checks if the browser has been running
   more than the specified TTL. In that case, the browser is destroyed
   and a new one is created to substitute it, with the same page loaded
   (the session cookies are transferred from the old browser to the new
   one). A value of 0 for the parameter indicates that this
   functionality is disabled.



Browser Lifecycle
=================================================================================

The specific browser pool for a wrapper is created the first time the
wrapper is executed. When the pool is created, a number of browsers
equal to the value of the parameter “min pool size” are started and the
Initialization Sequence is executed. If not all those browsers can be
started because the browser limit has been reached (i.e. the sum of the
browsers in specific pools and the general pool has reached the general
pool limit) a warning is logged.



At execution time, when the associated wrapper asks for a browser to the
specific browser pool, if there is any free/unlocked browser, then it is
returned to the wrapper and it is marked as locked. If there are not
free/unlocked browsers:


-  If the specific pool contains the configured maximum number of browsers,
   then an error is thrown (maximum number of browsers in the specific pool
   has been reached).

-  If the specific pool does not contain the maximum number of browsers:

   -  If the sum of the number of browsers, of the corresponding type, in
      all the pools (specific pools and global pool) is below the global
      pool limit, then a new browser is created in the specific pool.
   -  If the global limit to the number of browsers has been reached, an
      unlocked/free browser in the general pool is shifted to the
      wrapper-specific pool (the free browser from the global pool is
      destroyed and a new one is created in the specific pool). If no such
      unlocked/free browser can be found, an error is thrown (global
      maximum number of browsers has been reached).




When the associated wrapper ends an execution and returns a browser to
the specific browser pool, the browser is marked as free/unlocked.



Note that once a wrapper is in a wrapper-specific pool, it never returns
to the general pool. It can be destroyed, however (see section :ref:`Managing
Sessions` for further detail).


Managing Sessions
=================================================================================

Web applications which make use of sessions are usually configured with
a session timeout. If a session becomes inactive and the timeout
expires, the session is destroyed.



If the initialization sequence (executed each time a browser is created
in the specific pool) creates a HTTP session in a web application, but
afterwards the browser remains unused in the pool for a time longer than
the session timeout of the web application, then the session is
destroyed in the web server, and the next time a wrapper tries to use
that browser it will fail. To avoid these situations, we can configure
the wrapper-specific browser pool to execute a “refresh” sequence on
browsers which have been idle for a certain interval, and in this way
maintain their HTTP sessions active. The following three configuration
parameters can be used:

-  Max idle time to refresh. When this timeout expires without activity
   in a browser (i.e. the browser remains in the pool without being
   used), then the “Touch Session Sequence” is executed.
-  Refresh Validation Interval. It indicates the interval used to check
   if the max idle time to refresh has expired in any browser.
-  Touch Session Sequence. Sequence executed when the max idle time to
   refresh expires, to maintain active a HTTP session. After executing
   this sequence the browser should be located in the same page as
   before (sometimes a simple reload -NSEQL command Refresh- can be
   enough).



A value of 0 for the parameter Max idle time to refresh or Refresh
Validation interval indicates that this functionality is disabled.





Batch Executions
=================================================================================



In some situations a wrapper is scheduled to be executed in batches, and
the interval between each batch of executions can be considerable (e.g.
a wrapper is scheduled to be executed several times from 10a.m. to
11a.m. every day). If we use a specific browser pool, its browsers will
sit idle most of the time, while still counting toward the global
browser limit. Furthermore, there is an overhead in maintaining the HTTP
sessions alive through the idle times (in the previous example, from
11a.m to 10a.m. of the next day).



An alternative consists in destroying the browsers of the pool when they
are not used for a certain amount of time. This way, when the next batch
of executions starts, the browsers are created again and the
initialization sequence is executed. The following two configuration
parameters can be used for this purpose:

-  Max idle time to destroy. When this timeout expires without any
   activity having taken place in the browser (i.e. the browser remains
   in the pool without being used), then the browser is destroyed and
   removed from the pool (no other browser is created to substitute it
   at that moment).
-  Destroy validation interval: It indicates the interval used to check
   if the max idle time to destroy has expired in any browser.



A value of 0 for any of the two parameters indicates that this
functionality is disabled.





Error Management
=================================================================================

During the execution of the configured sequences, the following error
events are considered and controlled:

-  If the Touch Session Sequence fails, then the Initialization Sequence
   is executed. If this sequence also fails, then the browser is
   destroyed and removed from the pool.
-  If the Initialization Sequence fails when the browser is created,
   then the browser is destroyed and removed from the pool.
-  If the Back Sequence fails, then the Initialization Sequence is
   executed. If this sequence also fails, then the browser is destroyed
   and removed from the pool.




