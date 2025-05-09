# rsync-server

A `rsync` server with `sshd` in Docker. Forked from [axiom-data-science/rsync-server](https://github.com/axiom-data-science/rsync-server) with the `rsyncd` daemon disabled.

## Quickstart

Build the image and start a server:

```shell
docker build -t rsync-server --pull .

docker run \
    --name rsync-server \
    -p 9000:22 \
    -e PASSWORD=secret \
    rsync-server
```

## Usage

### Environment Variables

|     Parameter     | Function |
| :---------------: | -------- |
| `PASSWORD`        | the `ssh` password. **One of `PASSWORD` or `PASSWORD_FILE` is required.**|
| `PASSWORD_FILE`   | path to a file containing the `ssh` password. **One of `PASSWORD` or `PASSWORD_FILE` is required.**|
| `AUTHORIZED_KEYS` | the `ssh` key (for root user). defaults empty |

### Set password via file

```shell
docker run \
    -p 9000:22 \
    -v /path/to/password_file:/password \
    -e PASSWORD_FILE=/password \
    rsync-server
```

### Use SSH key authentication

You may mount your public key or
`authorized_keys` file to `/root/.ssh/authorized_keys`. This file
must have owner root, group root, and 400 octal permissions.

Alternatively, you may specify the `AUTHORIZED_KEYS` environment variable.

**You must set a password via `PASSWORD` or `PASSWORD_FILE`, even if you are using key authentication.**

```shell
docker run \
    -e PASSWORD=secret \
    -v /path/to/authorized_keys:/root/.ssh/authorized_keys
    -p 9000:22 \
    rsync-server
```

```shell
rsync -av -e "ssh -i /path/to/private.key -p 9000" /path/to/source/ root@localhost:/path/to/destination/
```
