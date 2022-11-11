# rsync-with-ssh-agent

## Synopsis

```
      - uses: mschmitt/rsync-with-ssh-agent@HEAD
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

* All local dot files are excluded from uploads. 
* If `--dry-run` is removed from `RSYNC_OPTIONS`, `--delete` is implied, which may (and will) lead to data loss on the receiving end. 
* Check Job output before removing the `--dry-run` option by setting `RSYNC_OPTIONS` to an empty string.

## Personal note

I use per-repository SSH keys and configure the receiving rsync in _~/.ssh/authorized_keys_ as follows:

```
restrict,command="rrsync /foo/deploy" ssh-ed25519 AAAAC3...
```

`rrsync` (_restricted rsync_) is part of the `rsync` package.
