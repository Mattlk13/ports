# Created by: Bill Fumerola <billf@chc-chimes.com>
# $FreeBSD$

PORTNAME?=	wireshark
PORTVERSION=	2.6.6
PORTREVISION?=	1
CATEGORIES=	net ipv6
MASTER_SITES=	http://www.wireshark.org/download/src/ \
		http://ftp.uni-kl.de/pub/wireshark/src/ \
		http://wireshark.askapache.com/download/src/all-versions/ \
		ftp://ftp.uni-kl.de/pub/wireshark/src/ \
		http://prdownloads.sourceforge.net/wireshark/ \
		http://ftp.yz.yamagata-u.ac.jp/pub/network/security/wireshark/src/ \
		ftp://ftp.yz.yamagata-u.ac.jp/pub/network/security/wireshark/src/ \
		http://wireshark.zing.org/download/src/
DISTNAME=	${DATADIR_NAME}-${PORTVERSION}

MAINTAINER=	marcus@FreeBSD.org
COMMENT?=	Powerful network analyzer/capture tool

LICENSE=	GPLv2
LICENSE_FILE=	${WRKSRC}/COPYING

CONFLICTS?=	wireshark-qt5-[0-9]*

DATADIR_NAME=	wireshark
DATADIR=	${PREFIX}/share/${DATADIR_NAME}

USE_HARDENING=	safestack

GNU_CONFIGURE=	yes
USES=		alias cpe compiler:c++11-lang gettext-runtime gmake \
		libtool localbase pathfix \
		perl5 pkgconfig python:build ssl tar:xz
USE_PERL5=	build
USE_GNOME=	glib20 libxml2
CONFIGURE_ARGS+=	--program-transform-name="" \
			--with-ssl=${OPENSSLBASE} \
			--with-lz4 \
			--with-nghttp2 \
			--with-snappy
INSTALL_TARGET=	install-strip
USE_LDCONFIG=	yes

LIB_DEPENDS+=	libgpg-error.so:security/libgpg-error \
		libgcrypt.so:security/libgcrypt \
		liblz4.so:archivers/liblz4 \
		libnghttp2.so:www/libnghttp2 \
		libsnappy.so:archivers/snappy

CFLAGS+=	-funit-at-a-time
PLIST_SUB+=	PORTVERSION=${PORTVERSION}

.for x in capinfos editcap mergecap rawshark text2pcap dftest randpkt \
    reordercap captype randpktdump udpdump sharkd dumpcap
.if defined(LITE)
PKGNAMESUFFIX=		-lite
PLIST_SUB+=		${x:tu}="@comment $x not installed" \
			${x:tu}_LIB="@comment $x not installed " \
			${x:tu}_MAN="@comment $x not installed "
CONFIGURE_ARGS+=	--enable-$x=no
OPTIONS_EXCLUDE+=	RTP SNMP LUA CARES ANDROIDDUMP SSHDUMP
.else
PLIST_SUB+=		${x:tu}=bin/$x \
			${x:tu}_MAN="" \
			${x:tu}_LIB=""
.endif
.endfor

OPTIONS_DEFINE=		RTP SNMP MAXMIND LUA DECRYPT ANDROIDDUMP CARES SSHDUMP CISCODUMP

OPTIONS_RADIO=		GUI
OPTIONS_SINGLE=		KERBEROS

OPTIONS_RADIO_GUI=	GTK2 GTK3 QT5
OPTIONS_SINGLE_KERBEROS=KRB_BASE KRB_HEIMDAL KRB_MIT KRB_NONE

OPTIONS_DEFAULT?=	SNMP MAXMIND KRB_BASE GTK3 DECRYPT

RTP_DESC=		Enable support for playing back RTP streams
DECRYPT_DESC=		Decryption support for SSL and IPSec
ANDROIDDUMP_DESC=	Build androiddump extcap tool
MAXMIND_DESC=		Enable MaxMindDB-based GeoIP geolocation support
CARES_DESC=		Enable support for asynchronous DNS via c-ares
KERBEROS_DESC=		Kerberos dissection support
KRB_BASE_DESC=		Kerberos support via base system
KRB_HEIMDAL_DESC=	Kerberos support via security/heimdal
KRB_MIT_DESC=		Kerberos support via security/krb5
KRB_NONE_DESC=		Disable Kerberos support
SSHDUMP_DESC=		Build sshdump extcap tool
CISCODUMP_DESC=		Build ciscodump extcap tool

GTK2_USE=	GNOME=gtk20,cairo
GTK3_USE=	GNOME=gtk30,cairo
QT5_USES=		qt:5
QT5_USE=	qt=core,gui,widgets,printsupport,buildtools_build,linguisttools_build

LUA_USES=		lua:5[1-2]
KRB_BASE_USES=		gssapi
KRB_HEIMDAL_USES=	gssapi:heimdal
KRB_MIT_USES=		gssapi:mit

RTP_LIB_DEPENDS=	libportaudio.so:audio/portaudio \
    			libspandsp.so:comms/spandsp
RTP_CPPFLAGS=		-I${LOCALBASE}/include
MAXMIND_LIB_DEPENDS=	libmaxminddb.so:net/libmaxminddb
SNMP_LIB_DEPENDS=	libsmi.so:net-mgmt/libsmi
CARES_LIB_DEPENDS=	libcares.so:dns/c-ares
DECRYPT_LIB_DEPENDS=	libgnutls.so:security/gnutls \
			libgcrypt.so:security/libgcrypt
SSHDUMP_LIB_DEPENDS=	libssh.so:security/libssh
CISCODUMP_LIB_DEPENDS=	libssh.so:security/libssh

KRB_NONE_CONFIGURE_ON=	--without-krb5
KRB_NONE_CONFIGURE_OFF=	--with-krb5 --with-krb5-crypto-openssl
KRB_NONE_CONFIGURE_ENV_OFF=	KRB5_CONFIG=${KRB5CONFIG}

GTK2_CONFIGURE_ON=	--with-gtk=2 --with-qt=no
GTK3_CONFIGURE_ON=	--with-gtk=3 --with-qt=no
QT5_CONFIGURE_ON=	--with-qt=5 --with-gtk=no
LUA_CONFIGURE_WITH=	lua
RTP_CONFIGURE_WITH=	portaudio spandsp
SNMP_CONFIGURE_WITH=	libsmi
CARES_CONFIGURE_WITH=	c-ares
MAXMIND_CONFIGURE_WITH=maxminddb
DECRYPT_CONFIGURE_WITH=	gnutls
ANDROIDDUMP_CONFIGURE_ON=--enable-androiddump=yes
ANDROIDDUMP_CONFIGURE_OFF=--enable-androiddump=no
SSHDUMP_CONFIGURE_ON=--enable-sshdump=yes
SSHDUMP_CONFIGURE_OFF=--enable-sshdump=no
CISCODUMP_CONFIGURE_ON=--enable-sshdump=yes
CISCODUMP_CONFIGURE_OFF=--enable-sshdump=no

LUA_CPPFLAGS=		-I${LUA_INCDIR}

QT5_CONFIGURE_ENV=	PATH="${PATH}:${QT_BINDIR}"
QT5_EXTRA_PATCHES=	${FILESDIR}/extrapatch-configure-qt5
QT5_MAKE_ENV=		PATH="${PATH}:${QT_BINDIR}"
QT5_VARS=		PKGNAMESUFFIX=-qt5
QT5_PLIST_SUB=		GUI_GTK="@comment "

GTK2_PLIST_SUB=		GUI_GTK=""
GTK3_PLIST_SUB=		GUI_GTK=""

OPTIONS_SUB=		yes

.if defined(LITE)
CONFIGURE_ARGS+=--enable-androiddump=no \
		--enable-sshdump=no \
		--enable-ciscodump=no
PLIST_SUB+=	ANDROIDDUMP_LIB="@comment " \
		ANDROIDDUMP_MAN="@comment " \
		SSHDUMP_LIB="@comment " \
		SSHDUMP_MAN="@comment " \
		CISCODUMP_LIB="@comment " \
		CISCODUMP_MAN="@comment "
.endif

.include <bsd.port.options.mk>

.if empty(PORT_OPTIONS:MGTK2) && empty(PORT_OPTIONS:MGTK3) && \
    empty(PORT_OPTIONS:MQT5)
CONFIGURE_ARGS+=	--disable-wireshark
PLIST_SUB+=		GUI="@comment "
.else
USES+=			desktop-file-utils shared-mime-info
INSTALLS_ICONS=		yes
PLIST_SUB+=		GUI=""
.endif

.if !defined(LITE)
.if ${PORT_OPTIONS:MANDROIDDUMP}
PLIST_SUB+=	ANDROIDDUMP_LIB="" \
		ANDROIDDUMP_MAN=""
.else
PLIST_SUB+=	ANDROIDDUMP_LIB="@comment " \
		ANDROIDDUMP_MAN="@comment "
.endif
.if ${PORT_OPTIONS:MSSHDUMP}
PLIST_SUB+=	SSHDUMP_LIB="" \
		SSHDUMP_MAN=""
.else
PLIST_SUB+=	SSHDUMP_LIB="@comment " \
		SSHDUMP_MAN="@comment "
.endif
.if ${PORT_OPTIONS:MCISCODUMP}
PLIST_SUB+=	CISCODUMP_LIB="" \
		CISCODUMP_MAN=""
.else
PLIST_SUB+=	CISCODUMP_LIB="@comment " \
		CISCODUMP_MAN="@comment "
.endif
.endif

.if ${PORT_OPTIONS:MKRB_BASE} && exists(${LOCALBASE}/include/krb5.h)
IGNORE=			cannot build with base Kerberos if krb5 port is installed. \
			Use ``make config'' to choose a different option
.endif

.if empty(PORT_OPTIONS:MGTK2) && empty(PORT_OPTIONS:MGTK3) && \
	${PORT_OPTIONS:MRTP}
IGNORE=			the RTP support requires GTK+ frontend
.endif

post-patch:
	@${REINPLACE_CMD} -e 's|llua|llua-${LUA_VER}|g ; \
		s|-DQT_GUI_LIB|-DQT_GUI_LIB $$CPPFLAGS| ' \
		${WRKSRC}/configure
	# XXX: Should be optional as to whether or not this flag works at runtime
	@${REINPLACE_CMD} -e 's|-msse4.2||g' \
		${WRKSRC}/configure
	@${REINPLACE_CMD} -e 's|/usr/share/applications|${DESKTOPDIR}|g ; \
		s|/usr/share|${LOCALBASE}/share|g ; \
		s|{mime/packages,applications}|mime/packages| ; \
		s|{apps,mimetypes}|apps $$(DESTDIR)${LOCALBASE}/share/icons/hicolor/$$$${size}x$$$${size}/mimetypes|' \
		${WRKSRC}/Makefile.in

.if ${PORT_OPTIONS:MGTK2} || ${PORT_OPTIONS:MGTK3}
post-install:
	${MV} ${STAGEDIR}${PREFIX}/bin/${PORTNAME}-gtk \
		${STAGEDIR}${PREFIX}/bin/${PORTNAME}
.endif

.include <bsd.port.mk>
