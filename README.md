# Zendesk EI Connector

The Zendesk [Connector](https://docs.wso2.com/display/EI640/Working+with+Connectors) allows you to access the [Zendesk v2 API ](https://developer.zendesk.com/rest_api/docs/core/introduction) through WSO2 Enterprise Integrator (WSO2 EI). Many businesses use the Zendesk API to automate and enhance their customer support.

## Compatibility

| Connector version | Supported Zendesk API version | Supported WSO2 ESB/EI version |
| ------------- | ------------- | ------------- |
| [1.0.2](https://github.com/wso2-extensions/esb-connector-zendesk/tree/org.wso2.carbon.connector.zendesk-1.0.2) | V2 | ESB 5.0.0, ESB 4.9.0, EI 6.3.0, EI 6.4.0    |

## Getting started

#### Download and install the connector

1. Download the connector from the [WSO2 Store](https://store.wso2.com/store/assets/esbconnector/details/e24deae4-145a-405d-a57d-1db775ca7efa) by clicking the Download Connector button.
2. Then you can follow this [Documentation](https://docs.wso2.com/display/EI640/Working+with+Connectors+via+the+Management+Console) to add and enable the connector via the Management Console in your EI instance.
3. For more information on using connectors and their operations in your EI configurations, see [Using a Connector](https://docs.wso2.com/display/EI640/Using+a+Connector).
4. If you want to work with connectors via EI tooling, see [Working with Connectors via Tooling](https://docs.wso2.com/display/EI640/Working+with+Connectors+via+Tooling).

#### Configuring the connector operations

To get started with Zendesk connector and their operations, see [Configuring Zendesk Operations](docs/config.md).


## Building From the Source

Follow the steps given below to build the Zendesk connector from the source code:

1. Get a clone or download the source from [Github](https://github.com/wso2-extensions/esb-connector-zendesk).
2. Run the following Maven command from the `esb-connector-zendesk` directory: `mvn clean install`.
3. The Zendesk connector zip file is created in the `esb-connector-zendesk/target` directory

## How You Can Contribute

As an open source project, WSO2 extensions welcome contributions from the community.
Check the [issue tracker](https://github.com/wso2-extensions/esb-connector-zendesk/issues) for open issues that interest you. We look forward to receiving your contributions.
