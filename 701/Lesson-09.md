# Linux 701
## Lesson 9: API

Ready the CLI

```console
cd ~/School/Codia/701
```

Ready services

```console
sudo systemctl start nginx
```

Keep 701bio running in another terminal (`python bio.py` / `node bio.js` / `go run bio.go db-process.go`)
___

*An API is just HTTP with a contract. 701bio already has one. We are not adding routes today — we are **calling** them*

| Method | Path | Auth | Body |
| --- | --- | --- | --- |
| GET | `/api/profile` | `X-Api-Key` or `?key=` | JSON out |
| GET | `/api/notes` | same | JSON out |
| POST | `/api/notes` | same | `title`, `body` form POST |

*JSON out, POST in. Both, every language*

### Make a key
| **B-1** :// (logged in)

```console
localhost/dev
```

*Create a key. Copy the full hex. You will not see it again*

*Play: create a second key with a different name. The list shows `…` plus four hint characters*

### JSON GET

| **2** :$

```console
cp core/09-api-demo.sh api-demo.sh && \
chmod +x api-demo.sh && \
code core/09-api-demo.sh
```

*Note the script is BASH. The API does not care which language the server is*

| **3** :$ *(put your key as the first argument)*

```console
./api-demo.sh YOUR_KEY_HERE
```

*You should see JSON for profile, JSON for notes, then a POST that writes `notes/api-note.md` (slug from the title)*

*Confirm on disk:*

```console
ls notes
cat notes/*.md
```

*Play: omit the key — JSON `{ "ok": false, "error": "bad api key" }`*

```console
curl -s http://127.0.0.1:9001/api/profile
```

*Play: GET in the browser with a query string*

| **B-4** ://

```console
localhost/api/profile?key=YOUR_KEY_HERE
```

*Same payload as `X-Api-Key`. Machines like headers. Browsers like `?key=`*

### POST from curl
*The backend already accepts POST. You do not rewrite `bio.*`*

```console
curl -X POST -H "X-Api-Key: YOUR_KEY_HERE" \
  --data-urlencode "title=From curl" \
  --data-urlencode "body=Still just HTTP." \
  http://127.0.0.1:9001/api/notes
```

*Then open the public profile, click Notes — the new file is there if `public: true` (API notes are public so you can see them)*

### What the server does with the key
*It hashes the key the same way as the password (`sha256` of `701bio` + the key) and looks up `api_keys.key_hash`*

*The raw key is shown once at create time. SQL keeps the hash plus a four-character hint*

Operative Python:

```py
        h = hash_pass(key)
        cur.execute("SELECT user_id FROM api_keys WHERE key_hash=?", (h,))
```

<!-- Update Databases for new table for developer keys, required for API access -->

<!-- Create web UI for logged-in admins to view Developer Dash and create API keys -->

<!-- Send both JSON and POST-XML requests to and from API for each language -->

<!-- CLI Tool, as lightweight as possible: Make BASH script with proper functions to handle server-side API queries, interacting with the API on the server from the CLI through a bash script with args; responses in text/terminal columns by default, flag options for JSON or XML responses, use saved file or heredoc for long CLI inputs -->

<!-- API interacts with the Web app from Lesson 8, getting avatar, bio fields, links, shared file URL, and specific notes based on slug. -->

___

# The Take
## API
- Same app, extra paths
- A key is a second password stored hashed in SQL (`api_keys`)
- JSON for machines, HTML for humans
- Header `X-Api-Key` or query `?key=`
## POST vs JSON GET
- GET `/api/profile` → JSON
- GET `/api/notes` → JSON
- POST `/api/notes` → form fields, JSON reply
- Both exist in Python, Node, and Go on this app
## BASH
- `curl` is enough to prove the contract
- Lesson 10 wraps the same calls in functions
___

#### [Lesson 10: Frontend State Intergration](https://github.com/JesseSteele/Codia/blob/master/701/Lesson-10.md)
