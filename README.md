---
page_type: sample
languages:
- powershell
products:
- azure
- azure-netapp-files
description: "This project demonstrates how to use Powershell with NetApp Files SDK for Microsoft.NetApp resource provider to deploy NFSv3 or NFSv4.1 Volume."
---

# Azure NetAppFiles SDK NFSv3/NFS4.1 Sample Powershell

This project demonstrates how to deploy NFSv3/NFSv4.1 protocol type volume using Powershell and Azure NetApp Files SDK.

In this sample application we perform the following operations:

* Creation
  * ANF Account
  *	Capacity pool 
  * Primary NFS v4.1 Volume 
 
* Deletion, the clean up process takes place (not enabled by default, please set the parameter CleanupResources to $true if you want the clean up code to take a place),deleting all resources in the reverse order following the hierarchy otherwise we can't remove resources that have nested resources still live.


If you don't already have a Microsoft Azure subscription, you can get a FREE trial account [here](http://go.microsoft.com/fwlink/?LinkId=330212).

## Prerequisites

1. Azure Subscription
1. Subscription needs to be enabled for Azure NetApp Files. For more information, please refer to [this](https://docs.microsoft.com/azure/azure-netapp-files/azure-netapp-files-register#waitlist) document.
1. Resource Group created
1. Virtual Network with a delegated subnet to Microsoft.Netapp/volumes resource. For more information, please refer to [Guidelines for Azure NetApp Files network planning](https://docs.microsoft.com/en-us/azure/azure-netapp-files/azure-netapp-files-network-topologies)
1. Azure PowerShell, please refer to [Install Azure PowerShell](https://docs.microsoft.com/en-us/powershell/azure/install-az-ps?view=azps-4.8.0)
1. Run the following PS command:
	* Install-Module Az.NetAppFiles


# How the project is structured

The following table describes all files within this solution:

| Folder     | FileName                | Description                                                                                                                         |
|------------|-------------------------|-------------------------------------------------------------------------------------------------------------------------------------|
| src        | CreateANFVolume.ps      | Authenticates and executes all operations                                                                                           |
| src\Common | CommonSDK.psm1          | PowerShell module that exposes some functions to perform creation and deletion Azure NetApp Files resource that calls the actual Az.NetAppFiles cmdlets							 |
| src\Common | Utils.psm1              | Static class that exposes a few methods that helps on various tasks, like writting a log to the console for example.                |
| src\Common | AzureAuth.psm1	       | PoweShell module that exposes functions to connect to Azure and choose target subscription                                          |

# How to run the PowerShell script

1. Clone it locally
    ```powershell
    git clone https://github.com/Azure-Samples/netappfiles-powershell-nfs-sdk-sample.git
    ```
	
1. Modes: There are two options to run the script

	Basic mode - Single script create Azure NetApp files with no validation.
	 * Change folder to **netappfiles-powershell-nfs-sdk-sample\src\Basic**
	 * Open CreateANFVolume and edit all the parameters
	 * Save and close
	 * Run the following command
	 ``` powershell
	 CreateANFVolume.ps1
	 ```

	Sample output
	![e2e execution](./media/Basic/e2e-execution.png)

	OR
	
    Advanced mode - More advanced way to run the script to create Azure Netapp Files with validation using modules
	 * Change folder to **netappfiles-powershell-nfs-sdk-sample\src\Advanced**
	 * Change values bewtween brackets [ ] below and then run the command 
     ```powershell 
	 CreateANFVolume.ps1 -SubscriptionId '[subscriptionId]' -ResourceGroupName '[Azure Resource Group Name]' -Location '[Azure Location]' -NetAppAccountName '[ANF Account Name]' -NetAppPoolName '[ANF Capacity Pool Name]' -ServiceLevel [Ultra,Premium, Standard] -NetAppVolumeName '[ANF Volume Name]' -ProtocolType [NFSv3,NFSv4.1] -SubnetId '[Subnet ID]'
     ```
	
	>Note: The below table shows all the mandatory and optional parameters
	
	| Parameter  		| Mandatory | Default Value |
	|-------------------|-----------|---------------|
	| -SubscriptionId   | Yes		| 				|
	| -ResourceGroupName| Yes       | 				|
	| -Location 		| Yes       | 				|
	| -NetAppAccountName| Yes		|				|
	| -NetAppPoolName	| Yes		|				|
	| -ServiceLevel		| Yes		|				|
	| -NetAppPoolSize	| No		| 4398046511104 |
    | -NetAppVolumeName	| Yes		|				|
    | -ProtocolType		| Yes		| 				|
    | -NetAppVolumeSize	| No		| 107374182400	|
    | -SubnetId			| Yes		|				|
    | -EPUnixReadOnly	| No		| False			| 
    | -EPUnixReadWrite	| No		| True			|
    | -AllowedClientsIp	| No		| 0.0.0.0/0		|
    | -CleanupResources	| No		| False			|
	
	Sample output
	![e2e execution](./media/Advanced/e2e-execution.png)

# Troubleshoot

If you encounter the below issue when running the PoweShell command

```
.\CreateANFVolume.ps1 : .\CreateANFVolume.ps1 cannot be loaded. The file .\CreateANFVolume.ps1 is not digitally signed. You cannot 
run this script on the current system.
```

Run the following command
Set-ExecutionPolicy -Scope CurrentUser -ExecutionPolicy Bypass

# References

* [Sign in with Azure PowerShell](https://docs.microsoft.com/en-us/powershell/azure/authenticate-azureps?view=azps-4.8.0)
* [Azure PowerShell AZ Module](https://docs.microsoft.com/en-us/powershell/azure/new-azureps-module-az?view=azps-4.8.0)
* [AZ.NetAppFile](https://docs.microsoft.com/en-us/powershell/module/az.netappfiles/?view=azps-4.8.0#netapp-files)
* [Resource limits for Azure NetApp Files](https://docs.microsoft.com/en-us/azure/azure-netapp-files/azure-netapp-files-resource-limits)
* [Azure Cloud Shell](https://docs.microsoft.com/en-us/azure/cloud-shell/quickstart)
* [Download Azure SDKs](https://azure.microsoft.com/downloads/)
