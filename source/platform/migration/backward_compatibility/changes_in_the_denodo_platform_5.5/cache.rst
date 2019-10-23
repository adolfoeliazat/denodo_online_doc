=====
Cache
=====

Default Fetch Size
============================================================================

Starting with Denodo 5.5, the default fetch size when retrieving data
from the cache data source is now set to 1,000. In Denodo 4.7 and
earlier, it is 10.

In most cases, the data is retrieved much faster with a fetch size of
1,000. However, when the number of rows is small but very big, we
recommend testing if this value meets your performance needs. Rows are
big when they contain several fields of type blob that occupy several
megabytes. This can happen when the blob fields contain big pictures,
long PDF documents, etc.


