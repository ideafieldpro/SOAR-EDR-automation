# SOAR & EDR Automation

## Objective

This project demonstrates the implementation of Security Orchestration, Automation and Response (SOAR) capabilities integrated with Endpoint Detection and Response (EDR) using Lima Charlie and Tines. The focus was on creating automated workflows for security incident detection and response.

### Skills Learned

- Configuration and deployment of Lima Charlie EDR
- Creation of custom detection rules
- Integration of automation workflows using Tines
- Development of incident response playbooks
- Implementation of automated notification systems

### Tools Used

- **Lima Charlie** for EDR capabilities
- **Tines** for automation and orchestration
- [**Draw.io**](http://Draw.io) for workflow documentation
- **PowerShell** for system administration
- **Slack** for notifications

## Part 1

### Step 1: Design the Workflow in Draw.io

1. **Open Draw.io:**
    - Go to [Draw.io](https://draw.io/) and start a new diagram.
2. **Create the Initial Square:**
    - Use a square shape to signify the start of the workflow.
    - Label it "SORE EDR Playbook."
3. **Add Detection and Notification Steps:**
    - Create a rounded rectangle for LimaCharlie detection.
        - Label it "Lima Charlie detects hack tool."
    - Add a step for Tines receiving the detection.
        - Label it "Tines receives detection."
    - Create two branches from Tines:
        - One leading to Slack notification.
        - Another leading to email notification.
4. **User Prompt for Isolation:**
    - Add a rectangle labeled "User Prompt: Isolate Machine?"
    - Create branches for user responses ("Yes" and "No").

### Step 2: Define Actions Based on User Input

1. **If User Selects 'Yes':**
    - Duplicate the LimaCharlie action.
    - Label it "LimaCharlie isolates machine."
    - Add a notification step back to Slack confirming isolation status.
2. **If User Selects 'No':**
    - Add a message to Slack stating, "Computer was not isolated; please investigate."

### Step 3: Finalize the Workflow

- Review your workflow in Draw.io for accuracy.
- Ensure all steps are clearly labeled and logical.
- Save your diagram for future reference.
  ![SOAR and EDR diagram drawio](https://github.com/user-attachments/assets/5bbd4ed5-85f6-4ee4-89a4-f11613454668)


### Part 1 Commentary

- **Why make a logical diagram?**
  - Logical diagrams are crucial for enhancing understanding, improving security measures, facilitating communication, and supporting compliance efforts in the field of cybersecurity.

## Part 2

### Step 1: Deploy a Windows Server Instance

1. **Log in to your Cloud Provider:**
    - Access your Cloud Provider account.
    - Click on "Deploy New Server."
2. **Select Server Options:**
    - Choose "Cloud Compute" and select "Shared CPU."
    - Select a location for your server that is suitable for you.
3. **Choose Operating System:**
    - Under the operating system options, select "Windows Standard" (latest version).
    - Review the pricing plans and select the most cost-effective option.
4. **Configure Server Settings:**
    - Disable auto-backups if desired.
    - Remove IPv6 settings if unnecessary.
    - Set a suitable hostname (e.g., `ideafieldsec-SOAR-EDR`).
5. **Deploy the Server:**
    - Click on "Deploy Now" and wait approximately 15 minutes for the server to be ready.
      ![chrome_6jzjYAcpmu](https://github.com/user-attachments/assets/a1ffbf14-cd82-4525-9182-b52f68cd47d1)


### Step 2: Create an Organization in Lima Charlie

1. **Access LimaCharlie:**
    - Navigate to [Lima Charlie's website](https://limacharlie.io/) and sign in/up using your chosen email provider.
2. **Verify Your Email:**
    - Follow the instructions to verify your email address.
3. **Create an Organization:**
    - Click the blue button to create a new organization.
    - Enter an organization name (e.g., `ideafieldsec`) and select the data residency region closest to you.
    - Click "Create Organization."
      ![chrome_REhK6fIAfA](https://github.com/user-attachments/assets/c390124b-a863-4133-9d84-78ce1a8b0e47)


### Step 3: Set Up Firewall Rules

1. **Access Server Firewall Settings:**
    - Navigate to the firewall settings for your deployed server from your cloud provider.
2. **Add Firewall Rules:**
    - Create a new firewall group.
    - Configure the firewall to allow Microsoft Remote Desktop Protocol (RDP) on port 3389 from your IP address, if you wish to use RDP to access the server.
3. **Update Firewall Group:**
    - Ensure all changes are applied and wait for up to 120 seconds for them to take effect.
      ![chrome_zdgCrL53bu](https://github.com/user-attachments/assets/878bf2da-6845-4285-91b6-57deca6e113d)


### Step 4: Install LimaCharlie Agent on Windows Machine

1. **Access Installation Keys:**
    - In LimaCharlie, navigate to the "Sensors" section and select "Installation Keys."
    - Click "Create Installation Key" and enter a description.
      ![chrome_nhCMpy7Y9y](https://github.com/user-attachments/assets/f6f75c52-4e6a-4818-8e9f-a74c280c7598)

2. **Download the Installation File:**
    - Go to the EDR section for Windows 64-bit and copy the download link for the Lima Charlie executable.
3. **Open PowerShell:**
    - On your Windows server, right-click on PowerShell and select "Run as Administrator."
4. **Navigate to Downloads Directory:**
    
    ```powershell
    cd Downloads
    dir
    ```
    
5. **Install the Agent:**
    
    ```powershell
    .\hcp_win_x64_release_4.32.2.exe -i <Your_Installation_Key>
    ```
    
    Replace `<Your_Installation_Key>` with the key you created earlier.
   ![chrome_OUpZ6IRmaq](https://github.com/user-attachments/assets/966f5ade-1f27-4e75-bc9a-77461b5b249d)


7. **Verify Installation:**
    - Check under "Services" in Windows to confirm that the LimaCharlie service is running.
      ![chrome_7w6FAoyDGn](https://github.com/user-attachments/assets/b04f18f0-55f7-4830-88c2-c7e828fc1c11)


### Step 5: Confirm Sensor Enrollment

1. **Review Sensors List:**
    - In Lima Charlie, go back to the "Sensors" section.
    - Confirm that your newly installed sensor appears in the list with relevant details (hostname, IP addresses).
      ![chrome_ViG6bsMEfr](https://github.com/user-attachments/assets/aabb0777-bb32-4898-b295-641e95378d5e)


### Step 6: Generate Security Events

1. **Event Generation:**
    - Use various commands via PowerShell or perform actions that generate security events (e.g., creating or modifying files).

### Part 2 Commentary

- Ensure that firewall rules are configured correctly to avoid connectivity issues with Lima Charlie.
- Regularly check for updates and best practices from Lima Charlie documentation to stay informed about new features and improvements.
- Use netstat and other monitoring tools within Lima Charlie to assess network activity and identify any suspicious behavior effectively.

## Part 3

### Step 1: Download the Lasagna Tool

1. Navigate to the [Lasagna GitHub releases page](https://github.com/your-repo-link/releases) from your Windows server.
2. Click on `lasagna.exe` under the latest release.
3. Note: You may need to disable Windows Security real-time protection:
    - Search for "Windows Security" in your Windows search bar.
    - Go to "Virus & threat protection."
    - Click on "Manage settings" under "Virus & threat protection settings."
    - Disable "Real-time protection."

### Step 2: Allow Lasagna to Bypass SmartScreen

1. If Windows blocks the download, click on the three dots next to the warning and select "Keep anyway."
2. Open your Downloads folder and locate `lasagna.exe`.
   ![chrome_eLYQp02k9u](https://github.com/user-attachments/assets/97a0bfeb-089e-40c6-8898-670940b78d9d)

### Step 3: Test Lasagna Tool with PowerShell

1. Open PowerShell:
    - Hold `Shift` and right-click in the Downloads folder to open PowerShell from this location.
    - Select "Open PowerShell window here."
2. Run the command:
    
    ```powershell
    .\lasagna.exe
    ```
    
3. Ensure it runs successfully by checking for any output.
   ![chrome_LNp7OYJta8](https://github.com/user-attachments/assets/76e4cdd7-0624-47c3-ab67-e017b66a9772)


### Step 4: Set Up LimaCharlie to Monitor Events

1. Log into your LimaCharlie account.
2. Navigate to **Sensors**:
    - Click on **Sensors List** and select the sensor corresponding to your machine.
3. Scroll down to the timeline and look for events generated by Lasagna.
   ![chrome_de9hbLEPq7](https://github.com/user-attachments/assets/554e7ef3-2ed3-4982-8131-9fe93b4ae66a)


### Step 5: Create a New Detection Rule

1. Go to **Automation > D&R Rules** in LimaCharlie.
2. Click on **New Rule**.
   - If you don’t see the rule, you can go to the Add-ons button at the top right of LimaCharlie and add the ext-sigma rules
3. Instead of building from scratch, search for an existing rule related to credential access tools by typing "credential."
4. Select a relevant credential dumping rule and click on it.
   ![chrome_u9wDHU2v5e](https://github.com/user-attachments/assets/5db13bb1-ca9b-4681-8b33-1236f9ee3d80)


### Step 6: Modify the Detection Rule

1. Click on the **Raw** button to view the rule's code.
  ![chrome_EzieyN8pHA](https://github.com/user-attachments/assets/942f4ba7-6033-46bd-97fa-b9818e5091a8)

2. Copy the entire rule code and paste it into a new rule window.
3. Edit the rule:
    - Replace references of `<name>.exe` with `lasagna.exe`.
    - Ensure case sensitivity is set to false.

### Example Code Snippet

Detect

```
events:
  - NEW_PROCESS
  - EXISTING_PROCESS
op: and
rules:
  - op: is windows
  - op: or
    rules:
      - case sensitive: false
        op: ends with
        path: event/FILE_PATH
        value: lazagne.exe
      - case sensitive: false
        op: ends with
        path: event/COMMAND_LINE
        value: all
      - case sensitive: false
        op: contains
        path: event/COMMAND_LINE
        value: lazange
      - case sensitive: false
        op: is
        path: event/HASH
        value: 467e49f1f795c1b08245ae621c59cdf06df630fc1631dc0059da9a032858a486
```

Respond

```
- action: report
  metadata:
    author: ideafieldsec
    description: Detects Lazagne (SOAR-EDR Tool)
    falsepositives:
      - Unknown
    level: medium
    tags:
      - attack.credential_access
  name: ideafieldsec - HackTool - Lazagne (SOAR-EDR)
```

### Step 7: Configure Response Actions

1. In the response section of your rule, set it up to report when conditions are met.
2. Assign appropriate metadata such as author name, description, and tags related to credential access.
   ![chrome_sS2rlDpC3R](https://github.com/user-attachments/assets/b4bd0f53-4839-4f49-9915-a1aa0bb15a42)


### Step 8: Save and Test Your Rule

1. Click **Save** to create the rule.
2. To test, copy an event that matches your criteria from the timeline.
3. Navigate back to your detection rule and click on **Test Event**.
4. Paste the copied event and evaluate the results.
   ![chrome_IWExnPY1iC](https://github.com/user-attachments/assets/dc69dfcb-41aa-42e2-8cb8-fe8a4215dd87)


### Part 3 Commentary

- **Best Practices:** Always document changes made to detection rules for future reference. Ensure you regularly review and update rules based on evolving threats.
- **Common Pitfalls:** Monitor for false positives; refine your criteria as needed. Be cautious about Windows Security settings, as they may interfere with testing.

## Part 4

### Step 1: Create a Slack Account

1. Navigate to [Slack.com](https://slack.com/).
2. Click on the **"Get Started Free"** button.
3. Enter a valid email address to receive a confirmation code.
4. Create your account and select **"Create a Workspace."**
    - **Workspace Name:** Enter a name such as `ideafieldsec`.
    - Click **Next**.
5. Set the display name for the workspace (e.g., `Demo`).
    - Skip the team invitation step.
6. Click **"Start Free Now"** to complete the setup.

### Step 2: Create an Alerts Channel

1. In the Slack interface, locate the **left-hand side panel**.
2. Click on **“Add Channels.”**
3. Select **“Create New Channel.”**
    - **Channel Name:** Enter `alerts`.
    - Set it as a **Public Channel**.
4. Skip inviting people to the channel.
5. Verify that your channel is visible.
   ![slack_jEg9JaskLN](https://github.com/user-attachments/assets/c31f1888-c172-4c5b-a428-a3b2cc23e3ff)


### Step 3: Sign into Tines (Threat Intelligence)

1. Go to [tines.c](https://tines.com/)[om](https://tin.com/).
2. Click on **“Sign In.”**
3. Choose to sign in, using your valid email account to facilitate confirmation.

### Step 4: Configure Lima Charlie Outputs

1. Log in to your Lima Charlie account.
2. Select your organization from the dashboard.
3. Click on **“Outputs.”**
4. Click on **“ADD Output.”**
5. From the options, select **“Detections.”**
  ![chrome_F87FT1Rap3](https://github.com/user-attachments/assets/a5b19d83-fd94-44ff-a71a-1a5ad124f0f3)

6. Choose your destination, Tines.
7. Set up the output:
    - **Name:** Enter something indicative, like `ideafieldsec-SOAR-EDR` .
    - **Webhook URL:** Paste the URL that you will receive from Tines.
      ![chrome_Y6c8UNNyTj](https://github.com/user-attachments/assets/5ca3a2b3-b100-49a6-bdea-603091aed68f)

8. Save your output settings.


### Step 5: Establish Webhook Action in Tines

1. Return to your Tines dashboard.
2. Create a new action by selecting **“Web Hook.”**
    - **Action Name:** Enter `Retrieve Detections`.
    - **Description:** Briefly describe its purpose `Retrieve LimaCharlie Detections`.

### Step 6: Test the Setup

1. Generate an event on your server using PowerShell (e.g., type `.\lasagna.exe all` in the command line).
2. Return to Lima Charlie and refresh the detection list.
  ![chrome_ArNQbRJL0U](https://github.com/user-attachments/assets/5e53db3b-62ab-42bf-91c5-0f7b7796882b)
3. Check the `Webhook` box in Tines for incoming messages related to the detection.
   ![chrome_flVvIVPT9n_mod](https://github.com/user-attachments/assets/504d259d-949f-499c-8c90-7497b450b252)

### Part 4 Commentary

- Establishing a dedicated alerts channel in Slack helps streamline communication among security analysts, allowing them to respond quickly to incidents.
- Using webhooks is an efficient way to connect Lima Charlie's detection capabilities with Slack, ensuring real-time alerts.
- Common pitfalls include failures in webhook configuration; ensure URLs are accurate and that Slack permissions allow messages from integrations.

## Part 5

### Step 1: Review Existing Workflow

1. **Analyze Previous Parts:** Ensure you understand the workflows created in the previous sections. This includes confirming detection of infected hosts.
2. **Retrieve Detections:**
    - Click on **Retrieve Detection** in Lima Charlie.
    - Ensure events are visible; look for relevant detection events.

### Step 2: Create Slack and Tines Integration

1. **Add Tines Application:**
    - In your Slack workspace, navigate to **Apps**, then click on **Manage Apps**.
    - Search for and add the **Tines** application.
      ![slack_VX4TI7unFW](https://github.com/user-attachments/assets/125cc861-9982-43e7-8f3e-b3e956a4d695)

### Step 3: Sending Alerts via Slack

1. **Set Up Template for Slack Messages:**
    - Drag the Slack message action into your Tines workflow.
      ![chrome_76bEtYhHAr](https://github.com/user-attachments/assets/90d02680-dce2-46b9-9e82-56f9a58dc285)

    - Configure it to send messages to the created **Alert Channel**.
    - Use the channel ID copied from Slack.
      ![slack_yKsYGDRNqN](https://github.com/user-attachments/assets/fbdb3e36-0b5f-44fe-b097-c7fd463f5b8b)
    - The message format could include:
        
        ```
        Hello, an alert has been generated regarding {detection title}. Please take necessary actions.
        ```
        
2. **Test Slack Integration:**
    - Trigger an event in Tines to ensure alerts are sent to Slack successfully.
      ![slack_YZWPqiWzg7](https://github.com/user-attachments/assets/5d099d23-baf1-4da3-8a27-0af0d5eb4a0f)


### Step 5: Sending Alerts via Email

1. **Add Email Action:**
    - Drag the **Send Email** action into your Tines story following the Slack message action.
2. **Configure Email Settings:**
    - Use an email address you can access easily.
    - Set the subject and body:
        
        ```
        Subject: Alert Notification
        Body: An alert has been generated regarding {detection title}. Please check the details.
        ```
        
3. **Test Email Functionality:**
    - Retrieve a detection and trigger the email action to confirm delivery.
      ![chrome_BP9ILqDF5v](https://github.com/user-attachments/assets/afb69ae6-d378-415a-907a-6248b2887d24)
      ![lQAA346qWA](https://github.com/user-attachments/assets/d1991fa4-ba3f-4b73-99c3-9a59563f3cca)


### Step 6: User Prompt for Machine Isolation

1. **Create User Prompt Page:**
    - Drag the Page tool into your workflow, naming it `User Prompt`.
      ![chrome_XSoEuPiJQD](https://github.com/user-attachments/assets/880dc210-587d-4bca-8ccf-ab6041cba5bf)

    - Configure it to ask whether the user wants to isolate the machine using Boolean Yes/No buttons.
2. **Link User Actions:**
    - Connect Yes to isolation actions and No to an alert message stating, "Computer not isolated; please investigate."
      ![chrome_y1p3VuZxmC](https://github.com/user-attachments/assets/d406e1eb-545c-4440-b249-5aca9cd2fe3a)


### Step 7: Automating Isolation Actions

1. **Setup Isolation Trigger:**
    - Drag the action to isolate a sensor into your workflow when the user selects Yes.
2. **Configure Sensor Isolation:**
    - Ensure that you use the correct sensor ID from Tine's detection body.
3. **Test Isolation Process:**
    - Test by running through the user prompt and select Yes to verify that isolation occurs as intended.
      ![chrome_6qOFv015kr](https://github.com/user-attachments/assets/0f8f5595-8742-468b-b71f-00f7144c8222)


### Step 8: Finalize Notifications Post-Isolation

1. **Send Post-Isolation Notifications:**
    - After isolation, send another Slack message informing stakeholders that the machine has been isolated.
      ![slack_cF7YWxnJlF](https://github.com/user-attachments/assets/d5f1b6a0-63a6-4701-8d73-dba4c075d33b)

2. **Test Entire Workflow:**
    - Ensure that all components work seamlessly together, from detection to alerts.


### Part 5 Commentary

- **Best Practices:** Always test each component of your workflow independently before integrating them together. This helps identify issues early in the development phase.
- **Common Pitfalls:** Ensure that API keys and credentials are correctly configured; otherwise, integrations may fail without clear error messages.


## Results and Achievements

The implementation successfully demonstrated:

- Automated detection of security events
- Streamlined incident response procedures
- Reduced manual intervention requirements
- Enhanced visibility into security incidents
- Improved response times to potential threats

## Conclusion

This project showcases practical implementation of modern security automation tools, demonstrating the ability to design and deploy enterprise-grade security solutions. The integration of EDR and SOAR capabilities provides a robust framework for threat detection and response.
