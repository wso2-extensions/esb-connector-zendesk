# Working with Project Jobs in Zendesk

[[  Overview ]](#overview)  [[ Operation details ]](#operation-details)  [[  Sample configuration  ]](#sample-configuration)

### Overview 


The search operation allows to search for tickets, users, organizations, and forum topics. 

**search**
```xml
<zendesk.search>
    <sortBy>{$ctx:sortBy}</sortBy>
    <sortOrder>{$ctx:sortOrder}</sortOrder>
    <query>{$ctx:query}</query>
</zendesk.search> 
```

**Properties**
* sortBy: The field on which the results will be sorted. Possible values are updated_at, created_at, priority, status, and ticket_type
* sortOrder: Whether to sort the results in ascending (asc) or descending (desc) order or by most relevant results (relevance). Defaults to relevance.
* query: The search text to be matched or a search string. For further details, refer to [Query](https://developer.zendesk.com/rest_api/docs/core/search#query).

**Sample request**

Following is a sample request that can be handled by the search operation.

```json
{
    "username":"wso2connector.user@gmail.com",
    "apiUrl":"https://wso2connector.zendesk.com",
    "password":"1qaz2wsx@",
    "query":"\"status>unsolved+requester:wso2connector.user@gmail.com+type:ticket\"",
    "sortOrder":"asc",
    "sortBy":"created_at"
} 
```
**Related Zendesk documentation**

https://developer.zendesk.com/rest_api/docs/core/search#search

#### Sample configuration

Following is a sample proxy service that illustrates how to connect to Zendesk with the init operation, and then use the search operation. The sample request for this proxy can be found in the search sample request.
```xml
As a best practice, create a separate sequence for handling the response payload for errors. In the following sample, this sequence is "faultHandlerSeq".
```
**Sample Proxy**
```xml
<?xml version="1.0" encoding="UTF-8"?>
<proxy xmlns="http://ws.apache.org/ns/synapse" name="zendesk_search" transports="https" statistics="disable" trace="disable" startOnLoad="true">
     <target>
     <inSequence onError="faultHandlerSeq">
      <property name="username" expression="json-eval($.username)"/>
      <property name="apiUrl" expression="json-eval($.apiUrl)"/>
      <property name="password" expression="json-eval($.password)"/>
      <property name="sortBy" expression="json-eval($.sortBy)"/>
      <property name="sortOrder" expression="json-eval($.sortOrder)"/>
      <property name="query" expression="json-eval($.query)"/>
      <zendesk.init>
         <username>{$ctx:username}</username>
         <apiUrl>{$ctx:apiUrl}</apiUrl>
         <password>{$ctx:password}</password>
      </zendesk.init>
      <zendesk.search>
         <sortBy>{$ctx:sortBy}</sortBy>
         <sortOrder>{$ctx:sortOrder}</sortOrder>
         <query>{$ctx:query}</query>
      </zendesk.search>
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