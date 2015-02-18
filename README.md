dockerfile-tiny-nodejs
======================
tiny instantiation of nodejs , supervisord using buildroot linux image

building the root image from scratch
------------------------------------
if you want to build the root image from scratch, follow the instructions in the buildimage.sh

or do the following

```
#checkout this repo
git clone https://github.com/lexlapax/dockerfile-tiny-nodejs
cd dockerfile-tiny-nodejs
chmod +x buildimage.sh
./buildimage.sh
```


### buildroot config
after you download the buildroot source tree, and run "make menuconfig"

make sure to choose the following:
> Target options -> Target Architecture -> x86_64

> Target options -> Target Architecture Variant -> generic (already selected)

> Toolchain -> Enable large file (files > 2 GB) support

> Toolchain -> Enable IPv6 support

> Toolchain -> Enable WCHAR support                                                              

> Toolchain -> Enable C++ support                                                                 

> Target packages -> Interpreter languages and scripting -> python

> Target packages -> Interpreter languages and scripting -> external python modules -> python-versiontools

> Target packages -> Interpreter languages and scripting -> external python modules -> python-setuptools


### make docker container a little usable
#### install sshd
> Target packages -> Networking applications -> openssh

#### install supervisord
> Target packages -> System tools -> supervisor

### nodejs specific config
> Target packages -> Interpreter languages and scripting -> nodejs

> Target packages -> Interpreter languages and scripting -> Module Selection ---> (under nodejs)

> Target packages -> Interpreter languages and scripting -> Module Selection -> NPM for the target                                    

> Target packages -> Interpreter languages and scripting -> Module Selection -> Express web application framework

> Target packages -> Interpreter languages and scripting -> Module Selection -> CoffeeScript

> Target packages -> Interpreter languages and scripting -> Module Selection -> Additional modules -- add "mime minimatch"


### post image
> mkdir -p output/images/fixup/sbin output/images/fixup/etc 

> touch output/images/fixup/sbin/init output/images/fixup/etc/resolv.conf

> cd output/images

> cp rootfs.tar fixup.tar

> tar rvf fixup.tar -C fixup .

> and then build the docker image from the fixup tar file
