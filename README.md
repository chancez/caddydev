# caddydev
Tool for developing custom [Caddy](http://caddyserver.com) middleware/add-ons. It basically runs caddy like `go run` does except it splices in your add-on so you can invoke it with the new directive in your Caddyfile.

## Installation
```shell
$ go get github.com/caddyserver/caddydev
```

## Example
#### 1. Pull hello middleware.
```shell
$ go get github.com/abiosoft/hello-caddy
```
#### 2. Start caddydev.
**Don't forget to use a Caddyfile that has your new directive in it!**
```shell
$ cd $GOPATH/src/github.com/abiosoft/hello-caddy
$ caddydev
Starting caddy...
0.0.0.0:2015
```
#### 3. Test it.
```
$ curl localhost:2015
Hello, I'm a caddy middleware
```
This works because the hello package comes with a sample Caddyfile that invokes it.

[github.com/abiosoft/hello-caddy](https://github.com/abiosoft/hello-caddy) can be the template for your new middleware. Follow the link to learn more.

## Usage
caddydev creates and starts a custom Caddy on the fly with the currently developed middleware integrated.

The following configurations defines how caddydev runs. Only `directive` is required, everything else is optional.

Config | Type | Info | Default
-------|------|------|--------
directive | string |Directive of the middleware being developed. e.g. `hello` |
source | string | Path to middleware source (or Go import path if available in $GOPATH) | current directory
after | string | Priority. After which directive should this directive be placed. |
update | boolean | Pull latest caddy source before building | false
args | string | arguments to pass to resulting caddy binary |
go_args | string | 'go build' arguments to build with e.g. `-race -x -v` |

There are two ways to specify this.

#### 1. config.json file
If specified, cli args are not required.
```json
{
    "directive" :   "hello",
    "source"    :   "path/to/source or github.com/user/project",
    "after"     :   "proxy",
    "update"    :   false,
    "args"      :   "-port 8080 -http=false",
    "go_args"   :   "-race -x -v"
}
```
#### 2. CLI args
If specified, takes priority over any matching config in `config.json`.
```bash
$ caddydev -h
Usage: caddydev [[options] directive [caddy args] [go [build args]]]

options:
  -c, --conf="config.json" Config file to read from.
  -s, --source="."         Source code directory or go get path.
  -a, --after=""           Priority. After which directive should our new directive be placed.
  -u, --update=false       Pull latest caddy source code before building.
  -o, --output=""          Path to save custom build. If set, the binary will only be generated, not executed.
                           Set GOOS, GOARCH, GOARM environment variables to generate for other platforms.
  -h, --help=false         Show this usage.

directive:
  directive of the middleware being developed.

caddy args:
  args to pass to the resulting custom caddy binary.

go build args:
  args to pass to 'go build' while building custom binary prefixed with 'go'.
  go keyword is used to differentiate caddy args from go build args.
  e.g. go -race -x -v.
```

### Note
caddydev is in active development and can still change significantly.

### Disclaimer
This software is provided as-is and you assume all risk.
