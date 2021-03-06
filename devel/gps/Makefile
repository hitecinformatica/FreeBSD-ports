# Created by: John Marino <marino@FreeBSD.org>
# $FreeBSD$

PORTNAME=	gps
PORTVERSION=	2016
PORTREVISION=	1
CATEGORIES=	devel
MASTER_SITES=	http://downloads.dragonlace.net/src/
PKGNAMESUFFIX=	-ide
DISTNAME=	gps-gpl-${PORTVERSION}-src

MAINTAINER=	ports@FreeBSD.org
COMMENT=	GNAT Programming Studio - IDE for Ada and many other languages

LICENSE=	GPLv3

BROKEN_aarch64=		fails to compile: No rule to make target 'default'

BUILD_DEPENDS=	gprbuild:devel/gprbuild \
		xmlada>=4.4:textproc/xmlada \
		gnatcoll>=2016:devel/gnatcoll \
		templates_parser>11:textproc/templates_parser
LIB_DEPENDS=	libfontconfig.so:x11-fonts/fontconfig \
		libgtkada.so:x11-toolkits/gtkada3 \
		libfreetype.so:print/freetype2 \
		libclang.so.3.8:devel/llvm38
RUN_DEPENDS=	rsvg-convert:graphics/librsvg2 \
		gdb${PORTGDB}:devel/gdb \
		gprbuild:devel/gprbuild \
		${PYTHON_PKGNAMEPREFIX}pep8>0:devel/pep8 \
		${PYTHON_PKGNAMEPREFIX}jedi>0:devel/py-jedi

GNU_CONFIGURE=	yes
USES=		ada:run gettext gmake perl5 pkgconfig python
USE_XORG=	x11
USE_PERL5=	build
USE_GNOME=	glib20 pygobject3 gtk30 pango atk cairo gdkpixbuf2
PYCOMPILE=	${PREFIX}/lib/${PYTHON_VERSION}/compileall.py
AWKREVDATE=	'/Last Changed Date/ {print "\"s|unknown date|" $$4 "|\""}'
GPSVER=		GPS_VERSION=${PORTVERSION}
ALL_TARGET=	default
NO_MTREE=	yes
SUB_FILES=	gps
PORTGDB=	${GDBVER:S/.//g}

# note: librsvg2 is needed to display the svg icons in the menu.  Actually
# needed is lib/gdk-pixbuf-2.0/%%GTK2_VER%%/loaders/libpixbufloader-svg.so ,
# but it's a variable library needed only at runtime, which is obnoxious so
# we just request the sole binary to pull in the package which effectively
# accomplishes what is desired.

OPTIONS_DEFINE=		DEBUG DOCS EXAMPLES
OPTIONS_SUB=		yes

DOCS_BUILD_DEPENDS=	sphinx-build:textproc/py-sphinx
DEBUG_CONFIGURE_ON=	Build=Debug
DEBUG_CONFIGURE_OFF=	Build=Production

CONFIGURE_TARGET=	${ARCH:S/amd64/x86_64/}-aux-${OPSYS:tl}${OSREL}
CONFIGURE_ENV+=		AWK=/usr/bin/awk
CONFIGURE_ARGS=		--with-gtk=3.0 \
			--with-clang=${PREFIX}/llvm38/lib
MAKE_ARGS+=		PROCESSORS=${MAKE_JOBS_NUMBER}
MAKE_ARGS+=		PYTHON_CMD=${PYTHON_CMD}
PORTDOCS=		html pdf
PORTEXAMPLES=		demo python remote tutorial xml

.include <bsd.port.options.mk>

SUB_LIST=		ADA_PATH=${PREFIX}/${ADAXX}-aux/bin
GDBVER!=		${GREP} ^PORTVERSION ${.CURDIR}/../gdb/Makefile | \
			${AWK} '{print $$2}'

post-extract:
	@${MV} ${WRKSRC}/templates_parser ${WRKSRC}/TP.ignore
	@${MV} ${WRKSRC}/Makefile.gnat ${WRKSRC}/Makefile.gnat.ignore
	@${FIND} ${WRKSRC}/share -name "*.pyc" -delete
	${RM} ${WRKSRC}/share/support/core/cross_references.py

post-patch:
	@${RM} ${WRKSRC}/share/plug-ins/*.orig
	@${ECHO} "GNAT_SOURCES=" > ${WRKSRC}/Makefile.gnat
	@${REINPLACE_CMD} -e 's/^with .*templates_parser\//with "/g' \
		${WRKSRC}/gnatdoc/gnatdoc.gpr \
		${WRKSRC}/vcs/vcs.gpr
	@${REINPLACE_CMD} -e 's|@PREFIX@|${PREFIX}|g' \
		${WRKSRC}/widgets/widgets.gpr
	@${SED} -i '' -e \
		's|= "gnat"|= "${LOCALBASE}/${ADAXX}-aux/bin/gnat"|' \
		${WRKSRC}/share/support/core/gps_utils/gnat_rules.py
	@${REINPLACE_CMD} -e '/Gdb_Command/ s|"gdb"|"gdb${PORTGDB}"|' \
		${WRKSRC}/gvd/src/debugger-gdb_mi.adb \
		${WRKSRC}/gvd/src/debugger-gdb.adb
	@${REINPLACE_CMD} -i '' -e 's|ISO-8859|ISO8859|' \
		${WRKSRC}/kernel/src/gps-kernel-charsets.adb \
		${WRKSRC}/share/support/core/os_utils.py \
		${WRKSRC}/configure* \
		${WRKSRC}/po/build_skeleton.pl
	@${REINPLACE_CMD} -e 's|iso-8859|ISO8859|' \
		${WRKSRC}/cli/src/gps-cli_scripts.adb \
		${WRKSRC}/cli/src/gnatdoc_main.adb

do-build:
	@${CP} ${PREFIX}/include/gnat_util/impunit.* ${WRKSRC}/distrib
	cd ${WRKSRC} && ${SETENV} ${MAKE_ENV} \
		${MAKE_CMD} ${MAKE_ARGS} ${ALL_TARGET}
	cd ${WRKSRC}/docs && ${SETENV} ${MAKE_ENV} \
		${MAKE_CMD} ${MAKE_ARGS} all
	${PYTHON_CMD} ${PYCOMPILE} ${WRKSRC}/share/plug-ins
	${PYTHON_CMD} ${PYCOMPILE} ${WRKSRC}/share/library
	${PYTHON_CMD} ${PYCOMPILE} ${WRKSRC}/share/support

post-install:
	${RM} -r ${STAGEDIR}${PREFIX}/share/doc/gps/html/*/_sources
	${INSTALL_SCRIPT} ${WRKDIR}/gps ${STAGEDIR}${PREFIX}/bin
	echo "This is the GPS GPL Edition for the ${OPSYS} Ports Collection." \
		> ${STAGEDIR}${PREFIX}/share/gps/about.txt
	${FIND} ${STAGEDIR}${PREFIX} -type d -empty -delete

.include <bsd.port.mk>
