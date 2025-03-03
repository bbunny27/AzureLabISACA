# Azure CLI
### Creating a Resource Group
```bash
az group create --name rg-lab --location westus2
```

### Creating a Virtual Network 

1. Create a resource group called `rg-lab` and then proceed:

```bash
az network vnet create -g rg-lab -n vnet-hub --address-prefix 10.0.0.0/16 --subnet-name vnet-hub-subnet1 --subnet-prefix 10.0.1.0/24 -l westus2
```

### Network Security Group & Security Rule

1. Network Security Group:
```bash
az network nsg create --name nsg-hub --resource-group rg-lab --location westus2
```

2. Security Rule:
```bash
az network nsg rule create -g rg-lab --nsg-name nsg-hub --name vnet-hub-allow-ssh --direction inbound --destination-address-prefix 10.0.1.0/24 --destination-port-range 22 --access allow --priority 100 
```

3. Attach to Subnet:
```bash
az network vnet subnet update -g rg-lab -n vnet-hub-subnet1 --vnet-name vnet-hub --network-security-group nsg-hub
```

### Create a Virtual Machine

```bash
az vm create --resource-group rg-lab --name vnet-hub-vm1 --image UbuntuLTS --vnet-name vnet-hub --subnet vnet-hub-subnet1 --admin-username azureuser --admin-password Azure123456!
```

```bash
az network vnet subnet list -g rg-lab --vnet-name vnet-hub -o table
```

### Tearing Down the Resources.
1. Deleting VM's
```bash
az vm delete --resource-group rg-lab --name vnet-hub-vm1 --yes
```
2. Deleting NSG's.
```bash
az network nsg delete --resource-group rg-lab --name nsg-hub
```
3. Deleting Virtual Networks.
```bash
az network vnet delete --resource-group rg-lab --name vnet-hub
```
4. Delete the Resource Group. This removes all resource inside of it.
```bash
az group delete --name rg-lab --yes --no-wait
```
These command will ensure a clean removal of all resources. Please double check on your portal to ensure that everything is deleted. ISACA is not resposible for any charges you may incur by leaving services enabled.
