## r-u-ready to accept connections pgsql/mysql or any web service?

`./r-u-ready` is a script created to wait for services like pgsql/mysql/django/flask in docker containers. It was inspired by [eficode/wait-for](https://github.com/eficode/wait-for) and [vishnubob/wait-for-it](https://github.com/vishnubob/wait-for-it).

When using this tool, you only need to pick the `r-u-ready` file as part of your project.

## Usage

```
./r-u-ready service [-h host] [-P port] [-u username] [-p password] [-t timeout] [-- command args]

  service REQUIRED                    pgsql | mysql | web

  -h HOST | --host=HOST               Domain/IP host (default: 127.0.0.1)
  -P PORT | --port=PORT               Port (default: 5432 for psql, 3306 for mysql, 80 for web)

  -u USERNAME | --username=USERNAME   MySQL username (required for mysql only)
  -p PASSWORD | --password=PASSWORD   MySQL password for the given username (required for mysql only)

  -q | --quiet                        Do not output any status messages
  -t TIMEOUT | --timeout=TIMEOUT      Timeout in seconds, zero for no timeout (default: 15 seconds)
  -- COMMAND ARGS                     Execute command with args after the test finishes
```

## Examples

To check if [google.com](https://google.com) is available:

```
$ ./r-u-ready web -h google.com -- echo "Google.com site is up"

Waiting for web server at google.com: to be ready...
web server at google.com: is ready for connection

Google.com site is up
```

To wait for mysql database to accept connections:

```
$ ./r-u-ready mysql -h 127.0.0.1 -P 3306 -u root -p <password> -t 300

Waiting for mysql server at 127.0.0.1:3306 to be ready...
mysql server at 127.0.0.1:3306 is ready for connection
```

To wait for database container to become available:

```
version: '3.7'

services:
  db:
    image: postgres:alpine

  backend:
    build: backend
    command: sh -c './r-u-ready pgsql -h db -t 60'
    depends_on:
      - db
```

## Note

Make sure postgresql-client or mariadb-client or curl is installed in your Dockerfile before running the command.
```
RUN apk add --update curl
RUN apk add postgresql-client
RUN apk add mariadb-client
```
