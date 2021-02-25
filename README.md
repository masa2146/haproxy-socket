global
        daemon
        maxconn 2000000
defaults
        timeout tunnel 1h
frontend http
    bind :8080
    default_backend stats
 
frontend wwws
        bind 0.0.0.0:8081
        timeout client 1h
        default_backend www_backend
        maxconn 2000000
        acl is_websocket hdr(Upgrade) -i WebSocket
        use_backend websocket_backend if is_websocket
 
backend stats
    mode http
    stats enable
 
    stats enable
    stats uri /haproxy
    stats refresh 1s
    stats show-legends
    stats admin if TRUE
 
backend www_backend
        mode http
        stats enable
        stats uri /haproxy
        option forwardfor
 
	server server1 178.18.198.108:7050 maxconn 1024 check inter 1s
        server server2 185.21.4.92:7050 maxconn 1024 check inter 1s
        server server3 37.1.145.132:7050 maxconn 1024 check inter 1s
 
backend websocket_backend
        mode http
        option forwardfor
        option http-server-close
        option forceclose
        no option httpclose
 
        server server1 178.18.198.108:7050 maxconn 1024 check inter 1s
        server server2 185.21.4.92:7050 maxconn 1024 check inter 1s
        server server3 37.1.145.132:7050 maxconn 1024 check inter 1s
