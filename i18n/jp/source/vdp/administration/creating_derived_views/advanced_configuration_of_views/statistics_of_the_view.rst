======================
Statistics of the View
======================

The Cost-based optimization uses the statistics of the views to estimate
the cost of alternative execution plans for queries.

To gather the statistics of a view, select **Enable statistics** and
then, click **Gather statistics for selected fields**. The Tool will
instruct the Server to gather and store the statistics of the view:
number of rows, number of different values of each field, etc. After the
process has finished, the table below will be filled in. You can also
set the statistics manually by entering the **Number of rows** and/or
filling in the values in the table below.

The statistics of a view can be disabled by clearing the **Enable
statistics** box. When the statistics of a view are disabled, they are
not deleted but they will not be used in the cost-based optimization
process.

The section :ref:`Cost-Based Optimization` explains how the cost-based
optimization works.

The statistics of a view are not included in version control because
they are environment-dependent.

