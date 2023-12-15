<div align="center">

consider adding a logo and cover-art

<!-- <h1 align="center"> -->
# Inventory Management System
<!-- </h1> -->

</div>


## **📚 Index**

1. [Overview](#--Overview)
2. Getting Started

_____________________________________________________________________________________


## **🎯 Overview**

The IMS or Inventory Management System is a centralised system, mainly designed for managing Inventory related details for servers and the applications running on them. It eases the Inventory, Configuration and Runtime Status management process by offering a user-friendly and simple CLI-based interface - that can be used for interacting with the stored inventories.

Some of the key-features of this tool that make its management process easier, are as follows:
- A robust RBAC System for better security & authentication
- Data upload from Inventory-Spreadsheets 
- CRUD Operations on the uploaded Inventory-data

As of now, this document aims to empower Keen & Able Employees with a general and basic understanding of the **Inventory Management System** for managing their Project-Inventories. 

<div align="center">
 
| **Some more features that are to be added include Monitoring, Logs, Versioning, Backup & Restoration. Please note that this document is a work in progress and that it is liable to further changes.** |
|----------|
</div>

_____________________________________________________________________________________

<!-- <div align="center"> -->

## 🌱 **Getting Started** 
<!-- </div> -->

### **Setting-Up**                                                      
In order **to setup IMS** on a machine, the latest version can be downloaded from [here](). Once downloaded, follow these steps from the extracted directory: 
- Open the 'etcd-v3.5.10-linux-amd64' sub-directory or install etcd from [here](https://etcd.io/docs/v3.4/install/#install-pre-built-binaries) if system has a different architecture
- Copy the 'etcd' binary to /usr/local/bin and run the 'etcd' command inside the extracted directory
- In 2 separate terminal tabs, run the 'ims-server' and 'imsclient' executables 

### **Creating Inventory-Spreadsheet**                              
Once the etcd-database and IMS-Server are running in the background, the IMS-Client can now be used for managing inventories. However, it is first required for the user to have their Inventory details added in a Spreadsheet that follows [this format](https://docs.google.com/spreadsheets/d/1uD_DGOMjUwTxNnKYPTUNstaMcq13ON43Eb_JR8EJIVc/edit#gid=0). Once the spreadsheet has been created, it should be downloaded in the XLSX format for uploading it to IMS.

While creating an Inventory-Spreasheet, the following should be ensured:
- No merged cells exist
- Project names & Column-headers do not contain spaces. Ex: `PROJECT 1` should be `PROJECT_1` under the Project column
- Names of column headers are not changed
- The standardised format is being followed

### **Creating Custom-Commands**                    
IMS provides the user with the flexibility to create their own custom commands for retrieving inventory-data, by defining column-headers from the Inventory Spreadsheet, for displaying the inventory-data. 

| Example: `get nodes` could be used to tabularly display Inventory-data with columns like RAM, CPU, WWWN, etc. |
|----------|

Now when the user would run 'get <custom command>', the columns defined under that mentioned command will be displayed in the IMS-Client application.

In order to create custom-commands, the user is required to open the `ims.conf` file from the extracted directory. An explanation of what the content of the `ims.conf` includes is as follows:
```
http://127.0.0.1:3000                                                                -> IP of Machine Running the IMS-Server
                                                                                     -> Mandatory Gap
3                                                                                    -> Number of commands
NAMESPACE                                                                            -> Command Name
PROJECT=project_name                                                                 -> Project Name (Same as Project Column)
HOSTNAME,IP,APPLICATION_ENVIRONMENT,DATA_CENTER,SETUP_ENVIRONMENT                    -> Included Columns
DEPLOYMENTS                                                                          -> Command Name
PROJECT=Shiksha_Portal                                                               -> Project Name (Same as Project Column)
HOSTNAME,IP,APP_NAME,OWNER_CONCTACT_DETAIL,APP_DESCRIPTION,GIT_ENVIRONMENT           -> Included Columns
NODES                                                                                -> Command Name
PROJECT=Shiksha_Portal                                                               -> Project Name (Same as Project Column)
RAM,MEMFREE,MEMUSED,SWAPFREE,SWAPTOTAL,TYPE,OS,OS_VERSION,KERNEL_VERSION,DATA_CENTER -> Included Columns
```

In the above file, the IP address is of the machine running the IMS-Server. Since we are running IMS-Server on the same machine as the IMS-Client, we use the loop-back address. While creating the custom-commands, the following should therefore be ensured:
- IP Address should be not be changed and the mandatory gap should not be cleared 
- The sequence of the Command Names, Project Names and Included Columns should not change
- Any spaces (" ") should be avoided
- Mentioned Project Name and Included Columns should match the Inventory-Spreadsheet

### **Managing Inventories With IMS-Client**                               
Once the Inventory-Spreadsheet and Custom-Commands have been made, the `ims-client` executable should be run for getting started with managing inventories. Provided below are explanations of the steps required to use IMS-Client for Managing Inventories.

1. After the `ims-client` executable has been started, the user would be required to login. Login with User `admin` with Password `admin`, that is created when this application has been run for the first time. 
![img](https://i.imgur.com/RymRcQM.png)
