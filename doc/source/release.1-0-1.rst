.. _pmm/release/1-0-1:

|pmm.name| |release|
********************************************************************************

:Date: June 10, 2016
:PMM Server: https://hub.docker.com/r/percona/pmm-server/
:PMM Client: https://www.percona.com/downloads/TESTING/pmm/pmm-client.tar.gz

Upgrading
=========

.. note:: This beta release introduces changes to the ``pmm-data`` container,
   which is used for storing collected data.
   To upgrade, you will need to remove and re-create this container,
   losing all your collected data.

1. Stop and remove the ``pmm-server`` container:

   .. prompt:: bash

      docker stop pmm-server && docker rm pmm-server

2. Remove the ``pmm-data`` container:

   .. prompt:: bash

      docker rm pmm-data

3. Create the :ref:`PMM data container <data-container>`.

4. Create and run the :ref:`PMM Server container <server-container>`.

5. :ref:`Upgrade PMM Client <deploy-pmm.updating>` on all your monitored hosts.

New Features
============

* **Grafana 3.0**: PMM now includes the latest version of Grafana
  for visualizing collected metrics data.

* **MongoDB Metrics**: With the addition of ``mongodb_exporter`` for Prometheus
  and MongoDB dashboards for Grafana,
  you can now use PMM for monitoring MongoDB metrics.

* **Consul**: Instead of ``prom-config-api``,
  PMM now uses Consul to provide an API service
  for communication between PMM Client and Prometheus.

* **Nginx**: PMM now uses Nginx, instead of a custom web server.

* **Server Summary**: Aggregated query metrics are now available in QAN.

* **MySQL InnoDB Metrics Advanced**: New dashboard for MySQL metrics.

* The web interface is now fully accessible via port 80.

  * ``/qan/``: Query Analytics
  * ``/graph/``: Metrics Monitor (Grafana)
  * ``/prometheus/``: Prometheus web UI
  * ``/consul/``: Consul web UI
  * ``/v1/``: Consul API

  The only other port is 9001 used by |qan| |api|.

* ``pmm-admin`` tool now  includes the ability to add MongoDB instance
  and specify the port after the address of the PMM Server.

.. |release| replace:: Beta 1.0.1

.. include:: .res/replace.txt
