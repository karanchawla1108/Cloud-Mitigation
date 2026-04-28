### Step 1: Create Resource Group
 
1. Login to [Azure Portal](https://portal.azure.com)
2. Search → **"Resource groups"**
3. Click **"+ Create"**
4. Fill:
   - **Subscription**: Your subscription
   - **Resource group**: `TechLink-RG`
   - **Region**: `UK South`
5. Click **"Review + create"** → **"Create"**
---
 
### Step 2: Create Virtual Network
 
1. Search → **"Virtual networks"**
2. Click **"+ Create"**
**Basics**:
- **Resource group**: `TechLink-RG`
- **Name**: `TechLink-VNet`
- **Region**: `UK South`
**IP Addresses**:
- **Address space**: `10.0.0.0/16`
- Delete default subnet
- Click **"+ Add subnet"**:
  - **Name**: `App-Subnet`
  - **Address range**: `10.0.1.0/24`
  - Click **"Add"**
- Click **"+ Add subnet"** again:
  - **Name**: `Database-Subnet`
  - **Address range**: `10.0.2.0/24`
  - Click **"Add"**
Click **"Review + create"** → **"Create"**
 
---
 
### Step 3: Create Network Security Group (App)
 
1. Search → **"Network security groups"**
2. Click **"+ Create"**
3. Fill:
   - **Name**: `App-NSG`
   - **Resource group**: `TechLink-RG`
   - **Region**: `UK South`
4. Create
**Configure Inbound Rules**:
 
Go to App-NSG → **"Inbound security rules"** → **"+ Add"**
 
**Rule 1 - HTTP**:
```
Source: Any
Destination port: 80
Protocol: TCP
Action: Allow
Priority: 100
Name: Allow-HTTP
```
 
**Rule 2 - HTTPS**:
```
Source: Any
Destination port: 443
Protocol: TCP
Action: Allow
Priority: 110
Name: Allow-HTTPS
```
 
**Rule 3 - SSH**:
```
Source: Any
Destination port: 22
Protocol: TCP
Action: Allow
Priority: 120
Name: Allow-SSH
```
 
---
 
### Step 4: Create Network Security Group (Database)
 
1. **"+ Create"** another NSG
2. **Name**: `Database-NSG`
3. Same resource group/region
**Configure Inbound Rules**:
 
**Rule - Internal Only**:
```
Source: IP Addresses
Source IP: 10.0.1.0/24
Destination port: 1433,5432,3306
Protocol: TCP
Action: Allow
Priority: 100
Name: Allow-Internal-DB
```
 
> Default deny rule blocks all other traffic.
 
---
 
### Step 5: Attach NSGs to Subnets
 
1. Go to **Virtual networks** → `TechLink-VNet`
2. Click **"Subnets"**
**For App-Subnet**:
- Click `App-Subnet`
- **Network security group**: Select `App-NSG`
- **Save**
**For Database-Subnet**:
- Click `Database-Subnet`
- **Network security group**: Select `Database-NSG`
- **Save**
---
 
### Step 6: Create Virtual Machine 1
 
1. Search → **"Virtual machines"**
2. Click **"+ Create"** → **"Azure virtual machine"**
**Basics**:
```
Resource group: TechLink-RG
VM name: TechLink-VM1
Region: UK South
Image: Ubuntu Server 22.04 LTS
Size: Standard_B1s
Authentication: Password
Username: azureuser
Password: ComplexPass123!
```
 
**Networking**:
```
Virtual network: TechLink-VNet
Subnet: App-Subnet
Public IP: Create new
NIC NSG: None (using subnet NSG)
```
 
Click **"Review + create"** → **"Create"**
 
Wait 2-3 minutes.
 
---
 
### Step 7: Create Virtual Machine 2
 
Repeat Step 6 with:
- **VM name**: `TechLink-VM2`
- Everything else identical
