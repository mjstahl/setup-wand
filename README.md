# setup-wand

Installs a released [wand](https://github.com/mjstahl/wand) binary and puts it
on `PATH`.

```yaml
- uses: mjstahl/setup-wand@v1
- run: wand test test/
```

## Inputs

| | | |
|---|---|---|
| `version` | `latest` | A version such as `0.1.0`, or `latest` |
| `token` | `${{ github.token }}` | Used to resolve `latest` through the GitHub API |

## Outputs

| | |
|---|---|
| `version` | the version installed, without the leading `v` |
| `path` | the directory the binary was installed into |

## What it does

Works out the runner's platform, downloads the matching archive from wand's
releases, **verifies its `sha256`**, unpacks it into the runner tool cache,
and adds it to `PATH`. A second run on the same runner reuses the cached
copy.

Before returning, it runs `wand e '1 + 2'` from a directory that is not a
wand tree. wand carries its own standard library, so that is the whole
claim the binary makes, and checking it here means a broken download fails
in this step rather than in yours.

## Platforms

`linux-x86_64`, `linux-aarch64`, `macos-aarch64`, `macos-x86_64`.

GitHub's Intel macOS runners are not schedulable in practice, so
`macos-x86_64` is untested here, though the action installs it the same way
as the rest.

## Why a workflow is the place to start

CI is a controlled environment where per-repo tooling is already normal:
installing a language nobody has heard of is unremarkable in a workflow and
a conversation on a laptop. It also lets a script be adopted one repository
at a time.

This action's own test suite is written in wand — see `test/`.
