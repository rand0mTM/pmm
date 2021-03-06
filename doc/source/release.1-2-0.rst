.. _pmm/release/1-2-0:

|pmm.name| |release|
********************************************************************************

:Date: July 14, 2017

For install and upgrade instructions, see :ref:`deploy-pmm`.

Changes in PMM Server
=====================

The following changes were introduced in *PMM Server* 1.2.0:

Updated Components
------------------

* `Consul 0.8.5 <https://github.com/hashicorp/consul/blob/master/CHANGELOG.md#085-june-27-2017>`_

* `Grafana 4.3.2 <https://github.com/grafana/grafana/blob/master/CHANGELOG.md#432-2017-05-31>`_

* `Orchestrator 2.1.5 <https://github.com/github/orchestrator/releases/tag/v2.1.5>`_

* `Prometheus 1.7.1 <https://github.com/prometheus/prometheus/releases/tag/v1.7.1>`_

.. rubric:: New Features

* :pmmbug:`737`: New graphs in **System Overview** dashboard:

  - **Memory Advanced Details**

  - **Saturation Metrics**

* :pmmbug:`1090`: Added ESXi support for *PMM Server* virtual appliance.

UI Fixes
--------

* :pmmbug:`707`: Fixed QPS metric in **MySQL Overview** dashboard
  to always show *queries per second* regardless of the selected interval.

* :pmmbug:`708`: Fixed tooltips for graphs that displayed incorrectly.

* :pmmbug:`739`, :pmmbug:`797`: Fixed *PMM Server* update feature
  on landing page.

* :pmmbug:`823`: Fixed arrow padding for collapsible blocks in QAN.

* :pmmbug:`887`: Disabled the **Add** button when no table is specified
  for showing query info in QAN.

* :pmmbug:`888`: Disabled the **Apply** button in QAN settings
  when nothing is changed.

* :pmmbug:`889`: Fixed the switch between UTC and local time zone
  in the QAN time range selector.

* :pmmbug:`909`: Added message ``No query example``
  when no example for query is available in QAN.

* :pmmbug:`933`: Fixed empty tooltips for **Per Query Stats** column
  in the query details section of QAN.

* :pmmbug:`937`: Removed the percentage of total query time
  in query details for the ``TOTAL`` entry in QAN
  (because it is 100% by definition).

* :pmmbug:`951`: Fixed the **InnoDB Page Splits** graph formula
  in the **MySQL InnoDB Metrics Advanced** dashboard.

* :pmmbug:`953`: Enabled stacking for graphs
  in **MySQL Performance Schema** dashboard.

* :pmmbug:`954`: Renamed **Top Users by Connections** graph
  in **MySQL User Statistics** dashboard
  to **Top Users by Connections Created**
  and added the **Connections/sec** label to the Y-axis.

* :pmmbug:`957`: Refined titles for **Client Connections**
  and **Client Questions** graphs in **ProxySQL Overview** dashboard
  to mentioned that they show metrics for all host groups
  (not only the selected one).

* :pmmbug:`961`: Fixed the formula for **Client Connections** graph
  in **ProxySQL Overview** dashboard.

* :pmmbug:`964`: Fixed the gaps for high zoom levels
  in **MySQL Connections** graph on the **MySQL Overview** dashboard.

* :pmmbug:`976`: Fixed Orchestrator hadling by ``supervisorctl``.

* :pmmbug:`1129`: Updated the **MySQL Replication** dashboard
  to support new ``connection_name`` label
  introduced in ``mysqld_exporter`` for multi-source replication monitoring.

* :pmmbug:`1054`: Fixed typo in the tooltip for the **Settings** button in QAN.

* :pmmbug:`1055`: Fixed link to Query Analytics from Metrics Monitor
  when running *PMM Server* as a virtual appliance.

* :pmmbug:`1086`: Removed HTML code that showed up
  in the QAN time range selector.

.. rubric:: Bug Fixes

* :pmmbug:`547`: Added warning page to Query Analytics app
  when there are no PMM Clients running the QAN service.

* :pmmbug:`799`: Fixed Orchestrator to show correct version.

* :pmmbug:`1031`: Fixed initialization of **Query Profile** section in QAN
  that broke after upgrading Angular.

* :pmmbug:`1087`: Fixed QAN package building.

.. rubric:: Other Improvements


* :pmmbug:`348`: Added daily log rotation for nginx.

* :pmmbug:`968`: Added Prometheus build information.

* :pmmbug:`969`: Updated the Prometheus memory usage settings
  to leverage new flag.
  For more information about setting memory consumption by PMM,
  see `FAQ <https://www.percona.com/doc/percona-monitoring-and-management/faq.html#how-to-control-memory-consumption-for-prometheus>`_.

Changes in PMM Client
=====================

The following changes were introduced in *PMM Client* 1.2.0:

.. rubric:: New Features

* :pmmbug:`1114`: Added *PMM Client* packages for Debian 9 ("stretch").

.. rubric:: Bug Fixes

* :pmmbug:`481`, :pmmbug:`1132`: Fixed fingerprinting for queries
  with multi-line comments.

* :pmmbug:`623`: Fixed ``mongodb_exporter`` to display correct version.

* :pmmbug:`927`: Fixed bug with empty metrics for MognoDB query analytics.

* :pmmbug:`1126`: Fixed ``promu build`` for ``node_exporter``.
  
* :pmmbug:`1201`: Fixed ``node_exporter`` version.

.. rubric:: Other Improvements

* :pmmbug:`783`: Directed ``mongodb_exporter`` log messages to ``stderr``
  and excluded many generic messages from the default ``INFO`` logging level.

* :pmmbug:`756`: Merged upstream ``node_exporter`` `version 0.14.0 <https://github.com/prometheus/node_exporter/blob/master/CHANGELOG.md#v0140--2017-03-21>`_.

  Several collectors were deprecated in this release:

  * ``gmond`` - Out of scope.
  * ``megacli`` - Requires forking, to be moved to textfile collection.
  * ``ntp`` - Out of scope.

  It also introduced the following breaking change:

  * Collector errors are now a separate metric:
    ``node_scrape_collector_success``,
    not a label on ``node_exporter_scrape_duration_seconds``

* :pmmbug:`1011`: Merged upstream ``mysqld_exporter`` `version 0.10.0 <https://github.com/prometheus/mysqld_exporter/blob/master/CHANGELOG.md#v0100--2017-04-25>`_.

This release introduced the following breaking change:

* ``mysql_slave_...`` metrics now include an additional ``connection_name``
  label to support MariaDB multi-source replication.

.. |release| replace:: 1.2.0

.. include:: .res/replace.txt
