#Siege
=====

Siege is a web load testing tool. It was originally built and tested on GNU/Linux. It has been ported to other platforms. See the MACHINES document for more details. 

This program was built using the GNU autoconf mechanism.  If you are familiar with GNU applications, then siege should present few problems  especially on the above mentioned platforms. For best results, use gcc.

IMPORTANT: If you are upgrading from an earlier version, you MUST delete the older version before installing this one. The simplest way to remove the older version to run "make uninstall" in the old source directory. If you no longer have the old source, you can configure the new version to be installed in the same place as the old version.  Then BEFORE you run "make install", run "make uninstall" first. 


###1. Compiling & Installing
In a nutshell, to install the application in the default directory, ( /usr/local ), run the following commands:

   $ ./configure (IMPORTANT: see step 2 for enabling https support)
   $ make
   $ make uninstall (if you have an older version installed in PREFIX)
   $ make install

   This will install the application ( siege ) in the default directory
   /usr/local/bin.  If that directory is in your PATH, then to run siege
   and view the online help type:
   $ siege --help 
	
   To learn more about siege, make sure /usr/local/man is in your MANPATH
   and type:
   $ man siege

   For more detailed information about running siege and stress testing
   HTTP servers, type:
   $ man layingsiege 

   For more details, read on. Especially if you want to install siege
   in a directory other that /usr/local/bin  

###2. Configuration
The configure script attempts to guess the values which are set on your platform.  If all goes well, you should only have to run it with some preferred arguments.  The more notable ones are listed below:

   --help                 prints the configure script's help section
   --prefix=/some/dir     installs the files in /some/dir
   --bindir=/some/bin     installs the executable in /some/bin
   --mandir=/some/man     installs the man page in /some/man
   --with-ssl=/some/dir   where dir is where you installed ssl, this
                          flag is used to enable https protocol.

Since siege is a pretty esoteric program, I prefer to install it in my home directory.  Really, how many people are laying siege to http servers and do you really want them running it by accident?  For this reason, I run configure with my home directory as the prefix.
   
   $ ./configure --prefix=/export/home/jdfulmer

If you don't already, make sure $HOME/bin and $HOME/man are set appropriately in your .profile.  In my case, I set them like this:

   # jdfulmer's profile
   PATH=/export/home/jdfulmer/bin:$PATH
   MANPATH=/export/home/jdfulmer/man:$MANPATH

   export PATH MANPATH
   ~
   ~
   
To reload your profile without logging out, do this:
   
   $ . .profile

If it runs successfully, the configure script creates the Makefiles which lets you build the program.  After you configure your environment, the next step is to build siege. If that next step fails, you may have to return to this step.  Reasons for reconfiguring are mentioned below.  If configure failed to create Makefiles, then you have problems which may be beyond the scope of this document, such as no compiler ( you'll have to get one ), no libraries ( again, an acquisition on your part ).

####HTTPS support
To enable https, you must have ssl installed on your system.  Get the latest version from http://www.openssl.org.  AFTER ssl is installed, then you have to configure siege to use it:

   $ ./configure --prefix=/some/dir --with-ssl=/ssl/install/dir

The openssl default installation is /usr/local/ssl.  So if you configured openssl with the default directory, then you would configure siege like this:

   $ ./configure --prefix=/some/dir --with-ssl=/usr/local/ssl
   $ make
   $ make uninstall ( if you have a previous version already installed )
   $ make install

###3. Compilation
To compile the program, execute the second step of the nutshell version mentioned in item #1: type "make" and hope for the best.  If your environment was configured without errors, then configure should have generated the Makefiles that will enable this step to work.

The make command will invoke your compiler and build siege.  If you are using gcc on any of the platforms mentioned above, then you should not have problems. In general, any ANSI C compiler should work.  Siege does not currently support K&R compilers and older versions of the operating systems mentioned in MACHINES.

Some systems may require options that were not set by the configure script. You can set them using the configure step mentioned above:
   
   $ CC=c89 CFLAGS=-O2 LIBS=-lposix ./configure 

You can also set them by editing the Makefiles that were created as a result of running configure, but this is not preferred. 

###4. Installation
If the program compiled successfully, follow the third nutshell step and type "make install"  This will install the package in the directories that you've selected in the configuration step.  If they are not already, make sure PREFIX/bin and PREFIX/man are in your PATH and MANPATH respectively. This process is described in detail in item #2. 

   Files installed:
   siege          -->    SIEGE_HOME/bin/siege
   bombardment    -->    SIEGE_HOME/bin/bombardment
   siege2csv      -->    SIEGE_HOME/bin/siege2csv
   .siegerc       -->    $HOME/.siegerc
   siege.1        -->    SIEGE_HOME/man/man1/siege.1
   bombardment.1  -->    SIEGE_HOME/man/man1/bombardment.1
   siege2csv.1    -->    SIEGE_HOME/man/man1/siege2csv.1
   layingsiege.1  -->    SIEGE_HOME/man/man1/layingsiege.1
   urls_text.1    -->    SIEGE_HOME/man/man1/urls_txt.1
   urls.txt       -->    SIEGE_HOME/etc/urls.txt 

###5. Uninstall
To remove the package, type "make uninstall"  To make the source directory completely clean, type "make distclean".  There are differences of opinion regarding this option.  Some people claim that it should not be available as it depends the original Makefiles from the source directory.  Since I tend to hoard all source code, I like this feature.

The point is, if you've installed one version of siege in /usr/local and another version in $HOME, then make uninstall is obviously not going to work in both locations.  The safest thing to do is manually remove the files which were installed by make install.  The files and their locations are described in item #4.

###6. Read the documentation
The online help is pretty straight forward ( siege --help ):

   Usage: siege [options]
   Options:
    -V, --version        VERSION, prints version number to screen.
    -h, --help           HELP, prints this section.
    -v, --verbose        VERBOSE, prints notification to screen.
    -c, --concurrent=NUM CONCURRENT users, default is 10
    -u, --url="URL"      URL, a single user defined URL for stress testing.
    -i, --internet       INTERNET user simulation, hits the URLs randomly.
    -b, --benchmark      BENCHMARK, signifies no delay for time testing.
    -t, --times=NUM      TIMES, number of times to run the test, default is 25
    -t, --time=NUMm      TIME based testing where "m" is the modifier S, M, or H
                         no space between NUM and "m", ex: --time=1H, 1 hour test.
    -f, --file=FILE      FILE, change the configuration file to file.
    -l, --log            LOG, logs the transaction to PREFIX/var/siege.log
    -m, --mark="text"    MARK, mark the log file with a string separator.
    -d, --delay=NUM      Time DELAY, random delay between 1 and num designed
                         to simulate human activity. Default value is 3  

   For more detailed information, consult the man pages:
   $ man siege
   $ man layingsiege
   $ man siege.config

   All the siege man pages are also available online:
   http://www.joedog.org/siege/docs/man/index.html

   OR, read the html manual, doc/manual.html  The manual is also available online:
   http://www.joedog.org/siege/docs/manual.html 

###7. The .siegerc file
Edit the .siegerc file in your home directory.  This file contains runtime directives for siege.  Each directive is well documented with comments. Some directives exist ONLY in this file; they don't have a command line option. If you are upgrading from an earlier version, your original version is kept and a new resource file is installed as .siegerc.new.  In order to take advantage of any new directives, you might want to use this new file instead.

###8. Tell me about it
If you like/dislike siege please let me know.  See name and email below:

Please consult the file, COPYING for complete license information.

Copyright (C)2000-2007 Jeffrey Fulmer <jeff@joedog.org>, et al.

Permission is granted to anyone to make or distribute verbatim
copies of this document as received, in any medium, provided that
the copyright notice and this permission notice are preserved, thus
giving the recipient permission to redistribute in turn.

Permission is granted to distribute modified versions of this
document, or of portions of it, under the above conditions,
provided also that they carry prominent notices stating who last
changed them.
