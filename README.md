# Logger

> Simple log functionality for bash

- Writes everything to /dev/stderr.
- STDIN support.
- Easy to setup.

## Usage

Simply source the script and use the functions it defines.


```bash
$ source ./logger; log_error "error"
2020-10-05T21:38:33+02:00[1] error
```

An important part of the logger utility is the stdin support.

```bash
$ source ./logger; echo 'warn' | log_warn
2020-10-05T21:38:33+02:00[2] warn
```

## Functions

Let's checkout all the functions that exist.

```bash
$ grep '^function ' ./logger
function log ()
function logify ()
function log_error() { log 1 "$1"; }
function log_warn()  { log 2 "$1"; }
function log_info()  { log 3 "$1"; }
function log_debug() { log 4 "$1"; }
function log_trace() { log 5 "$1"; }
```

So what is this `logify` function? What it does is call the command and
arguments and write any stderr using the `LOGGER_TEMPLATE`

## Variables

```bash
$ grep '^LOGGER_[A-Z]*=' ./logger
LOGGER_FILE="${LOGGER_FILE:-/dev/null}";
LOGGER_LEVEL="${LOGGER_LEVEL:-3}";
LOGGER_TEMPLATE="${LOGGER_TEMPLATE:-%s[%s] %b\n}";
```

You can write the log to a file by defining the `LOGGER_FILE`.

```bash
$ source ./logger; echo 'warn' | LOGGER_FILE="./logger.log" log_info "info"
2020-10-05T21:38:33+02:00[3] info
$ cat ./logger.log
2020-10-05T21:38:33+02:00[3] info
$ rm ./logger.log # Cleaning up

```

How to use the `LOGGER_LEVEL`?

```bash
$ source ./logger; log_trace "trace"

```

Notice that trace is not printed. By default the `LOGGER_LEVEL` is set to
info(3) by default.

```bash
$ source ./logger; LOGGER_LEVEL=5 log_trace "trace"
2020-10-05T21:38:33+02:00[5] trace
```

You can change the `LOGGER_TEMPLATE` if desired. The template is a printf
templates. See `man printf` for more information.

By default the `LOGGER_TEMPLATE` is the following.

```bash
$ source ./logger; echo "$LOGGER_TEMPLATE";
%s[%s] %b\n
$ source ./logger; printf "$LOGGER_TEMPLATE" '<datetime>' '<code>' '<message>'
<datetime>[<code>] <message>
```

## Improvements

- Allow changing the date format.

## License

TODO
