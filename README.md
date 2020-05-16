# Git-Bash post{script} lifecycle script test

## Goal

Isolate why some long-running scripts in [Git-Bash][] trigger post{script} lifecycle scripts after being killed with `control-c` while others do not.

## Setup

The usual: `npm install`

The primary issue

## Tests

There are four scripts, each of which has an accompanying post{script} that simply echoes its name.

Killing long-running scripts with `control-c` _should_ trigger the post{script}, but sometimes fails.

### `npm run echo`

This is just a simple control which prints **echo script**, then triggers `postecho` which prints **postecho script**.

### `npm run pyserver`

This uses cross-env-shell to start a python3 webserver on port 8000. The `postpyserver` script fails to run after killing this with `control-c`.

### `npm run server`

Runs [http-server][] which listens on port 8080. Killing this should trigger `postserver` which echoes its name.

### `npm run watch`

This starts a [chokidar][] file watcher in the current directory. The `postwatch` script fails to run after killing this with `control-c`.

## Notes

This only seems to be an issue with Git-Bash for some long-running processes.  Post{script} lifecycle scripts work correctly from cmd.exe and Powershell, the same as they do on Mac/Linux.  

## Related issues

These may be relevant:

* [npm config set script-shell](https://docs.npmjs.com/misc/config#script-shell) 
* Stack Overflow: [How to set shell for npm run-scripts in Windows](https://stackoverflow.com/questions/23243353/how-to-set-shell-for-npm-run-scripts-in-windows)
 * node#16103 [Ctrl + C Doesn't Kill Gracefully | Node v6.11.3 - Git Bash](https://github.com/nodejs/node/issues/16103)
 * nodemon#1109 [Nodemon does not kill process on exit with Git Bash on Windows 10](https://github.com/remy/nodemon/issues/1109)


[git-bash]: https://gitforwindows.org/
[http-server]: https://www.npmjs.com/package/http-server
[chokidar]: https://www.npmjs.com/package/chokidar
