.. _pmm/release/1-1-3:

|pmm.name| |release|
********************************************************************************

:Date: April 21, 2017
:PMM Server: https://hub.docker.com/r/percona/pmm-server/
:PMM Client: https://www.percona.com/downloads/pmm-client/

For install instructions, see :ref:`deploy-pmm`.

New in PMM Server
=================

* :pmmbug:`649`: Added the *InnoDB Page Splits* and *InnoDB Page Reorgs*
  graphs to the *MySQL InnoDB Metrics Advanced* dashboard.

* Added the following graphs to the *MongoDB ReplSet* dashboard:

  * Oplog Getmore Time
  * Oplog Operations
  * Oplog Processing Time
  * Oplog Buffered Operations
  * Oplog Buffer Capacity

* Added descriptions for graphs in the following dashboards:

  * MongoDB Overview
  * MongoDB ReplSet
  * PMM Demo

New in PMM Client
=================

* :pmmbug:`491`: Improved ``pmm-admin`` error messages.

* :pmmbug:`523`: Added the ``--verbose`` option for ``pmm-admin add``.

* :pmmbug:`592`: Added the ``--force`` option for ``pmm-admin stop``.

* :pmmbug:`702`: Added the ``db.serverStatus().metrics.repl.executor`` stats
  to ``mongodb_exporter``.
  These new stats will be used for graphs in future releases.

* :pmmbug:`731`: Added real time checks to ``pmm-admin check-network`` output.

* The following commands no longer require connection to *PMM Server*:

  * ``pmm-admin start --all``
  * ``pmm-admin stop --all``
  * ``pmm-admin restart --all``
  * ``pmm-admin show-passwords``

  .. note:: If you want to start, stop, or restart a specific service,
     connection to *PMM Server* is still required.

.. |release| replace:: 1.1.3

.. include:: .res/replace.txt
