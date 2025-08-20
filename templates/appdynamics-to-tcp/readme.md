# appdynamics-to-tcp Template

The appdynamics-to-tcp template provides functionality to query, filter/transform and send from AppDynamics API requests to a TCP listener, via an integration-hub pipeline.

<p align="center">
<img src="../../assets/images/appdynamics-to-tcp/flow_appdynamics-to-tcp.jpg" />
</p>

## Prerequisites

Prior to using this template you must have already defined an AppDynamics REST datasource that your pipeline will use.

### Creating a REST Datasource

- The following properties are required when creating.

  <br><img src="../../assets/images/datasource-rest.png" /><br>
  <br />
    <table>
      <tr>
        <td><code>Host</code></td>
        <td>The hostname of the AppDynamics controller you wish to use</td>
      </tr>
      <tr>
        <td><code>Port</code></td>
        <td>The port that the controller API is listening on (443)</td>
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
        <td>The HTTP method to be used (This should be set to 'HTTPS' for the controller API)</td>
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
                <td>The hostname for authenticating<br> appdynamics: <code>controller.saas.appdynamics.com</code></td>
              </tr>
              <tr>
                <td><code>Authorization Url</code></td>
                <td>The endpoint for the API provider authorization server. <br /><br />(<b>This is not required for Appdynamics</b>)</td>
              </tr>
              <tr>
                <td><code>Token Url</code></td>
                <td>The provider's authentication server, to exchange an authorization code for an access token<br> appdynamics: <code>/controller/api/oauth/access_token</code</td>
              </tr>
              <tr>
                <td><code>Redirect Url</code></td>
                <td>The redirect_uri must match one of the URLs the developer registered when creating the application, and the authorization server should reject the request if it does not match. <br /><br />(<b>This is not required for Appdynamics</b>)</td>
              </tr>
              <tr>
                <td><code>Client Secret</code></td>
                <td>The client secret for your Appdynamics App Client</td>
              </tr>
              <tr>
                <td><code>Client Id</code></td>
                <td>The client ID for your Appdynamics App Client</td>
              </tr>
              <tr>
                <td><code>Scope</code></td>
                <td>The scope of access you are requesting, which may include multiple space-separated values. <br /><br />(<b>This is not required for Appdynamics</b>)</td>
              </tr>
              <tr>
                <td><code>Grant Type</code></td>
                <td>For Appdynamics, this should be set to <code>client_credentials</code></td>
              </tr>
              <tr>
                <td><code>Client Authentication Method</code></td>
                <td>Method to use when sending the auth request: (Header, Body OR Both) </br></br> This needs to be set to <code>Body</code> for Appdynamics</td>
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
