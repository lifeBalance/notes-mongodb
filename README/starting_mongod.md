# Start the MongoDB server
Once we have installed [MongoDB][1] as described in the previous section, before we start using it for the first time we have to follow a couple of additional steps:

## The data directory
Create the directory to which the `mongod` process will write data. By default, the `mongod` process uses the `/data/db` directory. If you create a directory other than this one, you must specify that directory with the `--dbpath` option (more about this option later).

The following example command creates the default `/data/db` directory:

```bash
$ mkdir -p /data/db
```

* Set **permissions** for the data directory. Before running the `mongod` process for the first time, ensure that the user account running `mongod` has **read** and **write** permissions for the data directory. Let's make it world readable/writable (valid for development purposes):

  ```bash
  $ chmod 777 /data/db
  ```

* If we have added the location of the [MongoDB][1] binaries to the `PATH` variable, and if we use the **default data directory** (`/data/db`) starting `mongod` is as simple as typing in our terminal:

  ```bash
  $ mongod
  ```

But if our `PATH` does not include the location of the `mongod` binary, we must enter the full path to the mongod binary at the system prompt:

```bash
$ ~/mongodb/bin/mongod
```

That is, assuming that we have installed [MongoDB][1] to `$HOME/mongodb` and that we are using the **default data directory**. If we are using other location for our data, we must specify it at the command line using the `--dbpath` option:

```bash
$ mongod --dbpath <path to data directory>
```

If we have followed the previous steps properly, now we should have a **MongoDB server** running at `localhost:27017`. To stop the server press `Control+C` in the terminal where the `mongod` instance is running, it performs a **clean shutdown**.

## Other options
There are several options we can use when starting the **server**, check [the docs][2] for a more detailed list. Here we are gonna mention just the most common options.

### Specifying a port
By default `mongod` starts at port **27017** but if we have some app already running there, or we want to start several `mongod` processes, we must specify a **port** for each one using the `--port` option:

```bash
$ mongod --port <number>
```

### Starting mongod as a Daemon
Having `mongod` running interactively means keeping a terminal tab open for the process to run. Not everybody likes that, so we have the possibility of running `mongod` as a **daemon**, in the background. We just have to start the process using the `--fork` option.

The downside to that, is that we lost any feedback from the server. To put a remedy to that, there's another option available that allows us to write all of this output to a **log file**. We must create a **log directory** though, since [MongoDB][1] doesn't include a default value for this directory, but the log file itself is created automatically if it doesn't exist.

Putting everything together, this is what we should run to start `mongod` as a daemon and write its logs to a folder of our choice:

```
$ mongod --fork --logpath ~/mongologs/mongodb.log
about to fork child process, waiting until server is ready for connections.
forked process: 32555
child process started successfully, parent exiting
```

## The MongoDB Shell
Once the `mongod` process is running, we can connect to it using the **MongoDB Shell**. We can do that opening another tab in our terminal and running:

```bash
$ mongo
MongoDB shell version: 3.0.0
connecting to: test
>
```
Type `exit` when you are done.

## Shutting down mongod
If you want to stop the `mongod` process there are several options:

1. We could kill the `mongod` process using the `kill` command and passing it the **process id**:

  ```bash
  $ kill 630
  ```

  > Never use `kill -9` (i.e. `SIGKILL`) to terminate a `mongod` instance.

2. As we said in a previous section, `Control+C` also performs a **clean shutdown**, but it only works when we are running the `mongod` instance in **interactive mode**, meaning we didn't use the `--fork` option when starting the server.

3. Start session on the `mongo` shell and issuing the following commands:
```
> use admin
switched to db admin
> db.shutdownServer()
...
> exit
```

These are all perfectly viable ways of shutting the `mongod` process in a clean way.

---
[:arrow_backward:][back] ║ [:house:][home] ║ [:arrow_forward:][next]

<!-- navigation -->
[home]: ../README.md
[back]: installing.md
[next]: problems_starting_mongod.md


<!-- links -->
[1]: https://www.mongodb.org/
[2]: https://docs.mongodb.org/manual/tutorial/manage-mongodb-processes/
