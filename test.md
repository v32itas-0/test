
---
author:
  name: Linode Community
  email: docs@linode.com
description: 'Setting up Lineage 2 Java Server. Using Linode Slackware64 14.2 Image'
keywords: ["list", "of", "keywords", "and key phrases"]
license: '[CC BY-ND 4.0](https://creativecommons.org/licenses/by-nd/4.0)'
published: 2017-11-29
modified_by:
  name: Linode
title: 'How to set up L2J Server on Slackware64 14.2'
contributor:
  name: Your Name
  link: Github/Twitter/LinkedIn URL
external_resources:
  - '[L2J Server official site](https://l2jserver.com/)'
  
---


# How to set up L2J Server on Slackware64 14.2

## Introduction
L2J is a free open-source Lineage 2 server emulator fully written in Java released under GPLv3.0 license.

## Before You Begin
You only need a fresh Slackware64 14.2 Linode to begin with this guide.


## Setting up fresh Slackware64 14.2 Linode
{{< note >}}
You begin with a root user
{{< /note >}}
###  Choosing slackpkg mirror
1.	Opening `/etc/slackpkg/mirrors` 




		/usr/bin/nano /etc/slackpkg/mirrors

2. Scroll down to:

		#----------------------------------------------------------------
		# Slackware64-14.2
		#----------------------------------------------------------------
		# USE MIRRORS.SLACKWARE.COM (DO NOT USE FTP - ONLY HTTP FINDS A NEARBY MIRROR)
		# http://mirrors.slackware.com/slackware/slackware64-14.2/

 3. Uncomment the mirror link line by removing the hashtag at the beginning of the line. (Use arrow keys to move the cursor):
 4. 
		#----------------------------------------------------------------
		# Slackware64-14.2
		#----------------------------------------------------------------
		# USE MIRRORS.SLACKWARE.COM (DO NOT USE FTP - ONLY HTTP FINDS A NEARBY MIRROR)
		http://mirrors.slackware.com/slackware/slackware64-14.2/

5. Use `CTRL + O` to write changes to file and click Enter. Then `CTRL + X` to exit.

### Updating and upgrading 

1. Updating GPG keys:
		
		/usr/sbin/slackpkg update gpg
2. Updating package list:

		/usr/sbin/slackpkg update
3. Upgrading all installed packages.

		/usr/sbin/slackpkg upgrade-all
4. When prompted to upgrade `/etc/lilo.conf` it's safe to chose (N)o since Linode uses diffetent kernel image than Slackware default one.
5. When prompted to chose what to do with new config files. Chose (K)eep option.

### Installing slackpkg+

1. Downloading slackpkg+ package:

		/usr/bin/wget https://netcologne.dl.sourceforge.net/project/slackpkgplus/slackpkg%2B-1.7.0-noarch-10mt.txz

2. Installing slackpkg+ package:

		/sbin/installpkg slackpkg+-1.7.0-noarch-10mt.txz

### Enabling alienbob repository for slackpkg+

1. Opening `/etc/slackpkg/slackpkgplus.conf`

		/usr/bin/nano /etc/slackpkg/slackpkgplus.conf

2.  Uncommenting 'alienbob' mirror, by removing a hashtag in the beginning of the line here:

	* Before:
	
			# Slackware 14.2 - x86_64
			#MIRRORPLUS['multilib']=http://bear.alienbase.nl/mirrors/people/alien/multilib/14.2/
			#MIRRORPLUS['alienbob']=http://bear.alienbase.nl/mirrors/people/alien/sbrepos/14.2/x86_64/
			#MIRRORPLUS['restricted']=http://bear.alienbase.nl/mirrors/people/alien/restricted_sbrepos/14.2/x86_64/
			#MIRRORPLUS['slacky']=http://repository.slacky.eu/slackware64-14.2/
			
	* After:
	
			# Slackware 14.2 - x86_64
			#MIRRORPLUS['multilib']=http://bear.alienbase.nl/mirrors/people/alien/multilib/14.2/
			MIRRORPLUS['alienbob']=http://bear.alienbase.nl/mirrors/people/alien/sbrepos/14.2/x86_64/
			#MIRRORPLUS['restricted']=http://bear.alienbase.nl/mirrors/people/alien/restricted_sbrepos/14.2/x86_64/
			#MIRRORPLUS['slacky']=http://repository.slacky.eu/slackware64-14.2/



3. Adding alienbob repository to list of repositories we want to use

	* Before:

			# List repositories you want to use (defined below)
			# remember to launch 'slackpkg update' if you modify that row.
			#REPOPLUS=( slackpkgplus restricted alienbob slacky )
			REPOPLUS=( slackpkgplus )

	* After:

			# List repositories you want to use (defined below)
			# remember to launch 'slackpkg update' if you modify that row.
			#REPOPLUS=( slackpkgplus restricted alienbob slacky )
			REPOPLUS=( slackpkgplus alienbob)

4. Write changes to file `CTRL + O` and hit `ENTER`, Then `CTRL + X` to close the file. The whole file after changes should look like this:

		# Configuration for slackpkg+. See /usr/doc/slackpkg+-*/README for more details                                                                                                                                                                                                                                               
		# Please read manpage: "man slackpkgplus.conf" and documentation /usr/doc/slackpkg+-*/README                                                                                                                                                                                                                                  
		                                                                                                                                                                                                                                                                                                                              
		# Enable (on) / Disable (off) slackpkg+                                                                                                                                                                                                                                                                                       
		SLACKPKGPLUS=on                                                                                                                                                                                                                                                                                                               
		                                                                                                                                                                                                                                                                                                                              
		# set to '0' to never show the download progress bar                                                                                                                                                                                                                                                                          
		# set to '1' to show the bar only in download packages (default)                                                                                                                                                                                                                                                              
		# set to '2' to always show the download bar                                                                                                                                                                                                                                                                                  
		# set to '3' for a debug mode                                                                                                                                                                                                                                                                                                 
		VERBOSE=1                                                                                                                                                                                                                                                                                                                     
		                                                                                                                                                                                                                                                                                                                              
		# By default slackpkg+ deny to install 32bit packages.                                                                                                                                                                                                                                                                        
		#                                                                                                                                                                                                                                                                                                                             
		# Set this flag to 'on' allow slackpkg+ to install 32bit packages on a 64bit slackware                                                                                                                                                                                                                                        
		# installation (possibly unsafe). Please, do not install both 32 and 64bit of the same                                                                                                                                                                                                                                        
		# package to avoid problems, and NEVER upgrade existant 64bit packages with relative 32bit package.                                                                                                                                                                                                                           
		# Do not forget to install the multilibs.
		ALLOW32BIT=off

		# Enable (1) / Disable (0) the official slackpkg blacklist. May be useful to temporarily skip
		# the slackware blacklist. You can also override it from command line:
		# 'USEBL=0 slackpkg upgrade-all'
		USEBL=1

		# Add custom option to 'wget'.
		# You can solve the repository indisponibility issue by set a timeout here
		WGETOPTS="--timeout=20 --tries=2"

		# If you want replace wget with another downloader search DOWNLOADCMD in documentation
		# at /usr/doc/slackpkg+-*/README
		#DOWNLOADCMD="wget -O"

		# Defines if the changelog of any 3rd party repository must be searched in parent URL when not found in base URL.
		# Can be set to "on" or "off" (default)
		SEARCH_CLOG_INPARENT=on

		# Use the cache for metadata files (CHECKSUMS.md5,...). Enable it (on) to speedup the slackpkg update
		# process by downloading just new files (see README). Disabled by default (off)
		CACHEUPDATE=off

		# You can download-only by setting DOWNLOADONLY to 'on'. You may (you should) also use it in commandline
		# for example: "DOWNLOADONLY=on slackpkg upgrade-all". Useful for large upgrades.
		#DOWNLOADONLY=off

		# Enable (on) / Disable (off) notification events (see notifymsg.conf)
		#ENABLENOTIFY=off

		# Enable (on) / Disable (off) the greylist feature. See /etc/slackpkg/greylist
		GREYLIST=on

		# Defines if commands 'search' and 'file-search' are case-sensitive (on) or not (off). Default to "on"
		SENSITIVE_SEARCH=on

		# Defines if command 'file-search' does search Wole Word (on) of accept partial words (off). Note that
		# you may obtains many many results searching a short partial word
		WW_FILE_SEARCH=on

		# Select the show order in dialogbox. Available "package" "repository" "tag" "path" "arch"
		SHOWORDER=package

		# Allow to show more details of the package in slackpkg info <package>. Accept "none", "basic", "filelist"
		DETAILED_INFO=none

		# Enable (on) / Disable (off) a Strict GPG Check. A repository should contains packages signed
		# with the only original GPG-KEY. In some custom repository may be wanted to mix heterogeneous
		# packages; to use that repository set Strict GPG Check to off. P.S: a repository can
		# contain just ONE gpg-key; you may manually import the other.
		STRICTGPG=on

		# If two or more repositories contains some same packages, you can specify
		# from which repository you prefer to search it.
		# The syntax is "<repository_name>:<package_name>"
		# Accepts regular expressions. To give priority to an entire repository use "<repository_name>"

		# Examples:
		#PKGS_PRIORITY=( restricted:vlc )
		# OR
		#PKGS_PRIORITY=( myrepo )
		# 
		# if you have two reporitories to give priority you must set both in the same line
		#PKGS_PRIORITY=( myrepo restricted:vlc )
		#
		# if you want to install 'ktown' repository you must set it here
		#PKGS_PRIORITY=( ktown )
		# and DO NOT MISS to read special instruction on /usr/doc/slackpkg+-*/repositories.txt
		#
		# If you want a multilib system, uncomment the multilib repository and set:
		#PKGS_PRIORITY=( multilib )
		#
		# (Use /usr/doc/slackpkg+-*/setupmultilib.sh to setup a multilib configuration)
		#
		# For both multilib and ktown set
		#PKGS_PRIORITY=( multilib ktown )

		# Otherwise you can try to upgrade a package from a repository that contains a package with the
		# same tag of the already installed package. Tipically that means to upgrade a package from the
		# same author of the already installed package.
		# Note that this method may not works properly where two repositories contains a package with the
		# same tag.
		# Set TAG_PRIORITY to 'on' to enable this function
		TAG_PRIORITY=off

		# List repositories you want to use (defined below)
		# remember to launch 'slackpkg update' if you modify that row.
		#REPOPLUS=( slackpkgplus restricted alienbob slacky )
		REPOPLUS=( slackpkgplus alienbob )


		# Define mirrors (uncomment one or more mirror; remember to add it to REPOPLUS)
		# GPG Note: after adding/renaming a repository, you must to run 'slackpkg update gpg'
		#           some repositories as salixos, have a partial GPG support;
5. Updating GPG and Package list for slackpkg+

		/usr/sbin/slackpkg update gpg && /usr/sbin/slackpkg update

### Downloading and Installing sbotools

1. Downloading sbotools package:

		/usr/bin/wget https://pink-mist.github.io/sbotools/downloads/sbotools-2.7-noarch-1_SBo.tgz

2. Installing sbotools package:

		/sbin/installpkg /sbin/installpkg sbotools-2.7-noarch-1_SBo.tgz

3. Running `sbosnap fetch` to get slackbuilds list

		/usr/sbin/sbosnap fetch
