# PHP Docker Images

The GitHub Actions workflow builds `./2` and publishes
`ghcr.io/nickdown/php-docker-images:2` and
`ghcr.io/nickdown/php-docker-images:latest` on every push to `main`.
Both Dockerfiles use the floating `php:8.4-fpm` tag so rebuilds pick up the
latest PHP 8.4 patch release available in the official image.

To rebuild manually, open **Actions → Build and Push Docker Image → Run workflow**
and select `main`. The workflow always pulls the base image and builds without
cached layers. The manual trigger becomes available after the workflow changes
are pushed to `main`.

After publication, pull the updated image and recreate the containers that use it.
Publishing an image does not update running containers.
