# Purpose
This is a repository for docker image of processing files with ABBYY Cloud OCR, using Python 3.7.

# Application Code

It is a Python code that was gotten from [Quick start with OCR SDK for Python](https://www.ocrsdk.com/documentation/quick-start-guide/python-ocr-sdk).

This is not an asynchronous code, so it hangs a little bit waiting a return from the **ABBY Cloud**.

***IMPORTANT:*** The are a bunch of API methods available but this code takes care only of [processImage](https://www.ocrsdk.com/documentation/api-reference/process-image-method) method.

# Before Starting

First of all you need an account at [**ABBY Cloud OCR SDK**](https://cloud.ocrsdk.com/Account/Welcome) and also an application configured for your submission.<br/>
After creating your account, create an application and save its name. A password will be sent to you through mail.

# Building the Image
After cloning this repository, being on the main folder, run:<br/>
`docker build -t abbyy-process-image .`

# Running the Image

## Environment Variables

On your environment, code below is for Linux, export three important variables as follows:
*  **ABBYY_SERVER_URL** - URL of the ABBYY OCR Server where you have registered you account.
*  **ABBYY_APPID** - The application ID that you registered.
*  **ABBYY_PWD** - The application password that was sent to you by **ABBYY**.

```bash
export ABBYY_SERVER_URL="https://cloud-westus.ocrsdk.com"
export ABBYY_APPID="<YOUR_APPLICATION_ID>"
export ABBYY_PWD="<YOUR_APPLICATION_PASSWORD>"
```

## Python Code Arguments

The python code that is wrapped into this container needs the following mandatory arguments (<font style='color:red'>*They have to be sent in this order*</font>):<br/>
| Argument | Description | 
|---|---|
| source_file | The source file that will be uploaded to the cloud and will be processed by OCR tool. |
| target_file | The target file that will be processed into the cloud and will be downloaded to your workstation. |

The target file should be the output file name only since the code writes the file inside the container in the directory **/usr/app/output** that is mapped to your host by the **docker run** command bellow.

Also there are optional arguments that can be sent with shortcut letters, as follows:
| Optional Argument | Description | 
|---|---|
| -l<br/>or<br/>--language | Specifies the recognition language. *Default: English*<br/>[Supported languages](https://www.ocrsdk.com/documentation/specifications/recognition-languages) |

The following arguments represent a mutual exclusive group and just one of them can be used, and in fact, must be used for processing the OCR request.
| Optional Argument | Description | 
|---|---|
| -txt<br/><br/>*This is DEFAULT.*| The recognized text is exported to the file line by line from left to right. E.g. if the text was originally put in columns, the first lines of every column will be saved, then the second lines, etc.<br/>Please take into account the fact that in this format only text will be saved. No images or barcodes will remain in the output file. If you want to save the barcode recognition results in the exported file, use the txtUnstructured format. |
| -txtUnstructured | The exported file contains the text that was saved according to the order of the original blocks. |
| -rtf | Exports the result into **RTF** format. |
| -docx | Exports the result into **DOCX** format. |
| -xlsx | Exports the result into **XLSX** format. |
| -pptx | Exports the result into **PPTX** format. |
| -pdfSearchable | The entire image is saved as a picture, the recognized text is put under it. |
| -pdfa | The file is saved in the **PDF/A-1b** format, with the entire image saved as a picture, and recognized text put under it. |
| -pdfTextAndImages | The recognized text is saved as text, and the pictures are saved as pictures. |
| -xml | Exports the result into **XML** format. |
| <nobr>-xmlForCorrectedImage</nobr> | The same as xml, but all coordinates written into the output **XML** file relate to the corrected image, not the original. |
| -auto | Automatically choosen by platform. |

## Calling example output as XML

You must have set your environment variables before starting the container.

The following example, code below is for Linux, submits a PDF and request its return to be a XML called `result.xml`.

Run the following:
```bash
docker run ricardosouzamorais/abbyy-process-image
```