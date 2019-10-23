==============
Defining a Map
==============

A map represents a list of key-value pairs. The following types of maps
exist:

-  ``simple``. This type of maps can be used with the functions ``MAP``
   (see appendix :ref:`MAP`) and ``REPLACEMAP`` (see appendix
   :ref:`REPLACEMAP`).
-  ``i18n``. This type of maps represents internationalization
   configurations referring to specific locations. Some examples of
   parameters configured through these files are: symbols used as
   decimal, date format, etc.
   
   Virtual DataPort has i18n maps for the most common
   internationalization settings and these maps cannot be replaced or
   deleted. However, you can create new ones.
   
   See section :ref:`Internationalization` for more details about the
   internationalization features of Virtual DataPort.

To create maps, use the statement ``CREATE MAP``. You have to specify the type of the map
(``I18N`` or ``SIMPLE``), the map name and the list of key-value pairs
that form the map.


.. code-block:: bnf
   :caption: Syntax of the CREATE MAP statement
   :name: Syntax of the CREATE MAP statement

   CREATE MAP { I18N | SIMPLE } <name:identifier>
       ( [<name:literal> = <value:literal>]+ ) )


The following example creates the *simple* map ``currency_codes``, which
maps some of the ISO 4217 currency codes to its name.



.. code-block:: vql
   :caption: Creation of a map of type simple
   :name: Creation of a map of type simple

   CREATE MAP SIMPLE currency_codes (
       'AED' = 'United Arab Emirates dirham'
       'AFN' = 'Afghan afghani'
       'ALL' = 'Albanian lek'
       'AMD' = 'Armenian dram'
       'ANG' = 'Netherlands Antillean guilder'
       'AOA' = 'Angolan kwanza'
       'ARS' = 'Argentine peso'
       'AUD' = 'Australian dollar'
       'AWG' = 'Aruban florin'
       'AZN' = 'Azerbaijani manat '
       'BAM' = 'Bosnia and Herzegovina'
   );


Virtual DataPort establishes a dependency between the custom maps you
create and the views that use them.

If a user tries to delete a map that has a dependency with another view,
the statement will fail if it does not have the ``CASCADE`` clause at
the end. If you add this clause, the Server will delete the custom map
*and* the elements that depend on them.

