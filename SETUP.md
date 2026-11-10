# Role Profile entry app: setup guide

About 20 minutes, one time. You need the Google account that will own the sheet (ideally a LiveWell account such as admin@livewellfc.org, so it doesn't depend on any one employee).

## 1. Put the workbook in Google Drive

1. Upload `Job_Description_Template.xlsx` to Google Drive.
2. Open it, then choose **File > Save as Google Sheets**. Use the new Google Sheets copy from now on.
3. Clear the example content on **Employee 1** ("Jane Example (delete me)" and the gray italic rows) before Callie starts, or it will load into her form.
4. On **Employee Roster**, fix any incomplete emails. Lily Koksal's is currently `lily.koksal@` and she won't be able to sign in until it's complete.
5. Share the sheet only with admins. Employees never need access to the sheet itself; the app writes to it for them.

## 2. Add the app code

1. In the sheet, go to **Extensions > Apps Script**.
2. Replace everything in `Code.gs` with the contents of the `Code.gs` file.
3. Click **+ (Add a file) > HTML**, name it exactly `Index`, and paste in the contents of `Index.html`.
4. Click **Save**.

## 3. Run the setup check

1. In the function menu at the top of the editor, choose **checkSetup**, then click **Run**.
2. Google asks you to authorize the script (it needs the sheet and permission to send sign-in emails). Click through: **Review permissions > choose your account > Advanced > Go to project > Allow**.
3. Open the **Execution log**. It lists any roster problems (bad emails, tabs that don't exist, a tab assigned twice). Fix those on the roster and run it again until it says "Roster looks good."

## 4. Publish the app

1. Click **Deploy > New deployment**, choose the gear icon, then **Web app**.
2. Set **Execute as: Me** and **Who has access: Anyone**.
   - "Anyone" is what lets the page work inside Wix without a Google sign-in. Access is still limited: only people on the roster can get a code, and each person can only reach their own tab.
   - If "Anyone" isn't offered, your Google Workspace admin has blocked it. It can be allowed in the Admin console under Apps Script settings.
3. Click **Deploy** and copy the **Web app URL** (it ends in `/exec`).

Test it in a browser tab before Wix: open the URL, enter your email, and sign in with the code.

## 5. Embed it on Wix

For each employee page (for example `livewellfc.org/employees/jd/beth-koksal`):

1. In the Wix editor, add **Embed Code > Embed a Site** (the iframe option, not "Custom code").
2. Paste the web app URL with the person's name added at the end:
   `https://script.google.com/macros/s/XXXX/exec?u=beth-koksal`
   The `u=` part is first and last name, lowercase, with hyphens (for example `heather-wright-renick`). It only greets the person and pre-fills their email; the code still goes to the roster address, so a coworker who opens Beth's page can't get in as Beth.
3. Stretch the embed to full width and make it tall (about 1,400 px) so people don't scroll inside a small box.
4. Optional: leave out `?u=` and use one shared page; people then type their email instead.

## How it works for employees

- They open their page, click **Email me a code**, and type the 6-digit code. They stay signed in on that browser for 30 days.
- They move through sections 1 to 8. Changes save automatically a couple of seconds after they stop typing, and the status in the top bar shows when it's saved.
- When they come back, everything they entered is loaded from their tab, so they pick up where they left off.
- Admins (Callie, Heather, and admin@livewellfc.org on the roster) see a menu at the top to switch to any employee's profile.

## Safeguards built in

- **No overwriting other tabs.** The server looks up each person's tab from the roster. The browser never chooses it, so even a tampered page can't write elsewhere.
- **Formulas stay intact.** Item #, Match Check, and Hours per Year are never written; the app shows calculated values the same way the sheet does.
- **Two windows open at once.** If a profile was saved from another window or by an admin, the app stops saving and asks the person to reload instead of overwriting.
- **Removing someone from the roster** revokes their access immediately.
- Text is stored exactly as typed, so an entry can't turn into a formula, and "June 1" stays as text rather than becoming a date.

## Making changes later

- **Edit the code:** after saving changes in Apps Script, go to **Deploy > Manage deployments**, click the pencil, choose **Version: New version**, and **Deploy**. The URL stays the same, so Wix doesn't need updating.
- **Add an employee:** add a row to the roster and point it at an unused tab (duplicate an employee tab if you need more than 10). Then add a Wix page with their `?u=` link.
- **Dropdown choices** come from the **Lists** tab, so edits there show up in the app automatically. As the Lists tab notes, keep the three "JD Match" wordings unchanged.
- **Don't move rows** on employee tabs (inserting or deleting rows inside a section). The app writes to fixed rows that match the template.
- Edits made directly in the sheet show up the next time someone opens their profile.

## Limits to know

- Sign-in emails come from the sheet owner's Gmail. Google allows 100 emails a day on a free account and 1,500 on Workspace, far more than you need.
- CommonSpirit's email filter might hold the first code. If someone doesn't get it within a minute, have them check spam or quarantine, and consider asking IT to allow messages from the sheet owner's address.
- Each table section has a fixed number of rows (for example 35 duties, 25 written responsibilities), matching the template. The app says when a section is full.
