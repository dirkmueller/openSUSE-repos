# openSUSE-repos

**Definitions for openSUSE repository management via zypp-services.**

This feature was originally requested as part of https://code.opensuse.org/leap/features/issue/91

See official [docs](https://doc.opensuse.org/projects/libzypp/HEAD/zypp-services.html#services-usecase-4) for zypp-services for more details.


## Example manual usage of zypper as
```
$ tree /somewhere # zypp expects repo/repoindex.xml
/somewhere
└── repo
    └── repoindex.xml

$ zypper addservice /somewhere openSUSE # Use openSUSE prefix for all reposistories managed by service
$ zypper ref -s # optionally force refresh services

Repositories managed by zypp-services can be easily identified as they will have openSUSE: prefix (or any other that you have chosen).
```


## Restoring original distribution repositories
openSUSE-repos does backup of all existing  default distribution repo files under /etc/zypp/repos.d/*.rpmsave

As of today uninstalling openSUSE-repos **will not** restore original distribution repo files.
You can restore original repo files by running following as root.
Note: You should not use rpmconf, as the original file was simply moved under a new name.

```
# zypper remove openSUSE-repos-*

# ls -la /etc/zypp/repos.d/*.rpmsave # review list of repos that will be restored
# for file in /etc/zypp/repos.d/*.rpmsave; do echo mv $file `echo $file | sed -s "s/\.rpmsave//"`; done
# zypper ref
```


## How to contribute?

Package is developed in [GitHub/openSUSE](https://github.com/openSUSE/openSUSE-repos/).

Package needs to be manually updated in [OBS](https://build.opensuse.org/package/show/Base:System/openSUSE-repos) once changes are merged in GitHub.

Make sure to install osc and required obs services by openSUSE-repos package

```
$ sudo zypper in openSUSE-release-tools obs-service-tar
```

Fork the repository in OBS, fetch latest request and make a submit request.

```
$ osc bco Base:System/openSUSE-repos
cd home:i*:branches:Base:System/openSUSE-repos
osc service runall
osc addremove
osc commit # changelog can be reviewed by osc vc
osc sr # submit request back to Base:System
```

Don't forget to send changes back to Tumbleweed and Leap once changes are merged to Base:System.

```
$ osc sr Base:System openSUSE-repos openSUSE:Factory
$ osc sr openSUSE:Factory openSUSE-repos openSUSE:Leap:15.6 # once merged to Factory
```

That's all. Happy Hacking
