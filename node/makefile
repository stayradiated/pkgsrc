NAME=node
VERSION=0.10.28
NPM_VERSION=1.4.13

export PATH:= /usr/bin:${PATH}

clean:
	rm -rf "${NAME}-v${VERSION}"

source:
	rm -rf "${NAME}-v${VERSION}"
	rm -rf "npm-${VERSION}"

	wget -nc "http://nodejs.org/dist/v${VERSION}/${NAME}-v${VERSION}.tar.gz"
	wget -nc "http://registry.npmjs.org/npm/-/npm-${NPM_VERSION}.tgz"

	tar -xzvf "${NAME}-v${VERSION}.tar.gz"
	tar -xzvf "npm-${NPM_VERSION}.tgz"

node:
	rm -rf files

	cd "${NAME}-v${VERSION}"; \
		./configure --prefix=/usr/pkg; \
		make; \
		make DESTDIR=../files install
	mv files/usr/pkg/* files/
	rm -rf files/usr/pkg

npm:
	PATH=`pwd`/files/bin:${PATH}
	rm -rf files/lib/node_modules/npm
	echo "prefix = /usr/pkg" >> package/.npmrc
	mkdir -p files/lib/node_modules/.bin
	cp -r package files/lib/node_modules
	mv files/lib/node_modules/package files/lib/node_modules/npm
	cd files/lib/node_modules/npm; \
		make install
	files/bin/npm update npm -g --prefix files

	# mv files/usr/share/man files/usr; \
	# rmdir ${PKG}/usr/share

package:
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
