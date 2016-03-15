
Simple PyDrive Extension. (Python)
====

This package provides a simple extension of [PyDrive](http://pythonhosted.org/PyDrive/) that wraps the Google Drive API tasks as like a common os module.

## Installation

*Note*: This program was only tested on **Windows** with **Python2.7**.
**Linux** and **Mac OS** are not officially supported,
but the following instructions might be helpful for installing on those environments.

### Dependencies
Please install the following required python modules.

* [**google-api-python-client**](https://code.google.com/archive/p/google-api-python-client/)
* [**PyDrive**](http://pythonhosted.org/PyDrive/)

You can easily install these packages with the following commands.

``` bash
  > pip install --upgrade google-api-python-client
  > pip install PyDrive
```

## Quick Start

PyDriveEx can make the GoogleDrive instance in just one line with the default PyDrive authentication setting.

``` python
from pydrive_ex.drive import GoogleDrive

gdrive = GoogleDrive()          # Create Google Drive instance with default setting.
gfile_list = gdrive.listdir()   # Get the file list of the root directory.
print gfile_list                # Print the file list.
```

The output will be:

``` bash
1 directories, 2 files.
  <Dir>  Dir1
  <Dir>  Dir2
  <File> Text.txt
  <File> image.png
```

To make this code work, you need to prepare the your Google Drive setting to run PyDrive.

1. Go to APIs Console and make your own project.
2. On **Services** menu, turn Drive API on.
3. On **API Access** menu, create OAuth2.0 client ID.
  * Select *Application type* to be Web application.
4. Download client_secrets.json and put it in your working directory.

I recommend you to create **settings.yaml** to save credentials file.
Authentication process will be skipped from the second time.

``` yaml
client_config_backend: file
client_config_file: client_secrets.json

save_credentials: True
save_credentials_backend: file
save_credentials_file: credentials.json
```

## File Management

### Upload a new file

You can easily upload a new file via GoogleDriveFile instance or
GoogleDrive.uploadFile function.

``` python
from pydrive_ex.drive import GoogleDrive

gdrive = GoogleDrive()          # Create Google Drive instance with default setting.
txt_file = gdrive.createFile("Hello.txt")   # Create Google Drive File instance with 'Hello.txt'
txt_file.setContentString("Hello World!\n") # Set the content string of the file.
txt_file.upload()                           # Upload the text file.

image_file = gdrive.createFile("gimages/TestImage.png")       # Create Google Drive File instance with 'TestImage.png'
image_file.setContentFile("images/TestImage.png") # Specify the local file.
image_file.upload()                               # Upload the text file.

# For more convenient, GoogleDrive object has uploadFile function.
gdrive.uploadFile("gimages/TestImage.png", "images/TestImage.png")
```

GoogleDrive.uploadFile will automatically create the Google Drive file
if the file does not exist.
If the file exists, this method just update the Google Drive file.
Most of the case, GoogleDrive.uploadFile is convenient for uploading the file.

You can specify the directory structure via "/" like a usual local file path.

### Delete a file

Since the original PyDrive does not support a delete function,
I extend the delete functionality in PyDriveEx.

``` python
from pydrive_ex.drive import GoogleDrive

gdrive = GoogleDrive()          # Create Google Drive instance with default setting.

gdrive.deleteFile("Hello.txt")  # Delete Google Drive File with 'Hello.txt'
gdrive.deleteFile("gimages")    # You can specify Google Drive Directory with 'gimages'
```

FileDeleteError will be raised if the Google Drive File cannot be deleted.

## License

The MIT License 2016 (c) tody