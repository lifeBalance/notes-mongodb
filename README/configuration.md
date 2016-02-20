# Configuring MongoDB
If we installed [MongoDB][1] using [Homebrew][2], we'll have a configuration file named `mongo.conf` at `/usr/local/etc/`. But if we chose to install [MongoDB][1] manually, the installation doesn't include a configuration file to set up some default behaviors. The only **default setting** that we get out of the box is the location of the data, at `/data/db`. Apart from that, the options are wide open.

So say we decide to start `mongod` and we have moved our database to a location other than the default, and also we want to run it as a daemon and our logs redirected to some file, the command to accomplish all that gets pretty verbose:

```bash
$ mongod --fork --logpath ~/.mongologs/mongodb.log --dbpath /Users/javi/.mongodata/db
```

Having to type all that every time we start the server is a bit cumbersome. Fortunately, according to the [docs][3] it's possible to set all of these options in the **configuration file**. This way we just have to pass this file along with the `--config` option (`-f` for short):

```bash
$ mongod -f <path-to-file>
```
The downside to this, we'll have to remember the location of the configuration file.

## Writing a configuration file
The configuration file contains settings that are equivalent to the `mongod` command-line options. This file must be written using valid [YAML][4]. Below I've included a simple example:

```yaml
systemLog:
   destination: file
   path: '/Users/javi/.mongologs/mongodb.log'
   logAppend: true
processManagement:
   fork: true
net:
   bindIp: 127.0.0.1
   port: 27017
security:
    authorization: 'enabled'
storage:
    dbPath: '/Users/javi/.mongodata/db'
```

Check the [official documentation][3] for a detailed list of options.

---
[:arrow_backward:][back] ║ [:house:][home] ║ [:arrow_forward:][next]

<!-- navigation -->
[home]: ../README.md
[back]: agent_mongod.md
[next]: problems_starting_mongod.md

<!-- links -->
[1]: https://www.mongodb.org/
[2]: http://brew.sh/
[3]: https://docs.mongodb.org/manual/administration/configuration/
[4]: http://www.yaml.org/
