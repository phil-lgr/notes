#TCP Optimizations for Linux (ref. [book](http://shop.oreilly.com/product/0636920028048.do))

##TCP Slow-Start Restart (SSR)
Mechanism which resets TCP congestion windows (cwnd) after it has been idle for a define period of time. 
It can have negative impact on performance for HTTP keepalive connection.

`sysctl net.ipv4.tcp_slow_start_after_idle`

`sysctl -w net.ipv4.tcp_slow_start_after_idle=0`
