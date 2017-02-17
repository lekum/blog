+++
subtitle = ""
title = "Introduction to Autotools"
bigimg = ""
date ="2015-07-29T15:58:51+02:00"

+++

Recently I have discovered the [Autotools](https://en.wikipedia.org/wiki/GNU_build_system) suite for compiling/packaging C applications. I have played with it a little bit, following the examples and hints from this [book](http://shop.oreilly.com/product/0636920033677.do), and I have found that it is an incredibly useful toolkit.

I am going to describe an example of how a tiny C command-line application can be packaged and delivered using Autotools. I will also include a dynamic library in order to make it more realistic.

<!-- TEASER_END -->

Let's start with the C code. We have these three files:

`upper.h`
```c
void to_upper(char *buf, const char *original);
```

`upper.c`
```c
#include <upper.h>
#include <ctype.h>

void to_upper(char *buf, const char *original)
{
    const char *p = original;
    char *q = buf;
    while (*p != '\0')
    {
        *q = toupper(*p);
        p++,q++;

    }
    *q = '\0';
}
```

`upper_line.c:`
```c
#include <upper.h>
#include <stdio.h>

int main()
{
    char original_line[81];
    char new_line[81];
    puts("Please enter a line:");
    fgets(original_line, 81, stdin);
    to_upper(new_line, original_line);
    printf("\nUppercase version of the line:\n%s\n", new_line);
    return 0;
}
```

We have created a function `to_upper` that performs an uppercase conversion of a string, based upon the *charwise* conversion of the `toupper` function of `ctype.h`. We have also created a command line utility that prompts the user for a line and then prints its uppercase version. The idea is now automate the compiling, linking and packaging of this application, consisting on a binary (`upper_line`) and a dynamically linked library (`libupper`).

In order to use Autotools, we need some packages installed in our system. For a Ubuntu-based distro, these packages could be: `build-essential`, `autoconf`, `automake` and `libtool`.

First, we need to create a `Makefile.am` file, which is something similar to a Makefile but with a higher level of abstraction. This could be our file:

`Makefile.am`
```makefile
AM_CFLAGS=-g -Wall -O3

include_HEADERS = upper.h

lib_LTLIBRARIES = libupper.la
libupper_la_SOURCES = upper.c

bin_PROGRAMS = upper_line
upper_line_SOURCES = upper_line.c
upper_line_LDADD = libupper.la
```
I will describe its contents:

- The first line is the optional compiler flags.
- The line `include_HEADERS` list the header files that should be included when compiling and finally installed in the destination.
- The line `lib_LTLIBRARIES` list the dynamically linked libraries that should be generated and installed. Their extension will be `.la` as we are going to use [Libtool](http://www.gnu.org/software/libtool/) to create them.
- The line `libupper_la_SOURCES` list the source files needed to generate the target `libupper.la` that we have declared in the previous line.
- The line `bin_PROGRAMS` list the binary files that should be generated and finally installed in the destination.
- The line `upper_line_SOURCES` list the source files needed to generate the target `upper_line` that we have declared in the previous line.
- The line `upper_line_LDADD` list the libraries that should be dinamically linked to generate the target `upper_line`.

Once we have this file, we run `autoscan` in the same folder and it will generate a file named `configure.scan` that has extracted some information of the `Makefile.am` and features some macro invocations:

`configure.scan`
```makefile
#                                               -*- Autoconf -*-
# Process this file with autoconf to produce a configure script.

AC_PREREQ([2.69])
AC_INIT([FULL-PACKAGE-NAME], [VERSION], [BUG-REPORT-ADDRESS])
AC_CONFIG_SRCDIR([upper_line.c])
AC_CONFIG_HEADERS([config.h])

# Checks for programs.
AC_PROG_CC

# Checks for libraries.

# Checks for header files.

# Checks for typedefs, structures, and compiler characteristics.

# Checks for library functions.

AC_CONFIG_FILES([Makefile])
AC_OUTPUT
```

In order to tailor it to our project, we will need to rename it to `configure.ac` and substitute `FULL-PACKAGE-NAME`, `VERSION` and `BUG-REPORT-ADDRESS` for the values that we want. In addition, we will need to call the macros `AM_INIT_AUTOMAKE` (in order to use Automake to create the Makefile) and `LT_INIT` (in order to use Libtool to generate the library).

This could be our `configure.ac` after the substitutions:

`configure.ac`
```makefile
#                                               -*- Autoconf -*-
# Process this file with autoconf to produce a configure script.

AC_PREREQ([2.69])
AC_INIT([upper-line], [1.0], [/dev/null])
AC_CONFIG_SRCDIR([upper_line.c])
AC_CONFIG_HEADERS([config.h])

# Checks for programs.
AM_INIT_AUTOMAKE
LT_INIT
AC_PROG_CC

# Checks for libraries.

# Checks for header files.

# Checks for typedefs, structures, and compiler characteristics.

# Checks for library functions.

AC_CONFIG_FILES([Makefile])
AC_OUTPUT
```
To comply with the GNU Coding standars, we will need to create these files, no matter if they are just empty: `NEWS`, `README`, `AUTHORS`, `ChangeLog`.

Once we have it, whe run `autoreconf -iv`. If everything goes as expected, we will have a `configure` script in the current folder. If we execute `./configure`, we will have a `Makefile`. Finally, if we run `make distcheck`, we will have a package, something like `upper-line-1.0.tar.gz`. This *.tar.gz* file will be the package that should be distributed to the users of the application.

If we deflate the package, we will have the typical source code structure of an application that can be installed with `./configure && make && sudo make install`. If you want to uninstall, just type `sudo make uninstall`. The output of the installation process will show you the folder in which the binaries, headers and libraries are installed. In Ubuntu, you will probably have to add `/usr/local/lib` to the `LD_LIBRARY_PATH` environment variable or add the path to the `/etc/ld.so.conf.d/` and run `ldconfig`.

And if we want to automate the proccess, we could write a bash script that does the entire chain to create the package:

`package.sh`
```bash
#! /bin/bash

version=1.0
package_name=upper-line
package_dist_dir=dist

if [ -e $package_dist_dir ]; then rm -r $package_dist_dir; fi
mkdir -p $package_dist_dir
cp Makefile.am upper.c upper.h upper_line.c $package_dist_dir
cd $package_dist_dir
autoscan
sed -e 's/FULL-PACKAGE-NAME/'$package_name'/' -e 's/VERSION/'$version'/' -e 's|BUG-REPORT-ADDRESS|/dev/null|' \
    -e '10iAM_INIT_AUTOMAKE' \
    -e '10iLT_INIT' \
    < configure.scan > configure.ac
touch NEWS README AUTHORS ChangeLog
autoreconf -iv
./configure
make distcheck
```

If you thought that Makefiles were cool, this is the next level. Autotools is clever enough to make the tedious and error-prone task of packaging of distributions ready to install in any POSIX-compliant platform a piece of cake. Of course, there are many options available that I have not described here, since this is only an introduction and something that I have just learnt today.

Happy hacking!
