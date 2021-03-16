# The Libyui Testing Image

[![Workflow Status](https://github.com/libyui/ci-libyui-container/workflows/CI/badge.svg?branch=master)](
https://github.com/libyui/ci-libyui-container/actions?query=branch%3Amaster)

This git repository contains the configuration used to build the docker
image used for CI tests in [Libyui GitHub Actions](https://github.com/libyui/libyui/actions).
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
Use the [`ci-test`](ci-test) script for building.
