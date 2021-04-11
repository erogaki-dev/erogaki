# Redis Flows

```
redis-cli config set notify-keyspace-events AKE
```

(this can probably be more specific)

## image request and receive - erogaki

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

Unsubscribe from keyevents, get the error message and delete it.

redis:

```
UNSUBSCRIBE __keyevent@0__:set decensored-images:<uuid>
UNSUBSCRIBE __keyevent@0__:set errors:<uuid>
GET errors:uuid
DEL errors:uuid
```

Send the received error message to the user.

## image receive and processing - DeepCreamPy

### loop for new image uuid

redis looping:

```
BLPOP censored-images:deepcreampy:bar <seconds>
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

### if procesing was sucessful: return the image

redis:

```
SET uncensored-images:uuid <image-data>
```

### if procesing wasn't sucessful: return an error

redis:

```
SET errors:uuid "no regions to decensor found"
```

## image receive and processing - hent-ai

### loop for new image uuid

redis looping:

```
BLPOP censored-images:hent-ai:bar censored-images:hent-ai:mosaic <seconds>
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

#### if procesing was sucessful: make image processing request for DeepCreamPy

redis:

```
RPUSH censored-image:deepcreampy:bar <uuid>
```

#### if procesing wasn't sucessful: return an error

redis:

```
SET errors:uuid "no regions to decensor found"
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

redis:

```
SET errors:uuid "no regions to decensor found"
```
