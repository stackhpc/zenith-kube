FROM debian:bullseye-slim

# Install tini, which we will use to marshal the processes, and curl
RUN apt-get update && \
    apt-get install -y curl tini && \
    rm -rf /var/lib/apt/lists/*

COPY apiserver-wait /usr/bin/

ENTRYPOINT ["/usr/bin/tini", "-g", "--"]
CMD ["apiserver-wait"]
