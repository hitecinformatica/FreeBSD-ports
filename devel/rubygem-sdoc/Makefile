# Created by: Steve Wills <swills@FreeBSD.org>
# $FreeBSD$

PORTNAME=	sdoc
PORTVERSION=	0.4.2
PORTREVISION=	2
CATEGORIES=	devel rubygems
MASTER_SITES=	RG

MAINTAINER=	ruby@FreeBSD.org
COMMENT=	Create rdoc html with javascript search index

LICENSE=	MIT

RUN_DEPENDS=	rubygem-json1>=1.7.7:devel/rubygem-json1 \
		rubygem-rdoc>=4.0:devel/rubygem-rdoc

NO_ARCH=	yes
USE_RUBY=	yes
USES=		gem

PLIST_FILES=	bin/sdoc bin/sdoc-merge

.include <bsd.port.mk>
