FROM fyneio/fyne-cross-images:windows

ENV GO_VERSION=1.25.7
ENV GO_AMD64_CHECKSUM="12e6d6a191091ae27dc31f6efc630e3a3b8ba409baf3573d955b196fdf086005"
ENV GO_ARM64_CHECKSUM="ba611a53534135a81067240eff9508cd7e256c560edd5d8c2fef54f083c07129"
ENV FYNE_VERSION=v2.7.2

RUN set -eux; \
    arch="$(dpkg --print-architecture)"; \
    url=; \
    case "$arch" in \
        'amd64') \
            url="https://go.dev/dl/go${GO_VERSION}.linux-amd64.tar.gz";\
            sha256=${GO_AMD64_CHECKSUM};\
            ;; \
        'arm64') \
            url="https://go.dev/dl/go${GO_VERSION}.linux-arm64.tar.gz";\
            sha256=${GO_ARM64_CHECKSUM};\
            ;; \
        *) echo >&2 "error: unsupported architecture '$arch'"; exit 1 ;; \
    esac; \
    rm -rf /usr/local/go; \
    curl -sSL ${url} -o go.tgz; \
    echo ${sha256} go.tgz | sha256sum -c -; \
    tar -C /usr/local -zxf go.tgz; \
    rm go.tgz; \
    go version;

RUN set -eux; \ 
    go install -ldflags="-w -s" -v "fyne.io/fyne/v2/cmd/fyne@${FYNE_VERSION}"; \
    mv /go/bin/fyne /usr/local/bin/fyne; \
    go clean -cache -modcache;