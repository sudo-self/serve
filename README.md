# <img src="https://raw.githubusercontent.com/syntaqx/serve/main/docs/logo.svg?sanitize=true" width="250">

`serve` is a static http server anywhere you need one.

![Untitled 2](https://user-images.githubusercontent.com/119916323/233626674-7716f5c7-a443-48ad-ba88-862926e8fe3d.jpg)


## brew install serve

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
