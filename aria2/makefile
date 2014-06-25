NAME=aria2
VERSION=1.18.5
DESTDIR=${HOME}/pkgsrc/git/aria2/files

export PATH:= /usr/bin:${PATH}

build:
	rm -rf files
	rm -rf "${NAME}-${VERSION}"
	wget -nc "https://downloads.sourceforge.net/project/${NAME}/stable/${NAME}-${VERSION}/${NAME}-${VERSION}.tar.bz2"
	shasum -c ./shasum
	tar -xjvf "${NAME}-${VERSION}.tar.bz2"
	cd "${NAME}-${VERSION}"; \
		./configure \
			--prefix=/usr/pkg \
			--disable-dependency-tracking \
			--with-appletls \
      --without-openssl \
      --without-gnutls \
      --without-libgmp \
      --without-libnettle \
      --without-libgcrypt; \
		make; \
		make DESTDIR=${DESTDIR} install
	mv files/usr/pkg/* files/
	rm -rf files/usr/pkg
	pkg_info -X pkg_install | egrep '^(MACHINE_ARCH|OPSYS|OS_VERSION|PKGTOOLS_VERSION)' > build-info
	echo "@cwd /usr/pkg" > contents
	echo "@name ${NAME}-${VERSION}" >> contents
	(cd files; find * -type f -or -type l | sort) >> contents
	pkg_create \
		-B build-info \
		-c comment \
		-d description \
		-f contents \
		-I /usr/pkg \
		-p files \
		-U ${NAME}-${VERSION}.tgz
	rm -rf "${NAME}-${VERSION}"
	rm -rf "${NAME}-${VERSION}.tar.gz"
	rm -rf files
