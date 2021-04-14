## Provision non-relational Azure data services

In the sample scenario, you've decided to create the following data stores:

-   A Cosmos DB for holding information about the volume of items in stock. You need to store current and historic information about volume levels, so you can track how levels vary over time. The data is recorded daily.
-   A Data Lake store for holding production and quality data.
-   A blob container for holding images of the products the company manufactures.
-   File storage for sharing reports.

In this exercise, you'll provision and configure the Cosmos DB account, and test it by creating a database, a container, and a sample document. You'll also provision an Azure Storage account that can provide blob, file, and Data Lake storage.

You'll perform this exercise using the Azure portal.

```
Note: Azure can take as little as 5 minutes or as long as 20 minutes to create the Azure Cosmos DB account.
```

### Task 1: Provision and configure a Cosmos DB database and container
----------------------------------------------------------

### Step 1 : Create a Cosmos DB account

1.  Sign in to the [Azure portal](https://portal.azure.com/learn.docs.microsoft.com).

2.  From the left-hand navigation menu in the Azure portal, select Create a resource.

    ![Image From the left-hand navigation menu in the Azure portal. The user has selected Create a resource](https://docs.microsoft.com/en-us/learn/wwl-data-ai/explore-provision-deploy-non-relational-data-services-azure/media/7-create-resource.png)

3.  On the New page, select Azure Cosmos DB.

    ![Image of the New page in the Azure portal. The user has selected Azure Cosmos DB](https://docs.microsoft.com/en-us/learn/wwl-data-ai/explore-provision-deploy-non-relational-data-services-azure/media/7-new-page.png)

4.  On the Create Azure Cosmos DB Account page, on the Basics tabs, enter the details of the account using the values in the following table, and then select Review + create:

    TABLE 1
    | Field | Value |
    | --- | --- |
    | Subscription | Concierge Subscription |
    | Resource Group | [sandbox resource group] (this resource group will have been created for you in the sandbox) |
    | Account Name | Enter a unique name, such as your initials, the date (in numeric format), and the text *cosmosdbaccount*. For example, *jpws01012020cosmosdbaccount* |
    | API | Core (SQL) |
    | Location | Accept the default location |
    | Capacity mode | Provisioned throughput |
    | Apply Free Tier Discount | Do Not Apply |
    | Account Type | Non-Production |
    | Geo-Redundancy | Disable |
    | Multi-region Writes | Disable |

5.  Wait while your settings are validated. If there's a problem, it will be reported at this stage, and you can go back and correct the issue.

6.  Select Create. It can take 10 or 15 minutes to create the account.

    ![Image of the Validation page in the Azure portal. The user has selected Create](https://docs.microsoft.com/en-us/learn/wwl-data-ai/explore-provision-deploy-non-relational-data-services-azure/media/7-create-cosmos-db.png)

### Step 2 : Create a database and a container

1.  In the Azure portal, in the left-hand navigation menu, select All resources, and then select your Cosmos DB account.

    ![Image of the left-hand navigation menu in the Azure portal. The user has selected All resources.](https://docs.microsoft.com/en-us/learn/wwl-data-ai/explore-provision-deploy-non-relational-data-services-azure/media/7-resources.png)

2.  On the page for your Cosmos DB account, select Data Explorer.

    ![Image of the Cosmos DB account page. The user has selected Data Explorer.](https://docs.microsoft.com/en-us/learn/wwl-data-ai/explore-provision-deploy-non-relational-data-services-azure/media/7-cosmos-db-account.png)

3.  On the Data Explorer page, select New Container.

    ![Image of the Data Explorer page. The user has selected New Container.](https://docs.microsoft.com/en-us/learn/wwl-data-ai/explore-provision-deploy-non-relational-data-services-azure/media/7-data-explorer.png)

4.  In the Add Container dialog box, create a new container with the following values, and then select OK:

    TABLE 2
    | Field | Value |
    | --- | --- |
    | Database ID | Select Create new, and enter contosodb |
    | Provision database throughput | Check |
    | Throughput | Select Manual, and specify 400 RU/s (the default) |
    | Container ID | productvolumes |
    | Partition key | /productid (Each product will have a new level recorded each day. Partitioning by product ID enables you to quickly report how the levels for a product vary over time.) |
    | My partition key is larger than 100 bytes | Leave unchecked |

    ![Image of the Add Container dialog box. The user has provided the parameters for a new database and container.](https://docs.microsoft.com/en-us/learn/wwl-data-ai/explore-provision-deploy-non-relational-data-services-azure/media/7-container.png)

5.  In the Data Explorer window. Expand contosodb, expand productvolumes, and then click Items. The container should currently be empty.

    ![Image of the Data Explorer window after the contosodb database has been created. The user is viewing the items in the productvolumes container.](https://docs.microsoft.com/en-us/learn/wwl-data-ai/explore-provision-deploy-non-relational-data-services-azure/media/7-empty-items.png)

6.  Select New Item to create a new document.

    ![Image of the Items page for the customers container. The user has selected New Item.](https://docs.microsoft.com/en-us/learn/wwl-data-ai/explore-provision-deploy-non-relational-data-services-azure/media/7-new-item.png)

7.  Replace the text that appears in the document window with the following JSON document. This is an example document showing the amount of product 99 in stock on 01/01/2020.

    JSONCopy

    ```
    {
        "productid": 99,
        "date": "01/01/2020",
        "in-stock": 500
    }

    ```

8.  Select Save. The document will be added to the container. The new document will have some additional fields that Cosmos DB uses to track and manage the document. You can ignore these fields for now.

    ![Image of the new document, including the fields added by Cosmos DB.](https://docs.microsoft.com/en-us/learn/wwl-data-ai/explore-provision-deploy-non-relational-data-services-azure/media/7-new-document.png)

You've now provisioned a new Cosmos DB account, and created a database and container.

### Task 2 : Provision Azure Storage
-----------------------

### Step 1 : Create an Azure Storage account for Data Lake Storage

1.  On the left-hand navigation menu in the Azure portal, select Create a resource.

2.  On the New page, select Storage account - blob, file, table, queue.

    ![Image of the New page in the Azure portal. The user has selected Storage account - blob, file, table, queue](https://docs.microsoft.com/en-us/learn/wwl-data-ai/explore-provision-deploy-non-relational-data-services-azure/media/7-new-page-2.png)

3.  On the Create storage account page, on the Basics tabs, enter the details of the account using the values in the following table:

    TABLE 3
    | Field | Value |
    | --- | --- |
    | Subscription | Concierge Subscription |
    | Resource Group | [sandbox resource group] |
    | Storage account Name | Enter a unique name, such as your initials, the date (in numeric format), and the text *storage*. For example, *jpws01012020storage* |
    | Performance | Standard |
    | Account kind | Storage V2 (general purpose v2) |
    | Replication | Read-access geo-redundant storage (RA-GRS) |
    | Access tier | Hot |

4.  Select Advanced. On the Advanced page, in the Data Lake Storage Gen2 section, select Enabled, and then select Review + create.

    ![Image of the Advanced page in the Azure portal. The user has enabled Data Lake Storage](https://docs.microsoft.com/en-us/learn/wwl-data-ai/explore-provision-deploy-non-relational-data-services-azure/media/7-advanced.png)

5.  If your settings are validated correctly, select Create.

    It takes approximately 15-20 seconds for the storage account to be provisioned.

### Step 2 : Create a container for Data Lake storage

1.  In the Azure portal, on the left-hand navigation menu, select All resources, and then select your storage account.

2.  On the page for your storage account, under Data Lake Storage, select Containers.

    ![Image of the storage account page. The user has selected Containers under Data Lake Storage.](https://docs.microsoft.com/en-us/learn/wwl-data-ai/explore-provision-deploy-non-relational-data-services-azure/media/7-data-lake-containers.png)

3.  On the Containers page, select + Container, and create a new container named productqualitydata. Leave the Public access level set to Private (no anonymous access), and then click Create.

    ![Image of the storage account page. The user is creating a new Data Lake Storage container named productqualitydata.](https://docs.microsoft.com/en-us/learn/wwl-data-ai/explore-provision-deploy-non-relational-data-services-azure/media/7-new-dl-container.png)

4.  When the container has been created, double-click the productqualitydata container.

5.  On the productqualitydata page, click + Add Directory, and add a directory named plantA.

6.  Add a second directory named plantB.

    Contoso has two manufacturing plants named *Plant A* and *Plant B*. Other applications will upload manufacturing data from each of these plants to the appropriate directory for later analysis.

    ![Image of the productqualitydata page. The user has created two directories named plantA and plantB.](https://docs.microsoft.com/en-us/learn/wwl-data-ai/explore-provision-deploy-non-relational-data-services-azure/media/7-directories.png)

### Step 3 : Create a container for Blob storage

1.  In the Azure portal, on the left-hand navigation menu, select All resources, and then select your storage account.

2.  On the Overview page, select Containers.

    ![Image of the Overview page for the storage account. The user has selected Containers.](https://docs.microsoft.com/en-us/learn/wwl-data-ai/explore-provision-deploy-non-relational-data-services-azure/media/7-blob-containers.png)

3.  On the Containers page, select + Container, and create a new container named images. Set the Public access level to Blob (anonymous read access for blobs only).

    ![Image of the Containers page for the storage account. The user is creating a new container named images.](https://docs.microsoft.com/en-us/learn/wwl-data-ai/explore-provision-deploy-non-relational-data-services-azure/media/7-create-blob-container.png)

    Contoso will use this container to hold product images.

```
Note: The container created for Data Lake Storage will also appear in the Containers page. You could store image data in a Data Lake Storage container, but Contoso want to keep the images separate from product quality data.
```

### Step 4 : Create a file share

1.  On the storage account page, under File service select File shares.

    ![Image of the Overview page for the storage account. The user has selected File shares.](https://docs.microsoft.com/en-us/learn/wwl-data-ai/explore-provision-deploy-non-relational-data-services-azure/media/7-file-shares.png)

2.  On the File shares page, select + File share.

    ![Image of the File shares page for the storage account. The user has selected + File share.](https://docs.microsoft.com/en-us/learn/wwl-data-ai/explore-provision-deploy-non-relational-data-services-azure/media/7-new-file-share.png)

3.  Create a new file share named reports. Leave the Quota empty.

    ![Image of the New file share dialog box. The user has entered the name of the new file share.](https://docs.microsoft.com/en-us/learn/wwl-data-ai/explore-provision-deploy-non-relational-data-services-azure/media/7-create-file-share.png)

4.  On the File shares page, double-click the reports file share.

5.  On the reports page, select + Add directory, and add a directory named manufacturing.

6.  Add a second directory named complaints.

    ![Image of the reports page, showing the manufacturing and complaints directories.](https://docs.microsoft.com/en-us/learn/wwl-data-ai/explore-provision-deploy-non-relational-data-services-azure/media/7-reports.png)

    Contoso will use these directories to hold documents relating to the manufacturing process and customers' complaints. A user that has been granted access to the reports file share can upload and download files from these directories.
