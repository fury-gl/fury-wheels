# FURY wheels

This repository automates [FURY](https://github.com/fury-gl/fury) wheel building using [multibuild](https://github.com/matthew-brett/multibuild), [Travis CI](https://travis-ci.com/fury-gl/fury-wheels)

[![Build Status](https://travis-ci.com/fury-gl/fury-wheels.svg?branch=master)](https://travis-ci.com/fury-gl/fury-wheels)


How it works
============

The wheel-building repository:

-   does a fresh build of any required C / C++ libraries;
-   builds a fury wheel, linking against these fresh builds;
-   processes the wheel using
    [delocate](https://pypi.python.org/pypi/delocate) (OSX) or
    [auditwheel](https://pypi.python.org/pypi/auditwheel) `repair`
    ([Manylinux1](https://www.python.org/dev/peps/pep-0513)). `delocate`
    and `auditwheel` copy the required dynamic libraries into the wheel
    and relinks the extension modules against the copied libraries;
-   uploads the built wheels to a Rackspace container - see "Using the
    repository" above. The containers were kindly donated by Rackspace
    to scikit-learn).

The resulting wheels are therefore self-contained and do not need any
external dynamic libraries apart from those provided as standard as
defined by the manylinux1 standard.



Triggering a build
==================

You will likely want to edit the `.travis.yml` and `appveyor.yml` files
to specify the `BUILD_COMMIT` before triggering a build - see below.

You will need write permission to the github repository to trigger new
builds on the travis-ci interface. Contact us on the mailing list if you
need this.

You can trigger a build by:

-   making a commit to the `fury-wheels` repository (e.g. with
    `git commit --allow-empty`); or
-   clicking on the circular arrow icon towards the top right of the
    travis-ci page, to rerun the previous build.

In general, it is better to trigger a build with a commit, because this
makes a new set of build products and logs, keeping the old ones for
reference. Keeping the old build logs helps us keep track of previous
problems and successful builds.


Which fury version does the repository build?
============================================

The `fury-wheels` repository will build the commit specified in the
`BUILD_COMMIT` at the top of the `.travis.yml` and `appveyor.yml` files.
This can be any naming of a commit, including branch name, tag name or
commit hash.

NB: The directory **fury_sources** must be updated to the latest repository version in order to have all the commits.

PyPI
====
Download wheels from bintray.com

```
./download_wheels.py $BUILD_COMMIT
```
where `BUILD_COMMIT` can be anything like `v0.1.1`.
This script will download the wheels in to the `tmp/` folder.

Upload wheels to PyPI

```
twine upload tmp/*.whl
```

For the twine access user and password ask [Serge Koudoro](mailto:skab12@gmail.com) or [Eleftherios Garyfallidis](mailto:garyfallidis@gmail.com)

