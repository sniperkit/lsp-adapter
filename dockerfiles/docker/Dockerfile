FROM golang:1.10-alpine
WORKDIR /go/src/github.com/sourcegraph/lsp-adapter
COPY . .
RUN CGO_ENABLED=0 GOBIN=/usr/local/bin go install github.com/sourcegraph/lsp-adapter

FROM rcjsuen/docker-langserver

RUN apk add --no-cache tini
ENTRYPOINT ["tini", "--"]

COPY --from=0 /usr/local/bin/lsp-adapter /usr/local/bin/
EXPOSE 8080
CMD ["lsp-adapter", "-proxyAddress=0.0.0.0:8080", "-glob=Dockerfile*", "/docker-langserver/bin/docker-langserver", "--stdio"]