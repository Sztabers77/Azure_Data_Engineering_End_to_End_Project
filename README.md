# Azure_Data_Engineering_Project

 # 1. Architecture of the project:

   ![Screenshot_24](https://github.com/Sztabers77/Azure_Data_Engineering_Project/assets/155321276/9652eae5-fd32-4d70-bd19-187e1b8e383c)


 # 2.  Environment setup:
   - Create resource group and all needed resources within: Data Factory, Databricks, Key Vault, Data Lake Gen 2, Synapse Analytics (https://learn.microsoft.com/en-us/azure/developer/intro/azure-developer-create-resources):

     ![Screenshot_5](https://github.com/Sztabers77/Azure_Data_Engineering_End_to_End_Project/assets/155321276/0f192f32-c6ca-4a5b-b1b1-e8dfcf4c1aae)

   - Download SQL Server and create one with SSMS feature, next download database backup file 'AdventureWorksLT2017.bak' from https://learn.microsoft.com/en-us/sql/samples/adventureworks-install-configure?view=sql-server-ver16&tabs=ssms and attach this file to the SQL server creating a database:
  
     ![Screenshot_6](https://github.com/Sztabers77/Azure_Data_Engineering_End_to_End_Project/assets/155321276/38549189-857c-4267-b8fb-a0646dd65fb9)
     ![Screenshot_7](https://github.com/Sztabers77/Azure_Data_Engineering_End_to_End_Project/assets/155321276/c4956e17-339b-40ce-8d61-b1534bb8baf8)

   - Create new user and login in SSMS (assign db_datareader role by right-clicking at your user's in the Security -> Users tab) and genereate 'username' and 'password' secrets in the Key Vault with the same values as in your DB:

     ![Screenshot_8](https://github.com/Sztabers77/Azure_Data_Engineering_End_to_End_Project/assets/155321276/4b3a5005-138b-498c-a933-48b44340fd4e)
     ![Screenshot_9](https://github.com/Sztabers77/Azure_Data_Engineering_End_to_End_Project/assets/155321276/89ad67e6-1813-4120-9172-910ae487449e)

#  3. Prerequistes to create the pipeline:
   - Launch Data Factory studio and following this steps - Manage tab -> Integration Runtime -> New - install Self-Hosted Integration Runtime feature with which we can connect with our on-prem SQL server installed in the previous step:
  
     ![Screenshot_10](https://github.com/Sztabers77/Azure_Data_Engineering_End_to_End_Project/assets/155321276/51095d3d-d4b4-41f7-955e-b6133e4ebdb2)
     ![Screenshot_11](https://github.com/Sztabers77/Azure_Data_Engineering_End_to_End_Project/assets/155321276/f5ab15a2-630f-49db-92bc-718d4e1fdf60)
     ![Screenshot_12](https://github.com/Sztabers77/Azure_Data_Engineering_End_to_End_Project/assets/155321276/9726c63a-96dc-4bbe-9ee9-62dda3fa2532)
     ![Screenshot_13](https://github.com/Sztabers77/Azure_Data_Engineering_End_to_End_Project/assets/155321276/2d4e2cdd-d706-4f8c-a54f-d4d9f835e9b0)
     ![selfhostedinstallfinished](https://github.com/Sztabers77/Azure_Data_Engineering_End_to_End_Project/assets/155321276/9e60328e-276d-4944-8675-8c8415467fa9)


   - In the same Manage tab click on 'Linked Service' and create all Linked Services needed in our pipeline - for Databricks Linked Service we'll need a token generated with this instructions - https://docs.databricks.com/en/dev-tools/auth/pat.html#:~:text=In%20your%20Databricks%20workspace%2C%20click,Click%20Generate%20new%20token. and stored in the Key Vault as you could see in the previous screenshot (dbwtoken):
  
     ![Azure Databricks linked service1](https://github.com/Sztabers77/Azure_Data_Engineering_End_to_End_Project/assets/155321276/16ec2f81-8e1d-4011-89e0-63183dd818ac)
     ![Azure databricks linked service2](https://github.com/Sztabers77/Azure_Data_Engineering_End_to_End_Project/assets/155321276/c90e4455-d639-40e8-ba2b-61a41273806f)
     ![azure_linked_service1](https://github.com/Sztabers77/Azure_Data_Engineering_End_to_End_Project/assets/155321276/d49a827e-582c-490a-9e54-fd646b317443)
     ![Screenshot_22](https://github.com/Sztabers77/Azure_Data_Engineering_End_to_End_Project/assets/155321276/fff6c586-a008-4b16-a1d8-6c22637ebe7f)
     ![Screenshot_21](https://github.com/Sztabers77/Azure_Data_Engineering_End_to_End_Project/assets/155321276/19728d90-70f9-46ec-8117-bcf29ba823ee)
     ![Screenshot_20](https://github.com/Sztabers77/Azure_Data_Engineering_End_to_End_Project/assets/155321276/e18f9989-241e-496d-96e3-0e8d093ebef4)

   - Go to the Data Lake storage account and create 3 containers: 'bronze', 'silver' and 'gold' which we'll use to store transformed data:

     ![datalakecontainers](https://github.com/Sztabers77/Azure_Data_Engineering_End_to_End_Project/assets/155321276/f6027229-7733-4ed8-8bee-89d67122c531)

   - Go back to ADF -> Author tab -> Datasets and create all datasets needed in our pipeline:

     ![parquettable dataset3](https://github.com/Sztabers77/Azure_Data_Engineering_End_to_End_Project/assets/155321276/b400be29-5059-4e16-b1f6-421155b57cfa)
     ![parquettable dataset0](https://github.com/Sztabers77/Azure_Data_Engineering_End_to_End_Project/assets/155321276/8adc3e6a-b41b-49ea-a103-0533a4701f98)
     ![parquettable dataset2](https://github.com/Sztabers77/Azure_Data_Engineering_End_to_End_Project/assets/155321276/bda6fb85-66dd-4804-a28b-f12d9cafc882)
     ![Screenshot_23](https://github.com/Sztabers77/Azure_Data_Engineering_End_to_End_Project/assets/155321276/4d253fe1-d047-498b-87ee-d9281eda6b7d)
     ![parquettable dataset4](https://github.com/Sztabers77/Azure_Data_Engineering_End_to_End_Project/assets/155321276/1386dfce-b99d-4b43-92c6-ece5819c960f)
     ![sqlServerCopy datasets](https://github.com/Sztabers77/Azure_Data_Engineering_End_to_End_Project/assets/155321276/551b6e79-8599-4cf9-a0f9-4cf76b063e07)
     ![sqldbtables datasets](https://github.com/Sztabers77/Azure_Data_Engineering_End_to_End_Project/assets/155321276/9596b13a-4d11-4ec8-ab4c-64c1ba4f56c9)

#  4. Transform the data in the Databricks notebooks:

   - Create a Spark cluster in the Compute tab:

     ![Screenshot_26](https://github.com/Sztabers77/Azure_Data_Engineering_Project/assets/155321276/37b36ce5-7e9a-4161-8516-ac79886a88bd)

   - Create 3 notebooks and write the code in the cells as you see here:
     
     ![Screenshot_25](https://github.com/Sztabers77/Azure_Data_Engineering_End_to_End_Project/assets/155321276/7a178fdb-d681-42d8-b355-102b48e3800b)
     ![databricksstorageamount1](https://github.com/Sztabers77/Azure_Data_Engineering_Project/assets/155321276/a886be15-cc16-4045-9d51-cc03fda3a0ad)
     ![databricksstorageamount2](https://github.com/Sztabers77/Azure_Data_Engineering_Project/assets/155321276/1c9f303f-dc7d-4e02-a29c-72db644549bc)
     ![databrickbronzetosilver1](https://github.com/Sztabers77/Azure_Data_Engineering_Project/assets/155321276/06223100-39f6-45ed-be0b-805951064db2)
     ![databricksbronzetosilver2](https://github.com/Sztabers77/Azure_Data_Engineering_Project/assets/155321276/5392be18-9184-4e6e-a0bd-ef2f0aeb74cc)
     ![databrickssilvertogold1](https://github.com/Sztabers77/Azure_Data_Engineering_Project/assets/155321276/72d37ef3-ab1e-4fe7-857a-fe377daa9c20)
     ![databrickssilvertogold2](https://github.com/Sztabers77/Azure_Data_Engineering_Project/assets/155321276/72e021a5-83f0-4722-bc24-36e665713d28)
     ![databrickssilvertogold3](https://github.com/Sztabers77/Azure_Data_Engineering_Project/assets/155321276/dd53e064-b7fd-44a0-9488-cceafd84b4ac)


#  5. Create the pipeline:

   - go to the Author tab and create new pipeline using next activities: Lookup (in the Settings use this Query:
     "SELECT
     s.name as SchemaName,
     t.name as TableName
     FROM sys.tables t
     INNER JOIN sys.schemas s
     ON t.schema_id = s.schema_id
     WHERE s.name LIKE 'SalesLT'"), ForEach (Copy data inside), 2x Databricks Notebook with the next set ups:

     ![lookforalltablesactivity2](https://github.com/Sztabers77/Azure_Data_Engineering_Project/assets/155321276/26e289bd-c08b-43fe-826e-884350f5d536)
     ![for each1](https://github.com/Sztabers77/Azure_Data_Engineering_Project/assets/155321276/c723e824-9895-40ff-8b0e-8bdf010ca28c)
     ![foreach2](https://github.com/Sztabers77/Azure_Data_Engineering_Project/assets/155321276/3745864c-8a7a-4ea5-9baf-3a313dbaa443)
     ![copyeach1](https://github.com/Sztabers77/Azure_Data_Engineering_Project/assets/155321276/57a277be-33d7-4383-9d4c-f2cb48b7b6e4)
     ![copyeach2](https://github.com/Sztabers77/Azure_Data_Engineering_Project/assets/155321276/845a30b9-e852-4ef3-8c7b-15ead00d2049)
     ![copyeach3](https://github.com/Sztabers77/Azure_Data_Engineering_Project/assets/155321276/891d4e5c-1041-4e40-886c-904630c95520)
     ![bronzetosilver1](https://github.com/Sztabers77/Azure_Data_Engineering_Project/assets/155321276/3390aa6e-e92a-4744-9c44-15c4b0b2265b)
     ![bronzetosilver2](https://github.com/Sztabers77/Azure_Data_Engineering_Project/assets/155321276/751acb23-830a-4a8a-94e3-86a328da3b8f)
     ![bronzetosilver3](https://github.com/Sztabers77/Azure_Data_Engineering_Project/assets/155321276/df7dfae9-5c4d-4c38-86c9-4723dc7c3937)
     ![silvertogold1](https://github.com/Sztabers77/Azure_Data_Engineering_Project/assets/155321276/372ae6a9-7ebb-4912-bfed-e6dd7dfa12a5)
     ![silvertogold2](https://github.com/Sztabers77/Azure_Data_Engineering_Project/assets/155321276/832688c9-bc1f-4612-ac63-da9c7ac717bf)
     ![silvertogold3](https://github.com/Sztabers77/Azure_Data_Engineering_Project/assets/155321276/a9f4fbfa-dab7-4d89-943b-fc24d04a52d7)


#  6. Data loading in the Synapse workspace:

   - Open Synapse Analytics, go to the Data tab, click on the plus sign and create a new serverless SQL Database:

     ![synapse1](https://github.com/Sztabers77/Azure_Data_Engineering_Project/assets/155321276/c96d826e-3909-4b39-9a06-b2d5e200b02c)
     ![synapse2](https://github.com/Sztabers77/Azure_Data_Engineering_Project/assets/155321276/27256c8e-dc7f-4a48-a54b-c63f63989b39)
     ![synapse3](https://github.com/Sztabers77/Azure_Data_Engineering_Project/assets/155321276/01c06a53-30f0-42db-85c5-1c279fe852dd)


   - Go to the Develop tab and click on the plus sign to create a new SQL script. Write down this stored procedure, which we'll use in the pipeline to dynamically create views. These views will be stored in the newly created SQL database for every table in the gold container in our data lake using delta format.

     ![synapse4](https://github.com/Sztabers77/Azure_Data_Engineering_Project/assets/155321276/0125b219-3d4a-4a2d-8333-fb001dc9d53a)
     ![synapse5](https://github.com/Sztabers77/Azure_Data_Engineering_Project/assets/155321276/defe4b50-daaa-40cb-a20b-f0a3b3e83300)

  - Go to the Manage tab and create new linked service to connect with serverless DB (use Serverless SQL endpoint in Synapse properties to establish domain name):

    ![synapse7](https://github.com/Sztabers77/Azure_Data_Engineering_Project/assets/155321276/f8c155c2-a8e1-4cc2-8ef0-8ccfa7aed834)
    ![synapse9](https://github.com/Sztabers77/Azure_Data_Engineering_Project/assets/155321276/87453da4-faeb-4306-9e15-0202874770fd)
    ![synapse10](https://github.com/Sztabers77/Azure_Data_Engineering_Project/assets/155321276/830ab2cf-3f13-47b2-beb9-4476d78dd503)

  - Go to the Integrate tab and create a pipeline to create the views using stored procedure created before:

    ![synapse11](https://github.com/Sztabers77/Azure_Data_Engineering_Project/assets/155321276/1ec15ebf-9a01-4dd0-aca8-0f57f054d845)
    ![synapse12](https://github.com/Sztabers77/Azure_Data_Engineering_Project/assets/155321276/44a0f7cb-8021-45c0-9384-be57170de813)
    ![synapse13](https://github.com/Sztabers77/Azure_Data_Engineering_Project/assets/155321276/e29f0664-ad31-49e0-87ca-c8c4c5a1b125)
    ![synapse14](https://github.com/Sztabers77/Azure_Data_Engineering_Project/assets/155321276/50bd33ab-dec6-4c8c-8e06-c9377d5a0ebc)
    ![synapse15](https://github.com/Sztabers77/Azure_Data_Engineering_Project/assets/155321276/5a3bc613-4eea-465e-b95f-61f668aecbf2)
    ![synapse16](https://github.com/Sztabers77/Azure_Data_Engineering_Project/assets/155321276/bb1e7461-0d2c-4a1e-aeaf-b34a1bafbe4a)
    
# 7. Security:

   - In the Azure Portal go to Microsoft entra ID resource and create new security group so every new member of the group will automatically has access to all created resources inside our resource group:

     ![azureAD1](https://github.com/Sztabers77/Azure_Data_Engineering_Project/assets/155321276/48f58543-42ff-4e2f-b91e-e0019787d5a3)
     ![azureAD2](https://github.com/Sztabers77/Azure_Data_Engineering_Project/assets/155321276/4e4c7f4e-f2ef-459b-9296-48a18b296398)

# 8. Creating Power Bi report:

   - In the Power Bi Desktop go to Get Data -> More -> Azure Synapse Analytics SQL -> fill the Server name with the same Serverless SQL endpoint in Synapse properties as we use earlier and the DB name with our database created in the Synapse workspace. Next follow the steps at the screenshots:

     ![pbi1](https://github.com/Sztabers77/Azure_Data_Engineering_Project/assets/155321276/3c38f850-3bcc-4d0c-85fe-2488f8e69a07)
     ![pbi2](https://github.com/Sztabers77/Azure_Data_Engineering_Project/assets/155321276/ae395398-34da-4e09-bec8-04ea757a629a)
     ![pbi3](https://github.com/Sztabers77/Azure_Data_Engineering_Project/assets/155321276/9a42e40d-0aa2-4bf7-89c3-02a645d8cb7c)
     ![pbi4](https://github.com/Sztabers77/Azure_Data_Engineering_Project/assets/155321276/3d34f8c3-f896-45bf-a624-2f18b8f056e3)
     ![pbi5](https://github.com/Sztabers77/Azure_Data_Engineering_Project/assets/155321276/d7c6da80-4335-4783-a0b8-ebb24063470c)

- After the data load create your custom report using charts, DAX and M languages:

     ![pbi6](https://github.com/Sztabers77/Azure_Data_Engineering_Project/assets/155321276/373aa491-5fb8-43eb-b009-aab3cd77e504)


# 9. Testing:

   - 


     




     



































     



     


  



      
     



     

  
     





