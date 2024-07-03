# Function Secure 101 Sample

This sample shows the simple case of a blob trigger that has been modified to use managed identity in the cloud, and also emulators plus your Entra Id identity on the local developer machine.  The steps to build this are documented here: [https://aka.ms/functions-secure101].

## Running locally using only emulators (no internet / Azure connection needed)

1. Add `local.settings.json` to the root of this folder and paste in these contents. 

```json
{
  "IsEncrypted": false,
  "Values": {
    "FUNCTIONS_WORKER_RUNTIME": "dotnet-isolated",
    "AzureWebJobsStorage": "UseDevelopmentStorage=true",
    "StorageConnection": "UseDevelopmentStorage=true"
  }
}
```

2. In a new terminal, start azurite (this example uses docker, but VS Code Azurite extension is ok too)

```bash
docker run -p 10000:10000 -p 10001:10001 -p 10002:10002 mcr.microsoft.com/azure-storage/azurite
```

3.  Start and run the function with either `F5` in Visual Studio or the following command in a new terminal window:

```bash
func start
```

You should see a successful start.  You can now use Azure Storage Explorer or another tool to upload a file to the `samples-workitems` Blob container.

```text
[2024-07-03T16:50:49.896Z] Worker process started and initialized.

Functions:

        BlobTrigger1: blobTrigger
```

## Running locally using Entra ID identity and remote Azure resources

1. Add or change the `local.settings.json` to the root of this folder and paste in these contents.  You will need to substitute values with your own resource values for storage and user assigned managed identity (the clientid).

```json
{
  "IsEncrypted": false,
  "Values": {
    "FUNCTIONS_WORKER_RUNTIME": "dotnet-isolated",
    "AzureWebJobsStorage__blobServiceUri": "https://strpaulyukstorage.blob.core.windows.net/",
    "AzureWebJobsStorage__queueServiceUri": "https://strpaulyukstorage.queue.core.windows.net/",
    "AzureWebJobsStorage__credential": "managedidentity",
    "AzureWebJobsStorage__clientId": "10f91864-9cc2-4d96-9a26-33c1eee284ac",
    "StorageConnection__blobServiceUri": "https://strpaulyukstorage.blob.core.windows.net/",
    "StorageConnection__queueServiceUri": "https://strpaulyukstorage.queue.core.windows.net/",
    "StorageConnection__credential": "managedidentity",
    "StorageConnection_clientId": "10f91864-9cc2-4d96-9a26-33c1eee284ac"
  }
}
```
2.  Start and run the function with either `F5` in Visual Studio or the following command in a new terminal window:

```bash
func start
```

You should see the functions start and list with no 403 errors.  You can now use Azure Storage Explorer or another tool to upload a file to the `samples-workitems` Blob container.

```text
[2024-07-03T16:50:49.896Z] Worker process started and initialized.

Functions:

        BlobTrigger1: blobTrigger
```
