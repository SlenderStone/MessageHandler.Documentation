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
| AuthorizationRequest            | Performs a request to obtain an access token, based on your client id and `client secret. |
| RefreshTokenRequest             | Refreshes an access token based on the refresh token obtained through an `AuthorizationRequest`. |
| EnvironmentRequest              | Gets the details of a specific environment.      |
| EnvironmentsRequest             | List all environments that the requesting account has access to. |
| HandlerConfigurationRequest     | Gets the configuration details of a specific handler deployed to a specific environment. |
| HandlerConfigurationsRequest    | Gets the configuration details of all handlers deployed to a specific environment. |
| HandlerDefinitionRequest        | Gets the definition of a specific handler. |
| HandlerDefinitionsRequest       | Gets the definition of all handlers. |
| VariablesRequest                | Gets the variables of a specific account, if authorized |