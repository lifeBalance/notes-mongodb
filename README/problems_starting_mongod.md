# Common problems starting mongod
Some issues you may find when trying to start a `mongod` process.

## No database
Sometimes we get an error like:

```
$ mongod -f mongodb/mongod.conf
about to fork child process, waiting until server is ready for connections.
forked process: 33834
ERROR: child process failed, exited with error number 100
```

Error number **100** usually means that `mongod` can't find the data directory, so make sure this folder exist and, if it's not at the default location, that you are passing its path via configuration file, command line options or `.plist` file.

## Database permissions
Having a database is not enough, we also need permissions to read/write. Imagine you start successfully the `mongod` process and when you try a basic writing operation to the `test` database:

```
> db.people.insert({'name':'Javi'})
WriteResult({
	"writeError" : {
		"code" : 13,
		"errmsg" : "not authorized on test to execute command...
    ...
```

Often, people forget to set the proper permissions during installation. If we check permissions:
```
$ ls -ld ~/.mongodata/db
drwxr-xr-x  7 javi  staff       238 Dec  8 01:02 ./
```

> Notice I have my database at `~/.mongodata/db`, it doesn't have to be at `/data/db`.

We can see where the problem is, so let's set world readable/writable (valid for development purposes):

```bash
$ chmod 777 ~/.mongodata/db
$ ls -ld .mongodata/db
drwxrwxrwx  7 javi  staff  238 Dec  8 01:02 .mongodata/db
```
**IMPORTANT**: Don't forget to restart the `mongod` process to get rid of the error:exclamation:

## Server already running
It often happens (especially after coming back from a long break) that we want to start a server and we are prompted with some inexplicable error. If it's the case that you're trying to start `mongod`, and the terminal spits at you a long list of complaints, check if you have an old `mongod` instance already running in the background:

```bash
$ lsof -i :27017
COMMAND PID USER   FD  TYPE             DEVICE SIZE/OFF NODE NAME
mongod  630 javi   6u  IPv4 0x23d13495ddf529b3      0t0  TCP *:27017 (LISTEN)
```

So in this case, the reason why we can't start a `mongod` process, is because we already have one started.

## The rlimits Warning
Sometimes we could find the following warning:

```
** WARNING: soft rlimits too low. Number of files is 256, should be at least 1000
```

This is because in most UNIX based OS's the resources available to any process are limited. There are two types of **resource limits** (`rlimits`):

1. **Hard resource limits**, set by the root user using the `chuser` command for each user.
2. **Soft resource limits**, set by each individual user using the `ulimit` command, as long as the values are smaller than the hard resource limit values.

If we are running `mongod` in a development environment it's safe to just ignore the warning. To fix this warning, we could increase the number or `rlimits` using the `ulimit` command at the same time we launch `mongod`, something like:

```bash
$ ulimit -n 1024 && mongod
```

But it could be easier to set this limits in a `.plist` file, adding the following property:
```xml
<key>SoftResourceLimits</key>
<dict>
  <key>NumberOfFiles</key>
  <integer>1024</integer>
</dict>
```

---
[:arrow_backward:][back] ║ [:house:][home] ║ [:arrow_forward:][next]

<!-- navigation -->
[home]: ../README.md
[back]: starting_mongod.md
[next]: configuration.md

<!-- links -->
[1]: https://www.mongodb.org/
