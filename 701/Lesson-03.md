# Linux 701
## Lesson 3: Database Connections

Ready the CLI

```console
cd ~/School/Codia/701
```

Ready services

```console
sudo systemctl start nginx mariadb postgresql
```

___

### Databases
*This lesson will implement four databases into the backend web app we wrote in the previous lesson*

- SQLite
- MySQL/MariaDB
- PostgreSQL

### Basic Backend Structure for `INSERT` and `UPDATE`
*We will use the same three backend files for each of our databases; these files will remain unchanged*

*The database configs and access will be included in separate files*

*Note the filenames will be the same in these scripts, regardless of which database we use:*

- `db.*` for the database config
- `process-db.*` for the implementation that processes database calls

*Note that we have new "Create" & "Update" `type="submit"` buttons*

*These are the `functions.*` and `backend-app.*` files that will use the database includes...*

#### Python

| **1** :$

```console
cp core/03-functions.py functions.py && \
cp core/03-backend-app.py backend-app.py && \
code core/03-functions.py core/03-backend-app.py
```


#### Node.js

| **2** :$

```console
cp core/03-functions.js functions.js && \
cp core/03-backend-app.js backend-app.js && \
code core/03-functions.js core/03-backend-app.js
```

*Note we use `.js` as the extension for the `functions.*` file so we don't need to specify the file extension in the main Node script*


#### Go

| **3** :$

```console
cp core/03-functions.go functions.go && \
cp core/03-backend-app.go backend-app.go && \
code core/03-functions.go core/03-backend-app.go
```


### Basic Database Implementations for `INSERT` and `UPDATE`
#### SQLite

*Access the SQLite terminal*

| **4** :$

```console
sudo -u www sqlite3 /srv/www/backendapp.db
```

*Create the database and table we need*

```
Database: backendapp.db
```

*Note the database is created when opened, no user or password with SQLite*

| **5** :>

```sql
CREATE TABLE users (
    fullname TEXT NOT NULL,
    username TEXT PRIMARY KEY,
    email TEXT NOT NULL,
    webpage TEXT,
    number INTEGER CHECK (number >= 0 AND number <= 100),
    password TEXT NOT NULL
);
.quit
```

*Note our database configs (for each backend language)*

| **6** :$

```console
cp core/03-sqlite-db1.py sqlite-db.py && \
cp core/03-sqlite-db1.js sqlite-db.js && \
cp core/03-sqlite-db.go sqlite-db.go && \
code core/03-sqlite-db1.py core/03-sqlite-db1.js core/03-sqlite-db.go
```


*Note our database process implementations (for each backend language)*

| **7** :$

```console
cp core/03-sqlite-process.py sqlite-process.py && \
cp core/03-sqlite-process.js sqlite-process.js && \
cp core/03-sqlite-process.go sqlite-process.go && \
code core/03-sqlite-process.py core/03-sqlite-process.js core/03-sqlite-process.go
```


*Now, see it in action...*

##### Python

| **8** :$

```console
cp sqlite-db.py db.py
cp sqlite-process.py db_process.py
python backend-app.py
```

| **B-8** ://

```console
localhost
```

*(When finished: <kbd>Ctrl</kbd> + <kbd>C</kbd> in the terminal to exit)*

##### Node.js

| **9** :$

```console
cp sqlite-db.js db.js
cp sqlite-process.js db-process.js
node backend-app.js
```

| **B-9** ://

```console
localhost
```

*(When finished: <kbd>Ctrl</kbd> + <kbd>C</kbd> in the terminal to exit)*

| **10** :$

```console
cp sqlite-db.go db.go
cp sqlite-process.go db-process.go
go run backend-app.go
```

| **B-10** ://

```console
localhost
```

*(When finished: <kbd>Ctrl</kbd> + <kbd>C</kbd> in the terminal to exit)*

#### MySQL/MariaDB

*Access the MariaDB terminal*

| **11** :$

```console
sudo systemctl start mariadb
mariadb -u admin -padminpassword
```

*Create the database credentials and table we need*

```
Database: backendapp_db
DB User:  beadbuser
Password: beadbpass
```

| **12** :>

```sql
CREATE DATABASE backendapp_db;
CREATE USER 'beadbuser'@'localhost' IDENTIFIED BY 'beadbpass';
GRANT ALL PRIVILEGES ON backendapp_db.* TO 'beadbuser'@'localhost';
FLUSH PRIVILEGES;
USE backendapp_db;

CREATE TABLE users (
    fullname VARCHAR(32) NOT NULL,
    username VARCHAR(32) PRIMARY KEY,
    email VARCHAR(255) NOT NULL,
    webpage VARCHAR(255),
    number INT CHECK (number >= 0 AND number <= 100),
    password VARCHAR(32) NOT NULL
);
quit;
```

*Note our database configs (for each backend language)*

| **13** :$

```console
cp core/03-mysql-db1.py mysql-db.py && \
cp core/03-mysql-db1.js mysql-db.js && \
cp core/03-mysql-db.go mysql-db.go && \
code core/03-mysql-db1.py core/03-mysql-db1.js core/03-mysql-db.go
```


*Note our database process implementations (for each backend language)*

| **14** :$

```console
cp core/03-mysql-process.py mysql-process.py && \
cp core/03-mysql-process.js mysql-process.js && \
cp core/03-mysql-process.go mysql-process.go && \
code core/03-mysql-process.py core/03-mysql-process.js core/03-mysql-process.go
```


*Now, see it in action...*

##### Python

| **15** :$

```console
cp mysql-db.py db.py
cp mysql-process.py db_process.py
python backend-app.py
```

| **B-15** ://

```console
localhost
```

*(When finished: <kbd>Ctrl</kbd> + <kbd>C</kbd> in the terminal to exit)*

##### Node.js

| **16** :$

```console
cp mysql-db.js db.js
cp mysql-process.js db-process.js
node backend-app.js
```

| **B-16** ://

```console
localhost
```

*(When finished: <kbd>Ctrl</kbd> + <kbd>C</kbd> in the terminal to exit)*

| **17** :$

```console
cp mysql-db.go db.go
cp mysql-process.go db-process.go
go run backend-app.go
```

| **B-17** ://

```console
localhost
```

*(When finished: <kbd>Ctrl</kbd> + <kbd>C</kbd> in the terminal to exit)*

#### PostgreSQL

*Access the PostgreSQL terminal*

| **18** :$

```console
sudo systemctl start postgresql
PGPASSWORD=adminpassword psql -U admin -d postgres -h localhost -w
```

*Create the database credentials and table we need*

```
Database: backendapp_db
DB User:  beadbuser
Password: beadbpass
```

| **19** :>

```sql
CREATE DATABASE backendapp_db;
CREATE ROLE beadbuser WITH LOGIN PASSWORD 'beadbpass';
GRANT ALL PRIVILEGES ON DATABASE backendapp_db TO beadbuser;
\c backendapp_db

CREATE TABLE users (
    fullname VARCHAR(32) NOT NULL,
    username VARCHAR(32) PRIMARY KEY,
    email VARCHAR(255) NOT NULL,
    webpage VARCHAR(255),
    number INT CHECK (number >= 0 AND number <= 100),
    password VARCHAR(32) NOT NULL
);
\q
```

*Note our database configs (for each backend language)*

| **20** :$

```console
cp core/03-postgres-db1.py postgres-db.py && \
cp core/03-postgres-db1.js postgres-db.js && \
cp core/03-postgres-db.go postgres-db.go && \
code core/03-postgres-db1.py core/03-postgres-db1.js core/03-postgres-db.go
```


*Note our database process implementations (for each backend language)*

| **21** :$

```console
cp core/03-postgres-process.py postgres-process.py && \
cp core/03-postgres-process.js postgres-process.js && \
cp core/03-postgres-process.go postgres-process.go && \
code core/03-postgres-process.py core/03-postgres-process.js core/03-postgres-process.go
```


*Now, see it in action...*

##### Python

| **22** :$

```console
cp postgres-db.py db.py
cp postgres-process.py db_process.py
python backend-app.py
```

| **B-22** ://

```console
localhost
```

*(When finished: <kbd>Ctrl</kbd> + <kbd>C</kbd> in the terminal to exit)*

##### Node.js

| **23** :$

```console
cp postgres-db.js db.js
cp postgres-process.js db-process.js
node backend-app.js
```

| **B-23** ://

```console
localhost
```

*(When finished: <kbd>Ctrl</kbd> + <kbd>C</kbd> in the terminal to exit)*

##### Go

| **24** :$

```console
cp postgres-db.go db.go
cp postgres-process.go db-process.go
go run backend-app.go
```

| **B-24** ://

```console
localhost
```

*(When finished: <kbd>Ctrl</kbd> + <kbd>C</kbd> in the terminal to exit)*

### Expanded Backend Structure for `SELECT`
*We will re-write this app to select, edit, and update users in the database*

*Note we will keep `backend-app.*` files unchanged for your reference; now we will use `backend-users-app.*`, then copy and deploy these as `backend.*`*

*Still, we have no login or access restrictions*

*These are only examples of using `INSERT`, `UPDATE`, and now `SELECT`*

*Note our database configs haven't changed because the database and login credentials are the same*

*Same as before, the filenames will be the same in these scripts, regardless of which database we use:*

- `db.*` for the database config
- `process-db.*` for the implementation that processes database calls

*Note that we have new HTML tables with links to edit each user*

#### CSS Additions

| **25** :$

```console
cp core/03-style.css style.css && \
code core/03-style.css
```


#### Python

| **26** :$

```console
cp core/03-backend-users-app.py backend-users-app.py && \
code core/03-backend-users-app.py
```


#### Node.js

| **27** :$

```console
cp core/03-backend-users-app.js backend-users-app.js && \
code core/03-backend-users-app.js
```


#### Go

| **28** :$

```console
cp core/03-backend-users-app.go backend-users-app.go && \
code core/03-backend-users-app.go
```


### Expanded Database Implementations for `SELECT`
#### SQLite

*Access the SQLite terminal*

| **29** :$

```console
sudo -u www sqlite3 /srv/www/backendapp.db
```

*Update the database table for new fields*

```
user_type
date_created
date_updated
```

*Note the database is created when opened, no user or password with SQLite*

| **30** :>

```sql
ALTER TABLE users ADD COLUMN user_type TEXT CHECK (user_type IN ('member', 'admin')) DEFAULT 'member';
ALTER TABLE users ADD COLUMN date_created DATETIME DEFAULT CURRENT_TIMESTAMP;
ALTER TABLE users ADD COLUMN date_updated DATETIME DEFAULT CURRENT_TIMESTAMP;
.quit
```

| **31** :$

```console
cp core/03-sqlite-full-process.py sqlite-full-process.py && \
cp core/03-sqlite-full-process.js sqlite-full-process.js && \
cp core/03-sqlite-full-process.go sqlite-full-process.go && \
code core/03-sqlite-full-process.py core/03-sqlite-full-process.js core/03-sqlite-full-process.go
```


*Now, see it in action...*

##### Python

| **32** :$

```console
cp sqlite-db.py db.py
cp sqlite-full-process.py db_process.py
cp backend-users-app.py backend.py
python backend.py
```

| **B-32** ://

```console
localhost
```

*(When finished: <kbd>Ctrl</kbd> + <kbd>C</kbd> in the terminal to exit)*

##### Node.js

| **33** :$

```console
cp sqlite-db.js db.js
cp sqlite-full-process.js db-process.js
cp backend-users-app.js backend.js
node backend.js
```

| **B-33** ://

```console
localhost
```

*(When finished: <kbd>Ctrl</kbd> + <kbd>C</kbd> in the terminal to exit)*

##### Go

| **34** :$

```console
cp sqlite-db.go db.go
cp sqlite-full-process.go db-process.go
cp backend-users-app.go backend.go
go run backend.go
```

| **B-34** ://

```console
localhost
```

*(When finished: <kbd>Ctrl</kbd> + <kbd>C</kbd> in the terminal to exit)*

#### MySQL/MariaDB

*Access the MariaDB terminal*

| **35** :$

```console
sudo systemctl start mariadb
mariadb -u admin -padminpassword
```

*Update the database table for new fields*

```
user_type
date_created
date_updated
```

| **36** :>

```sql
ALTER TABLE users ADD COLUMN user_type ENUM('member', 'admin') DEFAULT 'member';
ALTER TABLE users ADD COLUMN date_created DATETIME DEFAULT CURRENT_TIMESTAMP;
ALTER TABLE users ADD COLUMN date_updated DATETIME DEFAULT CURRENT_TIMESTAMP;
quit;
```

*Note our database process implementations (for each backend language)*

| **37** :$

```console
cp core/03-mysql-full-process.py mysql-full-process.py && \
cp core/03-mysql-full-process.js mysql-full-process.js && \
cp core/03-mysql-full-process.go mysql-full-process.go && \
code core/03-mysql-full-process.py core/03-mysql-full-process.js core/03-mysql-full-process.go
```


*Now, see it in action...*

##### Python

| **38** :$

```console
cp mysql-db.py db.py
cp mysql-full-process.py db_process.py
python backend.py
```

| **B-38** ://

```console
localhost
```

*(When finished: <kbd>Ctrl</kbd> + <kbd>C</kbd> in the terminal to exit)*

##### Node.js

| **39** :$

```console
cp mysql-db.js db.js
cp mysql-full-process.js db-process.js
node backend.js
```

| **B-39** ://

```console
localhost
```

*(When finished: <kbd>Ctrl</kbd> + <kbd>C</kbd> in the terminal to exit)*

##### Go

| **40** :$

```console
cp mysql-db.go db.go
cp mysql-full-process.go db-process.go
go run backend.go
```

| **B-40** ://

```console
localhost
```

*(When finished: <kbd>Ctrl</kbd> + <kbd>C</kbd> in the terminal to exit)*

#### PostgreSQL

*Access the PostgreSQL terminal*

| **41** :$

```console
sudo systemctl start postgresql
PGPASSWORD=adminpassword psql -U admin -d postgres -h localhost -w
```

*Update the database table for new fields*

```
user_type
date_created
date_updated
```

| **42** :>

```sql
ALTER TABLE users ADD COLUMN user_type VARCHAR(6) CHECK (user_type IN ('member', 'admin')) DEFAULT 'member';
ALTER TABLE users ADD COLUMN date_created TIMESTAMP DEFAULT CURRENT_TIMESTAMP;
ALTER TABLE users ADD COLUMN date_updated TIMESTAMP DEFAULT CURRENT_TIMESTAMP;
\q
```

*Note our database process implementations (for each backend language)*

| **43** :$

```console
cp core/03-postgres-full-process.py postgres-full-process.py && \
cp core/03-postgres-full-process.js postgres-full-process.js && \
cp core/03-postgres-full-process.go postgres-full-process.go && \
code core/03-postgres-full-process.py core/03-postgres-full-process.js core/03-postgres-full-process.go
```


*Now, see it in action...*

##### Python

| **44** :$

```console
cp postgres-db.py db.py
cp postgres-full-process.py db_process.py
python backend.py
```

| **B-44** ://

```console
localhost
```

*(When finished: <kbd>Ctrl</kbd> + <kbd>C</kbd> in the terminal to exit)*

##### Node.js

| **45** :$

```console
cp postgres-db.js db.js
cp postgres-full-process.js db-process.js
node backend.js
```

| **B-45** ://

```console
localhost
```

*(When finished: <kbd>Ctrl</kbd> + <kbd>C</kbd> in the terminal to exit)*

##### Go

| **46** :$

```console
cp postgres-db.go db.go
cp postgres-full-process.go db-process.go
go run backend.go
```

| **B-46** ://

```console
localhost
```

*(When finished: <kbd>Ctrl</kbd> + <kbd>C</kbd> in the terminal to exit)*

### Database Engine as Setting
- *The database engine (SQLite, MySQL/MariaDB, or PostgreSQL) can be a setting in a config file*
- *A good place for this is inside the `db.*` file itself*
- *So, `sqlite`, `mysql`, or `postgresql` would be a setting alongside `database=`, `user=`, etc*
- *No other files would need to be changed*

#### Simplified DB Configs
*These are prepped to be copied via CLI `cp` later to `db.*` per server language...*

| **47** :$

```console
cp core/03-sqlite-db2.py sqlite-db.py && \
cp core/03-mysql-db2.py mysql-db.py && \
cp core/03-postgres-db2.py postgres-db.py && \
cp core/03-sqlite-db2.js sqlite-db.js && \
cp core/03-mysql-db2.js mysql-db.js && \
cp core/03-postgres-db2.js postgres-db.js && \
cp core/03-sqlite-db.conf sqlite-db.conf && \
cp core/03-mysql-db.conf mysql-db.conf && \
cp core/03-postgres-db.conf postgres-db.conf && \
code core/03-sqlite-db2.py core/03-mysql-db2.py core/03-postgres-db2.py core/03-sqlite-db2.js core/03-mysql-db2.js core/03-postgres-db2.js core/03-sqlite-db.conf core/03-mysql-db.conf core/03-postgres-db.conf
```








#### Master DB Processors
*These will contain the execution for each SQL engine, but select them based on the `db_type` setting in the `db.*` config...*

| **48** :$

```console
cp core/03-db_process.py db_process.py && \
cp core/03-db-process.js db-process.js && \
cp core/03-db-process.go db-process.go && \
code core/03-db_process.py core/03-db-process.js core/03-db-process.go
```


#### Implementation
*Now, see it in action...*

##### Python
###### SQLite

| **49** :$

```console
cp sqlite-db.py db.py
python backend.py
```

| **B-49** ://

```console
localhost
```

*(When finished: <kbd>Ctrl</kbd> + <kbd>C</kbd> in the terminal to exit)*

###### MySQL/MariaDB

| **50** :$

```console
cp mysql-db.py db.py
python backend.py
```

| **B-50** ://

```console
localhost
```

*(When finished: <kbd>Ctrl</kbd> + <kbd>C</kbd> in the terminal to exit)*

###### PostgreSQL

| **51** :$

```console
cp postgres-db.py db.py
python backend.py
```

| **B-51** ://

```console
localhost
```

*(When finished: <kbd>Ctrl</kbd> + <kbd>C</kbd> in the terminal to exit)*

##### Node.js
###### SQLite

| **52** :$

```console
cp sqlite-db.js db.js
node backend.js
```

| **B-52** ://

```console
localhost
```

*(When finished: <kbd>Ctrl</kbd> + <kbd>C</kbd> in the terminal to exit)*

###### MySQL/MariaDB

| **53** :$

```console
cp mysql-db.js db.js
node backend.js
```

| **B-53** ://

```console
localhost
```

*(When finished: <kbd>Ctrl</kbd> + <kbd>C</kbd> in the terminal to exit)*

###### PostgreSQL

| **54** :$

```console
cp postgres-db.js db.js
node backend.js
```

| **B-54** ://

```console
localhost
```

*(When finished: <kbd>Ctrl</kbd> + <kbd>C</kbd> in the terminal to exit)*

##### Go
###### SQLite

| **55** :$

```console
cp sqlite-db.conf db.conf
go run backend.go
```

| **B-55** ://

```console
localhost
```

*(When finished: <kbd>Ctrl</kbd> + <kbd>C</kbd> in the terminal to exit)*

###### MySQL/MariaDB

| **56** :$

```console
cp mysql-db.conf db.conf
go run backend.go
```

| **B-56** ://

```console
localhost
```

*(When finished: <kbd>Ctrl</kbd> + <kbd>C</kbd> in the terminal to exit)*

###### PostgreSQL

| **57** :$

```console
cp postgres-db.conf db.conf
go run backend.go
```

| **B-57** ://

```console
localhost
```

*(When finished: <kbd>Ctrl</kbd> + <kbd>C</kbd> in the terminal to exit)*

___

# The Take
- Python, Node.js, and Go are all capable of working with various SQL engines
- The interaction per SQL engine can be in a separate file, function, or object from the rest of the program
  - The Python, Node.js, or Go program can remain the same, regardless of which SQL engine is used
  - Which SQL engine to use can be any mere option in a config file
  - We might say such an app is "database engine agnostic" beyond the database config
- Since Go compiles at or before run time, database config files should use plain text (ie `.conf`), not `.go` extensions
  - This keeps them editable by the SysAdmin
  - Keep them as simple as possible
  - Plan for your code to use this structure
___

#### [Lesson 4: URL Rewrite & Parsing](https://github.com/JesseSteele/Codia/blob/master/701/Lesson-04.md)