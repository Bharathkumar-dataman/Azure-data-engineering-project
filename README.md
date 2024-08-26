# Azure-data-engineering-project
   ## Project overview: The goal of this project is to perform data validation on AP Morgan incoming  data from internal applications using various tools and technologies 
## Tech stack :
 ##  *Azure blob storaage 
 ##  *Azure sql database
 ##  *Azure data factory
 ##  *Azure databricks
 ##  *Azure keyvault 
               

  <img width="394" alt="Project2_Archiecture (1)" src="https://github.com/user-attachments/assets/0b1e3c36-98cb-4bbf-be28-b01749643fd8">
  
# Project implementation   
  ## * store the incoming data (csv file from internal applications) in azure data lake storage as landing data 
  ## * create SQLdatabase in azure to store data which stores validation passed data 
  ## * connect to sqldb with ssms and create a metadata/schema you can use my sql code as a example : https://github.com/Bharathkumar-dataman/Azure-data-engineering-project/blob/main/SQL%2BTable.sql
  
   ## * launch a azure databricks work on bussiness logic implementation 

# important best pratices to follow 
   ## store sql sever password, SAS token  in azure keyvault 
   ## create secretscope in databricks backed by azure keyvault( refer databricks documentation)
  
  
  
  ##    * create keyvault to store secrets (best pratices)
  ##    * create azure data factory and data pipeline and trigger when data comes to storage account and data moves to azure databricks to check validation such that 

  ## i have provided data validation logic code here : https://github.com/Bharathkumar-dataman/Azure-data-engineering-project/blob/main/validation.ipynb
  
  ##   * duplicate rows, date format ,data column names , desired date format is stored in a azure Sql server 
  ##   * if validation fails file will be rejected and move to reject folder 
  ##   * move all the passed files to staging folder 
  ##   * write the passed files as the delta table in the azure databricks 

# Create Azure databricks Linked service in ADF
 ## connect databricks with ADF using linkedin service ( AS i said  earlier follow best pratices by storing SAS tokens in keyvault secrets )( assuming you know how to connect otherwise check in databricks documentation)

# Create ADF pipeline to call notebook and test end to end flow 
 ## create a notebook in ADF with parameters such that  "file/name" , and ends with " .csv " (we want only csv files )
 ## Add a storage event trigger ( if incoming file comes landing with parameters like file ends with .csv then autimatically adf pipelines triggers to databrickes notebook where it checks data validation and schema from azure sql , if rejected it automatically lands on rejeted folder , if validation passed it will  pass on to create delta tables in databricks )
 ## you can check for code for validation :  https://github.com/Bharathkumar-dataman/Azure-data-engineering-project/blob/main/linked%20in%20service%20SQL.sql

 ## important note :Enable even grid registeration  subscription  


  # challenges during project implementation 
   ##  carefull when assigning RBAC access to keyvault , databricks want to fetch data from sql server to validate the data so store secrets (passwords , username ) in  keyvault as a best pratice and also allow access policies for databrick to fetech data .
   
# important note: we can even use azure fuctions or apps to send a notification  when files landed on our storage  account .



   
