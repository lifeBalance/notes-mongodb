# Install MongoDB on OS X using Homebrew
There are a couple of ways of installing [MongoDB][1]:

1. Using the the popular OS X package manager [Homebrew][2].
2. Downloading it from the [MongoDB Download site][3] and install it **manually**.

In this section we are gonna cover the first way, which is the officially recommended way of installing **MongoDB** on **OS X**. So if you are on a Mac and use **Homebrew** as your package manager, installing **MongoDB** is quite easy.

## Update Homebrew
First of all, we have to update Homebrew’s package database:

```bash
$ brew update
```

## Three flavours of MongoDB
Then we can install [MongoDB][1] itself. We have several options here:

1. Install just the [MongoDB][1] binaries:

  ```bash
  $ brew install mongodb
  ```

2. Build [MongoDB][1] from Source with TLS/SSL Support

  ```bash
  $ brew install mongodb --with-openssl
  ```

3. Install the Latest Development Release of [MongoDB][1]:

  ```bash
  $ brew install mongodb --devel
  ```
## Installing the MongoDB binaries
In this case we are gonna install just the MongoDB binaries so let's run:
```
$ brew install mongodb
==> Downloading https://homebrew.bintray.com/bottles/mongodb-3.2.3.el_capitan.bottle.tar.gz
######################################################################## 100.0%
==> Pouring mongodb-3.2.3.el_capitan.bottle.tar.gz
==> Caveats
To have launchd start mongodb at login:
  ln -sfv /usr/local/opt/mongodb/*.plist ~/Library/LaunchAgents
Then to load mongodb now:
  launchctl load ~/Library/LaunchAgents/homebrew.mxcl.mongodb.plist
Or, if you don't want/need launchctl, you can just run:
  mongod --config /usr/local/etc/mongod.conf
==> Summary
🍺  /usr/local/Cellar/mongodb/3.2.3: 17 files, 208.7M
```

Installing MongoDB using [Homebrew][2] provides a installation with sane defaults, such as:

* Configuration file: `/usr/local/etc/mongod.conf`
* A data directory: `/usr/local/var/mongodb`
* A place for logs: `/usr/local/var/log/mongodb/mongo.log`
* A `plist` file: `/usr/local/opt/mongodb/homebrew.mxcl.mongodb.plist`

### Caveats
The output of the command above is quite helpful, especially the **Caveats**. According to them, we can start the `mongod` process in two different manners:

1. Automatically every time we login into the system.
2. Manually, running the `mongod` command.

#### Launching MongoDB automatically at login
The Homebrew distribution of MongoDB includes a `plist` file so the `launchd` daemon uses to launch the `mongod` process at login.

> For a more detailed explanation of `launchd` read what I've written [here][4].

The `homebrew.mxcl.mongodb.plist` is an **XML** file which specifies the settings according to which the `mongod` process will be launched. These are its contents:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
  <key>Label</key>
  <string>homebrew.mxcl.mongodb</string>
  <key>ProgramArguments</key>
  <array>
    <string>/usr/local/opt/mongodb/bin/mongod</string>
    <string>--config</string>
    <string>/usr/local/etc/mongod.conf</string>
  </array>
  <key>RunAtLoad</key>
  <true/>
  <key>KeepAlive</key>
  <false/>
  <key>WorkingDirectory</key>
  <string>/usr/local</string>
  <key>StandardErrorPath</key>
  <string>/usr/local/var/log/mongodb/output.log</string>
  <key>StandardOutPath</key>
  <string>/usr/local/var/log/mongodb/output.log</string>
  <key>HardResourceLimits</key>
  <dict>
    <key>NumberOfFiles</key>
    <integer>4096</integer>
  </dict>
  <key>SoftResourceLimits</key>
  <dict>
    <key>NumberOfFiles</key>
    <integer>4096</integer>
  </dict>
</dict>
</plist>
```

**OS X** keeps these files in several locations on the filesystem. In this case, the file should be placed in the `~/Library/LaunchAgents` directory, or as **Homebrew** recommends create a symbolic link to this location:
```
$ ln -sfv /usr/local/opt/mongodb/*.plist ~/Library/LaunchAgents
```
##### A couple of useful commands
**OS X** uses the `launchctl` command to interface with the `launchd` daemon. So once we have the `.plist` file in place, we can use a couple of useful commands for start/stop the server:
```
$ launchctl stop homebrew.mxcl.mongodb
```

And to start it again:
```
$ launchctl start homebrew.mxcl.mongodb
```

#### Using a configuration file
For those people who don't want/need having the `mongod` process launched at login, the **caveats** provide the alternative of using a **configuration file**:
```
$ mongod --config /usr/local/etc/mongod.conf
```

The problem with this approach, is having to remember the exact location that Homebrew placed this file. A simple solution is adding an **alias** to our shell configuration:
```
alias mongo='mongod --config /usr/local/etc/mongod.conf'
```

This way we just have to run `mongod` to have it running with the following settings:
```
systemLog:
  destination: file
  path: /usr/local/var/log/mongodb/mongo.log
  logAppend: true
storage:
  dbPath: /usr/local/var/mongodb
net:
  bindIp: 127.0.0.1
```

These settings can be modified to fit our needs.

## Uninstalling MongoDB
One of the advantages of installing [MongoDB][1] using [Homebrew][2] is how easy is to get rid of it:

```bash
$ brew uninstall mongodb
```

---
[:arrow_backward:][back] ║ [:house:][home] ║ [:arrow_forward:][next]

<!-- navigation -->
[home]: ../README.md
[back]: ../README.md
[next]: manual_installation.md

<!-- links -->
[1]: https://www.mongodb.org/
[2]: http://brew.sh/
[3]: https://www.mongodb.org/downloads
[4]: https://github.com/lifeBalance/notes-mongodb/blob/master/README/agent_mongod.md
