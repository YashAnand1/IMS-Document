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

As of now, this document solely aims to empower Keen & Able Employees with the required knowledge for managing their Project-Inventories using the IMS. 

<div align="center">
 
| **Some more features that are to be added include Monitoring, Logs, Versioning, Backup & Restoration. Please note that this document is a work in progress and that it is liable to further changes.** |
|----------|
</div>

_____________________________________________________________________________________

<!-- <div align="center"> -->

## 🌱 **Getting Started** 
<!-- </div> -->

**Setting-Up**
In order **to setup IMS** on a machine, the latest version can be downloaded from [here](). Once downloaded, follow these steps from the extracted directory: 
- Open the 'etcd-v3.5.10-linux-amd64' sub-directory or install etcd from [here](https://etcd.io/docs/v3.4/install/#install-pre-built-binaries) if system has a different architecture
- Copy the 'etcd' binary to /usr/local/bin and run the 'etcd' command inside the extracted directory
- In 2 separate terminal tabs, run the 'ims-server' and 'imsclient' executables 

**Creating Inventory-Spreadsheet**
Once the etcd-database and IMS-Server are running in the background, the IMS-Client can now be used for managing inventories. However, it is first required for the user to have their Inventory details added in a Spreadsheet that follows [this format](https://docs.google.com/spreadsheets/d/1uD_DGOMjUwTxNnKYPTUNstaMcq13ON43Eb_JR8EJIVc/edit#gid=0). Once the spreadsheet has been created, it should be downloaded in the XLSX format for uploading it to IMS.

While creating an Inventory-Spreasheet, the following should be ensured:
- No merged cells exist
- Names of column headers are not changed
- The standardised format is being followed

**Creating Custom-Commands**
IMS provides the user with the flexibility to create their own custom commands for retrieving inventory-data, by defining column-headers from the Inventory Spreadsheet, for displaying the inventory-data. 

| **Use-case: When the user would run 'get <custom command>', the columns defined under that mentioned command will be displayed.**
**Example: `get nodes` could be used to tabularly display Inventory-data with columns like RAM, CPU, WWWN, etc.** |
|----------|

The overall 

**Uploading Spreadsheet**
In order to upload the spreadsheet into 
