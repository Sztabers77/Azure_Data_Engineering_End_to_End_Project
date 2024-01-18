# Azure_Data_Engineering_Project

1. Architecture of the project:

     ![Screenshot_24](https://github.com/Sztabers77/Azure_Data_Engineering_End_to_End_Project/assets/155321276/91a5a7c3-dec5-42ce-85ef-7ce532bfd245)


2. Environment setup:
   - Create resource group and all needed resources within: Data Factory, Databricks, Key Vault, Data Lake Gen 2, Synapse Analytics (https://learn.microsoft.com/en-us/azure/developer/intro/azure-developer-create-resources):

     ![Screenshot_5](https://github.com/Sztabers77/Azure_Data_Engineering_End_to_End_Project/assets/155321276/0f192f32-c6ca-4a5b-b1b1-e8dfcf4c1aae)

   - Download SQL Server and create one with SSMS feature, next download database backup file 'AdventureWorksLT2017.bak' from https://learn.microsoft.com/en-us/sql/samples/adventureworks-install-configure?view=sql-server-ver16&tabs=ssms and attach this file to the SQL server creating a database:
  
     ![Screenshot_6](https://github.com/Sztabers77/Azure_Data_Engineering_End_to_End_Project/assets/155321276/38549189-857c-4267-b8fb-a0646dd65fb9)
     ![Screenshot_7](https://github.com/Sztabers77/Azure_Data_Engineering_End_to_End_Project/assets/155321276/c4956e17-339b-40ce-8d61-b1534bb8baf8)

   - Create new user and login in SSMS (assign db_datareader role by right-clicking at your user in the Security -> Users tab) and genereate 'username' and 'password' secrets in the Key Vault with the same values as in your DB:

     ![Screenshot_8](https://github.com/Sztabers77/Azure_Data_Engineering_End_to_End_Project/assets/155321276/4b3a5005-138b-498c-a933-48b44340fd4e)
     ![Screenshot_9](https://github.com/Sztabers77/Azure_Data_Engineering_End_to_End_Project/assets/155321276/89ad67e6-1813-4120-9172-910ae487449e)

3. Prerequistes to create the pipeline:
   - Launch Data Factory studio and following this steps - Manage tab -> Integration Runtime -> New - install Self-Hosted Integration Runtime feature with which we can connect with our on-prem SQL server installed in the previous step:
  
     ![Screenshot_10](https://github.com/Sztabers77/Azure_Data_Engineering_End_to_End_Project/assets/155321276/51095d3d-d4b4-41f7-955e-b6133e4ebdb2)
     ![Screenshot_11](https://github.com/Sztabers77/Azure_Data_Engineering_End_to_End_Project/assets/155321276/f5ab15a2-630f-49db-92bc-718d4e1fdf60)
     ![Screenshot_12](https://github.com/Sztabers77/Azure_Data_Engineering_End_to_End_Project/assets/155321276/9726c63a-96dc-4bbe-9ee9-62dda3fa2532)
     ![Screenshot_13](https://github.com/Sztabers77/Azure_Data_Engineering_End_to_End_Project/assets/155321276/2d4e2cdd-d706-4f8c-a54f-d4d9f835e9b0)
     ![selfhostedinstallfinished](https://github.com/Sztabers77/Azure_Data_Engineering_End_to_End_Project/assets/155321276/9e60328e-276d-4944-8675-8c8415467fa9)


   - In the same Manage tab click on 'Linked Service' and create all Linked Service needed in our pipeline - for Databricks Linked Service we'll need a token generated with this instructions - https://docs.databricks.com/en/dev-tools/auth/pat.html#:~:text=In%20your%20Databricks%20workspace%2C%20click,Click%20Generate%20new%20token. and stored in the Key Vault as you could see in the previous screenshot (dbwtoken):
  
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

   4. Transform the data in the Databricks notebooks:

      - ![Screenshot_25](https://github.com/Sztabers77/Azure_Data_Engineering_End_to_End_Project/assets/155321276/7a178fdb-d681-42d8-b355-102b48e3800b)
      - 













     



     


  



      
     



     

  
     





