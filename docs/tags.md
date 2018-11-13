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
**Sample response**

Given below is a sample response for the addTags operation.

```json
{
  "tags": [
    "important",
    "customer"
  ]
}
```

**Related Zendesk documentation**
https://developer.zendesk.com/rest_api/docs/core/tags#add-tags

### Sample configuration

Following example illustrates how to connect to Zendesk with the init operation and addTags operation.

1. Create a sample proxy as below :

```xml
<?xml version="1.0" encoding="UTF-8"?>
<proxy xmlns="http://ws.apache.org/ns/synapse" name="addTags" transports="https,http" statistics="disable"
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
         <respond />
      </inSequence>
      <outSequence>
         <send />
      </outSequence>
   </target>
   <description />
</proxy>
```
2. Create a json file named addTags.json and copy the configurations given below to it:

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
3. Replace the credentials with your values.

4. Execute the following curl command:

```bash
curl http://localhost:8280/services/addTags -H "Content-Type: application/json" -d @addTags.json
```

5. Zendesk returns a json response similar to the one shown below:
 
```json
{
  "tags": [
    "important",
    "customer"
  ]
}
```
