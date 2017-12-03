cdist-type__zfs_dataset(7)
==========================

NAME
----
cdist-type__zfs - Manage ZFS datasets and zvols


DESCRIPTION
-----------
This type allows you to create, destroy, and manipulate properties of ZFS datasets and zvols.

.. note:: Since ZFS is a complex filesystem, this type provides only very basic facilities for
          manipulating ZFS datasets and zvols.

.. note:: The values of numeric properties can be specified using human-readable suffixes (for
          example, k, KB, M, Gb, and so forth, up to Z for zettabyte).  The following are all valid
          (and equal) specifications: 1536M, 1.5g, 1.50GB. (-- taken from ZFS manpage)

Dataset name is the object-ID. If the dataset name contains character '@' it is assumed that
it is a clone of an existing dataset.


REQUIRED PARAMETERS
-------------------
None.


OPTIONAL PARAMETERS
-------------------
state
    'present' or 'absent', defaults to 'present'. State 'absent' will destroy the dataset only if
    it has no children. Additionally, if the dataset contains a snapshot which is clones somewhere
    the destruction of such dataset will not be possible until that clone is promoted.

clone-from
    If the dataset does not exist, clone it from this snapshot.

zvol-blocksize
    When creating a zvol, make it this blocksize. If the zvol already exists, assert it has this
    blocksize or abort execution with an error. If this parameter is absent during zvol creation
    the default block size is used. If this parameter is used in conjunction with
    ``--parameter vololblocksize=...`` the behavior is undefined

zvol-size
    When creating a zvol, make it this size. If the zvol already exists, assert it has this
    size or abort execution with an error.


BOOLEAN PARAMETERS
------------------
allow-recursive-destruction
    If state is 'absent' recursively destroy all children.

zvol
    This is a zvol. If this parameter is specified on already created dataset which is not a zvol
    it will abort execution with an error.


MULTIPLE PARAMETERS
-------------------
property
    Format: ``property=value``. Assign this value to a property.


EXAMPLES
--------

.. code-block:: sh

    export CDIST_ORDER_DEPENDENCY=on

    # Create dataset /zroot/data/home
    __zfs /zroot/data/home --state present --property mountpoint=/home

    # Snapshot it
    __zfs /zroot/data/home@snap --state present

    # Destroy something else
    __zfs /zroot/data/something-else --state absent --allow-recursive-destruction

    # Create a zvol with 64K blocks
    __zfs /zroot/data/random-zvol --zvol --zvol-blocksize 64K --zvol-size 128G

    # Create dataset for InnoDB data files with recommended defaults
    __zfs /zroot/data/innodb-data --property recordsize=16K --property primarycache=metadata \
                                  --property lobgias=throughput


AUTHORS
-------
Trojsten Tech Team


COPYING
-------
Copyright \(C) 2017 Trojsten Tech Team. You can redistribute it
and/or modify it under the terms of the GNU General Public License as
published by the Free Software Foundation, either version 3 of the
License, or (at your option) any later version.
