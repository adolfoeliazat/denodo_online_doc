========
Get Page
========

.. rubric:: Description

This component lets the user obtain an active browser previously created
using the Create Persistent Browser component (see section :ref:`Create
Persistent Browser`).

.. rubric:: Input Parameters

browserId: browser identifier used to obtain the handler. If the
browserId parameter is null, a new persistent browser will be created
and its associated Page object will be returned.

.. rubric:: Output Values

This component returns a Page object that can be used as input for
navigation components such as Sequence or Extractor Sequence.

.. rubric:: Component Details

A description about how to use the persistent browsers management
components can be found in section :ref:`Create Persistent Browser`.

