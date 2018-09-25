FROM fluent/fluentd:v1.1-debian
USER root
RUN buildDeps="sudo build-essential libc-dev ruby-dev" \
 && echo "gem: --no-ri --no-rdoc" >> ~/.gemrc \
 && apt-get update \
 && apt-get install -y --no-install-recommends $buildDeps \
 && apt-get install -y --no-install-recommends default-libmysqlclient-dev inotify-tools procps\
 && sudo gem install mysql2 \
 && sudo gem install mysql2-cs-bind \
 && sudo gem install fluent-plugin-mysql \
 && gem install fluent-plugin-prometheus \
 && sudo gem sources --clear-all \
 && SUDO_FORCE_REMOVE=yes \
    apt-get purge -y --auto-remove \
                  -o APT::AutoRemove::RecommendsImportant=false \
                  $buildDeps \
 && rm -rf /var/lib/apt/lists/* \
           /home/fluent/.gem/ruby/2.3.0/cache/*.gem

ENV FLUENTD_CONF="fluent.conf"

RUN mkdir -p /fluentd/etc/buffer
RUN mkdir -p /fluentd/etc/config/system/

COPY fluent.conf /fluentd/etc/
COPY metrics.conf /fluentd/etc/config/system/

EXPOSE 24224 5140

CMD exec fluentd -c /fluentd/etc/${FLUENTD_CONF}
