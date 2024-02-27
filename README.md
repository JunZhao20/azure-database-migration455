# azure-database-migration455

In this project, i will architect and implement a cloud-based database system on Microsoft Azure, showcasing my hands-on expertise in cloud engineering.

## Setup

### Virtual Machine setup

1. Created and deploy the VM by selecting the RDP 3389 protocol in the Azure portal
2. Download the Microsoft Remote Desktop application
3. Download the RPD file from the the Azure VM portal.
4. Import the file within the Microsoft RD

### SQL Server and SSMS installation

1. Install [SQL Server 2022 Developer](https://go.microsoft.com/fwlink/p/?linkid=2215158&clcid=0x809&culture=en-gb&country=gb).
2. After SQL Server has been successfully installed, click on the 'Get SSMS' which will direct me to this the [SQL Server Management Studio](https://learn.microsoft.com/en-us/sql/ssms/download-sql-server-management-studio-ssms?view=sql-server-ver16) download page.
3. Follow the instructions to install SSMS.

### Restore Production database

1.  Load SSMS
2.  Connect to the SQL Production VM server by clicking on the server.
3.  Once connected, Right click on the Database Node and click on Restore Database.
4.  Drag and drop the AdventureWorks2022.bak file into:

           'C:\Program Files\Microsoft SQL Server\MSSQL16.MSSQLSERVER\MSSQL\Backup'

5.  Add the bak file to the be restored.
6.  Once added click 'Ok', when it is successful it would be added into my Database node.

## Accomplishments

### Milestone 2

In this milestone, i have provisioned my Windows production VM and established an connection using the Microsoft Remote Desktop application and connected to it using the RDP protocol.

Then, within the VM i locally installed the the SQL server 2022 Developer and the SSMS to allow for a efficient way for database management.

Finally, i restored the AdventureWorks2022.bak utilizing SSMS to store the AdventureWorks database.

### Milestone 3

In this milestone, i migrated the AdventureWorks database from the local VM to the production AdventureWorks on Azure SQL database.

The first step i took to accomplish this milestone was so that i had to create and deploy a Azure SQL Database via the Azure portal to serve as a target for the migration, in addition i had set the firewall rule to only allow access to my client ip address.

During the process of creating the Azure SQL Database, i had to create a new server to host the Azure SQL Database. Once this was created i was able to fully deploy the Azure SQL Database.

I downloaded [Azure Data Studio](https://azure.microsoft.com/en-gb/products/data-studio) locally on the production-vm.

Once the Azure Data Studio has been downloaded, i connected the local database on the production VM

In Azure Data Studio i installed two extensions to allow me to migrate the database schema and to migrate the data from the local database to the target Azure SQL Database.

    Extensions

    - Migrating schema : SQL Server Schema Compare
        1. Select source and target database
        2. compare the source and target database
        3. Apply for the once the compare is successful

    - Migrating data : Azure SQL Migration
        1. Follow the steps of the migration assessment

### Milestone 4

Within this milestone, it consists of me making backups for production database on my VM and onto Azure cloud storage, creating a new development environment and restoring the backup database from the production environment to then later creating a automated backup plan for the development production on SSMS.

#### On-premies Backup process

For this backup process, i had to create on-premies backup for the production AdventureWorks database on the production VM.

1. Expand the database node on SSMS, then right click on the database
2. Hover over tasks, and select backup.
3. A new backup window will pop up, then select the database back up type (FULL) and select 'back up to:' to disk.
4. It automatically shows me the directory, it will install to.
5. Confirm the backup by clicking on ok to finalize the backup for my database, this will then create a .bak file as the backup.

#### Upload to blob storage

From the locally stored back up on the production VM, i had to create a cloud storage account on Azure portal and a storage container to securely store the backup in the cloud.

1. Create a Storage account on Azure portal, follow instructions on this process (Name: production0storage).
2. Within the storage account (production0storage), create a container to storage the .bak file.
3. Navigate to the container i want to store the backup file then upload the .bak file by clicking on upload on the top left of the Azure portal.

#### Create development environment

Create a new VM which will be the development environment, this then will allow me to restore the database from the production database backup on to the development environment.

1.  Set up VM development by following 'Virtual Machine setup'.
2.  Download the backup .bak file from 'production0storage' storage account within the container on to the development VM locally and save the file into:

        'C:\Program Files\Microsoft SQL Server\MSSQL16.MSSQLSERVER\MSSQL\Backup'

3.  Click on the Database Node and click on Restore Database, this will show a restore database pop up window.
4.  Select Device, then locate the .bak file in the file PATH in step 2.
5.  Once added, enter the database name of the new database development environment.

#### Automated backup - Development database

This process utilizes SSMS backup management abilities allowing me to create a maintenance backup plan to automate weekly backups for the AdventureWorks development database.

1.  Activate the SQL Server Agent by right clicking on the Agent in the Object explorer and select 'Start'.
2.  Create new query in SSMS.
3.  Create an SQL server Credential within the new query:

        CREATE CREDENTIAL [YourCredentialName]
        WITH IDENTITY = '[Your Azure Storage Account Name]',
        SECRET = 'Access Key';

4.  Run the query and it should create a SQL Server Credential object the Credentials node (PATH : Security > Credentials)

5.  Then navigate to the Maintenance node and right click and select Maintenance Plan Wizard to start the automation process.

6.  Within the this window, Provide a name for this backup plan.

7.  Select the weekly backup under the Schedule, click next which i picked the Full backup.

8.  Continue to click next until i reach the destination page, in here i selected my database that i want to the backup process to apply to and within this page, i selected the 'back up to' to URL as this will allow the back up to be stored in my blob storage and be stored securely in the cloud.

9.  I then headed into the Destination tab, within this tab, select the previously selected SQL server credential we created earlier.

10. Then i entered the storage container name into the 'Azure storage container'.

11. finally, i clicked next until i reached the end keeping all the other pages default, at the end clicking on finish to finalize backup plan.

### Milestone 5

#### Mimicking data lose in production database

I decided to perform a intentional deletion on the Person.Password table which consists of users hash passwords.

This was performed on the Azure data studio on the production database

Before the data removal, there were 19,972 rows.

##### Intentional deletion query

        DELETE TOP (1000)
        FROM Person.[Password];

After the data removal, the new total of rows were 18,972.

#### Restoring production database

This process was accomplished within the Azure portal.

1. I navigated to the Azure SQL database service
2. I clicked on my production database which redirected me into the overview page for the database.
3. Then i located the Restore button at the header which would redirect me to the restore page.
4. I left the source to 'point-in-time' as default.
5. I then set the restore point date and time from 10 minutes before data deletion.
6. Then i set the new database name to 'production-adventureworks-db-restored'.
7. I left all the settings set to default onwards and i then reviewed and created the restored database.

### Milestone 6

#### Geo-replication

This process showcases the disaster recovery feature in Azure.

1. In the Azure portal i navigated to the restored production database and under data management i accessed the replica page.
2. I had to create a new server that is in a different region in the work so proceeded to create new SQL database server by clicking on the 'create new' hyper link under the server input.
3. I created a new server named 'replica-adventureworks' and set the server location to US East.
4. Then for the authentication method i selected 'Use both SQL and Microsoft Entra authentication'
5. Finally i then set the Microsoft Entra Admin to myself.
6. Once my new replica server deployed i continued the Geo replication process for my production database by setting the server name to the server i had just create ('replica-adventureworks')
7. I kept all the settings default then i reviewed and created the replica.

#### Failover and failback testing

This process displays the testing of the failover and failback procedure.

1. I had to create a failover group, i navigated to the primary production server (production-sql-db-server) where my restored AdventureWorks is located in.
2. Under the data management, i selected failover group and within here i created the failover group named 'adventureworks-production-failover' and selected the the replica server to go failover to.
3. Finally i reviewed the failover group and deployed it.

Once the failover group has been created, the failover and failback testing can be accomplished.

1. I navigated to the failover group 'adventureworks-production-failover'.
2. Once i loaded into the failover group, i started the failover by clicking on the 'Failover' button at the header. A warning popped up to confirm your action.
3. Continue to accept the message and the failover will process. Once its successfully the green tick has been moved to the secondary database server.
4. To failback to the primary server, you just click failover again and it would your production database back to my primary production server.
