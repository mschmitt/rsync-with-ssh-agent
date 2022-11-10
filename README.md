# rsync-with-ssh-agent

All remote-related information is considered secret and needs to be configured in repository secrets:
- SSH_HOST
- SSH_PORT (default: 22)
- SSH_USER
- SSH_KEY
- SSH_PASSPHRASE
- RSYNC_REMOTE_PATH (default: .)

Pass to the action:
- RSYNC_LOCAL_PATH (default: .)
- RSYNC_OPTIONS (default: --dry-run)

Local dot files are always excluded from uploads. If --dry-run is removed from RSYNC_OPTIONS, --delete is implied, which may lead to data loss on the receiving end. Check Job output before removing the --dry-run option.
