=========================================
Embedded Denodo Browser Configuration
=========================================

The Denodo Browser can be executed from the ITPilot Wrapper Server
without having to start a Browser Pool Server in a separate process,
since the Wrapper Server internally manages a Denodo Browser pool
itself. At wrapper generation time, a user can choose if a separate
Browser Pool will be used to run the Denodo Browser clients, or if they
will be executed in the internal pool of the Wrapper Server (please see
:doc:`/itpilot/generation_environment/index`). The parameters are basically the same ones that
were described in section :ref:`MSIE and Denodo browser configuration`
for the Denodo Browser, with additional parameters for configuring
document conversion, as described in sections :ref:`PDF conversion
configuration`, :ref:`Word / Excel Conversion Configuration` and :ref:`Conversion configuration`.
