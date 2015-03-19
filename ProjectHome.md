This is a very simple project than takes N number of files and uploads them via scp using ant.

It's designed to be drop-in release build in your build pipeline. The input is a bunch of files and one property file dropped into the artifacts folder.

For example:
```
artifacts/foo-42.jar
artifacts/bar-42.jar
artifacts/release.properties
```

And release.properties contains

```
release.path=/com/example/foo
release.files=foo-42.jar,bar-42.jar
```

Server details can either be put in this file or provided as build properties for the agent

```
server.name=example.com
server.path=/home/user/www
```

'server.path' and 'release.path' will be combined to make the final path. This allows the release to be agnostic of the final location. Useful when the release folder structure is part of a convention (i.e. Maven) but the server folder stucture points to the root of a web server.
Authentication is now handled by your native ~/.ssh/config file, so it's recommended that you login to the account running your build agent and setup the config file before running the build. Steps:

```
ssh-keygen -f someKeyFile
ssh-copy-id -i someKeyFile someValidUser@some.server.com
nano ~/.ssh/config
```

Putting the following in the file

```
Host some.server.com
User someValidUser
IdentityFile someKeyFile
```

This now means that no username or password information will be visible in the log.

Then just setup your build pipeline to trigger this project