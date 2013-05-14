# Turnkey Linux standard Python library

This project is forked from Turnkey Linux. They have automatic
packaging system for generating the missing files to build Debian
package, which is much more scalable. We will need to investigate it
in the future. But until then, this repository is here to store the
missing files to generate packages specifically for Ubuntu 12.04
(Precise Pangolin.)

The following describe the instructions to build and upload packages
to PPA for Ubuntu. For detailed steps, please refer to
[Ubuntu Packaging Guide][] and [Packaging][].

[Ubuntu Packaging Guide]: http://developer.ubuntu.com/packaging/html/
[Packaging]: https://help.launchpad.net/Packaging

## Prerequisite

    $ sudo apt-get install packaging-dev
    $ pbuilder-dist precise create

    $ mkdir ~/build
    $ cd ~/build
    $ git clone https://github.com/Avamagic/turnkey-pylib.git

    $ cd turnkey-pylib
    $ git archive --format=tar master > ../turnkey-pylib_0.3.orig.tar
    $ tar --delete -f ../turnkey-pylib_0.3.orig.tar 'debian'
    $ bzip2 ../turnkey-pylib_0.3.orig.tar

**Note:** replace `0.3` with appropriate version number.

## Build deb file locally without signing

    $ cd turnkey-pylib
    $ dpkg-buildpackage -us -uc

`deb` file will be located under the parent directory. `deb` file can
be installed to check if the source code is correct.

## Build package locally without signing

    $ debuild -S -I -uc -us

Package related files will also be located under the parent
directory. Instead of creating the `deb` file, this will generate
other related files and check the generated files as well.

## Build package in a clean environment

    $ pbuilder-dist precise build ../turnkey-pylib_0.3-0avamagic2.dsc

**Note:** replace `0.3-0avamagic2` with the appropriate version number.

This simulates how the package will be generated on the PPA build
system.

## Build package locally with signing

    $ debuild -S -I -kB5ED3CD3 -sa

**Note:** replace `B5ED3CD3` with your own GPG key.

## Upload package to PPA

Must run the previous step (signing) before uploading the package.

    $ dput ppa:ronhuang/avamagic ../turnkey-pylib_0.3-0avamagic2_source.changes

**Note:** replace `ppa:ronhuang/avamagic` with your own PPA.

**Note:** replace `0.3-0avamagic2` with the appropriate version
  number.
