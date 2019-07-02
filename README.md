# The Libyui Testing Image

[![Build Status](https://travis-ci.org/libyui/ci-libyui-container.svg?branch=master)](https://travis-ci.org/libyui/ci-libyui-container)

This git repository contains the configuration used to build the docker
image used for [TravisCI](https://travis-ci.org/).
The resulting docker image is available at https://registry.opensuse.org/.

## Automatic Rebuilds

- The image is rebuilt whenever a commit it pushed to the `master` branch.
- The [libyui-ci-libyui-container-master](
  https://ci.opensuse.org/view/libyui/job/libyui-ci-libyui-container-master/)
  Jenkins job copies the configuration to the
  [devel:libraries:libyui/ci-libyui-container](
  https://build.opensuse.org/package/show/devel:libraries:libyui/ci-libyui-container)
  OBS project
- The OBS tracks the dependencies and rebuilds the image if any dependant package
  is updated.

## Triggering a Rebuild Manually

If for some reason the automatic rebuild does not work or it failed you can
trigger the rebuild in the OBS just like for the other regular packages.


## The Image Content

This image is based on the latest openSUSE Tumbleweed image, additionally
it contains the packages needed for building and running tests in libyui packages.
[Examples](#examples) section below.

## Using the Image in the Other Projects

The image contains the `libyui-travis` script which runs all the checks and tests.

The workflow is:

- Copy the sources into the `/usr/src/app` directory.
- If the code needs additional packages install them using the `zypper install`
  command from the local `Dockerfile`. If the package can be used by more modules
  you can add it into the base Docker image here.
- Run the `libyui-travis` script.

## Examples

### `Dockerfile` example

```Dockerfile
FROM registry.opensuse.org/devel/libraries/libyui/containers/libyui-devel

# optionally install additional packages if needed:
# RUN zypper --non-interactive install --no-recommends \
#  foo-devel \
#  bar-devel

# copy the sources into the image
COPY . /usr/src/app
```

### `.travis.yml` Example

```yaml
sudo: required
language: bash
services:
  - docker

before_install:
  - docker build -t libyui-foo-image .
  # list the installed packages (just for easier debugging)
  - docker run --rm -it libyui-foo-image rpm -qa | sort

script:
  # the "libyui-travis" script is included in the Docker image
  # see https://github.com/libyui/ci-libyui-container/blob/master/package/libyui-travis
  - docker run -it --rm -e TRAVIS=1 -e TRAVIS_JOB_ID="$TRAVIS_JOB_ID" libyui-foo-image libyui-travis
```

(Replace `foo` with your package name to avoid collisions with the other packages
when running the same commands locally.)


## Building the Image Locally

### Using the OSC Tool

From the Git sources:

```sh
# you need the rubygem-libyui-rake package installed
rake osc:build
```

From the OBS checkout:

```sh
# check it out if not already present
osc co devel:libraries:libyui ci-libyui-container
cd devel:libraries:libyui/ci-libyui-container

# build it
osc build containers
```

### Using the Docker tool

️:warning: This approach is not 100% the same as building the image with `osc` described above.
The `osc` build injects some special modifications to allow building the image inside
the OBS build environment.

:information_source:️ You should prefer using the `osc` method if possible, use the `docker`
build only as a fallback when the `osc` build is not possible or does not work in your environment.

```sh
docker build -t ci-libyui-container-test -f package/Dockerfile package/
```
