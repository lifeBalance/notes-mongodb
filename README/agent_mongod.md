# Launching mongod when booting
OS X systems use a daemon named [launchd][2], which is an open-source **service management framework** for managing daemons, applications, processes, and scripts at boot time. [launchd][2] was developed at Apple by Dave Zarzycki and released in 2005 with Mac OS X Tiger.

The purpose of [launchd][2] is to replace a bunch of other services used in UNIX systems, under one unified system. There are two main programs in the **launchd system**: `launchd` and `launchctl`.


## launchd
`launchd` manages the daemons at both a system and user level. It has two main tasks:

1. The first is to boot the system.
2. The second is to load and maintain services.

The parameters of the services run by `launchd` are defined in configuration files known as **property lists**. These files are written in XML and easily recognizable because they use the `.plist` extension. Property lists are also known as **job definitions**.


### Daemons and agents
`launchd` differentiates between **agents** and **daemons**:

1. An **agent** is run on behalf of the logged in user. LaunchAgents are loaded when a user logs in.
2. A **daemon** runs on behalf of the `root` user or any user you specify under the `<username>` property. LaunchDaemons are loaded at system boot.

Depending on where the **property list** file is stored it will be treated as a daemon or an agent. **Daemons** are stored in two locations:

* Global daemons are in `/Library/LaunchDaemons`
* System daemons are in `/System/Library/LaunchDaemons`

**Agents** may be stored in:

* User Agents are in `~/Library/LaunchAgents`
* Global Agents in `/Library/LaunchAgents`
* System Agents in `/System/Library/LaunchAgents`


## launchctl
The other part of the **launchd system** is `launchctl`, which is a command line application which talks to `launchd` and knows how to parse the property list files used to describe `launchd` jobs. `launchctl` is uses mainly for:

* Load and unload daemons or agents.
* Start and stop them.

There's is a difference between loading and starting a job. Loading a job definition does not necessarily mean to start the job. Only when the `RunAtLoad` or `KeepAlive` properties have been specified, `launchd` will start the loaded job.


## A plist file for mongod
To have the `mongod` process running every time we boot our system, we could create the following **job definition**:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
  <key>Label</key>
  <string>org.mongo.mongod</string>
  <key>ProgramArguments</key>
  <array>
    <string>/Users/javi/mongodb/bin/mongod</string>
    <string>run</string>
    <string>--dbpath</string>
    <string>/Users/javi/.mongodata/db</string>
    <string>--logpath</string>
    <string>/Users/javi/.mongologs/mongodb.log</string>
    <string>--fork</string>
  </array>
  <key>RunAtLoad</key>
  <true/>
  <key>KeepAlive</key>
  <false/>
  <key>HardResourceLimits</key>
  <dict>
    <key>NumberOfFiles</key>
    <integer>1024</integer>
  </dict>
  <key>SoftResourceLimits</key>
  <dict>
    <key>NumberOfFiles</key>
    <integer>1024</integer>
  </dict>
</dict>
</plist>
```

Save the file as `org.mongodb.mongod.plist`, and since we want this process to be owned by the user, save it at `~/Library/LaunchAgents/`.

### Starting stoping mongod
Once the process is loaded and, according to our preference, running at load, to **stop** we would do:
```
$ launchctl stop ~/Library/LaunchAgents/org.mongodb.mongod.plist
```

To start it again we would use `start`; and we could use `load` or `unload` also. The downside to using `launchctl` is having to remember the location of the `.plist` file. Fortunately, there are several programs that work as wrappers around `launchctl`, and are much easier to work with. Check for example:

* [lunchy][2], a friendly wrapper for `launchctl`, written in Ruby.
* [plunchy][3], a Python version.

---
[:arrow_backward:][back] ║ [:house:][home] ║ [:arrow_forward:][next]

<!-- navigation -->
[home]: ../README.md
[back]: configuration.md
[next]: #

<!-- links -->
[1]: https://en.wikipedia.org/wiki/Launchd
[2]: https://github.com/eddiezane/lunchy
[3]: https://github.com/epochblue/plunchy
