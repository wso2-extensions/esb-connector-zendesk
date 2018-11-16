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
**Sample response**

Given below is a sample response for the listTicketAudits operation.

```json
{
  "audits": [
    {
      "created_at": "2011-09-25T22:35:44Z",
      "via": {
        "channel": "web"
      },
      "metadata": {
        "system": {
          "location": "San Francisco, CA, United States",
          "client": "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_6_8) AppleWebKit/535.1 (KHTML, like Gecko) Chrome/14.0.835.186 Safari/535.1",
          "ip_address": "76.218.201.212"
        },
        "custom": {
        }
      },
      "id": 2127301143,
      "ticket_id": 666,
      "events": [
        {
          "html_body": "<p>This is a new private comment</p>",
          "public": false,
          "body": "This is a new private comment",
          "id": 2127301148,
          "type": "Comment",
          "attachments": [
          ]
        },
        {
          "via": {
            "channel": "rule",
            "source": {
              "to": { },
              "from": {
                "id": 35079792,
                "title": "Assign to first responder"
              },
              "rel": "trigger"
            }
          },
          "id": 2127301163,
          "value": "open",
          "type": "Change",
          "previous_value": "new",
          "field_name": "status"
        }
      ],
      "author_id": 5246746
    },
    ...
    {
      ...
      "events": [
        ...
      ],
    }
  ],
  "next_page": null,
  "previous_page": null,
  "count": 5
}
```

**Related Zendesk documentation**
https://developer.zendesk.com/rest_api/docs/core/ticket_audits#listing-audits

### Sample configuration

**Sample Proxy**
Following example illustrates how to connect to Zendesk with the init operation and listTicketAudits operation.

1. Create a sample proxy as below :
```xml
<?xml version="1.0" encoding="UTF-8"?>
<proxy xmlns="http://ws.apache.org/ns/synapse" name="listTicketAudits" transports="https" statistics="disable" trace="disable" startOnLoad="true">
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
      <respond/>
     </inSequence>
      <outSequence>
        <send/>
      </outSequence>
     </target>
   <description/>
  </proxy>
```
2. Create a json file named listTicketAudits.json and copy the configurations given below to it:

```json
{
    "username":"wso2connector.user@gmail.com",
    "apiUrl":"https://wso2connector.zendesk.com",
    "password":"1qaz2wsx@",
    "ticketId":"123"
}
```
3. Replace the credentials with your values.

4. Execute the following curl command:

```bash
curl http://localhost:8280/services/listTicketAudits -H "Content-Type: application/json" -d @listTicketAudits.json
```

5. Zendesk returns a json response similar to the one shown below:
 
```json
{
  "audits": [
    {
      "created_at": "2011-09-25T22:35:44Z",
      "via": {
        "channel": "web"
      },
      "metadata": {
        "system": {
          "location": "San Francisco, CA, United States",
          "client": "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_6_8) AppleWebKit/535.1 (KHTML, like Gecko) Chrome/14.0.835.186 Safari/535.1",
          "ip_address": "76.218.201.212"
        },
        "custom": {
        }
      },
      "id": 2127301143,
      "ticket_id": 666,
      "events": [
        {
          "html_body": "<p>This is a new private comment</p>",
          "public": false,
          "body": "This is a new private comment",
          "id": 2127301148,
          "type": "Comment",
          "attachments": [
          ]
        },
        {
          "via": {
            "channel": "rule",
            "source": {
              "to": { },
              "from": {
                "id": 35079792,
                "title": "Assign to first responder"
              },
              "rel": "trigger"
            }
          },
          "id": 2127301163,
          "value": "open",
          "type": "Change",
          "previous_value": "new",
          "field_name": "status"
        }
      ],
      "author_id": 5246746
    },
    ...
    {
      ...
      "events": [
        ...
      ],
    }
  ],
  "next_page": null,
  "previous_page": null,
  "count": 5
}
```