# s3presignedUrl()
FileMaker custom function to generate a presigned URL to upload/download/delete an object in S3

s3presignedUrl ( method ; bucket ; region ; file ; expireSeconds ; accessKey ; secretKey ; obtionsObj )

## PARAMETERS
* method | GET or PUT or DELETE or HEAD
* bucket | the bucket name
* region | eg: ca-central-1
* theFilePath | the full path to the file in the bucket eg: /folder/filename.pdf
* expireSeconds | the length of time that the URL will be valid
* accessKey | AWS Access Key
* secretKey | AWS Secret Key
* optionsObj | empty or JSON object containing optional parameters

## OPTIONAL PARAMETERS
Optional parameters are passed as JSON inside the optionsObj parameter
* host | the provider's domain, with or without the bucket name. not required for AWS.

## USAGE

Generate a presigned URL with the desired method, then...

### GET

You can use Insert From URL to download the object to a container or variable (to download a binary object to a variable in FileMaker, you must add the --FM-return-container-variable cURL option). Alternatively, reference the presigned URL in a web viewer (in an src attribute, for example). Note: You may need to change the CORS settings for your bucket in order to load the object into a web viewer.

### PUT

Use Insert From URL with the following cURL options:
 
`"-X PUT -H " & Quote ( "Content-Length: " & $fileSizeInBytes ) & " -H " & Quote ( "Content-Type: " & $contentType ) & " --data-binary @$container -D $responseHeaders"`
 
`$contentType` is the mime type. For example `APPLICATION/PDF` or `IMAGE/PNG`. Having the correct content type stored in S3 is necessary to GET the object into a browser or web viewer. For example, without the content-type header `IMAGE/PNG`, a PNG file would simply by downloaded rather than displayed.
 
If the upload was successful, `PatternCount ( $responseHeaders ; "200 OK" )` will return True.

### DELETE

Use Insert From URL with the following cURL options:
 
`"-X DELETE -D $responseHeaders"`
 
If deletion was successful, `PatternCount ( $responseHeaders ; "204 No Content" )` will return True.

### HEAD

Use Insert From URL with the following cURL options:

`--head`

The result will include headers that you may wish to parse out, such as Content-Type and Content-Length. Here's one way to extract a header from the result `$result`:

```
Let ([
  ~header = "Content-Type" ;
  ~start = Position ( $result ; ~header ; 1 ; 1 ) + Length ( ~header ) + 2 ;
  ~end = Position ( $result ; "¶" ; ~start ; 1 ) ;
  ~value = Middle ( $result ; ~start ; ~end - ~start )
];
~value
)
```
