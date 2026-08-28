# Linux 701
## Lesson 12: Linux Installer Packages

- [Package Architectures](https://github.com/JesseSteele/Codia/blob/master/Cheat-Sheets/Package-Architectures.md)

Ready the CLI

```console
cd ~/School/Codia/701
```
___

*We will not make a new GitHub repo per package. Everything lives under `701/701bio/` in [JesseSteele/701](https://github.com/JesseSteele/701)*

*Nine trees: three languages × bin / git / tar*

*The `-bin` tree builds the package named **`701bio`** (binaries are already in it). `-git` builds `701bio-git`. `-tar` builds `701bio-tar`. There is no package named `701bio-bin`*

*Python, Node, and Go all produce those same three package names. Install **one** language. They conflict*

*Source trees `701bio-git` and `701bio-tar` also exist in this folder; we only draw the binary-containing trees below*

### Look at the Python binary tree

| **1** :$

```console
ls 701bio
code 701bio/README.md 701bio/python-bin/README.md
```

| **`701bio/python-bin/`** :

```
python-bin/
├─ arch/
│  └─ PKGBUILD
├─ deb/
│  └─ build/
│     ├─ debian/
│     │  ├─ changelog
│     │  ├─ compat
│     │  ├─ control
│     │  ├─ copyright
│     │  ├─ install
│     │  └─ rules
│     ├─ bio.py
│     ├─ api.py
│     ├─ client.py
│     ├─ client.html
│     ├─ codia.html
│     ├─ db.py
│     ├─ db_process.py
│     ├─ 701bio.service
│     ├─ 701bio-api.service
│     └─ 701bio-client.service
├─ rpm/
│  └─ rpmbuild/
│     ├─ SPECS/
│     │  └─ 701bio.spec
│     └─ SOURCES/
└─ README.md
```

| **`701bio/node-bin/`** :

```
node-bin/
├─ arch/
│  └─ PKGBUILD
├─ deb/
│  └─ build/
│     ├─ debian/
│     │  ├─ changelog
│     │  ├─ compat
│     │  ├─ control
│     │  ├─ copyright
│     │  ├─ install
│     │  └─ rules
│     ├─ bio.js
│     ├─ api.js
│     ├─ client.js
│     ├─ client.html
│     ├─ codia.html
│     ├─ db.js
│     ├─ db-process.js
│     ├─ 701bio.service
│     ├─ 701bio-api.service
│     └─ 701bio-client.service
├─ rpm/
│  └─ rpmbuild/
│     ├─ SPECS/
│     │  └─ 701bio.spec
│     └─ SOURCES/
└─ README.md
```

| **`701bio/go-bin/`** :

```
go-bin/
├─ arch/
│  ├─ PKGBUILD
│  └─ 701bio
├─ deb/
│  └─ build/
│     ├─ debian/
│     │  ├─ changelog
│     │  ├─ compat
│     │  ├─ control
│     │  ├─ copyright
│     │  ├─ install
│     │  └─ rules
│     └─ 701bio
├─ rpm/
│  └─ rpmbuild/
│     ├─ SPECS/
│     │  └─ 701bio.spec
│     └─ SOURCES/
│        └─ 701bio
└─ README.md
```

*Directories first, then files — same display as the [gophersay](https://github.com/JesseSteele/gophersay) README trees. [gophersay](https://github.com/JesseSteele/gophersay) ships a separate `-bin` package because the source package does not already contain the binary. 701bio puts the binaries in `701bio` itself, so the `-bin` **tree** creates the core package name*

### What a 701bio package should drop on the system

```
/usr/lib/701bio/                    app files (web UI, API host, client)
/usr/lib/systemd/system/701bio.service
/usr/lib/systemd/system/701bio-api.service
/usr/lib/systemd/system/701bio-client.service
/etc/701bio/config                  live config (only file in that directory)
/usr/share/701bio/config.sample
/var/lib/701bio/                    notes/, media/, sqlite file
```

*Installer detects `/srv/www` vs `/var/www` and links `/etc/701bio/config` → `/srv/www/701bio/config` or `/var/www/701bio/config`*

### Build (Python binary tree)

| **2** :$ *(Arch, never as root for `makepkg`)*

```console
mkdir -p python
cd 701bio/python-bin/arch
makepkg -s
cp 701bio-1.0.0-1-any.pkg.tar.zst ../../../python/
```

```console
cd ../deb/build
dpkg-buildpackage -us -uc
cp ../701bio_1.0.0-1_all.deb ../../../python/
```

```console
cd ../../rpm
rpmbuild --define "_topdir $PWD/rpmbuild" -ba rpmbuild/SPECS/701bio.spec
cp rpmbuild/RPMS/noarch/701bio-1.0.0-1.noarch.rpm ../../../python/
```

### Node binary tree

```console
mkdir -p node
cd 701bio/node-bin/arch
makepkg -s
cp 701bio-1.0.0-1-any.pkg.tar.zst ../../../node/
```

```console
cd ../deb/build
dpkg-buildpackage -us -uc
cp ../701bio_1.0.0-1_all.deb ../../../node/
```

```console
cd ../../rpm
rpmbuild --define "_topdir $PWD/rpmbuild" -ba rpmbuild/SPECS/701bio.spec
cp rpmbuild/RPMS/noarch/701bio-1.0.0-1.noarch.rpm ../../../node/
```

### Go binary tree

```console
mkdir -p go
cd 701bio/go-bin/arch
makepkg -s
cp 701bio-1.0.0-1-x86_64.pkg.tar.zst ../../../go/
```

```console
cd ../deb/build
dpkg-buildpackage -us -uc
cp ../701bio_1.0.0-1_amd64.deb ../../../go/
```

```console
cd ../../rpm
rpmbuild --define "_topdir $PWD/rpmbuild" -ba rpmbuild/SPECS/701bio.spec
cp rpmbuild/RPMS/x86_64/701bio-1.0.0-1.x86_64.rpm ../../../go/
```

*Walk `python-git` and `python-tar` the same way. `source=` and `pkgver()` change; the installed name is `701bio-git` or `701bio-tar`*

*After all of that, `python/`, `node/`, and `go/` in the 701 working directory each hold an Arch package, a `.deb`, and an `.rpm`*

### Install (Arch, Python)

```console
sudo pacman -U python/701bio-1.0.0-1-any.pkg.tar.zst
sudo systemctl enable --now 701bio 701bio-api 701bio-client
```

<!-- Basic web app for the three server languages, each put into packages just as the JesseSteele/ gophersay, gophersay-bin, gophersay-git, and gophersay-tar repoes from Cheat-Sheets/Package-Architectures.md; repos will be called 701bio-node-git, 701bio-python-bin, 701bio-go-tar, etc, all four package repo types for each of the three server languages, making 12 pagkaces total, all placed inside the JesseSteele/701 repo in the 701bio/ folder; each package includes the necessary database option and package install, along with web installer, use our custom front-end state manager included and as default, with options in the /etc/ folder settings for the others (Vue, React, Angular) and instructions to either automatically fetch them or (if intellectual property or other law requires) how to obtain them manually; installer will detect whether to use /srv/www or /var/www and then install to the /srv/www/701bio/ folder with the config linked to /etc/701bio/config for the settings; package installs as normal, then displays amessage to run a CLI install script, which can be interactive, such as for database & setup password, or just use flags for non-interactive, then use the web installer with the setup password to create the profile; timezone can be set in the config and defaults to the server to overturn setting available in the database from the dashboard -->

___

# The Take
## Nine trees, three package names
- `701bio` / `701bio-git` / `701bio-tar`
- Pick one language to install
## Three apps inside
- Web UI, API host, client
- `codia.html` default frontend
## Config
- `/etc/701bio/config` only
- sample lives in `/usr/share/701bio/config.sample`
___

# Done! Have a cookie: ### #
