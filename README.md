# s3presignedUrl()
FileMaker custom function to generate a presigned URL to upload/download/delete an object in S3

s3presignedUrl ( method ; bucket ; region ; file ; expireSeconds ; accessKey ; secretKey ; obtionsObj )

## PARAMETERS
* method | GET or PUT or DELETE
* bucket | the bucket name
* region | eg: ca-central-1
* theFilePath | the full path to the file in the bucket eg: /folder/filename.pdf
* expireSeconds | the length of time that the URL will be valid
* accessKey | AWS Access Key
* secretKey | AWS Secret Key
* optionsObj | empty or JSON object containing optional options

## OPTIONAL PARAMETERS
Optional parameters are passed as JSON inside the optionsObj parameter
* host | the provider's domain, with or without the bucket name. not required for AWS.
