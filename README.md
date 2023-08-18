# [Mamolinux Plymouth](https://hsbasu.github.io/mamolinux-plymouth)

<p align="center">
	<a href="https://github.com/mamolinux/mamolinux-plymouth/blob/master/COPYING">
		<img src="https://img.shields.io/github/license/mamolinux/mamolinux-plymouth?label=License" alt="License">
	</a>
	<a href="#">
		<img src="https://img.shields.io/github/repo-size/mamolinux/mamolinux-plymouth?label=Repo%20size" alt="GitHub repo size">
	</a>
	<a href="https://github.com/mamolinux/mamolinux-plymouth/releases/latest">
		<img src="https://img.shields.io/github/v/release/mamolinux/mamolinux-plymouth?label=Latest%20Stable%20Release" alt="GitHub release (latest by date)">
	</a>
</p>

Ubuntu-like Plymouth themes for MamoLinux. Replaces Ubuntu Logo with MamoLinux Logo.

## ToDo
1. [x] Add mamolinux-text theme
2. [x] Add mamolinux-logo theme

## Building from source
Building plymouth theme from source is a bit tricky. The way I do it for **MamoLinux** is as follows:
1. For the first time:
	1. Create the git repo on GitHub and clone it:
		```
		git clone git@github.com:hsbasu/mamolinux-plymouth.git
		cd mamolinux-plymouth
		```
	1. Add and fetch the source from ubuntu:
		```
		git remote add upstream-ubuntu https://git.launchpad.net/ubuntu/+source/plymouth
		git fetch upstream-ubuntu
		```
	For subsequent times, go to step 2.
2. Create a local release branch from the **unapplied** branch using commit hash or the unapplied branch for current ubuntu release:
	```
	git checkout -b mamolinux/<release> hash	# from commit hash
	```
	**Note**, If Launchpad PPA will be used to build the binary packages, then branch should be created from hashes where the patches are not applied.
	
	From the unapplied branch for current ubuntu release, use:
	```
	git checkout -b mamolinux/<release> upstream-ubuntu/ubuntu/<release>	# from unapplied branch
	```
	Replace `<release>` with `jammy`, `lunar` or other ubuntu release name.
4. Modify the sources:
	1. Put `mamolinux-logo.png` in `debian/local` and **delete** `ubuntu-logo.png`
	2. Replace `ubuntu-logo.png` with `mamolinux-logo.png` in
		```
		debian/local/plymouth.hook#L120-L125		# look at Lines between 120 and 125
		debian/patches/ubuntu-logo.patch#(L10-L15,L235-L240)
		debian/rules#(L40-L45,L80-L90)
		debian/source/include-binaries#L1-L5
		themes/ubuntu-logo/Makefile.am#L1-L5
		themes/ubuntu-logo/ubuntu-logo.script#L185-L190
		```
5. Commit the changes
	```
	git add debian/local/plymouth.hook\
			 debian/patches/ubuntu-logo.patch\
			 debian/rules\
			 debian/source/include-binaries\
			 themes/ubuntu-logo/Makefile.am\
			 themes/ubuntu-logo/mamolinux-logo.png
			 themes/ubuntu-logo/ubuntu-logo.png\
			 themes/ubuntu-logo/ubuntu-logo.script
	git commit
	dch -D <release> -l mamolinux	# Increase version number with Myself as Source Changer
	git commit -m "<latest-changelog-version>"
	```
6. Build packages using `pbuilder`. [Check this]() on how to build using `pbuilder`.
	1. Copy `mamolinux-plymouth` and `plymouth_0.9.4git20200323.orig.tar.xz` to `~/pbuilder`
	2. Open terminal at `~/pbuilder/mamolinux-plymouth`
	3. Build locally:
		1. Install the build dependencies available in *Build-Depends* section of `debian/contorl` file.
		2. run `dpkg-buildpackage --no-sign`
		
		This will create `*.deb` files at `~/pbuilder`
	4. To build locally and push to **Launchpad PPA**. Copy paste the following commands one-by-one:
		```
		debuild -S -d -us -uc	# creates unsigned sources
		pbuilder-dist <release> build ../plymouth_<version>.dsc	# Generates deb packages for local testing
		debuild -S -sa -d	# creates signed sources for uploading to Launchpad
		cd ..
		dput ppa:mamolinux/stable plymouth_<version>_source.changes
7. After a few minutes `plymouth_<version>` will appear as **building** [here](https://launchpad.net/~mamolinux/+archive/ubuntu/stable/+packages)

To build using launchpad recipe, after **Step 4** follow the steps below:
1. install `git-build-recipe`.
2. Create a file `plymouth.recipe` with the following content:
	```
	# git-build-recipe format 0.4 deb-version {debversion}
	https://github.com/mamolinux/mamolinux-plymouth mamolinux/<release>
	```
3. Install the build dependencies available in *Build-Depends* section of `debian/contorl` file.
4. Run `git-build-recipe --allow-fallback-to-native plymouth.recipe plymouth`. This will create the necessary `.dsc` file.
5. Run `pbuilder-dist <release> plymouth/plymouth_*.dsc`. This will create `*.deb` files at `~/pbuilder/<release>_result` directory.

## Contributors

### [Himadri Sekhar Basu](https://hsbasu.github.io)
Current maintainer.

### [Ubuntu Developers](https://launchpad.net/~ubuntu-devel-discuss-lists)
Original maintainer

## Donations
I am a freelance programmer. So, If you like this app and would like to offer me a coffee ( &#9749; ) to motivate me further, you can do so via:

[![](https://liberapay.com/assets/widgets/donate.svg)](https://liberapay.com/hsbasu/donate)
[![](https://www.paypalobjects.com/webstatic/i/logo/rebrand/ppcom.svg)](https://paypal.me/hsbasu)
[![](https://hsbasu.github.io/styles/icons/logo/svg/upi-logo.svg)](https://hsbasu.github.io/images/upi-qr.jpg)

## References:
1. [plymouth package for ubuntu](https://launchpad.net/ubuntu/+source/plymouth)
