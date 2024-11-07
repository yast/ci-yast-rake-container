# The YaST rake tasks container image

[![CI](https://github.com/yast/ci-yast-rake-container/actions/workflows/ci.yml/badge.svg?branch=master)](https://github.com/yast/ci-yast-rake-container/actions/workflows/ci.yml)
[![OBS](https://github.com/yast/ci-yast-rake-container/actions/workflows/submit.yml/badge.svg)](https://github.com/yast/ci-yast-rake-container/actions/workflows/submit.yml)

This git repository defines a Docker image used in GitHub actions for submitting
the packages automatically to [OBS](https://build.opensuse.org).

The resulting docker image is available at https://registry.opensuse.org/.

## Automatic rebuilds

- The image is rebuilt whenever a commit it pushed to the `master` branch.
- The [submit.yml](./.github/workflows/submit.yml) GitHub Action commits the
  configuration to the [YaST:Head/ci-yast-rake-container](
  https://build.opensuse.org/package/show/YaST:Head/ci-yast-rake-container)
- The OBS tracks the dependencies and rebuilds the image if any dependant
  package is updated.

:warning: *Important note: For submitting the image to OBS it uses it's own
image. That means if for some reason the image gets broken then the
autosubmission will not work because of the chicken-egg problem. In that case
submit the image manually using the `rake osc:commit` command.*

## Triggering a rebuild manually

If for some reason the automatic rebuild in OBS do not work or it failed you can
trigger the rebuild just manually, just like with regular RPM packages.

## The image content

This image is based on the latest openSUSE Tumbleweed, additionally it contains
the [yast-rake](https://github.com/yast/yast-rake) Ruby gem package and other
tools needed for running the `rake osc:sr` or `rake osc:commit`.

## Building the image locally

### Using the OSC tool

From the Git sources:

```sh
# you need the rubygem-yast-rake package installed
rake osc:build
```

From the OBS checkout:

```sh
# check it out if not already present
osc co YaST:Head/ci-yast-rake-container
cd YaST:Head/ci-yast-rake-container

# build it
osc build containers
```

### Using the Docker tool

️:warning: This approach is not 100% the same as building the image with `osc`
described above. The `osc` build injects some special modifications to allow
building the image inside the OBS build environment.

:information_source:️ You should prefer using the `osc` method if possible, use
the `docker` build only as a fallback when the `osc` build is not possible or
does not work in your environment.

```sh
docker build -t ci-yast-rake-container-test -f package/Dockerfile package/
```
