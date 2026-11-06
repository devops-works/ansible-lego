# ansible-lego

Installs a pinned release of [lego](https://github.com/go-acme/lego/), the ACME
client, as `/usr/local/bin/lego`.

The role used to install whatever release was latest on GitHub. lego 5 (May
2026) reorganised the command line: flags moved from global to subcommands,
`--path` disappeared, `run` and `renew` merged. Hosts rebuilt after that date
silently received an incompatible binary. The version is now pinned and the
role reinstalls whenever the installed binary reports another version, so a
bump of `lego_version` rolls out on the next run.

## Variables

- `lego_version`: release to install, without the leading `v` (default:
  `4.35.2`, the last 4.x). Bump deliberately, after migrating the callers'
  commands to the new CLI.
- `lego_arch`: architecture suffix of the release asset (default: `amd64`).
- `lego_force_install`: reinstall even when the installed binary already
  reports `lego_version` (default: `false`).

The archive is verified against the release's sha256 checksum file.

## Example

```yaml
- hosts: lb
  roles:
    - role: ansible-lego
      vars:
        lego_version: "4.35.2"
```
