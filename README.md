# nginxChain
Proof of concept for chained reverse proxies and X-Forwarded-For dealing.

It removes any `X-Forwarded-For` header if it set without `X-Nginx-Proxy` header - what means it was spoofed (provided by an user).

Check:
`docker compose up`
and then
```shell
 for i in {1..10}; do echo "Connection #$i"; curl -sH "X-Forwarded-For: 8.8.8.8"  http://localhost:80/nginx_forwards; done
```

Result:
```shell
Connection #1
Headers debug:
{"host":"localhost","accept":"*\/*","x-remote-addr":"192.168.20.2","x-nginx-proxy":"true","x-forwarded-for":"192.168.20.6, 192.168.20.2","connection":"close","user-agent":"curl\/8.4.0"}
Forwarded IPs are:
192.168.20.6, 192.168.20.2
Connection #2
Headers debug:
{"host":"localhost","accept":"*\/*","x-remote-addr":"192.168.20.3","x-nginx-proxy":"true","x-forwarded-for":"192.168.20.6, 192.168.20.3","connection":"close","user-agent":"curl\/8.4.0"}
Forwarded IPs are:
192.168.20.6, 192.168.20.3
Connection #3
Headers debug:
{"host":"localhost","accept":"*\/*","x-remote-addr":"192.168.20.6","x-nginx-proxy":"true","x-forwarded-for":"192.168.20.6","connection":"close","user-agent":"curl\/8.4.0"}
Forwarded IPs are:
192.168.20.6
Connection #4
Headers debug:
{"host":"localhost","accept":"*\/*","x-remote-addr":"192.168.20.5","x-nginx-proxy":"true","x-forwarded-for":"192.168.20.6, 192.168.20.5","connection":"close","user-agent":"curl\/8.4.0"}
Forwarded IPs are:
192.168.20.6, 192.168.20.5
Connection #5
Headers debug:
{"host":"localhost","accept":"*\/*","x-remote-addr":"192.168.20.3","x-nginx-proxy":"true","x-forwarded-for":"192.168.20.6, 192.168.20.3, 192.168.20.2, 192.168.20.2, 192.168.20.2, 192.168.20.2, 192.168.20.3","connection":"close","user-agent":"curl\/8.4.0"}
Forwarded IPs are:
192.168.20.6, 192.168.20.3, 192.168.20.2, 192.168.20.2, 192.168.20.2, 192.168.20.2, 192.168.20.3
Connection #6
Headers debug:
{"host":"localhost","accept":"*\/*","x-remote-addr":"192.168.20.5","x-nginx-proxy":"true","x-forwarded-for":"192.168.20.6, 192.168.20.5","connection":"close","user-agent":"curl\/8.4.0"}
Forwarded IPs are:
192.168.20.6, 192.168.20.5
Connection #7
Headers debug:
{"host":"localhost","accept":"*\/*","x-remote-addr":"192.168.20.3","x-nginx-proxy":"true","x-forwarded-for":"192.168.20.6, 192.168.20.3","connection":"close","user-agent":"curl\/8.4.0"}
Forwarded IPs are:
192.168.20.6, 192.168.20.3
Connection #8
Headers debug:
{"host":"localhost","accept":"*\/*","x-remote-addr":"192.168.20.3","x-nginx-proxy":"true","x-forwarded-for":"192.168.20.6, 192.168.20.5, 192.168.20.2, 192.168.20.2, 192.168.20.3","connection":"close","user-agent":"curl\/8.4.0"}
Forwarded IPs are:
192.168.20.6, 192.168.20.5, 192.168.20.2, 192.168.20.2, 192.168.20.3
Connection #9
Headers debug:
{"host":"localhost","accept":"*\/*","x-remote-addr":"192.168.20.6","x-nginx-proxy":"true","x-forwarded-for":"192.168.20.6","connection":"close","user-agent":"curl\/8.4.0"}
Forwarded IPs are:
192.168.20.6
Connection #10
Headers debug:
{"host":"localhost","accept":"*\/*","x-remote-addr":"192.168.20.3","x-nginx-proxy":"true","x-forwarded-for":"192.168.20.6, 192.168.20.3, 192.168.20.3","connection":"close","user-agent":"curl\/8.4.0"}
Forwarded IPs are:
192.168.20.6, 192.168.20.3, 192.168.20.3

```