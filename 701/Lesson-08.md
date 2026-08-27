# Linux 701
## Lesson 8: Web App & Installer

Ready the CLI

```console
cd ~/School/Codia/701
```
___


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

___

#### [Lesson 9: API](https://github.com/JesseSteele/Codia/blob/master/701/Lesson-09.md)
