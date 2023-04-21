# <img src="https://raw.githubusercontent.com/syntaqx/serve/main/docs/logo.svg?sanitize=true" width="250">

`serve` is a static http server anywhere you need one.

[homebrew]:   https://brew.sh/
[git]:        https://git-scm.com/
[golang]:     https://golang.org/
[releases]:   https://github.com/syntaqx/serve/releases
[modules]:    https://github.com/golang/go/wiki/Modules
[docker-hub]: https://hub.docker.com/r/syntaqx/serve

[![Mentioned in Awesome Go](https://awesome.re/mentioned-badge.svg)](https://github.com/avelino/awesome-go)

[![codecov](https://codecov.io/gh/syntaqx/serve/branch/main/graph/badge.svg?token=FGkU1ntp8z)](https://codecov.io/gh/syntaqx/serve)
[![Go Report Card](https://goreportcard.com/badge/github.com/syntaqx/serve)](https://goreportcard.com/report/github.com/syntaqx/serve)

[![GoDoc](https://godoc.org/github.com/syntaqx/serve?status.svg)](https://godoc.org/github.com/syntaqx/serve)

[![GitHub Release](https://img.shields.io/github/release-pre/syntaqx/serve.svg)][releases]
[![Docker Cloud Automated build](https://img.shields.io/docker/cloud/automated/syntaqx/serve.svg)][docker-hub]
[![Docker Cloud Build Status](https://img.shields.io/docker/cloud/build/syntaqx/serve.svg)][docker-hub]
[![Docker Pulls](https://img.shields.io/docker/pulls/syntaqx/serve.svg)][docker-hub]

![Untitled 2](https://user-images.githubusercontent.com/119916323/233626674-7716f5c7-a443-48ad-ba88-862926e8fe3d.jpg)


##brew install server

> It's basically `python -m SimpleHTTPServer 8080` written in Go

* HTTPS (TLS)
* CORS support
* Request logging
* `net/http` compatible




Here's an example using `docker-compose.yml` to configure `serve` to use HTTPS:

```yaml
version: '3'
services:
  web:
    image: syntaqx/serve
    volumes:
      - ./static:/var/www
      - ./fixtures:/etc/ssl
    environment:
      - PORT=1234
    ports:
      - 1234
    command: serve -ssl -cert=/etc/ssl/cert.pem -key=/etc/ssl/key.pem -dir=/var/www
```

Then simply open your browser to http://localhost:8080 to view your server.

### Options

The following configuration options are available:

* `--host` host address to bind to (defaults to `0.0.0.0`)
* `--port` listening port (defaults to `8080`)
* `--ssl` enable https (defaults to `false`)
* `--cert` path to the ssl cert file (defaults to `cert.pem`)
* `--key` path to the ssl key file (defaults to `key.pem`)
* `--dir` directory path to serve (defaults to `.`, also configurable by `arg[0]`)
* `--users` path to users file (defaults to `users.dat`); file should contain lines of username:password in plain text

## Development

To develop `serve` or interact with its source code in any meaningful way, be
sure you have the following installed:

### Prerequisites

* [Git][git]
* [Go 1.13][golang]

### Tooling

* [pre-commit](https://pre-commit.com/)

> __Note__: While the tooling isn't explicitly required in order to build and
> run the project, it's for everyone's benefit that you leverage it.

### Install

You can download and install the project from GitHub by simply running:

```sh
git clone git@github.com:syntaqx/serve.git && cd $(basename $_ .git)
make install
```

This will install `serve` into your `$GOPATH/bin` directory, which assuming is
properly appended to your `$PATH`, can now be used:

```sh
$ serve version
serve version v0.0.6-8-g5074d63 windows/amd64
```

## Using `serve` manually

Besides running `serve` using the provided binary, you can also embed a
`serve.FileServer` into your own Go program:

```go
package main

import (
    "log"
    "net/http"

    "github.com/syntaqx/serve"
)

func main() {
    fs := serve.NewFileServer()
    log.Fatal(http.ListenAndServe(":8080", fs))
}
```



As for any pre-built image usage, it is the image user's responsibility to
ensure that any use of this image complies with any relevant licenses for all
software contained within.
