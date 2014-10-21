# docker-polipo

[Polipo](http://www.pps.univ-paris-diderot.fr/~jch/software/polipo/) is a small and fast caching web proxy (a web cache, an HTTP proxy, a proxy server).
This is a [Docker](https://www.docker.com) image that eases setup.

## About Polipo

> *From [the official readme](http://www.pps.univ-paris-diderot.fr/~jch/software/polipo/):*

Polipo has some features that are, as far as I know, unique among currently available proxies:

* Polipo will use HTTP/1.1 pipelining if it believes that the remote server supports it, whether the incoming requests are pipelined or come in simultaneously on multiple connections (this is more than the simple usage of persistent connections, which is done by e.g. Squid);
* Polipo will cache the initial segment of an instance if the download has been interrupted, and, if necessary, complete it later using Range requests;
* Polipo will upgrade client requests to HTTP/1.1 even if they come in as HTTP/1.0, and up- or downgrade server replies to the client's capabilities (this may involve conversion to or from the HTTP/1.1 chunked encoding);
* Polipo has complete support for IPv6 (except for scoped (link-local) addresses).
* Polipo can optionally use a technique known as Poor Man's Multiplexing to reduce latency even further. 

In short, Polipo uses a plethora of techniques to make web browsing (seem) faster. 

## Usage

This docker image is available as a [trusted build on the docker index](https://index.docker.io/u/clue/polipo/),
so there's no setup required.
Using this image for the first time will start a download.
Further runs will be immediate, as the image will be cached locally.

The recommended way to run this container looks like this:

```bash
$ docker run -d -p 8080:8123 clue/polipo proxyAddress=0.0.0.0
```

This is a rather common setup following docker's conventions:

* `-d` will run a detached session running in the background
* `-p {OutsidePort}:8123` will bind the HTTP proxy listening port to the given outside port
* `clue/polipo` the name of this docker image
* `name=value` any number of additional configuration variables can be passed as is

### Configure your clients

You can now configure your clients (web browser, instant messenger, apt etc.) to use a HTTP proxy:

Type: HTTP Proxy
Server: 127.0.0.1 (or your public facing IP)
Port: 8080 (or your chosen {OutsidePort})
