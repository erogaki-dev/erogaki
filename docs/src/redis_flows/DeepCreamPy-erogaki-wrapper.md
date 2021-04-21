# DeepCreamPy-erogaki-wrapper

## wait for new image uuid

```
BLPOP masked-images:bar|mosaic 0
```

returns:

```
1) key
2) value = uuid
```


## get new image data

### if `key === bar`

redis:

```
GET masked-images:uuid
```

returns:

```
<image-data>
```

### if `key === mosaic`

redis:

```
GET masked-images:uuid
GET censored-images:uuid
```

returns:

```
<image-data> x2
```

## process image/s

process the image/s

## if processing was sucessful: return the image

redis:

```
SET decensored-images:uuid <image-data>
```

### if processing wasn't sucessful: return an error

`error`:

```json
{
    "component": "DeepCreamPy-erogaki-wrapper",
    "instance": "<instance-name>",
    "name": "<error-name>",
    "description": "<error-description>"
}
```

redis:

```
SET errors:uuid error
```
