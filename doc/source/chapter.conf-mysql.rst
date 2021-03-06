.. _conf-mysql:

Configuring |mysql| for Best Results
********************************************************************************

|pmm| supports all commonly used variants of |mysql|, including
|percona-server|, |mariadb|, and |amazon-rds|.  To prevent data loss and
performance issues, |pmm| does not automatically change |mysql| configuration.
However, there are certain recommended settings that help maximize monitoring
efficiency. These recommendations depend on the variant and version of |mysql|
you are using, and mostly apply to very high loads.

|pmm| can collect query data either from the |slow-query-log| or from
|performance-schema|.  The |slow-query-log| provides maximum details,
but can impact performance on heavily loaded systems. On
|percona-server| the query sampling feature may reduce the performance
impact.

|performance-schema| is generally better for recent versions of other |mysql|
variants. For older |mysql| variants, which have neither sampling, nor
|performance-schema|, configure logging only slow queries.

You can add configuration examples provided in this guide to :file:`my.cnf` and
restart the server or change variables dynamically using the following syntax:

.. code-block:: sql

   SET GLOBAL <var_name>=<var_value>

The following sample configurations can be used depending on the variant and
version of |mysql|:

* If you are running |percona-server| (or |xtradb-cluster|), configure the
  |slow-query-log| to capture all queries and enable sampling. This will provide
  the most amount of information with the lowest overhead.

  ::

   log_output=file
   slow_query_log=ON
   long_query_time=0
   log_slow_rate_limit=100
   log_slow_rate_type=query
   log_slow_verbosity=full
   log_slow_admin_statements=ON
   log_slow_slave_statements=ON
   slow_query_log_always_write_time=1
   slow_query_log_use_global_control=all
   innodb_monitor_enable=all
   userstat=1

* If you are running |mysql| 5.6+ or |mariadb| 10.0+, configure
  :ref:`perf-schema`.

  ::

   innodb_monitor_enable=all
   performance_schema=ON

* If you are running |mysql| 5.5 or |mariadb| 5.5, configure logging only slow
  queries to avoid high performance overhead.

  .. note:: This may affect the quality of monitoring data gathered by
            |abbr.qan|.

  ::

   log_output=file
   slow_query_log=ON
   long_query_time=0.01
   log_slow_admin_statements=ON
   log_slow_slave_statements=ON

.. _pmm/conf-mysql/user-account/pmm/creating:

Creating a |mysql| User Account to Be Used with |pmm|
================================================================================

When adding a |mysql| instance to monitoring, you can specify the |mysql| server
superuser account credentials.  However, monitoring with the superuser account
is not secure. If you also specify the |opt.create-user| option, it will create
a user with only the necessary privileges for collecting data.

.. seealso::

   Using the |pmm-admin.add| command to add a monitoring service
      :ref:`pmm-admin.add-mysql-metrics`

You can also set up the ``pmm`` user manually with necessary privileges and pass
its credentials when adding the instance.

To enable complete |mysql| instance monitoring, a command similar to the
following is recommended:

.. prompt:: bash

   sudo pmm-admin add mysql --user root --password root --create-user

The superuser credentials are required only to set up the ``pmm`` user with
necessary privileges for collecting data.  If you want to create this user
yourself, the following privileges are required:

.. code-block:: sql

   GRANT SELECT, PROCESS, SUPER, REPLICATION CLIENT, RELOAD ON *.* TO 'pmm'@' localhost' IDENTIFIED BY 'pass' WITH MAX_USER_CONNECTIONS 10;
   GRANT SELECT, UPDATE, DELETE, DROP ON performance_schema.* TO 'pmm'@'localhost';

If the ``pmm`` user already exists,
simply pass its credential when you add the instance:

.. prompt:: bash

   sudo pmm-admin add mysql --user pmm --password pass

For more information, run as root
|pmm-admin.add|
|opt.mysql|
|opt.help|.

.. _slow-log-settings:

Configuring the |slow-query-log| in |percona-server|
================================================================================

If you are running |percona-server|, a properly configured slow query log will
provide the most amount of information with the lowest overhead.  In other
cases, use :ref:`Performance Schema <perf-schema>` if it is supported.

By definition, the slow query log is supposed to capture only *slow queries*.
These are the queries the execution time of which is above a certain
threshold. The threshold is defined by the |long_query_time|_ variable.

In heavily loaded applications, frequent fast queries can actually have a much
bigger impact on performance than rare slow queries.  To ensure comprehensive
analysis of your query traffic, set the |long_query_time| to **0** so that all
queries are captured.

However, capturing all queries can consume I/O bandwidth and cause the
|slow-query-log| file to quickly grow very large. To limit the amount of
queries captured by the |slow-query-log|, use the *query sampling* feature
available in |percona-server|.

The |log_slow_rate_limit|_ variable defines the fraction of queries captured by
the |slow-query-log|.  A good rule of thumb is to have approximately 100 queries
logged per second.  For example, if your |percona-server| instance processes
10_000 queries per second, you should set ``log_slow_rate_limit`` to ``100`` and
capture every 100th query for the |slow-query-log|.

.. note:: When using query sampling, set |log_slow_rate_type|_ to ``query``
   so that it applies to queries, rather than sessions.

   It is also a good idea to set |log_slow_verbosity|_ to ``full``
   so that maximum amount of information about each captured query
   is stored in the slow query log.

A possible problem with query sampling is that rare slow queries might not get
captured at all.  To avoid this, use the |slow_query_log_always_write_time|_
variable to specify which queries should ignore sampling.  That is, queries with
longer execution time will always be captured by the slow query log.

By default, slow query log settings apply only to new sessions.  If you want to
configure the slow query log during runtime and apply these settings to existing
connections, set the |slow_query_log_use_global_control|_ variable to ``all``.

.. |long_query_time| replace:: ``long_query_time``
.. _long_query_time: http://dev.mysql.com/doc/refman/5.7/en/server-system-variables.html#sysvar_long_query_time

.. |log_slow_rate_limit| replace:: ``log_slow_rate_limit``
.. _log_slow_rate_limit: https://www.percona.com/doc/percona-server/5.7/diagnostics/slow_extended.html#log_slow_rate_limit

.. |log_slow_rate_type| replace:: ``log_slow_rate_type``
.. _log_slow_rate_type: https://www.percona.com/doc/percona-server/5.7/diagnostics/slow_extended.html#log_slow_rate_type

.. |log_slow_verbosity| replace:: ``log_slow_verbosity``
.. _log_slow_verbosity: https://www.percona.com/doc/percona-server/5.7/diagnostics/slow_extended.html#log_slow_verbosity

.. |slow_query_log_always_write_time| replace:: ``slow_query_log_always_write_time``
.. _slow_query_log_always_write_time: https://www.percona.com/doc/percona-server/5.7/diagnostics/slow_extended.html#slow_query_log_always_write_time

.. |slow_query_log_use_global_control| replace:: ``slow_query_log_use_global_control``
.. _slow_query_log_use_global_control: https://www.percona.com/doc/percona-server/5.7/diagnostics/slow_extended.html#slow_query_log_use_global_control

.. _perf-schema:

Configuring |performance-schema|
================================================================================

The default source of query data for |pmm| is the |slow-query-log|.  It is
available in |mysql| 5.1 and later versions.  Starting from |mysql| 5.6
(including |percona-server| 5.6 and later), you can choose to parse query data
from the |perf-schema| instead of |slow-query-log|.  Starting from |mysql|
5.6.6, |perf-schema| is enabled by default.

|perf-schema| is not as data-rich as the |slow-query-log|, but it has all the
critical data and is generally faster to parse. If you are not running
|percona-server| (which supports :ref:`sampling for the slow query log
<slow-log-settings>`), then |performance-schema| is a better alternative.

To use |perf-schema|, set the ``performance_schema`` variable to ``ON``:

.. include:: .res/code/sql.org
   :start-after: +show-variables.like.performance-schema+
   :end-before: #+end-block

If this variable is not set to **ON**, add the the following lines to the
|mysql| configuration file |my.cnf| and restart |mysql|:

.. include:: .res/code/sql.org
   :start-after: +my-conf.mysql.performance-schema+
   :end-before: #+end-block

If you are running a custom Performance Schema configuration, make sure that the
``statements_digest`` consumer is enabled:

::

 mysql> select * from setup_consumers;
 +----------------------------------+---------+
 | NAME                             | ENABLED |
 +----------------------------------+---------+
 | events_stages_current            | NO      |
 | events_stages_history            | NO      |
 | events_stages_history_long       | NO      |
 | events_statements_current        | YES     |
 | events_statements_history        | YES     |
 | events_statements_history_long   | NO      |
 | events_transactions_current      | NO      |
 | events_transactions_history      | NO      |
 | events_transactions_history_long | NO      |
 | events_waits_current             | NO      |
 | events_waits_history             | NO      |
 | events_waits_history_long        | NO      |
 | global_instrumentation           | YES     |
 | thread_instrumentation           | YES     |
 | statements_digest                | YES     |
 +----------------------------------+---------+
 15 rows in set (0.00 sec)

.. important::

   |perf-schema| instrumentation is enabled by default in |mysql| 5.6.6 and
   later versions. It is not available at all in |mysql| versions prior to 5.6.

   If certain instruments are not enabled, you will not see the corresponding
   graphs in the :ref:`dashboard.mysql-performance-schema` dashboard.  To enable
   full instrumentation, set the option |opt.performance-schema-instrument| to
   ``'%=on'`` when starting the |mysql| server.

   .. code-block:: bash

      $ mysqld --performance-schema-instrument='%=on'

   This option can cause additional overhead and should be used with care.

   .. seealso::

      |mysql| Documentation: |opt.performance-schema-instrument| option
         https://dev.mysql.com/doc/refman/5.7/en/performance-schema-options.html#option_mysqld_performance-schema-instrument

If the instance is already running, configure the |qan| agent to collect data
from |perf-schema|:

1. Open the |qan.name| dashboard.
#. Click the |gui.settings| button.
#. Open the |gui.settings| section.
#. Select |opt.performance-schema| in the |gui.collect-from| drop-down list.
#. Click |gui.apply| to save changes.

If you are adding a new monitoring instance with the |pmm-admin| tool, use the
|opt.query-source| *perfschema* option:

|tip.run-this.root|

.. include:: .res/code/sh.org
   :start-after: +pmm-admin.add.mysql.user.password.create-user.query-source+
   :end-before: #+end-block
		   
For more information, run
|pmm-admin.add|
|opt.mysql|
|opt.help|.

Configuring |mysql| 8.0 for |pmm|
================================================================================

|mysql| 8 (in version 8.0.4) changes the way clients are authenticated by
default. The |opt.default-authentication-plugin| parameter is set to
``caching_sha2_password``. This change of the default value implies that |mysql|
drivers must support the SHA-256 authentication. Also, the communication channel
with |mysql| 8 must be encrypted when using ``caching_sha2_password``.

The |mysql| driver used with |pmm| does not yet support the SHA-256 authentication.

With currently supported versions of |mysql|, |pmm| requires that a dedicated |mysql|
user be set up. This |mysql| user should be authenticated using the
``mysql_native_password`` plugin.  Although |mysql| is configured to support SSL
clients, connections to |mysql| Server are not encrypted.

There are two workarounds to be able to add |mysql| Server version 8.0.4
or higher as a monitoring service to |pmm|:

1. Alter the |mysql| user that you plan to use with |pmm|
2. Change the global |mysql| configuration

.. rubric:: Altering the |mysql| User

Provided you have already created the |mysql| user that you plan to use
with |pmm|, alter this user as follows:

.. include:: .res/code/sql.org
   :start-after: +alter.user.identified.with.by+
   :end-before: #+end-block

Then, pass this user to ``pmm-admin add`` as the value of the ``--user``
parameter.

This is a preferred approach as it only weakens the security of one user.

.. rubric:: Changing the global |mysql| Configuration

A less secure approach is to set |opt.default-authentication-plugin|
to the value **mysql_native_password** before adding it as a
monitoring service. Then, restart your |mysql| Server to apply this
change.

.. include:: .res/code/sql.org
   :start-after: +my-conf.mysqld.default-authentication-plugin+
   :end-before: #+end-block
   
.. seealso::

   Creating a |mysql| User for |pmm|
      :ref:`privileges`

   More information about adding the |mysql| query analytics monitoring service
      :ref:`pmm-admin.add-mysql-queries`

   |mysql| Server Blog: |mysql| 8.0.4 : New Default Authentication Plugin : caching_sha2_password
      https://mysqlserverteam.com/mysql-8-0-4-new-default-authentication-plugin-caching_sha2_password/

   |mysql| Documentation: Authentication Plugins
      https://dev.mysql.com/doc/refman/8.0/en/authentication-plugins.html

   |mysql| Documentation: Native Pluggable Authentication
      https://dev.mysql.com/doc/refman/8.0/en/native-pluggable-authentication.html

.. _pmm/mysql/conf/dashboard:

Settings for Dashboards
================================================================================

Not all dashboards in |metrics-monitor| are available by default for all |mysql|
variants and configurations: |oracle|'s |mysql|, |percona-server|. or |mariadb|.
Some graphs require |percona-server|, specialized plugins, or additional
configuration.

Collecting metrics and statistics for graphs increases overhead.  You can keep
collecting and graphing low-overhead metrics all the time, and enable
high-overhead metrics only when troubleshooting problems.

.. seealso::

   More information about |pmm| dashboards
      :ref:`pmm.metrics-monitor`

.. _pmm/mysql/conf/dashboard/mysql-innodb-metrics:

|mysql| |innodb| Metrics
--------------------------------------------------------------------------------

InnoDB metrics provide detailed insight about |innodb| operation.  Although you
can select to capture only specific counters, their overhead is low even when
they all are enabled all the time.  To enable all |innodb| metrics, set the
global variable |opt.innodb-monitor-enable| to ``all``:

.. code-block:: sql

   mysql> SET GLOBAL innodb_monitor_enable=all

.. seealso::

   |mysql| Documentation: |opt.innodb-monitor-enable| variable
      https://dev.mysql.com/doc/refman/5.7/en/innodb-parameters.html#sysvar_innodb_monitor_enable

.. _pmm/mysql/conf/dashboard/mysql-user-statistics:

|mysql| User Statistics
--------------------------------------------------------------------------------

User statistics is a feature of |percona-server| and |mariadb|.  It provides
information about user activity, individual table and index access.  In some
cases, collecting user statistics can lead to high overhead, so use this feature
sparingly.

To enable user statistics, set the |opt.userstat| variable to ``1``.

.. seealso::

   |percona-server| Documentation: |opt.userstat|
      https://www.percona.com/doc/percona-server/5.7/diagnostics/user_stats.html#userstat

   |mysql| Documentation
      `Setting variables <https://dev.mysql.com/doc/refman/5.7/en/set-variable.html>`_

.. _pmm/mysql/conf/dashboard/mysql-query-response-time:

|percona-server| Query Response Time Distribution
--------------------------------------------------------------------------------

Query response time distribution is a feature available in |percona-server|.  It
provides information about changes in query response time for different groups
of queries, often allowing to spot performance problems before they lead to
serious issues.

.. warning::

   This feature causes very high overhead, especially on systems processing more
   than 10000 queries per second.  Use it only temporarily when troubleshooting
   problems.

To enable collection of query response time:

1. Install the |query-response-time| plugins:

   .. code-block:: sql

      mysql> INSTALL PLUGIN QUERY_RESPONSE_TIME_AUDIT SONAME 'query_response_time.so';
      mysql> INSTALL PLUGIN QUERY_RESPONSE_TIME SONAME 'query_response_time.so';
      mysql> INSTALL PLUGIN QUERY_RESPONSE_TIME_READ SONAME 'query_response_time.so';
      mysql> INSTALL PLUGIN QUERY_RESPONSE_TIME_WRITE SONAME 'query_response_time.so';

#. Set the global varible |opt.query-response-time-stats| to ``ON``:

   .. code-block:: sql
		   
      mysql> SET GLOBAL query_response_time_stats=ON;


.. seealso::

   |percona-server| Documentation:

      - |opt.query-response-time-stats|
	(https://www.percona.com/doc/percona-server/5.7/diagnostics/response_time_distribution.html#query_response_time_stats)
      - `Response time distribution <https://www.percona.com/doc/percona-server/5.7/diagnostics/response_time_distribution.html#installing-the-plugins>`_

.. include:: .res/replace.txt
