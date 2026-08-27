# Linux 701
## Lesson 12: Linux Installer Packages

Ready the CLI

```console
cd ~/School/Codia/701
```
___

*We will not make a new GitHub repo per package. Everything lives under `701/701bio/` in [JesseSteele/701](https://github.com/JesseSteele/701)*

*Three languages × four package styles = **twelve** trees*

| Lang | Source | VCS | Tarball | Binary |
| --- | --- | --- | --- | --- |
| Python | `701bio/python` | `python-git` | `python-tar` | `python-bin` |
| Node | `701bio/node` | `node-git` | `node-tar` | `node-bin` |
| Go | `701bio/go` | `go-git` | `go-tar` | `go-bin` |

*Each tree has Arch `PKGBUILD`, Debian `debian/`, and RPM `.spec` — same contrast as [Package Architectures](https://github.com/JesseSteele/Codia/blob/master/Cheat-Sheets/Package-Architectures.md) and the [gophersay](https://github.com/JesseSteele/gophersay) family*

### Look at one

| **1** :$

```console
ls 701bio
ls 701bio/python
code 701bio/python/PKGBUILD 701bio/python/debian/control 701bio/python/701bio-python.spec
```

*Walk `python-git`, `python-tar`, `python-bin` next. Same app, different `source=`*

### What a 701bio package should drop on the system

```
/usr/lib/701bio/                    app files (bio.*, db.*)
/usr/lib/systemd/system/701bio.service
/var/lib/701bio/                    notes/, media/, sqlite file (data)
```

*`backup=()` / conffiles so an upgrade does not smash notes*

### git vs tar vs bin
- **source** (`python`, `node`, `go`): files in the tree, `makepkg` / `dpkg-buildpackage` / `rpmbuild` from here
- **git** (`*-git`): `source=("...::git+https://github.com/JesseSteele/701.git")` and Arch `pkgver()` from the repo
- **tar** (`*-tar`): a release tarball of the same files
- **bin** (`*-bin`): prebuilt. Python and Node still ship the interpreter scripts; Go bin ships the `701bio` binary (`go build`)

*This is the gophersay pattern: `gophersay`, `gophersay-git`, `gophersay-tar`, `gophersay-bin`*

### Arch example (Python source)

| **2** :$ *(Arch, never as root for `makepkg`)*

```console
cd 701bio/python
makepkg -s
```

*Then:*

```console
sudo pacman -U 701bio-python-*.pkg.tar.zst
```

*After install, enable the unit (Arch packages usually do not `systemctl enable` for you):*

```console
sudo systemctl enable --now 701bio
```

### Debian

*Maintainer layout is already in `debian/` inside each tree*

```console
cd 701bio/python
dpkg-buildpackage -us -uc
sudo dpkg -i ../701bio-python_*.deb
```

### RPM

```console
mkdir -p ~/rpmbuild/{SPECS,SOURCES}
cp 701bio/python/701bio-python.spec ~/rpmbuild/SPECS/
cp -a 701bio/python/app ~/rpmbuild/SOURCES/701bio-python
rpmbuild -ba ~/rpmbuild/SPECS/701bio-python.spec
```

### Node and Go trees
*Same three package formats. Node `depends` on `nodejs`. Go source/git/tar compile with `go build`; `go-bin` installs the binary and skips the compiler at install time*

| **3** :$

```console
ls 701bio/go 701bio/go-git 701bio/go-tar 701bio/go-bin
code 701bio/go-git/PKGBUILD 701bio/go-bin/PKGBUILD
```

*Compare `source=` and `pkgver()` on the git tree against the bin tree*

### After a package install
*Nginx still reverse-proxies `9001`. The unit from Lesson 11 is now a file the package owns*

| **B-4** :// *(if the unit is running)*

```console
localhost
```

<!-- Basic web app for the three server languages, each put into packages just as the JesseSteele/ gophersay, gophersay-bin, gophersay-git, and gophersay-tar repoes from Cheat-Sheets/Package-Architectures.md; repos will be called 701bio-node-git, 701bio-python-bin, 701bio-go-tar, etc, all four package repo types for each of the three server languages, making 12 pagkaces total, all placed inside the JesseSteele/701 repo in the 701bio/ folder; each package includes the necessary database option and package install, along with web installer, use our custom front-end state manager included and as default, with options in the /etc/ folder settings for the others (Vue, React, Angular) and instructions to either automatically fetch them or (if intellectual property or other law requires) how to obtain them manually; installer will detect whether to use /srv/www or /var/www and then install to the /srv/www/701bio/ folder with the config linked to /etc/701bio/config for the settings; package installs as normal, then displays amessage to run a CLI install script, which can be interactive, such as for database & setup password, or just use flags for non-interactive, then use the web installer with the setup password to create the profile; timezone can be set in the config and defaults to the server to overturn setting available in the database from the dashboard -->

___

# The Take
## Packages
- Arch, Debian, RPM all wrap the **same** 701bio
- Twelve folders, one product
- systemd unit from Lesson 11 goes in the package
- Data lives under `/var/lib/701bio` so upgrades do not smash notes
## git / tar / bin
- git: clone upstream, `pkgver()` from the repo
- tar: a snapshot tarball
- bin: skip compile at install (Go binary; Python/Node scripts)
## 701bio
- Lightweight bio site
- Public tabs, file notes, API keys
- Teachable: you watched every layer from port `9001` to `pacman -U`

___

# Done! Have a cookie: ### #
