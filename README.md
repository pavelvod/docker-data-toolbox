
### Instructions

Create volumes for postres:

```
docker volume create --driver local \
    --opt type=none \
    --opt device=/path/to/your/folder/postgres-store \
    --opt o=bind postgres-store
```

and notebooks/mlflow:
```
docker volume create --driver local \
    --opt type=none \
    --opt device=/path/to/your/folder/file-store \
    --opt o=bind file-store

```

then build and run:

```
docker-compose up
```
