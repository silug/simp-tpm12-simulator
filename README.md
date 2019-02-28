## simp-tpm12-simulator

<!-- vim-markdown-toc GFM -->

* [Overview](#overview)
  * [This is a SIMP project](#this-is-a-simp-project)
* [Setup](#setup)
  * [Requirements](#requirements)
  * [Building the `simp-tpm12-simulator` RPMs](#building-the-simp-tpm12-simulator-rpms)
  * [Beginning with simp-tpm12-simulator](#beginning-with-simp-tpm12-simulator)
* [Usage](#usage)
* [Development](#development)

<!-- vim-markdown-toc -->

## Overview

Rake and config files to build/package newer versions of the [IBM TPM 1.2
simulator][ibmswtpm12] as EL6 and EL7 RPMs.

### This is a SIMP project

This module is a component of the [System Integrity Management
Platform][simp], a compliance-management framework built on Puppet.

If you find any issues, please submit them to our [bug tracker][simp-jira].

## Setup

### Requirements

The TPM 1.2 simulator build process requires:

* An EL6 or EL7 host with:
  - `rpm`
  - `tar`
  - `curl` (for direct downloads)
  - `rpm-build`
* Ruby 2.1+ with RubyGems.
* [bundler][bundler] 1.14+


### Building the `simp-tpm12-simulator` RPMs
To build both the `simp-tpm12-simulator` RPMs:

```sh
# Use bundler to install all necessary gems (https://bundler.io)
bundle install

# Download source + build all rpms for your hosts' build dist (e.g. el7, el6)
bundle exec rake pkg:rpm

# The RPM will be in the dist/ directory of each repo
ls -l simp-tpm12-simulator.el?/dist/*.rpm
```

### Beginning with simp-tpm12-simulator

The TPM 1.2 simulator relies upon a couple of rpm packages which should be
installed on any target system intended to use the module. The packages are
`gcc`, the GNU Compiler Collection, and [`trousers`][trousers], an implementation
of the Trusted Computing Group's Software Stack(TSS) specification.  Additionally
tpm-tools, a group of tools to manage and utilize the Trusted Computing
Group's TPM hardware, is recommended.

## Usage

To install the rpm, copy the rpm file to the target system and install it
with the command:

```yaml
yum localinstall simp-tpm12-simulator-*.rpm
```

This will install the simulator programs, utilities, and servics necessary
to start and utilize the TPM 1.2 simulator.

To initialize and use the TPM simulator on EL7, issue the following commands:

```yaml
# systemctl start tpm12-simulator
# systemctl start tpm12-tpmbios
# systemctl restart tpm12-simulator
# systemctl start tpm12-tpmbios
# systemctl start tpm12-tpminit
# systemctl start tpm12-tcsd
```

To initialize and use the TPM simulator on EL6, issue the following commands:

```yaml
# service tpm12-simulator start
# service tpm12-tpmbios start
# service tpm12-simulator restart
# service tpm12-tpmbios start
# service tpm12-tpminit start
# service tpm12-tcsd start
```

> Note: Procedures for initializing the TPM simulator are detailed in the source code at libtpm/README.

## Development

Please read our [Contribution Guide](http://simp-doc.readthedocs.io/en/stable/contributors_guide/index.html).

[bundler]:    https://bundler.io
[simp]:       https://github.com/NationalSecurityAgency/SIMP/
[simp-jira]:  https://simp-project.atlassian.net/
[ibmswtpm12]: https://sourceforge.net/projects/ibmswtpm/
[trousers]:   https://sourceforge.net/projects/trousers/

