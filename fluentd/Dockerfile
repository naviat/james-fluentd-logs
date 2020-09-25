FROM fluentd/fluentd:v1.7-1

# USe root account to use apk
USER root

# below RUN includes plugin as examples elasticsearch is not required
# you may customize including plugins as you wish

RUN apk add --no-cached --update --virtual .build-deps &&
    sudo build-base ruby-dev &&
    sudo gem install fluent-plugin-elasticsearch &&
    sudo gem sources --clear-all &&
    apk del .build-deps &&
    rm -rf /tmp/* /var/tmp/* /usr/lib/ruby/gems/*/cache/*.gem

COPY conf/fluentd.conf /fluent/etc/

COPY entrypoint.sh /bin/

USER fluent
