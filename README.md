* *Name*: Fun-Bin
* *Author*: anakin <anakin@nightprojects.org> | <anak1n@funtoo.org>
* *Description*: Creating an isolated environment for building binary packages for Funtoo.

-----------------------------------------------------------------------------------------------------------

##What does it do exactly?
I, for one, find it more convenient to build the source packages on a build server with `FEATURES="buildpkg"` and then pull the binaries from the build server. It allows for fast installation on target machines, where the host of this program will build all of them. What it does is creates an isolated environment with a chroot, builds a kernel based on your target machine, builds packages and updates once a week via a cronjob. 

##How does it work?
Running `/usr/bin/funbin` will initialize the program, which then the user will input the information needed. Then it will create the proper directories, grab the stage3 of their choice, and create the isolated environment. The user will then need to chroot into their chroot themselves to install the packages they want. This simply creates all needed things for it to work. 

----------------------------------------------------------------------------------------------------------- 

Host machine will be set up with:

    FEATURES="buildpkg"
    EMERGE_DEFAULT_OPTS="--quiet-build=y --autounmask=n"

Target machine should have:

    PORTAGE_BINHOST="URL.com/of/packages"
    EMERGE_DEFAULT_OPTS="-g"

Then simply the user needs to run:

    # emerge sys-foo/bar
    [binary   N    ] sys-foo/bar-1.0.0-r1  USE="foo -bar"

##NOTE:
The `package.use` file of your `PORTAGE_BINHOST` needs to match the package.use of your target machine, else it will not pull in the proper binary. Any flags you set on the `PORTAGE_BINHOST` need to be set in the target machine! You also need to set the same `ARCHITECTURE` in the `make.conf`s, this can easily be done by using the SAME stage3's for your installs. Example: if your target machine is using a `core-i7 stage3`, you need to build the `PORTAGE_BINHOST` with a `core-i7 stage3`. If you're using a `generic-64 stage3`, you need your `PORTAGE_BINHOST` to have a `generic-64 stage3`, and so on. 


