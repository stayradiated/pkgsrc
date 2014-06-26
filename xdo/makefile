NAME=xdo
VERSION=0.3
DESTDIR=${HOME}/pkgsrc/git/${NAME}/files

export PATH:= /usr/bin:${PATH}

build:
	rm -rf files
	rm -rf "${NAME}-${VERSION}"
	wget -nc "https://github.com/baskerville/${NAME}/archive/${VERSION}.tar.gz"
	shasum -c ./shasum
	tar -xjvf "${VERSION}.tar.gz"
	cd "${NAME}-${VERSION}"; \
		make \
			PREFIX=/opt/x11; \
		make \
			DESTDIR=${DESTDIR} \
			PREFIX=/usr/pkg \
			MANPREFIX=/usr/pkg/man \
			install
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
	rm -rf "${VERSION}.tar.gz"
	rm -rf files
