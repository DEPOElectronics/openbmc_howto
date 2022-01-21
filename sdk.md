# Создать SDK проекта

Building the OpenBMC SDK

Looking for a way to compile your programs for 'ARM' but you happen to be running on a 'PPC' or 'x86' system? You can build the sdk receive a fakeroot environment.

$ bitbake -c populate_sdk obmc-phosphor-image
$ ./tmp/deploy/sdk/openbmc-phosphor-glibc-x86_64-obmc-phosphor-image-armv5e-toolchain-2.1.sh

Follow the prompts. After it has been installed the default to setup your env will be similar to this command

. /opt/openbmc-phosphor/2.1/environment-setup-armv5e-openbmc-linux-gnueabi
