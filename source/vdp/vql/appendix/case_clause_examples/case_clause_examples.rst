====================
CASE Clause Examples
====================

Consider the following Virtual DataPort view named ``internet_inc``:

+------------------------------+-----------------------+--------------------+
| id | summary                 | ttime                 | taxid              |
+====+=========================+=======================+====================+
| 1  | Error in ADSL router    | 2005-06-29 19:19:41.0 | B78596011          |
+----+-------------------------+-----------------------+--------------------+
| 2  | Incident in ADSL router | 2005-06-29 19:19:41.0 | B78596012          |
+----+-------------------------+-----------------------+--------------------+
| 3  | Install additional line | 2005-06-29 19:19:41.0 | B78596013          |
+----+-------------------------+-----------------------+--------------------+
| 4  | Bandwidth increase      | 2005-06-29 19:19:41.0 | B78596014          |
+----+-------------------------+-----------------------+--------------------+

**Examples**

**Example 1**



.. code-block:: sql

   SELECT id
       , summary
       , CASE 
           WHEN LEN(summary) > 22
               THEN summary
           ELSE id
           END
   FROM internet_inc

+-------------------------+-------------------------+-------------------------+
| id                      | summary                 | case                    |
+=========================+=========================+=========================+
| 1                       | Error in ADSL router    | 1                       |
+-------------------------+-------------------------+-------------------------+
| 2                       | Incident in ADSL router | Incident in ADSL router |
+-------------------------+-------------------------+-------------------------+
| 3                       | Install additional line | Install additional line |
+-------------------------+-------------------------+-------------------------+
| 4                       | Bandwidth increase      | 4                       |
+-------------------------+-------------------------+-------------------------+

**Example 2**



.. code-block:: sql

   SELECT id 
       , CASE
           WHEN id = 1 THEN true
           ELSE id
       END AS is_first
   FROM internet_inc


.. code-block:: sql

   Error executing sentence: Incorrect select sentence: CASE argument
   IINC_ID is not compatible with the rest of values.
   
   
The type of the *result* of the ``WHEN`` clause is incompatible with the
one of the ELSE clause. The first one has type *boolean* and the other,
*long*.

**Example 3**



.. code-block:: sql

   SELECT id
       ,CASE 
           WHEN id = 1
               THEN 'first'
           ELSE id
           END AS is_first
   FROM internet_inc

+--------------------------------------+--------------------------------------+
| id                                   | is\_first                            |
+======================================+======================================+
| 1                                    | first                                |
+--------------------------------------+--------------------------------------+
| 2                                    | 2                                    |
+--------------------------------------+--------------------------------------+
| 3                                    | 3                                    |
+--------------------------------------+--------------------------------------+
| 4                                    | 4                                    |
+--------------------------------------+--------------------------------------+

.. note:: If the type of the *results* of the ``WHEN`` or ``ELSE``
   clauses are not the same, they are automatically converted to obtain a
   valid result. In this case the *results* are converted to *String*.
   
**Example 4**

The CASE clause can also be used in the WHERE part of a query.



.. code-block:: sql

   SELECT *
   FROM internet_inc
   WHERE true = (
           CASE id
               WHEN 1
                   THEN true
               ELSE false
               END
           )

+--------------------+----------------------+-----------------------+-----------+
| id                 | summary              | ttime                 | taxid     |
+====================+======================+=======================+===========+
| 1                  | Error in ADSL router | 2005-06-29 19:19:41.0 | B78596011 |
+--------------------+----------------------+-----------------------+-----------+

**Example 5**

These two queries are equivalent and obtain the same result, but use
``CASE`` in different ways:

.. code-block:: sql
   :emphasize-lines: 3, 4

   SELECT id
       , CASE id
           WHEN CASE id
                   WHEN 1
                       THEN 1
                   ELSE 2
                   END
               THEN 'first'
           WHEN 2
               THEN 'second'
           ELSE 'other'
           END
   FROM internet_inc;

.. code-block:: sql
   :emphasize-lines: 3, 4

   SELECT id
       , CASE id
           WHEN CASE 
                   WHEN id = 1
                       THEN 1
                   ELSE 2
                   END
               THEN 'first'
           WHEN 2
               THEN 'second'
           ELSE 'other'
           END
   FROM internet_inc;
   
+--------------------------------------+--------------------------------------+
| id                                   | case                                 |
+======================================+======================================+
| 1                                    | first                                |
+--------------------------------------+--------------------------------------+
| 2                                    | first                                |
+--------------------------------------+--------------------------------------+
| 3                                    | other                                |
+--------------------------------------+--------------------------------------+
| 4                                    | other                                |
+--------------------------------------+--------------------------------------+

.. note:: CASE returns the result of the first ``WHEN`` clause that
   evaluates to true. In this example, the first and second ``WHEN``
   conditions are true, but it returns the result of the first one.
