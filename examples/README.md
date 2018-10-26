# Usage

Build

```
docker-compose -f examples/build.yml build
```

Run an example

```
docker-compose -f examples/basic.yml up
```

Stop an example without clearing persistent storage (volumes)

```
docker-compose -f examples/basic.yml down
```

Stop and example and clear persistent storage (volumes)

```
docker-compose -f examples/basic.yml down -v
```
