Task 2: Database Deployment & Data Security
 
### Step 1: Create SQL Server
 
1. Search → **"SQL servers"**
2. Click **"+ Create"**
**Basics**:
```
Resource group: TechLink-RG
Server name: techlink-sql-server123
Location: UK South
Authentication: SQL authentication
Admin login: sqladmin
Password: ComplexPass123!
```
 
**Networking**:
```
Connectivity: Public endpoint
Allow Azure services: Yes 
Add current IP: Yes 
```
 
Click **"Review + create"** → **"Create"**
 
---
 
### Step 2: Create Database
 
1. Go to SQL Server → Click **"+ Create database"**
**Basics**:
```
Database name: techlink-db
Workload: Development
Compute + storage: Configure database
  → Looking for basic, standard, premium?
  → Select "Basic" (5 DTUs, 2GB)
  → Apply
```
 
**Security**:
```
Microsoft Defender: Not now
```
 
Click **"Review + create"** → **"Create"**
 
---
 
### Step 3: Verify Encryption
 
**TDE (At Rest)**:
1. Go to database → **"Security"** → **"Data encryption"**
2. Status should show: **Enabled** 
**TLS (In Transit)**:
1. SQL Server → **"Security"** → **"Networking"**
2. Minimum TLS: **1.2** 
**Connection String**:
1. Database → **"Connection strings"**
2. Should contain: `Encrypt=True` 
---
