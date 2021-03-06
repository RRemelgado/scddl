Scientific Computing Data Set Download
======================================

**scddl** (pronounced **scuttle**) downloads data sets for scientific
computing.

[![Build Status](https://travis-ci.com/idiv-biodiversity/scddl.svg?branch=master)](https://travis-ci.com/idiv-biodiversity/scddl)
[![Codacy Badge](https://api.codacy.com/project/badge/Grade/8f8c1bd0b2b84e57be194b3c55cd3e89)](https://www.codacy.com/app/idiv-biodiversity/scddl?utm_source=github.com&amp;utm_medium=referral&amp;utm_content=idiv-biodiversity/scddl&amp;utm_campaign=Badge_Grade)


Table of Contents
-----------------

<!-- toc -->

- [Goals and Features](#goals-and-features)
  * [Consistency](#consistency)
  * [Usability](#usability)
- [Supported Data Sets](#supported-data-sets)
  * [Source Data Sets](#source-data-sets)
  * [Derived Data Sets](#derived-data-sets)
- [Usage](#usage)

<!-- tocstop -->


Goals and Features
------------------

### Consistency

-   **integrity checks**

    Data sets that provide file integrity information, e.g. MD5 checksums, are
    rigorously checked.

-   **strict versioning**

    Data sets that are not inherently versioned will be tagged with the
    download date. This makes **reproducible research** possible. There **will
    be no link to the latest** version to enforce this strict versioning.

    A result of this is that **existing files are never overwritten**. All
    running jobs would have inconsistent results if files would be updated in
    place.


### Usability

-   **centralized storage location**

    Especially on scientific computing platforms, the data sets are intended to
    be downloaded to globally accessible storage locations. This avoids that
    users or groups have to maintain their own copies and that their file
    system quotas are stressed. Also, new users can immediately start working
    instead of having to download their data sets first.

-   **improved file system performance**

    Another advantage of centralized storage is that the file system can better
    cache the data sets. This can result in improved I/O performance,
    especially when a single data set is used concurrently by many users. Your
    mileage may vary, based on caching capability of the used file system and
    on the data set usage patterns.

-   **periodic, automatic updates**

    The download tools can be run as **cron jobs** or **systemd timers**. This
    way, you can easily create periodic, automated updates of data sets.

-   **logging to syslog**

    When run non-interactively, i.e. as **cron jobs** or **systemd timers**,
    the download tools send their output to **syslog** with their script name
    as the tag, e.g. the tool **ncbidl.sh** would use **ncbidl** as tag. You
    can then search for these tags, e.g.:

    ```bash
    journalctl -t ncbidl
    ```


Supported Data Sets
-------------------

### Source Data Sets

Source data sets are downloaded directly off the internet.

- [EBI](ftp://ftp.ebi.ac.uk): `ebidl.sh`
- [Ensembl](https://ftp.ensembl.org): `ensembldl.sh`
- [NCBI](https://ftp.ncbi.nlm.nih.gov): `ncbidl.sh`
- [UCSC](ftp://hgdownload.cse.ucsc.edu): `ucscdl.sh`

### Derived Data Sets

Derived data sets are built from source data sets. They automatically download
their sources, if these are not available yet.

- [diamond](https://github.com/bbuchfink/diamond): `diamonddb.sh`
  - builds diamond database from NCBI sources using the `makedb` sub-command


Usage
-----

Each tool provides online help via the `--help` command line argument, e.g.:

```bash
bash ncbidl.sh --help
```

The download tools can also be used as **cron jobs**, e.g.:

```
@monthly time bash /path/to/ncbidl.sh /data/db blast/db/nr
@monthly time bash /path/to/ncbidl.sh /data/db blast/db/nt
```
