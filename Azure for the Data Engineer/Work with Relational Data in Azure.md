# Work with Relational Data in Azure

- **Provision an Azure SQL database to store application data**

    ```powershell
    az configure --defaults group=learn-a692e9b8-f9dd-41c3-bccb-9cfe383f29e1 sql-server=[server-name]

    # Run az sql db list to list all databases on your Azure SQL logical server.
    az sql db list

    # we want to see only the database names
    az sql db list | jq '[.[] | {name: .name}]'

    # Run this az sql db show command to get details about the Logistics database
    az sql db show --name Logistics

    # This time, pipe the output to jq to limit output to only the name, maximum size, and status of the Logistics database
    az sql db show --name Logistics | jq '{name: .name, maxSizeBytes: .maxSizeBytes, status: .status}'
    ```

    ```powershell
    # Run this az sql db show-connection-string command to get the connection string to the Logistics database in a format that sqlcmd can use
    az sql db show-connection-string --client sqlcmd --name Logistics

    # Run the sqlcmd statement from the output of the previous step to create an interactive session
    sqlcmd -S tcp:contoso-1.database.windows.net,1433 -d Logistics -U martina -P "password1234$" -N -l 30

    ```

- **Azure Database for PostgreSQL Server**

    ```powershell
    serverName=wingtiptoys-$RANDOM
    userName=azureuser

    # Use the az postgres server create method to create a new database
    az postgres server create \
       --name $serverName \
       --resource-group learn-a374d24a-9e83-422c-919b-4a3346eef954 \
       --location centralus \
       --sku-name B_Gen5_1 \
       --storage-size 20480 \
       --backup-retention 15 \
       --version 10 \
       --admin-user $userName \
       --admin-password "P@ssw0rd"

    ```

    ```powershell
    # create a firewall rule
    az postgres server firewall-rule create \
      --resource-group learn-a374d24a-9e83-422c-919b-4a3346eef954 \
      --server uniqueserverdummy \
      --name AllowAll \
      --start-ip-address 0.0.0.0 \
      --end-ip-address 255.255.255.255

    # remove firewall rules
    az postgres server firewall-rule delete \
      --name AllowAll \
      --resource-group learn-a374d24a-9e83-422c-919b-4a3346eef954 \
      --server-name <server-name>
    ```

- **Scale multiple Azure SQL Databases with SQL Elastic Pools**

    ```powershell
    ADMIN_LOGIN="ServerAdmin"
    RESOURCE_GROUP=learn-5998cf19-24bf-4674-bc3a-8a41a3a0d233
    SERVERNAME=FitnessSQLServer-$RANDOM
    LOCATION="westeurope"
    PASSWORD="strOng2paSSword"

    # create a server
    az sql server create \
    --name $SERVERNAME \
    --resource-group $RESOURCE_GROUP \
    --location $LOCATION \
    --admin-user $ADMIN_LOGIN \
    --admin-password $PASSWORD

    # Add a database named FitnessVancouverDB to FitnessSQLServer-nnnn
    az sql db create \
    --resource-group $RESOURCE_GROUP \
    --server $SERVERNAME \
    --name FitnessVancouverDB

    # Add a database named FitnessParisDB to FitnessSQLServer-nnnn
    az sql db create \
    --resource-group $RESOURCE_GROUP \
    --server $SERVERNAME \
    --name FitnessParisDB
    ```

    ![Work%20with%20Relational%20Data%20in%20Azure/dtuvcore.png](Work%20with%20Relational%20Data%20in%20Azure/dtuvcore.png)

- **Azure SQL Database**

    ```powershell
    # Set an admin login and password for your database
    export ADMINLOGIN='ServerAdmin'
    export PASSWORD='strOng2paSSword'
    # Set the logical SQL server name. We'll add a random string as it needs to be globally unique.
    export SERVERNAME=server$RANDOM
    export RESOURCEGROUP=learn-19db465c-6e5d-45b2-b6a5-d807525d2c0c
    # Set the location, we'll pull the location from our resource group.
    export LOCATION=$(az group show --name $RESOURCEGROUP | jq -r '.location')

    # create a new Azure SQL Database logical server
    az sql server create \
        --name $SERVERNAME \
        --resource-group $RESOURCEGROUP \
        --location $LOCATION \
        --admin-user $ADMINLOGIN \
        --admin-password "$PASSWORD"

    # Now run the following to create the database called marketplaceDb on the logical server you just created
    az sql db create --resource-group $RESOURCEGROUP \
        --server $SERVERNAME \
        --name marketplaceDb \
        --sample-name AdventureWorksLT \
        --service-objective Basic

    # get the connection string for this database
    az sql db show-connection-string --client sqlcmd --name marketplaceDb --server $SERVERNAME | jq -r

    # output
    sqlcmd -S tcp:server12345.database.windows.net,1433 -d marketplaceDb -U [username] -P [password] -N -l 30

    # Create and configure a Linux virtual machine
    az vm create \
      --resource-group $RESOURCEGROUP \
      --name appServer \
      --image UbuntuLTS \
      --size Standard_DS2_v2 \
      --generate-ssh-keys

    # response
    {
      "fqdns": "",
      "id": "/subscriptions/nnnnnnnn-nnnn-nnnn-nnnn-nnnnnnnnnnnn/resourceGroups/learn-nnnnnnnn-nnnn-nnnn-nnnn-nnnnnnnnnnnn/providers/Microsoft.Compute/virtualMachines/appServer",
      "location": "westus",
      "macAddress": "nn-nn-nn-nn-nn-nn",
      "powerState": "VM running",
      "privateIpAddress": "nn.nn.nn.nn",
      "publicIpAddress": "nnn.nnn.nnn.nnn",
      "resourceGroup": "learn-nnnnnnnn-nnnn-nnnn-nnnn-nnnnnnnnnnnn",
      "zones": ""
    }

    # ssh to vm
    ssh publicIpAddress

    # install tools on VM
    echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bash_profile
    echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bashrc
    source ~/.bashrc
    curl https://packages.microsoft.com/keys/microsoft.asc | sudo apt-key add -
    curl https://packages.microsoft.com/config/ubuntu/16.04/prod.list | sudo tee /etc/apt/sources.list.d/msprod.list
    sudo apt-get update
    sudo ACCEPT_EULA=Y apt-get install -y mssql-tools unixodbc-dev
    ```

- **Firewalls**
    - configured at the **server** and/or **database** level, and will specifically state which network resources are allowed to establish a connection to the database.
    - **Server-level firewall rules**
        - Allow access to Azure services
        - IP address rules
        - Virtual network rules
    - **Database-level firewall rules**
        - IP address rules

    ### Server-level firewall rules

    ![Work%20with%20Relational%20Data%20in%20Azure/2-server-ip-rule-1.png](Work%20with%20Relational%20Data%20in%20Azure/2-server-ip-rule-1.png)

    These rules enable clients to access your entire Azure SQL server, that is, all the databases within the same logical server. There are three types of rules that can be applied at the server level.

    The **Allow access to Azure services** rule allows services within Azure to connect to your Azure SQL Database. When enabled, this setting allows communications from all Azure public IP addresses. This includes all Azure Platform as a Service (PaaS) services, such as Azure App Service and Azure Container Service, as well as Azure VMs that have outbound Internet access. This rule can be configured through the **ON/OFF** option in the firewall pane in the portal, or by an IP rule that has 0.0.0.0 as the start and end IP addresses.

    I**P address rules** are rules that are based on specific public IP address ranges. IP addresses connecting from an allowed public IP range will be permitted to connect to the database.

    **Virtual network rules** allow you to explicitly allow connection from specified subnets inside one or more Azure virtual networks (VNets). Virtual network rules can provide greater access control to your databases and can be a preferred option depending on your scenario. Since Azure VNet address spaces are private, you can effectively eliminate exposure to public IP addresses and secure connectivity to those addresses you control.

    **Database-level firewall rules**

    These rules allow access to an individual database on a logical server and are stored in the database itself. For database-level rules, only **IP address rules** can be configured. They function the same as when applied at the server-level, but are scoped to the database only.

    Whenever possible, as a best practice, use **database-level IP** firewall rules to enhance security and to make your database more portable.

    Use **server-level IP** firewall rules for administrators and when you have several databases with the same access requirements, and you don't want to spend time configuring each database individually

    ```powershell
    sqlcmd -S tcp:serverNNNN.database.windows.net,1433 -d marketplaceDb -U 'ServerAdmin' -P 'strOng2paSSword' -N -l 30

    # receive and error
    # by defeault DB does not allow access from any connection

    sqlcmd -S tcp:server6674.database.windows.net,1433 -d marketplaceDb -U 'ServerAdmin' -P 'strOng2paSSword' -N -l 30

    # Use a database-level IP address rule
    EXECUTE sp_set_database_firewall_rule N'Allow appServer database level rule', '[From IP Address]', '[To IP Address]';
    GO
    EXECUTE sp_set_database_firewall_rule N'My Firewall Rule', '40.112.128.214', '40.112.128.214'

    # delete the rule above
    EXECUTE sp_delete_database_firewall_rule N'Allow appServer database level rule';
    GO
    ```

    Using a database-level rule allows access to be isolated specifically to the database. This can be useful if you'd like to keep your network access configured per database.

    **Use a server-level IP address rule**

    ![Work%20with%20Relational%20Data%20in%20Azure/2-ip-address-rule.png](Work%20with%20Relational%20Data%20in%20Azure/2-ip-address-rule.png)

    - requires a static IP or an IP from a defined IP address range; if the IP is dynamic and changes, we'd have to update the rule to ensure connectivity
    - The appServer VM is currently configured with a dynamic IP address, so this IP address is likely to change at some point, breaking our access as soon as that happens

    **Use a server-level virtual network rule**

    What we've done here effectively removes any public access to the SQL server, and only permits access from the specific subnet in the Azure VNet we defined. If we were to add additional app servers in that subnet, no additional configuration would be necessary, as any server in that subnet would have the ability to connect to the SQL server.

- **Access Control**
    - SQL authentication method uses a **username** and **password**
    - **Azure Active Directory** authentication
    - Use **Azure Active Directory** authentication to centrally manage identities of database users and as an alternative to SQL Server authentication.

    ```sql
    # create user
    CREATE USER ApplicationUser WITH PASSWORD = 'YourStrongPassword1';
    GO

    # add roles to user
    ALTER ROLE db_datareader ADD MEMBER ApplicationUser;
    ALTER ROLE db_datawriter ADD MEMBER ApplicationUser;
    GO

    # deny the user ApplicationUser the ability to select data from the SalesLT.Address table
    DENY SELECT ON SalesLT.Address TO ApplicationUser;
    GO
    ```

    **Encryption**

    - Azure SQL Database enforces Transport Layer Security (TLS) encryption at all times for all connections, which ensures all data is encrypted "in transit" between the database and the client.
    - Dynamic data masking
- **Auditing in practice**

    As a best practice, avoid enabling both server blob auditing and database blob auditing together, unless:

    - You want to use a different storage account or retention period for a specific database.
    - You want to audit event types or categories for a specific database that differs from the rest of the databases on the server. For example, you might have table inserts that need to be audited but only for a specific database.
- **Configure and [ASP.NET](http://asp.NET) app that queries an Azure SQL database**

    Microsoft provides several tools that you can use to upload data to your SQL database:

    - SQL Server Integration Services (SSIS)
    - The SQL *BULK INSERT* statement
    - The Bulk Copy Program (bcp) utility

    ```sql
    bcp <database>.dbo.mytable format nul -c -f mytable.fmt -t, -S <server>.database.windows.net -U <username> -P <password>
    ```

    The `bcp` utility has several parameters that control the functionality of the utility. You can specify:

    - The target table (`<database>.<schema>.<table>`)
    - The data to be imported and details about the data (`format nul -c -f mytable.fmt -t,`)
    - The connection details for your database (`-S <server>.database.windows.net -U <username> -P <password>`)

- **Implementing a Relational Database in Microsoft Azure SQL Database**
    - **SQL Server (IaaS)**
        - good for migrating on-premises SQL DB
        - Full Control by You
        - Server on a VM
        - Manually manage backups, recovery
        - Manually availability
        - there is downtime
    - **Azure SQL Database**
        - PaaS
        - Built in backups, patching, recovery etc.
        - ability to manage resources
        - hard to migrate from SQL server
        - cant assigned private IP address
        - Elastic Pool
        - Managed Instance - set of databases
        - Single Database - dedicated resources

            ![Work%20with%20Relational%20Data%20in%20Azure/Screen_Shot_2020-03-25_at_4.55.26_PM.png](Work%20with%20Relational%20Data%20in%20Azure/Screen_Shot_2020-03-25_at_4.55.26_PM.png)

            ![Work%20with%20Relational%20Data%20in%20Azure/Screen_Shot_2020-03-25_at_4.56.26_PM.png](Work%20with%20Relational%20Data%20in%20Azure/Screen_Shot_2020-03-25_at_4.56.26_PM.png)

        - DTU - based assign bundle resources
        - vCore - adjust resources to different DBs and diff setup
        - Tiers - general purpose, business critical, hyperscale

    - **Single Database & Elastic Pool**

    - **Security SQL Database**
        - Network - IP firewall rules, Virtual Networks (subnets)
        - Access Management - Authentication, row-level security
        - Threat Protection - Monitor Logs, Threat Protection
        - Information Protection and Encryption - TLS, Transparent Data Encryption, Dynamic data masking (credit card numbers),
    - **Managed Instance (Deployment Options)**
        - set of databases
        - provide a native virtual network implementation
        - SQL Server On-premises vs Managed Instance
            - High-availability is build in and pre-configured
        - good for migration on premisses applications or IaaS
        - native virtual network implementation
        - good for lift and shift scenarios
    - **SQL Database backups**
        - full backup - every week
        - differential backup - 12 hours
        - transactional backups - committed/uncommitted
        - stored in a standard blob
        - encrypted
        - LTR not available for Managed Instances - on copy only backups mannually

    - **Elastic Database Jobs**
        - Scheduling management jobs
        - Management, reporting, data movement
        - executes jobs on many Azure SQL DBs
        - Requires Job database
        - Target Group - servers, databases, elastic pool etc.
    - **SQL Agent Jobs**
        - used for managed instances
        - job step, schedule, notify
        - properties can't be changed
    - **Sync between Azure and SQL Server On-premises**
        - Sync on-premises and azure sql databases
        - distributed apps
        - globally distributed apps
        - not for disaster recovery, read scale, ETL, migration
        - hub database - master
        - hub member - child db
        - sync database - metadata, schemas
        - Sync data - trigger based - transactional consistency is not guaranteed

            ![Work%20with%20Relational%20Data%20in%20Azure/Screen_Shot_2020-03-25_at_8.37.39_PM.png](Work%20with%20Relational%20Data%20in%20Azure/Screen_Shot_2020-03-25_at_8.37.39_PM.png)

        - each table has to have primary key
        - snapshot must be enabled
        - some types are not supported
- **Implementing Hybrid Data Solutions in Microsoft Azure**
    - components on premises and on the cloud
    - active with data replication (on-premise, cloud)
    - Disaster recovery
    - Tiering of data
    - Backup of data - online/offline
    - when performing service backups - logical backups need to be performed. Not physical since the version might not be the same.