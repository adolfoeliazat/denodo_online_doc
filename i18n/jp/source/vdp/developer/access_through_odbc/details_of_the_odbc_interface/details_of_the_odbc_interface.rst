==========================================
Details of the ODBC Interface
==========================================

This section describes information specific to the ODBC interface of Denodo.

.. contents::
   :local:
   :backlinks: none
   :depth: 1
   
How the ODBC Interface Reports the Datetime and Interval Data Types
===================================================================

The ODBC interface of Denodo follows the same protocol as PostgreSQL.

The table below lists how the data types of Denodo are mapped to the data types of PostgreSQL.

.. csv-table:: 
   :header: "Type Name in Denodo", "Type Name for PostgreSQL", "Type Code for PostgreSQL"
   
   "localdate", "date", "1082"
   "time", "time", "1083"
   "timestamp", "timestamp", "1114"
   "date (deprecated)", "timestamptz", "1184"
   "timestamptz", "timestamptz", "1184"
   "interval_year_month", "interval", "1186"
   "interval_day_second", "interval", "1186"

The types date and timestamptz are reported with the same type (TIMESTAMP WITH TIMEZONE) so a client application cannot distinguish them. This facilitate the migration from versions prior to Denodo 7.0. Client applications do not need to distinguish between these types and treat both as timestamptz.