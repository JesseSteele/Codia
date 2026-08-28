# Linux 701
## Lesson 9: API

Ready the CLI

```console
cd ~/School/Codia/701
```

Ready services

```console
sudo systemctl start nginx mariadb postgresql
```

Keep the three SQL tabs from Lesson 8 open.

___

*An API is HTTP with a contract. We will stand up a **host** on its own port (no Nginx) and a **client** that Nginx renders. The client talks to the host over IPv4*

*701bio the web UI stays on `9001` behind Nginx. The API host is a second app*

### Dummy database
*Same three people and three notes in all three engines. Query this first. 701bio's own tables come back later*

| **1** :$

```console
cp core/09-dummy-sqlite.sql dummy-sqlite.sql && \
cp core/09-dummy-mariadb.sql dummy-mariadb.sql && \
cp core/09-dummy-postgres.sql dummy-postgres.sql && \
code core/09-dummy-sqlite.sql core/09-dummy-mariadb.sql core/09-dummy-postgres.sql
```

| **S2** :>

```sql
.read dummy-sqlite.sql
SELECT name, city FROM people;
```

| **S0** :>

```sql
source dummy-mariadb.sql;
SELECT name, city FROM people;
```

| **2** :$

```console
sudo -u postgres dropdb --if-exists backendapp_db
sudo -u postgres createdb backendapp_db
sudo -u postgres psql -d backendapp_db -c "CREATE USER beadbuser WITH PASSWORD 'beadbpass'; GRANT ALL PRIVILEGES ON DATABASE backendapp_db TO beadbuser;"
psql -U postgres -d backendapp_db -f dummy-postgres.sql
```

| **S1** :>

```sql
\c backendapp_db
SELECT name, city FROM people;
```

### Config (not hardcoded URLs)

| **3** :$

```console
cp core/09-config.sample config.sample && \
code core/09-config.sample
```

*Web UI, API host, and client each have commented examples: `example.tld`, `api.example.tld`, `example.tld/701bio`, `192.168.1.50:9002`, `[2001:db8::50]:9002`*

*The live file will be `/etc/701bio/config` (the only file in that directory). Lesson 12 links it into `/srv/www/701bio/config` or `/var/www/701bio/config`*

### API host — Python + PostgreSQL

| **4-p** :$

```console
cp core/09-api.py api.py && \
cp core/09-db1.py db.py && \
cp core/05-db-process.py db_process.py && \
code core/09-api.py
```

| **5-p** :$

```console
python api.py
```

*Host binds `0.0.0.0:9002`. No Nginx on this port*

| **6** :$

```console
cp core/09-api-demo.sh api-demo.sh && \
chmod +x api-demo.sh && \
code core/09-api-demo.sh
```

| **7** :$

```console
./api-demo.sh
```

*JSON people, JSON notes, a POST, then XML*

*Play: `curl -s http://127.0.0.1:9002/api/people`*

### API host — Node + MariaDB

| **4-n** :$

```console
cp core/09-api.js api.js && \
cp core/09-db1.js db.js && \
cp core/05-db-process.js db-process.js && \
code core/09-api.js
```

| **5-n** :$

```console
node api.js
```

### API host — Go + SQLite

| **4-g** :$

```console
cp core/09-api.go api.go && \
cp core/05-db-process.go db-process.go && \
cp core/09-db1.conf db.conf && \
code core/09-api.go
```

| **5-g** :$

```console
go run api.go db-process.go
```

*Or choose any **one** of the other database options... (alternatively, choose one)*

| **10-pm** :$

```console
cp core/09-db2.py db.py && \
python api.py
```

| **10-ps** :$

```console
cp core/09-db3.py db.py && \
python api.py
```

| **10-np** :$

```console
cp core/09-db2.js db.js && \
node api.js
```

| **10-ns** :$

```console
cp core/09-db3.js db.js && \
node api.js
```

| **10-gm** :$

```console
cp core/09-db2.conf db.conf && \
go run api.go db-process.go
```

| **10-gp** :$

```console
cp core/09-db3.conf db.conf && \
go run api.go db-process.go
```

### RSA keypair (second layer)

*LENG snakeoil is already RSA. Same tool, a pair for the API host to verify the client*

| **11** :$

```console
openssl genrsa -out 701bio.pem 2048
openssl rsa -in 701bio.pem -pubout -out 701bio.pub.pem
code 701bio.pub.pem
```

*Host loads `701bio.pub.pem`. Client signs `METHOD\\nPATH\\nBODY` with the private PEM (`openssl dgst -sha256 -sign`) and sends `X-701bio-Sig` (base64)*

| **12** :$

```console
BODY='title=signed&body=rsa'
SIG=$(printf 'POST\n/api/notes\n%s' "$BODY" | openssl dgst -sha256 -sign 701bio.pem | base64 -w0)
BIO_REQUIRE_RSA=1 python api.py
```

*In another terminal:*

```console
curl -s -X POST -H "X-701bio-Sig: $SIG" --data "$BODY" http://127.0.0.1:9002/api/notes
```

*Omit the header → `{ "ok": false, "error": "bad signature" }` when `BIO_REQUIRE_RSA=1`*

### Client sandbox (Nginx on port 8080)

*Yes: two Nginx configs can run at once if they `listen` on different ports. LENG's "only one at a time" is because those samples all wanted port 80*

| **13** :$

```console
cp core/09-client.html client.html && \
cp core/09-client.py client.py && \
cp core/09-client.js client.js && \
cp core/09-client.go client.go && \
cp core/09-client.conf 701bio-client.conf && \
code core/09-client.html core/09-client.py core/09-client.conf
```

| **14** :$

```console
sudo cp 701bio-client.conf /etc/nginx/conf.d/701bio-client.conf && \
sudo ln -sfn /etc/nginx/conf.d/701bio-client.conf /etc/nginx/enabled.d/701bio-client.conf && \
sudo nginx -t && \
sudo systemctl reload nginx
```

| **15-p** :$

```console
python client.py
```

| **B-15** :// **client**

```console
localhost:8080
```

*Buttons GET people / GET notes. Form POST. Two `<pre>` blocks: the XML/JSON that was sent, and the XML/JSON that came back. The JS `fetch` target is `http://127.0.0.1:9002` — IPv4 to the host, not through Nginx*

| **15-n** :$

```console
node client.js
```

| **15-g** :$

```console
go run client.go
```

*The tiny Python/Node/Go client serves the same `client.html` and can POST `/save` to write the last body under `api-responses/` and into SQL*

### 701bio tables again
*Dummy was for the contract. The product API still talks to 701bio notes and profile. Run the Lesson 8 installer again (tables were dropped at the end of 8)*

| **16-p** :$

```console
cp core/08-bio.py bio.py && \
cp core/05-db1.py db.py && \
python bio.py
```

| **B-16** ://

```console
localhost/install
```

<!-- Update Databases for new table for developer keys, required for API access -->
<!-- Create web UI for logged-in admins to view Developer Dash and create API keys -->
<!-- Send both JSON and POST-XML requests to and from API for each language -->
<!-- CLI Tool, as lightweight as possible: Make BASH script with proper functions to handle server-side API queries, interacting with the API on the server from the CLI through a bash script with args; responses in text/terminal columns by default, flag options for JSON or XML responses, use saved file or heredoc for long CLI inputs -->
<!-- API interacts with the Web app from Lesson 8, getting avatar, bio fields, links, shared file URL, and specific notes based on slug. -->

___

# The Take
## Three apps
- Web UI: Nginx → `9001`
- API host: IPv4 `:9002`, no Nginx
- Client: Nginx `:8080` → `9003`
## Pairing
- Node + MariaDB, Python + PostgreSQL, Go + SQLite
- Engine is `db.*`, not a rewrite of `api.*`
## RSA
- `openssl genrsa` 2048, PEM on disk
- `X-701bio-Sig` over `METHOD\\nPATH\\nBODY`
## Config
- URLs live in config, not in the client source
___

#### [Lesson 10: Frontend State Intergration](https://github.com/JesseSteele/Codia/blob/master/701/Lesson-10.md)
