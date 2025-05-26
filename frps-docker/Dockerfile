FROM alpine:latest AS builder1
RUN apk add --no-cache wget curl
RUN latest_tag=$(curl -s https://api.github.com/repos/fatedier/frp/releases/latest | grep tag_name | cut -d '"' -f 4) && \
wget https://github.com/fatedier/frp/archive/refs/tags/${latest_tag}.tar.gz -O frp-${latest_tag}.tar.gz && \
tar -xvf frp-${latest_tag}.tar.gz && \
rm -rf frp-${latest_tag}.tar.gz && \
mv /frp* /frp

FROM golang:latest AS builder2
WORKDIR /frp
COPY --from=builder1 /frp/ /frp/
RUN make

FROM alpine:latest
WORKDIR /frp
COPY --from=builder2 /frp/bin /frp/bin
COPY --from=builder2 /frp/conf/frps.toml /etc/frp/frps.toml
ENTRYPOINT [ "bin/frps", "-c", "/etc/frp/frps.toml" ]
