---
title: Create a VM with a static private IP address - Azure CLI
description: Learn how to create a virtual machine with a static private IP address using the Azure CLI.
author: asudbring
ms.author: allensu
ms.service: virtual-network
ms.subservice: ip-services
ms.topic: how-to
ms.date: 10/01/2021
ms.custom: template-how-to 
---

# Create a virtual machine with a static private IP address using the Azure CLI

A virtual machine (VM) is automatically assigned a private IP address from a range that you specify. This range is based on the subnet in which the VM is deployed. The VM keeps the address until the VM is deleted. Azure dynamically assigns the next available private IP address from the subnet you create a VM in. Assign a static IP address to the VM if you want a specific IP address in the subnet.

[!INCLUDE [azure-cli-prepare-your-environment.md](../../../includes/azure-cli-prepare-your-environment.md)]

- An Azure account with an active subscription. [Create an account for free](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
- This tutorial requires version 2.0.28 or later of the Azure CLI. If using Azure Cloud Shell, the latest version is already installed.

## Create a resource group

An Azure resource group is a logical container into which Azure resources are deployed and managed.

Create a resource group with [az group create](/cli/azure/group#az_group_create) named **myResourceGroup** in the **eastus2** location.

```azurecli-interactive
  az group create \
    --name myResourceGroup \
    --location eastus2
```

## Create a virtual machine

Create a virtual machine with [az vm create](/cli/azure/vm#az_vm_create). 

The following command creates a Windows Server virtual machine. When prompted, provide a username and password to be used as the credentials for the virtual machine:

```azurecli-interactive
  az vm create \
    --name myVM \
    --resource-group myResourceGroup \
    --public-ip-address myPublicIP \
    --public-ip-sku Standard \
    --size Standard_A2 \
    --image MicrosoftWindowsServer:WindowsServer:2019-Datacenter:latest \
    --admin-username azureuser
```

## Change private IP address to static

In this section, you'll change the private IP address from **dynamic** to **static** for the virtual machine you created previously. 

Use [az network nic ip-config update](/cli/azure/network/nic/ip-config#az_network_nic_ip_config_update) to update the network interface configuration.

The following command changes the private IP address of the virtual machine to static:

```azurecli-interactive
  az network nic ip-config update \
    --name ipconfigmyVM \
    --resource-group myResourceGroup \
    --nic-name myVMVMNic \
    --private-ip-address 10.0.0.4
```

> [!WARNING]
> Though you can add private IP address settings to the operating system, we recommend not doing so until after reading [Add a private IP address to an operating system](virtual-network-network-interface-addresses.md#private).

## Clean up resources

When no longer needed, you can use [az group delete](/cli/azure/group#az_group_delete) to remove the resource group and all of the resources it contains:

```azurecli-interactive
  az group delete --name myResourceGroup --yes
```

## Next steps

- Learn more about [public IP addresses](public-ip-addresses.md#public-ip-addresses) in Azure.
- Learn more about all [public IP address settings](virtual-network-public-ip-address.md#create-a-public-ip-address).
- Learn more about [private IP addresses](private-ip-addresses.md) and assigning a [static private IP address](virtual-network-network-interface-addresses.md#add-ip-addresses) to an Azure virtual machine.
- Learn more about creating [Linux](../../virtual-machines/windows/tutorial-manage-vm.md?toc=%2fazure%2fvirtual-network%2ftoc.json) and [Windows](../../virtual-machines/windows/tutorial-manage-vm.md?toc=%2fazure%2fvirtual-network%2ftoc.json) virtual machines.
