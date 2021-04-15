# Redis Flows

```
redis-cli config set notify-keyspace-events AKE
```

(this can probably be more specific)

## erogaki: image request and receive

### image request

redis:

```
SET censored-images:uuid <image-data>
SUBSCRIBE __keyevent@0__:set decensored-images:<uuid>
SUBSCRIBE __keyevent@0__:set errors:<uuid>
RPUSH censored-image:hent-ai|deepcreampy:bar|mosaic <uuid>
```

### after keyevent `decensored-images:<uuid>`

Unsubscribe from keyevents, get the decensored image and delete it.

redis:

```
UNSUBSCRIBE __keyevent@0__:set decensored-images:<uuid>
UNSUBSCRIBE __keyevent@0__:set errors:<uuid>
GET decensored-images:uuid
DEL decensored-images:uuid
```

Send the received image to the user.

### after keyevent `errors:<uuid>`

Unsubscribe from keyevents, get the error json and delete it.

redis:

```
UNSUBSCRIBE __keyevent@0__:set decensored-images:<uuid>
UNSUBSCRIBE __keyevent@0__:set errors:<uuid>
GET errors:uuid
DEL errors:uuid
```

Use the json to craft an error message for the user and send it.

## DeepCreamPy-erogaki-wrapper: image receive and processing

### wait for new image uuid

```
BLPOP censored-images:deepcreampy:bar 0
```

returns:

```
1) key
2) value = uuid
```

### get new image data

redis:

```
GET censored-images:uuid
```

returns:

```
<image-data>
```

### process image

process the image

### if processing was sucessful: return the image

redis:

```
SET uncensored-images:uuid <image-data>
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

## hent-AI-erogaki-wrapper: image receive and processing

### wait for new image uuid

```
BLPOP censored-images:hent-ai:bar censored-images:hent-ai:mosaic 0
```

returns:

```
1) key
2) value = uuid
```

### if key is `censored-images:hent-ai:bar`

#### get new image data

redis:

```
GET censored-images:uuid
```

returns:

```
<image-data>
```

#### process image

process the image

#### if procesing was sucessful: make image processing request for DeepCreamPy-erogaki-wrapper

redis:

```
RPUSH censored-image:deepcreampy:bar <uuid>
```

#### if procesing wasn't sucessful: return an error

`error`:

```json
{
    "component": "hent-AI-erogaki-wrapper",
    "instance": "<instance-name>",
    "name": "<error-name>",
    "description": "<error-description>"
}
```

redis:

```
SET errors:uuid error
```

### if key is `censored-images:hent-ai:mosaic`

#### get new image data

redis:

```
GET censored-images:uuid
```

returns:

```
<image-data>
```

### process image

process the image

### if procesing was sucessful: return the image

redis:

```
SET uncensored-images:uuid <image-data>
```

### if procesing wasn't sucessful: return an error

`error`:

```json
{
    "component": "hent-AI-erogaki-wrapper",
    "instance": "<instance-name>",
    "name": "<error-name>",
    "description": "<error-description>"
}
```

redis:

```
SET errors:uuid error
```
