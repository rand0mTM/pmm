.. _pmm/release/1-1-0:

|pmm.name| |release|
********************************************************************************

:Date: February 7, 2017
:PMM Server: https://hub.docker.com/r/perconalab/pmm-server/
:PMM Client: https://www.percona.com/downloads/TESTING/pmm/

.. note::

   This beta release is highly experimental
   with features that are not ready for production.
   Do not upgrade to it from previous versions.
   Use it only in a test environment.

For install instructions, see :ref:`deploy-pmm`.

Changes
=======

Introduced Amazon Machine Image (AMI) and VirtualBox images for PMM Server:

* OVA image for VirtualBox is available from the
  `testing download area
  <https://www.percona.com/redir/downloads/TESTING/pmm/>`_.

* Public Amazon Machine Image (AMI) is
  `ami-9a0acb8c <https://console.aws.amazon.com/ec2/v2/home?region=us-east-1#Images:visibility=public-images;imageId=ami-9a0acb8c>`_.

**New in PMM Server:**

* Grafana 4.1.1

* Prometheus 1.5.0

* Consul 0.7.3

* Updated the **MongoDB ReplSet** dashboard
  to show the storage engine used by the instance

* :pmmbug:`551`: Fixed |qan| changing query format
  when a time-based filter was applied to the digest

**New in PMM Client:**

* :pmmbug:`530`: Fixed |pmm-admin| to support special characters
  in passwords

* Added displaying of original error message
  in ``pmm-admin config`` output

**Known Issues:**

* Several of the MongoDB metrics related to MongoRocks engine
  do not display correctly.
  This issue will be resolved in the GA production release.

.. |release| replace:: 1.1.0 Beta

.. include:: .res/replace.txt
