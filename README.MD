# gRPC example

This section explains how to use Traefik and linkerd as reverse proxy for gRPC application 

# Traefik

 Traefik Server bench

## Traefik http 

### Launch traefik and grpc server

```
docker-compose -f docker-compose-traefik.yml up
```

Server hello listen on port :7777

Server world listen on port :7778

Traefik listen on port :4140

```
client -> Traefik -> Hello -> traefik -> World
```

### Use it 

Launch Client

```
./curl localhost:4140/toto
```

## Traefik gRPC

### Launch traefik and grpc server

```
docker-compose -f docker-compose-traefik-grpc.yml up
```

Server hello listen on port :7777

Server world listen on port :7778

Traefik listen on port :4140

```
client -> Traefik -> Hello -> traefik -> World
```

### Use it 

Launch Client

```
helloworld-client 127.0.0.1:4140
```

[See error](#traefik-error)

## Note/Issues

Traefik redirect only to an http server over http2

Traefik don't manage grpc protocol. We can't define grpc route

# Linkerd

Linkerd Server bench 

## Linkerd gRPC

### Launch Linkerd and grpc server

```
 docker-compose -f docker-compose-linkerd.yml up
 ```

Server hello listen on port :7777

Server world listen on port :7778

Linkerd listen on port :4140 

```
client -> Linkerd -> Hello -> Linkerd -> World
```

### Use it 

Launch Client

```
go get -u github.com/linkerd/linkerd-examples/docker/helloworld/helloworld-client
```

```
helloworld-client 127.0.0.1:4140
```

# Tips

Greeter client came form google.golan.org/grpc/examples has been modified to send request to frontend.local:4443

```
./greeter_client
```

Greeter Server came form google.golan.org/grpc/examples has been modified to received request to :50051

```
./greeter_server&
```

### gRPC Server certificat

Autosigned certificate has been generated for frontend.local and backend.local

# Traefik Error

```
hello_1    | 2017/10/01 12:40:41 transport: http2Client.notifyError got notified that the client transport was broken read tcp 172.20.0.4:36840->172.20.0.3:4140: read: connection reset by peer.
hello_1    | 2017/10/01 12:40:41 transport: http2Client.notifyError got notified that the client transport was broken unexpected EOF.
hello_1    | 2017/10/01 12:40:41 grpc: addrConn.resetTransport failed to create client transport: connection error: desc = "transport: write tcp 172.20.0.4:36844->172.20.0.3:4140: write: broken pipe"; Reconnecting to {traefik:4140 <nil>}
```





