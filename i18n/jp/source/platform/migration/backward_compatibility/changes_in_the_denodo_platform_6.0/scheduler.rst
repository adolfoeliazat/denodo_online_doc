========================
Changes in Scheduler 6.0
========================

Counters for the Number of Documents and Errors Are Stored in a Long
============================================================================

As to Denodo 5.5, the counters for the number of
extracted/exported/cached tuples and errors were stored in an Integer
data type. Starting with Denodo 6.0, those counters are stored in a Long
one. This affects the methods from ``AbstractExtractionJobReport`` (and
its subclasses) and ``JobData`` (and its subclasses).


