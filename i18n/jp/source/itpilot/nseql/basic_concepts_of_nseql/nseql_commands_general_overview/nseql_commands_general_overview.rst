================================
NSEQL Commands: General Overview
================================

NSEQL includes commands for reproducing in an automatic fashion the main
actions that can be performed manually with an Internet browser. More
specifically:

-  The command *navigate* facilitates navigation to a URL (as if a user
   had typed the URL in the browser address bar).
-  *goBack*, *goForward*, *refresh* and *stop* allow actions to be
   executed as if the *back*, *forward*, *refresh* (also
   called *reload*) and *stop* buttons of the browser had
   been clicked.
-  The command family *clickOnAnchorByXXX* and *clickOnAreaByXXX* is
   used to locate a link or an HTML map in the current browser page and
   execute a click on it.
-  The *selectAnchorByXXX* group of commands allows for a link to be
   selected within the current browser page. Further actions can be
   carried out on it and/or its contents once the link is selected with
   this command.
-  The command family *findFormByXXX* locates and selects a form to
   apply actions to its elements through subsequent commands.
-  The command families *setInputValue*, *setTextAreaValue* and
   *selectIndexByXXX* assign values to fields of the selected form.
-  The commands *submitForm*, *clickOnElement*, *clickOnInputElement*,
   *fireEvent* execute actions on elements in the last form selected.
-  The command family *findFrameByXXX* selects a *frame* of the current
   browser page by its name, source or position so that actions invoked
   thereafter refer to this *frame*. With *resetFrame* the active frame
   becomes the home page again.
-  The command family *findElementByXXX* locates and selects an element
   in the current *frame*. Different location criteria can be used:
   element type, attribute values, content text, position,and xpath
   expressions.
-  The command family *findChildElementByXXX* lets users locate and
   select elements that are children of the currently selected element.
   It is possible to use different criteria to locate them: element
   type, value of a specific attribute, contained text, position …
-  The command family *selectOptionByXXX* lets users choose one option
   in a previously selected <*select*> element.
-  The command *setChecked* lets users check or uncheck a previously
   selected *<input type=”checkbox”>* element.
-  *clickOnSelectedElement*, *fireEventOnSelectedElement* and
   *transposeSelectedTable* execute actions on the last element
   selected.
-  A navigation sequence can involve several browser windows. The NSEQL
   commands for manipulating said windows are *allowOpenNewWindow*,
   *selectWindow* and *closeWindow*. New windows can also be created
   through *openNewWindow* and set or access the window name through
   *setWindowName* and *getWindowName*.
-  The command *waitPages* makes the browser wait until a specific
   number of pages are downloaded before executing the remainder of the
   NSEQL program commands. The command *extendedWaitPages* performs this
   same operation, but allowing the system to guess the number of pages
   required to continue the navigation. The *wait* command allows to
   wait a fixed amount of time, specified as an input parameter.
-  The command *postData* directly issues POST-type http requests.
-  The command *setAmbientProperties* configures the elements of the
   pages that should be downloaded and executed by the browser (e.g.
   images, Java applets, ActiveX controls, etc.). Another command for
   configuring browser behavior is *setProxyAuthInfo*.
-  The commands *getSourceCode*, *queryDownloadedPages*, *getTitle,*
   *getLocation* and others obtain certain data from the current page,
   the current form or the last navigation. These data can be useful for
   debugging the NSEQL programs (see the :doc:`/itpilot/generation_environment/index` for a
   description of the generation environments of NSEQL programs that are
   available in ITPilot).
-  The commands *onDialog*, *onPrintDialog*, *onWebPageDialog* and
   OnCertificateDialogDenodoBrowser allow managing pop-up browser
   windows, performing tasks such as placing content in dialog text
   fields, pressing specific buttons, moving cursors, and so on.
-  The commands *convertWordToHtml, convertExcelToHtml* and
   *convertPDFToHtml* turn a Microsoft Word/Microsoft Excel/Adobe PDF
   resource into an HTML resource to allow for the subsequent extraction
   of information using ITPilot.
-  The command *saveFile* downloads a resource and saves it to the
   local filesystem.
-  The *executeJS* and *ExecuteJSBeforeLoad* commands allow the
   execution of JavaScript code on a browser-loaded page.
-  The *cancelNextNavigation* command lets the user cancel the
   navigation triggered by an event, to allow processing the page as it
   is before the action (e.g. with the real URLs which are going to be
   accessed after the JavaScript code execution).
-  The commands *openWebPageDialogsInNewWindow*,
   *OpenAlertDialogsInNewWindow* and *OpenNewWindowsUnnamed* let users
   enable and disable different JavaScript functions in order to
   simplify certain browsing and data extraction actions.
-  The commands *clearPersistentCookies* and *clearCache* allow the user
   to clear the browsing data.
