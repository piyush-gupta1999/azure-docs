---
title: Quickstart - Create Azure Managed Instance for Apache Cassandra cluster from the Azure portal
description: This quickstart shows how to create an Azure Managed Instance for Apache Cassandra cluster using the Azure portal.
author: TheovanKraay
ms.author: thvankra
ms.service: managed-instance-apache-cassandra
ms.topic: quickstart
ms.date: 05/31/2022
ms.custom: ignite-fall-2021, mode-ui
---
# Quickstart: Create an Azure Managed Instance for Apache Cassandra cluster from the Azure portal

Azure Managed Instance for Apache Cassandra provides automated deployment and scaling operations for managed open-source Apache Cassandra datacenters, accelerating hybrid scenarios and reducing ongoing maintenance.

This quickstart demonstrates how to use the Azure portal to create an Azure Managed Instance for Apache Cassandra cluster.

## Prerequisites

If you don't have an Azure subscription, create a [free account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) before you begin.

## Create a managed instance cluster

1. Sign in to the [Azure portal](https://portal.azure.com/).

1. From the search bar, search for **Managed Instance for Apache Cassandra** and select the result.

   :::image type="content" source="./media/create-cluster-portal/search-portal.png" alt-text="Screenshot of search for Azure SQL Managed Instance for Apache Cassandra." lightbox="./media/create-cluster-portal/search-portal.png" border="true":::

1. Select **Create Managed Instance for Apache Cassandra cluster** button.

   :::image type="content" source="./media/create-cluster-portal/create-cluster.png" alt-text="Create the cluster." lightbox="./media/create-cluster-portal/create-cluster.png" border="true":::

1. From the **Create Managed Instance for Apache Cassandra** pane, enter the following details:

   * **Subscription** - From the drop-down, select your Azure subscription.
   * **Resource Group**- Specify whether you want to create a new resource group or use an existing one. A resource group is a container that holds related resources for an Azure solution. For more information, see [Azure Resource Group](../azure-resource-manager/management/overview.md) overview article.
   * **Cluster name** - Enter a name for your cluster.
   * **Location** - Location where your cluster will be deployed to.
   * **Initial Cassandra admin password** - Password that is used to create the cluster.
   * **Confirm Cassandra admin password** - Reenter your password.
   * **Virtual Network** - Select an Exiting Virtual Network and Subnet, or create a new one. 
   * **Assign roles** - Virtual Networks require special permissions in order to allow managed Cassandra clusters to be deployed. Keep this box checked if you are creating a new Virtual Network, or using an existing Virtual Network without permissions applied. If using a Virtual network where you have already deployed Azure SQL Managed Instance Cassandra clusters, uncheck this option.

   :::image type="content" source="./media/create-cluster-portal/create-cluster-page.png" alt-text="Fill out the create cluster form." lightbox="./media/create-cluster-portal/create-cluster-page.png" border="true":::

   > [!NOTE]
   > The Deployment of a Azure Managed Instance for Apache Cassandra requires internet access. Deployment fails in environments where internet access is restricted. Make sure you aren't blocking access within your VNet to the following vital Azure services that are necessary for Managed Cassandra to work properly. See [Required outbound network rules](network-rules.md) for more detailed information.
   > - Azure Storage
   > - Azure KeyVault
   > - Azure Virtual Machine Scale Sets
   > - Azure Monitoring
   > - Azure Active Directory
   > - Azure Security

1. Next select the **Data center** tab.

1. Enter the following details:

   * **Data center name** - Type a data center name in the text field.
   * **Availability zone** - Check this box if you want availability zones to be enabled.
   * **SKU Size** - Choose from the available Virtual Machine SKU sizes.
   * **No. of disks** - Choose the number of p30 disks to be attached to each Cassandra node.
   * **No. of nodes** - Choose the number of Cassandra nodes that will be deployed to this datacenter.

   :::image type="content" source="./media/create-cluster-portal/create-datacenter-page.png" alt-text="Review summary to create the datacenter." lightbox="./media/create-cluster-portal/create-datacenter-page.png" border="true":::

   > [!WARNING]
   > Availability zones are not supported in all regions. Deployments will fail if you select a region where Availability zones are not supported. See [here](../availability-zones/az-overview.md#azure-regions-with-availability-zones) for supported regions. The successful deployment of availability zones is also subject to the availability of compute resources in all of the zones in the given region. Deployments may fail if the SKU you have selected, or capacity, is not available across all zones. 

1. Next, click **Review + create** > **Create**

   > [!NOTE]
   > It can take up to 15 minutes for the cluster to be created.

   :::image type="content" source="./media/create-cluster-portal/review-create.png" alt-text="Review summary to create the cluster." lightbox="./media/create-cluster-portal/review-create.png" border="true":::

1. After the deployment has finished, check your resource group to see the newly created managed instance cluster:

   :::image type="content" source="./media/create-cluster-portal/managed-instance.png" alt-text="Overview page after the cluster is created." lightbox="./media/create-cluster-portal/managed-instance.png" border="true":::

1. To browse through the cluster nodes, navigate to the cluster resource and open the **Data Center** pane to view them:

   :::image type="content" source="./media/create-cluster-portal/datacenter.png" alt-text="Screenshot of datacenter nodes." lightbox="./media/create-cluster-portal/datacenter.png" border="true":::

## Scale a datacenter

1. Now that you have deployed a cluster with a single data center, you can scale the nodes up or down by highlighting the data center, and selecting the `Scale` button:

   :::image type="content" source="./media/create-cluster-portal/datacenter-scale-1.png" alt-text="Screenshot of scaling datacenter nodes." lightbox="./media/create-cluster-portal/datacenter-scale-1.png" border="true":::

1. Next, move the slider to the desired number, or just edit the value. When finished, hit `Scale`. 

   :::image type="content" source="./media/create-cluster-portal/datacenter-scale-2.png" alt-text="Screenshot of selecting number of datacenter nodes." lightbox="./media/create-cluster-portal/datacenter-scale-2.png" border="true":::

   > [!NOTE]
   > The length of time it takes for nodes to scale depends on various factors, it may take several minutes. When Azure notifies you that the scale operation has completed, this does not mean that all your nodes have joined the Cassandra ring. Nodes will be fully commissioned when they all display a status of "healthy", and the datacenter status reads "succeeded".

## Add a datacenter

1. To add another datacenter, click the add button in the **Data Center** pane:

   :::image type="content" source="./media/create-cluster-portal/add-datacenter.png" alt-text="Screenshot of adding a datacenter." lightbox="./media/create-cluster-portal/add-datacenter.png" border="true":::

   > [!WARNING]
   > If you are adding a datacenter in a different region, you will need to select a different virtual network. You will also need to ensure that this virtual network has connectivity to the primary region's virtual network created above (and any other virtual networks that are hosting datacenters within the managed instance cluster). Take a look at [this article](../virtual-network/tutorial-connect-virtual-networks-portal.md#peer-virtual-networks) to learn how to peer virtual networks using Azure portal. You also need to make sure you have applied the appropriate role to your virtual network before attempting to deploy a managed instance cluster, using the below CLI command.
   >
   > ```azurecli-interactive  
   >     az role assignment create \
   >     --assignee a232010e-820c-4083-83bb-3ace5fc29d0b \
   >     --role 4d97b98b-1d4f-4787-a291-c67834d212e7 \
   >     --scope /subscriptions/<subscriptionID>/resourceGroups/<resourceGroupName>/providers/Microsoft.Network/virtualNetworks/<vnetName>
   > ```

1. Fill in the appropriate fields:

   * **Datacenter name** - From the drop-down, select your Azure subscription.
   * **Availability zone** - Check this box if you want availability zones to be enabled in this datacenter.
   * **Location** - Location where your datacenter will be deployed to.
   * **SKU Size** - Choose from the available Virtual Machine SKU sizes.
   * **No. of disks** - Choose the number of p30 disks to be attached to each Cassandra node.
   * **No. of nodes** - Choose the number of Cassandra nodes that will be deployed to this datacenter.
   * **Virtual Network** - Select an Exiting Virtual Network and Subnet.  

   :::image type="content" source="./media/create-cluster-portal/add-datacenter-2.png" alt-text="Add Datacenter." lightbox="./media/create-cluster-portal/add-datacenter-2.png" border="true":::

   > [!WARNING]
   > Notice that we do not allow creation of a new virtual network when adding a datacenter. You need to choose an existing virtual network, and as mentioned above, you need to ensure there is connectivity between the target subnets where datacenters will be deployed. You also need to apply the appropriate role to the VNet to allow deployment (see above).

1. When the datacenter is deployed, you should be able to view all datacenter information in the **Data Center** pane:

   :::image type="content" source="./media/create-cluster-portal/multi-datacenter.png" alt-text="View the cluster resources." lightbox="./media/create-cluster-portal/multi-datacenter.png" border="true":::

## Update Cassandra configuration

The service allows update to Cassandra YAML configuration on a datacenter via the portal or by [using CLI commands](manage-resources-cli.md#update-yaml). To update settings in the portal:

1. Find `Cassandra Configuration` under settings. Highlight the data center whose configuration you want to change, and click update:

   :::image type="content" source="./media/create-cluster-portal/update-config-1.png" alt-text="Screenshot of the select data center to update config." lightbox="./media/create-cluster-portal/update-config-1.png" border="true":::

1. In the window that opens, enter the field names in YAML format, as shown below. Then click update.

   :::image type="content" source="./media/create-cluster-portal/update-config-2.png" alt-text="Screenshot of updating the data center Cassandra config." lightbox="./media/create-cluster-portal/update-config-2.png" border="true":::

1. When update is complete, the overridden values will show in the `Cassandra Configuration` pane:

   :::image type="content" source="./media/create-cluster-portal/update-config-3.png" alt-text="Screenshot of the updated Cassandra config." lightbox="./media/create-cluster-portal/update-config-3.png" border="true":::

   > [!NOTE]
   > Only overridden Cassandra configuration values are shown in the portal.

   > [!IMPORTANT]
   > Ensure the Cassandra yaml settings you provide are appropriate for the version of Cassandra you have deployed. See [here](https://github.com/apache/cassandra/blob/cassandra-3.11/conf/cassandra.yaml) for Cassandra v3.11 settings and [here](https://github.com/apache/cassandra/blob/cassandra-4.0/conf/cassandra.yaml) for v4.0. The following YAML settings are **not** allowed to be updated:
   >
   > - cluster_name
   > - seed_provider
   > - initial_token
   > - autobootstrap
   > - client_ecncryption_options
   > - server_encryption_options
   > - transparent_data_encryption_options
   > - audit_logging_options
   > - authenticator
   > - authorizer
   > - role_manager
   > - storage_port
   > - ssl_storage_port
   > - native_transport_port
   > - native_transport_port_ssl
   > - listen_address
   > - listen_interface
   > - broadcast_address
   > - hints_directory
   > - data_file_directories
   > - commitlog_directory
   > - cdc_raw_directory
   > - saved_caches_directory 

## Troubleshooting

If you encounter an error when applying permissions to your Virtual Network using Azure CLI, such as *Cannot find user or service principal in graph database for 'e5007d2c-4b13-4a74-9b6a-605d99f03501'*, you can apply the same permission manually from the Azure portal. Learn how to do this [here](add-service-principal.md).

> [!NOTE] 
> The Azure Cosmos DB role assignment is used for deployment purposes only. Azure Managed Instanced for Apache Cassandra has no backend dependencies on Azure Cosmos DB.  

## Connecting to your cluster

Azure Managed Instance for Apache Cassandra does not create nodes with public IP addresses, so to connect to your newly created Cassandra cluster, you will need to create another resource inside the VNet. This could be an application, or a Virtual Machine with Apache's open-source query tool [CQLSH](https://cassandra.apache.org/doc/latest/cassandra/tools/cqlsh.html) installed. You can use a [template](https://azure.microsoft.com/resources/templates/vm-simple-linux/) to deploy an Ubuntu Virtual Machine. 


### Connecting from CQLSH

After the virtual machine is deployed, use SSH to connect to the machine, and install CQLSH using the below commands:

```bash
# Install default-jre and default-jdk
sudo apt update
sudo apt install openjdk-8-jdk openjdk-8-jre

# Install the Cassandra libraries in order to get CQLSH:
echo "deb http://www.apache.org/dist/cassandra/debian 311x main" | sudo tee -a /etc/apt/sources.list.d/cassandra.sources.list
curl https://downloads.apache.org/cassandra/KEYS | sudo apt-key add -
sudo apt-get update
sudo apt-get install cassandra

# Export the SSL variables:
export SSL_VERSION=TLSv1_2
export SSL_VALIDATE=false

# Connect to CQLSH (replace <IP> with the private IP addresses of a node in your Datacenter):
host=("<IP>")
initial_admin_password="Password provided when creating the cluster"
cqlsh $host 9042 -u cassandra -p $initial_admin_password --ssl
```
### Connecting from an application

As with CQLSH, connecting from an application using one of the supported [Apache Cassandra client drivers](https://cassandra.apache.org/doc/latest/cassandra/getting_started/drivers.html) requires SSL to be enabled. See samples for connecting to Azure Managed Instance for Apache Cassandra using [Java](https://github.com/Azure-Samples/azure-cassandra-mi-java-v4-getting-started) and [.NET](https://github.com/Azure-Samples/azure-cassandra-mi-dotnet-core-getting-started). For Java, we highly recommend enabling [speculative execution policy](https://docs.datastax.com/en/developer/java-driver/4.10/manual/core/speculative_execution/). You can find a demo illustrating how this works and how to enable the policy [here](https://github.com/Azure-Samples/azure-cassandra-mi-java-v4-speculative-execution).

Disabling certificate verification is recommended because certificate verification will not work unless you map I.P addresses of your cluster nodes to the appropriate domain. If you have an internal policy which mandates that you do SSL certificate verification for any application, you can facilitate this by adding entries like `10.0.1.5 host1.managedcassandra.cosmos.azure.com` in your hosts file for each node. If taking this approach, you would also need to add new entries whenever scaling up nodes. 




## Clean up resources

If you're not going to continue to use this managed instance cluster, delete it with the following steps:

1. From the left-hand menu of Azure portal, select **Resource groups**.
1. From the list, select the resource group you created for this quickstart.
1. On the resource group **Overview** pane, select **Delete resource group**.
1. In the next window, enter the name of the resource group to delete, and then select **Delete**.

## Next steps

In this quickstart, you learned how to create an Azure Managed Instance for Apache Cassandra cluster using Azure portal. You can now start working with the cluster:

> [!div class="nextstepaction"]
> [Deploy a Managed Apache Spark Cluster with Azure Databricks](deploy-cluster-databricks.md)
