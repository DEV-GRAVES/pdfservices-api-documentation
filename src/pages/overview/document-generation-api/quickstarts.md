# Quickstarts
Use Document Generation API to merge JSON data into Word based document
templates and produce high fidelity PDF and Word documents from any
application.


## Live Demo

The [Document Generation API Demo](https://documentcloud.adobe.com/dc-docgen-playground/index.html#/) demonstrates how easy it is to generate customized documents from Word-based document templates and input JSON data.

## How It Works

**Use MS Word Add-In to design document templates**

![image](../images/design_document_templates.gif)

**Prepare your JSON data**

```javascript
{
    "Client" : {
      "Name" : "Some Corp Inc",
      "Address" : "Somewhere Street"
    }
}
```

**Create Data Driven Word and PDF documents using Document Generation
API**

There are two ways to access Document Generation API:

-   Use cloud based [REST
    API](https://www.adobe.com/go/dcsdk_APIdocs#post-documentGeneration).
-   Or, directly use our offering through [PDFServices
    SDK](../pdf-services-api#sdk).

<InlineAlert slots="text"/>

To get started with PDFServices SDK, refer [Quickstarts Section](../pdf-services-api)


The samples below illustrate how to merge Word based document template
with the JSON data to generate the output document in the Word or PDF
format.

### Generate Word document

<CodeBlock slots="heading, code" repeat="4" languages="Java, .NET, Node JS, Rest API" /> 

#### Java

```javascript 
// Get the samples from https://www.adobe.com/go/pdftoolsapi_java_samples
// Run the sample:
// mvn -f pom.xml exec:java -Dexec.mainClass=com.adobe.pdfservices.operation.samples.documentmerge.MergeDocumentToDOCX
 
   package com.adobe.pdfservices.operation.samples.documentmerge;
 
   public class MergeDocumentToDOCX {
 
      // Initialize the logger.
      private static final Logger LOGGER = LoggerFactory.getLogger(MergeDocumentToDOCX.class);
 
      public static void main(String[] args) {
 
          try {
 
            // Initial setup, create credentials instance.
            Credentials credentials = Credentials.serviceAccountCredentialsBuilder()
                    .fromFile("pdfservices-api-credentials.json")
                    .build();
 
            // Setup input data for the document merge process.
            JSONObject jsonDataForMerge = new JSONObject("{\"customerName\": \"Kane Miller\",\"customerVisits\": 100}");
 
            // Create an ExecutionContext using credentials.
            ExecutionContext executionContext = ExecutionContext.create(credentials);
 
            // Create a new DocumentMergeOptions instance.
            DocumentMergeOptions documentMergeOptions = new DocumentMergeOptions(jsonDataForMerge, OutputFormat.DOCX);
 
            // Create a new DocumentMergeOperation instance with the DocumentMergeOptions instance.
            DocumentMergeOperation documentMergeOperation = DocumentMergeOperation.createNew(documentMergeOptions);
 
            // Set the operation input document template from a source file.
            FileRef documentTemplate = FileRef.createFromLocalFile("src/main/resources/documentMergeTemplate.docx");
            documentMergeOperation.setInput(documentTemplate);
 
            // Execute the operation.
            FileRef result = documentMergeOperation.execute(executionContext);
 
            // Save the result to the specified location.
            result.saveAs("output/documentMergeOutput.docx");
 
          } catch (ServiceApiException | IOException | SdkException | ServiceUsageException ex) {
              LOGGER.error("Exception encountered while executing operation", ex);
          }
      }
   }
     
```

#### .NET

```javascript
// Get the samples from https://www.adobe.com/go/pdftoolsapi_net_samples
// Run the sample:
// cd MergeDocumentToDocx/
// dotnet run MergeDocumentToDOCX.csproj

  namespace MergeDocumentToDOCX
   {
       class Program
       {
           private static readonly ILog log = LogManager.GetLogger(typeof(Program));
  
           static void Main()
           {
               //Configure the logging.
               ConfigureLogging();
               try
               {
                   // Initial setup, create credentials instance.
                   Credentials credentials = Credentials.ServiceAccountCredentialsBuilder()
                            .FromFile(Directory.GetCurrentDirectory() + "/pdfservices-api-credentials.json")
                            .Build();
  
                   // Create an ExecutionContext using credentials.
                   ExecutionContext executionContext = ExecutionContext.Create(credentials);
  
                   // Setup input data for the document merge process.
                   JObject jsonDataForMerge = JObject.Parse("{\"customerName\": \"Kane Miller\",\"customerVisits\": 100}");
  
                   // Create a new DocumentMerge Options instance.
                   DocumentMergeOptions documentMergeOptions = new DocumentMergeOptions(jsonDataForMerge, OutputFormat.DOCX);
  
                   // Create a new DocumentMerge Operation instance with the DocumentMerge Options instance.
                   DocumentMergeOperation documentMergeOperation = DocumentMergeOperation.CreateNew(documentMergeOptions);
  
                   // Set the operation input document template from a source file.
                   documentMergeOperation.SetInput(FileRef.CreateFromLocalFile(@"documentMergeTemplate.docx"));
  
                   // Execute the operation.
                   FileRef result = documentMergeOperation.Execute(executionContext);
  
                   // Save the result to the specified location.
                   result.SaveAs(Directory.GetCurrentDirectory() + "/output/DocumentMergeOutput.docx");
               }
               catch (ServiceUsageException ex)
               {
                   log.Error("Exception encountered while executing operation", ex);
               }
               catch (ServiceApiException ex)
               {
                   log.Error("Exception encountered while executing operation", ex);
               }
               catch (SDKException ex)
               {
                   log.Error("Exception encountered while executing operation", ex);
               }
               catch (IOException ex)
               {
                   log.Error("Exception encountered while executing operation", ex);
               }
               catch (Exception ex)
               {
                   log.Error("Exception encountered while executing operation", ex);
               }
           }
  
           static void ConfigureLogging()
           {
               ILoggerRepository logRepository = LogManager.GetRepository(Assembly.GetEntryAssembly());
               XmlConfigurator.Configure(logRepository, new FileInfo("log4net.config"));
           }
       }
   }
```

#### Node JS

```javascript
// Get the samples from http://www.adobe.com/go/pdftoolsapi_node_sample
// Run the sample:
// node src/documentmerge/merge-document-to-docx.js

 const PDFServicesSdk = require('@adobe/pdfservices-node-sdk');

 try {
   // Initial setup, create credentials instance.
   const credentials =  PDFServicesSdk.Credentials
       .serviceAccountCredentialsBuilder()
       .fromFile("pdfservices-api-credentials.json")
       .build();

   // Setup input data for the document merge process.
   const jsonString = "{\"customerName\": \"Kane Miller\", \"customerVisits\": 100}",
       jsonDataForMerge = JSON.parse(jsonString);

   // Create an ExecutionContext using credentials.
   const executionContext = PDFServicesSdk.ExecutionContext.create(credentials);

   // Create a new DocumentMerge options instance.
   const documentMerge = PDFServicesSdk.DocumentMerge,
       documentMergeOptions = documentMerge.options,
       options = new documentMergeOptions.DocumentMergeOptions(jsonDataForMerge, documentMergeOptions.OutputFormat.DOCX);

   // Create a new operation instance using the options instance.
   const documentMergeOperation = documentMerge.Operation.createNew(options);

   // Set operation input document template from a source file.
   const input = PDFServicesSdk.FileRef.createFromLocalFile('resources/documentMergeTemplate.docx');
   documentMergeOperation.setInput(input);

   // Execute the operation and Save the result to the specified location.
   documentMergeOperation.execute(executionContext)
       .then(result => result.saveAsFile('output/documentMergeOutput.docx'))
       .catch(err => {
           if(err instanceof PDFServicesSdk.Error.ServiceApiError
               || err instanceof PDFServicesSdk.Error.ServiceUsageError) {
               console.log('Exception encountered while executing operation', err);
           } else {
               console.log('Exception encountered while executing operation', err);
           }
       });
 }
 catch (err) {
     console.log('Exception encountered while executing operation', err);
 }
```

#### Rest API

```javascript
curl --location --request POST 'https://cpf-ue1.adobe.io/ops/:create?respondWith=%7B%22reltype%22%3A%20%22http%3A%2F%2Fns.adobe.com%2Frel%2Fprimary%22%7D' \
--header 'Authorization: Bearer {{Placeholder for token}}' \
--header 'Accept: application/json, text/plain, */*' \
--header 'x-api-key: {{Placeholder for client_id}}' \
--header 'Prefer: respond-async,wait=0' \
--form 'contentAnalyzerRequests="{
   \"cpf:engine\":{
      \"repo:assetId\":\"urn:aaid:cpf:Service-52d5db6097ed436ebb96f13a4c7bf8fb\"
   },
   \"cpf:inputs\":{
      \"documentIn\":{
         \"dc:format\":\"application/vnd.openxmlformats-officedocument.wordprocessingml.document\",
         \"cpf:location\":\"InputFile0\"
      },
      \"params\":{
         \"cpf:inline\":{
            \"outputFormat\": \"docx\",
            \"jsonDataForMerge\": {
              \"customerName\": \"Kane Miller\",
              \"customerVisits\": 100,
              \"itemsBought\": [
                {
                  \"name\": \"Sprays\",
                  \"quantity\": 50,
                  \"amount\": 100
                },
                {
                  \"name\": \"Chemicals\",
                  \"quantity\": 100,
                  \"amount\": 200
                }
              ],
              \"totalAmount\": 300,
              \"previousBalance\": 50,
              \"lastThreeBillings\": [100, 200, 300],
              \"photograph\": \"data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAQAAAC1HAwCAAAAC0lEQVR42mP88h8AAu0B9XNPCQQAAAAASUVORK5CYII=\"
            }
         }
      }
   },
   \"cpf:outputs\":{
      \"documentOut\":{
         \"dc:format\":\"application/vnd.openxmlformats-officedocument.wordprocessingml.document\",
         \"cpf:location\":\"multipartLabel\"
      }
   }
}"' \
--form 'InputFile0=@"{{Placeholder for the input document template (absolute path)}}"'
```

### Generate PDF document

<CodeBlock slots="heading, code" repeat="4" languages="Java, .NET, Node JS, Rest API" /> 

#### Java

```javascript 
// Get the samples from https://www.adobe.com/go/pdftoolsapi_java_samples
// Run the sample:
// mvn -f pom.xml exec:java -Dexec.mainClass=com.adobe.pdfservices.operation.samples.documentmerge.MergeDocumentToPDF
 
    public class MergeDocumentToPDF {
   
      // Initialize the logger.
      private static final Logger LOGGER = LoggerFactory.getLogger(MergeDocumentToPDF.class);
   
      public static void main(String[] args) {
   
            try {
   
              // Initial setup, create credentials instance.
              Credentials credentials = Credentials.serviceAccountCredentialsBuilder()
                      .fromFile("pdfservices-api-credentials.json")
                      .build();
   
              // Setup input data for the document merge process.
              String content = new String(Files.readAllBytes(Paths.get("src/main/resources/salesOrder.json")));
              JSONObject jsonDataForMerge = new JSONObject(content);
   
              // Create an ExecutionContext using credentials.
              ExecutionContext executionContext = ExecutionContext.create(credentials);
   
              //Create a new DocumentMergeOptions instance.
              DocumentMergeOptions documentMergeOptions = new DocumentMergeOptions(jsonDataForMerge, OutputFormat.PDF);
   
              // Create a new DocumentMergeOperation instance with the DocumentMergeOptions instance.
              DocumentMergeOperation documentMergeOperation = DocumentMergeOperation.createNew(documentMergeOptions);
   
              // Set the operation input document template from a source file.
              FileRef documentTemplate = FileRef.createFromLocalFile("src/main/resources/salesOrderTemplate.docx");
              documentMergeOperation.setInput(documentTemplate);
   
              // Execute the operation.
              FileRef result = documentMergeOperation.execute(executionContext);
   
              // Save the result to the specified location.
              result.saveAs("output/salesOrderOutput.pdf");
   
            } catch (ServiceApiException | IOException | SdkException | ServiceUsageException ex) {
                LOGGER.error("Exception encountered while executing operation", ex);
            }
        }
     }
     
```

#### .NET

```javascript
// Get the samples from https://www.adobe.com/go/pdftoolsapi_net_samples
// Run the sample:
// cd MergeDocumentToPDF/
// dotnet run MergeDocumentToPDF.csproj

  namespace MergeDocumentToPDF
   {
       class Program
       {
           private static readonly ILog log = LogManager.GetLogger(typeof(Program));
  
           static void Main()
           {
               //Configure the logging.
               ConfigureLogging();
               try
               {
                   // Initial setup, create credentials instance.
                   Credentials credentials = Credentials.ServiceAccountCredentialsBuilder()
                                 .FromFile(Directory.GetCurrentDirectory() + "/pdfservices-api-credentials.json")
                                 .Build();
  
                   // Create an ExecutionContext using credentials.
                   ExecutionContext executionContext = ExecutionContext.Create(credentials);
  
                   // Setup input data for the document merge process.
                   var content = File.ReadAllText(@"salesOrder.json");
                   JObject jsonDataForMerge = JObject.Parse(content);
  
                   // Create a new DocumentMerge Options instance.
                   DocumentMergeOptions documentMergeOptions = new DocumentMergeOptions(jsonDataForMerge, OutputFormat.PDF);
  
                   // Create a new DocumentMerge Operation instance with the DocumentMerge Options instance.
                   DocumentMergeOperation documentMergeOperation = DocumentMergeOperation.CreateNew(documentMergeOptions);
  
                   // Set the operation input document template from a source file.
                   documentMergeOperation.SetInput(FileRef.CreateFromLocalFile(@"salesOrderTemplate.docx"));
  
                   // Execute the operation.
                   FileRef result = documentMergeOperation.Execute(executionContext);
  
                   // Save the result to the specified location.
                   result.SaveAs(Directory.GetCurrentDirectory() + "/output/salesOrderOutput.pdf");
               }
               catch (ServiceUsageException ex)
               {
                   log.Error("Exception encountered while executing operation", ex);
               }
               catch (ServiceApiException ex)
               {
                   log.Error("Exception encountered while executing operation", ex);
               }
               catch (SDKException ex)
               {
                   log.Error("Exception encountered while executing operation", ex);
               }
               catch (IOException ex)
               {
                   log.Error("Exception encountered while executing operation", ex);
               }
               catch (Exception ex)
               {
                   log.Error("Exception encountered while executing operation", ex);
               }
           }
  
           static void ConfigureLogging()
           {
               ILoggerRepository logRepository = LogManager.GetRepository(Assembly.GetEntryAssembly());
               XmlConfigurator.Configure(logRepository, new FileInfo("log4net.config"));
           }
       }
   }
```

#### Node JS

```javascript
// Get the samples from http://www.adobe.com/go/pdftoolsapi_node_sample
// Run the sample:
// node src/documentmerge/merge-document-to-pdf.js

  const PDFServicesSdk = require('@adobe/pdfservices-node-sdk'),
      fs = require('fs');
 
  try {
    // Initial setup, create credentials instance.
    const credentials =  PDFServicesSdk.Credentials
        .serviceAccountCredentialsBuilder()
        .fromFile("pdfservices-api-credentials.json")
        .build();
 
    // Setup input data for the document merge process.
    const jsonString = fs.readFileSync('resources/salesOrder.json'),
        jsonDataForMerge = JSON.parse(jsonString);
 
    // Create an ExecutionContext using credentials.
    const executionContext = PDFServicesSdk.ExecutionContext.create(credentials);
 
    // Create a new DocumentMerge options instance.
    const documentMerge = PDFServicesSdk.DocumentMerge,
        documentMergeOptions = documentMerge.options,
        options = new documentMergeOptions.DocumentMergeOptions(jsonDataForMerge, documentMergeOptions.OutputFormat.PDF);
 
    // Create a new operation instance using the options instance.
    const documentMergeOperation = documentMerge.Operation.createNew(options)
 
    // Set operation input document template from a source file.
    const input = PDFServicesSdk.FileRef.createFromLocalFile('resources/salesOrderTemplate.docx');
    documentMergeOperation.setInput(input);
 
    // Execute the operation and Save the result to the specified location.
    documentMergeOperation.execute(executionContext)
        .then(result => result.saveAsFile('output/salesOrderOutput.pdf'))
        .catch(err => {
            if(err instanceof PDFServicesSdk.Error.ServiceApiError
                || err instanceof PDFServicesSdk.Error.ServiceUsageError) {
                console.log('Exception encountered while executing operation', err);
            } else {
                console.log('Exception encountered while executing operation', err);
            }
        });
  } catch (err) {
      console.log('Exception encountered while executing operation', err);
  }
  
```

#### Rest API

```javascript
curl --location --request POST 'https://cpf-ue1.adobe.io/ops/:create?respondWith=%7B%22reltype%22%3A%20%22http%3A%2F%2Fns.adobe.com%2Frel%2Fprimary%22%7D' \
--header 'Authorization: Bearer {{Placeholder for token}}' \
--header 'Accept: application/json, text/plain, */*' \
--header 'x-api-key: {{Placeholder for client_id}}' \
--header 'Prefer: respond-async,wait=0' \
--form 'contentAnalyzerRequests="{
   \"cpf:engine\":{
      \"repo:assetId\":\"urn:aaid:cpf:Service-52d5db6097ed436ebb96f13a4c7bf8fb\"
   },
   \"cpf:inputs\":{
      \"documentIn\":{
         \"dc:format\":\"application/vnd.openxmlformats-officedocument.wordprocessingml.document\",
         \"cpf:location\":\"InputFile0\"
      },
      \"params\":{
         \"cpf:inline\":{
            \"outputFormat\": \"pdf\",
            \"jsonDataForMerge\": {
              \"customerName\": \"Kane Miller\",
              \"customerVisits\": 100,
              \"itemsBought\": [
                {
                  \"name\": \"Sprays\",
                  \"quantity\": 50,
                  \"amount\": 100
                },
                {
                  \"name\": \"Chemicals\",
                  \"quantity\": 100,
                  \"amount\": 200
                }
              ],
              \"totalAmount\": 300,
              \"previousBalance\": 50,
              \"lastThreeBillings\": [100, 200, 300],
              \"photograph\": \"data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAQAAAC1HAwCAAAAC0lEQVR42mP88h8AAu0B9XNPCQQAAAAASUVORK5CYII=\"
            }
         }
      }
   },
   \"cpf:outputs\":{
      \"documentOut\":{
         \"dc:format\":\"application/pdf\",
         \"cpf:location\":\"multipartLabel\"
      }
   }
}"' \
--form 'InputFile0=@"{{Placeholder for the input document template (absolute path)}}"'
```

As a result of the Document Generation API, template tags are replaced
with the input JSON data.

![image](../images/generate_document.gif)

## API Limitations

The Document Generation API has the following limitations:

-   **Document template size limit**: Maximum supported document
    template file size is 100MB.
-   **Input JSON size limit**: Maximum supported input JSON size is
    10MB.
