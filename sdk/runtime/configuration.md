# Configuration
This document explains the user variables, handler configuration of the routing, runtime and messagehandler itself.

## User Variables
|      Property Name     | Property Value | Description |
|:----------------------:|----------------|--|
| UserVariables           | List<Variables>| List for keeping user variables eg. Connectionstring, password... |

## Routing
|      Property Name     | Property Value | Description |
|:----------------------:|----------------|--|
| HandlerConfigurationRouting | HandlerRouting | Class for keeping router configuration data eg. ChannelId, EndpointId...  |

## Runtime
|      Property Name     | Property Value | Description |
|:----------------------:|----------------|--|
| HandlerInstanceId      | string         | Indentifier that represents HandlerInstance. |
| HandlerConfigurationId | string         | Identifier that represents HandlerConfiguration. |
| AccountId              | string         | Indentifier that represents Account. |
| EnvironmentId          | string         | Indentifier that represents Environment. |
| ChannelId              | string         | Indentifier that represents Channel. |
| TransportType          | string         | Represents TransportType. |
| Connectionstring       | string         | Represents Connectionstring. |

## MessageHandler
|      Property Name     | Property Value | Description |
|:----------------------:|----------------|--|
| HandlerConfigurationValues| Dictionary<string, object> | Dictionary for keeping Handler configuration values. |