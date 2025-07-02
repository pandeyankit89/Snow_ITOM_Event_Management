## ServiceNow ITOM Event Management - Cisco ThousandEyes Integration

### **🔗Helpful URL:**   `https://www.servicenow.com/community/developer-forum/configuring-the-thousandeyes-connecter-for-event-management/m-p/2638874`
---

### 🔧 Actions from ServiceNow Admin

1. **Go to:** `https://mysnow.service-now.com/cmdb_ci_endpoint_http_list.do`

2. **Create a new HTTP(S) Endpoint CI**  
   - **Name:**  `https://mysnow.service-now.com/api/sn_em_connector/em/inbound_event?source=thousandeyes`

3. **Create a User:**  
   - **Username:** `ThousandEye`  
   - **Roles:** `itil`, `web_service_admin`, `evt_mgmt_integration`

4. **Create an OAuth Configuration for the ThousandEyes Push Connector**  
   - **Navigate:** `All > System OAuth > Application Registry > New`  
   - **Select:** `Create an OAuth API endpoint for external clients`  
   - **Add Redirect URLs** (comma-separated and no-space after comma):
     ```
     https://app.thousandeyes.com/webhooks-oauth-callback/,https://app.thousandeyes.com/namespace/integrations/AuthCallbackPage.html
     ```
   - **Timestamps:** Ensure set to `86400000` (1000 days)
   - **Client Secret:** Ensure size is less than `50`characters
   - **Collect:**
     - **Auth URL:** `https://mysnow.service-now.com/oauth_auth.do`
     - **Client ID:** `xxxxxxxxxxxxxxxxxxxx`
---

## 🔧 Actions from ThousandEyes Admin

5. **Login to ServiceNow**  
   - URL: `https://mysnow.service-now.com/login.do`  
   - Use the `ThousandEye` user created in Step #3

6. **Create Webhook in ThousandEyes**  
   - Navigate: `Manage > Alert Rule > Select Rule > Notification > Edit Webhook > Add New Webhook`
   - **Details:**
     - **Name:** `Servicenow_QA`
     - **URL:** `https://mysnow.service-now.com/api/sn_em_connector/em/inbound_event?source=thousandeyes`
     - **Auth Type:** OAuth
     - **Grant Type:** Implicit
     - **Auth URL:** `https://mysnow.service-now.com/oauth_auth.do`
     - **Client ID:** `xxxxcopyfromstep#4xxxxx`

7. **Get Token**  
   - Click `Get Token` → It will open a new ServiceNow tab.  
   - Ensure the logged-in user is the same as Step #3 → Click `Allow`.
   - After `Allow`, it should redirect automatically from service-now page to thousand-eyes page.

8. **Test the Integration**  
   - Click `Test` → A test event should be created in ITOM Event Management.

9. **Enable Webhook**  
   - Check the box for `Servicenow_QA` in the Webhook section → Save changes.

10. **Repeat Step #9** for all alert rules where incidents are required.
---

### 🔧 Actions from ITOM Event Management Admin

11. **Verify Test Event**
   - Navigate: `All Events`
   - Filter by Source: `ThousandEyes`
   - Confirm event time matches Step #8.

12. **Create Event Rule**
   - Configure an Event Rule for ThousandEyes events as per your organizational policy.
---
