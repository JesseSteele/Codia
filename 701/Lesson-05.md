# Linux 701
## Lesson 5: Async and AJAX

Ready the CLI

```console
cd ~/School/Codia/701
```

Ready services

```console
sudo systemctl start nginx
```
___

*AJAX means the browser talks to the server **without reloading the page***

*In [Linux 501 Lesson 6](https://github.com/JesseSteele/Codia/blob/master/501/Lesson-06.md) we did this with PHP. Here the backend is Python, Node.js, or Go behind Nginx on port `9001`*

*We copy from `core/` like 501. We do **not** copy into a `web/` folder — Nginx already reverse-proxies `localhost` to `127.0.0.1:9001`*

### SQL engine as a setting
*Lesson 3 ended by putting `db_type` inside `db.*`. Starting here, that file is **global**. Change the engine, keep the app*

*Default going forward is **SQLite** (no service). MariaDB and PostgreSQL still work if you copy the matching config*

| **1** :$

```console
cp core/05-db.py db.py && \
cp core/05-db.js db.js && \
cp core/05-db.conf db.conf && \
cp core/05-db-process.py db_process.py && \
cp core/05-db-process.js db-process.js && \
cp core/05-db-process.go db-process.go && \
code core/05-db.py core/05-db.js core/05-db.conf core/05-db-process.py
```

*Note:*
- *Python imports `db` and `db_process` — that is why the Python copy is `db_process.py` (underscore)*
- *Node `require('./db')` and `require('./db-process')`*
- *Go keeps settings in a **text** `db.conf` (Lesson 3: do not use a `.go` config)*
- *`db_type` / `dbType` is `sqlite`, `mysql`, or `postgresql`*

Operative Python:

```py
db_type = 'sqlite'
db_name = 'backendapp.db'
```

```py
def get_db_connection():
    if db.db_type == 'sqlite':
        import sqlite3
        conn = sqlite3.connect(db.db_name, check_same_thread=False)
        ...
```

*To use MariaDB instead:*

```console
cp core/05-mysql-db.py db.py
```

*PostgreSQL:*

```console
cp core/05-postgres-db.py db.py
```

*Same idea for `.js` and `.conf`*

*Play: open `db.py`, change nothing, just notice there is one setting and three engines*

### AJAX guestbook
*A `<form>` that `fetch()` POSTs. The page does not reload. Rows come back as JSON*

*The first paint is still HTML. After that, JSON is the payload*

#### Python

| **2** :$

```console
cp core/05-ajax.py ajax.py && \
code core/05-ajax.py
```

*Note `e.preventDefault()` on the form, then `fetch('/ajax', { method: 'POST' })`*

Operative browser JavaScript (inside the Python file):

```js
document.getElementById('gb').addEventListener('submit', async function (e) {
  e.preventDefault();
  const body = new URLSearchParams({
    name: document.getElementById('name').value,
    note: document.getElementById('note').value
  });
  const r = await fetch('/ajax', { method: 'POST', body });
  const data = await r.json();
  ...
});
```

Operative Python (JSON, not a new HTML document):

```py
        if self.path.startswith('/ajax'):
            ...
            self.send_header("Content-type", "application/json")
            self.wfile.write(json.dumps({"rows": rows}).encode())
```

| **3** :$

```console
python ajax.py
```

| **B-3** ://

```console
localhost
```

*Type a name and a note. Click Send*

*Watch the terminal: each AJAX call is still a real HTTP request*

*Watch the page: the URL did not change. The list did*

*Play: submit twice, then refresh — the SQL table kept the rows*

*Play: <kbd>Ctrl</kbd> + <kbd>Shift</kbd> + <kbd>C</kbd> → Network. Filter `ajax`. See JSON*

*(When finished: <kbd>Ctrl</kbd> + <kbd>C</kbd> in the terminal to exit)*

#### Node.js

| **4** :$

```console
cp core/05-ajax.js ajax.js && \
code core/05-ajax.js
```

*Same `fetch`. Same `/ajax`. Node's `http.createServer` looks at `req.method`*

| **5** :$

```console
node ajax.js
```

| **B-5** ://

```console
localhost
```

*Play: submit, then in another terminal:*

```console
sqlite3 backendapp.db "SELECT * FROM guestbook;"
```

*(When finished: <kbd>Ctrl</kbd> + <kbd>C</kbd> in the terminal to exit)*

#### Go

| **6** :$

```console
cp core/05-ajax.go ajax.go && \
code core/05-ajax.go
```

*Go still needs `db-process.go` in the same `package main`*

| **7** :$

```console
go run ajax.go db-process.go
```

| **B-7** ://

```console
localhost
```

*(When finished: <kbd>Ctrl</kbd> + <kbd>C</kbd> in the terminal to exit)*

### XMLHttpRequest vs `fetch`
*501 used `XMLHttpRequest()`. That still works. These files use `fetch()` because it is the current common practice for the same idea*

*Both are AJAX: a request that does not replace the whole document*

*JSON here, not XML — 501 already said AJAX can use either*

### What just happened
- *The browser ran JavaScript*
- *The server never sent a new HTML document on submit*
- *JSON is the payload, HTML is only the first paint*
- *This is the same pattern 701bio will use for profile tabs (lazy load) and autosave*

*If Nginx is stopped, `localhost` on port 80/443 dies. `localhost:9001` still works. Keep Nginx up — we are a proxied service now*

<!-- Demonstrate how Async and AJAX work, using similar teaching workflow as we used in 501 Lesson 6, but not as elaborate on different types of AJAX; we want to use the native server langauge Asynchronous tools as much as possible, but making it simple; once demonstrated with basic conventions, later lesson steps will show SQL quaries, INSERT, SELECT, UPDATE, DELETE, but extremely simple and no login; do this for each SQL engine if code would change significantly, using comment-able lines in the same file for the SQL-engine difference so Ss can choose the engine while seeing the rest of the code remain static -->

___

# The Take
## AJAX
- The page stays put; `fetch()` (or `XMLHttpRequest`) talks to the server
- POST can return JSON instead of HTML
- `e.preventDefault()` stops the browser from doing a full form GET/POST
- The terminal still logs every request — "no reload" is a **browser** story
## SQL engine setting
- One `db.*` file holds `sqlite` / `mysql` / `postgresql`
- App code should not care which engine is live
- SQLite is the classroom default because it is a file, not a service
- Python copy is `db_process.py`; Node/Go keep `db-process.*`
## Nginx
- Keep using the reverse proxy from Lesson 1
- Apps listen on `9001` as `127.0.0.1`
___

#### [Lesson 6: Login, email, SESSION & COOKIES](https://github.com/JesseSteele/Codia/blob/master/701/Lesson-06.md)
