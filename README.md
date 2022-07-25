# Azure Exporter for Scrapy
[Scrapy feed export storage backend](https://doc.scrapy.org/en/latest/topics/feed-exports.html#storage-backends) for [Azure Storage](https://docs.microsoft.com/en-us/azure/storage/).

## Installation
```bash
pip install git+https://github.com/scrapy-plugins/scrapy-feedexporter-azure-storage
```
## Usage
* Add this storage backend to the [FEED_STORAGES](https://docs.scrapy.org/en/latest/topics/feed-exports.html#std-setting-FEED_STORAGES) Scrapy setting. For example:
```python
# settings.py
FEED_STORAGES = {'azure': 'scrapy_azure_exporter.AzureFeedStorage'}
```
* Provide [Authentication](https://docs.microsoft.com/en-us/python/api/overview/azure/storage-blob-readme?view=azure-python) for Azure account via any of the following settings.
  - `AZURE_CONNECTION_STRING` 
  - `AZURE_ACCOUNT_URL_WITH_SAS_TOKEN`
  - `AZURE_ACCOUNT_URL` & `AZURE_ACCOUNT_KEY` - If using this method, specify both of them.
```python 
For example,
AZURE_ACCOUNT_URL = "https://<your-storage-account-name>.blob.core.windows.net/"
AZURE_ACCOUNT_KEY = "Account key for the Azure account"
```
* Add Azure URI of the location (the location where the feed needs to be exported) to the FEEDS dictionary. Reference: [FEEDS](https://docs.scrapy.org/en/latest/topics/feed-exports.html#feeds)

```python
FEEDS = {
    "azure://<account_name>.blob.core.windows.net/<container_name>/<file_name.extension>": {
        "format": "json"
    }
}
```
## Supported feed options
 - The below feed options are supported. See their usage and details [here](https://docs.scrapy.org/en/latest/topics/feed-exports.html#feeds).
   - format
   - encoding
   - fields
   - item_classes
   - item_filter
   - indent
   - item_export_kwargs
   - overwrite
   - store_empty
   - uri_params
   - batch_item_count
 - The following ones are specific to this storage backend.
   - `blob_type` - The Azure Blob Types. This can be `"AppendBlob"`, `"BlockBlob"`, or `"PageBlob'`. Default is `"BlockBlob"`. ([Ref](https://docs.microsoft.com/en-us/azure/storage/blobs/storage-blob-pageblob-overview?tabs=dotnet))
   - `overwrite` - Whether to overwrite the file if it already exists (True) or append to its content (False).
     - It's worth mentioning that for appending to the file, the conditions: `overwrite=False` and `blob_type="AppendBlob"` should be met. Because files can be appended only when using `AppendBlob` For more info, see ([Understanding blob types](https://docs.microsoft.com/en-us/rest/api/storageservices/understanding-block-blobs--append-blobs--and-page-blobs))
 - The `postprocessing` feed option is currently unsupported.