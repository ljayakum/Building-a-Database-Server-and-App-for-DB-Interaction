# Building-a-Database-Server-and-App-for-DB-Interaction
## 🎯 Objective

To learn how to set up and use an AWS-managed relational database instance (Amazon RDS) for application development.

## 🏁 Goal

- Deploy a relational database using Amazon RDS  
- Choose and configure a supported database engine (e.g., MySQL, PostgreSQL)  
- Connect the database to an application  
- Perform basic CRUD operations  
- Understand the benefits of managed database services

- ## 🏗️ Provided Infrastructure

When I start the lab, the following infrastructure is pre-configured:
![preconfigured](https://github.com/user-attachments/assets/61fc60cb-36f5-493c-b84d-20a287d64579)

## 🔧 Task 1: Create a Security Group for RDS

Follow these steps to create a security group that allows your web server to access the RDS DB instance:

1. Open the **AWS Management Console**.
2. In the search bar next to **Services**, type and select **VPC**.
3. In the left navigation pane, click **Security groups**.
4. Click **Create security group**, then configure:
   - **Security group name:** `DB Security Group`
   - **Description:** `Permit access from Web Security Group`
   - **VPC:** Select `Lab VPC`  
     > 💡 Tip: Remove the default VPC if needed and choose `Lab VPC`.

5. Under **Inbound rules**, click **Add rule**:
   - **Type:** `MySQL/Aurora (3306)`
   - **Source:** Select `Web Security Group` by typing `sg` and choosing the group.

6. Click **Create security group** to finish.
![security group](https://github.com/user-attachments/assets/825faac2-441c-42d2-8128-d63e7e903805)

## 🌐 Task 2: Create a DB Subnet Group

In this task, I will create a **DB Subnet Group** to specify which subnets RDS can use. Each group must include subnets in at least two Availability Zones for high availability.

### 📝 Steps:

1. Open the **AWS Management Console**.
2. In the search bar, type and select **RDS**.
3. In the left navigation pane, choose **Subnet groups**.  
   > 💡 If the pane is hidden, click the menu icon (☰) in the top-left corner.

4. Click **Create DB Subnet Group** and configure:
   - **Name:** `DB-Subnet-Group`
   - **Description:** `DB Subnet Group`
   - **VPC:** `Lab VPC`

5. Scroll to the **Add subnets** section:
   - Under **Availability Zones**, select: `us-east-1a` and `us-east-1b`
   - Under **Subnets**, select the subnets with CIDR ranges:  
     - `10.0.1.0/24`  
     - `10.0.3.0/24`

6. Verify the selected subnets appear in the **Subnets selected** table.

![created rds](https://github.com/user-attachments/assets/24feac6d-57b7-43ad-991c-f1ae5dd2ee76)


8. Click **Create**.

> ✅ This DB Subnet Group will be used when launching the RDS database in the next task.
>
## 🗄️ Task 3: Create an Amazon RDS DB Instance (MySQL)

In this task, I will launch a **Multi-AZ MySQL RDS database instance** for high availability and durability.

### 📝 Steps:

1. In the AWS Management Console, go to **RDS**.
2. In the left navigation pane, click **Databases**.
3. Click **Create database**.  
   > 💡 If prompted, choose **Switch to the new database creation flow**.

### 🔧 Configuration:

#### 🔢 Engine & Template
- **Engine:** MySQL  
- **Template:** Dev/Test  
- **Availability & durability:** Multi-AZ DB instance

#### ⚙️ Settings
- **DB instance identifier:** `lab-db`
- **Master username:** `main`
- **Master password:** `lab-password`
- **Confirm password:** `lab-password`

#### 🖥️ Instance Class
- **Class type:** Burstable classes (t classes)
- **Instance type:** `db.t3.micro`

#### 💾 Storage
- **Type:** General Purpose (SSD)
- **Allocated storage:** 20 GiB

#### 🌐 Connectivity
- **VPC:** Lab VPC
- **VPC Security Group:** Select `DB Security Group`, deselect `default`

#### 📊 Monitoring (expand Additional Configuration)
- Uncheck: `Enable Enhanced monitoring`

#### ⚙️ Additional Configuration
- **Initial DB name:** `lab`
- Uncheck:
  - `Enable automatic backups`
  - `Enable encryption`

4. Click **Create database**.  
   > ⚠️ If you get a permission error about `iam:CreateRole`, ensure you unchecked **Enable Enhanced monitoring**.

5. After creation, click the **lab-db** link to view details.

6. Wait for the status to change to **Modifying** or **Available** (may take ~4 mins).

7. Scroll to **Connectivity & security** and copy the **Endpoint** (e.g., `lab-db.xxxx.us-east-1.rds.amazonaws.com`).

8. Paste and save the **Endpoint** in a text editor — you’ll need it later.

![creating data dase](https://github.com/user-attachments/assets/99b12393-155e-4a91-a892-e8aa841a8104)

![settings](https://github.com/user-attachments/assets/7dd8d263-0b56-4a32-b132-741b0eb30584)

## 🌐 Task 4: Connect the Web Application to Your RDS Database

In this task, I will connect a pre-configured web application to the RDS MySQL database created in the previous task.

### 📝 Steps:

1. From the lab interface, open the **AWS Details** dropdown and copy the **WebServer IP address**.
2. Open a new browser tab, paste the IP address, and press **Enter**.
3. Once the web app loads, click on the **RDS** link in the top menu.

### 🔧 Configure the Connection:

Fill in the form using the following values:

- **Endpoint:** (Paste the RDS endpoint copied earlier)
- **Database:** `lab`
- **Username:** `main`
- **Password:** `lab-password`

4. Click **Submit**.![data base](https://github.com/user-attachments/assets/bbc1401c-b707-4115-b994-e45df26a915e)

> ✅ The app will connect to the RDS database and run a command to populate data. After a few seconds, the **Address Book** interface will appear.

> ![final output](https://github.com/user-attachments/assets/668dc639-7892-4dbe-a399-b71a7b0805af)


### 🧪 Test the App:

- Add, edit, and delete contacts.
- Confirm the data is saved and persists across sessions.

> ☁️ Data is stored in your RDS instance and replicated across **two Availability Zones** automatically (Multi-AZ deployment).
>
>  ## ✅ Lab Wrap-Up (Quick Notes)

- Created a **Security Group** to allow web server access to RDS.
- Set up a **DB Subnet Group** with 2 Availability Zones.
- Deployed a **Multi-AZ MySQL RDS instance**.
- Connected a **web app** to the RDS database.
- Verified **CRUD operations** through the Address Book app.

### 🔍 Key Takeaways:
- Learned to launch and secure an RDS instance.
- Practiced setting up high availability (Multi-AZ).
- Integrated RDS with a web front-end.




