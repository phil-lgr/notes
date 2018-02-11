# Useful Networking Commands

Check latency of each network hop for a packet

   `traceroute google.com`

#TCP Optimizations for Linux (ref. [book](http://shop.oreilly.com/product/0636920028048.do))

###TCP Initial Congestion Window
Default is 10 segments on Linux kernel 2.6.39+, one segment is 1460 bytes

###TCP Fast Open (TFO)
Allow data to be sent with the SYN request (ref [IEFT spec](https://datatracker.ietf.org/doc/rfc7413/?include_text=1))

On Linux, the TFO setting can be checked with the following command:

` sysctl net.ipv4.tcp_fastopen`

###TCP Slow-Start Restart (SSR)
Mechanism which resets TCP congestion windows (cwnd) after it has been idle for a define period of time. 
It can have negative impact on performance for HTTP keepalive connection.

On Linux, the SSR
setting can be checked and disabled via the following commands:

`sysctl net.ipv4.tcp_slow_start_after_idle`

`sysctl -w net.ipv4.tcp_slow_start_after_idle=0`
