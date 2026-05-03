# Project Handover Guide

## 1. Project Information
Project name: vinif  
Frappe site name: vinif.test  
Frappe version: v15.x  
Bench version: see docs/HANDOVER_VERSIONS.txt  

## 🔧 System Requirements
Follow as the documentation in official frappe https://docs.frappe.io/framework/user/en/installation#debian-ubuntu

| Dependency       | Frappe v14/v15|
|------------------|---------------|
| **MariaDB**      | 10.6.6+      
| **PostgreSQL**   | 14+          
| **Python**       | 3.10+      
| **NodeJS**       | 18+         
| **Redis**        | 6           
| **Yarn**         | 1.12+ 
| **pip**          | 20+          

## 2. Apps

Install order:

1. frappe
2. vinif
3. hotel_recommend

Repositories:

```bash
https://github.com/thuy4tbn99/vinif
https://github.com/thuy4tbn99/vinif_hotel_recommend.git
```

## 3. Restore Step

### 3.1 Initialize Frappe Bench
```bash
bench init frappe-bench --frappe-branch version-15
cd frappe-bench
```

### 3.2 Get Applications
```bash
bench get-app https://github.com/thuy4tbn99/vinif
bench get-app https://github.com/thuy4tbn99/vinif_hotel_recommend.git
```

### 3.3 Grant Superuser Permission (PostgreSQL)
```sql
ALTER USER username WITH SUPERUSER;
```

### 3.4 Create New Site
```bash
bench new-site site_name.local
  --db-name dbname
  --db-type postgres
  --db-host localhost
  --db-port 5432
  --db-root-username username
  --db-root-password password
  --admin-password admin_pwd
  --verbose
```

### 3.5 Install Dependencies
```bash
bench pip install pandas pyvi
```

### 3.6 Install Apps
```bash
bench --site site_name.local install-app vinif
bench --site site_name.local install-app hotel-recommend
```

### 3.7 Restore Database & Files
```bash
bench --site site_name.local restore /path/to/database.sql.gz
  --with-public-files /path/to/files.tar
  --with-private-files /path/to/private-files.tar
  --db-root-username username
  --db-root-password password
  --db-name dbname
```

### 3.8 Set Active Site
```bash
bench use site_name.local
```

### 3.9 Start Server
```bash
bench start
```

### 3.10 Run Migration (in another terminal, folder frappe-bench)
```bash
bench --site site_name.local migrate
```
