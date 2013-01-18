---
layout: post
title: "Installing Vim on CentOS 6.3"
date: 2013-01-18 06:11
comments: true
categories: 
---

CentOS, even at version 6.3, comes with a very old version of vim: 7.2.411. Whilst this maybe sufficient for most every day tasks, when you carry your $VIMRUNTIME around with you like I do [1], it's irritating to call a familiar mapping only to have something like an E117 error thrown because your vim version is only 7.2.  Vim 7.3 added some great new stuff. There's no point in not using it.

So, first of all, we need to grab the sources from Mercurial. Vim is stored in the Google code repos & to get it we need have have Mercurial installed. If you haven't got it already, you can use yum to install it:

`$ sudo yum install mercurial`

After that cd to your build directory (in my case ~/Downloads/build) & run this in your terminal:

` $ hg clone https://vim.googlecode.com/hg/ vim`

Now we need to install our toolbox:

` $ sudo yum groupinstall 'Development Tools'

This will give us gcc, bison, flex plus a lot of other stuff we need to build vim. I always build vim +huge +perl +ruby +python. CentOS 6.3 comes with perl 5.10 & python 2.6.6 already installed. So we need to install ruby:

` $ sudo yum install ruby`

This will install ruby-1.8.7-p362 which is itself pretty old by now & not recommended for production use anymore in something like Ruby on Rails but for our purposes, it'll do fine.

Now we need the header files for the build so we run this command:

` $ sudo yum install perl-devel python-devel ruby-devel`

which will give us the requisite header files for our build.

Two more dependencies are needed or configure will throw an error & if you proceed, make will fail. These are:

` $ sudo yum install perl-ExtUtils-Embed ncurses-devel`

So now we're ready to go. Cd to the build directory (in my case ~/Downloads/build/vim) & run configure:

` $ ./configure --with-features=huge --enable-perlinterp --enable-rubyinterp --enable-pythoninterp`

Let configure do its thing & then go back through stdout & check all was OK. If you've got a standard install of everything via yum, configure should've found everything without too much trouble.

Now run `make`. After a short while, if all's well & no errors are written to stdout by make, we're ready to install. The default target for the install is /usr/local (if you don't have root on the box, pass the relevant flag to configure before you start to direct to your $HOME/) so we issue this command:

` $ sudo make install`

So finally call:

` $ type vim`

& it should return:

` $ vim is hashed (/usr/local/bin/vim)`

At the time of writing, passing ` $ vim --version` gives me `vim 7.3.762`

which is a hell of a lot more useful than 7.2.411...
