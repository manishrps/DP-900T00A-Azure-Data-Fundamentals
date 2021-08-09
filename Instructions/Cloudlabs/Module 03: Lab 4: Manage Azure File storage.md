# Manage Azure File storage

In this exercise, you'll learn how to create and manage file shares, and upload and download files in Azure File storage.


### Task 1: Create a file share
--------------------------------

Microsoft provides two graphical tools you can use to create and manage file shares in Azure Storage: the Azure portal, and Azure Storage Explorer.

#### Using the Azure portal:

1. In the Azure portal, in the left-hand navigation menu, select Home

    ![](media/lab4/task3/1.png)

2. On the Home page, select Storage accounts, and then select the storage account present in the page that starts with the name **storage**. Copy the storage account name into a notepad for later tasks.

    ![](media/lab4/select-storage.png)

3. On the Storage Account page, under **Data storage**, select **File Shares**. On the File shares page, select **+ File share** to create a new file share.

    ![](media/lab4/create-fs.png)
    
4. On the New file share window give **Name** as **reports** then click on **create**.

    ![](media/lab4/create-fs2.png)
    
5. After you've created a share, Click on **reports**. you can use the Azure portal to add directories to the share, upload files to the share, and delete the share. 

    ![](media/lab4/fs-menu.png)

6. The Connect command generates a PowerShell script that you can run to attach to the share from your local computer. You can then use the share as though it was a local disk drive. Click on **Connect** and copy the **PowerShell** script

    ![](media/lab4/connect-fs.png)
    
    ![](media/lab4/connect-fs1.png)    
    
7. Now in LabVM go to start menu and type **Powershell ISE** in search bar, then open it.

8. Go to View and click on show script pane to open editor panel. then paste the connection string you copied and click on execute button.

    ![](media/lab4/sh-pane.png) 
    ![](media/lab4/ise-1.png) 
    ![](media/lab4/ise-2.png)     
    
9. Now open windows explorer and verify mounted drive. You can upload and download files to the share using this mounted drive.

    ![](media/lab4/mount-fs.png)   

#### Using Azure Storage Explorer

Azure Storage Explorer is a utility that enables you to manage Azure Storage accounts from your desktop computer.  You can use Storage Explorer to create blob containers and file shares, as well as upload and download files.

1.  In the Azure portal, in the left-hand navigation menu, select Home

    ![](media/lab4/task3/1.png)

2.  On the Home page, select Storage accounts, and then select the storage account present in the page that starts with the name **storage**. Copy the storage account name into a notepad for later tasks.

3.  On the Storage Account page, under **Security + networking**, select **Shared access signature**.

    ![](media/lab4/sas.png)

4.  On the Shared access signature page, under Allowed services, select Blob and File. Deselect Queue and Table.

     - Under Allowed resource types, select Service, Container, and Object.

     - Accept the default permissions, and start and expiry date/time values.

     - Leave Allowed protocols set to HTTPS only, and then click Generate SAS and connection string.

    ![](media/lab4/task3/2-2new.png)

5.  **Make a note of the values in the SAS token, Blob service SAS URL, and File service SAS URL fields.** Copy these values to the clipboard, and paste them into a text file, using Notepad.

    ![](media/lab4/task3/3.png)

6.  Return to your desktop computer, and start Azure Storage Explorer.

    ![](media/lab4/task3/4-1.png)

7.  In Azure Storage Explorer, expand Local & Attached, right-click Storage Accounts, and then select **Azure Storage**, In the popup window select **Storage account or service**

    ![Image showing the Azure Storage Explorer. The user has selected Storage Accounts](https://docs.microsoft.com/en-us/learn/wwl-data-ai/explore-non-relational-data-stores-azure/media/6-local-attached.png)
    
    ![](media/lab4/se2.png)

8.  In the Connect to Azure Storage dialog box, select **Shared access signature (SAS)**, and then select Next.

    ![](media/lab4/se3.png)

9.  On the Enter Connection Info page. In **SAS connection string OR service URL** field, provide the Blob service SAS URL that you generated earlier in the Azure portal which you have copied into a notepad, the Display name fields on this page will populate automatically and then select **Next**.

    ![](media/lab4/task3/6.png)

10.  On the Connection Summary page, select Connect.

     ![](media/lab4/task3/7.png)

11. In Azure Storage Explorer, under Storage Accounts, expand Storage{deploymentID}. Verify that folders appear for Blob Containers and File Shares.

   ![](media/lab4/task3/8new.png)

12. Right-click File Shares, and then select Create File Share. Add a file share named documents.

   ![](media/lab4/task3/10-1.png)

### Task 2: Upload and download files
-------------------------------------

#### Using Azure cli

1.  Return to the azure portal and open the Cloud Shell window, and make sure you are in the labs folder. If not then enter **cd lab** command to enter into the lab folder

2.  Run the following command:

    ```
    bash findip.sh

    ```

    This command returns the public IP address of your Cloud Shell. Make a note of this address.

3.  Switch to the Azure portal and go to the page for your storage account starting with name **storage**.

4.  Under Settings, select Shared access signature, and create another SAS token, this time for your Cloud Shell. Specify the following settings, and then click Generate SAS and connection string:

    TABLE 2
    | Setting | Value |
    | --- | --- |
    | Allowed services | File |
    | Allowed reource types | Container, and Object |
    | Permissions | Accept the default permissions |
    | Start and expiry date/time | Accept the default values |
    | Allowed IP addresses | Enter the IP address that you noted in the step 3 of this task |
    | Allowed protocols | HTTPS only |

   ![](media/lab4/task3/10.png)
   
5. Make a note of the **SAS token** that is generated.

   ![](media/lab4/task3/11.png)

6.  Return to the Cloud Shell and run the following command. Replace <storage-account-name> with the name of your storage account, and replace <SAS-token> with the SAS token for your storage account that you generated in the previous step:

    ```
    azcopy copy 'docs' 'https://<storage-account-name>.file.core.windows.net/documents/productdocs<SAS-token>' --recursive

    ```

    ![](media/lab4/task3/12.png)
    This command uploads all the files in the *docs* folder to a folder named *productdocs* in the *documents* file share. It should upload seven items; one folder and six files.

7. To Download files from Azure file share run the following command. Replace <storage-account-name> with the name of your storage account, and replace <SAS-token> with the SAS token for your storage account that you generated in the previous step:

    ```
    azcopy copy "https://<storage-account-name>.file.core.windows.net/documents/productdocs<SAS-token>" "localfolder" --recursive

    ```        
    
#### Using Storage Explorer
------------------------------------------

1.  Return to Azure Storage Explorer on your desktop computer.

2.  You can upload files to Documents folder using using button in the menu.
    
    ![](media/lab4/upload-se.png)    
    
2.  Close the documents pane, and the select the documents file share to open it again. The productdocs folder should be listed in the right-hand pane.
    
3.  Double-click the productdocs folder, and then double-click the docs sub-folder. This sub-folder contains six documents:

    ![](media/lab4/task3/13.png)

4.  Select any of the files, and then select Open. The file will be downloaded and opened using an editor. If you have Microsoft Word installed on your desktop, it will start and display the file, otherwise WordPad will be used (in this case, you should see the text, but the graphics images in the file won't display correctly).

    The example below shows the *Front Reflector Bracket and Reflector Assembly 3.doc* file.

    ![Image showing the Front Reflector Bracket and Reflector Assembly 3.doc file](https://docs.microsoft.com/en-us/learn/wwl-data-ai/explore-non-relational-data-stores-azure/media/6-word-document.png)
