.. _pmm/release/1-1-4:

|pmm.name| |release|
********************************************************************************

:Date: May 29, 2017

For install and upgrade instructions, see :ref:`deploy-pmm`.

This release includes experimental support for MongoDB in Query Analytics,
including updated QAN interface.

Query Analytics for MongoDB
===========================

To enable MongoDB query analytics, use the ``mongodb:queries`` alias
when :ref:`adding the service <pmm-admin.add>`.
As an experimental feature, it also requires the ``--dev-enable`` option::

 sudo pmm-admin add --dev-enable mongodb:queries

.. note:: Currently, it monitors only collections that are present
   when you enable MongoDB query analytics.
   Query data for collections that you add later is not gathered.
   This is a known issue and it will be fixed in the future.

Query Analytics Redesign
========================

The QAN web interface was updated for better usability and functionality
(including the new MongoDB query analytics data).
The new UI is experimental and available by specifying ``/qan2``
after the URL of *PMM Server*.

.. note:: The button on the main landing page
   still points to the old QAN interface.

You can check out the new QAN web UI at https://pmmdemo.percona.com/qan2

Changes in PMM Server
=====================

* :pmmbug:`724`: Added the *Index Condition Pushdown (ICP)* graph
  to the *MySQL InnoDB Metrics* dashboard.

* :pmmbug:`734`: Fixed the *MySQL Active Threads* graph
  in the *MySQL Overview* dashboard.

* :pmmbug:`807`: Fixed the *MySQL Connections* graph
  in the *MySQL Overview* dashboard.

* :pmmbug:`850`: Updated the *MongoDB RocksDB* and *MongoDB WiredTiger*
  dashboards.

* :pmmbug:`890`: and :pmmbug:`891`: Removed the *InnoDB Deadlocks*
  and *Index Collection Pushdown* graphs
  from the *MariaDB* dashboard.
  Both graphs are available in the *MySQL InnoDB Metrics* dashboard.

* :pmmbug:`928`: Added tooltips with descriptions for graphs
  in the *MySQL Query Response Time* dashboard.
  Similar tooltips will be gradually added to all graphs.

Changes in PMM Client
=====================

* :pmmbug:`554`: Added options for ``pmm-admin``
  to enable MongoDB cluster connections.

* :pmmbug:`666` and :pmmbug:`746`: Fixed ``proxysql_exporter``.

* :pmmbug:`801`: Improved *PMM Client* upgrade process to preserve credentials
  that are used by services.

.. |release| replace:: 1.1.4

.. include:: .res/replace.txt
