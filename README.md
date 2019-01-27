# nginx
Csak hogy okosodjunk.
Az alábbi cfg fájlban egy alap wordpress eléréshez szükséges beállítás található meg (site-a).

NGINX is the second de-facto standard HTTP server. Just like Apache, it covers a
wide range of features. NGINX is built on a similar model as HAProxy so it has
no problem dealing with tens of thousands of concurrent connections. When used
as a gateway to some applications (e.g. using the included PHP FPM) it can often
be beneficial to set up some frontend connection limiting to reduce the load
on the PHP application. HAProxy will clearly be useful there both as a regular
load balancer and as the traffic regulator to speed up PHP by decongesting
it. Also since both products use very little CPU thanks to their event-driven
architecture, it's often easy to install both of them on the same system. NGINX
implements HAProxy's PROXY protocol, thus it is easy for HAProxy to pass the
client's connection information to NGINX so that the application gets all the
relevant information. Some benchmarks have also shown that for large static
file serving, implementing consistent hash on HAProxy in front of NGINX can be
beneficial by optimizing the OS' cache hit ratio, which is basically multiplied
by the number of server nodes.

HAProxy is :

  - a TCP proxy : it can accept a TCP connection from a listening socket,
    connect to a server and attach these sockets together allowing traffic to
    flow in both directions;

  - an HTTP reverse-proxy (called a "gateway" in HTTP terminology) : it presents
    itself as a server, receives HTTP requests over connections accepted on a
    listening TCP socket, and passes the requests from these connections to
    servers using different connections.

  - an SSL terminator / initiator / offloader : SSL/TLS may be used on the
    connection coming from the client, on the connection going to the server,
    or even on both connections.

  - a TCP normalizer : since connections are locally terminated by the operating
    system, there is no relation between both sides, so abnormal traffic such as
    invalid packets, flag combinations, window advertisements, sequence numbers,
    incomplete connections (SYN floods), or so will not be passed to the other
    side. This protects fragile TCP stacks from protocol attacks, and also
    allows to optimize the connection parameters with the client without having
    to modify the servers' TCP stack settings.

  - an HTTP normalizer : when configured to process HTTP traffic, only valid
    complete requests are passed. This protects against a lot of protocol-based
    attacks. Additionally, protocol deviations for which there is a tolerance
    in the specification are fixed so that they don't cause problem on the
    servers (e.g. multiple-line headers).

  - an HTTP fixing tool : it can modify / fix / add / remove / rewrite the URL
    or any request or response header. This helps fixing interoperability issues
    in complex environments.

  - a content-based switch : it can consider any element from the request to
    decide what server to pass the request or connection to. Thus it is possible
    to handle multiple protocols over a same port (e.g. HTTP, HTTPS, SSH).

  - a server load balancer : it can load balance TCP connections and HTTP
    requests. In TCP mode, load balancing decisions are taken for the whole
    connection. In HTTP mode, decisions are taken per request.

  - a traffic regulator : it can apply some rate limiting at various points,
    protect the servers against overloading, adjust traffic priorities based on
    the contents, and even pass such information to lower layers and outer
    network components by marking packets.

  - a protection against DDoS and service abuse : it can maintain a wide number
    of statistics per IP address, URL, cookie, etc and detect when an abuse is
    happening, then take action (slow down the offenders, block them, send them
    to outdated contents, etc).

  - an observation point for network troubleshooting : due to the precision of
    the information reported in logs, it is often used to narrow down some
    network-related issues.

  - an HTTP compression offloader : it can compress responses which were not
    compressed by the server, thus reducing the page load time for clients with
    poor connectivity or using high-latency, mobile networks.

HAProxy is not :

  - an explicit HTTP proxy, i.e. the proxy that browsers use to reach the
    internet. There are excellent open-source software dedicated for this task,
    such as Squid. However HAProxy can be installed in front of such a proxy to
    provide load balancing and high availability.

  - a caching proxy : it will return the contents received from the server as-is
    and will not interfere with any caching policy. There are excellent
    open-source software for this task such as Varnish. HAProxy can be installed
    in front of such a cache to provide SSL offloading, and scalability through
    smart load balancing.

  - a data scrubber : it will not modify the body of requests nor responses.

  - a web server : during startup, it isolates itself inside a chroot jail and
    drops its privileges, so that it will not perform any single file-system
    access once started. As such it cannot be turned into a web server. There
    are excellent open-source software for this such as Apache or Nginx, and
    HAProxy can be installed in front of them to provide load balancing and
    high availability.

  - a packet-based load balancer : it will not see IP packets nor UDP datagrams,
    will not perform NAT or even less DSR. These are tasks for lower layers.
    Some kernel-based components such as IPVS (Linux Virtual Server) already do
    this pretty well and complement perfectly with HAProxy.
