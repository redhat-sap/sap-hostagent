# sap-hostagent [![Build Status](https://travis-ci.com/redhat-sap/sap-hostagent.svg?branch=master)](https://travis-ci.com/redhat-sap/sap-hostagent)

SAP Host Agent is an agent that can accomplish several life-cycle management tasks, such as operating system monitoring, database monitoring, system instance control and provisioning.

It is recommended to install SAP Host Agent upfront in any HA environment.

You can find the latest Documentation in [SAP NOTE 1907566](https://launchpad.support.sap.com/#/notes/1907566)

This role installs or updates the SAP Host Agent on a RHEL 8.x system. It is provided as RPM package, tarball or as part of an SAP softwarebundle.
While Red Hat recommends RPM for easier upgrade, this role take care of all formats.

## Requirements

This role is intended to use on a RHEL system that gets SAP software.
So your system needs to be installed with at least the RHEL core packages, properly registered and prepared for HANA or Netweaver installation.

It needs access to the software repositories required to install SAP HANA (see also: [How to subscribe SAP HANA systems to the Update Services for SAP Solutions](https://access.redhat.com/solutions/3075991))

You can use the [redhat_sap.sap_rhsm](https://galaxy.ansible.com/redhat_sap/sap_rhsm) Galaxy Role to automate this process

To install SAP software on Red Hat Enterprise Linux 8 you need some additional packages which come in a special repository. To get this repository you need to have one
of the following products:

- [RHEL for SAP Solutions](https://access.redhat.com/solutions/3082481) (premium, standard, developer Edition)
- [RHEL for Business Partner NFRs](https://partnercenter.redhat.com/NFRPageLayout)

[Click here](https://developers.redhat.com/products/sap/download/) to achieve a personal developer edition of RHEL for SAP Solutions. Please register as a developer and download the developer edition.

- [Registration Link](http://developers.redhat.com/register) :
  Here you can either register a new personal account or link it to an already existing **personal** Red Hat Network account.
- [Download Link](https://access.redhat.com/downloads/):
  Here you can download the Installation DVD for RHEL with your previously registered account

*NOTE:* This is a regular RHEL installation DVD as RHEL for SAP Solutions is no additional
 product but only a special bundling. The subscription grants you access to the additional
 packages through our content delivery network(CDN) after installation.

It is also important that your disks are setup according to the [SAP storage requirements for SAP HANA](https://www.sap.com/documents/2015/03/74cdb554-5a7c-0010-82c7-eda71af511fa.html). This [BLOG](https://blogs.sap.com/2017/03/07/the-ultimate-guide-to-effective-sizing-of-sap-hana/) is also quite helpful when sizing HANA systems.


## Role Variables

```yaml

# SAP ProfilePath (You should not need to change the default)
sap_usr_sap: "/usr/sap"

# HANA Shared directory
sap_hana_shared: "/hana/shared"
# HANA log Directory
sap_hana_log: "/hana/log"
# HANA data Directory
sap_hana_data: "/hana/log"

```

The following variable define the UID and GID for the required user to run hostagent. As this might be used in other SAP software as well, the are prefixed as global variable:

```yaml
sap_sapadm_uid: 20202
sap_sapadm_pw_clear: "MyS3cret!"
sap_sapsys_gid: 20202
```

For the above user, you need to define a password for authentication and SSL encryption:
```yaml
sap_hana_hostagent_ssl_pw: "MyS3cret!"
```

It is recommended to use `ansible-vault` to encrypt these variables.

```yaml
# Define either one of the following variables depending on installation method
#sap_hana_hostagent_installdir: /path/to/unpacked/hostagent packages
#sap_hana_hostagent_rpm: /path/to/hostagentrpm
#sap_hana_hostagent_archive: /path/to/sap_hana_hostagent_archive
```
Please note: If you use RPM please honor the following:
- the default profile path `/usr/sap` must not be changed
- it is recommended to add the RPM package to a custom repository or satellite. In that case just add the name in `sap_hana_hostagent_rpm`


if you do an install or upgrade from archive you need to define your unarchive command. If you already have a previous version  on your system it is likely to be `/usr/sap/hostctrl/exe/SAPCAR`
So this is the default then

```yaml
sap_hana_hostagent_unarchive_cmd: "/usr/sap/hostctrl/exe/SAPCAR xvf"
```

## Dependencies

None

## Example Playbook

```yaml
    - hosts: servers
      roles:
      - role: sap-hostagent
```

## Example Inventory

When using RPM:

```yaml

```

When using SAR:

```yaml

```

## License

GPLv3

## Author Information

Red Hat SAP Community of Practice
