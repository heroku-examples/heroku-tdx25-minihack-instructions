# TDX25 - Heroku at Camp Mini Hacks - Instructions

> [!IMPORTANT]
> These instructions are work in progress and intended to be integrated into an upcoming TDX 2025 minihack.

## Use Case and Why Heroku?

In this workshop we're enhancing several agents with custom code written in languages of your choice, with **Heroku** and [Heroku AppLink](https://devcenter.heroku.com/articles/getting-started-heroku-integration), allowing **Apex**, **Flow**, and **Agentforce** to perform complex, compute-intensive calculations on Heroku’s scalable managed infrastructure.

> [!NOTE]
> The steps below do not require you to build or deploy the associated Heroku application, this has already been done for you. However if you want to later review the source code for these actions you can do so through [this](https://github.com/heroku-examples/heroku-tdx25-minihack-code) repository.

### Astro Airlines Travel Agent
bla
<p><img src="images/agent-response-karbon-calc.jpg">

### Trailblazer Outfitters Retail Agent
bla
<p><img src="images/agent-response-shipping-calc.jpg">

### Koa Car Agent
This Action dynamically evaluates real-time car valuations from industry sources (AutoTrader, Edmunds, KBB), assesses user credit status via finance APIs, and optimizes business margins while ensuring competitiveness against other car sellers. By leveraging Heroku’s scalable processing power, Agentforce-powered agents can make real-time, financing decisions, delivering personalized finance offers within Salesforce and empowering both dealers and buyers with transparent, competitive financing options.
<p><img src="images/agent-response-finance-calc.jpg">

## Requirements

- Access to a Salesforce Org with Agentforce enabled
- Access to a Heroku environment enrolled in the `tdx25-minihack` Heroku team
- Latest Salesforce CLI installed [link](https://developer.salesforce.com/docs/atlas.en-us.sfdx_setup.meta/sfdx_setup/sfdx_setup_install_cli.htm)
- Latest Heroku CLI installed [link](https://devcenter.heroku.com/articles/heroku-cli#install-the-heroku-cli)
- Latest Heroku AppLink CLI Plugin installed [link](https://devcenter.heroku.com/articles/heroku-integration-cli)

## Steps for All Agents

1. **Logging into Heroku**

    Login to Heroku and confirm you have access to the required Heroku team:
    
    ```
    heroku login
    heroku teams
    ```

    The `heroku teams` command should show `tdx25-minihack` in your list of teams

    ```
    Team             Role         
    ──────────────── ──────────── 
    tdx25-minihack   collaborator            
    ```

2. **Connecting Heroku to your Org**

    Run the command below and when prompted enter the user and password for your org and accept the required permissions prompt:
    
    ```
    heroku salesforce:connect my-org-yourname --app tdx25-minihack-actions --store-as-run-as-user 
    ```

    > Replace `yourname` in the command above, for example, for Chris Wall use `my-org-cwall`

3. **Linking the Heroku application with your Org**

    We have predeployed the action code to Heroku for you, however if you want to review it you can do so [here](https://github.com/heroku-examples/heroku-tdx25-minihack-code).
    
    The action code uses Heroku AppLink to seamlessly access customer and car data within the org, as well as several external services.
    
    Run the following command to link the Heroku appc containing several actions with your org:
    
    ```
    heroku salesforce:import api-docs.yaml --org-name my-org-yourname --app tdx25-minihack-actions --client-name ActionsService
    ```

    > As per the last step be sure to edit `my-org-yourname` in the command above

    Navigate to **Heroku** under the **Setup** to confirm the application has been linked.

    <img src="images/applink.jpg">

    <img src="images/operation.jpg">

    In the following steps you will configure an Agentforce Action for one of the above operations.

4. **Grant Permissions to the Heroku application**

    Ensure your Salefsorce user has permission to invoke the applicaiton logic.

    ```
    sf org assign permset --name ActionsService -o my-org
    ```
    > The above command assumes you have already authenticated your org with the `sf` CLI using an alias of `my-org`. If this is note the case use the `sf org login web --alias my-org` command to authenticate and then run the above command.

5. **Deploy to your Salesforce org**

    At this time Heroku actions need a small Flow wrapper in order to be accessible from Agentforce, deploy these using the command below:

    ```
    sf project deploy start --metadata Flow -o my-org
    ```

    > [!NOTE]
    > Support for Heroku applications without Flow wrappers is currently being rolled out. This step will be removed once this is complete. The goal is to have this step removed ahead of Salesforce TDX.

## Add an action to the Astro Airlines Travel Agent

1. **Step**

    bla

## Add an action to the Trailblazer Outfitters Retail Agent

1. **Step**

    bla

## Add an action to the Koa Car Agent

1. **Creating an Agentforce Action**

    To create an Agentforce Action search for **Agent Actions** under **Setup** to navigate to the **Agent Actions** page.

    Click **New Agent Action** in the top right corner, select **Flow**, search for **Calculate Finance Agreement**.

    Select the action and click **Next**, complete the checkboxes as shown below and click **Finish** 

    <img src="images/agent-action-finance-calc.jpg">

2. **Adding an Action to an Agent**

    Locate **Agents** under the **Setup** menu, click **Koa Car Agent**, and click **Open in Builder**.

    Click on the **Topics** tab, and **Koa Cars Sales Agent**, in the **This Topic's Actions** tab, click New > Add from Asset Library.

    Search for **Calculate Finance Agreement**, select it and click **Finish**.
    
3. **Testing your Heroku Action**

    In **Agent Builder** enter the following to invoke your action.

    `
    I want to buy a car with a $1,000 down payment, a max 5% interest rate, and a term over 3 years. Can you provide a competitive finance estimate?
    `

    <img src="images/agent-response-finance-calc.jpg">
