.. _pmm/release/1-1-2:

|pmm.name| |release|
********************************************************************************

:Date: April 3, 2017
:PMM Server: https://hub.docker.com/r/percona/pmm-server/
:PMM Client: https://www.percona.com/downloads/pmm-client/

For install instructions, see :ref:`deploy-pmm`.

PMM Server
================================================================================

* Updated to latest versions:

  * `Grafana 4.2 <https://grafana.com/blog/2017/03/22/grafana-4.2-release/>`_

  * `Consul 0.7.5 <https://github.com/hashicorp/consul/blob/master/CHANGELOG.md#075-february-15-2017>`_

  * `Prometheus 1.5.2 <https://github.com/hashicorp/consul/blob/master/CHANGELOG.md#075-february-15-2017>`_

  * `Orchestrator 2.0.3 <https://github.com/github/orchestrator/releases/tag/v2.0.3>`_

* Migrated *PMM Server* to use CentOS 7 as base operating system.

* Changed the entrypoint so that supervisor is PID 1.

* Added the following dashboards:

  * MongoDB InMemory

  * MongoDB MMAPv1

  * MariaDB

* :pmmbug:`633`: Set the following default values in :file:`my.cnf`:

  .. code-block:: text

     [mysqld]

     # Default MySQL Settings
     innodb_buffer_pool_size=128M
     innodb_log_file_size=5M
     innodb_flush_log_at_trx_commit=1
     innodb_file_per_table=1
     innodb_flush_method=O_DIRECT

     # Disable Query Cache by default
     query_cache_size=0
     query_cache_type=0

* :pmmbug:`676`: Added descriptions for graphs
  in Disk Performance and Galera dashboards.

PMM Client
==========

* Fixed ``pmm-admin remove --all`` to clear all saved credentials.

* Several fixes to ``mongodb_exporter``
  including :pmmbug:`629` and :pmmbug:`642`.

* :pmmbug:`504`: Added ability to change the name of a client
  with running services::

   $ sudo pmm-admin config --client-name new_name --force

  .. warning:: Some Metrics Monitor data may be lost.

.. |release| replace:: 1.1.2

.. include:: .res/replace.txt
