.. _audit:

Audit
=====

.. index:: Audit

The systems provides a sophisticated audit log, that can be viewed in the 
WebUI.

.. figure:: auditlog.png
   :width: 500

   *Audit Log*

privacyIDEA comes with an SQL audit module. (see :ref:`code_audit`)


Cleaning up entries
-------------------

.. index:: Audit Log Rotate

The ``sqlaudit`` module writes audit entries to an SQL database.
For performance reasons the audit module does no log rotation during
the logging process.

But you can set up a cron job to clean up old audit entries.

You can specify a *highwatermark* and a *lowwatermark*. To clean
up the audit log table, you can call ``pi-manage`` at command line::
   
   pi-manage rotate_audit --highwatermark 20000 --lowwatermark 18000

This will, if there are more than 20.000 log entries, clean all old
log entries, so that only 18000 log entries remain.

Access rights
~~~~~~~~~~~~~

You may also want to run the cron job with reduced rights. I.e. a user who
has no read access to the original pi.cfg file, since this job does not need
read access to the SECRET or PEPPER in the pi.cfg file.

So you can simply specify a confgi file with only the content::

   PI_AUDIT_SQL_URI = <your database uri>

Then you can call ``pi-manage`` like this::

   PRIVACYIDEA_CONFIGFILE=/home/cornelius/src/privacyidea/audit.cfg \
   pi-manage rotate_audit

This will read the configuration (only the database uri) from the config file
``audit.cfg``.
