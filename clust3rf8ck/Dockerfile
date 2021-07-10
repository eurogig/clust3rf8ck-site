FROM nginx:alpine as build

RUN apk add --update \
    wget
    
ARG HUGO_VERSION="0.84.1"
RUN wget --quiet "https://github.com/gohugoio/hugo/releases/download/v${HUGO_VERSION}/hugo_${HUGO_VERSION}_Linux-64bit.tar.gz" && \
    tar xzf hugo_${HUGO_VERSION}_Linux-64bit.tar.gz && \
    rm -r hugo_${HUGO_VERSION}_Linux-64bit.tar.gz && \
    mv hugo /usr/bin

COPY ./ /site
WORKDIR /site
RUN hugo

#Copy static files to Nginx
FROM arm64v8/nginx:alpine
COPY --from=build /site/public /usr/share/nginx/html
COPY --from=build /site/default.conf /etc/nginx/conf.d/default.conf
RUN mkdir -p /var/cache/nginx
RUN chown -R nginx:nginx /usr/share/nginx/html && \
        chown -R nginx:nginx /var/cache/nginx && \
        chown -R nginx:nginx /var/log/nginx && \
        chown -R nginx:nginx /etc/nginx/conf.d
RUN touch /var/run/nginx.pid && \
        chown -R nginx:nginx /var/run/nginx.pid

USER nginx

WORKDIR /usr/share/nginx/html

