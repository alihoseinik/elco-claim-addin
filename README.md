# Claim Case Lookup – test Outlook add-in

Adds a **Claim Lookup** button to the ribbon when you read an email. The button opens a pane that shows the exact IDs of that email: the Graph/REST ID, internetMessageId, conversationId and the mailbox. It also works in the Kundendienst.Mitte shared mailbox. The pane only reads the email. Nothing is sent anywhere unless you use the optional "Send to flow" section.

## 1. Host the files (HTTPS is required)

Outlook loads the pane from a web address, so these files must be online over HTTPS:

```
taskpane.html
commands.html
assets/icon-16.png … icon-128.png
```

Quickest way, GitHub Pages:
1. Create a repository named **elco-claim-addin** and upload the files above, keeping the `assets` folder.
2. Go to Settings → Pages → Source: *Deploy from a branch* → `main` / root → Save.
3. After about a minute, check that `https://<your-github-user>.github.io/elco-claim-addin/taskpane.html` opens in a browser. It will say "Open this inside Outlook", which is expected.

Any other HTTPS web space works too, for example your VPS or a SharePoint-independent web server.

## 2. Point the manifest at your host

Open `manifest.xml` in Notepad, press Ctrl+H, and replace

`YOUR-HOST` → `<your-github-user>.github.io`

then click "Replace all". If you used a different folder name than `elco-claim-addin`, fix that part of the paths too.

## 3. Install it in Outlook

Outlook on the web: open https://aka.ms/olksideload → **My add-ins** → **Add a custom add-in** → **Add from File…** → choose `manifest.xml` → Install.

In classic Outlook for Windows, the add-in can take up to 24 hours to appear because of caching. Test on the web first.

## 4. Test

1. Open any email and click **Claim Lookup** in the ribbon (in Outlook on the web it can sit under the **⋯ / Apps** menu).
2. Open an email in the **Kundendienst.Mitte** shared mailbox. The pane should show the "Shared mailbox" badge and the shared mailbox's address.
3. Click **Copy as JSON** and keep the result. The `restId` is what a flow uses with `GET /users/{mailbox}/messages/{restId}`.

## Optional step 2: send the email to a flow

Create a flow with the trigger *When an HTTP request is received* and set "Who can trigger" to *Anyone* for the test. Paste its URL into the pane and click **Send this email to flow**. Whatever the flow returns with a *Response* action is shown in the pane. This is the basis for showing the enrichment data right next to the email.

## Remove

Go to https://aka.ms/olksideload → My add-ins → Custom add-ins → ⋯ → Remove.
