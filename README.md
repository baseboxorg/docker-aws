AWS CLI
=======

This git repo provides [AWS CLI](http://aws.amazon.com/cli/)
from [PIP](https://pypi.python.org/pypi/awscli) in a Docker container.

Project:            https://github.com/jumanjihouse/docker-aws<br/>
Docker image:       https://registry.hub.docker.com/u/jumanjiman/aws/<br/>
Upstream changelog: https://github.com/aws/aws-cli/blob/develop/CHANGELOG.rst

[![Download size](https://images.microbadger.com/badges/image/jumanjiman/aws.svg)](http://microbadger.com/images/jumanjiman/aws "View on microbadger.com")
[![Version](https://images.microbadger.com/badges/version/jumanjiman/aws.svg)](http://microbadger.com/images/jumanjiman/aws "View on microbadger.com")
[![Source code](https://images.microbadger.com/badges/commit/jumanjiman/aws.svg)](http://microbadger.com/images/jumanjiman/aws "View on microbadger.com")
[![Docker Registry](https://img.shields.io/docker/pulls/jumanjiman/aws.svg)](https://registry.hub.docker.com/u/jumanjiman/aws)&nbsp;
[![Circle CI](https://circleci.com/gh/jumanjihouse/docker-aws.png?circle-token=5303a3a083c3d19463bbd1b08937b24b3417d70e)](https://circleci.com/gh/jumanjihouse/docker-aws/tree/master 'View CI builds')

[![Throughput Graph](https://graphs.waffle.io/jumanjihouse/docker-aws/throughput.svg)](https://waffle.io/jumanjihouse/docker-aws/metrics)


How-to
------

### Build and test

We use circleci to build, test, and publish the image to Docker hub.
We use [BATS](https://github.com/sstephenson/bats) to run the test harness.
BATS output resembles:

    ✓ awscli shows help with no options
    ✓ awscli is the correct version
    ✓ ci-build-url label is present

    3 tests, 0 failures

:warning: Build requires Docker 1.9.0 or higher for `--build-arg`.
If you want to build locally, you must export an environment variable
to specify the `VERSION` of awscli.


### Pull an already-built image

    docker pull jumanjiman/aws


### Labels

Each built image has labels that generally follow http://label-schema.org/

We add a label, `ci-build-url`, that is not currently part of the schema.
This extra label provides a permanent link to the CI build for the image.

View the ci-build-url label on a built image:

    docker inspect \
      -f '{{ index .Config.Labels "io.github.jumanjiman.ci-build-url" }}' \
      jumanjiman/aws

Query all the labels inside a built image:

    docker inspect jumanjiman/aws | jq -M '.[].Config.Labels'


### Run

Interactively:

    docker run --rm -it \
    -e AWS_ACCESS_KEY_ID=<snip> \
    -e AWS_SECRET_ACCESS_KEY=<snip> \
    -e AWS_DEFAULT_REGION=us-west-2  \
    --read-only \
    --cap-drop all \
    jumanjiman/aws ec2 describe-instances

As a simplification, add this to your `~/.bashrc`:

    # Use a remote docker host.
    export DOCKER_HOST='tcp://192.168.254.162:2375'

    # Put your keys in the redacted values.
    function aws {
      docker run --rm -it \
      -e AWS_ACCESS_KEY_ID=redacted \
      -e AWS_SECRET_ACCESS_KEY=redacted \
      -e AWS_DEFAULT_REGION=us-west-2 \
      jumanjiman/aws $@
    }

Then `source ~/.bashrc` and simply run `aws <your args>`.
