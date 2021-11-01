# zenith-kube

This repository contains utilities for running Zenith clients on Kubernetes clusters.

## zenith-kube-mitm

Simple "man-in-the-middle" (MITM) proxy that is designed to sit between an L7 (HTTP)
edge proxy and a Kubernetes API server in order for SSL client certificate authentication
to continue to work.

Usually, an L4 (TCP) proxy is required for SSL client certificate authentication to
work as the proxy server does not have the private key to re-send the SSL client
certificate. However in some cases an L4 proxy is not available and an L7 proxy is
required instead, e.g. for using virtual hosts.

When using this MITM proxy, the edge proxy is responsible for TLS termination and
verification of SSL client certificates if present in the request (so must have the
Kubernetes CA certificate available to it). This is standard functionality available
in all mainstream proxy servers.

When an SSL client certificate is present and successfully verified by the edge proxy,
the DN from the certificate should be put into a header that is sent to the MITM
proxy. The MITM proxy will then look for this header and translate the DN into the
required
[authenticating proxy headers](https://kubernetes.io/docs/reference/access-authn-authz/authentication/#authenticating-proxy)
before forwarding the request to the Kubernetes API server. Requests that do not
contain this header are forwarded unchanged.
