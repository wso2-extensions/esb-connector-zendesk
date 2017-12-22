# Configuring Zendesk Operations

[[Initializing the Connector]](#initializing-the-connector)  [[Obtaining user credentials]](#obtaining-user-credentials)

> NOTE: To work with the Zendesk connector, you need to have a Zendesk account. If you do not have a Zendesk account, go to [https://www.zendesk.com/register#getstarted](https://www.zendesk.com/register#getstarted) and create a Zendesk account.

To use the Zendesk connector, add the <zendesk.init> element in your configuration before carrying out any other Zendesk operations. 

Zendesk uses basic access authentication where user provides username and password.




## Using the Zendesk API

* **Follow the steps below to using the Zendesk API:**

    1. Requests to connect to the Zendesk API can be done only by a [verified user](https://support.zendesk.com/hc/en-us/articles/203663786-Verifying-a-user-s-email-address). 
        * For additional information on the authentication process refer [here](https://developer.zendesk.com/rest_api/docs/core/introduction#security-and-authentication).

## Configuring ESB

Follow the instructions below to import your Zendesk certificate to access the Zendesk API over https:

1. Extract the certificate by opening your browser and navigating to https://{company}.zendesk.com . Save the certificate file in <EI_HOME>/repository/resources/security folder as zendesk.crt where the name of the certificate is zendesk.
2. Open a command line terminal and execute the following command from <EI_HOME>/repository/resources/security location to import Zendesk certificate into the keystore.  
    ```xml
    keytool -importcert -file zendesk.crt -keystore client-truststore.jks -alias zendesk
    ```
    Provide **wso2carbon** as the password.
3.  Please ensure that the following Axis2 configurations are added and enabled in the <EI_HOME>\conf\axis2\axis2.xml file.
    Required message formatters

    **messageFormatters**
    ```xml
    <messageFormatter contentType="image/gif" class="org.wso2.carbon.relay.ExpandingMessageFormatter" />
    ```
    Required message builders

        **messageBuilders**
    ```xml
    <messageBuilder contentType="image/gif" class="org.wso2.carbon.relay.BinaryRelayBuilder" />
    ```

## Initializing the Connector
Specify the init method as follows:

**init**
```xml
<zendesk.init>
    <username>{$ctx:username}</username>
    <apiUrl>{$ctx:apiUrl}</apiUrl>
    <password>{$ctx:password}</password>
</zendesk.init>
```
**Properties** 
* username: The user name used for authentication. 
* apiUrl: The URL of the Zendesk account. Only the URL should be sent and the version should be omitted, e.g., https://wso2connector.zendesk.com.
* password:  The password used for authentication.
  

Now that you have connected to Zendesk, use the information in the following topics to perform various operations with the connector:

[Working with Attachments](attachments.md)

[Working with Search](search.md)

[Working with Tags](tags.md)

[Working with Ticket Audits](ticket_audits.md)

[Working with Tickets](tickets.md)

```xml
To ensure the security of the customer's sensitive data (e.g., user name and password), proxy services should only allow HTTPS access to ensure transport-level security. The transports property in the <proxy> element in your ESB configuration restricts the access to HTTPS only (<proxy transports="https"/>).
```