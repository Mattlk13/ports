# $FreeBSD$

PORTNAME=	dnsdbq
PORTVERSION=	1.0.0
DISTVERSIONPREFIX=	v
CATEGORIES=	dns

MAINTAINER=	truckman@FreeBSD.org
COMMENT=	DNSDB API Client, C Version

LICENSE=		APACHE20 ISCL
LICENSE_COMB=		multi
LICENSE_FILE_APACHE20=	${WRKSRC}/Apache-2.0
LICENSE_FILE_ISCL=	${WRKSRC}/ISC

BROKEN_mips=		fails to compile: dnsdbq.c:817: passing argument 1 of 'curl_global_init' with different width due to prototype
BROKEN_mips64=		fails to compile: dnsdbq.c:817: passing argument 1 of 'curl_global_init' with different width due to prototype
BROKEN_powerpc64=	fails to compile: dnsdbq.c:817: passing argument 1 of 'curl_global_init' with different width due to prototype

LIB_DEPENDS=	libjansson.so:devel/jansson \
		libcurl.so:ftp/curl

USES=		localbase
USE_GITHUB=	yes
GH_ACCOUNT=	dnsdb

do-install:
	${INSTALL_PROGRAM} ${WRKSRC}/${PORTNAME} ${STAGEDIR}${PREFIX}/bin/
	${MKDIR} ${STAGEDIR}${PREFIX}/share/man/man1/
	${INSTALL_MAN} ${WRKSRC}/${PORTNAME}.man ${STAGEDIR}${PREFIX}/share/man/man1/${PORTNAME}.1

PLIST_FILES=	bin/${PORTNAME} share/man/man1/${PORTNAME}.1

.include <bsd.port.mk>
