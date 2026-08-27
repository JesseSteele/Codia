# Linux 701
## Lesson 7: Files & Uploads

Ready the CLI

```console
cd ~/School/Codia/701
```

Ready services

```console
sudo systemctl start nginx
```
___

*SQL is for accounts and keys. **Notes are files.** That is a product choice, not an accident*

```
notes/           markdown, with a tiny YAML header
media/pictures/
media/audio/
media/video/
```

*You can `vim notes/hello.md` on the server. The web app will see it. There is **no** version history — the file is the note*

*501cms kept `publication_history` in SQL. 701bio will not. Saving overwrites the file*

### Save a note as a file

#### Python

| **1** :$

```console
cp core/07-files.py files.py && \
code core/07-files.py
```

*Note the header we write:*

```
---
title: Hello
public: true
---
markdown body
```

Operative save:

```py
            text = f"---\ntitle: {title}\npublic: {public}\n---\n{body}\n"
            (NOTES / f"{slug}.md").write_text(text)
```

| **2** :$

```console
python files.py
```

| **B-2** ://

```console
localhost
```

*Save a note titled `Hello`, check Public, write a sentence*

*Then in another terminal:*

```console
ls notes
cat notes/*.md
```

*Play: edit the file with nano, refresh the browser list*

```console
nano notes/hello.md
```

*Play: upload a small picture. Confirm it landed in `media/pictures/`*

```console
ls media/pictures
```

*(When finished: <kbd>Ctrl</kbd> + <kbd>C</kbd> in the terminal to exit)*

#### Node.js

| **3** :$

```console
cp core/07-files.js files.js && \
code core/07-files.js
```

*Same folders. Same YAML header. Multipart upload writes the file bytes into `media/...`*

| **4** :$

```console
node files.js
```

| **B-4** ://

```console
localhost
```

*(When finished: <kbd>Ctrl</kbd> + <kbd>C</kbd> in the terminal to exit)*

#### Go

| **5** :$

```console
cp core/07-files.go files.go && \
code core/07-files.go
```

*Go uses `r.ParseMultipartForm` for the file. Notes still use `url.ParseQuery`*

| **6** :$

```console
go run files.go
```

| **B-6** ://

```console
localhost
```

*(When finished: <kbd>Ctrl</kbd> + <kbd>C</kbd> in the terminal to exit)*

### Public flag
*In the note header: `public: true`*

*701bio profile tabs (next lesson) will only show public items*

*There is no front-facing media index. The profile has four tabs: notes / pictures / audio / video. That is the whole catalog visitors see*

### Why not version the files
*vim on the server is the editor of last resort. A second SQL history would fight that*

*Browser autosave (Lesson 8) is `localStorage` — the same *feel* as 501cms — and it is **not** a second copy on disk*

<!-- Use a 'media' folder -->
<!-- validate, thumbnail, and process using the same BASH scripts and UI/UX workflow we had from media uploads in the 501 unit; this may need Linux install commands for the media processing BASH script package dependencies, with appropriate commands for package managers we are using according to Lesson 6 (pacman, apt, dnf, zypper) -->
<!-- Create our own, in-house drag-and-drop JS uploader tool, custom for this, with API and SDK allowing it to be used in other projects; media viewer and players will all use the browser as much as possible to remain simple, good UI theory, and lightweight; this uploader JS library should be easy to understand and use a .min. version for our deployment; start with the simple file picker, then just integrate this, using comments inside the JS library file as the pedagogy -->

___

# The Take
## Files vs SQL
- Accounts, sessions, API keys, media *index* → SQL
- Note bodies → `notes/*.md` on disk
- Uploads → `media/{pictures,audio,video}/`
## Public flag
- In the note header: `public: true`
- 701bio profile tabs will only show public items
## No versioning
- Unlike 501cms history tables, saving a note **overwrites** the file
- Autosave (next lesson) is in the **browser**, not a second copy on disk
## Uploads
- Multipart POST, not a JSON body
- Keep the original filename, under the kind folder
___

#### [Lesson 8: Web App & Installer](https://github.com/JesseSteele/Codia/blob/master/701/Lesson-08.md)
