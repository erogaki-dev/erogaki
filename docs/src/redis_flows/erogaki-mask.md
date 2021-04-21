# erogaki-mask

### wait for new image uuid

```
BLPOP mask-requests:bar|mosaic 0
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

### mask image

mask the image

## if masking was successul: create decensor request

```
RPUSH decensor-requests:<key> <uuid>
```

## if masking wasn't successful: return error

`error`:

```json
{
    "component": "erogaki-mask",
    "instance": "<instance-name>",
    "name": "<error-name>",
    "description": "<error-description>"
}
```

redis:

```
SET errors:uuid error
```
