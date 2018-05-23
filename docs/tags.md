# Working with Tags in Zendesk

[[  Overview ]](#overview)  [[ Operation details ]](#operation-details)  [[  Sample configuration  ]](#sample-configuration)

### Overview 


The addTags operation allows tags to be added to tickets, users and organizations.

**addTags**
```xml
<zendesk.addTags>
    <componentId>{$ctx:componentId}</componentId>
    <componentType>{$ctx:componentType}</componentType>
    <tags>{$ctx:tags}</tags>
</zendesk.addTags> 
```

**Properties**
* componentId: The ID of the component to which the tags are added to.
* componentType: The type of the component which the tags are added to.
* tags: The tags which are added.

**Sample request**

Following is a sample request that can be handled by the addTags operation.

```json
{
    "username":"apptest.mahesh@gmail.com",
    "apiUrl":"https://abc387.zendesk.com",
    "password":"Colombo123",
    "componentType":"tickets",
    "componentId":"19",
    "tags":["testTag123"]
}
```
**Related Zendesk documentation**

https://developer.zendesk.com/rest_api/docs/core/tags#add-tags

#### Sample configuration

Following is a sample proxy service that illustrates how to connect to Zendesk with the init operation, and then use the addTags operation. The sample request for this proxy can be found in the addTags sample request.
```xml
As a best practice, create a separate sequence for handling the response payload for errors. In the following sample, this sequence is "faultHandlerSeq".
```
**Sample Proxy**
```xml
<?xml version="1.0" encoding="UTF-8"?>
<proxy xmlns="http://ws.apache.org/ns/synapse" name="zendesk_addTags" transports="https,http" statistics="disable"
   trace="disable" startOnLoad="true">
   <target>
      <inSequence onError="faultHandlerSeq">
         <property name="username" expression="json-eval($.username)" />
         <property name="apiUrl" expression="json-eval($.apiUrl)" />
         <property name="password" expression="json-eval($.password)" />
         <property name="componentId" expression="json-eval($.componentId)" />
         <property name="componentType" expression="json-eval($.componentType)" />
         <property name="tags" expression="json-eval($.tags)" />
         <zendesk.init>
            <username>{$ctx:username}</username>
            <apiUrl>{$ctx:apiUrl}</apiUrl>
            <password>{$ctx:password}</password>
         </zendesk.init>
         <zendesk.addTags>
            <componentId>{$ctx:componentId}</componentId>
            <componentType>{$ctx:componentType}</componentType>
            <tags>{$ctx:tags}</tags>
         </zendesk.addTags>
         <filter source="$axis2:HTTP_SC" regex="^[^2][0-9][0-9]">
            <then>
               <switch source="$axis2:HTTP_SC">
                  <case regex="401">
                     <property name="ERROR_CODE" value="600401" />
                     <property name="ERROR_MESSAGE" expression="json-eval($.error)" />
                     <property name="error_description" expression="json-eval($.description)" />
                  </case>
                  <case regex="404">
                     <property name="ERROR_CODE" value="600404" />
                     <property name="ERROR_MESSAGE" expression="json-eval($.error)" />
                     <property name="error_description" expression="json-eval($.description)" />
                  </case>
                  <case regex="403">
                     <property name="ERROR_CODE" value="600403" />
                     <property name="ERROR_MESSAGE" expression="json-eval($.error)" />
                     <property name="error_description" expression="json-eval($.description)" />
                  </case>
                  <case regex="400">
                     <property name="ERROR_CODE" value="600400" />
                     <property name="ERROR_MESSAGE" expression="json-eval($.error)" />
                     <property name="error_description" expression="json-eval($.description)" />
                  </case>
                  <case regex="500">
                     <property name="ERROR_CODE" value="600500" />
                     <property name="ERROR_MESSAGE" expression="json-eval($.error)" />
                     <property name="error_description" expression="json-eval($.description)" />
                  </case>
                  <default>
                     <property name="ERROR_CODE" expression="$axis2:HTTP_SC" />
                     <property name="ERROR_MESSAGE" expression="json-eval($.error)" />
                     <property name="error_description" expression="json-eval($.description)" />
                  </default>
               </switch>
               <sequence key="faultHandlerSeq" />
            </then>
         </filter>
         <respond />
      </inSequence>
      <outSequence>
         <send />
      </outSequence>
   </target>
   <description />
</proxy>
```