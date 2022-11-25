# rsync-with-ssh-agent

 [![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT) [![rsync tests](https://github.com/mschmitt/rsync-with-ssh-agent/actions/workflows/tests.yml/badge.svg)](https://github.com/mschmitt/rsync-with-ssh-agent/actions/workflows/tests.yml)

## Name

rsync-with-ssh-agent - Sync files by rsync with SSH key, agent and passphrase

## Synopsis

```yaml
- uses: mschmitt/rsync-with-ssh-agent@v2
  with: 
      SSH_KEY: ${{ secrets.SSH_KEY }}
      SSH_PASSPHRASE: ${{ secrets.SSH_PASSPHRASE }}
      SSH_USER: ${{ secrets.SSH_USER }}
      SSH_HOST: ${{ secrets.SSH_HOST }}
      SSH_PORT: ${{ secrets.SSH_PORT }}
      RSYNC_LOCAL_PATH: .
      RSYNC_REMOTE_PATH: .
      RSYNC_OPTIONS: '--include .htaccess --exclude ".*" --recursive --delete-excluded --verbose --dry-run' 
```

## Description

An rsync uploader that is believed to offer a reasonable amount of security, due to its support for an SSH passphrase.

Together with restricted rsync on the receving end (`rrsync`, see _Application Note_ below), it should be able to securely handle most upload scenarios.

## Inputs

* Mandatory
    * `SSH_HOST`
    * `SSH_USER`
    * `SSH_KEY`
    * `SSH_PASSPHRASE`
* Optional
    * `SSH_PORT` (default: 22)
    * `RSYNC_LOCAL_PATH` (default: .)
    * `RSYNC_REMOTE_PATH` (default: .)
    * `RSYNC_OPTIONS` (default: empty)

## See also

The wiki for in-depth explanations and examples: 

* https://github.com/mschmitt/rsync-with-ssh-agent/wiki

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
