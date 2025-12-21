# General Settings / Bottom Bar

General Settings is reachable via the cog icon in the bottom-left corner. 
The dialog wraps three tabs: **Users**, **Roles**, and **Server**.

---

## Users tab

- The list shows every account together with its assigned roles and whether the user is active.
- **Add User** opens a dialog that requires a username and password (both at least 3 characters long) and lets you assign one or more roles via chips. You can also edit any account except the built-in `admin` user, which is always active and forced to change its password on first login.
- After saving, the UI refreshes the list so you can immediately check the newly created user or re-open the dialog to tweak the password or roles.

---

## Roles & Permissions tab

- **Add Role** opens a minimal dialog (name only); the actual permissions live in the matrix below the toolbar.
- The matrix groups permissions by feature or CRUD resource, shows a search field, and collapses each group until you expand it (searching automatically expands everything so matches are always visible). Clicking a checkbox immediately sends the update to the backend and shows a toast; admin and other protected roles cannot be edited, renamed, or deleted.
- Long-pressing the dots menu lets you duplicate a role so you can copy its permission set and tweak the clone before assigning it to a user.
- The backend now derives permission groups from feature names or prefixes (e.g., Parking Times, Image to SD Card, Settings), so the matrix stays organized even when new permissions are added.

## Server tab

### Logging Level

Choose from `Debug`, `Info`, `Warning`, or `Error` to control how much data the ACAP runtime emits into the logs. The selection is persisted on the camera and takes effect immediately after you click **Save Settings**.

-### Web server TLS

- Use the TLS mode selector to switch between the generated self-signed certificate and a single uploaded PEM/key pair that is kept within the app’s local data store.
- When you select the uploaded mode, the card surfaces a status badge (`Uploaded certificate pair is stored and active` or `No valid uploaded certificate found`) plus a **Replace Certificate** button that reveals fresh `Certificate PEM` and `Private Key PEM` inputs.
- The **Upload Certificate Pair** button turns green once both files are selected and stays disabled otherwise; it validates the PEM pair before storing it so your camera never switches to an unusable certificate.
- Saving Settings is blocked until a valid uploaded pair exists, preventing the web server from restarting with broken TLS. When the running certificate actually changes (mode toggle or replaced pair), the backend closes the HTTP connection so Axis OS can reload the new certificate cleanly—expect a short reconnect while the browser re-establishes the session.

---

## Inspecting Logs

- **Live Logs:**  
  Open the “Live Logs” using the logging button (located at the bottom left corner) to view real-time log entries.

- **Persistent Logs:**  
  Access the “App Log” from the Apps Overview to inspect logs that have been saved persistently.

---
