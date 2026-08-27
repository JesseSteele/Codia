# Linux 701
## Lesson 11: Web App as System Service

Ready the CLI

```console
cd ~/School/Codia/701
```
___

*Until now you started 701bio with `python bio.py` and <kbd>Ctrl</kbd> + <kbd>C</kbd>. A product is a **systemd** unit: Nginx proxies, the app stays up*

*We did not rewrite 701bio to become a service. We wrapped it*

### Unit files

| **1** :$

```console
cp core/11-701bio-python.service 701bio-python.service && \
cp core/11-701bio-node.service 701bio-node.service && \
cp core/11-701bio-go.service 701bio-go.service && \
code core/11-701bio-python.service core/11-701bio-node.service core/11-701bio-go.service
```

*Note `WorkingDirectory=` — notes/ and media/ are relative to that directory*

*Note `User=www` — same user the reverse proxy talks toward*

*Note `ExecStart=` — Python interpreter, Node interpreter, or a Go binary*

Operative Python unit:

```
[Service]
Type=simple
User=www
WorkingDirectory=/srv/701bio
ExecStart=/usr/bin/python /srv/701bio/bio.py
Restart=on-failure
```

### Install a unit (Python example)

*Do **not** run this blindly on a shared classroom box without the teacher. Paths assume `/srv/701bio`*

*Stop the hand-started `python bio.py` first or port `9001` will clash*

| **2** :$ *(as directed)*

```console
sudo mkdir -p /srv/701bio
sudo cp bio.py db.py db_process.py /srv/701bio/
sudo cp -a notes media /srv/701bio/ 2>/dev/null || true
sudo cp 701bio-python.service /etc/systemd/system/701bio.service
sudo systemctl daemon-reload
sudo systemctl enable --now 701bio
systemctl status 701bio --no-pager
```

| **B-3** ://

```console
localhost
```

*Logs:*

```console
journalctl -u 701bio -f
```

*(<kbd>Ctrl</kbd> + <kbd>C</kbd> leaves the follow, not the service)*

### Node
*`ExecStart=/usr/bin/node /srv/701bio/bio.js`*

```console
sudo cp bio.js db.js db-process.js /srv/701bio/
sudo cp 701bio-node.service /etc/systemd/system/701bio.service
sudo systemctl daemon-reload
sudo systemctl restart 701bio
```

### Go binary
*Compile once, then the unit runs the binary:*

```console
go build -o 701bio bio.go db-process.go
sudo cp 701bio db.conf /srv/701bio/
sudo cp 701bio-go.service /etc/systemd/system/701bio.service
sudo systemctl daemon-reload
sudo systemctl restart 701bio
```

*`ExecStart=/srv/701bio/701bio` in the Go unit*

### Disable when class is over

```console
sudo systemctl disable --now 701bio
```

*Nginx still owns 80/443. The app still owns 9001. systemd just keeps the app from dying when you close the terminal*

<!-- Basic service config files for each webapp written in each of the three server languages, using the custom frontend state as the default -->

___

# The Take
## systemd
- `enable --now` starts on boot
- Logs: `journalctl -u 701bio -f`
- Nginx still owns 80/443; the app still owns 9001
- `WorkingDirectory` is where `notes/` and `media/` live
## Same app
- We did not rewrite 701bio to become a service
- We wrapped it
## Three ExecStart lines
- Python: interpreter + `bio.py`
- Node: interpreter + `bio.js`
- Go: compiled binary
___

#### [Lesson 12: Linux Installer Packages](https://github.com/JesseSteele/Codia/blob/master/701/Lesson-12.md)
