# How to use this Dockerfile

Build the scancontrol dependencies libriaries using this docker container!

In a terminal in this folder, run:
```bash
docker build --tag=pkgs-building-container --build-arg UBUNTU_VERSION=focal SCANCONTROL_SDK_VERSION=1.0.0 . #--no-cache
```

Then copy the `.deb` files over to your local host with:
```bash
docker cp $(docker ps -aqf "name=pkgs-building-container")://aravis-0.8.30/build .
docker cp $(docker ps -aqf "name=pkgs-building-container"):/scanCONTROL-Linux-SDK/libmescan/builddir .
docker cp $(docker ps -aqf "name=pkgs-building-container"):/scanCONTROL-Linux-SDK/libmescan/ .
```

And you can `COPY` them over again to your preferred devcontianer ready to be used!

## docker compose solution

Otherwise, you can just change the args in the docker-compose.yml file and run this:
```bash
docker compose build
```
