# Linux 701
## Lesson 10: Frontend State Intergration

Ready the CLI

```console
cd ~/School/Codia/701
```

Keep 701bio running
___

*`codia.html` is the default state helper in the browser. Vue, React, and AngularJS stay options. None of them replace the backend. They call the same API*

*701bio itself stays vanilla `fetch` on the profile so you can still read it*

*Put your API key in the browser console once:*

```javascript
localStorage.setItem('bio_key', 'YOUR_KEY_HERE')
```

### Default: `codia.html`

| **1** :$

```console
cp core/10-codia.html codia.html && \
code core/10-codia.html
```

| **B-1** ://

```console
localhost/codia.html
```

Operative state object:

```js
const Codia = {
  state: { notes: [], err: '' },
  set(patch) { Object.assign(this.state, patch); this.render(); },
  async load() { ... fetch('/api/notes?key=...') ... }
};
```

*A list in memory, a `render()`, a `load()`. That is the whole library for this product*

### Optional: Vue, React, AngularJS

| **2** :$

```console
cp core/10-vue.html vue.html && \
cp core/10-react.html react.html && \
cp core/10-angular.html angular.html && \
code core/10-vue.html core/10-react.html core/10-angular.html
```

| **B-3** :// **Vue**

```console
localhost/vue.html
```

| **B-4** :// **React**

```console
localhost/react.html
```

| **B-5** :// **AngularJS**

```console
localhost/angular.html
```

*Each optional page holds a list in memory and re-renders. Reload hits the same GET. Package config `frontend=codia|vue|react|angular`*

*Play: POST a note with curl (Lesson 9), then click Reload on `codia.html` — the list grows*

*Play: clear `bio_key` in the console, reload — you should see `bad api key`*

```javascript
localStorage.removeItem('bio_key')
```

### BASH functions for the same jobs

| **6** :$

```console
cp core/10-701bio.sh 701bio.sh && \
chmod +x 701bio.sh && \
code core/10-701bio.sh
```

<!-- Use Vue, React, and AngularJS to interact with the API, using state to handle the data in the browser; default is our own codia.html helper -->

___

# The Take
## Default
- `codia.html` — small `state` + `render` + `fetch`
## Options
- Vue, React, AngularJS still copy in
- Same GET `/api/notes`
## BASH
- The same jobs can be functions in a shell script
___

#### [Lesson 11: Web App as System Service](https://github.com/JesseSteele/Codia/blob/master/701/Lesson-11.md)
