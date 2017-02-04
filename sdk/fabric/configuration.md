# Configuration File Generation

The Fabric SDK has, among others, the responsibility to generate configuration files for use by deployed handlers. Usually these files are generated as part of the deployment process. 

This section describes the available configuration models, which class can parse them from `ApiRequest`'s and which file generators you can use to turn the models into actual files. 

## Models

The following configuration models are available:

| Class                          | Description   |
| ------------------------------ | ------------- |
| Dictionary<string, object>     | The actual configuration structure, and it's values, is unknown up front. Therefore they are represented as a [dynamic json object](/sdk/dynamic-json) turned into a Dictionary<string, object>. |
| HandlerRouting                 | The handler routing class encapsulates the `InputSubjectFilter` and the `OutputSubjectRoute` collections. |
| InputSubjectFilter             | Represents an input subject filter, which defines the source handlers (and it's output subjects) that the target handler is only interested in receiving messages from. If this collection is empty, then the target handler will process messages from all handlers pushing messages into the communication channel. |
| OutputSubjectRoute             | Represents a route, starting from an output subject defined on the target handler, towards an entity outside of the default communication channel. Currently these entities include other target communication channels or target endpoints. |
| Variable                       | Contains the variable information of the end user of the handler. |
| HandlerRuntime                 | Defines the technical details that a handler needs to operate, including information about the communication channel, logging destinations, metric sinks etc... |
| EnvironmentConfiguration       | This configuration is not intended for handlers like the above, it is intended to be used by the fabric locally as a cache of it's own configuration values. |

The following model entities serve as input, or intermediate state needed to generate the configuration model.

| Class                          | Description   |
| ------------------------------ | ------------- |
| HandlerDefinition              | Defines meta data for the handler, like name or version information. It also provides the `HandlerConfigurationDefinition`'s. |
| HandlerConfigurationDefinition | Defines which configuration properties exist for the handler. This data serves as input to generate the handler configuration file. |
| HandlerDefinitionVersion       | Subset of the handler definition, extracted from the actual configuration data. This data is needed to look up the full `HandlerDefinition`. |

## Parsers

The following class help in parsing model instances from `ApiRequest` responses.

| Class                              | Description   |
| ---------------------------------- | ------------- |
| EnvironmentsParser                 | Parses the response of an `EnvironmentsRequest` into a list of `EnvironmentConfiguration` entities. |
| EnvironmentParser                  | Parses the response of an `EnvironmentRequest` into a single `EnvironmentConfiguration` entity |
| HandlerConfigurationParser         | Can parse the response of a `HandlerConfigurationRequest` into a `HandlerDefinitionVersion`, a configuration dictionary, or a `HandlerRouting` instance. |
| HandlerDefinitionParser            | Parses the response of a `HandlerDefinitionRequest` into a `HanderDefinition` entity. |
| HandlerDefinitionsParser           | Parses the response of a `HandlerDefinitionsRequest` into a list of `HanderDefinition` entities. |
| VariableConfigurationParser        | Parses the response of a `VariablesRequest` into a list of `Variable` entities. |

## File Generators

| Class                              | Description   |
| ---------------------------------- | ------------- |
| HandlerConfigurationFileGenerator  | Generates a file, called `handler.configuration.json`, based on a handler configuration dictionary. |
| HandlerRoutingFileGenerator        | Generates a file, called `handler.routing.json`, based on a `HandlerRouting` instance. |
| HandlerRuntimeFileGenerator        | Generates a file, called `handler.runtime.json`, based on a `HandlerRuntime` instance. |
| VariableConfigurationFileGenerator | Generates a file, called `handler.variables.json`, based on a list of `Variable` instances. |
| EnvironmentFileGenerator           | Generates a file, called `environment.json`, based on an `EnvironmentConfiguration` instance. |
