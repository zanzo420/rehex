# Debian Packaging Notes

## Building snapshot versions

I use git-buildpackage and sbuild:

    gbp buildpackage \
        --git-upstream-tree=<SHA> \
        --git-ignore-branch \
        --git-builder="sbuild --dist=stretch --arch=amd64" \
        --git-force-create \
        --git-export-dir=debian/build/

Any changes must be committed before running. Resulting .deb packages and sbuild logs will be in `debian/build/`.

## Building release versions

TODO.

Probably want to use git-import-orig with actual dist tarballs and pristine-tar.

## Version Numbers

Releases:

    x.y.z-0~debian9
           ~debian10
           ~ubuntu1810
           ~ubuntu1904
           ~...

The tilde version on the Debian revision is so that an official (if one ever exists) package for the same version will be considered newer than ours and be preferred. The numeric distribution version ensures the correct version will be installed during a distribution upgrade.

The Debian revision is ZERO rather than ONE because Ubuntu use versions like x.y.z-0ubuntu1 where they haven't forked a Debian package, it remains zero elsewhere for consistency.

Alpha/Beta/Release Candidate:

    x.y.z~a1-0~debian9
    x.y.z~b1-0~debian9
    x.y.z~rc1-0~debian9

Debian revision follows same logic as above. Alpha (a) is older than Beta (b), which is older than Release Candidate (rc). Tilde versions are used again so that the final release will be considered newer than all pre-release versions.

Snapshots:

    x.y.z+gitSHA-0~debian9

Debian revision follows same logic as above. The (short) Git SHA is added to end of the last non-shapshot version number so the next non-shapshot release (including pre-releases) will be considered updates. These are currently not intended to make it into a Debian repository, so their relative ordering doesn't matter and we don't need to include a timestamp in the version too.
