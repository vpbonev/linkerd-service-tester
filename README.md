# linkerd-service-tester 🐰
Use this to test if your linkerd mesh is working.

You should be able to install this and see connections between alpha/beta/charlie/delta.

- All code included for the services
- Uses HTTP/2
- Creates four basic GRPC services that talk to each other across a couple of namespaces

This uses template metadata annotations to enable linkerd injection:
```
spec:
  template:
    metadata:
      annotations:
        linkerd.io/inject: enabled
```

It has proven useful for sanity checks... ✨

## Install

```
./install_services.sh
```

```
➜ linkerd -n foo stat deploy
NAME      MESHED   SUCCESS      RPS   LATENCY_P50   LATENCY_P95   LATENCY_P99   TCP_CONN
alpha        2/2   100.00%   2.0rps           1ms           1ms           1ms          6
beta         2/2   100.00%   2.0rps           1ms           1ms           1ms          6
charlie      2/2   100.00%   2.0rps           1ms           1ms           1ms          6

```
## Activate traffic splitting

This will split the beta service and allow for you to test traffic splitting...

`kubectl apply -f resources/`

## Custom install

Customise to fit your usecase and modify to use your own image based on this code...

```
helm install test-linkerd . \
--set=image.repository=myproxy/foobar/linkerd-service-tester:latest --set=gateway=foo.linkerd-system.svc.cluster.local
```
