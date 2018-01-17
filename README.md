# Blogr Www
The Blogr Website frontend.
Used for testing to have a small frontend to run javascript apps.

## Overview
Should write a quick overview of the application architecture with main points of interest.

## Development
Just build container and run locally

## Production 
Publish new docker image to DockerHub and Spinnaker will trigger a Pipeline that automaticaly deploys the image to development and beta kubernetes cluster. After waiting for user confirmation the pipeline if user approve release deploy it into the production cluster.

## Tech
- Nginx
- Docker
- HTML
