# Chapter 2: Working with Images

## What is an image?

* An image is a read-only package containing everything you need to run an application.
  
* This means they include application code, dependencies, a minimal set of OS constructs,and metadata.
* You can start multiple containers from a single image.
* The easiest way to get an image is to pull one from a registry
* `Docker Hub` is the most common registry, and pulling an image downloads it to your local machine where Docker can use it to start one or more containers.
* Other registries exist, and Docker works with them all.
* Images are made by stacking independent layers and representing them as a single unified object.
    * One layer might have the OS components
    * another layer might have application dependencies
    * another layer might have the application
    * Docker stacks these layers and makes them look like a unified system.
 

## More on Images

* images are like stopped containers.
  
* You can stop a container and create a new image from it.
* images are build-time constructs, whereas containers are run-time constructs.
* To start a container from an image:
    ```bash
    docker run
    ```
* Once the container is running, the image and the container are bound.
    * you cannot delete the image until you stop and delete the container.
    * If multiple containers use the same image, you can only delete the image after you’ve deleted all the containers using it.
* Containers are designed to run a single application or microservice.
    * As such, they should only contain application code and dependencies.
    * You should not include non-essentials such as build tools or troubleshooting tools.
    * if the application doesn’t need it at run-time, the image doesn’t include it. We call these slim images.

## Pulling images
* A clean Docker installation has an empty `local repository`.
  
* Local repository is an area on your local machine where Docker stores images for more convenient access.
* on Linux it’s usually located in `/var/lib/docker/<storage-driver>`.
* However, it will be inside the Docker VM if you’re using Docker Desktop.
* To inspect the images on your local repo, run this command:
    ```bash
    docker images
    ```
 * The process of getting images is called pulling.
 * Run the following commands to pull the redis image and verify it exists in your local repository.
     ```bash
     docker pull redis
     ```
     Output:
     ```
     $ docker images
      REPOSITORY   TAG       IMAGE ID       CREATED        SIZE
      ubuntu       latest    590e57acc18d   6 days ago     117MB
      redis        latest    acb90ced0bd7   4 weeks ago    200MB
      postgres     16        66a5efb5677f   4 weeks ago    637MB
      nginx        latest    d5f28ef21aab   4 weeks ago    279MB
      alpine       latest    4bcff63911fc   2 months ago   12.8MB
     ```
     * Docker made two assumptions when pulling the image.
         * It assumed you wanted to pull the image tagged as latest.
           
         * It assumed you wanted to pull the image from Docker Hub.
         * You can override both, but Docker will use these as defaults if you don’t override them.
      * The Redis image has eight layers. However, Docker only pulled seven layers because it already had a local copy of one of them.
        
         * This is because my system runs the Portainer Docker Desktop extension, which is based on an image that shares a common layer with the Redis image.
      * images can share layers, and Docker is clever enough only to pull the layers it doesn’t already have



## Image registries

* We store images in centralized places called registries.
  
* The job of a registry is to securely store images and make them easy to access from different environments.
* So we build the image, push it to a registery, and then that image can be pulled from the registery.
* Image registries contain one or more image repositories, and image repositories contain one or more images.
* Docker Hub has the concept of official repositories that are home to images vetted and curated by Docker and the application vendor.


 ## Image naming and tagging

 * The name of the image has the form: `<Registry>/<User/Org>/<Repository>:<image/tag>`
   * Example: `docker.io/library/nginx:1.27-alpine`
       *  Registry: `docker.io` (Docker Hub, default so you don't need to type it)
       *  User/Org: `library` (official images live here).
       *  Repository: `nginx`.
       *  Tag: `1.27-alpine`
       *  You normally just type `nginx:1.27-alpine` because docker.io/library is implied.
 * Addressing images from official repositories is easy. All you need to supply is the repository name and image name separated by a colon.
 * Sometimes we call the image name the tag.
 * The format for a docker pull command pulling an image from an official repository is:
    ```bash
    docker pull <repository>:<tag>
    ```
     * Example_1:
       ```bash
        docker pull redis:latest
       ```
       * pulls the image tagged as latest from the top-level redis repository.
     * Example_2:
       ```bash
       docker pull mongo:7.0.5
       ```
       * Pulls the image tagged as '7.0.5' from the official 'mongo' repository.

     * Example_3:
       ```bash
       docker pull busybox:glibc
       ```
       * Pulls the image tagged as 'glibc' from the official 'busybox' repository.

      
     * Example_4:
       ```bash
       docker pull alpine
       ```
       * Pulls the image tagged as 'latest' from the official 'alpine' repository.

 
 * Important Note: **Images tagged as latest are not guaranteed to be the most up-to-date in the repository.**
 * The format for a docker pull command pulling an image from the docker registry but from a unofficial repositories is: `<User/Org>/<Repository>:<image/tag>`
   * Example:
     ```bash
     docker pull nigelpoulton/tu-demo:v2
     ```
 * The format for a docker pull command pulling an image from a different registry is: `<Registry>/<User/Org>/<Repository>:<image/tag>`
   * Example:
     ```bash
     docker pull ghcr.io/regclient/regsync:latest
     ```
     * Output:
       ```bash
       latest: Pulling from regclient/regsync
        6f14f2b64ccf: Download complete
        7746d6728537: Download complete
        685af2c79c31: Download complete
        4c377311167a: Download complete
        662e9541e042: Download complete
        Digest: sha256:149a95d47d6beed2a1404d7c3b00dddfa583a94836587ba8e3b4fe59853c1ece
        Status: Downloaded newer image for ghcr.io/regclient/regsync:latest
        ghcr.io/regclient/regsync:latest
       ```

     * Notice how the pull looks the same as it did with Docker Hub.
     * This is because GHCR supports the OCI registry-spec and implements the Docker Registry v2 API.
 * Note: **You can give a single image as many tags as you want.**


## Images and layers

* images are a collection of loosely connected read-only layers where each layer comprises one or more files.





