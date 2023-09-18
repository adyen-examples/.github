# Adyen Webhooks

Webhooks are HTTP callbacks sent to an endpoint on your server. These webhooks are used to receive automatic updates about the payments.

This guide explains how to easily consume and test webhooks that the Adyen platform sends.


![Webhooks testing](pages/webhooks-testing-diagram.png)

## Prerequisites

1. Add a [Standard webhook](https://docs.adyen.com/development-resources/webhooks/#set-up-webhooks-in-your-customer-area) in your Customer Area
2. Implement the endpoint that can receive the webhooks (or use one of our existing [example-integrations](https://github.com/adyen-examples#%EF%B8%8F-official-integration-examples))

## Making your server reachable

Your endpoint that will consume the incoming webhook must be publicly accessible. In this example, we assume that `api/webhooks/notifications` is the endpoint that receives webhooks.

There are typically 3 options:
1. Deploy on your own server or a cloud provider
2. Deploy on Gitpod
3. Expose your localhost with tunneling software (i.e. ngrok, dev tunnels)

### Option 1: Cloud deployment
If you deploy on your cloud provider (or your own public server) the webhook URL will be the URL of the server.
```
  https://{your-cloud-provider}/api/webhooks/notifications
```

### Option 2: Gitpod
If you use [Gitpod](https://github.com/adyen-examples/.github/blob/main/pages/gitpod-get-started.md) the webhook URL will be the host assigned by Gitpod
```
  https://8080-myorg-myrepo-y8ad7pso0w5.ws-eu75.gitpod.io/api/webhooks/notifications
```

> **Note**: when starting a new Gitpod workspace the host changes, make sure to **update the Webhook URL**.

### Option 3: Localhost via tunneling software 

### NGROK

If you use a tunneling service like [ngrok](ngrok), the webhook URL will be the generated URL (ie `https://c991-80-113-16-28.ngrok.io`).

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

> **Note**: when restarting ngrok a new URL is generated, make sure to **update the Webhook URL**.

### DEV TUNNELS (.NET)

.NET developers might prefer to use Visual Studio dev tunnels to expose their localhost.
* Create your public (temporary/persistent) dev tunnel by following [this guide](https://medium.com/adyen/how-to-use-visual-studio-dev-tunnels-to-receive-webhooks-on-localhost-6cee1d12a670)

If you use Visual Studio 17.4+, the webhook URL will be the generated URL (i.e. `https://xd1r2txt-5001.euw.devtunnels.ms`).


Read more about webhooks in our [documentation](https://docs.adyen.com/development-resources/webhooks/).


