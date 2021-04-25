# Docker

You can easily run the complete erogaki project using our Docker images and our Docker Compose file (located [here](https://github.com/erogaki-dev/erogaki)).

Please note that the complete erogaki project (as defined in the Docker Compose file) needs quite a lot of RAM (somewhere around 13 Gibibyte).

## 1. Step: Create a Models Docker Volume

**The models aren't included with our releases, since we're unsure about their licensing.**

Create a Docker volume for the models (this volume will be shared by multiple parts of the erogaki infrastructure):

```
docker volume create erogaki-models
```

Then get the following models and put them into the `erogaki-models` docker volume (you can find out the mountpoint using `docker volume inspect erogaki-models`):

- `hent-AI model 268`
- `09-11-2019 DCPv2 model`

You can find a link to `hent-AI model 268` [here](https://github.com/erogaki-dev/hent-AI/blob/master/README.md#the-model).
And a link to `09-11-2019 DCPv2 model` [here](https://github.com/erogaki-dev/DeepCreamPy/blob/master/docs/INSTALLATION.md).

Your `erogaki-models` volume should then have the following subdirectories:

- `hent-AI model 268/`
- `09-11-2019 DCPv2 model/`

## 2. Step: Get the Files for Docker Compose

Clone <https://github.com/erogaki-dev/erogaki> to get all the relevant files.

## 3. Step: Provide a Discord Token

Provide a Discord token for erogaki-discord in `erogaki-discord.env`.

## 4. Step: Running the Erogaki Infrastructure

Finally you can run the whole erogaki project using:

```
docker-compose up
```
