# Working with Ticket Audits in Zendesk

[[  Overview ]](#overview)  [[ Operation details ]](#operation-details)  [[  Sample configuration  ]](#sample-configuration)

### Overview 


The listTicketAudits operation lists ticket audits. Audits are a read-only history of all updates to a ticket and the events that occur as a result of these updates. Each audit represents a single update to the ticket, and each audit includes a list of changes, such as:
* Changes to ticket fields
* Addition of a new comment
* Addition or removal of tags
* Notifications sent to Groups, Assignees, Requesters, and CCs

To learn more about adding new comments to tickets, see the [Ticket documentation](https://developer.zendesk.com/rest_api/docs/core/ticket_comments).

**listTicketAudits**
```xml
<zendesk.listTicketAudits>
    <ticketId>{$ctx:ticketId}</ticketId>
</zendesk.listTicketAudits> 
```

**Properties**
* ticketId: Required - The ID of the ticket whose audits you want to list.

**Sample request**

Following is a sample request that can be handled by the listTicketAudits operation.

```json
{
    "username":"wso2connector.user@gmail.com",
    "apiUrl":"https://wso2connector.zendesk.com",
    "password":"1qaz2wsx@",
    "ticketId":"123"
}
```
**Related Zendesk documentation**

https://developer.zendesk.com/rest_api/docs/core/ticket_audits#listing-audits

#### Sample configuration

Following is a sample proxy service that illustrates how to connect to Zendesk with the init operation, and then use the listTicketAudits operation. The sample request for this proxy can be found in the listTicketAudits sample request.
```xml
As a best practice, create a separate sequence for handling the response payload for errors. In the following sample, this sequence is "faultHandlerSeq".
```
**Sample Proxy**
```xml
<?xml version="1.0" encoding="UTF-8"?>
<proxy xmlns="http://ws.apache.org/ns/synapse" name="zendesk_listTicketAudits" transports="https" statistics="disable" trace="disable" startOnLoad="true">
     <target>
     <inSequence onError="faultHandlerSeq" >
      <property name="username" expression="json-eval($.username)"/>
      <property name="apiUrl" expression="json-eval($.apiUrl)"/>
      <property name="password" expression="json-eval($.password)"/>
      <property name="ticketId" expression="json-eval($.ticketId)"/>
      <zendesk.init>
         <username>{$ctx:username}</username>
         <apiUrl>{$ctx:apiUrl}</apiUrl>
         <password>{$ctx:password}</password>
      </zendesk.init>
      <zendesk.listTicketAudits>
         <ticketId>{$ctx:ticketId}</ticketId>
      </zendesk.listTicketAudits>
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