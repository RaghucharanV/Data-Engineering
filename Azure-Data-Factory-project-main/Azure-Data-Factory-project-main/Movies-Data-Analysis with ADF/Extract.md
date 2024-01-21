# Movies Analytics using Azure Data Factory
 Azure Data Factory's visual authoring experience to create a pipeline that copies movie data stored in Azure Blob Stoage to a separate location in that storage account and then executes a Mapping Data Flow to transform and write the data to Azure data Lake Gen2.

 The patterns used in this lab are examples of a modern data warehouse ingestion and transformation scenario using Azure Data Factory.


 # Setting up your environment

Create your data factory: Use the Azure Portal to create your Data Factory. Detailed instructions can be found at Create a Data Factory.

1.Go to resources and click on movies-data resource group click on create new button type Azure Data Factory. Create ADF with default requirements.



![Screenshot (779)](https://github.com/RaghucharanV/Azure-Data-Factory-project/assets/81848656/16d557dd-428b-4300-99e8-128a2df1013f)

![Screenshot (781)](https://github.com/RaghucharanV/Azure-Data-Factory-project/assets/81848656/de5bbcd4-069f-44ec-9cb5-ecf8f93d3325)

![Screenshot (782)](https://github.com/RaghucharanV/Azure-Data-Factory-project/assets/81848656/043c289d-d408-4eea-a3aa-0ba4026b0224)


2. Launch Studio
   ![Screenshot (783)](https://github.com/RaghucharanV/Azure-Data-Factory-project/assets/81848656/e466784b-7777-4fdc-87ff-6a761213dac6)


3. Create a Pipeline by clicking on the left sidebar monitor,

![Screenshot (784)](https://github.com/RaghucharanV/Azure-Data-Factory-project/assets/81848656/73a044ee-f317-486f-906b-f76f4f4ef08a)



4. Now the method to create Copy data activity in pipeline, is first build linked services http, and Blob storage.
on manage tab left side click on Linked Services next click on New search for Https. The fill the columns on your requirement
here base URL is github.com, Authentication type is Anonymous. Then Click on create.



![Screenshot (785)](https://github.com/RaghucharanV/Azure-Data-Factory-project/assets/81848656/85cf05e8-dd10-439f-9ad5-c2f4fb5115e3)


5. Create a Dataset base on the http req.

![Screenshot (786)](https://github.com/RaghucharanV/Azure-Data-Factory-project/assets/81848656/28efdcc1-278a-4168-ac65-dda74d1ba95f)

6.
![Screenshot (788)](https://github.com/RaghucharanV/Azure-Data-Factory-project/assets/81848656/47bd47ee-c75d-4720-9a99-e514966f87b6)

7. Same as above create a Blob storge linked Service and Dataset.
![Screenshot (787)](https://github.com/RaghucharanV/Azure-Data-Factory-project/assets/81848656/090f4095-d048-4258-be8c-808f6f8e730d)


  8. Perform Copy DataActivity, from HTTPS to Blob Storage copy csv file, by click on DEBUG activity it perform action.
     

![Screenshot (790)](https://github.com/RaghucharanV/Azure-Data-Factory-project/assets/81848656/f7ff281c-a179-43c1-9445-d02f2faa15c1)


9.You can check it in blob storge.....
![Screenshot (792)](https://github.com/RaghucharanV/Azure-Data-Factory-project/assets/81848656/2cba2bd9-6e8d-43c1-aad4-5c0cc704501f)
