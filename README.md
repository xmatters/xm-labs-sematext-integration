# Sematext xMatters Integration

Create xMatters Alerts from Sematext

<kbd>
  <img src="https://github.com/xmatters/xMatters-Labs/raw/master/media/disclaimer.png">
</kbd>

# Pre-Requisites

- Sematext
- xMatters account - If you don't have one, [get one](https://www.xmatters.com)!

# Files

- [xMatters Sematext Communication Plan](ServiceNowUpdates/xMattersConfig-script_include.xml)
- [xMattersIncident Script Include update XML](ServiceNowUpdates/xMattersIncident-script_include.xml)

# How it works

Sematext will create an xMatters alert when new Logs or Events in Sematext meet specific conditions.

# xMatters Configuration

### Create an xMatters Integration User

Although this step is not required, it is recommended. This will enhance security and ensure the integration does not unintentionally break when a password is changed.

**Note**: If you are installing this integration into an xMatters trial instance, you don't need to create a new user. Instead, locate the "Integration User" sample user that was automatically configured with the REST Web Service User role when your instance was created and assign them a new password. You can then skip ahead to the next section.

**To create an integration user:**

1. Log in to the target xMatters system.
2. On the **Users** tab, click the **Add New User** icon.
3. Enter the appropriate information for your new user.
   Example User Name **Sematext_API_User**
4. Assign the user the **REST Web Service User** role.
5. Click **Save**.

<br><br>

## Import and Configure the xMatters Communication Plan

To import the communication plan:

1. In the target xMatters system, on the **Developer** tab, click **Import Plan**.
2. Click **Browse**, and then locate the downloaded communication plan: [Sematext Alerts](SematextAlerts.zip)
3. Click **Import Plan**.
4. Once the communication plan has been imported, click **Plan Disabled** to enable the plan.
5. In the **Edit** drop-down list, select **Access Permissions**.

<kbd>
    <img src="/media/access_permissions.png" width="300">
</kbd>
<br><br>

6. Add the **Sematext_API_User** or Make the Plan **Accessible by All**.
7. **Save Changes**.

<kbd>
    <img src="/media/set_access_permissions.png" width="300">
</kbd>
<br><br>

8. In the **Edit** drop-down list, select **Forms**.
9. For the **Sematext Alert** form, in the **Not Deployed** drop-down list, click **Enable for Web Service**.

<kbd>
    <img src="/media/enable_ws.png" width="300">
</kbd>
<br><br>

10. After you Enable for Web Service, the drop-down list label will change to **Web Service**.
11. In the **Web Service** drop-down list, click **Sender Permissions**.
12. Add the **Sematext_API_User** you created above, and then click **Save Changes**.

<kbd>
    <img src="/media/sender_permissions.png" width="300">
</kbd>

<br><br>

## Get the xMatters Inbound Integration Endpoint URL

1. In the **Edit** drop down list on the _Sematext Alerts Communication Plan_ Click **Flow**.
2. Click on the _Sematext Alert_ Flow.

<kbd>
    <img src="/media/access_flow.png">
</kbd>

3. On the Flow Canvas, Double Click on the **Inbound Sematext Trigger** step.

<kbd>
    <img src="/media/trigger_step.png">
</kbd>

4. Change the _Authenticating User_ to the **Sematext_API_User**

You must supervise this user and it must have a Web Service User Role. If you cannot select the **Sematext_API_User** if it because you do not supervise that user or they do not have the REST Web Service User role. When you create a new user you are automatically set as that users supervisor. If you do not see the user that you created, most likely the REST Web Service role is missing.

5. Copy the URL listed under the **Trigger** section.

You will need this URL for configuring Sematext.

  <kbd>
    <img src="/media/trigger_url.png">
  </kbd>

<br><br>

## Customize your xMatters Flow

By default, this integration will create an xMatters Alert with basic property values from Sematext. You can customize this integration to include more properties and also add additional Flow Steps to post to chat ops tools, create tickets or anything else you can dream of.

**Note:**:If you customize the flow, just make sure that you do not remove the **xMatters Create Event (Sematext Alert)** Step. This step creates the actual xMatters alert. This step can come right after Create Sematext Alert Step or after other steps that you add to the canvas, it's up to you.

_Here is an example of a possible Flow:_
<kbd>
<img src="/media/sample_flow.png">
</kbd>

## Customize **Inbound Sematext Trigger** Step

1. In the Triggers tab under HTTP Requests section, click the Cog and then Edit.

<kbd>
    <img src="/media/edit_trigger.png">
</kbd>
<br><br>

2. Go to the **OUTPUTS** Tab and add new **Step Outputs**. Outputs are values that will be available for subsiquent steps.

<kbd>
    <img src="/media/create_outputs.png" width="550px">
</kbd>
<br><br>

3. Go to the **SCRIPT** tab and modify your script appropriately.

- Each Output added in Step 2 will need to be mapped to a value in your payload. This takes the incoming values from Sematext and sets the xMatters properties to hold these values.

Example: _output['New Output'] = payload['Another value'];_

    output['New Output'] - this is an output added in step 2. The output name is "New Output"
    payload['Another value'] - this is a value in your payload with the key "Another value" sent to xMatters from Sematext.

<kbd>
    <img src="/media/edit_script.png" width="650px">
</kbd>
<br><br>

## Add additional / custom properties to the Sematext Alert Form.

1. Exit Flow Designer.

2. Navigate to the Forms section of Sematext Communication Plan.

3. Beside Sematext Alert Form, click **Edit** and go to **Layout**.

<kbd>
    <img src="/media/sematext_alert_form.png">
</kbd>
<br><br>

4. Create new Properties.

<kbd>
    <img src="/media/add_property.png">
</kbd>

5. Drag new Properties onto the Form.

6. Save Changes.

<kbd>
    <img src="/media/form_layout.png" width="650px">
</kbd>
<br><br>

7. Make any required changes to **Messages** and **Responses**.

<kbd>
    <img src="/media/messages_responses.png" width="350px">
</kbd>

<br><br>

## Customize **xMatters Create Event (Sematext Alert)** Step

Once you have added new properties to the Sematext Alert form, they will become available in the **xMatters Create Event (Sematext Alert)** Step for you to map payload properties to.

1. On the Flow Canvas double click the **xMatters Create Event (Sematext Alert)** Step

<kbd>
    <img src="/media/create_event_step.png">
</kbd>
<br><br>

2. Drag items from the right to the appropriate fields on the left. Any new fields you have added to the form layout will be available here.

<kbd>
    <img src="/media/custom_create_event.png" width="650px">
</kbd>
<br><br>

# Configuring Sematext

## Create a New Notification Hook

Follow instructions here to create new Custom Notification Hooks in Sematext:
https://sematext.com/docs/integration/alerts-webhooks-integration/

Here is how you should configure each value:

**Name**: xMatters Alert

**URL**:
Set the URL (hook url) field to the xMatters Inbound Integration Endpoint URL.
[Get the xMatters Inbound Integration Endpoint URL here](#get-the-xmatters-inbound-integration-endpoint-url)

**Set Data as**: JSON

**HTTP method**: POST

**Add Parameters**:

description = $description
<br>
title = $title
<br>
app_id = \$app_id
<br><br>

Add any additional parameters / values you want to pass to xMatters

<kbd>
    <img src="/media/webhook.png">
</kbd>
<br><br>

## Create New Alert Rule

Follow instructions here to create new Alert Rule:
https://sematext.com/docs/alerts/

Configure your alert rule to use the Custom Notification Hook created in the last step.

<kbd>
    <img src="/media/alert_rule.png">
</kbd>

<br><br>

# Troubleshooting

Trigger a new Sematext Alert and check that it makes its way into xMatters.

You can check the Inbound Integration Activity Log in xMatters:
https://help.xmatters.com/ondemand/xmodwelcome/integrationbuilder/create-inbound-updates.htm
