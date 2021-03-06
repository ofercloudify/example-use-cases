# F5 Big-IP on Azure

## Prerequesites:

Prior to any deployment installation You have to upload plugins and create secrets.

### Plugins 

Upload:
* **cloudify-azure-plugin** - Tested for 2.1.0 version,
* **cloudify-utilities-plugin** - Tested for version 1.12.0

### Secrets

Create below secrets on secrets store management:
* **BIG-IP secrets:**
    * *bigip_username* - Username for BIG IP VE, it is set during provisioning and used during configuration, "admin" is not allowed
    * *bigip_password* - Password for BIG IP VE, it is set during provisioning and used during configuration. The supplied password must be between 6-72 characters long and must satisfy at least 3 of password complexity requirements from the following: Contains an uppercase character, Contains a lowercase character, Contains a numeric digit, Contains a special characterr. Control characters are not allowed
* **Azure secrets:**
    * *azure_client_id*
    * *azure_client_secret*
    * *azure_subscription_id*
    * *azure_tenant_id*

## Provisioning 

VNFM-F5-Prov-Azure-vm.yaml is responsible for creation BIG-IP Virtual Machine connected to 3 networks:
* Management,
* WAN,
* Public.

### Prerequesites

Before installation of VNFM-F5-Prov-Azure-vm.yaml, suitable resource group, networks and security group have to be created.
Go to *common* directory and install blueprint as described in readme file.
It should be installed only one time before start of provisioning services.

### BIG-IP provisioning

Resources created in Prerequesites subsection are fetched using existing_networks.yaml blueprint and VNFM-F5-Prov-Azure-vm.yaml is using it.
To provision BIG-IP:

``cfy install VNFM-F5-Prov-Azure-vm.yaml -b VNFM-F5-Prov-Azure-vm``


## Configuration

### Install
VNFM-F5-Conf.yaml is responsible for licensing BIG IP with provided registration key (bigip_license_key input).
Additionally VLAN configuration necessary for further LTM configuration is applied.

``cfy install VNFM-F5-Conf.yaml -b VNFM-F5-Conf``

### Uninstall
During uninstall the license is revoked so it can be used on different BIG IP VE or on the same one again.
Also VLAN configuration is deleted.
