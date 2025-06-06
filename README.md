# Copilot connectors HTTP

This repo demonstates how to create a custom Copilot connector for Microsoft 365 using HTTP requests.

# Setup

## Microsoft Entra ID app registration

1. Create a new Microsoft Entra ID app registration in the [Azure portal](https://portal.azure.com/).
1. Add the following API permissions to the app registration:
   - `ExternalConnection.ReadWrite.OwnedBy`
   - `ExternalItem.ReadWrite.OwnedBy`
1. Grant admin consent for the permissions.
1. Create a new client secret.

## Visual Studio Code setup

1. Install the [REST Client](https://marketplace.visualstudio.com/items?itemName=humao.rest-client) extension in Visual Studio Code.
1. Open command palette (Ctrl+Shift+P) and select "Preferences: Open Settings (JSON)".
1. Add the following settings to the JSON file, replacing `<tenantId>`, `<clientId>`, and `<clientSecret>` with the values from your Microsoft Entra ID app registration:

    ```json
    {
        "rest-client.environmentVariables": {
            "$shared": {},
            "gc-http": {
                "aadV2TenantId": "<tenantId>",
                "aadV2ClientId": "<clientId>",
                "aadV2ClientSecret": "<clientSecret>",
                "aadV2AppUri": "https://graph.microsoft.com"
            }
        }
    }
    ```

# Usage

1. Open [1-create-connection.http](1-create-connection.http) file.
1. Select the environment **gc-http** in the REST Client extension (bottom right corner of VS Code).
1. Send the **createConnection** request to create a new external connection.
1. Open [2-create-schema.http](2-create-schema.http) file.
1. Send the **createSchema** request to create a new schema for the external connection.
1. Send the **getOperationStatus** request to check the status of the schema creation. When the status is `completed`, you can proceed to the next step. This can take up to 5-10 minutes.
1. Open [3-create-item.http](3-create-item.http) file.
1. Send the **createItem** request to create a new item in the external connection.
1. Send the **getItem** request to get the item you just created.

# Make external items searchable

1. Navigate to the [Microsoft 365 admin center](https://admin.microsoft.com)
1. Expand the **Show all** section in the left navigation pane.
1. Expand the **Settings** section.
1. Select **Search & intelligence**.
1. Select **Data sources**.
1. Find the external connection you created in the previous steps, select **Include Connector Results**.
1. In the dialog, select **OK** to confirm.
1. Navigate to [Microsoft 365](https://m365.cloud.microsoft/)
1. In the search box, type `Daredevil` and press Enter.
1. You should see the item you created in the external connection in the search results.

> [!NOTE]
> Items may not appear immediately in the search results. It may take some time for the search index to update. If you don't see the item, wait a few minutes and try again.

# Add result type (optional)

1. Navigate to the [Microsoft 365 admin center](https://admin.microsoft.com)
1. Expand the **Show all** section in the left navigation pane.
1. Expand the **Settings** section.
1. Select **Search & intelligence**.
1. Select **Customizations**.
1. Select **Result types**.
1. Select **Add**.
1. In the dialog, enter a name for the result type **Marvel character** and select **Next**.
1. In the **Choose a source** step, select **Marvel Universe Characters** and select **Next**.
1. In the **Set rules for the result type** step, select **Next**.
1. In the **Choose a display template** step, copy the contents of [search/result-type.json](/search/result-type.json) and paste it into the **Paste the JSON script that you created with Layout Designer** field, then select **Next**.
1. In the **Review the result type settings** step, select **Add result type**.

> [!NOTE]
> It can take some time for the result type to be applied. If you don't see the result type in the search results, wait a few minutes and try again.


