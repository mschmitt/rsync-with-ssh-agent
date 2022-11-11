**Shared actions must be public, but this is a research project under development. Don't waste your precious time.**

# rsync-with-ssh-agent

## Inputs

- Mandatory
      - SSH_HOST
      - SSH_USER
      - SSH_KEY
      - SSH_PASSPHRASE
- Optional
      - SSH_PORT (default: 22)
      - RSYNC_LOCAL_PATH (default: .)
      - RSYNC_REMOTE_PATH (default: .)
      - RSYNC_OPTIONS (default: --dry-run)

Local dot files are always excluded from uploads. If --dry-run is removed from RSYNC_OPTIONS, --delete is implied, which may lead to data loss on the receiving end. Check Job output before removing the --dry-run option by setting RSYNC_OPTIONS to an empty string.

## Sample invocation

```
      - uses: mschmitt/rsync-with-ssh-agent@HEAD
        with: 
            SSH_KEY: ${{ secrets.SSH_KEY }}
            SSH_PASSPHRASE: ${{ secrets.SSH_PASSPHRASE }}
            SSH_USER: ${{ secrets.SSH_USER }}
            SSH_HOST: ${{ secrets.SSH_HOST }}
            SSH_PORT: ${{ secrets.SSH_PORT }}
            RSYNC_REMOTE_PATH: .
            RSYNC_LOCAL_PATH: .
            RSYNC_OPTIONS: '--dry-run'
```
