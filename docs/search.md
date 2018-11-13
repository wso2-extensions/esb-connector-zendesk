# Working with Search in Zendesk

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

**Sample response**

Given below is a sample response for the search operation.

```json
{
  "results": [
    {
      "name":        "Hello DJs",
      "created_at":  "2009-05-13T00:07:08Z",
      "updated_at":  "2011-07-22T00:11:12Z",
      "id":          211,
      "result_type": "group"
      "url":         "https://foo.zendesk.com/api/v2/groups/211.json"
    },
    {
      "name":        "Hello MCs",
      "created_at":  "2009-08-26T00:07:08Z",
      "updated_at":  "2010-05-13T00:07:08Z",
      "id":          122,
      "result_type": "group"
      "url":         "https://foo.zendesk.com/api/v2/groups/122.json"
    },
    ...
  ],
  "facets":    null,
  "next_page": "https://foo.zendesk.com/api/v2/search.json?query=\"type:Group hello\"&sort_by=created_at&sort_order=desc&page=2",
  "prev_page": null,
  "count":     1234
}
```
**Related Zendesk documentation**
https://developer.zendesk.com/rest_api/docs/core/search#search

### Sample configuration

Following example illustrates how to connect to Zendesk with the init operation and search operation.

1. Create a sample proxy as below :

```xml
<?xml version="1.0" encoding="UTF-8"?>
<proxy xmlns="http://ws.apache.org/ns/synapse" name="search" transports="https" statistics="disable" trace="disable" startOnLoad="true">
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
      <respond/>
     </inSequence>
      <outSequence>
        <send/>
      </outSequence>
     </target>
   <description/>
  </proxy>
```

2. Create a json file named search.json and copy the configurations given below to it:

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
3. Replace the credentials with your values.

4. Execute the following curl command:

```bash
curl http://localhost:8280/services/search -H "Content-Type: application/json" -d @search.json
```

5. Zendesk returns a json response similar to the one shown below:
 
```json
{
  "results": [
    {
      "name":        "Hello DJs",
      "created_at":  "2009-05-13T00:07:08Z",
      "updated_at":  "2011-07-22T00:11:12Z",
      "id":          211,
      "result_type": "group"
      "url":         "https://foo.zendesk.com/api/v2/groups/211.json"
    },
    {
      "name":        "Hello MCs",
      "created_at":  "2009-08-26T00:07:08Z",
      "updated_at":  "2010-05-13T00:07:08Z",
      "id":          122,
      "result_type": "group"
      "url":         "https://foo.zendesk.com/api/v2/groups/122.json"
    },
    ...
  ],
  "facets":    null,
  "next_page": "https://foo.zendesk.com/api/v2/search.json?query=\"type:Group hello\"&sort_by=created_at&sort_order=desc&page=2",
  "prev_page": null,
  "count":     1234
}
```
