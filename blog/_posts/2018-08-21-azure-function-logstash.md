---
layout: post
title: Azure Function is listening for blob changes and sending data to Logstash
description: >
  Azure function and Logstash
image: 
---


## Azure Function 
1. Lets create Azure function that will monitor blob changes and send data to Logstash.
Search fot Function app from your Azure portal.
Attantion: if you want to include your Azure Function later to the same Vnet  as your Logstash VM, you need to choose App Service plan, not Consumption plan for Azure function and chose the same region (for example, East US).
2. Choose EventGrid template.
3. Click on Add Event Grid tab on top
4. Add storage acccount that you are going to use to monitor changes (no metter how many containers you will create inside)
Attention: your blob storage type should be v2 or just blob. In other cases you wont be able to see and event tab. it meeans that you can use Event Grid with other types of blob Storages.
5. Paste the following code and click 'Save' after

```c#
#r "Microsoft.Azure.WebJobs.Extensions.EventGrid"
#r "Microsoft.WindowsAzure.Storage"
#r "Newtonsoft.Json"
#r "System.IO"

using Microsoft.Azure.WebJobs.Extensions.EventGrid;
using Microsoft.Azure.WebJobs.Host.Bindings.Runtime;
using Microsoft.WindowsAzure.Storage; 
using Microsoft.WindowsAzure.Storage.Blob;
using System;
using System.Text;
using System.Net.Http;
using System.Net.Http.Headers;

static string storageAccountConnectionString = System.Environment.GetEnvironmentVariable("blobforfunctionv2_STORAGE");
static string logstashHttpInputUrl = System.Environment.GetEnvironmentVariable("LOGSTASH_HTTP_INPUT_URL");
static string logstashHttpUser = System.Environment.GetEnvironmentVariable("LOGSTASH_HTTP_USER");
static string logstashHttpPassword = System.Environment.GetEnvironmentVariable("LOGSTASH_HTTP_PASSWORD");

static HttpClient client = new HttpClient() {
    DefaultRequestHeaders = { 
        Authorization = new AuthenticationHeaderValue("Basic", Convert.ToBase64String(Encoding.UTF8.GetBytes($"{logstashHttpUser}:{logstashHttpPassword}")))
    }
};

public static async Task Run(EventGridEvent eventGridEvent, TraceWriter log)
{
    log.Info(eventGridEvent.ToString());
    var blobUrl = (string)eventGridEvent.Data["url"];
    // Get the cloudBlob from the event's JObject.
    CloudBlob cloudBlob = GetCloudBlobFromUrl(blobUrl);

    // Retrieve storage account from connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(storageAccountConnectionString);

    // Create the blob client.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    // Retrieve reference to a previously created container.
    CloudBlobContainer container = blobClient.GetContainerReference(cloudBlob.Container.Name);

    // Create reference to a blob named "blobName".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference(cloudBlob.Name);

    var myStream = await blockBlob.OpenReadAsync();
    var streamContent = new StreamContent(myStream);
     
    streamContent.Headers.ContentType = new MediaTypeHeaderValue("application/octet-stream");
    streamContent.Headers.ContentLength = myStream.Length;
    streamContent.Headers.Add("BlobName", cloudBlob.Name);
    streamContent.Headers.Add("BlobUrl", blobUrl);

    HttpResponseMessage response = await client.PostAsync(logstashHttpInputUrl, streamContent);
    response.EnsureSuccessStatusCode();
    
}
private static CloudBlob GetCloudBlobFromUrl(string bloblUrl)
{
    var myUri = new Uri(bloblUrl);
    var myCloudBlob = new CloudBlob(myUri);
    return myCloudBlob;
}



```
6. Create project.json file and add this lines and click 'Save':
```json
{
  "frameworks": {
    "net46":{
      "dependencies": {
        "Microsoft.AspNet.WebApi.Client": "5.2.6",
        "Microsoft.Azure.WebJobs.Extensions.EventGrid": "1.0.0"
      }
    }
   }
}
```


7. Add your enviroment variable to Application Settings for your Azure Function:
blobforfunctionv2_STORAGE (your Azure Blob Storage connection string)
LOGSTASH_HTTP_INPUT_URL (http://your_public_ip:8080)
LOGSTASH_HTTP_USER (username that you created in your logstash pipeline)
LOGSTASH_HTTP_PASSWORD (password that your created in your logstash pipeline)





