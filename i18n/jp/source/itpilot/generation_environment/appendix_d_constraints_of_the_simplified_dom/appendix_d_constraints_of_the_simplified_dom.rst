=============================================
Appendix D: Constraints of the Simplified DOM
=============================================

The simplified DOM option described in the section :ref:`Preferences of Denodo
Browser` delimits the number of HTML tags that are taken into account
by the DOM tree that is being used internally by the Denodo Browser.
Besides, only certain attributes of those tags are processed. This
appendix lists, for each accepted HTML tag, its processed attributes.



-    A: HREF, ALT, TARGET, ID, REL, ON_CLICK
-    AREA: HREF, ALT, TARGET, ID, ON_CLICK
-    BASE: HREF, TARGET, ID
-    BODY: ONLOAD
-    BUTTON: NAME, VALUE, ID, ON_CLICK
-    FORM: ACTION, TARGET, METHOD, ID, NAME, ON_SUBMIT
-    FRAME: SRC, ID, NAME, ONLOAD
-    HTML
-    IFRAME: SRC, ID, NAME, ONLOAD
-    IMAGE: SRC, ON_CLICK, ID, ALT, TITLE
-    INPUT: NAME, VALUE, TYPE, CHECKED, ID, SRC, ON_CLICK, ALT
-    MAP: NAME, ID
-    OPTION: NAME, VALUE, ID, SELECTED
-    SCRIPT: SRC, ID, LANGUAGE, ONREADYSTATECHANGE, DEFER
-    SELECT: NAME, MULTIPLE, ID, ON_CHANGE
-    FRAMESET
-    TEXTAREA: NAME, ID, VALUE, ON_CLICK
