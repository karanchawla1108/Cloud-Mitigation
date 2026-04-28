##  Task 3: Load Balancer & DDoS Protection
 
### Step 1: Create Load Balancer
 
1. Search → **"Load balancers"**
2. Click **"+ Create"**
**Basics**:
```
Resource group: TechLink-RG
Name: TechLink-LB
Region: UK South
SKU: Standard
Type: Public
```
 
---
 
### Step 2: Frontend IP
 
**Frontend IP configuration**:
1. Click **"+ Add a frontend IP"**
```
Name: LoadBalancerFrontEnd
Public IP: Create new
  Name: TechLink-LB-IP
  SKU: Standard
  Assignment: Static
```
 
---
 
### Step 3: Backend Pool
 
**Backend pools**:
1. Click **"+ Add a backend pool"**
```
Name: TechLink-BackendPool
Virtual network: TechLink-VNet
```
 
**Add VMs**:
- Select `TechLink-VM1`
- Select `TechLink-VM2`
- Click **"Add"** → **"Save"**
---
 
### Step 4: Health Probe
 
**Inbound rules** → **Health probes**:
1. Click **"+ Add a health probe"**
```
Name: HTTP-HealthProbe
Protocol: HTTP
Port: 80
Path: /
Interval: 5 seconds
Unhealthy threshold: 2
```
 
---
 
### Step 5: Load Balancing Rule
 
**Load balancing rules**:
1. Click **"+ Add a load balancing rule"**
```
Name: HTTP-Rule
Frontend IP: Select created IP
Backend pool: TechLink-BackendPool
Protocol: TCP
Port: 80
Backend port: 80
Health probe: HTTP-HealthProbe
Session persistence: None
```
 
Click **"Review + create"** → **"Create"**
 
---
 
### Step 6: Enable DDoS Protection
 
1. Go to **Virtual network** → `TechLink-VNet`
2. Left menu → **"DDoS protection"**
3. **Enable** → **Basic** (free)
4. **Save**
> Basic DDoS is included with Standard Load Balancer at no cost.
 
---
 
