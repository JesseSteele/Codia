# Linux 701
## Lesson 8: Web App & Installer

Ready the CLI

```console
cd ~/School/Codia/701
```

Ready services

```console
sudo systemctl start nginx
```

### This lesson uses the main terminal plus three SQL tabs

Ready the SQL tabs and leave them open *(<kbd>Ctrl</kbd> + <kbd>Shift</kbd> + <kbd>T</kbd>; <kbd>Ctrl</kbd> + <kbd>PageUp</kbd>/<kbd>PageDown</kbd> to switch)*

| **S0** :$ *(password in the terminal, not safe outside these lessons!)*

```console
mariadb -u admin -padminpassword
```

| **S1** :$

```console
psql -U postgres
```

| **S2** :$

```console
sqlite3 backendapp.db
```

___

*This is where **701bio** starts: web installer, login `user_key`, public profile tabs, notes as `notes/*.md`, uploads in `media/`*

*Later lessons add an API host and a client. They should not force you to rewrite this web UI*

## Install and run

### Python

| **S2** :>

```sql
.tables
```

| **1** :$

```console
cp core/08-bio.py bio.py && \
cp core/05-db1.py db.py && \
cp core/05-db-process.py db_process.py && \
code core/08-bio.py
```

| **S2** :>

```sql
DROP TABLE IF EXISTS users;
DROP TABLE IF EXISTS sessions;
DROP TABLE IF EXISTS api_keys;
DROP TABLE IF EXISTS media;
```

Operative installer gate:

```py
        if not configured() and path != "/install":
            self.html(wrap("Install", "<h1>701bio</h1><p><a href='/install'>Run the installer</a></p>"))
            return
```

| **2** :$

```console
python bio.py
```

| **B-2** :// **installer**

```console
localhost/install
```

*Create an admin. Full name, username, password, bio*

*After submit you should land on `/login`*

| **B-2b** :// **login**

```console
localhost/login
```

*Log in. You should see Edit notes / Upload / Developer in the nav*

### Write a public note
| **B-2c** :// **editor**

```console
localhost/edit
```

*Title, check Public, write a sentence, Save*

*Autosave: type, wait a few seconds, refresh, confirm restore — `localStorage` key `as_` + slug*

*There is no history table. Save overwrites `notes/your-slug.md`*

*Play: in another terminal:*

```console
ls notes
cat notes/*.md
vim notes/*.md
```

*Refresh `/edit`. vim's save is the note*

### Public profile, lazy tabs
| **B-2d** :// **logged out**

```console
localhost/logout
```

*Then:*

```console
localhost
```

*You should see the bio, and four buttons. Click **Notes**. The tab `fetch`es `/ajax/public?tab=notes`. The page did not reload*

Operative lazy load:

```js
async function loadTab(tab) {
  document.getElementById('panel').innerHTML = '<p><i>Loading…</i></p>';
  const r = await fetch('/ajax/public?tab=' + tab);
  const data = await r.json();
  ...
}
```

*(Keep `python bio.py` running until you switch languages. <kbd>Ctrl</kbd> + <kbd>C</kbd> then)*

### Node.js

| **S2** :>

```sql
DROP TABLE IF EXISTS users;
DROP TABLE IF EXISTS sessions;
DROP TABLE IF EXISTS api_keys;
DROP TABLE IF EXISTS media;
```

| **3** :$

```console
cp core/08-bio.js bio.js && \
cp core/05-db1.js db.js && \
cp core/05-db-process.js db-process.js && \
code core/08-bio.js
```

| **4** :$

```console
node bio.js
```

| **B-4** ://

```console
localhost/install
```

*(When finished: <kbd>Ctrl</kbd> + <kbd>C</kbd> in the terminal to exit)*

### Go

| **S2** :>

```sql
DROP TABLE IF EXISTS users;
DROP TABLE IF EXISTS sessions;
DROP TABLE IF EXISTS api_keys;
DROP TABLE IF EXISTS media;
```

| **5** :$

```console
cp core/08-bio.go bio.go && \
cp core/05-db-process.go db-process.go && \
cp core/05-db1.conf db.conf && \
code core/08-bio.go
```

| **6** :$

```console
go run bio.go db-process.go
```

| **B-6** ://

```console
localhost/install
```

*(When finished: <kbd>Ctrl</kbd> + <kbd>C</kbd> in the terminal to exit)*

### Developer dash
*Logged in, open `/dev`. Create an API key. Copy it once*

| **B-7** :// *(logged in)*

```console
localhost/dev
```

*Create a key named `class`. Copy the hex. You will not see the full key again — only `…` plus the last four characters*

### Drop everything before Lesson 9

| **S0** :>

```sql
DROP DATABASE IF EXISTS backendapp_db;
```

| **S1** :>

```sql
DROP DATABASE IF EXISTS backendapp_db;
```

| **S2** :>

```sql
.quit
```

| **8** :$

```console
rm -f backendapp.db
```

<!-- Start by making the SQL engine a settings option with a global database file, using modules to work with the separate database engines -->
<!-- Quickly move to the dashboard page with basic the web renders, showing the mechanics, all using no login -->
<!-- Basic web app with single-user login, one-page user bio profile (Vication, Location, Motto, Bio, Type [male, female, brand, company, charity, customize, not shared]) with custom URL links (both link and label, using basic & custom-made SVG-mono icons for top ten social media sites) and file-uploads that can be linked; login options for password, email verification link & account recovery, 2FA Code Generator, and passkeys; a dashboard page that shows views and keeps track of IP requests & stats with simple, lightweight-minimal CSS & inline SVG charts & graphs, account page (with security, etc), edit bio page, and page to manage and edit the text notes -->
<!-- Add basic login with password first, then email code 2FA (explain that the email won't actually send from most local dev servers, but use a normal disappearing indicator with testing to show that the script executed), then Authenticator app after password, then passkey also; final rendering will use actual email process to email the code  -->
<!-- Web app has simple list of internal notes in simple text forms with title, slug, date, body, meta-credits, and tag fields, with the body using markdown and an optional "render" side panel that can be opened or collapsed, also a public/private/link-only setting; each will be able to be accessed through the API in Lesson 9 through the slug field -->
<!-- Engines module files with setting uption for each database type we are using -->
<!-- The dashboard will have a theme setting that will draw names from a css/ folder inside the web folder with seven CSS color scheme examples to select from, saved in the database or overturned in the config file -->
<!-- We will finalize this as a package in Lesson 12, so check future lessons to see how this will be used, and render the entire product here with a config.?? file in the non-readable web folder, but no front-end state integration yet; we are using only minimal CSS and fully-integrated AJAX for forms -->
<!-- We want this organized pedagogically and also using professional convensions in file structure; create the app as simply as possible, but show a normal convention for how the various pages & function/setting includes work without too much complication, then integrate them more fully in a tightly-knit, but professionally organized file structure; still no state interaction yet; we don't need Ss to look at everything in the notes, only some basic parts -->
<!-- This lesson takes a different turn in process; up until now, complete files were listed inside the lesson while now, we will only highlight important teaching points with the full file being inside 701/core/ and edited in the code editor, as in unit 501, drawing attention to some of the specific teaching points, and showing Ss an overview to understand the app from a birdseye view, and to be able to see real-world integration of the new teaching points; the key point of this will be the web installer and all of the previous components brought together -->

___

# The Take
## Installer
- First visit with no users → `/install`
- DROP tables (or the sqlite file) so the installer can run again
- Installer writes the admin row and creates tables
## 701bio notes
- Notes are files; saving replaces the file
- Autosave is in the browser (`localStorage` `as_` keys)
## Profile tabs
- Public only
- Loaded when clicked (`/ajax/public?tab=`)
- Images use `loading="lazy"`
## Later lessons
- 9: API host + client (separate apps)
- 10: `codia.html` default state, Vue/React/AngularJS optional
- 11: systemd units
- 12: packages
___

#### [Lesson 9: API](https://github.com/JesseSteele/Codia/blob/master/701/Lesson-09.md)
