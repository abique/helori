======
Helori
======

---------------------------------------------
Archlinux repositories hosting and management
---------------------------------------------

.. contents:: Table of contents
   :depth: 2

Goal
====

Helori's goal is to simplify custom Archlinux repository hosting and
management. It targets small business usage.

Let's assume that in my company we have 4 repositories:
 - third-parties, which contains custom made packages for third-parties or
   built packages from AUR
 - dev, developpment environment
 - test, test environment
 - prod, prod environmnet

We want to easily:
 - upload packages to a pool of packages
 - select which packages goes from the pool to a repository
 - keep every version of every packages
 - easy to rollback
 - compare packages and versions in dev, test and prod

Non-Goals
=========

 - high scalability: this service is not mean to be used by a lot of people,
   or even host a LOT of packages
 - efficiency, there is no need to trade simplicity over efficiency

Features
========

 - user management

   * external authentification

 - package pool

   * client upload tool
   * HTTP access to the pool (HEAD, GET)
   * upload ACL, note that sensible file like certificate or password
     configuration files should not be in the packages
   * no overwrite
   * no remove

 - repositories

   * http exposure of the repositories
   * add package
   * remove package

 - HTTP API

   * list package pool content
   * list repository content
   * compare repository content
   * basic operations on repo (add, remove package)

 - notifications hooks

   * execute shell scripts hooks on events

Overview
========

.. image:: overview.svg

HTTP Routing
============

 - /pool/ access to the archives pool
 - /repos/$repo_name/ access to the repository
 - /api/ API mapping

HTTP API
========

Authentication
~~~~~~~~~~~~~~

The authentication is done through HTTP headers:
::

  X-Helori-User: abique
  X-Helori-Password: WfAEaXmcrF4Hck3TpntJnX4

Package upload
~~~~~~~~~~~~~~

Request::

  PUT /pool/wine-stable-1.6.3-1.tar.pkg.x86_64.pkg.tar.xz HTTP/1.0
  X-Helori-User: abique
  X-Helori-Password: WfAEaXmcrF4Hck3TpntJnX4
  ...

List pool packages
~~~~~~~~~~~~~~~~~~

Request::

   GET /api/pool

Response::

   [...,"wine-stable-1.6.2-1","wine-stable-1.6.3-1","wine-stable-1.6.3-2",...]

Search pool by prefix
~~~~~~~~~~~~~~~~~~~~~

Request::

   GET /api/pool/prefix/wine-stable

Response::

   ["wine-stable-1.6.2-1","wine-stable-1.6.3-1","wine-stable-1.6.3-2"]

List repository packages
~~~~~~~~~~~~~~~~~~~~~~~~

Request::

   GET /api/repos/:repo_name

Response::

   [...,"wine-stable-1.6.2-1","wine-stable-1.6.3-1","wine-stable-1.6.3-2",...]

Search repository
~~~~~~~~~~~~~~~~~

Request::

   GET /api/pool/prefix/wine-stable

Response::

   ["wine-stable-1.6.2-1","wine-stable-1.6.3-1","wine-stable-1.6.3-2"]

Add package to a repository
~~~~~~~~~~~~~~~~~~~~~~~~~~~

Request::

  PUT /api/repo/:repo_name/pkgs/:package
  Content-Lenght: 0

Remove package from a repository
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Request::

  DELETE /api/repo/:repo_name/pkgs/:package

Configuration
=============

Indexing
========

Pool
~~~~

Packages are going to be indexed in memory for the pool. This avoid maintaining database on the disk, and at start a directory scan should be fast enough.

We need:
  * direct access to a package
  * list every versions available of a package
  * list packages by prefix

Repositories
~~~~~~~~~~~~

We rely on libalpm for the various queries.
