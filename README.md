asi quedaria el Readme:
 
Databricks Integration with Entra ID
Overview
This document outlines the process for integrating Databricks with Entra ID for user provisioning and cleanup. This integration leverages Personal Access Tokens (PAT) for authentication, utilizes SCIM (System for Cross-domain Identity Management) for user management, and supports scheduling automated or manual script execution to add or remove users from specified Entra ID groups in Databricks.
Prerequisites
Databricks Workspace Access: You must have administrative access to the Databricks workspace to generate Personal Access Tokens (PAT).
Entra ID (Azure AD): Ensure that the required Azure AD group(s) exist and are populated with the necessary users.
Key Vault: The Databricks access token must be stored in an Azure Key Vault, and the identity running the script must have access to this secret.
AzureAD PowerShell Module: Ensure the AzureAD module is installed. The script will attempt to install it automatically if missing.
Step 1: Obtain the Databricks Personal Access Token
1.1 Generate Personal Access Token (PAT)
Log in to Databricks at https://accounts.azuredatabricks.net.
Navigate to User Settings > Access Tokens.
Click Generate New Token.
Copy the generated token. Keep it secure.
1.2 Store the Token in Azure Key Vault
Place the token in an Azure Key Vault secret.
Grant access to the secret for the identity used by the Function App or scheduled task.
Step 2: Configure the Script for Additional Databricks Instances
2.1 Copy an Existing Script
Copy an existing Databricks integration script as a template from the configuration file (line 1-70).
Paste it below the last entry in the SCIM AD Groups configuration file.
2.2 Update the Parameters
Modify the script to point to the new Databricks instance and configure the relevant parameters. Specifically, update the following variables:
$databricksInstance: Set this to the URL of the Databricks workspace (e.g., "https://adb-6234138917436195.15.azuredatabricks.net").  
$accessGroups: The Azure AD group whose members you want to add or remove from Databricks.  
$scimToken: The URL pointing to the Key Vault secret that contains the generated Personal Access Token.  
$exceptionList: A list of user emails to exclude from removal in Databricks.  
Step 3: Script Execution
The script supports automated scheduling and manual triggers for adding and removing users.
3.1 Automatic Execution
The script has been scheduled to run XX times per day to accommodate user provisioning needs and ensure consistency with Entra ID.
3.2 Manual Trigger
The script can also be manually triggered when needed to synchronize users from the specified Azure AD groups to Databricks.
Step 4: User Cleanup in Databricks
The script now includes functionality to remove users who:
Are no longer part of the specified Azure AD groups.
Are not listed in the $exceptionList.
This ensures that Databricks remains synchronized with Entra ID, enhancing security and reducing unnecessary user access.
Note
Users in the $exceptionList will never be removed, even if they are no longer part of the specified Azure AD groups. Keep this list up to date to ensure compliance and avoid unexpected behaviors.
Conclusion
This enhanced integration streamlines user provisioning and cleanup via Azure AD groups, ensuring:
Simplified User Management: Automatically adding and removing users based on their membership in AD groups reduces manual intervention.
Improved Structure: Managing access through AD groups provides a scalable approach to user access control.
Centralized Control: Leveraging Azure AD ensures centralized user management across systems.
By automating user provisioning and cleanup, organizations achieve improved security, compliance, and operational efficiency.
For additional assistance or questions, please refer to the Cloud Horizontal team.
 
Databricks - Sign in
Databricks Sign in
 
