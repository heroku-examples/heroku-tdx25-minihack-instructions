# TDX25 - Heroku at Camp Mini Hacks - Instructions

> [!IMPORTANT]
> These instructions are work in progress and intended to be integrated into an upcoming TDX 2025 minihack.

## Use Case and Why Heroku?

With Heroku AppLink, we're enhancing the following agents with Heroku-powered Agentforce Actions, allowing Apex, Flow, and Agentforce to run custom code to perform complex, compute-intensive calculations on Heroku’s scalable managed infrastructure.

> [!NOTE]
> The steps below do not require you to build or deploy the associated Heroku application, this has already been done for you. However if you want to later review the source code for these actions you can do so through [this](https://github.com/heroku-examples/heroku-tdx25-minihack-code) repository.

### Astro Airlines Travel Agent
bla
<p><img src="images/astro-airlines-action.jpg" width="70%">

### Trailblazer Outfitters Retail Agent
bla
<p><img src="images/outfitters-retail-action.jpg" width="70%">

### Koa Car Agent
This Action dynamically evaluates real-time car valuations from industry sources (AutoTrader, Edmunds, KBB), assesses user credit status via finance APIs, and optimizes business margins while ensuring competitiveness against other car sellers. By leveraging Heroku’s scalable processing power, Agentforce-powered agents can make real-time, financing decisions, delivering personalized finance offers within Salesforce and empowering both dealers and buyers with transparent, competitive financing options.
<p><img src="images/koa-car-action.jpg" width="70%">

## Requirements

- Access to a Salesforce Org with Agentforce enabled
- Access to a Heroku environment enrolled in the `tdx25-minihack` Heroku team
- Latest Salesforce CLI installed [link](https://developer.salesforce.com/docs/atlas.en-us.sfdx_setup.meta/sfdx_setup/sfdx_setup_install_cli.htm)
- Latest Heroku CLI installed [link](https://devcenter.heroku.com/articles/heroku-cli#install-the-heroku-cli)
- Latest Heroku AppLink CLI Plugin installed [link](https://devcenter.heroku.com/articles/heroku-integration-cli)

## Steps

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

    <img src="images/applink.jpg" width="80%">

    <img src="images/operation.jpg" width="80%">

    Congrulations you have just brought the power of Heroku into your Salefsorce org and to the finger tips of your developers and admins!

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
    sf project deploy start -o my-org
    ```

    > [!NOTE]
    > Support for Heroku applications without Flow wrappers is currently being rolled out. This step will be removed once this is complete. The goal is to have this step removed ahead of Salesforce TDX.

4. **Creating an Agentforce Action**

    Your Heroku application is now availble to Apex and Flow users to use as they would any other platform action.

    To create an Agentforce Action search for **Agent Actions** under **Setup** to navigate to the **Agent Actions** page.

    Click **New Agent Action** in the top right corner, select **API**, then **Heroku** and search for `ActionsService`.

    Select the action and click **Next**, complete the checkboxes as shown below and click **Finish** 

    <img src="images/addaction.jpg" width="80%">

5. **Adding an Action to an Agent**

    Navigate **Agents** under the **Setup** menu and open the *Einstein Copilot** agent in **Agent Builder**.

    Go to the **Customer Service Assistant**, click on the **Topics** tab and click **New**, selecting **From Assest Library**

    Locate the **ActionsService** and add it to the topic.
    
6. **Testing your Heroku Action**

    In **Agent Builder** enter the following to invoke your action.

    `
    My customer XYX123 is wanting to purchase a car ABC456. The price is $25,000 and they have a downpayment of $1000. The interest rate is 5% and they want the term to be over 3 years. Can you give me the finance information for this and let me know if it is competitive with other services?
    `

    <img src="images/agent.jpg" width="80%">