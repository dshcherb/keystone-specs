..
 This work is licensed under a Creative Commons Attribution 3.0 Unported
 License.

 http://creativecommons.org/licenses/by/3.0/legalcode

==================
Alembic Migrations
==================

`bp alembic <https://blueprints.launchpad.net/keystone/+spec/alembic>`_


Move from SQLALChemy-migrate to Alembic as the tool for maintaining SQL Repos.


Problem Description
===================

OpenStack is moving from sqlalchemy-migrate to Alembic as the tool for
maintaining SQL migrations. Keystone needs to follow suit, or we will be using
an unsupported tool.

Proposed Change
===============

Continue using sqlalchemy-migrate for the current set of migrations.
All new migrations will use Alembic.

Alembic migrations will run after sqlachemy-migrate migrations on upgrade
and vice versa on downgrade. oslo.db's migration_cli utilities will be used.

In two releases, we should be able to collapse the last of the
sqlalchemy-migrate based migrations into a base Alembic based migration.

Security Impact
---------------

None

Notifications Impact
--------------------

None

Other End User Impact
---------------------

None

Performance Impact
------------------

None

Other Deployer Impact
---------------------

None

Developer Impact
----------------

Existing modules that need migrations will need to create an "alembic"
directory along with "migrate_repo". It can be done using
``alembic init alembic`` command in the directory with the module.

Implementation
==============

Assignee(s)
-----------


Primary assignee:

  Boris Bobrov (breton)

Other contributors:
  ayoung

Work Items
----------

* Start using migration_cli from oslo.db with disabled Alembic

* Create Alembic repos where sqlalchemy-migrate is now used, add a requirement
  to have Alembic repos if migrations are required and remove a requirement to
  have a "repo_migrate".

Dependencies
============

The feature requires to have a new dependency for Keystone -- "Alembic". Now
Alembic is required anyway by oslo.db, which is a requirement for Keystone.

Documentation Impact
====================

* Describe how to create migrations with Alembic

References
==========

* https://alembic.readthedocs.org/en/latest/tutorial.html#creating-an-environment
  for extension authors

* http://lists.openstack.org/pipermail/openstack-dev/2013-July/011767.html

* https://review.openstack.org/gitweb?p=openstack/oslo.db.git;a=blob;f=oslo/db/sqlalchemy/migration_cli/README.rst;h=ebbbdcbe3ab65da35a3322fec30767e60a840060;hb=HEAD
