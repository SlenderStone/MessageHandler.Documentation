# API Client

The API client consists of the following foundational classes:

| Class              | Description   |
| ------------------ | ------------- |
| Api                | Allows you to make an authorized connection to the MessageHandler REST api, resulting in an `AuthorizedClient` instance. |
| AuthorizedClient   | Allows you to execute `ApiRequest` instances against the MessageHandler REST api in an authorized manner, it will automatically refresh the access token when it expiers.      |
| ApiRequest         | Interface that defines all requests that can be made against the API. |

Next to the foundational classes, it also offers a number of request implementations that make calling the REST api a breeze.

## Requests

The following request classes are available to pass into an `AuthorizedClient`

| Class                           | Description   |
| ------------------------------- | ------------- |
| AuthorizationRequest            | right-aligned |
| EnvironmentRequest              | are neat      |
| EnvironmentsRequest             | are neat      |
| HandlerConfigurationRequest     | are neat      |
| HandlerConfigurationsRequest    | are neat      |
| HandlerDefinitionRequest        | are neat      |
| HandlerDefinitionsRequest       | are neat      |
| RefreshTokenRequest             | are neat      |
| VariablesRequest                | are neat      |