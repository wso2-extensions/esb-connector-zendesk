# Working with Project Jobs in Zendesk

[[  Overview ]](#overview)  [[ Operation details ]](#operation-details)  [[  Sample configuration  ]](#sample-configuration)

### Overview 

The following operations allow you to work with attachments. Click an operation name to see details on how to use it.
For a sample proxy service that illustrates how to work with attachments, see [Sample configuration](#sample-configuration).

| Operation        | Description |
| ------------- |-------------|
| [getAttachment](#retrieving-an-attachment)    | Retrieves an attachment. |
| [uploadFiles](#uploading-files)      | Uploads files. |
| [deleteUpload](#deleting-an-upload)      | Deletes an upload. |

### Operation details

This section provides more details on each of the operations.

#### Retrieving an attachment
The getAttachment operation retrieves an attachment's details. 

**getAttachment**
```xml
<zendesk.getAttachment>
    <attachmentId>{$ctx:attachmentId}</attachmentId>
</zendesk.getAttachment>Job>
```

**Properties**
* attachmentId: Required - ID of the attachment to be retrieved.

**Sample request**

Following is a sample request that can be handled by the getAttachment operation.

```json
{
    "username":"dilanijtest2@gmail.com",
    "apiUrl":"https://dilanijtest2.zendesk.com",
    "password":"1qaz2wsx@",
    "attachmentId":"968683092"
}
```

**Related Zendesk documentation**

https://developer.zendesk.com/rest_api/docs/core/attachments#getting-attachments

####  Uploading files

The uploadFiles operation allows you to upload a file to be used as an attachment to a ticket. Adding multiple attachments to the same upload is handled by splitting requests and passing the token received from the first request to each subsequent one.

**uploadFiles**
```xml
<zendesk.uploadFiles>
    <token>{$url:token}</token>
    <fileName>{$url:fileName}</fileName>
</zendesk.uploadFiles>
```

**Properties**
* token: The token received, which is used for multi-file upload.
* fileName: Required - The name of the file to be uploaded.

**Sample request**

Following is a sample request that can be handled by the uploadFiles operation.

```xml
http://localhost:8280/services/zendesk_uploadFiles?apiUrl=https://wso2connector.zendesk.com&username=wso2connector.user@gmail.com&password=1qaz2wsx@&fileName=picture.png&token=End7UhffGumiS7JoH2B4hGeHupX
```
**Related Zendesk documentation**

https://developer.zendesk.com/rest_api/docs/core/attachments#uploading-files

####  Deleting an upload

The deleteUpload operation deletes an uploaded file by specifying the token of the file. 

**deleteUpload**
```xml
<zendesk.deleteUpload>
    <token>{$ctx:token}</token>
</zendesk.deleteUpload>
```

**Properties**
* token: Required - The token of the uploaded file to be deleted.

**Sample request**

Following is a sample request that can be handled by the deleteUpload operation.

```json
{
    "username":"wso2connector.user@gmail.com",
    "apiUrl":"https://wso2connector.zendesk.com",
    "password":"1qaz2wsx@",
    "token":"B4Kx53Wgdkw3WZPgXdfgdCPNm2de5"
}
```
**Related Zendesk documentation**

https://developer.zendesk.com/rest_api/docs/core/attachments#deleting-uploads

#### Sample configuration

Following is a sample proxy service that illustrates how to connect to Zendesk with the init operation, and then use the createJob operation. The sample request for this proxy can be found in the getAttachment sample request. You can use this sample as a template for using other operations in this category.
```xml
As a best practice, create a separate sequence for handling the response payload for errors. In the following sample, this sequence is "faultHandlerSeq".
```
**Sample Proxy**
```xml
<?xml version="1.0" encoding="UTF-8"?>
<proxy xmlns="http://ws.apache.org/ns/synapse" name="zendesk_getAttachment" transports="https" statistics="disable" trace="disable" startOnLoad="true">
     <target>
     <inSequence onError="faultHandlerSeq">
      <property name="username" expression="json-eval($.username)"/>
      <property name="apiUrl" expression="json-eval($.apiUrl)"/>
      <property name="password" expression="json-eval($.password)"/>
      <property name="attachmentId" expression="json-eval($.attachmentId)"/>
      <zendesk.init>
         <username>{$ctx:username}</username>
         <apiUrl>{$ctx:apiUrl}</apiUrl>
         <password>{$ctx:password}</password>
      </zendesk.init>
      <zendesk.getAttachment>
         <attachmentId>{$ctx:attachmentId}</attachmentId>
      </zendesk.getAttachment>
      <filter source="$axis2:HTTP_SC" regex="^[^2][0-9][0-9]">
            <then>
               <switch source="$axis2:HTTP_SC">
                 <case regex="401">
                     <property name="ERROR_CODE" value="600401"/> 
                     <property name="ERROR_MESSAGE" expression="json-eval($.error)"/>
                     <property name="error_description" expression="json-eval($.description)"/>
                  </case>
                  <case regex="404">
                     <property name="ERROR_CODE" value="600404"/>                  
                     <property name="ERROR_MESSAGE" expression="json-eval($.error)"/>
                     <property name="error_description" expression="json-eval($.description)"/>
                  </case>
                  <case regex="403">
                     <property name="ERROR_CODE" value="600403"/>
                     <property name="ERROR_MESSAGE" expression="json-eval($.error)"/>
                     <property name="error_description" expression="json-eval($.description)"/>
                  </case>              
                  <case regex="400">
                     <property name="ERROR_CODE" value="600400"/>         
                     <property name="ERROR_MESSAGE" expression="json-eval($.error)"/>
                     <property name="error_description" expression="json-eval($.description)"/>
                  </case>
                  <case regex="500">
                     <property name="ERROR_CODE" value="600500"/> 
                     <property name="ERROR_MESSAGE" expression="json-eval($.error)"/>
                     <property name="error_description" expression="json-eval($.description)"/>
                  </case>
                  <default>
                     <property name="ERROR_CODE" expression="$axis2:HTTP_SC"/>
                     <property name="ERROR_MESSAGE" expression="json-eval($.error)"/>
                     <property name="error_description" expression="json-eval($.description)"/>
                  </default>
               </switch>
               <sequence key="faultHandlerSeq"/>
            </then>
         </filter>
         <respond/>
     </inSequence>
      <outSequence>
       <send/>
      </outSequence>
     </target>
   <description/>
  </proxy>
```