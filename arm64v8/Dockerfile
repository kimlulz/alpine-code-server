FROM martinussuherman/alpine:3.13-arm64v8-glibc

ENV \
   # container/su-exec UID \
   EUID=1001 \
   # container/su-exec GID \
   EGID=1001 \
   # container/su-exec user name \
   EUSER=vscode \
   # container/su-exec group name \
   EGROUP=vscode \
   # should user shell set to nologin? (yes/no) \
   ENOLOGIN=no \
   # container user home dir \
   EHOME=/home/vscode

COPY code-server /usr/bin/
RUN chmod +x /usr/bin/code-server

# Install dependencies
RUN \
   apk --no-cache --update add \
   bash \
   curl \
   git \
   gnupg \
   nodejs \
   openssh-client

RUN \
   export VERSION=$(curl -sX GET https://api.github.com/repos/coder/code-server/releases/latest | awk '/tag_name/{print $4;exit}' FS='[""]' | sed 's|^v||') && \
   echo "******************" && \
   echo "Version : $VERSION" && \
   echo "******************" && \
   curl -fsSL -o code-server-${VERSION}-linux-arm64.tar.gz https://github.com/coder/code-server/releases/download/v${VERSION}/code-server-${VERSION}-linux-arm64.tar.gz && \
   tar -zxf code-server-${VERSION}-linux-arm64.tar.gz && \
   rm code-server-${VERSION}-linux-arm64.tar.gz && \
   mv code-server-${VERSION}-linux-arm64 /usr/lib/code-server

ENTRYPOINT ["entrypoint-su-exec", "code-server"]
CMD ["--bind-addr 0.0.0.0:8080"]
