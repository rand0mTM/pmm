.. _pmm/release/1-4-1:

|pmm.name| |release|
********************************************************************************

:Date: November 2, 2017

|percona| announces the release of |pmm.name| |release| on Thursday, November
2nd, 2017.  This release contains fixes to bugs found after |pmm.name|
|prev-release| was released. It also introduces two important improvements. The
`btrfs`_ file system has been replaced with *XFS* in AMI and OVF images and the
|prometheus| dashboard has been enhanced to offer more information.

For install and upgrade instructions, see :ref:`deploy-pmm`.

.. rubric:: Improvements

* :pmmbug:`1567`: The `btrfs`_ file system has been replaced with *XFS* in
  :ref:`AMI and OVF images <deploy-pmm.server.installing>` to meet the
  requirements of `AWS Marketplace`_.
* :pmmbug:`1594`: In |metrics-monitor|, the |prometheus| dashboard has
  been updated to show more information about the running |prometheus|
  jobs and help estimate their efficiency.
  
.. rubric:: Bug fixes

* :pmmbug:`1620`: In some cases, |pmm| could not be :ref:`upgraded <deploy-pmm.updating>` to
  version |prev-release| by using the :guilabel:`Update` button on the landing
  page.
* :pmmbug:`1633`: |qan| would show error List of Tables is Empty for instances
  having been upgraded from earlier releases of |pmm|, due to incorrect values
  being stored in the database.  This has been addressed to identify the
  incorrect values and replace with accurate schema and table information.
* :pmmbug:`1634`: The |dbd.advanced-data-exploration| dashboard did
  not always display data points from |opt.external-metrics| monitoring
  services due to a too restrictive Grafana Template filter.

* :pmmbug:`1636`: Special characters prevented the removal of
  |opt.external-metrics| services using the |pmm-admin.remove| command.

.. |release|      replace:: 1.4.1
.. |prev-release| replace:: 1.4.0

.. _btrfs: https://btrfs.wiki.kernel.org
.. _`aws marketplace`: https://aws.amazon.com/marketplace

.. include:: .res/replace.txt
