# Linux 701
## Lesson 10: Frontend State Intergration

Ready the CLI

```console
cd ~/School/Codia/701
```

Keep 701bio running
___

*Vue, React, and AngularJS are **state** libraries in the browser. They do not replace our backend. They call the same API*

*701bio itself stays vanilla `fetch` on the profile so you can still read it*

*Put your API key in the browser console once:*

```javascript
localStorage.setItem('bio_key', 'YOUR_KEY_HERE')
```

### Copy the three demos next to the app
*Lesson 8 already serves `/vue.html`, `/react.html`, `/angular.html` from the working directory. Copy the files beside `bio.*` so the same origin can `fetch('/api/notes?key=...')`*

| **1** :$

```console
cp core/10-vue.html vue.html && \
cp core/10-react.html react.html && \
cp core/10-angular.html angular.html && \
code core/10-vue.html core/10-react.html core/10-angular.html
```

| **B-2** :// **Vue** *(701bio must be running)*

```console
localhost/vue.html
```

| **B-3** :// **React**

```console
localhost/react.html
```

| **B-4** :// **AngularJS**

```console
localhost/angular.html
```

*Each page holds a **list in memory** and re-renders. Reload button hits the same GET*

Operative Vue:

```js
const r = await fetch('/api/notes?key=' + encodeURIComponent(key));
const d = await r.json();
this.notes = d.notes || [];
```

*Play: POST a note with curl (Lesson 9), then click Reload on the Vue page — the list grows. No server rewrite*

*Play: clear `bio_key` in the console, reload — you should see `bad api key`*

```javascript
localStorage.removeItem('bio_key')
```

### BASH functions for the same jobs
*The lesson stub said go all the way: common server-side logic as BASH, not only JS*

| **5** :$

```console
cp core/10-701bio.sh 701bio.sh && \
chmod +x 701bio.sh && \
code core/10-701bio.sh
```

*Source it so the functions land in your shell:*

| **6** :$

```console
. ./701bio.sh
701bio_public_notes
701bio_note_titles
701bio_count_media
701bio_count_notes
```

*API wrappers (put your key):*

```console
701bio_api_profile YOUR_KEY_HERE
701bio_api_post_note YOUR_KEY_HERE "From bash" "Functions wrapping curl."
```

*Start helpers (they block like `python bio.py`):*

- *`701bio_start_python`*
- *`701bio_start_node`*
- *`701bio_start_go`*

*Sysadmins live here. Frontend people live in the HTML files. Same product*

<!-- Vue, React, Angular -->
<!-- Create our own, basic frontend state manager, extremely simple and smaller, more dependent on function/method parameters than knowledge of multiple objects and properties, just enough to handle a few forms -->
<!-- This interacts with our web app we made in Lesson 8; the custom state manager will be our default with options for Vue, React, and Angular, and examples for each -->

___

# The Take
## Frontend state
- Vue / React / AngularJS hold **lists in memory** and re-render
- They still GET/POST our API
- 701bio itself stays vanilla JS on the profile so students can read it
- Demos are same-origin files served by 701bio (`/vue.html`)
## BASH
- Functions wrap start, list public notes, count media, call the API
- `source` (`. ./file`) puts functions in the current shell
- Sysadmins live here; frontend people live in the HTML files
## Intergration
- The library does not replace the backend
- Three libraries, one JSON contract
___

#### [Lesson 11: Web App as System Service](https://github.com/JesseSteele/Codia/blob/master/701/Lesson-11.md)
