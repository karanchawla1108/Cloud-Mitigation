##  Task 4: Docker Containerization
 
### Step 1: Connect to VM1
 
**Get VM1 IP**:
1. Portal → **Virtual machines** → `TechLink-VM1`
2. Copy **Public IP address**
**SSH Connection**:
```bash
ssh azureuser@YOUR_VM1_IP
```
Password: `ComplexPass123!`
 
---
 
### Step 2: Install Docker on VM1
 
```bash
# Update packages
sudo apt update
 
# Install Docker
sudo apt install docker.io -y
 
# Start Docker
sudo systemctl start docker
sudo systemctl enable docker
 
# Add user to docker group
sudo usermod -aG docker $USER
 
# Verify installation
docker --version
```
 
**Output**: `Docker version 24.x.x`
 
---
 
### Step 3: Run Web Container
 
```bash
# Pull and run nginx
docker run -d -p 80:80 --name techlink-web nginx
 
# Verify running
docker ps
 
# Test locally
curl localhost
```
 
**Expected**: HTML output from nginx
 
---
 
### Step 4: Customize Webpage (Optional)
 
```bash
# Create custom HTML
cat > index.html << 'EOF'
<!DOCTYPE html>
<html>
<head>
    <title>TechLink Solutions</title>
    <style>
        body { 
            font-family: Arial; 
            text-align: center; 
            padding: 50px;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            color: white;
        }
        h1 { font-size: 3em; }
        .vm { color: #ffd700; }
    </style>
</head>
<body>
    <h1>TechLink Solutions</h1>
    <p class="vm">✓ VM1 - Docker Container Running</p>
    <p>Secure Cloud Infrastructure</p>
</body>
</html>
EOF
 
# Copy to container
docker cp index.html techlink-web:/usr/share/nginx/html/index.html
 
# Restart container
docker restart techlink-web
```
 
**Test**: Browse to `http://YOUR_VM1_IP`
 
---
 
### Step 5: Repeat on VM2
 
```bash
# Exit VM1
exit
 
# SSH to VM2
ssh azureuser@YOUR_VM2_IP
 
# Install Docker (same commands as Step 2)
sudo apt update
sudo apt install docker.io -y
sudo systemctl start docker
sudo systemctl enable docker
sudo usermod -aG docker $USER
 
# Run container (same as Step 3)
docker run -d -p 80:80 --name techlink-web nginx
docker ps
 
# Optional: Customize (change VM1 to VM2 in HTML)
```
 
---
 
### Step 6: Test Load Balancer
 
**Get Load Balancer IP**:
1. Portal → **Load balancers** → `TechLink-LB`
2. Copy **Frontend IP**
**Test**:
```bash
curl http://LOAD_BALANCER_IP
```
 
**Expected**: 
- Randomly shows VM1 or VM2 content
- Refresh multiple times to see distribution
---
 
### Step 7: Test Database Connection (Optional)
 
**Install SQL client**:
```bash
# On VM1
sudo apt install mssql-tools unixodbc-dev -y
 
# Test connection
sqlcmd -S techlink-sql-server123.database.windows.net \
       -U sqladmin \
       -P ComplexPass123! \
       -d techlink-db \
       -Q "SELECT @@VERSION"
```
 
**Expected**: SQL Server version info
