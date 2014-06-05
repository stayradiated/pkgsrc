NAME=node
VERSION=0.10.28

export PATH:= /usr/bin:${PATH}

build:
	rm -rf files
	rm -rf "${NAME}-v${VERSION}"
	wget -nc "http://nodejs.org/dist/v${VERSION}/${NAME}-v${VERSION}.tar.gz"
	tar -xzvf "${NAME}-v${VERSION}.tar.gz"
	rm -rf files
	cd "${NAME}-v${VERSION}"; \
		./configure --prefix=/usr/pkg; \
		make; \
		make DESTDIR=../files install
	mv files/usr/pkg/* files/
	rm -rf files/usr/pkg
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
	rm -rf "${NAME}-v${VERSION}"
	rm -rf "${NAME}-v${VERSION}.tar.gz"
	rm -rf files
