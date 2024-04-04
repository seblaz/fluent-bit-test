# Fluent-bit test

This repo sets up a Fluent-bit configuration that generates many chunks in a short period of time. Steps:

1. Start the containers:

```shell
docker compose up -d
```

2. Check the Grafana [dashboard](http://localhost:3000/d/ad6dc68d-06b7-48f1-87db-3a44a9b7529f/fluent-bit?orgId=1) (user=`admin`, password=`some-pass`). When the output service is up (`echo`), low cpu is used and the `busy`, `up`, and `total` chunk number is low.


3. Disable the output service:

```shell
docker compose down echo
```

4. Check the Grafana dashboard again. See how the chunks number behave. In particular, the number of chunks `up` gets maxed out. This causes Fluent-bit to be unable to flush chunks even if the output service goes up again.


5. Enable the output service:

```shell
docker compose up -d echo
```

6. Watch that Fluent-bit never flushes a chunk again and gets stuck.
