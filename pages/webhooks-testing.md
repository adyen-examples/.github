# Testing Adyen webhooks

Testing webhooks is not trivial: the Adyen platform is pushing (POST) the events (ie Payment Authorisation, Report available, Chargeback initiated, 
etc..) as they happen and it must be able to deliver those to your running application.

This guide explains how to consume and test webhooks.


![Webhooks testing](pages/webhooks-testing-diagram.png)

## Prerequisites

1. Add a Standard webhook in your Customer Area
2. Implement the webhook endpoint

### Making your server reachable

Your endpoint that will consume the incoming webhook must be publicly accessible.

There are typically 3 options:
* deploy on your own server or cloud provider
* deploy on Gitpod
* expose your localhost with tunneling software (i.e. ngrok)

#### Option 1: cloud deployment
If you deploy on your cloud provider (or your own public server) the webhook URL will be the URL of the server 
```
  https://{cloud-provider}/api/webhooks/notifications
```
In the Customer Area configure the webhook URL accordingly.

#### Option 2: Gitpod
If you use [Gitpod](https://github.com/adyen-examples/.github/blob/main/pages/gitpod-get-started.md) the webhook URL will be the host assigned by Gitpod
```
  https://myorg-myrepo-y8ad7pso0w5.ws-eu75.gitpod.io/api/webhooks/notifications
```
In the Customer Area configure the webhook URL accordingly.

**Note:** when starting a new Gitpod workspace the host changes, make sure to **update the Webhook URL**.

#### Option 3: localhost via tunneling software  

**NGROK**

If you use a tunneling service like [ngrok](ngrok) the webhook URL will be the generated URL (ie `https://c991-80-113-16-28.ngrok.io`)

```bash
  $ ngrok http 8080
  
  Session Status                online                                                                                           
  Account                       ############                                                                      
  Version                       #########                                                                                          
  Region                        United States (us)                                                                                 
  Forwarding                    http://c991-80-113-16-28.ngrok.io -> http://localhost:8080                                       
  Forwarding                    https://c991-80-113-16-28.ngrok.io -> http://localhost:8080           
```

In the Customer Area configure the webhook URL accordingly.

**Note:** when restarting ngrok a new URL is generated, make sure to **update the Webhook URL**.

**DEV TUNNELS (.NET)**

.NET developers migh prefer to use Visual Studio dev tunnels to expose the localhost:
* Add `https://*.devtunnels.ms` to your allowed origins
* Create your public (temporary/persistent) dev tunnel by following [this guide](https://learn.microsoft.com/en-us/aspnet/core/test/dev-tunnels?view=aspnetcore-7.0)

If you use Visual Studio 17.4 or higher, the webhook URL will be the generated URL (i.e. `https://xd1r2txt-5001.euw.devtunnels.ms`).

In the Customer Area configure the webhook URL accordingly.




