# Created by: Dmitry Sivachenko <demon@FreeBSD.org>
# $FreeBSD$

PORTNAME=	theano
PORTVERSION=	0.9.0
PORTREVISION=	3
CATEGORIES=	math python
MASTER_SITES=	CHEESESHOP/source/T/Theano
PKGNAMEPREFIX=	${PYTHON_PKGNAMEPREFIX}
DISTNAME=	Theano-${DISTVERSION}

MAINTAINER=	demon@FreeBSD.org
COMMENT=	Optimizing compiler for evaluating math expressions on CPUs and GPUs

LICENSE=	BSD3CLAUSE
LICENSE_FILE=	${WRKSRC}/LICENSE.txt

RUN_DEPENDS=	${PYNUMPY} \
		${PYTHON_PKGNAMEPREFIX}scipy>=0.8.0:science/py-scipy@${FLAVOR}

USES=		compiler:openmp python
USE_PYTHON=	concurrent distutils autoplist

SUB_FILES=	pkg-message
SUB_LIST=	CXX=${CXX}

post-patch:
	@${REINPLACE_CMD} -e 's|/bin/bash|/bin/sh|; s|python |${PYTHON_CMD} |' \
		${WRKSRC}/theano/misc/check_blas_many.sh

.include <bsd.port.mk>
