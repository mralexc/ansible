ssl_check_renewal
=========

Role that will allow for checking and renewal of SSL certificates. Required to
be run from a WSL instance to use powershell and "certreq" to issue the certs. 

Requirements
------------

Requires use of WSL system to access powershell.exe and the CA. The
community.crypto.openssl_privatekey, openssl_csr, and x509_certificate_info
modules require python3-cryptography to be installed. The role has this built in.

Role Variables
--------------

Update the following variables in vars/main.yml with Windows CA information and
CSR info. The Windows CA vars are all required to prevent any prompting at
issueing time.

```yaml
# UPDATE THESE
ssl_renewal_check_domain: YOURDOMAIN.COM
ssl_renewal_check_ca_config: WINDOWS_CA_SERVER_FQDN\\ISSUING_CA_NAME
ssl_renewal_check_cert_template: ISSUED_CERT_TEMPLATE_NAME

# UPDATE THESE
# CSR Vars
ssl_renewal_check_csr_country_name: COUNTRY
ssl_renewal_check_csr_state_or_province_name: STATE
ssl_renewal_check_csr_locality_name: CITY
ssl_renewal_check_csr_organization_name: ORGANIZATION
ssl_renewal_check_csr_organizational_unit_name: ORGANIZATIONAL UNIT
ssl_renewal_check_csr_email_address: email@org.com
```

Dependencies
------------

Depends on community.crypto modules.

Example Playbook
----------------

check_and_renew_ssl.yml:
```yaml
---
- name: Check SSL expiration and renew if necessary
  hosts: all
  roles:
    - ssl_renewal_check
```

How to use:

```bash
ansible-playbook -v -K check_and_renew_ssl.yml -i inventory/ -l HOST
```

This can be used with multiple hosts, but the limit is used in the example.
Verbosity is also nice to see.

Notes
----------------

Handlers is intentionally left in here to eventually extend functionality.
