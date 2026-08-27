# Linux 701
## Lesson 6: Login, email, SESSION & COOKIE

Ready the CLI

```console
cd ~/School/Codia/701
```

Ready services

```console
sudo systemctl start nginx
```
___

*PHP had `$_SESSION` and `setcookie()`. Python, Node, and Go do not. You **make** both:*

1. *A random token stored in SQL (`sessions` table) — this is the SESSION*
2. *A `Set-Cookie: user_key=...` header — this is the COOKIE the browser sends back*

*We will keep using `db.py` / `db.js` / `db.conf` from Lesson 5*

*If those copies are gone:*

```console
cp core/05-db.py db.py && \
cp core/05-db.js db.js && \
cp core/05-db.conf db.conf && \
cp core/05-db-process.py db_process.py && \
cp core/05-db-process.js db-process.js && \
cp core/05-db-process.go db-process.go
```

### Demo user
- *Username: `demo`*
- *Password: `demo123`*
- *Created automatically on first run*

### What you should see
*Log in → Hello page. DevTools → Storage → Cookies → `user_key`. Logout → cookie dies and the SQL row is deleted*

#### Python

| **1** :$

```console
cp core/06-login.py login.py && \
code core/06-login.py
```

*Note:*
- *`hashlib.sha256` for the password (teaching hash; production would use a slow KDF)*
- *`secrets.token_hex(32)` for the session token*
- *`HttpOnly` cookie so JavaScript cannot read it*
- *"Remember me" sets a longer `Max-Age`*

Operative cookie:

```py
        self._html(PAGE_IN % html.escape(row[1]), extra=[("Set-Cookie", f"user_key={tok}; Path=/; Max-Age={maxage}; HttpOnly")])
```

Operative SESSION lookup:

```py
    cur.execute("SELECT u.id, u.username, u.fullname FROM sessions s JOIN users u ON u.id=s.user_id WHERE s.token=?", (tok,))
```

| **2** :$

```console
python login.py
```

| **B-2** ://

```console
localhost
```

*Log in as `demo` / `demo123`*

*Open DevTools → Application (or Storage) → Cookies → `user_key`*

*Check Remember me, log in again, look at `Max-Age` / Expires*

*Click Logout. Cookie dies. SQL row is deleted*

*Play: in another terminal:*

```console
sqlite3 backendapp.db "SELECT token, user_id FROM sessions;"
```

*(When finished: <kbd>Ctrl</kbd> + <kbd>C</kbd> in the terminal to exit)*

#### Node.js

| **3** :$

```console
cp core/06-login.js login.js && \
code core/06-login.js
```

*Same cookie name: `user_key`. Same tables: `users`, `sessions`*

| **4** :$

```console
node login.js
```

| **B-4** ://

```console
localhost
```

*Play: log in on one browser (or a private window). The other is still logged out. SESSION is per cookie, not "the server remembers everyone"*

*(When finished: <kbd>Ctrl</kbd> + <kbd>C</kbd> in the terminal to exit)*

#### Go

| **5** :$

```console
cp core/06-login.go login.go && \
code core/06-login.go
```

| **6** :$

```console
go run login.go db-process.go
```

| **B-6** ://

```console
localhost
```

*Go sets the cookie with `http.SetCookie`. Logout sets `MaxAge: -1`*

*(When finished: <kbd>Ctrl</kbd> + <kbd>C</kbd> in the terminal to exit)*

### Why not put the password in the cookie
*If the cookie held the password, every request would send the password, and JavaScript (or a stolen cookie log) would have it*

*The cookie holds a **random token**. SQL maps token → user. Logout deletes the map*

*501cms used PHP `$_SESSION` plus cookies. 701bio will use this same `user_key` + `sessions` table*

<!-- Also a basic login code authenticator and forgotten password authenticator, not actually sending the email, but displaying the code inside the window styled using <pre> tags and an <hr> separator -->
<!-- Show in theory the email process, but explain that the actual email probably won't send from a test server -->
<!-- Introduce SESSION and COOKIE use -->

___

# The Take
## COOKIE
- Lives in the browser
- Sent on every request to that host
- `HttpOnly` keeps it away from XSS JavaScript
- `Max-Age` is "remember me"
## SESSION
- Lives on the server (here: a SQL table)
- The cookie only holds a random token, not the password
- Logout deletes the row **and** expires the cookie
## Remember me
- Same token, longer `Max-Age`
## Hash
- We store `sha256` of the password, not the password
- Teaching hash — production wants a slow KDF
___

#### [Lesson 7: Files & Uploads](https://github.com/JesseSteele/Codia/blob/master/701/Lesson-07.md)
