# azure-database-migration455

In this project, i will architect and implement a cloud-based database system on Microsoft Azure, showcasing your hands-on expertise in cloud engineering.

## Accomplishments

### Milestone 2

In this milestone, i have provisioned my Windows production VM and established an connection using the Microsoft Remote Desktop application and connected to it using the RDP protocal.

Then, within the VM i locally installed the the SQL server 2022 Developer and the SSMS to allow for a efficent way for database management.

Finally, i restored the AdventureWorks2022.bak utilsing SSMS to store the AdventureWorks database.

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


## Setup

### Virtual Machine setup

- Created and deploy the VM by selecting the RDP 3389 protocal in the Azure portal
- Download the Microsoft Remote Desktop application
- Download the RPD file from the the Azure VM portal.
- Import the file within the Microsoft RD

### SQL Server and SSMS installation

- Installed [SQL Server 2022 Developer](https://go.microsoft.com/fwlink/p/?linkid=2215158&clcid=0x809&culture=en-gb&country=gb).
- After SQL Server has been successfully installed, click on the 'Get SSMS' which will direct you to this the [SQL Server Management Studio](https://learn.microsoft.com/en-us/sql/ssms/download-sql-server-management-studio-ssms?view=sql-server-ver16) download page.
- Follow the instructions to install SSMS.

### Restore Production database

- Load SSMS
- Connect to the SQL Production VM server by clicking on the server.
- Once connected, Right click on the Database Node and click on Restore Database.
- Drag and drop the AdventureWorks2022.bak file into:

           'C:\Program Files\Microsoft SQL Server\MSSQL16.MSSQLSERVER\MSSQL\Backup'

- Add the bak file to the be restored.
- Once added click 'Ok', when it is successful it would be added into your Database node.
