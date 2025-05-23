# rest-to-tcp Template

The rest-to-tcp template provides functionality to query, filter/transform and send from HTTP requests to a TCP listener, via an integration-hub pipeline.

<p align="center">
<img src="../../assets/images/rest-to-tcp/1.0/flow_rest-to-tcp.jpg" />
</p>

## Prerequisites

Prior to using this template you must have already defined a REST datasource that your pipeline will use.

### Creating a REST Datasource

- The following properties are required when creating.

  <br><img src="../../assets/images/datasource-rest.png" /><br>
  <br />
    <table>
      <tr>
        <td><code>Host</code></td>
        <td>The hostname or IP address of the REST endpoint</td>
      </tr>
      <tr>
        <td><code>Port</code></td>
        <td>The port number of the REST endpoint</td>
      </tr>
      <tr>
        <td><code>Base URL</code></td>
        <td>The base URL path to add to subsequent requests</td>
      </tr>
      <tr>
        <td><code>Protocol</code></td>
        <td>Select either HTTPS or HTTP</td>
      </tr>
      <tr>
        <td><code>Method</code></td>
        <td>The HTTP method to be used</td>
      </tr>
      <tr>
        <td><code>Data Source Proxy</code></td>
        <td>If you need to use a proxy to make connection, the specify a pre-defined PROXY datasource</td>
      </tr>
      <tr>
        <td><code>SSL Context</code></td>
        <td>If you need to use custom CA certificate(s) then select the pre-defined SSL context</td>
      </tr>
      <tr>
        <td><code>Authorization Type</code></td>
        <td>Specify authentication method to use<br><br>
          <details>
            <summary>Basic Auth</summary>
            <table>
              <tr>
                <td><code>Username</code></td>
                <td>The username for authenticating</td>
              </tr>
              <tr>
                <td><code>Password</code></td>
                <td>The password for authenticating</td>
              </tr>
              <tr>
                <td><code>Authorization Header Prefix</code></td>
                <td>The authorization token type</td>
              </tr>
              <tr>
                <td><code>Authorization Header Name</code></td>
                <td>The authorization name</td>
              </tr>
              <tr>
                <td><code>SSL Context</code></td>
                <td>If you need to use custom CA certificate(s) then select the pre-defined SSL context</td>
              </tr>
            </table>
          </details>
          <details>
            <summary>OAuth</summary>
            <table>
              <tr>
                <td><code>Host</code></td>
                <td>The hostname for authenticating<br> azure: <code>login.microsoftonline.com</code></td>
              </tr>
              <tr>
                <td><code>Authorization Url</code></td>
                <td>The endpoint for the API provider authorization server, to retrieve the auth code<br> azure: <code>/&lt;TENANT ID&gt;/oauth2/v2.0/authorize</code><br></td>
              </tr>
              <tr>
                <td><code>Token Url</code></td>
                <td>The provider's authentication server, to exchange an authorization code for an access token<br> azure: <code>/&lt;TENANT ID&gt;/oauth2/v2.0/token</code</td>
              </tr>
              <tr>
                <td><code>Redirect Url</code></td>
                <td>The redirect_uri is must match one of the URLs the developer registered when creating the application, and the authorization server should reject the request if it does not match.</td>
              </tr>
              <tr>
                <td><code>Client Secret</code></td>
                <td>The client secret given to you by the API provider</td>
              </tr>
              <tr>
                <td><code>Client Id</code></td>
                <td>The ID for your client application registered with the API provider</td>
              </tr>
              <tr>
                <td><code>Scope</code></td>
                <td>The scope of access you are requesting, which may include multiple space-separated values <br> azure: <code>https://graph.microsoft.com/.default</code> </td>
              </tr>
              <tr>
                <td><code>Grant Type</code></td>
                <td>This will depend on the API service provider requirements</td>
              </tr>
              <tr>
                <td><code>Token Refresh</code></td>
                <td>The unit that should be combined with the *Token Refresh Unit*, refreshing the token will happen at this interval</td>
              </tr>
              <tr>
                <td><code>Token Refresh Unit</code></td>
                <td>This can be Seconds, Minutes, Hours or Days</td>
              </tr>
              <tr>
                <td><code>Authorization Header Prefix</code></td>
                <td>The authorization token type</td>
              </tr>
              <tr>
                <td><code>Authorization Header Name</code></td>
                <td>The authorization name</td>
              </tr>
              <tr>
                <td><code>Mail Support</code></td>
                <td>Set to true if connecting to Azure and you are wanting to interact with mail component</td>
              </tr>
              <tr>
                <td><code>SSL Context</code></td>
                <td>If you need to use custom CA certificate(s) then select the pre-defined SSL context</td>
              </tr>
            </table>
          </details>
        </td>
      </tr>
  </table>

<br/>

### Defining an Output Target

**Important:** _The following configuration applies ONLY to rest-to-tcp versions 1.2+_

This template requires that you have configured an `Output Target`, defining the destination for the processed data.

You can access the Output Targets by clicking on the `Output Targets` navigation item on the navbar on the left.

Here you will be presented with a list of Output Targets that have been configured previously.

<img src="../../assets/images/output_targets.jpg" width="700" />

<br />

To begin creating an Output Target, click the `CREATE OUTPUT TARGET` button located on the top-right of the page.

<img src="../../assets/images/create_output_target.jpg" width="700" />

<br />

Currently, there are two output targets that you can define:

| Type               | Description                                                      |
| :--------------------- | :--------------------------------------------------------------- |
| `TCP`             | Sends the processed message to a TCP listener      |
| `FILE`             | Sends the processed message to a FILE on the local filesystem                                     |

#### TCP Properties

<img src="../../assets/images/create_output_target_tcp.jpg" width="650" />

<br />

| Type               | Description                                                      |
| :--------------------- | :--------------------------------------------------------------- |
| `Output Target Name`             | Name of the Output Target you are creating      |
| `Data Type`             | Sends the processed message to a TCP listener      |
| `Host`             | Hostname or IP address of the TCP listener                                 |
| `Port`             | The port that the TCP listener listen on                                   |

#### FILE Properties

<img src="../../assets/images/create_output_target_file.jpg" width="650" />

<br />

| Type               | Description                                                      |
| :--------------------- | :--------------------------------------------------------------- |
| `Output Target Name`             | Name of the Output Target you are creating     |
| `Data Type`             | Sends the processed message to a TCP listener      |
| `Directory Name`             | The full path to the directory on the local filesystem where the file is located                                 |
| `File Name`             | The name of the file to output the processed message to                                   |
| `Append Newline`             | Toggle to determine whether new messages should be added on a new line or directly appended to the existing text without a line break                                  |