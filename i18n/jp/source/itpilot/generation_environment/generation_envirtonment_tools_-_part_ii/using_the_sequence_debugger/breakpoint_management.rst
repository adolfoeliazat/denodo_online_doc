=====================
Breakpoint Management
=====================

A breakpoint is a marker that can be placed in a command to notify the
debugger that the execution of the sequence has to be paused before
executing said command.



When a command has a breakpoint, it is marked with a black dot in the
first column, to the left of the command. A breakpoint can be in two
states:

-  Enabled means that the debugger will stop at the breakpoint. This
   state is marked represented with a black dot.
-  Disabled means that the command contains a breakpoint but the
   debugger will ignore it when executing the sequence. This state is
   marked as a black dot with a line crossing it.



These are the available options for managing breakpoints:

-  Adding a breakpoint: either double-click the first column of a
   command, or right-click the command and select the Toggle breakpoint
   option. A black dot will appear in the first column to mark the
   breakpoint.
-  Removing a breakpoint: either double-click the first column of a
   command that already has a breakpoint set, or right-click the command
   and select the Toggle breakpoint option. The black dot of the
   breakpoint will disappear.
-  Disabling a breakpoint: right-click the command with the breakpoint
   and select the Disable breakpoint option. The black dot of the
   breakpoint will be crossed with a line.
-  Enabling a breakpoint: right-click the command with the disabled
   breakpoint and select the Enable breakpoint option. The crossed black
   dot will be replaced with a regular black dot.

Some of the options have a variant in the popup menu that applies to all
commands at the same time: Remove all breakpoints, Disable all
breakpoints and Enable all the breakpoints.
