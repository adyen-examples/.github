# Setting up Adyen examples with GitHub Codespaces

This guide will walk you through the steps to run any Adyen examples in a GitHub Codespaces environment.

## Step 0: Choose Your Codespaces Setup

You have two options for setting up Codespaces with this repository:

### Option 1: Fork the Repository (Recommended for Personal Use)

Forking the repository allows you to manage your own Codespaces environment and secrets securely within your personal GitHub account.

1.  Navigate to the Adyen example repository you want to use (e.g., `adyen-node-online-payments`, `adyen-java-spring-online-payments`, etc.).
2.  Click the **"Fork"** button in the top-right corner.
3.  Choose where you want to fork the repository (usually your personal account).

Once forked, you will be working from your own copy of the repository.

### Option 2: Use the Original Repository Directly (Secrets Configured in Your Account)

You can launch Codespaces directly from the original public repository without forking. In this case, you will configure the necessary Codespaces secrets within *your own GitHub account*, scoped to this repository.

**Important:** Ensure that the necessary Adyen secrets are configured in your *personal GitHub account's* Codespaces secrets, with the repository path (e.g. `adyen-examples/adyen-node-online-payments`) as the scope.

## Step 1: Adyen Account Setup

Before launching the Codespace, you need to configure a few things in your Adyen Customer Area.

1. **API Credentials:**
   * If you don't have one, create a set of [API credentials](https://docs.adyen.com/development-resources/api-credentials/) for your test environment.
   * You will need the following values:
     * **API Key**
     * **Client Key**
     * **Merchant Account**

2. **Allowed Origins:**
   * To allow the Adyen UI components to load in the Codespace, you need to add the Codespace URL to your list of allowed origins.
   * Navigate to your API credentials, and in the **Client settings**, add the following URL to the **Allowed origins** list:
     ```
     https://*.github.dev
     ```

3. **Webhook HMAC Key:**
   * In your Customer Area, go to **Developers > Webhooks**. 
   * Create a new **Standard webhook**.
   * Generate an **HMAC Key**. You will need this for the next step.

## Step 2: Configure GitHub Codespaces Secrets

To securely use your Adyen credentials in the Codespace, you should set them as repository secrets. The dev container is configured to automatically pick them up.

1.  Depending on your chosen setup in Step 0:
    *   **If you forked the repository:** Navigate to your forked repository on GitHub and go to **Settings > Secrets and variables > Codespaces**.
    *   **If you are using the original repository directly:** Navigate to your *personal GitHub account* settings and go to **Settings > Codespaces > Secrets**. Create the secrets there, ensuring you set the **Repository access** to "Selected repositories" and add the specific Adyen example repository (e.g., `adyen-examples/adyen-node-online-payments`) as a selected repository.
2.  Create the following secrets:
    *   `ADYEN_API_KEY`: Your Adyen API Key.
    *   `ADYEN_CLIENT_KEY`: Your Adyen Client Key.
    *   `ADYEN_MERCHANT_ACCOUNT`: Your Adyen Merchant Account.
    *   `ADYEN_HMAC_KEY`: The HMAC key you generated during webhook setup.

## Step 3: Launching the Codespace

1.  Navigate to the `README.md` file of the example you want to run (e.g., `checkout-example/README.md`).
2.  Click the **Open in GitHub Codespaces** badge.
3.  This will create and launch a new Codespace. The project will be built automatically, and the application will start.
4.  The application will typically run on port 8080 (or as configured in the specific example's devcontainer)
5.  Use the Drop-in and try out payments on `TEST`

## Step 4: Configure the Webhook URL

Once the Codespace is running, you need to update your Adyen webhook with the public URL of the running application.

1.  In the Codespace, a terminal will open, and the application will be running on the configured port (typically 8080).
2.  Go to the **Ports** tab in the VS Code interface within your Codespace.
3.  Find the entry for the application port and copy the **Forwarded Address** (it will look something like `https://my-codespace-name-8080.preview.app.github.dev`).
4.  Go back to your Adyen Customer Area and navigate to **Developers > Webhooks**.
5.  Select the webhook you created earlier and paste the copied URL into the **Server configuration** URL field, appending the webhook endpoint:
    ```
    <your_forwarded_address>/api/webhooks/notifications
    ```
    For example:
    ```
    https://my-codespace-name-8080.preview.app.github.dev/api/webhooks/notifications
    ```
6.  Ensure the webhook is **enabled** and save your changes.
7.  Make sure to change visibility of your port to `public` to make the Webhook endpoint reachable by Adyen

>   You are now ready to test the integration within GitHub Codespaces!

## Available Examples

Adyen provides example projects in multiple programming languages, each with similar functionality:

### Programming Languages Supported
- **Node.js** - `adyen-node-online-payments`
- **Java Spring** - `adyen-java-spring-online-payments` 
- **Python** - `adyen-python-online-payments`
- **.NET** - `adyen-dotnet-online-payments`
- **PHP** - `adyen-php-online-payments`

### Common Example Types
Most Adyen example repositories include these payment integration examples:

- **Checkout Example** - Basic e-commerce checkout flow with different payment methods
- **Advanced Checkout Example** - Advanced 3-step checkout flow using the Checkout API
- **3DS2 Example** - 3D Secure 2 authentication with Native and Redirect flows
- **Authorisation Adjustment Example** - Payment adjustments, captures, and reversals
- **In-person Payments Example** - POS terminal payments using the Terminal API
- **Gift Card Example** - Gift card payments with partial orders
- **Pay by Link Example** - Pay by link flow for creating payment links
- **Subscription Example** - Subscription payments using Adyen tokenization
- **Giving Example** - Donation flow using Adyen Giving

> **Note:** The specific examples available may vary by programming language. Check the individual repository's README for the complete list of examples.

## Usage Tips

- **Updating Secrets**: After updating a secret, it's best to delete the existing Codespace and create a new one to ensure the changes are applied.
- **Port Forwarding**: Make sure the application port (typically `8080`) has its visibility set to **Public** in the `Ports` tab.
- **Testing Webhooks**: Use the **Webhook Test Configuration** option from your Customer Area to send test events directly to your Codespace.
- **Viewing Logs**: Monitor the application logs in the **Terminal** panel for debugging and status updates.
- **Managing Codespaces**: You can view and manage all your Codespaces at [github.com/codespaces](https://github.com/codespaces).

## Language-Specific Considerations

### Node.js Projects
- Applications typically run on port 8080
- Use `npm install` and `npm run dev` commands
- Environment variables are loaded from `.env` files or Codespaces secrets

### Java Spring Projects  
- Applications typically run on port 8080
- Use `./gradlew bootRun` or `mvn spring-boot:run` commands
- Configuration is usually in `application.properties` or `application.yml`

### Python Projects
- Applications may run on different ports (check the specific example)
- Use `pip install -r requirements.txt` and `python app.py` commands
- Environment variables are loaded from `.env` files or Codespaces secrets

### .NET Projects
- Applications typically run on port 5000 or 8080
- Use `dotnet restore` and `dotnet run` commands
- Configuration is usually in `appsettings.json`

### PHP Projects
- Applications typically run on port 8000 or 8080
- Use `composer install` and `php -S` commands
- Configuration is usually in `.env` files

> **Tip:** Each example's README will contain specific instructions for that programming language and framework.

## Troubleshooting

### Environment Variables Not Set
If you see an error about missing environment variables, make sure you've set all four required secrets in your repository's Codespaces settings.

### Port Not Accessible
If the application doesn't start or you can't access it:
1. Check the terminal for any error messages
2. Ensure the port is properly forwarded in the **Ports** tab
3. Try restarting the Codespace
4. Check the specific example's README for the correct port number

### Webhook Not Receiving Notifications
1. Verify the webhook URL is correctly set in your Adyen Customer Area
2. Check that the webhook is enabled
3. Ensure the HMAC key matches what you set in the Codespaces secrets
4. Make sure the port visibility is set to **Public**

### Application Won't Start
1. Check the terminal for build or dependency errors
2. Ensure all required environment variables are set
3. Try rebuilding the Codespace (delete and recreate)
4. Check the example's README for specific setup requirements

### Language-Specific Issues
- **Node.js**: Check `package.json` for correct scripts and dependencies
- **Java**: Ensure Java version matches the project requirements
- **Python**: Check Python version and virtual environment setup
- **.NET**: Verify .NET SDK version compatibility
- **PHP**: Check PHP version and extension requirements

For more help, check the specific example's README or the Adyen documentation for your chosen programming language.
