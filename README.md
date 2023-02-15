# CSI Driver for Dell EMC PowerMax
[![Go Report Card](https://goreportcard.com/badge/github.com/dell/csi-powermax?style=flat-square)](https://goreportcard.com/report/github.com/dell/csi-powermax)
[![License](https://img.shields.io/github/license/dell/csi-powermax?style=flat-square&color=blue&label=License)](https://github.com/dell/csi-powermax/blob/master/LICENSE)
[![Docker](https://img.shields.io/docker/pulls/dellemc/csi-powermax.svg?logo=docker&style=flat-square&label=Pulls)](https://hub.docker.com/r/dellemc/csi-powermax)
[![Last Release](https://img.shields.io/github/v/release/dell/csi-powermax?label=Latest&style=flat-square&logo=go)](https://github.com/dell/csi-powermax/releases)

**Repository for CSI Driver for Dell EMC PowerMax**

## Description
CSI Driver for PowerMax is part of the [CSM (Container Storage Modules)](https://github.com/dell/csm) open-source suite of Kubernetes storage enablers for Dell EMC products. CSI Driver for PowerMax is a Container Storage Interface (CSI) driver that provides support for provisioning persistent storage using Dell EMC PowerMax storage array. 

It supports CSI specification version 1.3.

This project may be compiled as a stand-alone binary using Golang that, when run, provides a valid CSI endpoint. It also can be used as a precompiled container image.

## Support
For any CSI driver issues, questions or feedback, please follow our [support process](https://github.com/dell/csm/blob/main/docs/SUPPORT.md)

## Building
This project is a Go module (see golang.org Module information for explanation). 
The dependencies for this project are in the go.mod file.

To build the source, execute `make clean build`.

To run unit tests, execute `make unit-test`.

To build an image, execute `make docker`.

You can run an integration test on a Linux system by populating the file `env.sh` with values for your Dell EMC PowerMax systems and then run "`make integration-test`".

## Runtime Dependencies
Both the Controller and the Node portions of the driver can only be run on nodes which have network connectivity to a “`Unisphere for PowerMax`” server (which is used by the driver). 

If you are using ISCSI, then the Node portion of the driver can only be run on nodes that have the iscsi-initiator-utils package installed.

## Driver Installation
Please consult the [Installation Guide](https://dell.github.io/csm-docs/docs/csidriver/installation)

## Using driver
Please refer to the section `Testing Drivers` in the [Documentation](https://dell.github.io/csm-docs/docs/csidriver/installation/test/) for more info.

## Documentation
For more detailed information on the driver, please refer to [Container Storage Modules documentation](https://dell.github.io/csm-docs/).

## Releasing ppc64le Image
Until Dell/EMC agrees to [add support for ppc64le](https://github.com/dell/csi-powermax/pull/24) and start hosting ppc64le images in their [DockerHub space](https://hub.docker.com/r/dellemc/csi-powermax), we/IBM will have to keep up with their x86 releases on [quay.io](https://quay.io/organization/powercloud).
Here is how one would do to make that happen;
1. have a ppc64le VM running a recent CentOS and check out a tagged version (eg. `v2.4.0`) from the [upstream](https://github.com/dell/csi-powermax)
1. check out a couple of files to support ppc64le, and build as described in [Building above](#building)
1. if you have access to a real PowerMax-backed environment, try to have a recent (if not the newest) version of OpenShift cluster running, deploy your image there per [instructions](#driver-installation)
1. once the driver pods are in `Running` state (and no excessive/repeated crashes), test the driver per [instructinos](#using-driver)
1. if all looks good, tag your local tested image as `quay.io/powercloud/dellemc-csi-powermax:v2.4.0` (change the version as appropriate) and push it with login with proper access to the org

Console output example from above steps;
```
# cat /etc/redhat-release
CentOS Stream release 8


# git status -s | grep -v ^??
 M Dockerfile
 M Makefile


# git diff Dockerfile
diff --git a/Dockerfile b/Dockerfile
index bc3ba27..9747021 100644
--- a/Dockerfile
+++ b/Dockerfile
@@ -1,5 +1,5 @@
 # Dockerfile to build PowerMax CSI Driver
-FROM docker.io/centos:centos8.3.2011
+FROM docker.io/ppc64le/centos:8

 # dependencies, following by cleaning the cache
 RUN yum install -y \


# git diff Makefile
diff --git a/Makefile b/Makefile
index e5d6ee4..13b7b57 100644
--- a/Makefile
+++ b/Makefile
@@ -26,7 +26,7 @@ clean:

 build: golint check
        go generate
-       CGO_ENABLED=0 GOOS=linux GO111MODULE=on go build
+       CGO_ENABLED=0 GOOS=linux GO111MODULE=on GOARCH=$(XGOARCH) go build

 install:
        go generate


# export XGOARCH=ppc64le

# make clean build
which: no golint in (/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/root/bin:/root/bin)
rm -f core/core_generated.go
go clean
go: downloading github.com/dell/gobrick v1.5.1
go: downloading github.com/dell/gofsutil v1.10.0
go: downloading github.com/dell/goiscsi v1.5.0
go: downloading github.com/dell/gopowermax/v2 v2.0.0
go: downloading github.com/sirupsen/logrus v1.9.0
go: downloading github.com/dell/gonvme v1.2.0
go: downloading golang.org/x/sync v0.0.0-20220819030929-7fc1605a5dde
go: downloading golang.org/x/sys v0.0.0-20220825204002-c680a09ffe64
go: creating new go.mod: module tmp
go: downloading golang.org/x/tools v0.6.0
go: added golang.org/x/lint v0.0.0-20210508222113-6edffad5e616
go: added golang.org/x/tools v0.6.0
GOLINT=/root/go/bin/golint bash check.sh --all
=== Checking format...
=== Finished
=== Vetting csi-powermax
=== Finished
=== Vetting csireverseproxy
go: downloading github.com/dell/gopowermax/v2 v2.0.0-20220801063136-61bb0123111e
=== Linting...
=== Finished
go generate
CGO_ENABLED=0 GOOS=linux GO111MODULE=on GOARCH=ppc64le go build


# make unit-test 2>&1 >unit-test.out
which: no golint in (/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/root/bin:/root/bin)
go: creating new go.mod: module tmp
go: added golang.org/x/lint v0.0.0-20210508222113-6edffad5e616
go: added golang.org/x/tools v0.6.0


# make docker
which: no golint in (/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/root/bin:/root/bin)
go generate
go run core/semver/semver.go -f mk >semver.mk
make -f docker.mk docker
make[1]: Entering directory '/root/csi-powermax.upstream'
docker build -t ""localhost:5000/csi-powermax":"v2.4.0.000X"" .
Emulate Docker CLI using podman. Create /etc/containers/nodocker to quiet msg.
STEP 1/7: FROM docker.io/ppc64le/centos:8
STEP 2/7: RUN yum install -y     e2fsprogs     which     xfsprogs     device-mapper-multipath     &&     yum clean all     &&     rm -rf /var/cache/run
--> Using cache 0328fd45b9725af496e14bc639904077c70a7b0790d20dd08020f8384150581f
--> 0328fd45b97
STEP 3/7: RUN which mkfs.ext4
--> Using cache 3e2f40aac788b84b62f7e88b7f0058bde38c8f1a512dbaaee52b721374926592
--> 3e2f40aac78
STEP 4/7: RUN which mkfs.xfs
--> Using cache 1923b9c6ce0a291c0ba8cfdaac6222d19db689bfd0387ecc6b58a74bb7f42459
--> 1923b9c6ce0
STEP 5/7: COPY "csi-powermax" .
--> db14eca0328
STEP 6/7: COPY "csi-powermax.sh" .
--> aa53a6c1082
STEP 7/7: ENTRYPOINT ["/csi-powermax.sh"]
COMMIT localhost:5000/csi-powermax:v2.4.0.000X
--> 8accbc15c29
Successfully tagged localhost:5000/csi-powermax:v2.4.0.000X
8accbc15c29aa829993cc9126e59e8fb0cb9de9fc39076b7a659bce65818a577
make[1]: Leaving directory '/root/csi-powermax.upstream'


# podman tag localhost:5000/csi-powermax:v2.4.0.000X quay.io/powercloud/dellemc-csi-powermax:v2.4.0


# podman login quay.io
Username: miyamoto_h
Password:
Login Succeeded!


# podman push quay.io/powercloud/dellemc-csi-powermax:v2.4.0
Getting image source signatures
Copying blob 031fcf8538c5 done
Copying blob 5f70bf18a086 skipped: already exists
Copying blob 5f70bf18a086 skipped: already exists
Copying blob d6a68fbe48bb skipped: already exists
Copying blob 7763250c4c07 skipped: already exists
Copying blob 76f869468176 skipped: already exists
Copying config 8accbc15c2 done
Writing manifest to image destination
Storing signatures


#
```