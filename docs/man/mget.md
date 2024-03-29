mget 1 "March 2019" Manta "Manta Commands"
=======================================

NAME
----

mget - download an object from Manta.

SYNOPSIS
--------

`mget` [OPTION...] PATH...

DESCRIPTION
-----------

Retrieves the content of object(s) specified by PATH(s), and dumps them to
stdout.  Note that mget does not perform incremental/resumable downloads, so any
network-level errors that occur during transfer will result in an incomplete
and/or corrupt output.  You should check for an exit status of 0 to know that
the request was successful.

Note that mget by default will emit a progress meter on stderr.  You can
disable this with `-q`.

EXAMPLES
--------

Retrieves the given object, with a progress indicator.

    $ mget ~~/stor/README.md > /tmp/README.md

Retrieves the given object, with a progress indicator, and stores it in the
file README.md in the current directory.

    $ mget -O ~~/stor/README.md

Finds and fetches a set of Javascript files, and pipes them to less.

    $ mfind -t o -n '.js$' ~~/stor/foo | xargs mget -q | less

OPTIONS
-------

`-a, --account login`
  Authenticate as account (login name).

`-H, --header='header: value'`
  Additional HTTP header to include. Can be specified multiple times. Must be
  specified for each additional header.

`-h, --help`
  Print a help message and exit.

`-i, --insecure`
  This option explicitly allows "insecure" SSL connections and transfers.  All
  SSL connections are attempted to be made secure by using the CA certificate
  bundle installed by default.

`-k, --key fingerprint`
  Authenticate using the SSH key described by FINGERPRINT.  The key must
  either be in `~/.ssh` or loaded in the SSH agent via `ssh-add`.

`--role=ROLE,ROLE,...`
  Specify which roles to assume for the request.

`-o, --output file`
  Write output to &lt;file&gt; instead of stdout.

`-O, --remote-name`
  Write output to a file using the requested object's name (i.e. the last
  element of the full object path) instead of stdout.

`-q, --quiet`
  Do not display a progress meter.

`--user user`
  Authenticate as user under account.

`-u, --url url`
  Manta base URL (such as `https://us-central.manta.mnx.io`).

`-v, --verbose`
  Print debug output to stderr.  Repeat option to increase verbosity.

ENVIRONMENT
-----------
`MANTA_USER`
  In place of `-a, --account`.

`MANTA_SUBUSER`
  In place of `--user`.

`MANTA_KEY_ID`
  In place of `-k, --key`.

`MANTA_ROLE`
  In place of `--role`.

`MANTA_URL`
  In place of `-u, --url`.

`MANTA_TLS_INSECURE`
  In place of `-i, --insecure`.

The shortcut `~~` is equivalent to `/:login`
where `:login` is the account login name.

DIAGNOSTICS
-----------

When using the `-v` option, diagnostics will be sent to stderr in bunyan
output format.  As an example of tracing all information about a request,
try:

    $ mget -vv ~~/stor/foo 2>&1 | bunyan

BUGS
----

DSA keys do not work when loaded via the SSH agent.

Report bugs at [Github](https://github.com/TritonDataCenter/node-manta/issues)
