# $FreeBSD$

PORTNAME=	tinc
PORTVERSION=	1.1pre15
CATEGORIES=	security
MASTER_SITES=	https://www.tinc-vpn.org/packages/ \
		http://www.tinc-vpn.org/packages/
PKGNAMESUFFIX=	-devel

MAINTAINER=	dinoex@FreeBSD.org
COMMENT=	Virtual Private Network (VPN) daemon

LICENSE=	GPLv3

LIB_DEPENDS=	liblzo2.so:archivers/lzo2

USES=		cpe ssl makeinfo readline
CPE_VENDOR=	tinc-vpn
GNU_CONFIGURE=	yes
CPPFLAGS+=	-I${LOCALBASE}/include
LDFLAGS+=	-L${LOCALBASE}/lib
CONFIGURE_ARGS=	--localstatedir=/var
USE_RC_SUBR=	tincd
INFO=		tinc
PLIST_FILES=	bin/tinc-gui sbin/tincd sbin/tinc \
		man/man8/tinc-gui.8.gz man/man5/tinc.conf.5.gz \
		man/man8/tinc.8.gz man/man8/tincd.8.gz 

.include <bsd.port.mk>
