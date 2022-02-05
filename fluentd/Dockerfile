#FROM fluent/fluentd-kubernetes-daemonset:v1.14.3-debian-elasticsearch7-1.0
FROM fluent/fluentd:v1.14.3-1.1
    
# Use root account to use apk
USER root

RUN mkdir -p /fluentd-out/ && \
    chown fluent:fluent /fluentd-out/

# Install Plugins
RUN apk add --no-cache --update --virtual .build-deps \
        sudo build-base ruby-dev

RUN sudo gem install fluent-plugin-formatter_sprintf \
 && sudo gem sources --clear-all \
 && apk del .build-deps \
 && rm -rf /tmp/* /var/tmp/* /usr/lib/ruby/gems/*/cache/*.gem

USER fluent