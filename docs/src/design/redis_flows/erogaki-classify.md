# erogaki-classify

## wait for new image uuid

```
BLPOP classify-requests 0
```

returns:

```
1) key
2) value = uuid
```

## get image data

redis:

```
GET censored-images:uuid
```

returns:

```
<image-data>
```

## classify image

classify the image

## if classification was successful: create detect request

### if classified bar with most confidence

```
RPUSH mask-requests:bar <uuid>
```

### if classified mosaic with most confidence

```
RPUSH mask-requests:mosaic <uuid>
```

## if classification wasn't successful: return error

`error`:

```json
{
    "component": "erogaki-classify",
    "instance": "<instance-name>",
    "name": "<error-name>",
    "description": "<error-description>"
}
```

redis:

```
SET errors:uuid error
```
