=================================================================================================
Checking Navigation Sequences in Systems with Cookie-Based Session Authentication and Maintenance
=================================================================================================

Some Web sites use session authentication and maintenance techniques
based on cookies that can cause the failure of the reproduction of a
sequence using the *Play* button, even though, in fact, the sequence is
being generated correctly.



In particular, some Web sites only provide users with authentication
forms when they are accessing the system for the first time after
starting up the browser (or after a certain session expiry time lapses).



Thus, if during the generation of a sequence that requires
login/password authentication an attempt is made to reproduce said
sequence in a new browser window, it may happen that the reproduction
fails due to the fact that the session in the Web site is still open
(and, thus, it is not possible to locate the login/password form that
did appear, however, when it was being generated).



A similar situation can arise when, in a Web site, the effects of any
other navigation event vary according to whether or not a session has
been established.



The solution to this problem is very simple: the sequence is being
generated correctly and the only difficulty arises when checking it to
ensure that it is functioning correctly. To overcome this difficulty the
ITPilot sequence generation toolbar lets the user eliminate the session
“cookies” when starting a recording session.
