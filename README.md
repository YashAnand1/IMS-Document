<div align="center">

consider adding a logo and cover-art

<!-- <h1 align="center"> -->
# Inventory Management System
<!-- </h1> -->

</div>

## **📚 Index**

- [**🎯 Overview**](#-overview)
- [**🌱 Getting Started**](#-getting-started)
  - [🌿 Setting-Up](#-setting-up)
  - [🌿 Creating Inventory-Spreadsheet](#-creating-inventory-spreadsheet)
  - [🌿 Creating Custom-Commands](#-creating-custom-commands)
  - [🌿 Managing Inventories With IMS-Client](#-managing-inventories-with-ims-client)
- [**🎊 User Access & Control**](#-User-Access--Control)
- [**💢 Troubleshooting Issues & Errors**](#-troubleshooting-issues--errors)
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

### 🌿 **Setting-Up**                                                      
In order **to setup IMS** on a machine, the latest version can be downloaded from [here](). Once downloaded, follow these steps from the extracted directory: 
- Open the 'etcd-v3.5.10-linux-amd64' sub-directory or install etcd from [here](https://etcd.io/docs/v3.4/install/#install-pre-built-binaries) if system has a different architecture
- Copy the 'etcd' binary to /usr/local/bin and run the 'etcd' command inside the extracted directory
- In 2 separate terminal tabs, run the 'ims-server' and 'imsclient' executables 

### 🌿 **Creating Inventory-Spreadsheet**                              
Once the etcd-database and IMS-Server are running in the background, the IMS-Client can now be used for managing inventories. However, it is first required for the user to have their Inventory details added in a Spreadsheet that follows [this format](https://docs.google.com/spreadsheets/d/1uD_DGOMjUwTxNnKYPTUNstaMcq13ON43Eb_JR8EJIVc/edit#gid=0). Once the spreadsheet has been created, it should be downloaded in the XLSX format for uploading it to IMS. It is recommended to download in the same directory as the IMS-Client application.

While creating an Inventory-Spreasheet, the following should be ensured:
- No merged cells exist
- Project names & Column-headers do not contain spaces. Ex: `PROJECT 1` should be `PROJECT_1` under the Project column
- Names of column headers are not changed
- The standardised format is being followed

### 🌿 **Creating Custom-Commands**                    
IMS provides the user with the flexibility to create their own custom commands for retrieving inventory-data, by defining column-headers from the Inventory Spreadsheet, for displaying the inventory-data. It is heavily recommended to follow the analogy provided below for creating Custom-Commands related to objects. However, it should be noted that if data is insufficient then user can choose objects according to their preference and data. The analogy is as follows:
```
kubectl get ns                                               –> Client/Project details - DC, DR
kubectl get deployments                                      –> Application details
kubectl get statefulsets                                     -> Clustered Applications
kubectl get replicasets                                      –> Load Balanced Applications
kubectl get configmap                                        -> Each Application's configuration
kubectl get nodes                                            -> Infra details - VMs, BareMetal, Kubernetes Cluster Name
kubectl get pods                                             -> Each Application's Instance
kubectl get pv                                               -> Total Storage (SAN/LAN)
kubectl get pvc                                              -> Individual Application's Shared Storage (Block Device, File)
Kubectl get svc                                              -> Application Endpoints (Ports)
Kubectl get ingress                                          -> Load Balancers & Ingress (In case of K8S-based application)
```

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

### 🌿 **Managing Inventories With IMS-Client**                               
Once the Inventory-Spreadsheet and Custom-Commands have been made, the `ims-client` executable should be run for getting started with managing inventories. Provided below are explanations of the steps required to use IMS-Client for Managing Inventories.

1. After the `ims-client` executable has been started, the user would be required to login. Login with User `admin` with Password `admin`, that is created when this application has been run for the first time. 

<div align="center">
![img](https://i.imgur.com/bUR5108.png)
</div>

2. Once logged in, use `upload <spreadsheetname.xlsx>` if Inventory-Spreadsheet created in the extracted directory or `upload </path/to/spreadsheet/spreadsheetname.xlsx>` if the sheet is in another directory.

<div align="center">
![img](https://i.imgur.com/wXMCgpy.png)
</div>

3. Once the spreadsheet has been uploaded, user can start interacting with the uploaded Inventory-Data using the Custom-Commands from the `ims.conf` file. As an example, the output of `get namespace` would be as follows:

<div align="center">

![img](https://i.imgur.com/H8dxFg0.png)
</div>

4. Since it is also possible that a user may wish to retrieve information from a specific data, they can use `list <column name>`. For example if a user wishes to list all of the hostnames, they can use `list hostnames`:

<div align="center">

![img](https://i.imgur.com/TWktVhM.png)
</div>

## 🎊 **User Access & Control**
IMS utilises Role Based Access Control for security purposes to decide what actions are permitted to which user. By default, `admin user` is created with complete access over IMS but users added afterwards, will be low-level users with limited access. The steps of utilising RBAC are as follows:

### **Create Users** using the following command:
```
CREATE --USR <USERNAME>
```

### **Delete Users** using the following command:
```
DELETE --USR <USERNAME>
```

### Command to **Switch Users** while staying in the IMS-Client application:
```
LOGIN
```

### Command to **Change User Password** of the current user using the following command:
```
PASSWD
```

### **Listing Users** using the following command:
```
LIST USR
```

## 💢 Troubleshooting Issues & Errors:

### Error: wrong number of fields
```
Command: upload Inventory.xlsx
2023/12/15 13:35:19 Entered into function
2023/12/15 13:35:19 reading file
2023/12/15 13:35:19 Failed to read CSV file: record on line 92: wrong number of fields
exit status 1
```
- Caused by extra text in cells outside of the data-table
- Also occurs if Spreadsheet has multiple tabs
- Ensure there is no text outside of the data-table & that Spreadsheet only has 1 tab 

### Issue: Output of `get <custom-command` Gives Empty Values
```
╭────┬────────┬─────────┬──────┬────┬────────────┬────────────────┬─────────────╮
│ SL │   RAM  │ MEMFREE │ TYPE │ OS │ OS_VERSION │ KERNEL_VERSION │ DATA_CENTER │
├────┼────────┼─────────┼──────┼────┼────────────┼────────────────┼─────────────┤
╰────┴────────┴─────────┴──────┴────┴────────────┴────────────────┴─────────────╯
NODES
```
- Caused by wrong/inconsistent data in `ims.conf`
- Check to make sure that the sequence mentioned in the **[Creating Custom-Commands](https://github.com/YashAnand1/IMS-Document/blob/main/README.md#creating-custom-commands)** section is correct
- Ensure that the project name is as same as the one mentioned in the Inventory-Spreadsheet
- Run `reload conf` after making changes to the `ims.conf`

### Issue: Output of `get <custom-command` Gives Empty Values
<nil> in tabular-output of get <custom-command>
```
╭────┬─────┬──────────┬───────────────┬─────────────────────────┬─────────────┬───────────────────╮
│ SL │     │ HOSTNAME │ IP            │ APPLICATION_ENVIRONMENT │ DATA_CENTER │ SETUP_ENVIRONMENT │
├────┼─────┼──────────┼───────────────┼─────────────────────────┼─────────────┼───────────────────┤
│ 1  │ XYZ │ Machine1 │ <nil>         │ application             │ dc-Delhi    │ <nil>             │
├────┼─────┼──────────┼───────────────┼─────────────────────────┼─────────────┼───────────────────┤
│ 2  │ XYZ │ Machine2 │ <nil>         │ application             │ dc-Delhi    │ <nil>             │
╰────┴─────┴──────────┴───────────────┴─────────────────────────┴─────────────┴───────────────────╯
```
- Occurs when a header that does not exist has been mentioned in the `ims.conf` for a custom-command
- Ensure that there is no spelling-mistake in the `ims.conf`
- Check if the mentioned header even exists in the Spreadsheet 

