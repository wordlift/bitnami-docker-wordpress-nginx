![Daily Builds](https://github.com/wordlift/bitnami-docker-wordpress-nginx/actions/workflows/daily.yaml/badge.svg)

# WordPress NGINX + php-redis

The aim of this repo is to create a [bitnami/wordpress-nginx](https://github.com/bitnami/bitnami-docker-wordpress-nginx/) docker image extended with the php-redis extension.

On a daily basis, a GitHub action in this repo will download the updated tags from the original repo, it will patch the Dockerfile with the php-redis extension and it will build the images.

The images are published to the Docker Hub at [wordlift/wordpress-nginx](https://hub.docker.com/r/wordlift/wordlift-nginx/tags).

This repo is sponsored by WordLift.

