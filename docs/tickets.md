# Working with Tickets in Zendesk

[[  Overview ]](#overview)  [[ Operation details ]](#operation-details)  [[  Sample configuration  ]](#sample-configuration)

### Overview 

The following operations allow you to work with tickets. Click an operation name to see details on how to use it.
For a sample proxy service that illustrates how to work with tickets, see [Sample configuration](#sample-configuration).

| Operation        | Description |
| ------------- |-------------|
| [createTicket](#creating-a-ticket)    | Creates a new ticket. |
| [deleteTicket](#deleting-a-ticket)      | Deletes a ticket. |
| [updateTicket](#updating-a-ticket)      | Updates a ticket. |
| [listTickets](#listing-tickets)      | Lists tickets. |
| [showMultipleTickets](#showing-multiple-tickets)      | Shows multiple tickets. |

### Operation details

This section provides more details on each of the operations.

####  Creating a ticket
The createTicket operation allows users to create new tickets.

**createTicket**
```xml
<zendesk.createTicket>
    <tags>{$ctx:tags}</tags>
    <customFields>{$ctx:customFields}</customFields>
    <status>{$ctx:status}</status>
    <subject>{$ctx:subject}</subject>
    <problemId>{$ctx:problemId}</problemId>
    <forumTopicId>{$ctx:forumTopicId}</forumTopicId>
    <requesterId>{$ctx:requesterId}</requesterId>
    <organizationId>{$ctx:organizationId}</organizationId>
    <type>{$ctx:type}</type>
    <externalId>{$ctx:externalId}</externalId>
    <dueAt>{$ctx:dueAt}</dueAt>
    <submitterId>{$ctx:submitterId}</submitterId>
    <groupId>{$ctx:groupId}</groupId>
    <assigneeId>{$ctx:assigneeId}</assigneeId>
    <priority>{$ctx:priority}</priority>
    <comment>{$ctx:comment}</comment>
    <collaboratorIds>{$ctx:collaboratorIds}</collaboratorIds>
</zendesk.createTicket>
```

**Properties**
* tags: An array of tags to add to the ticket.
* customFields: An array of the custom fields of the ticket.
* status: The status of the ticket. Allowed values are new, open, pending, hold, solved, or closed. Default is open.
* subject: Required - The subject of the ticket.
* problemId: For tickets of type "incident", the numeric ID of the problem the incident is linked to, if any.
* forumTopicId: The numeric ID of the topic the ticket originated from, if any.
* requesterId: The numeric ID of the user asking for support through the ticket.
* type: The type of the ticket. Allowed values are problem, incident, question, or task.
* externalId: A unique external ID to link Zendesk tickets to local records.
* dueAt: For tickets of type task, the due date of the task. Accepts the ISO 8601 date format (yyyy-mm-dd).
* submitterId: The numeric ID of the user submitting the ticket.
* groupId: The numeric ID of the group to assign the ticket to.
* assigneeId: The numeric ID of the agent to assign the ticket to.
* priority: The priority of the ticket. Allowed values are urgent, high, normal, or low.
* comment: Required - A comment object that describes the problem, incident, question, or task. It also allows you to add attachments in the uploads array using the tokens (IDs) received when uploading files. For more information, see [Ticket comments](https://developer.zendesk.com/rest_api/docs/core/ticket_audits#audit-events).
* collaboratorIds: An array of the numeric IDs of agents or end users to CC on the ticket. An email notification is sent to them when the ticket is created.


**Sample request**

Following is a sample request that can be handled by the createTicket operation.

```json
{
    "username":"wso2connector.user@gmail.com",
    "apiUrl":"https://wso2connector.zendesk.com/api/v2",
    "password":"1qaz2wsx@",
    "tags":"",
    "customFields":"",
    "status":"",
    "subject":"initialSubject",
    "problemId":"",
    "forumTopicId":"",
    "requesterId":"",
    "organizationId":"",
    "type":"",
    "externalId":"",
    "dueAt":"",
    "submitterId":"",
    "groupId":"",
    "assigneeId":"",
    "priority":"",
    "comment":{
            "body":"Initial Subject",
            "uploads":[]
              },
    "collaboratorIds":""
}
```

**Related Zendesk documentation**

https://developer.zendesk.com/rest_api/docs/core/tickets#creating-tickets

####  Deleting a ticket

The deleteTicket operation deletes a ticket.

**deleteTicket**
```xml
<zendesk.deleteTicket>
    <ticketId>{$ctx:ticketId}</ticketId>
</zendesk.deleteTicket>
```

**Properties**
* ticketId: Required - The ID of the ticket to be deleted.

**Sample request**

Following is a sample request that can be handled by the deleteTicket operation.

```json
{
    "username":"wso2connector.user@gmail.com",
    "apiUrl":"https://wso2connector.zendesk.com",
    "password":"1qaz2wsx@",
    "ticketId":"3"
}
```
**Related Zendesk documentation**

https://developer.zendesk.com/rest_api/docs/core/tickets#deleting-tickets

####  Updating a ticket

The udpateTicket operation allows you to update a ticket.

**udpateTicket**
```xml
<zendesk.updateTicket>
    <tags>{$ctx:tags}</tags>
    <customFields>{$ctx:customFields}</customFields>
    <status>{$ctx:status}</status>
    <subject>{$ctx:subject}</subject>
    <problemId>{$ctx:problemId}</problemId>
    <forumTopicId>{$ctx:forumTopicId}</forumTopicId>
    <requesterId>{$ctx:requesterId}</requesterId>
    <organizationId>{$ctx:organizationId}</organizationId>
    <type>{$ctx:type}</type>
    <externalId>{$ctx:externalId}</externalId>
    <dueAt>{$ctx:dueAt}</dueAt>
    <ticketId>{$ctx:ticketId}</ticketId>
    <groupId>{$ctx:groupId}</groupId>
    <assigneeId>{$ctx:assigneeId}</assigneeId>
    <priority>{$ctx:priority}</priority>
    <comment>{$ctx:comment}</comment>
    <collaboratorIds>{$ctx:collaboratorIds}</collaboratorIds>
</zendesk.updateTicket> 
```

**Properties**
* tags: An array of tags to add to the ticket.
* customFields: An array of the custom fields of the ticket.
* status: The status of the ticket. Allowed values are new, open, pending, hold, solved, or closed. Default is open.
* subject: Required - The subject of the ticket.
* problemId: For tickets of type "incident", the numeric ID of the problem the incident is linked to, if any.
* forumTopicId: The numeric ID of the topic the ticket originated from, if any.
* organizationId: The numeric ID of the organization to assign the ticket to.
* requesterId: The numeric ID of the user asking for support through the ticket.
* type: The type of the ticket. Allowed values are problem, incident, question, or task.
* externalId: A unique external ID to link Zendesk tickets to local records.
* dueAt: For tickets of type task, the due date of the task. Accepts the ISO 8601 date format (yyyy-mm-dd).
* ticketId: Required - The ID of the ticket to be updated.
* submitterId: The numeric ID of the user submitting the ticket.
* groupId: The numeric ID of the group to assign the ticket to.
* assigneeId: The numeric ID of the agent to assign the ticket to.
* priority: The priority of the ticket. Allowed values are urgent, high, normal, or low.
* comment: Required - A comment object that describes the problem, incident, question, or task. It also allows you to add attachments in the uploads array using the tokens (IDs) received when uploading files. For more information, see [Ticket comments](https://developer.zendesk.com/rest_api/docs/core/ticket_audits#audit-events).
* collaboratorIds: An array of the numeric IDs of agents or end users to CC on the ticket. An email notification is sent to them when the ticket is created.


**Sample request**

Following is a sample request that can be handled by the udpateTicket operation.

```json

{
    "username":"wso2connector.user@gmail.com",
    "apiUrl":"https://wso2connector.zendesk.com/api/v2",
    "password":"1qaz2wsx@",
    "tags":"",
    "customFields":"",
    "status":"open",
    "subject":"sampleSubject",
    "problemId":"",
    "forumTopicId":"",
    "requesterId":"",
    "organizationId":"",
    "type":"",
    "externalId":"",
    "dueAt":"",
    "ticketId":"32",
    "groupId":"",
    "assigneeId":"",
    "priority":"high",
    "comment":{
            "body":"sampleComment",
            "uploads":[]
              },
    "collaboratorIds":""
}
```
**Related Zendesk documentation**

https://developer.zendesk.com/rest_api/docs/core/tickets#updating-tickets

####  Listing tickets

The listTickets operation allows you to list tickets. Tickets are sorted chronologically by created date, from oldest to newest. Note that the first ticket listed may not be the absolute oldest ticket in your account due to [ticket archiving](https://support.zendesk.com/hc/en-us/articles/203657756-Ticket-Archiving). For more filter options, see [Working with Search](search.md) in Zendesk.
**listTickets**
```xml
<zendesk.listTickets>
    <userId>{$ctx:userId}</userId>
    <organizationId>{$ctx:organizationId}</organizationId>
    <isCcd>{$ctx:isCcd}</isCcd>
    <isRecent>{$ctx:isRecent}</isRecent>
    <externalId>{$ctx:externalId}</externalId>
</zendesk.listTickets> 
```

**Properties**
* userId: The ID of the user to be queried. If no user ID is specified, tickets for all users will be listed.
* organizationId: The ID of the organization to be queried. If no organization is specified, tickets for all organizations will be listed.
* isCcd: Whether to return only those tickets that have collaborators and to display the collaborator IDs.
* isRecent: Whether to return only the most recently created tickets.
* externalId: Allows the user to pass the external ID.


**Sample request**

Following is a sample request that can be handled by the listTickets operation.

```json
{
    "username":"wso2connector.user@gmail.com",
    "apiUrl":"https://wso2connector.zendesk.com",
    "password":"1qaz2wsx@",
    "userId":"",
    "organizationId":"",
    "isCcd": "",
    "isRecent": "",
    "externalId":""
}
```
**Related Zendesk documentation**

https://developer.zendesk.com/rest_api/docs/core/tickets#listing-tickets

####  Showing multiple tickets

The showMultipleTickets operation returns the specified tickets.  

**showMultipleTickets**
```xml
<zendesk.showMultipleTickets>
    <ticketIds>{$ctx:ticketIds}</ticketIds>
</zendesk.showMultipleTickets>
```

**Properties**
* ticketIds: Required - Comma-separated list of IDs of the tickets to show.

**Sample request**

Following is a sample request that can be handled by the showMultipleTickets operation.

```json
{
    "username":"wso2connector.user@gmail.com",
    "apiUrl":"https://wso2connector.zendesk.com",
    "password":"1qaz2wsx@",
    "ticketIds":"1,2,3,4,5,6,7,8,9,10"
}
```
**Related Zendesk documentation**

https://developer.zendesk.com/rest_api/docs/core/tickets#show-multiple-tickets

#### Sample configuration

Following is a sample proxy service that illustrates how to connect to Zendesk with the init operation, and then use the createTicket operation. The sample request for this proxy can be found in the createTicket sample request. You can use this sample as a template for using other operations in this category.
```xml
As a best practice, create a separate sequence for handling the response payload for errors. In the following sample, this sequence is "faultHandlerSeq".
```
**Sample Proxy**
```xml
<?xml version="1.0" encoding="UTF-8"?>
<proxy xmlns="http://ws.apache.org/ns/synapse" name="zendesk_createTicket" transports="https" statistics="disable" trace="disable" startOnLoad="true">
     <target>
     <inSequence onError="faultHandlerSeq" >
      <property name="username" expression="json-eval($.username)"/>
      <property name="apiUrl" expression="json-eval($.apiUrl)"/>
      <property name="password" expression="json-eval($.password)"/>
      <property name="tags" expression="json-eval($.tags)"/>
      <property name="customFields" expression="json-eval($.customFields)"/>
      <property name="status" expression="json-eval($.status)"/>
      <property name="subject" expression="json-eval($.subject)"/>
      <property name="problemId" expression="json-eval($.problemId)"/>
      <property name="forumTopicId" expression="json-eval($.forumTopicId)"/>
      <property name="requesterId" expression="json-eval($.requesterId)"/>
      <property name="organizationId" expression="json-eval($.organizationId)"/>
      <property name="type" expression="json-eval($.type)"/>
      <property name="externalId" expression="json-eval($.externalId)"/>
      <property name="dueAt" expression="json-eval($.dueAt)"/>
      <property name="submitterId" expression="json-eval($.submitterId)"/>
      <property name="groupId" expression="json-eval($.groupId)"/>
      <property name="assigneeId" expression="json-eval($.assigneeId)"/>
      <property name="priority" expression="json-eval($.priority)"/>
      <property name="comment" expression="json-eval($.comment)"/>
      <property name="collaboratorIds" expression="json-eval($.collaboratorIds)"/>
      <zendesk.init>
         <username>{$ctx:username}</username>
         <apiUrl>{$ctx:apiUrl}</apiUrl>
         <password>{$ctx:password}</password>
      </zendesk.init>
      <zendesk.createTicket>
         <tags>{$ctx:tags}</tags>
         <customFields>{$ctx:customFields}</customFields>
         <status>{$ctx:status}</status>
         <subject>{$ctx:subject}</subject>
         <problemId>{$ctx:problemId}</problemId>
         <forumTopicId>{$ctx:forumTopicId}</forumTopicId>
         <requesterId>{$ctx:requesterId}</requesterId>
         <organizationId>{$ctx:organizationId}</organizationId>
         <type>{$ctx:type}</type>
         <externalId>{$ctx:externalId}</externalId>
         <dueAt>{$ctx:dueAt}</dueAt>
         <submitterId>{$ctx:submitterId}</submitterId>
         <groupId>{$ctx:groupId}</groupId>
         <assigneeId>{$ctx:assigneeId}</assigneeId>
         <priority>{$ctx:priority}</priority>
         <comment>{$ctx:comment}</comment>
         <collaboratorIds>{$ctx:collaboratorIds}</collaboratorIds>
      </zendesk.createTicket>
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