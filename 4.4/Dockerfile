FROM netsandbox/request-tracker-base

LABEL maintainer="Christian Loos <cloos@netsandbox.de>"

SHELL ["/bin/bash", "-o", "pipefail", "-c"]

WORKDIR /usr/local/src
RUN curl -sSL "https://download.bestpractical.com/pub/rt/release/rt-4.4.4.tar.gz" -o rt.tar.gz \
  && echo "34c316a4a78d7ee9b95d4391530f9bb3ff3edd99ebbebfac6354ed173e940884  rt.tar.gz" | sha256sum -c \
  && tar -xzf rt.tar.gz

WORKDIR /usr/local/src/rt-4.4.4
RUN ./configure \
    --disable-gpg \
    --disable-smime \
    --enable-developer \
    --enable-gd \
    --enable-graphviz \
    --with-db-type=SQLite \
  && make install \
  && make initdb \
  && rm -rf /usr/local/src/*

COPY apache.rt.conf /etc/apache2/sites-available/rt.conf
RUN a2dissite 000-default.conf && a2ensite rt.conf

RUN chown -R www-data:www-data /opt/rt4/var/

COPY RT_SiteConfig.pm /opt/rt4/etc/RT_SiteConfig.pm

VOLUME /opt/rt4

COPY docker-entrypoint.sh /usr/local/bin/

ENTRYPOINT ["docker-entrypoint.sh"]

CMD ["apache2-foreground"]
