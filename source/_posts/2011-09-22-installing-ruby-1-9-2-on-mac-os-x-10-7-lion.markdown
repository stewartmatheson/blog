--- 
layout: post
title: Installing Ruby 1.9.2 on Mac os x 10.7 (Lion)
tags: 
- 1.9.2
- Coding
- compile
- iconv
- lion
- mac
- mysql
- openssl
- osx
- rails
- readline
- ruby
- rvm
- Tutorial
status: publish
type: post
published: true
meta: 
  aktt_tweeted: "1"
  _oembed_8038a2b2ca4c525cb6c0cda21f78bf1d: "{{unknown}}"
  _oembed_212cd86b1cb7cdd1605fdcdd09701c97: "{{unknown}}"
  _oembed_67a6c15d8d774ef8aff92a4055d24a7b: "{{unknown}}"
  _oembed_a0e9617e31126fd767839460bb1ba6e2: "{{unknown}}"
  _oembed_f169673aacf375cafbfa657c388ccbc8: "{{unknown}}"
  _oembed_7279ce3994fd8143c8dac0245a0664e0: "{{unknown}}"
  _oembed_6bd683b19068d9244d859034803fde49: "{{unknown}}"
  _oembed_695a6cc709244c532ee1a41d7e47acf0: "{{unknown}}"
  aktt_notify_twitter: "yes"
  _aktt_hash_meta: ""
  _edit_last: "1"
---
The other day I updated to Lion from Leopard. I know... I was keeping it real with an old OS but it got to the point where new apps where not working with leopard. While everything with the operating system update was fine there was a lot of time spent getting my ruby on rails install back up and running. One thing I did not understand is that I had made the leap to a 64bit system. This means a lot of recompiling as I hate package managers for osx.

### Reinstalling MySQL

First things first you will need to recompile MySQL from source. I am using 5.1. It seems to be the version of mysql that everyone is using. Some people might stick with sqlite however as I would never use it in production I would also never use it in development so I stick with MySQL. These commands are taken from  Dan Benjamin's hivelogic blog. Great work Dan! The link to the article is below in the sources section so if you run in to trouble check the article out.

```bash
cd /usr/local/src
curl -O http://mysql.he.net/Downloads/MySQL-5.1/mysql-5.1.33.tar.gz
tar xzvf mysql-5.1.33.tar.gz
cd mysql-5.1.33
CC=gcc CFLAGS="-O3 -fno-omit-frame-pointer" CXX=gcc
CXXFLAGS="-O3 -fno-omit-frame-pointer -felide-constructors
-fno-exceptions -fno-rtti"
./configure --prefix=/usr/local/mysql
--with-extra-charsets=complex --enable-thread-safe-client
--enable-local-infile --enable-shared --with-plugins=innobase
make
sudo make install
```

### Removing the old Ruby

After these steps you should have the database recompiled for the new architecture of Lion. Great lets move on. Its worth noting that I am using RVM. The first thing I needed to do after the update is remove my existing version of ruby. This is the 32bit version and will not work.

```bash
rvm remove 1.9.2
```

### Reinstalling dependencies

With that done we need to reinstall ruby 1.9.2. We can use RVM to do this however there are a few things we need to update before this. Lets start with libxml. Download the latest version and compile it.

```bash
tar xzvf libxml2-2.7.3.tar.gz 
cd libxml2-2.7.3
./configure --with-python=/System/Library/Frameworks/Python.framework/Versions/2.3/
make
sudo make install
```

After this we need to install libxlst.

```bash
cd /usr/local/src
curl -O ftp://xmlsoft.org/libxslt/libxslt-1.1.20.tar.gz
cd libxslt-1.1.20
./configure
make
sudo make install
```

All done now lets move on to readline.

<h3>Installing Readline</h3>

At the time of this writing I was unable to install readline via rvm packages. There is a patch that fixes this however I am not sure its in head yet. The catch here is that in the current state when you install readline it will say that the install has been successful. This is not true. There is <a href="http://groups.google.com/group/rubyversionmanager/browse_thread/thread/4130f40a654242a6?hl=en">a topic on the RVM mailing list</a> about this. For the moment I was able to get around this by compiling and installing readline myself. I have no doubt that this will be fixed soon but for the moment you will need to take this step. This last step is done assuming that you have readline 5.2 downloaded and untared. If you need help then I can add the download link.

```bash
export MACOSX_DEPLOYMENT_TARGET=10.7
export CFLAGS="-arch x86_64 -g -Os -pipe -no-cpp-precomp"
export CCFLAGS="-arch x86_64 -g -Os -pipe"
export CXXFLAGS="-arch x86_64 -g -Os -pipe"
export LDFLAGS="-arch x86_64 -bind_at_load"
./configure --prefix=/usr/local
cd shlib
sed -e 's/-dynamic/-dynamiclib/' Makefile > Makefile.good
mv Makefile.good Makefile
cd ..
make && sudo make install
```

Next lets install zlib and iconv and openssh via RVM.

```bash
rvm pkg install zlib
rvm pkg install iconv
rvm pkg install openssl
```

<h3>Installing Ruby</h3>
Now we can install ruby 1.9.2. This will install the 64 bit version for us.

```bash
rvm install 1.9.2-head -C --enable-shared,--with-readline-dir=/usr/local --with-iconv-dir=$rvm_path/usr  --with-zlib-dir=$rvm_path/usr --with-openssl-dir=$HOME/.rvm/usr,--build=x86_64-apple-darwin10
```

<h3>Finishing Up</h3>
Once you have recompiled ruby you will need to create a new gem set. For me I have a gem set for each one of my projects so it was a matter of creating the gem set again and then running a bundle install.

<h3>Sources</h3>
http://www.markhneedham.com/blog/2010/07/08/installing-ruby-1-9-2-with-rvm-on-snow-leopard/

http://stackoverflow.com/questions/6317980/bundle-rake-error

http://exceptionz.wordpress.com/2010/02/03/how-to-fix-the-iconv-require-error-in-ruby-1-9/

http://groups.google.com/group/rubyversionmanager/browse_thread/thread/4130f40a654242a6?hl=en

http://techdebug.com/blog/2009/01/03/compiling-readline-on-an-osx-105-intel-x86_64

https://rvm.beginrescueend.com/packages/zlib/

http://nokogiri.org/tutorials/installing_nokogiri.html

```
http://hivelogic.com/articles/installing-mysql-on-mac-os-x/
