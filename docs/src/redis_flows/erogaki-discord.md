# erogaki-discord: image request and receive

## user message of type: `!decensor manual bar`

### create image request

redis:

```
SET masked-images:uuid <image-data>
SUBSCRIBE __keyevent@0__:set decensored-images:<uuid>
SUBSCRIBE __keyevent@0__:set errors:<uuid>
RPUSH decensor-requests:bar <uuid>
```

### after keyevent `decensored-images:<uuid>`

Unsubscribe from keyevents, get the decensored image and delete all images.

redis:

```
UNSUBSCRIBE __keyevent@0__:set decensored-images:<uuid>
UNSUBSCRIBE __keyevent@0__:set errors:<uuid>
GET decensored-images:uuid
DEL decensored-images:uuid
DEL masked-images:uuid
```

Send the received image to the user.

### after keyevent `errors:<uuid>`

Unsubscribe from keyevents, get the error json and delete it and all images.

redis:

```
UNSUBSCRIBE __keyevent@0__:set decensored-images:<uuid>
UNSUBSCRIBE __keyevent@0__:set errors:<uuid>
GET errors:uuid
DEL errors:uuid
DEL masked-images:uuid
```

Use the json to craft an error message for the user and send it.

## user message of type: `!decensor auto bar`

### create image request

redis:

```
SET censored-images:uuid <image-data>
SUBSCRIBE __keyevent@0__:set decensored-images:<uuid>
SUBSCRIBE __keyevent@0__:set errors:<uuid>
RPUSH mask-requests:bar <uuid>
```

### after keyevent `decensored-images:<uuid>`

Unsubscribe from keyevents, get the decensored image and delete all images.

redis:

```
UNSUBSCRIBE __keyevent@0__:set decensored-images:<uuid>
UNSUBSCRIBE __keyevent@0__:set errors:<uuid>
GET decensored-images:uuid
DEL decensored-images:uuid
DEL masked-images:uuid
DEL censored-images:uuid
```

Send the received image to the user.

### after keyevent `errors:<uuid>`

Unsubscribe from keyevents, get the error json and delete it and all images.

redis:

```
UNSUBSCRIBE __keyevent@0__:set decensored-images:<uuid>
UNSUBSCRIBE __keyevent@0__:set errors:<uuid>
GET errors:uuid
DEL errors:uuid
DEL masked-images:uuid
DEL censored-images:uuid
```

Use the json to craft an error message for the user and send it.

## user message of type: `!decensor auto mosaic`

### create image request

redis:

```
SET censored-images:uuid <image-data>
SUBSCRIBE __keyevent@0__:set decensored-images:<uuid>
SUBSCRIBE __keyevent@0__:set errors:<uuid>
RPUSH mask-requests:mosaic <uuid>
```

### after keyevent `decensored-images:<uuid>`

Unsubscribe from keyevents, get the decensored image and delete all images.

redis:

```
UNSUBSCRIBE __keyevent@0__:set decensored-images:<uuid>
UNSUBSCRIBE __keyevent@0__:set errors:<uuid>
GET decensored-images:uuid
DEL decensored-images:uuid
DEL masked-images:uuid
DEL censored-images:uuid
```

Send the received image to the user.

### after keyevent `errors:<uuid>`

Unsubscribe from keyevents, get the error json and delete it and all images.

redis:

```
UNSUBSCRIBE __keyevent@0__:set decensored-images:<uuid>
UNSUBSCRIBE __keyevent@0__:set errors:<uuid>
GET errors:uuid
DEL errors:uuid
DEL masked-images:uuid
DEL censored-images:uuid
```

Use the json to craft an error message for the user and send it.

## user message of type: `!decensor`

### create image request

redis:

```
SET censored-images:uuid <image-data>
SUBSCRIBE __keyevent@0__:set decensored-images:<uuid>
SUBSCRIBE __keyevent@0__:set errors:<uuid>
RPUSH classify-requests <uuid>
```

### after keyevent `decensored-images:<uuid>`

Unsubscribe from keyevents, get the decensored image and delete all images.

redis:

```
UNSUBSCRIBE __keyevent@0__:set decensored-images:<uuid>
UNSUBSCRIBE __keyevent@0__:set errors:<uuid>
GET decensored-images:uuid
DEL decensored-images:uuid
DEL masked-images:uuid
DEL censored-images:uuid
```

Send the received image to the user.

### after keyevent `errors:<uuid>`

Unsubscribe from keyevents, get the error json and delete it and all images.

redis:

```
UNSUBSCRIBE __keyevent@0__:set decensored-images:<uuid>
UNSUBSCRIBE __keyevent@0__:set errors:<uuid>
GET errors:uuid
DEL errors:uuid
DEL masked-images:uuid
DEL censored-images:uuid
```

Use the json to craft an error message for the user and send it.
