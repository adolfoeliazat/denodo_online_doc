====================
Internationalization
====================

Virtual DataPort can integrate data sources from different countries and
format the output data with the format expected by the country (locale).

For example, Virtual DataPort can display the results of queries in a
specific time zone independent of the zone used by the data sources
(e.g. showing numbers with the decimal separator of Spanish, even if the
data was extracted from a Web service with a U.S locale).

For this, it is needed to have an internationalization configuration for
each of the countries/locations from which data handled by the Virtual DataPort
server can come, represented by a map of the type i18n (see section :ref:`Defining a Map`). Virtual DataPort includes maps already created for
the most common configurations of many countries. The name of those
configurations uses the standard prefix defined in the standard `ISO-3166 (Country Codes) <https://www.iso.org/iso-3166-country-codes.html>`_.
E.g., Spain ("es_euro"), Great Britain ("gb"),
France ("fr"), United States ("us"), etc.

The section :ref:`Managing Internationalization Configurations`
describes how to add new internationalization configurations. That is,
define new i18n maps.

Lastly, it is important to bear in mind that the default format to be
used to write *date*, *float* and *double* constants in the queries on a view is
established by the internationalization configuration being used. See
section :ref:`Managing Internationalization Configurations` for more
information about the parameters of an internationalization
configuration and section :ref:`Describing Catalog Elements` to know how to
obtain the parameters assigned to a certain configuration. The appendix
:doc:`/vdp/vql/appendix/syntax_of_condition_functions/date_processing_functions` lists all the functions that process date
values that may be useful to express them in the required format.

