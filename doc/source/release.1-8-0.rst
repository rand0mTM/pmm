.. _pmm/release/1-8-0:

|pmm.name| |release|
********************************************************************************

:Date: February 27, 2018

For more information about this release, see the `release announcement`_.

This release introduces many improvements in the user interface and
optimizations of performance.


.. contents::
   :local:

Improved: New landing page
================================================================================

:JIRA Ticket ID: :pmmbug:`1425`

The home page has been removed in favor of the |dbd.home| dashboard. The
|dbd.home| dashboard gives a general overview of your system and provides links
to useful resources. You can immediately go to a dashboard of your choice either
by using the conventional *Dashboard Dropdown* at the top of the page or the new
dashboard menu.

.. seealso::

   How to open a dashboard using |gui.dashboard-dropdown|
      :ref:`pmm.metrics-monitor.dashboard.opening`
   
Improved: Dashboard menu layout
================================================================================

:JIRA Ticket ID: :pmmbug:`1738`

The new dashboard menu provides a convenient grouping of dashboards making the
switching between dashboards easy: select a group, such as |mysql| or *OS*, and
choose from a short list in the menu that opens. The new dashboard menu is
available from every dashboard in |pmm|.

New: Automatic Memory Configuration
================================================================================

:pmmbug:`2046`

This release also features improved memory usage on the host system in
|pmm-server|. By default, |pmm| now automatically scales its use with system
size rather than using static amount of memory, unless you explicitly set the
maximum value (by using the |opt.metrics-memory| in |docker|, for example).

Bug fix releases
================================================================================

|tip.bug-fix-release-list| |release|:

- :ref:`pmm/release/1-9-1`

.. seealso::

   All releases
      :ref:`pmm/release/list`

Issues in this release
================================================================================

Release |release| of |pmm.name| contains new features, improvements, and bug
fixes registered in the following |jira| tickets:

.. rubric:: New Features

.. list-table::
   :widths: 20 80
   :header-rows: 1

   * - JIRA Ticket ID
     - Description
   * - :pmmbug:`1145`
     - Move |percona| dashboards to |grafana| plugin.
   * - :pmmbug:`1470`
     - Implement a custom |prometheus| rds_exporter to collect |amazon-cloudwatch| metrics.
   * - :pmmbug:`2046`
     - Configure memory consumption automatically for |pmm-server|. |pmm| now automatically scales its use with system size rather than using a static amount of memory.

.. rubric:: Improvements

.. list-table::
   :widths: 20 80
   :header-rows: 1

   * - JIRA Ticket ID
     - Description
   * - :pmmbug:`1425`
     - The home page has been removed in favor of the |dbd.home| dashboard to make the look and feel of |pmm| web interface more consistent.
   * - :pmmbug:`1738`
     - Add the *Dashboard* menu to make the discovery of dashboards easier.
   * - :pmmbug:`1976`
     - Apply the rules for computing CPU metrics; update the query that generates CPU utilization for the |dbd.mysql-overview|, |dbd.summary| and |dbd.system-overview| dashboards.
   * - :pmmbug:`2007`
     - Reduce the size of the |docker| container from ~475 MB to ~350 MB
   * - :pmmbug:`1711`
     - Add SQL and External Locks graphs to the |mysql-performance-schema| dashboard to show locks from perspective of count of events and from time.
   * - :pmmbug:`1763`
     - Update the forked |prometheus| exporter mysqld_exporter to the `latest upstream version <https://github.com/prometheus/mysqld_exporter>`_ to include recent bug fixes.
   * - :pmmbug:`2004`
     - Add the */ping* alias to nginx to enable a basic health check URL to the |pmm-server| API.
   * - :pmmbug:`1365`
     - Provide descriptions of all metrics in the |dbd.cross-server-graphs| dashboard.
   * - :pmmbug:`1343`
     - Provide descriptions of all metrics in the |dbd.summary| dashboard 
   * - :pmmbug:`1364`
     - Provide descriptions of all metrics in the |dbd.mysql-innodb-metrics| dashboard
   * - :pmmbug:`1994`
     - Provide descriptions of all metrics in the  |dbd.mysql-amazon-aurora| dashboard
   * - :pmmbug:`1922`
     - *pmm-server* as instance for all components on |pmm-server|
   * - :pmmbug:`2005`
     - Upgrade |orchestrator| to the latest stable release 
   * - :pmmbug:`1957`
     - Update Go to 1.9.4 which resolves a security problem in the go get command (For more information, see `Golang issue #23672 <https://github.com/golang/go/issues/23672>`_)
   * - :pmmbug:`2002`
     - In |qan|, elements of the |json| output can be collapsed; the text in the :guilabel:`Create Table` block can be copied.
   * - :pmmbug:`2128`
     - Improved formula for :guilabel:`Saturation Metrics` on the |dbd.system-overview| dashboard.

.. rubric:: Bug fixes

.. list-table::
   :widths: 20 80
   :header-rows: 1

   * - JIRA Ticket ID
     - Description
   * - :pmmbug:`2124`
     - The number of the new version is missing in the notification of the successful update of |pmm|
   * - :pmmbug:`1453`
     - mysqld_exporter wrong mapping of innodb_buffer_pool_pages 
   * - :pmmbug:`2029`
     - Deadlock when adding external exporter 
   * - :pmmbug:`1908`
     - RDS MySQL nodes do not display Current QPS 
   * - :pmmbug:`2100`
     - rds_exporter crashed after running for several minutes 
   * - :pmmbug:`1511`
     - PXC cluster is not recognized correctly when |mariadb| nodes are monitored
   * - :pmmbug:`1715`
     - |mongodb| |qan|: Wrong Per Query Stats
   * - :pmmbug:`1892`
     - Cannot detect tables in the Information Schema query
   * - :pmmbug:`1893`
     - In |qan|, the Tables element can be empty 
   * - :pmmbug:`1941`
     - Fingerprint storage bug 
   * - :pmmbug:`1933`
     - If some parts of collector got an error, whole collector is down
   * - :pmmbug:`1934`
     - mysqld_exporter/collector/info_schema_auto_increment.go fails if there are same table names but with different cases
   * - :pmmbug:`1951`
     - Regression in |pmm-admin.add| |opt.mysql-queries| only - not working
   * - :pmmbug:`2142`
     - Wrong calculation of the :option:`CPU Busy` parameter
   * - :pmmbug:`2148`
     - rds_exporter node_cpu metric in percents but node_exporter values are in seconds
  
.. |release| replace:: 1.8.0

.. _`release announcement`: https://www.percona.com/blog/2018/02/27/percona-monitoring-management-1-8-0-now-available/
		       
.. include:: .res/replace.txt
