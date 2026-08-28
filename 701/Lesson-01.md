# Linux 701
## Lesson 1: Web Hosting & Proxied Services

Ready the CLI

```console
cd ~/School/Codia/701
```
___

### Python Server
#### Port `9001`

| **1** :$

```console
cp core/01-hw1.py hw1.py && \
code core/01-hw1.py
```

*Note:*
- *We use `BaseHTTPRequestHandler` and `HTTPServer` Python `http.server` classes*
- *`port` is assigned to `9001`*
- *`localhost` is defined by using `127.0.0.1`*
- *Loading `BaseHTTPRequestHandler` requires a little more custom code and takes less advantage of Python's built-in HTTP handling code*



*Note that we don't use `sudo` to run this `.py` app because it uses a **non-privileged port** (above `1024`)*

| **2** :$

```console
python hw1.py
```

| **B-2** ://

```console
localhost:9001
```

*(When finished: <kbd>Ctrl</kbd> + <kbd>C</kbd> in the terminal to exit)*

*Note how the terminal responds each time you access or refresh the webpage*

*You could merely change the `PORT` constant to `80` in the above code, but we will show a little more of Python's capability with a slightly different code...*

#### Port `80` for HTTP

| **3** :$

```console
cp core/01-hw2.py hw2.py && \
code core/01-hw2.py
```

*Note:*
- *`port` is assigned to `80` for normal browser use*
- *Using port `80` requires permissions, so `python` must be run with `sudo`*
  - *This is **dangerous** and can create **security vulnerabilities** on a production server*
  - *Use for learning purposes only!*
- *Loading `socketserver` takes advantage of Python's built-in HTTP handling for the `with socketserver...` statement*



| **3** :$

```console
sudo python hw2.py
```

*Note no port is specified because it uses the default port `80`*

| **B-3** ://

```console
localhost
```

*(When finished: <kbd>Ctrl</kbd> + <kbd>C</kbd> in the terminal to exit)*

*You could change the `PORT` constant to `9001` to use `localhost:9001` as in our first example from `hw1.py`*

*To use SSL, we need to do more than change `PORT` to `443`...*

#### Port `443` for HTTPS-SSL

| **4** :$

```console
cp core/01-hw3.py hw3.py && \
code core/01-hw3.py
```

*Note:*
- *`port` is assigned to `443` for SSL*
- *Loading `ssl` allows us to use SSL features (noted with comments)*



| **5** :$

```console
sudo python hw3.py
```

*Note no port is specified because it uses the default port `443`*

*With this address you will get an SSL security warning from your browser*
  - *You can view the certificate for `O=Snakeoil/OU=Learning/CN=myComputer` as we created in our [LENG Desktop](https://github.com/JesseSteele/Codia/blob/master/Cheat-Sheets/LENG-Desktop.md)*

| **B-5** ://

```console
https://localhost
```

*(When finished: <kbd>Ctrl</kbd> + <kbd>C</kbd> in the terminal to exit)*

*Changing the `PORT` constant to anything but `443` won't work because this uses SSL*

*Let's include our [Diffie-Hellman Group](https://en.wikipedia.org/wiki/Diffie%E2%80%93Hellman_key_exchange) key file in this SSL config to see how that looks...*

#### SSL with DH

| **6** :$

```console
cp core/01-hw4.py hw4.py && \
code core/01-hw4.py
```

*Note everything is the same as `hw3.py` for SSL, except we have two `dhparams` statements (noted with `DH` comments)*


*SSL with DH*


| **7** :$

```console
sudo python hw4.py
```

| **B-7** ://

```console
https://localhost
```

*(When finished: <kbd>Ctrl</kbd> + <kbd>C</kbd> in the terminal to exit)*

### Node Server
#### Port `9001`

| **8** :$

```console
cp core/01-hw1.js hw1.js && \
code core/01-hw1.js
```

*Note:*
- *We use Node's built-in `http` module*
- *`port` is assigned to `9001`*
- *`localhost` is defined by using `127.0.0.1`*
- *Loading `http` brings native tools in Node for an HTTP server*



*Note that we don't use `sudo` to run this `.js` app because it uses a **non-privileged port** (above `1024`)*

| **9** :$

```console
node hw1.js
```

| **B-9** ://

```console
localhost:9001
```

*(When finished: <kbd>Ctrl</kbd> + <kbd>C</kbd> in the terminal to exit)*

*Note how the terminal responds each time you access or refresh the webpage*

*You could merely change the `PORT` constant to `80` in the above code, but we will show a little more of Node's capability with a slightly different code...*

#### Port `80` for HTTP

| **10** :$

```console
cp core/01-hw2.js hw2.js && \
code core/01-hw2.js
```

*Note:*
- *`port` is assigned to `80` for normal browser use*
- *Using port `80` requires permissions, so `node` must be run with `sudo`*
  - *This is **dangerous** and can create **security vulnerabilities** on a production server*
  - *Use for learning purposes only!*
- *This implements arrow functions, which isn't quite so old-school Node*
- *Running `server.listen` organizes things a little better*
- *Running `server.close` makes for a more graceful shutdown of the server*



| **11** :$

```console
sudo node hw2.js
```

*Note no port is specified because it uses the default port `80`*

| **B-11** ://

```console
localhost
```

*(When finished: <kbd>Ctrl</kbd> + <kbd>C</kbd> in the terminal to exit)*

*You could change the `PORT` constant to `9001` to use `localhost:9001` as in our first example from `hw1.js`*

*To use SSL, we need to do more than change `PORT` to `443`...*

#### Port `443` for HTTPS-SSL

| **12** :$

```console
cp core/01-hw3.js hw3.js && \
code core/01-hw3.js
```

*Note:*
- *`port` is assigned to `443` for SSL*
- *Loading `https` allows us to use SSL features (noted with comments)*



| **13** :$

```console
sudo node hw3.js
```

*Note no port is specified because it uses the default port `443`*

*With this address you will get an SSL security warning from your browser*
  - *You can view the certificate for `O=Snakeoil/OU=Learning/CN=myComputer` as we created in our [LENG Desktop](https://github.com/JesseSteele/Codia/blob/master/Cheat-Sheets/LENG-Desktop.md)*

| **B-13** ://

```console
https://localhost
```

*(When finished: <kbd>Ctrl</kbd> + <kbd>C</kbd> in the terminal to exit)*

*Changing the `PORT` constant to anything but `443` won't work because this uses SSL*

*Let's include our [Diffie-Hellman Group](https://en.wikipedia.org/wiki/Diffie%E2%80%93Hellman_key_exchange) key file in this SSL config to see how that looks...*

#### SSL with DH

| **14** :$

```console
cp core/01-hw4.js hw4.js && \
code core/01-hw4.js
```

*Note everything is the same as `hw3.js` for SSL, except we have one `dhparams` statement (noted with a DH comment)*


*SSL with DH*


| **15** :$

```console
sudo node hw4.js
```

| **B-15** ://

```console
https://localhost
```

*(When finished: <kbd>Ctrl</kbd> + <kbd>C</kbd> in the terminal to exit)*

### Go Server
*Go is different from Python and Node*
  - *Go compiles as a stand-alone program and does not need Python or Node-V8 to be running in order for it to execute*
  - *We will use a no-compile command (`go run somefile.go`) so Go will compile, store the compiled file in a temp directory, then execute it*
  - *Go should normally be compiled first for a production server (`go build somefile.go`)*

#### Port `9001`

| **16** :$

```console
cp core/01-hw1.go hw1.go && \
code core/01-hw1.go
```

*Note:*
- *We use Go's `net` package to listen on TCP, then write a tiny HTTP response by hand*
- *`port` is assigned to `9001`*
- *`localhost` is defined by using `127.0.0.1`*
- *Loading `net` brings us tools in Go for an HTTP server*



*Note that we don't use `sudo` to run this `.go` app because it uses a **non-privileged port** (above `1024`)*

| **17** :$

```console
go run hw1.go
```

| **B-17** ://

```console
localhost:9001
```

*(When finished: <kbd>Ctrl</kbd> + <kbd>C</kbd> in the terminal to exit)*

*Note how the terminal responds each time you access or refresh the webpage*

*You could merely change the `PORT` constant to `80` in the above code, but we will show a little more of Go's capability with a slightly different code...*

#### Port `80` for HTTP

| **18** :$

```console
cp core/01-hw2.go hw2.go && \
code core/01-hw2.go
```

*Note:*
- *`port` is assigned to `80` for normal browser use*
- *Using port `80` requires permissions, so `go` must be run with `sudo`*
  - *This is **dangerous** and can create **security vulnerabilities** on a production server*
  - *Use for learning purposes only!*
- *Loading `net/http` takes advantage of Go's built-in HTTP handling so we don't need to create loops*
- *Loading `log` takes advantage of Go's built-in error logging so we don't need an `if` statement to trigger errors*



| **19** :$

```console
sudo go run hw2.go
```

| **B-19** ://

```console
localhost
```

*(When finished: <kbd>Ctrl</kbd> + <kbd>C</kbd> in the terminal to exit)*

#### Port `443` for HTTPS-SSL

| **20** :$

```console
cp core/01-hw3.go hw3.go && \
code core/01-hw3.go
```

*Note:*
- *`port` is assigned to `443` for SSL*
- *Loading `crypto/tls` allows us to use SSL features (noted with comments)*



| **21** :$

```console
sudo go run hw3.go
```

*Note no port is specified because it uses the default port `443`*

*With this address you will get an SSL security warning from your browser*
  - *You can view the certificate for `O=Snakeoil/OU=Learning/CN=myComputer` as we created in our [LENG Desktop](https://github.com/JesseSteele/Codia/blob/master/Cheat-Sheets/LENG-Desktop.md)*

| **B-21** ://

```console
https://localhost
```

*(When finished: <kbd>Ctrl</kbd> + <kbd>C</kbd> in the terminal to exit)*

*Changing the `PORT` constant to anything but `443` won't work because this uses SSL*

*Let's include our [Diffie-Hellman Group](https://en.wikipedia.org/wiki/Diffie%E2%80%93Hellman_key_exchange) key file in this SSL config to see how that looks...*

#### SSL with DH
*We don't need this because Go already has this and used it in `hw3.go` from the ECDHE cyphers, which is another cool thing about Go*

### Nginx Reverse-Proxy Server
The more efficient web server

*While these three **backend** (server-side) languages are capable of handling their own web service for HTTP (`80`) and HTTPS (`443`), Nginx is much better equipped for a few reasons:*
- *Terminating SSL connections*
- *Handling high traffic from the web*
- *Serving more than one website or app*
  - *If on ports `80` or `443`, such a Python, Node.js, or Go app might be the only website allowed on the entire server*

*As a base of reference, this is what a basic reverse-proxy single-file config for Nginx could be...*

| **21a** :$

```console
cp core/01-nginx-reverse-proxy.conf nginx-reverse-proxy.conf && \
code core/01-nginx-reverse-proxy.conf
```

*...but we aren't actually using this Nginx config because we have the two-part configs from our [LENG Desktop](https://github.com/JesseSteele/Codia/blob/master/Cheat-Sheets/LENG-Desktop.md) configuration*

*This is the short version, only with the `server` blocks for port `443` and port `80` redirecting, and passing as a proxy to internal port `9001` for our Python/Node.js/Go app...*

| **21b** :$

```console
cp core/01-rphttps.conf rphttps.conf && \
code core/01-rphttps.conf
```

*Assuming the [LENG Desktop](https://github.com/JesseSteele/Codia/blob/master/Cheat-Sheets/LENG-Desktop.md), we will move the reverse-proxy SSL Nginx config into place...*

| **22** :$

```console
sudo ln -sfn /etc/nginx/conf.d/rphttps.conf /etc/nginx/enabled.d/active.conf
```

*Start our Nginx server for this configuration*

| **23** :$

```console
sudo systemctl start nginx
```

### Proxied Services
Apps that run behind an Nginx reverse-proxy server

*Write our Python, Node.js, and Go web apps to run behind an Nginx reverse-proxy web server*
- *Note that when running behind an Nginx reverse-proxy server, these apps no longer need to address incoming traffic from the web*
- *These will be much like slimmed-down versions of the `hw1.*` and `hw2.*` examples above*
- *These basic "Hello World!" apps only run on the internal server, in these examples using port `9001`*
- *Developing our web apps as **proxied services** like these are what we will build on in the lessons ahead*

#### Python

| **24** :$

```console
cp core/01-hwrp.py hwrp.py && \
code core/01-hwrp.py
```


| **25** :$

```console
sudo python hwrp.py
```

| **B-25** ://

```console
https://localhost
```

*(When finished: <kbd>Ctrl</kbd> + <kbd>C</kbd> in the terminal to exit)*

#### Node.js

| **26** :$

```console
cp core/01-hwrp.js hwrp.js && \
code core/01-hwrp.js
```


| **27** :$

```console
sudo node hwrp.js
```

| **B-27** ://

```console
https://localhost
```

*(When finished: <kbd>Ctrl</kbd> + <kbd>C</kbd> in the terminal to exit)*

#### Go

| **28** :$

```console
cp core/01-hwrp.go hwrp.go && \
code core/01-hwrp.go
```


| **29** :$

```console
sudo go run hwrp.go
```

| **B-29** ://

```console
https://localhost
```

*(When finished: <kbd>Ctrl</kbd> + <kbd>C</kbd> in the terminal to exit)*

*For future lessons, we don't need HTTPS specifically, so let's go back to our non-SSL Nginx config...*

| **30** :$

```console
sudo ln -sfn /etc/nginx/conf.d/rphttp.conf /etc/nginx/enabled.d/active.conf
```

| **31** :$

```console
sudo systemctl restart nginx
```

___

# The Take
- Python, Node.js, and Go support their own web servers
  - Serving web apps
  - SSL/TLS
  - [Diffie-Hellman](https://en.wikipedia.org/wiki/Diffie%E2%80%93Hellman_key_exchange) keys
    - Go handles this natively with ECDHE cyphers; other languages need it declared explicitly
- Nginx is still more efficient
  - High-traffic
  - More than one web app on the same server
- Go scripts compile into their own binaries that need no additional installation on the server
  - There is a way to run Go apps with a single command, but this still compiles the script before running
  - Developing or compiling Go apps requires the Go package to be installed for compiling purposes
- Python and Node.js require their own packages to run scripts
- Nginx can serve statick Text/HTML files
- Nginx can proxy web traffic to internal "**proxied services**"
- Python, Node.js, and Go can be used to write **proxied services**, even to be served on the web through Nginx

___

#### [Lesson 2: Method & RegEx Handling](https://github.com/JesseSteele/Codia/blob/master/701/Lesson-02.md)