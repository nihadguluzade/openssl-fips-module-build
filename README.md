OpenSSL with FIPS Module Build

## Step 0: Prerequisites

0. Windows 7 Recommended with 32 Bit
1. Programming languages\Visual C++\Common Tools for Visual C++ 2015; You may download Visual C++ 2015/2017 Build Tools from here http://landinghub.visualstudio.com/visual-cpp-build-tools
2. Open Visual C++ Build Tools
3. Perl should be installed and location shoule be added in PATH system variable like: 'C:\Perl32\bin'. Also you may download pearl from here https://www.activestate.com/activeperl/downloads (if needed)
4. NASM (Netwide Assembler) installed and location should be added to the PATH system variable like: 'C:\Program Files\NASM' [After adding PATH, you may require to reopen your command prompt or reload environment variables]. Also may download from here https://sourceforge.net/projects/nasm/ (if needed)

## Step 1: Build the FIPS Object Module from Source

### Download FIPS Module and Compile:
1. Download openssl-fips-2.0.16.tar.gz from: here https://www.openssl.org/source/
2. Extract/Unzip downloaded file in some directory; like we are creating here `openssl-fips-2.0.16`
3. Open a VC++ or `VS2013 x86 Native Tools Command Prompt` to execute commands
4. Go to into extracted directory, then execute following command in your command prompt

> cd ..\openssl-fips-2.0.16\

..\openssl-fips-2.0.16> Set PROCESSOR_ARCHITECTURE=x86

..\openssl-fips-2.0.16> ms\do_fips


Follow screen instructions and done.

4. After getting a message of `FIPS BUILD SUCCESS`, you may find generated files in below directory:

..\openssl-fips-2.0.16>	 `C:\usr\local\ssl\fips-2.0`

You have successfully generated a build of FLIPS for OpenSSL.

#### Note: 
- You will have following files in C:\usr\local\ssl\fips-2.0\lib directory on build success: `fipscanister.lib`, `fipscanister.lib.sha1` and `fips_premain.c`.


## Step 2: Building a FIPS Capable OpenSSL

### Download OpenSSL and Configure OpenSSL :
1. Download openSSL-1.0.2k.tar.gz from here: https://www.openssl.org/source/old/1.0.2/openssl-1.0.2k.tar.gz
2. Extract/Unzip downloaded file in some directory; like we are creating here `openssl-1.0.2k`
3. Download and Install Cygwin Terminal (just base install)
4. Execute following commands:

> cd ..\openssl-1.0.2k

> perl Configure VC-WIN32 fips --with-fipsdir=C:\usr\local\ssl\fips-2.0

> ms\do_nasm

> nmake -f ms\ntdll.mak

Can open 'out32dll' directory to verify and see all required dll files

#### Note: In `--with-­fipsdir` you need to enter a path of compiled fips directory, where you have saved your build in Step 1, 4th point.


#### HOW TO CHECK OPENSSL VERSION IN COMPILED BUILD:

> cd out32dll

> openssl version

#### HOW TO TEST OPENSSL BUILD:

> cd out32dll

> notepad test.txt   (write something inside and save like: test)

> openssl md5 test.txt  (without OPENSSL_FIPS) flag

You will get a hash value in return

Now add flag for OPENSSL_FIPS to verify

> set OPENSSL_FIPS=1

> openssl md5 test.txt  (without OPENSSL_FIPS) flag

In return you will get error as openssl fips wont allow you to use md5 as FIPs wont allow you md5
