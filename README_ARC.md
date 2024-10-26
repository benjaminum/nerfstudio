# Nerfstudio Docker image for Intel Arc

## Prerequisites

Linux host with Ubuntu 22.04 and installed GPU drivers.
See https://dgpu-docs.intel.com/driver/client/overview.html#installing-client-gpus-on-ubuntu-desktop-22-04-lts

## Building the image

Build the image with 
```bash
docker build --tag nerfstudio_arc -f Dockerfile_intel .
```

## Running Nerfstudio in a container

Run a container with the image using
```bash
docker run --device=/dev/dri/card1:/dev/dri/card1 --device=/dev/dri/renderD128:/dev/dri/renderD128 --rm -it nerfstudio_arc bash
```
Note that the container will be removed on exit. Remove `--rm` to keep the container.
Make sure to map the right GPU devices. In some cases the first card is `/dev/dri/card0`.

To check if the device is visible inside the docker container run `sycl-ls`
```bash
sycl-ls
# This should output the following if the device is properly mapped
# [ext_oneapi_level_zero:gpu:0] Intel(R) Level-Zero, Intel(R) Arc(TM) A770 Graphics 1.3 [1.3.30049]
```

You can now run the example using the nerfacto method
```bash
# Download some test data:
ns-download-data nerfstudio --capture-name=poster
# Train model
ns-train nerfacto --data data/nerfstudio/poster
```