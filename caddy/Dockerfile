# modified from anybox/caddy to include tls.dns.route53

#
# Builder
#
FROM golang:1.10-alpine as builder
LABEL maintainer "choochootrain"
# This dockerfile insall caddy from source and manage plugins we needs
# To this is mainly inspired from Abisoft' works
# https://github.com/abiosoft/caddy-docker/blob/master/Dockerfile


ARG version="0.11.0"

RUN apk add --no-cache curl git

# caddy
RUN git clone https://github.com/mholt/caddy --depth 1 -b "v${version}" /go/src/github.com/mholt/caddy \
    && cd /go/src/github.com/mholt/caddy \
    && git checkout -b "v${version}"

# tls.dns.route53 plugin
RUN go get -v -d github.com/caddyserver/dnsproviders/route53 \
    && printf "package caddymain\nimport _ \"github.com/caddyserver/dnsproviders/route53\"" > \
        /go/src/github.com/mholt/caddy/caddy/caddymain/route53.go

# builder dependency
RUN git clone https://github.com/caddyserver/builds /go/src/github.com/caddyserver/builds

# build
RUN cd /go/src/github.com/mholt/caddy/caddy \
    && git checkout -f \
    && go run build.go \
    && mv caddy /go/bin

#
# Final stage
#
FROM alpine:3.7
LABEL maintainer "choochootrain"

RUN apk add --no-cache openssh-client ca-certificates

# install caddy
COPY --from=builder /go/bin/caddy /usr/bin/caddy

# validate install
RUN /usr/bin/caddy -version
RUN /usr/bin/caddy -plugins

ENTRYPOINT ["/usr/bin/caddy"]
CMD ["--conf", "/etc/caddy/Caddyfile", "--log", "stdout"]
