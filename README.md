# rsync-with-ssh-agent

 [![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT) [![rsync tests](https://github.com/mschmitt/rsync-with-ssh-agent/actions/workflows/tests.yml/badge.svg)](https://github.com/mschmitt/rsync-with-ssh-agent/actions/workflows/tests.yml)

## Name

rsync-with-ssh-agent - Sync files by rsync with SSH key, agent and passphrase

## Synopsis

```
- uses: mschmitt/rsync-with-ssh-agent@v1
  with: 
      SSH_KEY: ${{ secrets.SSH_KEY }}
      SSH_PASSPHRASE: ${{ secrets.SSH_PASSPHRASE }}
      SSH_USER: ${{ secrets.SSH_USER }}
      SSH_HOST: ${{ secrets.SSH_HOST }}
      SSH_PORT: ${{ secrets.SSH_PORT }}
      RSYNC_LOCAL_PATH: .
      RSYNC_REMOTE_PATH: .
      RSYNC_OPTIONS: '--dry-run --include=.htaccess'
```

## Description

An rsync uploader that is believed to offer a reasonable amount of security, due to its support for an SSH passphrase.

Along with restricted rsync on the receving end (`rrsync`, see _Application Note_ below), it should be able to securely handle most upload scenarios.

**See notes on remote deletion below.**

## Inputs

* Mandatory
    * SSH_HOST
    * SSH_USER
    * SSH_KEY
    * SSH_PASSPHRASE
* Optional
    * SSH_PORT (default: 22)
    * RSYNC_LOCAL_PATH (default: .)
    * RSYNC_REMOTE_PATH (default: .)
    * RSYNC_OPTIONS (default: --dry-run)

## Notes

### Rsync options and parameters
* Dot files are excluded from uploads and may be re-included as shown above.
* Spaces in `RSYNC_REMOTE_PATH` *must* be backslash-escaped. Better avoid them altogether. (See `man rsync`: _ADVANCED USAGE_)
* `RSYNC_LOCAL_PATH` ending in a slash tranfers the directory contents, no slash transfers the directory itself. (See `man rsync`: _USAGE_)
* Implicit rsync options in the action are:
  * `--exclude '.*'`
  * `--recursive`
  * `--delete-excluded` (which itself implies `--delete`)
  * `--verbose`
* `RSYNC_OPTIONS` are inserted before the implicit options.
* A future minor update may introduce additional rsync options to be inserted after the implicit options.

### Authentication

* SSH with password instead of key is not supported and never will be.
* Untested with empty passphrase.

### Remote deletion in API v1 (current)

* The rsync invocation is configured to delete files from the destination that do not exist on the source. **This will easily lead to data loss on the receiving end.** 

* The default `--dry-run` in `RSYNC_OPTIONS` will prevent accidental deletion on early integration attempts.

* If `--dry-run` is removed from `RSYNC_OPTIONS`, `--delete` is implied. 

* Carefully check job output for unwanted _delete_ output before setting `RSYNC_OPTIONS` without the `--dry-run` option.

* Risk of catastrophic data loss is greatly reduced if used with `rrsync` on the receiving end (see _Application note_ below).

* Remote deletion defaults are subject to change in a future API v2.

### Compatibility

* Should be sufficiently cross-platform due to removal of `bash` specific features.

* May require additional installation of `ssh` and `rsync` in the environment. This is *not* the case on Gitlab's _Ubuntu_ runners.

## Application note

The author uses per-repository SSH keys and configures the receiving _rsync_ in _~/.ssh/authorized_keys_ as follows:

```
restrict,command="rrsync /foo/deploy" ssh-ed25519 AAAAC3...
```

`rrsync` (_restricted rsync_) is part of the `rsync` package.

## See it in use

* https://github.com/mschmitt/littlelink/tree/persona/informal/.github/workflows
* https://github.com/mschmitt/rsync-with-ssh-agent/blob/main/.github/workflows

## License

```
MIT License

Copyright (c) 2022 Martin Schmitt

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```
