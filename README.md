# dind-template
Template for Docker In Docker (DIND)

Screwdriver supports DIND when using `executor-k8s` as build executor.

*Cluster admin must enable [`DOCKER_FEATURE_ENABLED`](https://docs.screwdriver.cd/cluster-management/configure-api#executor-plugin) for executor-k8s configuration*

If this feature is enabled in your build cluster then [job annotations](https://docs.screwdriver.cd/user-guide/configuration/annotations#job-level-annotations) can be used to enable DIND in your Screwdriber build. This is an example template which is used to build and publish a docker container to Docker Hub from Screwdriver using DIND

## Usage

### Docker hub Repo to publish your container

Set environment variable `DOCKER_REPO` to specify your Dockerhub repository details. *Eg: screwdrivercd/screwdriver*

### Authorization

Your pipeline must configure secrets `DOCKER_REGISTRY_USER` & `DOCKER_REGISTRY_TOKEN` to authorize a user who has permission to push to the `DOCKER_REPO` used.

### Multi-platforms build

Set environment variables `DOCKER_MULTI_PLATFORM_BUILDS_ENABLED` to `1` to enable Docker buildx build and `DOCKER_TARGET_PLATFORMS` to comman-separated list of target platforms

## Example

Screwdriver [Pipeline](https://cd.screwdriver.cd/pipelines/1/events)

```
docker-publish:
  template: sd/dind@latest
  environment:
    DOCKER_REPO: screwdrivercd/screwdriver

docker-multi-arch-builds:
  template: sd/dind@latest
  environment:
    DOCKER_REPO: screwdrivercd/screwdriver
    DOCKER_MULTI_PLATFORM_BUILDS_ENABLED: 1
    DOCKER_TARGET_PLATFORMS: linux/amd64,linux/arm64,linux/arm/v7,linux/arm/v6,linux/ppc64le,linux/s390x
```
