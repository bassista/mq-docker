# IBM MQ 7.5 on Docker

**Important!** This Dockerfile for 7.5.0 is based on the version for 8.0.0 supplied by IBM. This 7.5.0 version is **experimental** and should not be used in production. Specifically:

- The `config.mqsc` queue manager config script disables security.
- The `mq.sh` startup script does not run `mqconfig` to check that the system has sufficient resources to run MQ.

## How to run

1. Download the MQ v7.5 Linux tar from IBM, i.e. `mqadv_dev75_linux_x86-64.tar`.
1. If it's not already gzipped, gzip it.
1. Place the file on a local web server.
1. Edit the _Dockerfile_, setting the URL of the tar.gz in the `MQ_URL` environment variable.
1. Modify the _Dockerfile_ and `config.mqsc` in the `mymq7` directory to suit your needs.

Then run using the following:

    docker build --tag mq7 ./7.5.0/
    cd mymq7
    docker build -t mymq7 .
    docker run --env LICENSE=accept --env MQ_QMGR_NAME=QM1 --volume /var/demo:/var/mqm --publish 1414:1414 mymq7

## Further help

See the README in the root of this repo.
