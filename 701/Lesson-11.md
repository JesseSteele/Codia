# Linux 701
## Lesson 11: Web App as System Service

Ready the CLI

```console
cd ~/School/Codia/701
```
___

*Until now you started processes in the terminal and <kbd>Ctrl</kbd> + <kbd>C</kbd>. A product is **systemd**: Nginx proxies the UI and the client; the API host stays on its own port*

### Unit files

| **1** :$

```console
cp core/11-701bio-python.service 701bio-python.service && \
cp core/11-701bio-node.service 701bio-node.service && \
cp core/11-701bio-go.service 701bio-go.service && \
cp core/11-701bio-api.service 701bio-api.service && \
cp core/11-701bio-client.service 701bio-client.service && \
code core/11-701bio-python.service core/11-701bio-api.service core/11-701bio-client.service
```

*Three units: web UI, API host, client. `WorkingDirectory=` is where `notes/` and `media/` live. `User=www`*

Operative Python web UI:

```
[Service]
Type=simple
User=www
WorkingDirectory=/var/lib/701bio
ExecStart=/usr/bin/python /usr/lib/701bio/bio.py
Restart=on-failure
```

### Install units (Python example)

*Do **not** run this blindly on a shared classroom box without the teacher*

*Stop the hand-started `python bio.py` / `python api.py` / `python client.py` first*

| **2** :$ *(as directed)*

```console
sudo mkdir -p /usr/lib/701bio /var/lib/701bio
sudo cp bio.py db.py db_process.py api.py client.py client.html /usr/lib/701bio/
sudo cp -a notes media /var/lib/701bio/ 2>/dev/null || true
sudo cp 701bio-python.service /etc/systemd/system/701bio.service
sudo cp 701bio-api.service /etc/systemd/system/701bio-api.service
sudo cp 701bio-client.service /etc/systemd/system/701bio-client.service
sudo systemctl daemon-reload
sudo systemctl enable --now 701bio 701bio-api 701bio-client
systemctl status 701bio --no-pager
```

| **B-3** ://

```console
localhost
```

| **B-3b** :// **client**

```console
localhost:8080
```

*Logs:*

```console
journalctl -u 701bio -f
```

*(<kbd>Ctrl</kbd> + <kbd>C</kbd> leaves the follow, not the service)*

### Node

```console
sudo cp bio.js db.js db-process.js api.js client.js client.html /usr/lib/701bio/
sudo cp 701bio-node.service /etc/systemd/system/701bio.service
sudo systemctl daemon-reload
sudo systemctl restart 701bio
```

### Go binary

```console
go build -o 701bio bio.go db-process.go
go build -o 701bio-api api.go db-process.go
go build -o 701bio-client client.go
sudo cp 701bio 701bio-api 701bio-client db.conf client.html /usr/lib/701bio/
sudo cp 701bio-go.service /etc/systemd/system/701bio.service
sudo systemctl daemon-reload
sudo systemctl restart 701bio
```

### Disable when class is over

```console
sudo systemctl disable --now 701bio 701bio-api 701bio-client
```

<!-- Basic service config files for each webapp written in each of the three server languages, using the custom frontend state as the default -->

___

# The Take
## systemd
- `enable --now` starts on boot
- Logs: `journalctl -u 701bio -f`
- Nginx still owns 80/443 and 8080; the API host owns 9002 itself
## Three ExecStart lines (web UI)
- Python: interpreter + `bio.py`
- Node: interpreter + `bio.js`
- Go: compiled binary
___

#### [Lesson 12: Linux Installer Packages](https://github.com/JesseSteele/Codia/blob/master/701/Lesson-12.md)
