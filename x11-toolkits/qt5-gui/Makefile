# $FreeBSD$

PORTNAME=	gui
DISTVERSION=	${QT5_VERSION}
PORTREVISION=	2
CATEGORIES=	x11-toolkits graphics
PKGNAMEPREFIX=	qt5-

MAINTAINER=	kde@FreeBSD.org
COMMENT=	Qt graphical user interface module

BUILD_DEPENDS=	at-spi2-core>=0:accessibility/at-spi2-core
LIB_DEPENDS=	libdbus-1.so:devel/dbus \
		libfontconfig.so:x11-fonts/fontconfig \
		libfreetype.so:print/freetype2 \
		libharfbuzz.so:print/harfbuzz \
		libpng.so:graphics/png \
		libxcb.so:x11/libxcb \
		libxcb-image.so:x11/xcb-util-image \
		libxcb-keysyms.so:x11/xcb-util-keysyms \
		libxcb-render-util.so:x11/xcb-util-renderutil \
		libxcb-icccm.so:x11/xcb-util-wm \
		libxkbcommon.so:x11/libxkbcommon
RUN_DEPENDS=	xdg-open:devel/xdg-utils

USES=		jpeg localbase
USE_GL=		gl egl
USE_GNOME=	glib20
USE_QT5=	core dbus network qmake_build buildtools_build
QT_DIST=	base
USE_XORG=	ice sm x11 xi xrender
HAS_CONFIGURE=	yes
CONFIGURE_ARGS=	-no-eglfs \
		-no-evdev \
		-system-harfbuzz
USE_LDCONFIG=	${PREFIX}/${QT_LIBDIR_REL}

BUILD_WRKSRC=	${WRKSRC}/src/${PORTNAME}
INSTALL_WRKSRC=	${BUILD_WRKSRC}

# Build and install QtPlatformSupport and default QPA plugins (XCB,
# minimal and offscreen). QtGui won't work without (one of) them, but
# they depend on QtGui itself, so they can't be added as dependencies.
# QtPlatformSupport doesn't need to be installed (it's a static
# library), but might be needed by people porting Qt on new platforms.
MORE_WRKSRCS=	src/platformheaders \
		src/platformsupport \
		src/plugins/platforms \
		src/plugins/generic

# Image formats are split through different tarballs, these are the
# main ones; input contexts require no additional dependency.
MORE_WRKSRCS+=	src/plugins/imageformats \
		src/plugins/platforminputcontexts

# openglextensions is the static library to use for further things like qtcanvas3d
MORE_WRKSRCS+=	src/openglextensions

QT_DEFINES=	ACCESSIBILITY DBUS FONTCONFIG FREETYPE GLIB \
		IMAGEFORMAT_PNG OPENGL SHAPE XCB XKB XKBCOMMON XRENDER
QT_CONFIG=	accessibility accessibility-atspi-bridge dbus \
		fontconfig glib opengl png system-freetype \
		system-jpeg system-png xcb xcb-glx xcb-render \
		xcb-xlib xinput2 xlib xrender

# Inherited from Qt 4.
.if defined(PACKAGE_BUILDING)
RUN_DEPENDS+=	${LOCALBASE}/share/fonts/encodings/encodings.dir:x11-fonts/encodings \
		xorg-fonts-truetype>0:x11-fonts/xorg-fonts-truetype
.endif

CFLAGS_powerpc64=	-mminimal-toc

post-configure:
.for d in src/gui ${MORE_WRKSRCS}
	${MKDIR} ${WRKSRC}/${d}
	cd ${WRKSRC}/${d} && ${SETENV} ${QMAKE_ENV} ${_QMAKE} ${QMAKE_ARGS} ${WRKSRC}/${d}
.endfor

post-build:
.for d in ${MORE_WRKSRCS}
	@cd ${WRKSRC}/${d} && \
		${SETENV} ${MAKE_ENV} ${MAKE} ${MAKE_FLAGS} ${MAKEFILE} \
		${_MAKE_JOBS} ${MAKE_ARGS} ${ALL_TARGET}
.endfor

post-install:
.for d in ${MORE_WRKSRCS}
	@cd ${WRKSRC}/${d} && \
		${SETENV} ${MAKE_ENV} ${MAKE} ${MAKE_FLAGS} ${MAKEFILE} \
		${MAKE_ARGS} ${INSTALL_TARGET}
.endfor

.include <bsd.port.mk>
