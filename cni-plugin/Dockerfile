## compile cni-plugin utility
FROM linkerd2arm64/go-deps:c7fb42bd as golang
WORKDIR /linkerd-build
COPY pkg pkg
COPY controller controller
COPY cni-plugin cni-plugin
RUN CGO_ENABLED=0 GOOS=linux go build -o /go/bin/linkerd-cni -v -mod=readonly ./cni-plugin/

FROM linkerd2arm64/base:2019-02-19.01
WORKDIR /linkerd
RUN curl -kL -o $(which jq) https://github.com/linkerd2arm64/jq/releases/download/jq-1.6-arm64/jq-linux-arm64
COPY --from=golang /go/bin/linkerd-cni /opt/cni/bin/
COPY LICENSE .
COPY cni-plugin/deployment/scripts/install-cni.sh .
COPY cni-plugin/deployment/linkerd-cni.conf.default .
COPY cni-plugin/deployment/scripts/filter.jq .
ENV PATH=/linkerd:/opt/cni/bin:$PATH
CMD ["install-cni.sh"]
