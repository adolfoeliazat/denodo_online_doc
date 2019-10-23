===========================
Commands for Executing Code
===========================

List of commands for executing code:

.. contents::
   :depth: 1
   :local:
   :backlinks: none
   :class: twocols

EXECUTEJS
=========================================

Executes a JavaScript code on the current page.

**Parameters**

-  String js: JavaScript code to be executed.

**Returns**

   Nothing.


EXECUTEJSBEFORELOAD
=========================================

Executes a piece of JavaScript code in subsequent navigations, before
the page we are navigating to dispatches the “onload” event.

**Parameters**

-  String js: JavaScript code to be executed.
-  boolean persistent: If set to *true*, the JavaScript code will be
   executed in all subsequent navigations after this command is
   executed. If set to *false*, it will be executed only in the first
   navigation after the execution of the command.

**Returns**

   Nothing.

