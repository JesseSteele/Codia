# Linux 701
## Lesson 4: URL Rewrite & Parsing

Ready the CLI

```console
cd ~/School/Codia/701
```

Ready services

```console
sudo systemctl start nginx mariadb postgresql
```

___

### Rewrite GET URL
*Make pretty URLs converted to GET values*

*Previously, in [Lesson 5: RewriteMod (Pretty Permalinks)](https://github.com/JesseSteele/Codia/blob/master/501/Lesson-05.md), we re-wrote URLs using Apache settings in `.htaccess` inside the web folder*

*Python, Node.js, and Go can rewrite URLs as GET values from inside the app itself, meaning that no `.htaccess` `RewriteMod` settings are needed outside of the actual app*

### Single GET URL Value
*First, we will rewrite one, single GET value in the URL*

#### Python
*Create the Python app*

| **1** :$

```console
cp core/04-rewrite-get.py rewrite-get.py && \
code core/04-rewrite-get.py
```


#### Node.js
*Create the Node.js app*

| **2** :$

```console
cp core/04-rewrite-get.js rewrite-get.js && \
code core/04-rewrite-get.js
```


#### Go
*Create the Go app*

| **3** :$

```console
cp core/04-rewrite-get.go rewrite-get.go && \
code core/04-rewrite-get.go
```


#### Implementation
*Now, see it in action...*

##### Python

| **4** :$

```console
python rewrite-get.py
```

| **B-4** ://

```console
localhost/first-get-arg
```

Operative Python code:

```py
        # Rewrite rule: /GET-one-here → ?one=GET-one-here
        match = re.match(r'^/([^/]+)$', path)
        if match:
            query_components['one'] = [match.group(1)]
        else:
            # Fallback to standard query string parsing
            parsed_path = urllib.parse.urlparse(path)
            query_components = urllib.parse.parse_qs(parsed_path.query)
```

*(When finished: <kbd>Ctrl</kbd> + <kbd>C</kbd> in the terminal to exit)*

##### Node.js

| **5** :$

```console
node rewrite-get.js
```

| **B-5** ://

```console
localhost/first-get-arg
```

Operative Node.js code:

```js
    // Rewrite rule: /GET-one-here → ?one=GET-one-here
    const match = path.match(/^\/([^/]+)$/);
    if (match) {
        queryObject['one'] = match[1];
    } else {
        // Fallback to standard query string parsing
        queryObject = url.parse(req.url, true).query;
    }
```

*(When finished: <kbd>Ctrl</kbd> + <kbd>C</kbd> in the terminal to exit)*

##### Go

| **6** :$

```console
go run rewrite-get.go
```

| **B-6** ://

```console
localhost/first-get-arg
```

Operative Go code:

```go
    // Parse the path for URL rewrite
    path := r.URL.Path
    values := url.Values{}

    // Rewrite rule: /GET-one-here → ?one=GET-one-here
    re := regexp.MustCompile(`^/([^/]+)$`)
    if matches := re.FindStringSubmatch(path); len(matches) > 1 {
        values.Set("one", matches[1])
    } else {
        // Fallback to standard query string parsing
        var err error
        values, err = url.ParseQuery(r.URL.RawQuery)
        if err != nil {
            http.Error(w, "Error parsing query", http.StatusBadRequest)
            return
        }
    }

```

*(When finished: <kbd>Ctrl</kbd> + <kbd>C</kbd> in the terminal to exit)*

### Multiple GET URL Values
*Now, we will rewrite three GET values in the URL*

#### Python
*Create the Python app*

| **7** :$

```console
cp core/04-rewrite-multi-get.py rewrite-multi-get.py && \
code core/04-rewrite-multi-get.py
```


#### Node.js
*Create the Node.js app*

| **8** :$

```console
cp core/04-rewrite-multi-get.js rewrite-multi-get.js && \
code core/04-rewrite-multi-get.js
```


#### Go
*Create the Go app*

| **9** :$

```console
cp core/04-rewrite-multi-get.go rewrite-multi-get.go && \
code core/04-rewrite-multi-get.go
```


#### Implementation
*Now, see it in action...*

##### Python

| **10** :$

```console
python rewrite-multi-get.py
```

| **B-10** ://

```console
localhost/first-get-arg/second-get-arg/third-get-arg
```

Operative Python code:

```py
        # Rewrite rule: /GET-one-here/GET-two-here/GET-three-here → ?one=GET-one-here&two=GET-two-here&three=GET-three-here
        match = re.match(r'^/([^/]+)/([^/]+)/([^/]+)$', path)
        if match:
            query_components['one'] = [match.group(1)]
            query_components['two'] = [match.group(2)]
            query_components['three'] = [match.group(3)]
        else:
            # Fallback to standard query string parsing
            parsed_path = urllib.parse.urlparse(path)
            query_components = urllib.parse.parse_qs(parsed_path.query)
```

*(When finished: <kbd>Ctrl</kbd> + <kbd>C</kbd> in the terminal to exit)*

##### Node.js

| **11** :$

```console
node rewrite-multi-get.js
```

| **B-11** ://

```console
localhost/first-get-arg/second-get-arg/third-get-arg
```

Operative Node.js code:

```js
    // Rewrite rule: /GET-one-here/GET-two-here/GET-three-here → ?one=GET-one-here&two=GET-two-here&three=GET-three-here
    const match = path.match(/^\/([^/]+)\/([^/]+)\/([^/]+)$/);
    if (match) {
        queryObject['one'] = match[1];
        queryObject['two'] = match[2];
        queryObject['three'] = match[3];
    } else {
        // Fallback to standard query string parsing
        queryObject = url.parse(req.url, true).query;
    }
```

*(When finished: <kbd>Ctrl</kbd> + <kbd>C</kbd> in the terminal to exit)*

##### Go

| **12** :$

```console
go run rewrite-multi-get.go
```

| **B-12** ://

```console
localhost/first-get-arg/second-get-arg/third-get-arg
```

Operative Go code:

```go
    // Rewrite rule: /GET-one-here/GET-two-here/GET-three-here → ?one=GET-one-here&two=GET-two-here&three=GET-three-here
    re := regexp.MustCompile(`^/([^/]+)/([^/]+)/([^/]+)$`)
    if matches := re.FindStringSubmatch(path); len(matches) > 3 {
        values.Set("one", matches[1])
        values.Set("two", matches[2])
        values.Set("three", matches[3])
    } else {
        // Fallback to standard query string parsing
        var err error
        values, err = url.ParseQuery(r.URL.RawQuery)
        if err != nil {
            http.Error(w, "Error parsing query", http.StatusBadRequest)
            return
        }
    }
```

*(When finished: <kbd>Ctrl</kbd> + <kbd>C</kbd> in the terminal to exit)*

### User Profile with Pretty URL Rewrite
*Display user profile based on database entries from our backend app in [Lesson 3](https://github.com/JesseSteele/Codia/blob/master/701/Lesson-03.md)*

#### CSS Additions

| **13** :$

```console
cp core/04-style.css style.css && \
code core/04-style.css
```


#### Update Backend App for Public Profile Link
##### Python

| **14** :$

```console
cp core/04-backend-users-app.py backend-users-app.py && \
cp core/04-profile.py profile.py && \
code core/04-backend-users-app.py core/04-profile.py
```


##### Node.js

| **15** :$

```console
cp core/04-backend-users-app.js backend-users-app.js && \
cp core/04-profile.js profile.js && \
code core/04-backend-users-app.js core/04-profile.js
```


##### Go

| **16** :$

```console
cp core/04-backend-users-app.go backend-users-app.go && \
cp core/04-profile.go profile.go && \
code core/04-backend-users-app.go core/04-profile.go
```


#### Expanded Database Implementations for Public Profile `SELECT`
##### DB Process

| **17** :$

```console
cp core/04-db-process.py db-process.py && \
cp core/04-db-process.js db-process.js && \
cp core/04-db-process.go db-process.go && \
code core/04-db-process.py core/04-db-process.js core/04-db-process.go
```


*Now, see it in action...*

##### Backend Users App
###### Python

| **18** :$

```console
cp sqlite-db.py db.py
cp backend-users-app.py backend.py
python backend.py
```

| **B-18** ://

```console
localhost
```

*(When finished: <kbd>Ctrl</kbd> + <kbd>C</kbd> in the terminal to exit)*

**Per Database**

*You may want to test the other databases, but we will assume SQLite going forward after this lesson:*

| **SQLite Python** :$

```console
cp sqlite-db.py db.py
python backend.py
```

| **MySQL/MariaDB Python** :$

```console
cp mysql-db.py db.py
python backend.py
```

| **PostgreSQL Python** :$

```console
cp postgres-db.py db.py
python backend.py
```

###### Node.js

| **19** :$

```console
cp sqlite-db.js db.js
cp backend-users-app.js backend.js
node backend.js
```

| **B-19** ://

```console
localhost
```

*(When finished: <kbd>Ctrl</kbd> + <kbd>C</kbd> in the terminal to exit)*

**Per Database**

*We've made the point about integrating other databases, and may continue to update those database files in the future*

*MySQL/MariaDB and PostgreSQL need to be running as services via `systemctl start` in order to be accessed; not so with SQLite*

*So, future lessons will not test MySQL/MariaDB and PostgreSQL explicitly; we will assume SQLite going forward after this lesson*

*If you want to test the other databases, use this reference below after running the first `backend.*` test for any language:*

| **Start the services** :$

```console
sudo systemctl start mariadb postgresql
```

| **SQLite Node.js** :$

```console
cp sqlite-db.js db.js
node backend.js
```

| **MySQL/MariaDB Node.js** :$

```console
cp mysql-db.js db.js
node backend.js
```

| **PostgreSQL Node.js** :$

```console
cp postgres-db.js db.js
node backend.js
```

###### Go

| **20** :$

```console
cp sqlite-db.conf db.conf
cp backend-users-app.go backend.go
go run backend.go profile.go
```

*Note we needed to run both `backend.go` and `profile.go` because they are implied via `package main` in both, but that doesn't kick in until compiled; by arguing both in the `go run` command, they both are included by the `run` interpreter*

| **B-20** ://

```console
localhost
```

*(When finished: <kbd>Ctrl</kbd> + <kbd>C</kbd> in the terminal to exit)*

**Per Database**

*You may want to test the other databases, but we will assume SQLite going forward after this lesson:*

| **SQLite Go** :$

```console
cp sqlite-db.conf db.conf
go run backend.go profile.go
```

| **MySQL/MariaDB Go** :$

```console
cp mysql-db.conf db.conf
go run backend.go profile.go
```

| **PostgreSQL Go** :$

```console
cp postgres-db.conf db.conf
go run backend.go profile.go
```

___

# The Take
## URL Rewrites
- Python, Node.js, and Go handle URL rewrites internally
  - No server modifications are needed as PHP needs in Apache's `.conf` site files, or more commonly `.htaccess`
  - These URL rewrites easily convert to GET arguments that can direct where we need to be within our webapp
  - URL rewrites are often called "pretty URLs"
## Separate Files for Same App
- We can create a separate web product (ie: Profile view page as `profile.*`) in a separate file, but incorporate it into the same deployment
  - Python and Node.js need a kind of statement to include the other file, perhaps as a module
  - Go does not `include` the other file, but implies it through the same package, ie `main` in `package main` for any `.go` files in the same directory at run time or compile time
    - Go will need to `go run` both files if not compiled first
  - This is useful so that each different file could be treated as a different web product, such as for different Product Managers or different Products, etc
    - Specializing the organization also prevents different teams from creating complex conflicts for `git` versioning
## Databases
- MySQL/MariaDB and PostgreSQL need to run as services (via `systemctl start ...`)
  - This takes up system resources
- SQLite is simpler for many reasons:
  - It does not need to run as a service, lightening load on the system and creates less work for the SysAdmin
  - It does not need user or password settings, so config files are simpler
- The database processes are also in a separate file for the same reasons as the `profile.*` file
  - This way, a team or developer focused on SQL calls can maintain that file separately from the overall product roadmap
___

#### [Lesson 5: Async and AJAX](https://github.com/JesseSteele/Codia/blob/master/701/Lesson-05.md)