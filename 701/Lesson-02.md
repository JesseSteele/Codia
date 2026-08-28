# Linux 701
## Lesson 2: Method & RegEx Handling

Ready the CLI

```console
cd ~/School/Codia/701
```

Ready services

```console
sudo systemctl start nginx
```

___

*Before `<form>` handling, let's have a look at what our three server languages see...*

### Basic `GET` dump
*These three backend programs will dump all GET values based on the URL you use*

*These use `:9001` in the URL, so Nginx won't matter*

#### Python

| **1** :$

```console
cp core/02-dump-get.py dump-get.py && \
code core/02-dump-get.py
```


| **2** :$

```console
python dump-get.py
```

| **B-3** ://

```console
localhost:9001?ONE=one&TWO=two&THREE=three
```

*Feel free to hack the GET line `ONE=one&TWO=two&THREE=three` to make modifications*

*(When finished: <kbd>Ctrl</kbd> + <kbd>C</kbd> in the terminal to exit)*

#### Node.js

| **4** :$

```console
cp core/02-dump-get.js dump-get.js && \
code core/02-dump-get.js
```


| **5** :$

```console
node dump-get.js
```

| **B-5** ://

```console
localhost:9001?ONE=one&TWO=two&THREE=three
```

*Feel free to hack the GET line `ONE=one&TWO=two&THREE=three` to make modifications*

*(When finished: <kbd>Ctrl</kbd> + <kbd>C</kbd> in the terminal to exit)*

#### Go

| **6** :$

```console
cp core/02-dump-get.go dump-get.go && \
code core/02-dump-get.go
```


| **7** :$

```console
go run dump-get.go
```

| **B-7** ://

```console
localhost:9001?ONE=one&TWO=two&THREE=three
```

*Feel free to hack the GET line `ONE=one&TWO=two&THREE=three` to make modifications*

*(When finished: <kbd>Ctrl</kbd> + <kbd>C</kbd> in the terminal to exit)*

### Port vs Proxy

*Each of the browser URLs above denoted the port with `:9001`*

*Because we are using Nginx to redirect web traffic to port `9001`, we actually don't need this in our URL*

*Note that Nginx is running...*

| **8** :$

```console
sudo systemctl status nginx
```

*Stop Nginx...*

| **9** :$

```console
sudo systemctl stop nginx
```

*Start any **one** of the backend scripts... (choose one)*

| **10-p** :$

```console
python dump-get.py
```

| **10-n** :$

```console
node dump-get.js
```

| **10-g** :$

```console
go run dump-get.go
```

*Now try any of the backend scripts with a `:9001`-GET URL again...*

| **B-10** ://

```console
localhost:9001?ONE=one&TWO=two&THREE=three
```

*They should work because the backend service is running on port `9001` and we specified that in our URL with `:9001`*

*Now try any of the backend scripts with only `localhost`-GET URL...*

| **B-11** ://

```console
localhost/?ONE=one&TWO=two&THREE=three
```

*(When finished: <kbd>Ctrl</kbd> + <kbd>C</kbd> in the terminal to exit)*

*They shouldn't work because browsers listen on ports `80` and `443`, but nothing is running that listens to those ports (since Nginx is turned off)*

*Turn on Nginx to listen to port `443` and to port `80` that redirects to port `443`...*

| **12** :$

```console
sudo systemctl start nginx
```

*Start any **one** of the backend scripts... (choose one)*

| **12-p** :$

```console
python dump-get.py
```

| **12-n** :$

```console
node dump-get.js
```

| **12-g** :$

```console
go run dump-get.go
```

*Again, try any of the backend scripts with only `localhost`-GET URL...*

| **B-12** ://

```console
localhost/?ONE=one&TWO=two&THREE=three
```

*Note that the Nginx reverse proxy handles the normal browser traffic, then passes it to port `9001` so that the URL doesn't need to include `:9001`*

*Any time you have a backend service running on a port, Nginx can probably just pass web traffic to it*

*From here on out, we will use Nginx for our proxy pass*

### Handling specific GET `<form>` items
*Basic HTML `<form>` backend services that handle GET requests*

#### Python

| **13** :$

```console
cp core/02-form-get.py form-get.py && \
code core/02-form-get.py
```


| **14** :$

```console
python form-get.py
```

| **B-14** ://

```console
localhost
```

*(When finished: <kbd>Ctrl</kbd> + <kbd>C</kbd> in the terminal to exit)*

#### Node.js

| **15** :$

```console
cp core/02-form-get.js form-get.js && \
code core/02-form-get.js
```


| **16** :$

```console
node form-get.js
```

| **B-16** ://

```console
localhost
```

*(When finished: <kbd>Ctrl</kbd> + <kbd>C</kbd> in the terminal to exit)*

#### Go

| **17** :$

```console
cp core/02-form-get.go form-get.go && \
code core/02-form-get.go
```


| **18** :$

```console
go run form-get.go
```

| **B-18** ://

```console
localhost
```

*(When finished: <kbd>Ctrl</kbd> + <kbd>C</kbd> in the terminal to exit)*

### Handling specific POST `<form>` items
*Basic HTML `<form>` backend services that handle POST requests*

*Note in all three examples, when we POST, we don't want the `<form action=` to be `localhost` because that would result in `localhost/localhost`*

*In GET, we would receive the full and proper URL with `<form action="localhost"`*

*In POST, we need `<form action="/"`*

#### Python

| **19** :$

```console
cp core/02-form-post.py form-post.py && \
code core/02-form-post.py
```


| **20** :$

```console
python form-post.py
```

| **B-20** ://

```console
localhost
```

*(When finished: <kbd>Ctrl</kbd> + <kbd>C</kbd> in the terminal to exit)*

*Note that the Python code only identifies POST entries that have values*

*Node.js and Go will see the POST entry from the `<form>` even if the `<input>` is empty*

*Because of this, Python needed some more logic to handle various scenarios; look for comments:*
- *`## If POST, but no entries, say so`*
- *`## If no POST, leave empty`*
- *`# With no form, say so`*

*Python also needs to process a no-POST (AKA GET) scenario, without that `def do_GET(self)` block, Python could serve the filesystem of the PWD where the `.py` file was running*

*This serves as an example of how complex it can be for Python to handle POST vs GET requests*

#### Node.js

| **21** :$

```console
cp core/02-form-post.js form-post.js && \
code core/02-form-post.js
```


| **22** :$

```console
node form-post.js
```

| **B-22** ://

```console
localhost
```

*(When finished: <kbd>Ctrl</kbd> + <kbd>C</kbd> in the terminal to exit)*

#### Go

| **23** :$

```console
cp core/02-form-post.go form-post.go && \
code core/02-form-post.go
```


| **24** :$

```console
go run form-post.go
```

| **B-24** ://

```console
localhost
```

*(When finished: <kbd>Ctrl</kbd> + <kbd>C</kbd> in the terminal to exit)*

<!-- DEV: POST forms must print success on POST, not only after a GET reload -->
### Validate & Sanitize with RegEx
*Basic RegEx checks for POST submissions*

#### Basic valida-sanitize-quote
##### Python

| **25** :$

```console
cp core/02-regex-post.py regex-post.py && \
code core/02-regex-post.py
```


| **26** :$

```console
python regex-post.py
```

| **B-26** ://

```console
localhost
```

*(When finished: <kbd>Ctrl</kbd> + <kbd>C</kbd> in the terminal to exit)*

##### Node.js

| **27** :$

```console
cp core/02-regex-post.js regex-post.js && \
code core/02-regex-post.js
```


| **28** :$

```console
node regex-post.js
```

| **B-28** ://

```console
localhost
```

*(When finished: <kbd>Ctrl</kbd> + <kbd>C</kbd> in the terminal to exit)*

##### Go

| **29** :$

```console
cp core/02-regex-post.go regex-post.go && \
code core/02-regex-post.go
```


| **30** :$

```console
go run regex-post.go
```

| **B-30** ://

```console
localhost
```

*(When finished: <kbd>Ctrl</kbd> + <kbd>C</kbd> in the terminal to exit)*

#### With `.error` styling
*Basic CSS styling for success messages and errors*

| **31** :$

```console
cp core/02-style.css style.css && \
code core/02-style.css
```


*...That gets included by simply making our HTML document start with this:*

```html
<!DOCTYPE html>
<html>
<head>
    <link rel="stylesheet" href="style.css">
</head>
<body>
```

*Also, these three examples will dynamically add `class="error"` to the `<form>` through a replace function*

| **Python** :

```py
for key in errors.keys():
    html_form = html_form.replace(f'name="{key}"', f'name="{key}" class="error"')
```

| **Node.js** :

```js
for (let key in errors) {
    htmlForm = htmlForm.replace(new RegExp(`name="${key}"`, 'g'), `name="${key}" class="error"`);
}
```

| **Go** :

```go
for key := range errors {
    htmlForm = strings.Replace(htmlForm, fmt.Sprintf(`name="%s"`, key), fmt.Sprintf(`name="%s" class="error"`, key), -1)
}
```

*This `style.css` file does **NOT** need to be in the web directory, just in the same directory as the `.py`/`.js`/`.go` file calling it*

*(This is one of the beautiful things about such languages as these running a web app on its own port server behind Nginx)*

##### Python

| **32** :$

```console
cp core/02-regex-post-css.py regex-post-css.py && \
code core/02-regex-post-css.py
```


| **33** :$

```console
python regex-post-css.py
```

| **B-33** ://

```console
localhost
```

*(When finished: <kbd>Ctrl</kbd> + <kbd>C</kbd> in the terminal to exit)*

##### Node.js

| **34** :$

```console
cp core/02-regex-post-css.js regex-post-css.js && \
code core/02-regex-post-css.js
```


| **35** :$

```console
node regex-post-css.js
```

| **B-35** ://

```console
localhost
```

*(When finished: <kbd>Ctrl</kbd> + <kbd>C</kbd> in the terminal to exit)*

##### Go

| **36** :$

```console
cp core/02-regex-post-css.go regex-post-css.go && \
code core/02-regex-post-css.go
```


| **37** :$

```console
go run regex-post-css.go
```

| **B-37** ://

```console
localhost
```

*(When finished: <kbd>Ctrl</kbd> + <kbd>C</kbd> in the terminal to exit)*

#### With `<form>` & function building
*Replacing to add a `class="error"` attribute in a static `<form>` is fun, but it would be better to build each `<input>` with a function that takes errors into consideration*

*Each example will use a `functions.*` file to check POST content and to render a `<form>`. This will generate `<form> <input>` with both `vlaue=` content and any errors*

*The overall file for the rendered page will be smaller so you can see more of the backend language doing the lifting of running a backend web app*

##### Python

| **38** :$

```console
cp core/02-functions.py functions.py && \
cp core/02-regex-post-function.py regex-post-function.py && \
code core/02-functions.py core/02-regex-post-function.py
```


| **39** :$

```console
python regex-post-function.py
```

| **B-39** ://

```console
localhost
```

*(When finished: <kbd>Ctrl</kbd> + <kbd>C</kbd> in the terminal to exit)*

##### Node.js

| **40** :$

```console
cp core/02-functions.js functions.js && \
cp core/02-regex-post-function.js regex-post-function.js && \
code core/02-functions.js core/02-regex-post-function.js
```

*Note we use `.js` as the extension for the `functions.*` file so we don't need to specify the file extension in the main Node script*


| **41** :$

```console
node regex-post-function.js
```

| **B-41** ://

```console
localhost
```

*(When finished: <kbd>Ctrl</kbd> + <kbd>C</kbd> in the terminal to exit)*

##### Go

| **42** :$

```console
cp core/02-functions.go functions.go && \
cp core/02-regex-post-function.go regex-post-function.go && \
code core/02-functions.go core/02-regex-post-function.go
```


| **43** :$

```console
go run regex-post-function.go functions.go
```

| **B-43** ://

```console
localhost
```

*(When finished: <kbd>Ctrl</kbd> + <kbd>C</kbd> in the terminal to exit)*

___

# The Take
- Go does not need to specify importing other files in the same directory that use `package main`, unlike Python and Node.js
- Nginx can forward an app (Go, Node, Python, etc) running on a port to the standard domain
  - Running on port `9001` normally needs `domain.tld:9001`
  - Nginx uses `proxy_pass http://127.0.0.1:9001;` so `domain.tld` works just the same
- Go, Python, and Node.js can parse GET and POST methods
  - They all need to be prepared before use, unlike PHP
  - Python:
    - Parse the HTTP request:
      - `post_data = self.rfile.read(int(content_length))`
      - `query_components = urllib.parse.parse_qs(post_data.decode())`
    - `<input name="input_name"` = `query_components['fullname'][0]`
  - Node.js:
    - Parse the HTTP request:
      - `req.on('data', chunk => { body += chunk.toString(); });`
      - `const queryObject = querystring.parse(body);`
    - `<input name="input_name"` = `queryObject.input_name`
  - Go:
    - Parse the HTTP request: `func handler(w http.ResponseWriter, r *http.Request)`
    - `values, err := url.ParseQuery(r.URL.RawQuery)` (parse GET from URL)
    - `values := r.PostForm` (parse POST, safe)
      - Both `values` above: `<input name="input_name"` = `values["input_name"]`
    - `if len(r.PostForm) == 0 {` (test for FORM POST presence)
    - `if len(r.Form) == 0 {` (test for FORM presence, whether POST or GET)
    - `if r.Method == "POST" { ... }` (test for POST request method)
    - `if r.Method == "GET" { ... }` (test for GET request method)
      - All `if` above: `<input name="input_name"` = `r.FormValue("input_name")` only from FORM, whether POST or GET
      - Unsafe direct use of `PostForm`: `<input name="input_name"` = `r.PostForm["input_name"][0]` only from POST
        - That `[0]` key on the end is necessary because of how `PostForm` works, not with `FormValue`
        - This could cause a runtime panic in Go if not properly tested for `nil[0]`
___

#### [Lesson 3: Database Connections](https://github.com/JesseSteele/Codia/blob/master/701/Lesson-03.md)