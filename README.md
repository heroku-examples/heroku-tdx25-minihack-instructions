# TDX25 - Heroku at Camp Mini Hacks - Instructions

> [!IMPORTANT]
> These instructions will be integrated into an upcoming TDX 2025 mini-hack.

In this workshop, we're enhancing several agents with custom code written in languages of your choice, using **Heroku** and [Heroku AppLink](https://devcenter.heroku.com/articles/getting-started-heroku-integration). This allows **Apex**, **Flow**, and **Agentforce** to perform complex, compute-intensive calculations on Heroku’s scalable managed infrastructure.

> [!NOTE]
> The steps below do not require you to build or deploy the associated Heroku application, as this has already been done for you. However, if you want to review the source code for these actions later, you can do so through [this](https://github.com/heroku-examples/heroku-tdx25-minihack-code) repository.

## Requirements

- Access to a Salesforce Org with Agentforce enabled and the TDX mini-hack objects and apps deployed.
- Access to a Heroku environment enrolled in the `tdx25-minihack` Heroku team.
- The latest Salesforce CLI installed [link](https://developer.salesforce.com/docs/atlas.en-us.sfdx_setup.meta/sfdx_setup/sfdx_setup_install_cli.htm).
- The latest Heroku CLI installed [link](https://devcenter.heroku.com/articles/heroku-cli#install-the-heroku-cli).
- The latest Heroku AppLink CLI Plugin installed [link](https://devcenter.heroku.com/articles/heroku-integration-cli).
- The `git` CLI

## Steps for All Agents

1. **Logging into Heroku and Cloning the Repository**

    Log in to Heroku and confirm that you have access to the required Heroku team:
    
    ```sh
    heroku login
    heroku teams
    ```

    The `heroku teams` command should show `tdx25-minihack` in your list of teams:

    ```sh
    Team             Role         
    ──────────────── ──────────── 
    tdx25-minihack   collaborator            
    ```
    Clone this repository and change to the directory created: 

    ```sh
    git clone https://github.com/heroku-examples/heroku-tdx25-minihack-instructions
    cd heroku-tdx25-minihack-instructions
    ```

2. **Connecting Heroku to Your Org**

    Run the command below. When prompted, enter the username and password for your org and accept the required permissions prompt:
    
    ```sh
    heroku salesforce:connect my-org-yourname --app tdx25-minihack-actions --store-as-run-as-user 
    ```

    > Replace `yourname` in the command above. For example, for Chris Wall, use `my-org-cwall`.

3. **Linking the Heroku Application with Your Org**

    The action code uses **Heroku AppLink** to seamlessly access data within the org. Run the following command to link the Heroku app to your org:
    
    ```sh
    heroku salesforce:import api-docs.yaml --org-name my-org-yourname --app tdx25-minihack-actions --client-name ActionsService
    ```

    > As per the last step, be sure to edit `my-org-yourname` in the command above.

    Navigate to **Heroku** under **Setup** to confirm that the application has been linked.

    <img src="images/applink.jpg">

    <img src="images/operation.jpg">

    In the following steps, you will configure an Agentforce Action for one of the above operations.

4. **Grant Permissions to the Heroku Application**

    Ensure your Salesforce user has permission to invoke the application logic.

    ```sh
    sf org assign permset --name ActionsService -o my-org
    ```

    > The above command assumes you have already authenticated your org with the `sf` CLI using an alias of `my-org`. If this is not the case, use the `sf org login web --alias my-org` command to authenticate and then run the above command.

5. **Deploy to Your Salesforce Org**

    Heroku actions need a small Flow wrapper to be used with Agentforce. Deploy these using the command below:

    ```sh
    sf project deploy start --metadata Flow -o my-org
    ```

    > Support for using Heroku applications directly with Agentforce is being rolled out to our service. This step will be removed once this is complete.

## Add an Action to the Astro Airlines Travel Agent

This action uses flight information from Salesforce and CO₂ emissions data to estimate the carbon footprint of a flight. Heroku integrates in real-time with environmental data APIs (ICAO Carbon Emissions Calculator, Google Flights API, IATA CO₂ Data), collates and queries flight data in memory, and calculates CO₂ emissions per route, traveler, aircraft type, and distance traveled.

1. **Creating an Agentforce Action**

    To create an Agentforce Action, search for **Agent Actions** under **Setup** to navigate to the **Agent Actions** page. Click **New Agent Action** in the top right corner, select **Flow**, search for **Calculate Carbon Footprint**, select the action, and click **Next**. Complete the checkboxes as shown below and click **Finish**. 

    <img src="images/agent-action-carbon-calc.jpg">

2. **Adding an Action to an Agent**

    Locate **Agents** under the **Setup** menu, click **Koa Car Agent**, and click **Open in Builder**. Click on the **Topics** tab and **Koa Cars Sales Agent**. In the **This Topic's Actions** tab, click **New** > **Add from Asset Library**. Search for **Calculate Carbon Footprint**, select it, and click **Finish**.
    
3. **Testing Your Heroku Action**

    In **Agent Builder**, enter the following to invoke your action:

    ```
    What is the total carbon footprint for my upcoming flight?
    ```

    <img src="images/agent-response-carbon-calc.jpg">

## Add an Action to the Trailblazer Outfitters Retail Agent

This action retrieves product information, including size and weight. It dynamically calls APIs of approved shipping companies, collates the shipping costs and timelines in memory, and returns them to Agentforce along with a recommendation.

1. **Creating an Agentforce Action**

    To create an Agentforce Action, search for **Agent Actions** under **Setup** to navigate to the **Agent Actions** page. Click **New Agent Action** in the top right corner, select **Flow**, search for **Calculate Shipping Options**, select the action, and click **Next**. Complete the checkboxes as shown below and click **Finish**. 

    <img src="images/agent-action-shipping-calc.jpg">

2. **Adding an Action to an Agent**

    Locate **Agents** under the **Setup** menu, click **Koa Car Agent**, and click **Open in Builder**. Click on the **Topics** tab and **Koa Cars Sales Agent**. In the **This Topic's Actions** tab, click **New** > **Add from Asset Library**. Search for **Calculate Shipping Options**, select it, and click **Finish**.

3. **Testing Your Heroku Action**

    Enter the following in **Agent Builder**:

    ```
    Can you recommend shipping options to get my item within a week for under $50?
    ```

    <img src="images/agent-response-shipping-calc.jpg">

## Add an Action to the Koa Car Agent

This action evaluates real-time car valuations from industry sources (AutoTrader, Edmunds, KBB), assesses user credit status via finance APIs, and optimizes business margins while ensuring competitiveness. By leveraging Heroku’s scalable processing power, Agentforce-powered agents can make real-time financing decisions, delivering personalized finance offers within Salesforce.

1. **Creating an Agentforce Action**

    To create an Agentforce Action, search for **Agent Actions** under **Setup** to navigate to the **Agent Actions** page. Click **New Agent Action** in the top right corner, select **Flow**, search for **Calculate Finance Agreement**, select the action, and click **Next**. Complete the checkboxes as shown below and click **Finish**. 

    <img src="images/agent-action-finance-calc.jpg">

2. **Adding an Action to an Agent**

    Locate **Agents** under the **Setup** menu, click **Koa Car Agent**, and click **Open in Builder**. Click on the **Topics** tab and **Koa Cars Sales Agent**. In the **This Topic's Actions** tab, click **New** > **Add from Asset Library**. Search for **Calculate Finance Agreement**, select it, and click **Finish**.

3. **Testing Your Heroku Action**

    Enter the following in **Agent Builder**:

    ```
    I want to buy a car with a $1,000 down payment, a max 5% interest rate, and a term over 3 years. Can you provide a competitive finance estimate?
    ```

    <img src="images/agent-response-finance-calc.jpg">
