# Atlassian Confluence Ansible Playbook

This Ansible playbook is designed to install and configure Atlassian Confluence on a Linux server. It includes the following roles:
confluenceinstall - installes confluence
linux_certreq - requests certificate on behalf of linux server for SSL config
confluence_certreq -  installs certificates into confluence server and configures SSL files as needed

## Table of Contents
- [Atlassian Confluence Ansible Playbook](#Atlassian-Confluence-Ansible-Playbook)
- [Table of Contents](#table-of-contents)
- [Prerequisites](#prerequisites)
- [ConfluenceInstall Configuration](#ConfluenceInstall-Configuration)
- [Linux_Certconfig Configuration](#Linux_Certconfig-Configuration)
- [Notes](#notes)

## Prerequisites

* A domain name configured for Confluence
* Ansible 2.7
* Ansible-Galaxy community.general and community.crypto installed.
* A functional PKI infrastructure with a certificate authority (CA) host available at `pki_ca_host`.
* A service account (`svc_account`) with the necessary permissions to request and download certificates.

## ConfluenceInstall Configuration

The following variables are used in this Ansible playbook.


| Variable        | Description                                                                                  |
|----------------|----------------------------------------------------------------------------------------------|
| `installation` | Installation directory for Atlassian Confluence.                                             |
| `link`         | Symbolic link to the current Confluence version.                                              |
| `filename`     | File name of the Confluence archive.                                                         |
| `foldername`   | Folder name of the Confluence version after extraction.                                       |
| `version`      | Version number of the installed Confluence.                                                   |
| `javahome`     | Installation directory for the JDK.                                                           |
| `jdkname`      | File name of the JDK archive.                                                                |
| `domain`       | Domain name for the Confluence instance.                                                     |
| `baseurl`      | Base URL for the Confluence instance.                                                        |
| `confluencehome` | Directory for Confluence's home directory, which stores data like attachments, settings, etc.|
| `appprops`     | Application properties file for Confluence.                                                   |
| `serverxml`    | Server configuration file for Confluence.                                                    |
| `setenv`       | Script to set environment variables for Confluence.                                           |
| `setjre`       | Script to set the JRE for Confluence.  

## Linux_Certconfig Configuration

| Variable                   | Description                                                                                   |
| --------------------------- | --------------------------------------------------------------------------------------------- |
| `target_application`       | The target application (Confluence).                                                           |
| `custom_certpath`          | The directory where the generated certificates will be copied (`/opt/certs/` by default).     |
| `ad_username`              | The username of the service account (`svc_account` by default).                                |
| `ad_password`              | The password of the service account (`secretpass` by default).                                |
| `pki_ca_host`              | The hostname of the certificate authority (`ca_hostname` by default).                           |
| `pki_template_type`        | The certificate template type (`example.com Webserver v1` by default).                         |
| `domain`                   | The target domain (`example.com` by default).                                                 |
| `csr_common_name`          | The common name for the certificate (based on `baseurl`).                                       |
| `csr_email_address`        | The email address associated with the certificate request (using `ad_username` and `domain`). |
| `csr_country`              | The country associated with the certificate (`US` by default).                                 |
| `csr_state`                | The state associated with the certificate (`GA` by default).                                   |
| `csr_location`             | The location associated with the certificate (`Marietta` by default).                          |
| `csr_organization`         | The organization associated with the certificate (`LM` by default).                           |
| `csr_ou`                   | The organizational unit associated with the certificate (`RHEL8` by default).                   |
| `san_names`                | The Subject Alternative Name (SAN) entries for the certificate (automatically generated).      |

## Notes

The Confluence memory settings are automatically calculated based on the total system memory. Half of the system memory is allocated to Confluence by default. You can modify this value by changing the `half_mem` variable in the `fancy memory stuff` block.

This Ansible playbook is designed for Red Hat Enterprise Linux (RHEL) 8 and may require modifications for other Linux distributions.
