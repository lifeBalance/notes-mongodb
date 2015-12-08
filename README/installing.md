# Install MongoDB on OS X
There are a couple of ways of installing [MongoDB][1]:

1. Using the the popular OS X package manager [Homebrew][2].
2. Downloading it from the [MongoDB Download site][3] and install it manually.

## Install it with Homebrew
If you are on a Mac and use [Homebrew][2] as your package manager, installing [MongoDB][1] is quite easy. First of all, we have to update Homebrew’s package database:

```bash
$ brew update
```

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

Using [Homebrew][2] is the officially recommended way of installing [MongoDB][1] on OS X. It provides a installation with sane defaults, such as:

* Configuration file: `/usr/local/etc/mongod.conf`
* A data directory: `/usr/local/var/mongodb`
* A place for logs: `/usr/local/var/log/mongodb/mongo.log`
* A `plist` file: `/usr/local/opt/mongodb/homebrew.mxcl.mongodb.plist`

This way of installation also provides with a couple of useful commands for start/stop the server:
```bash
$ launchctl stop homebrew.mxcl.mongodb
```

And to start it again:
```bash
$ launchctl start homebrew.mxcl.mongodb
```

But if you can't or don't want to use [Homebrew][2], keep reading.

## Install MongoDB Manually
1. First of all we have to download the MongoDB binaries from the [MongoDB Download site][3]. We can do so from the command line:

  ```bash
  $ curl -O https://fastdl.mongodb.org/osx/mongodb-osx-x86_64-3.0.7.tgz
  ```
2. Then we have to extract the files from the downloaded archive, also doable from the command line:

  ```bash
  $ tar -zxvf mongodb-osx-x86_64-3.0.7.tgz
  ```

3. Copy the extracted folder to the location from which MongoDB will run, say we decide to run it from our `$HOME` directory:

  ```bash
  $ mkdir -p mongodb
  $ cp -R -n mongodb-osx-x86_64-3.0.7/ ~/mongodb
  ```

4. Ensure the location of the **binaries** is in the `PATH` variable. The [MongoDB][1] binaries are in the `bin/` directory, inside the `mongodb` folder, so in order to add it to our `PATH` we would add this line to our shell's configuration file:

  ```bash
  export PATH=$HOME/mongodb/bin:$PATH
  ```

## Uninstalling MongoDB
If we installed [MongoDB][1] using [Homebrew][2], we are gonna uninstall it using:

```bash
$ brew uninstall mongodb
```

If we took the **manual** option, we'll just have to delete the same folder and files we added, namely:
* The folder containing the **binaries**.
* The folder containing the **data**.
* Any other configuration file we may have added (a `plist`, `.mongorc.js`, etc).

---
[:arrow_backward:][back] ║ [:house:][home] ║ [:arrow_forward:][next]

<!-- navigation -->
[home]: ../README.md
[back]: ../README.md
[next]: #

<!-- links -->
[1]: https://www.mongodb.org/
[2]: http://brew.sh/
[3]: https://www.mongodb.org/downloads
