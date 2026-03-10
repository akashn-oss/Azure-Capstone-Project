# Azure Cloud Security File Upload System (Capstone Project)  
  
## Overview  
  
This project demonstrates a secure cloud-based file upload architecture built using Microsoft Azure services. It combines multiple smaller Azure exercises into a single capstone system that showcases how **frontend cloud applications, object storage, and administrative infrastructure** can work together.  
  
The system allows users to upload files through a web interface hosted on Azure. Uploaded files are stored securely in Azure Blob Storage, while an administrative virtual machine can monitor and manage the stored files using Azure CLI and role-based access control.  
  
The goal of this project is to demonstrate practical understanding of cloud infrastructure, storage security, and access management in Microsoft Azure.  
  
---  
  
# Architecture  

                   User  
                    │  
                    ▼  
            Azure Static Web App  
           (Web Upload Interface)  
                    │  
                    ▼  
            Azure Blob Storage  
          (File Storage Container)  
                    │  
                    ▼  
          Admin Virtual Machine  
        (Security Monitoring Node)

  
### Architecture Explanation  
  
**User**  
  
Users access the web application through a browser and upload files.  
  
**Azure Static Web App**  
  
Hosts the frontend interface for uploading files. This is a Platform-as-a-Service (PaaS) offering that automatically handles deployment and hosting of the web application.  
  
**Azure Blob Storage**  
  
Uploaded files are stored in a private blob container. Blob storage is used because it is optimized for storing large amounts of unstructured data such as files, images, and documents.  
  
**Admin Virtual Machine**  
  
A Linux virtual machine is used as an administrative node to monitor and manage files in storage. The VM accesses the storage account using Azure CLI and RBAC permissions.  
  
---  
  
# Azure Services Used  
  
| Service                       | Type     | Purpose                                  |
| ----------------------------- | -------- | ---------------------------------------- |
| Azure Static Web Apps         | PaaS     | Hosts frontend upload interface          |
| Azure Blob Storage            | PaaS     | Stores uploaded files                    |
| Azure Virtual Machine         | IaaS     | Administrative monitoring and management |
| Shared Access Signature (SAS) | Security | Temporary storage access for uploads     |
| Azure RBAC                    | Security | Identity-based access control            |
  
---  
  
# Step 1: Create a Resource Group  
  
A resource group was created to organize and manage all project resources.  
  
Example resource group name:  

Cloud-capstone-Project

  
Steps:  
  
1. Open Azure Portal  
2. Navigate to **Resource Groups**  
3. Click **Create**  
4. Provide a name and select a region  
5. Deploy the resource group
---  
  
# Step 2: Create Azure Storage Account  
  
A storage account was created to store uploaded files.  
  
Configuration used:  

Storage Type: StorageV2  
Performance: Standard  
Redundancy: Locally Redundant Storage (LRS)  
Secure Transfer: Enabled  
Access Tier: Hot  
Region: Korea Central
  
Steps:  
  
1. Search for **Storage Accounts** in Azure Portal  
2. Click **Create**  
3. Select the resource group  
4. Configure the storage settings  
5. Deploy the storage account  

<img width="1907" height="1005" alt="image" src="https://github.com/user-attachments/assets/ebf46abd-d0ed-4cce-a738-368f0837a474" />


---  
  
# Step 3: Create Blob Storage Container  
  
Inside the storage account, a blob container was created to store uploaded files.  
  
Container name:  

uploads

  
Access level:  

Private

  
Steps:  
  
1. Open the storage account  
2. Navigate to **Containers**  
3. Click **Create Container**  
4. Enter the container name  
5. Set access level to private  

  <img width="1907" height="963" alt="Screenshot 2026-03-10 154530" src="https://github.com/user-attachments/assets/35d266a0-e338-4e5e-82ac-6db2ceb071c6" />

---  
  
# Step 4: Configure CORS  
  
CORS (Cross-Origin Resource Sharing) was configured to allow the web application to upload files directly to blob storage.  
  
Configuration used:  

Allowed Origins: *  
Allowed Methods: GET, PUT, OPTIONS, HEAD  
Allowed Headers: *  
Exposed Headers: *  
Max Age: 200

  
Steps:  
  
1. Open storage account  
2. Navigate to **Resource Sharing (CORS)**  
3. Add the CORS rule  
4. Save configuration  

  <img width="1504" height="481" alt="image" src="https://github.com/user-attachments/assets/0b225228-08f5-435d-a471-b134ce7cfcb1" />

---  
  
# Step 5: Generate Shared Access Signature (SAS)  
  
A Shared Access Signature was generated to allow the frontend application to upload files without exposing the storage account key.  
  
Permissions used:  

Read  
Create  
Write  
List

  
Steps:  
  
1. Open the container **uploads**  
2. Navigate to **Shared Access Tokens**  
3. Select the required permissions  
4. Set expiration time  
5. Generate SAS token and URL  
  
The SAS token is appended to the blob service URL and used by the frontend application for uploading files.  

  <img width="1299" height="925" alt="Screenshot 2026-03-09 200712" src="https://github.com/user-attachments/assets/8533152d-a7a4-4e91-9350-96b10a33aa79" />

---  
  
# Step 6: Build Web Upload Interface  
  
A simple frontend application was created using HTML, CSS, and JavaScript.  
  
Features include:  
  
- File selection  
- Upload button  
- Upload status messages  
- Secure file upload using SAS token  
  
Example upload logic:  
const blobUrl = sasUrl.split("?")[0] + "/" + file.name + "?" + sasUrl.split("?")[1];   
const response = await fetch(blobUrl, {  
  method: "PUT",  
  headers: {  
    "x-ms-blob-type": "BlockBlob"  
  },  
  body: file  
});

The file is uploaded directly from the browser to Azure Blob Storage using the SAS URL.

<img width="1836" height="1046" alt="image" src="https://github.com/user-attachments/assets/23dbc948-f1d7-413c-ae04-eb6fa0f5ee5f" />

---

# Step 7: Deploy Static Web App

The frontend project was deployed using Azure Static Web Apps.

Deployment process:

1. Create GitHub repository
    
2. Upload frontend project files
    
3. Create Azure Static Web App
    
4. Connect the GitHub repository
    
5. Configure deployment workflow
    
6. Deploy the web application
    

Azure automatically builds and hosts the application.

<img width="489" height="425" alt="Screenshot 2026-03-09 190234" src="https://github.com/user-attachments/assets/a801eeb0-af31-444a-ad0c-6effa2796515" />


<img width="1028" height="992" alt="Screenshot 2026-03-09 185924" src="https://github.com/user-attachments/assets/318b5475-d8e3-4017-95af-3527e108c85e" />

---

# Step 8: Verify File Upload

User can select a file from their system and upload it using the web interface.

If the upload succeeds, the file appears in the **uploads container** inside the storage account.

User receive a status message indicating whether the upload succeeded or failed.

<img width="1907" height="1040" alt="Screenshot 2026-03-10 160704" src="https://github.com/user-attachments/assets/7face13c-eddb-4bd0-9db0-cf421c38f23a" />

Link:https://orange-glacier-0960c7b00.1.azurestaticapps.net/
---

# Step 9: Create Admin Virtual Machine

A Linux virtual machine was deployed to act as an administrative node.

Configuration used:

Operating System: Ubuntu Server  
VM Size: B1s  
Authentication: SSH Key  
Public Port: 22 (SSH)

Steps:

1. Navigate to **Virtual Machines**
    
2. Click **Create VM**
    
3. Select resource group
    
4. Configure VM size and authentication
    
5. Deploy VM
    
<img width="1851" height="833" alt="Screenshot 2026-03-10 194139" src="https://github.com/user-attachments/assets/53ebc1cb-cdbd-4c43-8607-81017ccfe1fa" />

---

# Step 10: Connect to the Virtual Machine

The VM can be accessed using Bastion.

<img width="1113" height="614" alt="Screenshot 2026-03-10 093417" src="https://github.com/user-attachments/assets/02ddcf8d-2704-455a-8d4e-7ea26a8715c8" />


---

# Step 11: Install Azure CLI on VM

Azure CLI allows command-line management of Azure resources.

Install CLI:

curl -sL https://aka.ms/InstallAzureCLIDeb | sudo bash

Login to Azure:

az login

This allows the VM to authenticate with Azure.

---

# Step 12: Configure RBAC Permissions

The VM user must have permission to access blob storage.

Role assigned:

Storage Blob Data Contributor

Steps:

1. Open storage account
    
2. Navigate to **Access Control (IAM)**
    
3. Add role assignment
    
4. Assign role to Azure user identity
    

This allows the VM to read, write, and manage blobs.

<img width="1650" height="936" alt="Screenshot 2026-03-10 194443" src="https://github.com/user-attachments/assets/3a764255-c2a9-4508-a74f-a39e6d6620e4" />

---

# Step 13: Monitor Uploaded Files from VM

Using Azure CLI, the VM can list and manage uploaded files.

List blobs:az storage blob list --account-name capstoneblobproject  --container-name uploads --auth-mode login --output table

Download blob:az storage blob download  --account-name capstoneblobproject  --container-name uploads --name filename.pdf --file filename.pdf

Delete blob:az storage blob delete --account-name capstoneblobproject  --container-name uploads --name filename.pdf

This allows administrators to monitor and manage stored files.

<img width="1895" height="450" alt="Screenshot 2026-03-10 162239" src="https://github.com/user-attachments/assets/c918ca3c-1610-47c1-8831-4e0aadcd2b77" />
<br>
<img width="841" height="835" alt="Screenshot 2026-03-10 162423" src="https://github.com/user-attachments/assets/8e0b74dc-85f4-4240-bf6f-2eb4e9eef50a" />
<br>
<img width="1890" height="192" alt="Screenshot 2026-03-10 162613" src="https://github.com/user-attachments/assets/1116eb0b-417b-4049-a23a-3214fc89d04e" />

---

# Security Concepts Demonstrated

## Shared Responsibility Model

|Layer|Responsibility|
|---|---|
|Infrastructure|Microsoft Azure|
|Application Code|User|
|Virtual Machine Security|User|

---

## Secure Storage Access

Security mechanisms implemented:

- Private blob container
    
- SAS token-based upload access
    
- HTTPS-only communication
    
- Limited permissions
    

---

## Identity and Access Management

Azure Role-Based Access Control was used to restrict storage access.

Role assigned:

Storage Blob Data Contributor

This ensures only authorized users can manage stored data.

---

# Learning Outcomes

This project demonstrates practical understanding of:

- Azure cloud infrastructure
    
- Blob object storage
    
- Secure file upload workflows
    
- Azure identity and access control
    
- Role-based permissions
    
- Azure CLI administration
    
- Cloud storage monitoring
