ARG GO_VERSION=1.26
FROM golang:$GO_VERSION

# Install golangci-lint
RUN curl -sSfL https://raw.githubusercontent.com/golangci/golangci-lint/master/install.sh | sh -s -- -b $(go env GOPATH)/bin v1.59.0

# Install tools
RUN go install mvdan.cc/gofumpt@latest
RUN mkdir -p /tmp/gocov-build && \
    cd /tmp/gocov-build && \
    go mod init temp && \
    go get github.com/axw/gocov/gocov@latest && \
    go get golang.org/x/tools@latest && \
    GOFLAGS=-mod=mod go build -o $(go env GOPATH)/bin/gocov github.com/axw/gocov/gocov
RUN go install github.com/matm/gocov-html/cmd/gocov-html@latest

WORKDIR /app
COPY . .

CMD ["golangci-lint", "run", "--config", "/app/.golangci.yml"]
