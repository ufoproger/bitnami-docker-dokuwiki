FROM docker.io/bitnami/minideb:buster
LABEL maintainer "Bitnami <containers@bitnami.com>"

ENV HOME="/" \
    OS_ARCH="amd64" \
    OS_FLAVOUR="debian-10" \
    OS_NAME="linux"

COPY prebuildfs /
# Install required system packages and dependencies
RUN install_packages acl ca-certificates curl gzip libargon2-1 libaudit1 libbsd0 libbz2-1.0 libc6 libcap-ng0 libcom-err2 libcurl4 libexpat1 libffi6 libfftw3-double3 libfontconfig1 libfreetype6 libgcc1 libgcrypt20 libglib2.0-0 libgmp10 libgnutls30 libgomp1 libgpg-error0 libgssapi-krb5-2 libhogweed4 libicu63 libidn2-0 libjpeg62-turbo libk5crypto3 libkeyutils1 libkrb5-3 libkrb5support0 liblcms2-2 libldap-2.4-2 liblqr-1-0 libltdl7 liblzma5 libmagickcore-6.q16-6 libmagickwand-6.q16-6 libmcrypt4 libmemcached11 libmemcachedutil2 libncurses6 libnettle6 libnghttp2-14 libp11-kit0 libpam0g libpcre3 libpng16-16 libpq5 libpsl5 libreadline7 librtmp1 libsasl2-2 libsqlite3-0 libssh2-1 libssl1.1 libstdc++6 libsybdb5 libtasn1-6 libtidy5deb1 libtinfo6 libunistring2 libuuid1 libx11-6 libxau6 libxcb1 libxdmcp6 libxext6 libxml2 libxslt1.1 libzip4 procps tar zlib1g
RUN . /opt/bitnami/scripts/libcomponent.sh && component_unpack "php" "7.3.27-0" --checksum 7acd7eaaebce8422f8a482402fed235e2d2a49abea0b8bebcc8e97b887424570
RUN . /opt/bitnami/scripts/libcomponent.sh && component_unpack "apache" "2.4.46-4" --checksum 35f10bccc2a1e55c050c9c483e28ee729e11f873acadd327d7361b53938a0101
RUN . /opt/bitnami/scripts/libcomponent.sh && component_unpack "libphp" "7.3.27-0" --checksum 2286192dff818006a16c4ad42a79e8564b4e9924faced7b0d7edef6516f6ccc3
RUN . /opt/bitnami/scripts/libcomponent.sh && component_unpack "render-template" "1.0.0-3" --checksum 8179ad1371c9a7d897fe3b1bf53bbe763f94edafef19acad2498dd48b3674efe
RUN . /opt/bitnami/scripts/libcomponent.sh && component_unpack "gosu" "1.12.0-2" --checksum 4d858ac600c38af8de454c27b7f65c0074ec3069880cb16d259a6e40a46bbc50
RUN . /opt/bitnami/scripts/libcomponent.sh && component_unpack "dokuwiki" "20200729.0.0-1" --checksum d2f0229f0f6c663aeae3f77b297f8c415aae81543a2206686387a0b622411086
RUN apt-get update && apt-get upgrade -y && \
    rm -r /var/lib/apt/lists /var/cache/apt/archives
RUN chmod g+rwX /opt/bitnami

COPY rootfs /
RUN /opt/bitnami/scripts/php/postunpack.sh
RUN /opt/bitnami/scripts/apache/postunpack.sh
RUN /opt/bitnami/scripts/apache-modphp/postunpack.sh
RUN /opt/bitnami/scripts/dokuwiki/postunpack.sh
ENV APACHE_ENABLE_CUSTOM_PORTS="no" \
    APACHE_HTTPS_PORT_NUMBER="" \
    APACHE_HTTP_PORT_NUMBER="" \
    BITNAMI_APP_NAME="dokuwiki" \
    BITNAMI_IMAGE_VERSION="20200729.0.0-debian-10-r161" \
    PATH="/opt/bitnami/php/bin:/opt/bitnami/php/sbin:/opt/bitnami/apache/bin:/opt/bitnami/common/bin:$PATH"

EXPOSE 8080 8443

USER 1001
ENTRYPOINT [ "/opt/bitnami/scripts/dokuwiki/entrypoint.sh" ]
CMD [ "/opt/bitnami/scripts/apache/run.sh" ]
