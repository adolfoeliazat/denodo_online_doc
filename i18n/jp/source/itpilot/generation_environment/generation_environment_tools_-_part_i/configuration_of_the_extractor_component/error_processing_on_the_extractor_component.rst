===========================================
Error Processing on the Extractor Component
===========================================



The contextual menu can be used to open the Extractor component error
configuration dialog, as it was explained in section :ref:`Error Processing on the Web Browsing Automation`. In addition to the *Runtime error* explained in that
section, the Extractor component adds the *Invalid record error*. This
error occurs if the structure of the records obtained by the DEXTL
specification does not match the record structure defined in the wizard
at generation time. This can happen, for instance, if the value
extracted for a field cannot be converted to the type specified for the
field in the record structure. This error can be handled by *raising* it
(wrapper execution stops) or ignoring it (wrapper execution continues).
Besides, the actions *Trace record* and *Output record* (see section :ref:`Error Processing on the Web Browsing Automation`) can be executed with this type of error.



