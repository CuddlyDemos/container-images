FROM golang as builder

ARG PLUGINS='k8s_gateway:github.com/ori-edge/k8s_gateway'
ARG REPOS='github.com/ori-edge/k8s_gateway'
ARG COREDNS_VER='v1.8.0'

RUN git clone https://github.com/coredns/coredns.git

WORKDIR /go/coredns

RUN git checkout ${COREDNS_VER}
RUN go mod download
RUN for i in ${PLUGINS}; do echo $i >> plugin.cfg ; done
RUN for i in ${REPOS} ; do go get $i ; done
RUN make coredns SYSTEM="GOOS=linux"

FROM scratch

COPY --from=builder  /etc/ssl/certs /etc/ssl/certs
COPY --from=builder /go/coredns/coredns /.

EXPOSE 53

ENTRYPOINT [ "/coredns" ]
